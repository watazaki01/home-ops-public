# home-ops-public

自宅運用、Docker、Home Assistant、監視、Codex引き継ぎのための公開用sanitizedテンプレート集です。

Sanitized public reference templates for home operations with Docker, Home Assistant, monitoring, and Codex handoff.

このrepoは公開されています。privateな運用ファイルをそのままコピーしないでください。

This repository is intentionally public. Do not copy private operational files here.

## 目的 / Purpose

実環境を公開せず、再利用しやすい考え方だけを共有します。

Share reusable ideas without exposing the real home environment.

- Codex handoff templates / Codex引き継ぎテンプレート
- Secret handling templates / secret管理テンプレート
- Docker monitoring patterns / Docker監視パターン
- Home Assistant + AI architecture notes / Home Assistant + AI構成メモ
- GitHub Actions concepts / GitHub Actions設計例
- Rule-based checks before paid API calls / 有料API前のルールベース判定
- Public-safe runbooks / 公開可能なrunbook

## 公開禁止 / Never Publish

- Real IP addresses / 実IPアドレス
- Real hostnames / 実ホスト名
- Tailnet details / Tailnet詳細
- SSH usernames tied to production access / 本番アクセスに結びつくSSHユーザー名
- API keys, tokens, private keys, or webhook URLs / API key、token、秘密鍵、webhook URL
- Real `.env` files / 実 `.env`
- Home Assistant entity IDs that expose private home layout / 家の構成が分かるHome Assistant entity ID
- Real logs, reports, screenshots, or backup files / 実ログ、report、screenshot、backup
- Exact private production paths if they increase attack usefulness / 攻撃ヒントになる本番パス

## 推奨構成 / Suggested Structure

```text
docs/
  codex-handoff-template.md
  secret-handling-template.md
  docker-monitoring-pattern.md
  home-assistant-ai-pattern.md
examples/
  env.example
  docker-compose.example.yml
  github-actions-example.yml
```

## 公開ルール / Publishing Rule

まずprivate repoで下書きし、手動でsanitizeしてから公開します。

Draft privately first, sanitize manually, then publish here only after review.
