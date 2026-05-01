# Home Ops Repo Split Template / 自宅運用repo分割テンプレート

This is a public-safe template for splitting home operations repositories.

これは自宅運用repo分割の公開用テンプレートです。実IP、secret、host名は入れないでください。

## Recommended Split / 推奨分割

| Repository | Visibility | Role |
|---|---:|---|
| `home-ops` | private | Parent operations ledger / 親運用台帳 |
| `service-app` | private | Service application source / アプリ本体 |
| `docker-stacks` | private | Compose source / Docker compose管理 |
| `home-assistant-config` | private | HA config source excluding secrets / secret除外のHA設定 |
| `home-ops-public` | public | Sanitized templates / 公開用テンプレート |

## Secret Rule / secretルール

Store real secrets in a password manager, not GitHub.

secret本体はGitHubではなくpassword managerへ置きます。

## Runtime Rule / runtimeルール

Repository changes and production runtime changes are separate.

repo変更と本番runtime反映は別物として扱います。
