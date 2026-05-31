# secret境界パターン / Secret Boundary Pattern

This is a public, sanitized pattern for small home-lab or self-hosted operations.

No real hostnames, tokens, keys, webhook URLs, IPs, or `.env` values should be copied into this document.

## 目的 / Goal

Keep source control useful without turning it into a secret store.

## 推奨分担 / Recommended Split

| Place | Role |
|---|---|
| Password manager | Source of truth for secret material. |
| Local secret broker, such as 1Password Connect | Preferred runtime read path for repeated local automation. |
| GitHub Secrets | Runtime/bootstrap copy only when GitHub Actions needs it. |
| Server `.env` files | Runtime copy for Docker/services; back it up privately. |
| GitHub repositories | Docs, templates, fake examples, secret names, and recovery steps. |

## ルール / Rules

- Commit `.env.example`, never real `.env`.
- Commit secret names and password-manager item names only when appropriate.
- Do not commit SSH private keys, API keys, webhook URLs, certificates, or service account tokens.
- If a secret is exposed in chat, logs, or an issue, revoke it and reissue it.
- Prefer service accounts for automation credentials.
- Prefer a local secret broker for repeated reads instead of calling the password manager API in a tight loop.
- Cache only non-secret summaries, health states, and timestamps in runtime status artifacts.
- Prefer read-only checks and draft PRs for unattended automation unless the runtime class is pre-approved.

## Runtime secret access pattern

For repeated local automation, avoid direct password-manager reads on every job run.
Use this order:

1. Read an existing non-secret status artifact.
2. Read through a local secret broker or Connect-style service.
3. Fall back to a service account based CLI read for bootstrap or recovery.
4. Use interactive desktop approval for one-off human work.

Do not print the value at any step. Pass the secret directly to the target process
or write it to a temporary owner-only file that is deleted after use.

## Docker Composeパターン / Docker Compose Pattern

Bad:

```yaml
services:
  app:
    environment:
      - DISCORD_WEBHOOK_URL=https://example.invalid/real-secret
```

Good:

```yaml
services:
  app:
    env_file:
      - .env
    environment:
      - APP_MODE=production
```

```dotenv
DISCORD_WEBHOOK_URL=op://example-vault/example-item/DISCORD_WEBHOOK_URL
```

## GitHub Actionsパターン / GitHub Actions Pattern

Use one bootstrap secret to read values from a password manager:

```yaml
- uses: 1password/load-secrets-action/configure@v3
  with:
    service-account-token: ${{ secrets.OP_SERVICE_ACCOUNT_TOKEN }}

- uses: 1password/load-secrets-action@v3
  with:
    export-env: true
  env:
    API_KEY: "op://example-vault/example-api-key/API_KEY"
```

Validate presence without printing values:

```bash
if [ -z "${API_KEY:-}" ]; then
  echo "::error::API_KEY did not load"
  exit 1
fi
```

## 人間承認の境界 / Human Approval Boundary

Automation may proceed from repository work to production apply when that class of
work is pre-approved and the runbook includes preflight, rollback, postflight,
and reporting.

明示承認がなくても、事前承認済みclassで、preflight、rollback、postflight、報告が揃う場合は、本番反映まで自律実行できます。

Always require explicit owner approval for:

- credential lifecycle changes
- token/API key issue, revoke, rotation, or permission expansion
- billing, paid resources, plan changes, quota increases
- secret display outside the approved runtime path
- destructive operations that cannot be rolled back
- broad network/security changes that can lock out operators

## 復旧台帳 / Recovery Ledger

Keep a private ledger that records:

- where each secret lives
- which runtime uses it
- how to rotate it
- which GitHub Secrets are temporary copies
- which `.env` files are backed up in the password manager
- which secret broker or Connect endpoint is the preferred runtime path

The ledger should never include secret values.
