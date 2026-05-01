# Automated Watch Pattern / 自動監視パターン

A low-cost pattern for checking GitHub PR attention items while a local AI assistant app is offline.

ローカルAIアシスタントアプリが落ちている間も、低コストでGitHub PRの要対応項目を見つけるパターンです。

## Idea / 考え方

Use GitHub Actions on a low-frequency schedule.

低頻度のGitHub Actionsだけを使います。

Check:

- open PRs
- requested changes
- review comments
- failing checks
- failing statuses

If attention is needed, create or update an issue named:

```text
Codex attention inbox
```

## Cost Rule / コストルール

Do not call paid AI APIs for routine polling.

routine pollingには有料AI APIを使いません。

Use paid AI only for real analysis when the issue requires it.

本当に分析が必要な時だけ有料AIを使います。
