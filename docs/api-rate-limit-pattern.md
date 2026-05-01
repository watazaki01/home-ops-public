# API Rate Limit Pattern / APIレート制限パターン

Public-safe pattern for small home-ops automations that call GitHub, a password manager, VPN APIs, or AI tools.

GitHub、password manager、VPN API、AI toolsを使う小さな自宅運用自動化向けの公開用パターンです。

## Principles / 原則

- Do not poll too often.
- Do not write a GitHub issue or Discord message when nothing changed.
- Treat `429`, `Too Many Requests`, and secondary rate limits as normal operational states.
- Back off instead of retrying in a tight loop.
- Never print tokens, private keys, webhook URLs, or secret values.

- 高頻度pollingを避ける。
- 変化がなければGitHub issueやDiscordへ書かない。
- `429` や `Too Many Requests` は異常ではなく運用上あり得る状態として扱う。
- 連打せずbackoffする。
- token、秘密鍵、webhook URL、secret値を表示しない。

## Suggested Behavior / 推奨動作

| State | Action |
|---|---|
| Healthy / no change | local log only |
| Temporary 429 | backoff and retry a few times |
| Repeated 429 | stop and notify operations channel |
| Needs approval | create/update one GitHub issue |
| Needs human work | create/update one GitHub issue |

## Backoff Shape / backoff例

```text
attempt 1: wait 10s
attempt 2: wait 40s
attempt 3: wait 90s
attempt 4: wait 160s
attempt 5: stop and report
```

Use the service-provided `Retry-After` header when available.

`Retry-After` headerがある場合は、それを優先します。

## Password Manager Note / password managerの注意

Password manager APIs can be the first bottleneck in a home automation stack.

自宅運用では、GitHubよりpassword managerの制限が先に詰まることがあります。

Recommended:

- read secrets at startup, not inside tight loops
- prefer item IDs over fuzzy item names when the tool supports it
- cache short-lived runtime env locally with strict file permissions
- consider a local secret cache such as 1Password Connect for repeated local reads

## GitHub Note / GitHubの注意

For small repos, GitHub App or Actions limits are usually enough if polling is low-frequency.

小規模repoなら、低頻度pollingであればGitHub AppやActionsの制限は通常十分です。

Still:

- update one existing issue instead of creating many issues
- close stale status issues when resolved
- avoid posting healthy status comments every few minutes
