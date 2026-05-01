# secret境界パターン / Secret Boundary Pattern

This is a public, sanitized pattern for small home-lab or self-hosted operations.

No real hostnames, tokens, keys, webhook URLs, IPs, or `.env` values should be copied into this document.

## 目的 / Goal

Keep source control useful without turning it into a secret store.

## 推奨分担 / Recommended Split

| Place | Role |
|---|---|
| 1Password | Source of truth for secret material. |
| GitHub Secrets | Runtime/bootstrap copy only when GitHub Actions needs it. |
| Server `.env` files | Runtime copy for Docker/services. |
| GitHub repositories | Docs, templates, fake examples, secret names, and recovery steps. |

## ルール / Rules

- Commit `.env.example`, never real `.env`.
- Commit secret names and 1Password item names only when appropriate.
- Do not commit SSH private keys, API keys, webhook URLs, certificates, or service account tokens.
- If a secret is exposed in chat, logs, or an issue, revoke it and reissue it.
- Prefer service accounts for automation credentials.
- Prefer read-only checks and draft PRs for unattended automation.

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

Use one bootstrap secret to read values from 1Password:

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

Automation may prepare safe changes, but production-impacting operations should require explicit owner approval:

- service restart
- Docker compose up/down/rebuild
- firmware or OS update
- secret rotation
- DNS/Tailscale/network policy change
- destructive file operation

## 復旧台帳 / Recovery Ledger

Keep a private ledger that records:

- where each secret lives
- which runtime uses it
- how to rotate it
- which GitHub Secrets are temporary copies
- which `.env` files are backed up in 1Password

The ledger should never include secret values.
