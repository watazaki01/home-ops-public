# home-ops-public

自宅運用、Docker、Home Assistant、監視、Codex引き継ぎのための公開用sanitizedテンプレート集です。

Sanitized public reference templates for home operations with Docker, Home Assistant, monitoring, and Codex handoff.

このrepoは公開されています。privateな運用ファイルをそのままコピーしないでください。

## はじめに / Start here

DockerやHome Assistantの運用を整理したい人、AIに作業を任せる前に権限・確認・復元の境界を決めたい人向けの資料集です。完成済みの自動運用製品や、そのまま本番へ導入できる構成一式ではありません。

Reference material for maintainers who want safer monitoring, recovery, and AI-assisted operations—not a turnkey deployment.

1. [秘密情報の境界](docs/secret-boundary-pattern.md)を確認する。
2. [引き継ぎテンプレート](docs/codex-handoff-template.md)で目的と禁止事項を整理する。
3. [変更の反映方針](docs/runtime-apply-policy-template.md)と[復元訓練](docs/backup-restore-drill-template.md)を準備する。

読むだけならAPI鍵や実環境への接続は不要です。例の実行には環境ごとの検証が必要で、外部サービスには費用がかかる場合があります。

改善提案は[貢献ガイド](CONTRIBUTING.md)をご覧ください。

## License / ライセンス

このリポジトリのオリジナル資料・例は[MIT License](LICENSE)で提供します。第三者の製品・リンク先コンテンツの権利はそれぞれの権利者に帰属します。

セキュリティ上の問題は[非公開報告の案内](SECURITY.md)をご覧ください。

## 目的と提供内容

実環境を公開せず、再利用しやすい考え方だけを共有します。

Share reusable ideas without exposing the real home environment.

- Codex引き継ぎテンプレート
- secret管理テンプレート
- runtime secret access / Connect-style broker pattern
- Docker監視パターン
- Home Assistant + AI構成メモ
- GitHub Actions設計例
- 有料API前のルールベース判定
- 公開可能なrunbook

## docs

- [Secret境界パターン](docs/secret-boundary-pattern.md)
- [runtime secret access pattern](docs/runtime-secret-access-pattern.md)
- [Codex引き継ぎテンプレート](docs/codex-handoff-template.md)
- [自動監視パターン](docs/automated-watch-pattern.md)
- [一時障害に強いstatus refresh](docs/resilient-status-refresh-pattern.md)
- [DiscordからCodex依頼への流れ](docs/discord-codex-request-pattern.md)
- [APIレート制限パターン](docs/api-rate-limit-pattern.md)
- [メンテナンス時間帯設計](docs/maintenance-window-pattern.md)
- [復元訓練テンプレート](docs/backup-restore-drill-template.md)
- [runtime反映ポリシーテンプレート](docs/runtime-apply-policy-template.md)
- [自宅運用repo分割テンプレート](docs/home-ops-repo-split-template.md)
- [公開release基準](docs/release-criteria.md)
- [Tailscale最小権限化の段階移行](docs/tailscale-least-privilege-rollout-pattern.md)
- [IoT通信ログから最小権限Firewall候補を作る](docs/iot-flow-least-privilege-analysis-pattern.md)
- [GitHub自動運用のノイズ抑制](docs/github-automation-hygiene-pattern.md)
- GitHub Actions: 公開release安全チェック でsecret/private情報らしき文字列を検出

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
  runtime-secret-access-pattern.md
  automated-watch-pattern.md
  resilient-status-refresh-pattern.md
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
