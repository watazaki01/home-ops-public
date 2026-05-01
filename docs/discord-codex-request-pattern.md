# DiscordからCodex依頼への流れ / Discord To Codex Request Pattern

Public-safe pattern for using Discord as a lightweight request UI while keeping GitHub issues as the audit log.

Discordを気軽な依頼UIにしつつ、GitHub issueを作業台帳と承認ログにする公開用パターンです。

## 目的 / Goal

- Let a human send a short request from a phone.
- Keep the durable task record in GitHub issues.
- Require explicit approval before risky operations.
- Avoid exposing secrets in chat, issues, or logs.

- スマホから短く依頼できるようにする。
- 永続的な作業記録はGitHub issueに残す。
- リスクのある操作は明示承認を必須にする。
- secretをチャット、issue、ログに出さない。

## チャンネル分離 / Channel Split

```text
#codex-requests
  human requests, follow-up questions, approvals, Codex answers

#codex-ops
  failures, abnormal reports, approval-required notices, completion notices
```

正常な定期チェック結果は投稿しません。正常時はローカルログだけに残します。

Do not post healthy scheduled check results. Keep them in local logs only.

## 流れ / Flow

```text
human in #codex-requests
  -> lightweight router
  -> GitHub issue
  -> low-risk checker or on-demand Codex runner
  -> issue comment / draft PR
  -> Discord answer when useful
```

## 承認ルール / Approval Rule

Risky operations should not run from a Discord message alone.

Discord投稿だけで危険操作を即実行しません。

Approval examples:

```text
承認: <target> の <operation> を実行してよい
承認 #123: <target> の <operation> を実行してよい
```

When multiple approval-required issues exist, require the issue number.

承認待ちissueが複数ある場合は、issue番号付き承認を要求します。

## 承認が必要な例 / Risky Operation Examples

- production runtime changes
- Docker/Home Assistant restarts
- VPN/Tailscale settings
- password manager items or service accounts
- GitHub Secrets / deploy keys / app permissions
- SSH keys, sudoers, authorized_keys
- destructive file operations
- paid API use outside an approved workflow

## 公開安全性 / Public Safety

Do not publish:

- Discord bot token
- webhook URL
- GitHub token or app private key
- real channel IDs if they identify a private server
- real logs containing private hostnames or paths

公開repoには、実tokenや実URLではなく、構成と判断基準だけを置きます。
