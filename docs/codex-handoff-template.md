# Codex引き継ぎテンプレート / Codex Handoff Template

Use this template to help a new Codex chat understand a project without prior conversation history.

Replace all placeholders before publishing or using.

## 最初に読むもの / Read This First

1. `CODEX_HANDOFF.md`
2. `README.md`
3. `docs/architecture.md`
4. Target service docs

## repoの役割 / Repository Roles

| Repository | Visibility | Role |
|---|---:|---|
| `example-private-ledger` | private | Parent operations ledger. |
| `example-service` | private | Service source repository. |
| `example-public` | public | Sanitized public templates only. |

## secretルール / Secret Rule

Do not commit secret values. Use a password manager as the source of truth.

Safe to commit:

- Secret names
- Placeholder values
- Rotation procedures
- `.env.example`

Forbidden:

- API keys
- tokens
- SSH private keys
- webhook URLs
- real `.env` files

## 開始チェックリスト / Startup Checklist

- What repo is source of truth?
- Is this docs-only, code, or production?
- Does it need SSH?
- Does it need secrets?
- Is there a rollback path?
- What needs human approval?
