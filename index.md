# Home Ops Public

自宅運用、Docker、Home Assistant、監視、Codex引き継ぎのための公開用sanitizedテンプレート集です。

このサイトは、実環境のsecretや構成詳細を公開せず、再利用しやすい考え方だけを共有するための公開メモです。

## まず読むもの

- [secret境界パターン](docs/secret-boundary-pattern.md)
- [runtime secret access pattern](docs/runtime-secret-access-pattern.md)
- [Codex引き継ぎテンプレート](docs/codex-handoff-template.md)
- [一時障害に強いstatus refresh](docs/resilient-status-refresh-pattern.md)
- [DiscordからCodex依頼への流れ](docs/discord-codex-request-pattern.md)
- [APIレート制限パターン](docs/api-rate-limit-pattern.md)
- [メンテナンス時間帯設計](docs/maintenance-window-pattern.md)
- [復元訓練テンプレート](docs/backup-restore-drill-template.md)
- [runtime反映ポリシーテンプレート](docs/runtime-apply-policy-template.md)
- [自宅運用repo分割テンプレート](docs/home-ops-repo-split-template.md)
- [公開release基準](docs/release-criteria.md)

## 公開禁止

- 実IPアドレス、実ホスト名、Tailnet詳細
- API key、token、秘密鍵、webhook URL
- 実 `.env`、実ログ、実report、screenshot、backup
- 本番アクセスに結びつくSSHユーザー名やprivate production path
- 家の構成や生活パターンが分かるHome Assistant entity ID

## GitHub repo

- [watazaki01/home-ops-public](https://github.com/watazaki01/home-ops-public)
