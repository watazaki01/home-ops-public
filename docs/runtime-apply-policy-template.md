# Runtime Apply Policy Template / runtime反映ポリシーテンプレート

Repository changes and production runtime changes are separate.

repo変更と本番runtime反映は別物として扱います。

## Classes / 分類

| Class | Meaning |
|---|---|
| `docs_only` | Documentation only / docsのみ |
| `source_only` | Source/config repo only / repo上のsourceのみ |
| `runtime_readonly_check` | Readonly production check / 読み取り確認のみ |
| `runtime_apply_low_risk` | Low-risk runtime apply / 低リスク反映 |
| `runtime_apply_restart` | Restart required / restartあり |
| `runtime_apply_dangerous` | Secrets/network/sudo/cron / 危険操作 |

## Rule / ルール

Production runtime changes need explicit approval and rollback.

本番runtime変更には明示承認とrollbackが必要です。
