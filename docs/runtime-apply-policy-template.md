# runtime反映ポリシーテンプレート / Runtime Apply Policy Template

Repository changes and production runtime changes are separate.

repo変更と本番runtime反映は別物として扱います。

## 分類 / Classes

| Class | Meaning |
|---|---|
| `docs_only` | Documentation only / docsのみ |
| `source_only` | Source/config repo only / repo上のsourceのみ |
| `runtime_readonly_check` | Readonly production check / 読み取り確認のみ |
| `runtime_apply_low_risk` | Low-risk runtime apply / 低リスク反映 |
| `runtime_apply_restart` | Restart required / restartあり |
| `runtime_apply_dangerous` | Secrets/network/sudo/cron / 危険操作 |

## ルール / Rule

Production runtime changes need explicit approval and rollback.

本番runtime変更には明示承認とrollbackが必要です。
