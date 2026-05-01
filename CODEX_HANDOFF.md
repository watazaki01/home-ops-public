# CODEX_HANDOFF

このリポジトリは公開用です。

ここにcommitする内容は、知らない人が読んでも問題ないものだけにします。

This repository is public. Everything committed here must be safe for strangers to read.

## 役割

`home-ops-public` はsanitized済みテンプレートと外部参考用資料だけを置く場所です。

実運用の正本は、たとえば `example-private-ledger` のようなprivate運用台帳repoに置きます。

English note: this repo contains sanitized templates and public-safe reference material only.

## 置いてよいもの

- 汎用的なarchitecture template
- Codex handoff template
- secret handling template
- Docker / Home Assistant monitoring pattern
- fake `.env.example`
- public-safe runbook

## 置いてはいけないもの

- 実IPアドレス
- 実ホスト名
- Tailnet詳細
- 本番アクセスに結びつくSSHユーザー名
- API key、token、秘密鍵、webhook URL
- 実 `.env`
- 家の構成が分かるHome Assistant entity ID
- 実ログ、report、screenshot、backup
- 攻撃ヒントになるprivate production path

## 公開ルール

まずprivate repoで下書きし、手動でsanitizeしてから公開します。

Draft privately first, sanitize manually, then publish here only after review.
