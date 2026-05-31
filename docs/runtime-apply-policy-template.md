# runtime反映ポリシーテンプレート / Runtime Apply Policy Template

Repository changes and production runtime changes are separate, but a well-run
system can pre-authorize some runtime applies when the guardrails are explicit.

repo変更と本番runtime反映は別物として扱います。ただし、guardrailが明確なら、一部のruntime反映は事前承認済みの自律実行として扱えます。

## 分類 / Classes

| Class | Meaning | Default handling |
|---|---|---|
| `docs_only` | Documentation only / docsのみ | Apply freely |
| `source_only` | Source/config repo only / repo上のsourceのみ | Test and merge normally |
| `runtime_readonly_check` | Readonly production check / 読み取り確認のみ | Run freely, redact secrets |
| `runtime_apply_preapproved` | Pre-authorized low-risk apply / 事前承認済み低リスク反映 | Run autonomously with evidence |
| `runtime_apply_restart` | Restart required / restartあり | Run only when the runbook pre-approves it or owner approves it |
| `runtime_apply_dangerous` | Secrets/network/sudo/cron/billing/destructive / 危険操作 | Require explicit owner approval |

## 自律実行できる条件 / Autonomous Apply Requirements

A runtime apply can be autonomous only when all of these are true:

- the owner has pre-approved the class of work in writing
- the exact target and blast radius are known
- the command is idempotent or has a documented rollback
- preflight checks are recorded
- backup or rollback path is available before apply
- postflight checks are recorded after apply
- the result is reported to the operations ledger or issue tracker
- no secret values are printed

次を満たす場合だけ、自律反映してよいです。

- ownerがその作業classを事前承認している
- 対象と影響範囲が明確
- コマンドが冪等、またはrollback手順がある
- preflightを記録する
- 反映前にbackupまたはrollback経路がある
- postflightを記録する
- 結果を運用台帳またはissueへ残す
- secret値を表示しない

## 明示承認が必要なもの / Always Require Explicit Approval

- credential lifecycle changes: token/API key issue, revoke, rotation, permission expansion
- secret display or export outside the approved runtime path
- billing, paid resources, plan changes, quota increases
- irreversible or hard-to-rollback destructive operations
- network/security policy changes that can lock out operators
- broad `sudo` or administrator changes without a narrow runbook

## 実行ログ / Evidence

Record a short apply note:

```text
Target: <service or host>
Class: runtime_apply_preapproved
Preflight: <summary>
Change: <what was applied>
Rollback: <how to revert>
Postflight: <summary>
Result: ok / watch / failed
```

The evidence must be useful without exposing secret values.
