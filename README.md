# home-ops-public

自宅運用、Docker、Home Assistant、監視、Codex引き継ぎのための公開用sanitizedテンプレート集です。

Sanitized public reference templates for home operations with Docker, Home Assistant, monitoring, and Codex handoff.

このrepoは公開されています。privateな運用ファイルをそのままコピーしないでください。

## 目的

実環境を公開せず、再利用しやすい考え方だけを共有します。

Share reusable ideas without exposing the real home environment.

- Codex引き継ぎテンプレート
- secret管理テンプレート
- Docker監視パターン
- Home Assistant + AI構成メモ
- GitHub Actions設計例
- 有料API前のルールベース判定
- 公開可能なrunbook

## docs

- [Secret境界パターン](docs/secret-boundary-pattern.md)
- [Codex引き継ぎテンプレート](docs/codex-handoff-template.md)
- [自動監視パターン](docs/automated-watch-pattern.md)
- [DiscordからCodex依頼への流れ](docs/discord-codex-request-pattern.md)
- [APIレート制限パターン](docs/api-rate-limit-pattern.md)
- [メンテナンス時間帯設計](docs/maintenance-window-pattern.md)
- [復元訓練テンプレート](docs/backup-restore-drill-template.md)
- [runtime反映ポリシーテンプレート](docs/runtime-apply-policy-template.md)
- [自宅運用repo分割テンプレート](docs/home-ops-repo-split-template.md)

## 公開禁止

- 実IPアドレス
- 実ホスト名
- Tailnet詳細
- 本番アクセスに結びつくSSHユーザー名
- API key、token、秘密鍵、webhook URL
- 実 `.env`
- 家の構成が分かるHome Assistant entity ID
- 実ログ、report、screenshot、backup
- 攻撃ヒントになるprivate production path

## 推奨構成

```text
docs/
  codex-handoff-template.md
  secret-boundary-pattern.md
  automated-watch-pattern.md
  discord-codex-request-pattern.md
  api-rate-limit-pattern.md
  maintenance-window-pattern.md
  runtime-apply-policy-template.md
examples/
  env.example
  docker-compose.monitoring.example.yml
.github/
  workflows/codex-attention-watch.yml
  ISSUE_TEMPLATE/
```

## 公開ルール

まずprivate repoで下書きし、手動でsanitizeしてから公開します。

Draft privately first, sanitize manually, then publish here only after review.
