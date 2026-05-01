# 公開release基準

`home-ops-public` をGitHub Pagesへ公開する前に確認する基準です。

## releaseしてよい状態

- 実secret、token、秘密鍵、webhook URLが含まれていない。
- 実IPアドレス、実ホスト名、Tailnet名、production user名が含まれていない。
- 実 `.env`、実ログ、実report、実backup、実screenshotが含まれていない。
- Home Assistant entity IDなど、家の間取りや生活パターンが推測できる情報が含まれていない。
- private repo名やprivate runtime pathを、攻撃ヒントになる形で書いていない。
- 内容が「実運用の正本」ではなく、sanitized template / pattern / runbookとして読める。
- 日本語を正本にし、必要な箇所だけ英語を補助として併記している。

## release前チェック

```bash
# 実運用では secret / token / private key / webhook URL / private IP の検出patternを使って確認する。
# pattern文字列そのものは、検査コマンドが自己一致しないようCIやlocal script側に分離する。
rg -n --glob '!**/.git/**' --glob '!docs/release-criteria.md' "<secret-or-private-infra-pattern>" .
```

上のチェックで一致が出た場合は、公開前に必ず人間が確認します。

## GitHub Pages公開ルール

- Pagesは `main` branchのrootから公開する。
- 公開トップは `index.md` とする。
- `README.md` はGitHub repo上の説明、`index.md` はPages用の入口として扱う。
- 公開後も、private repoからの転記は必ずsanitizeしてからcommitする。

## 変更の扱い

軽微なdocs修正はmainへ直接commitしてよいです。

次の変更はPRまたは明示的なowner確認を推奨します。

- 新しい運用パターンを追加する変更
- 実環境に近い例を追加する変更
- screenshotやログ断片を追加する変更
- 外部リンクや連絡先を追加する変更

## 公開後の確認

- GitHub Pages URLが開ける。
- トップページから主要docsへ移動できる。
- GitHub repo description / homepage URL が現在の公開URLと一致している。
