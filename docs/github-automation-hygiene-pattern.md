# GitHub自動運用のノイズ抑制パターン

定期automationがIssueやdraft PRを増殖させないための公開用パターンです。

## 単一Issueを再利用する

毎日のcanaryや定期監視は、実行ごとに新規Issueを作らず、同じ目的の完了済みIssueを再利用します。

1. 未処理の同名Issueがあれば更新する。
2. 完了済みの同名Issueがあれば、完了labelを外して再利用する。
3. どちらもない場合だけ新規作成する。
4. 実行結果はcommentまたは更新時刻で履歴化する。

## draft PRを統合する

同じ定期Issueから複数draft PRが作られた場合、日付だけで最新を選びません。

- 変更ファイルと目的でgroup化する。
- 各groupの有効な最新版をintegration branchへ集約する。
- 構文、unit test、diff checkを実行する。
- 統合PRをmerge後、旧draftへ統合先をcommentしてcloseする。

自動で全draftをmergeしたり、未確認のbranchを削除したりしないでください。

## Repo設定の軽量baseline

- vulnerability alertsを有効化する。
- public repoではsecret scanningとpush protectionを有効化する。
- merge後branch自動削除を有効化する。
- branch protectionは既存の直接push automationをPR方式へ移行してから有効化する。

## 定期整合性チェック

- default branchとremote/local HEAD
- dirty worktree
- open PR、open Issue、重複queue
- 直近Actionsのfailureとin-progress
- Dependabot/security alert
- 完了labelなのにopenのIssue
- 旧queueと現行queueの併存

closeやlabel更新には理由を日本語など運用言語で残し、履歴を削除しない運用にします。
