# IoT通信ログから最小権限Firewall候補を作るパターン

IoT通信ログを端末別に集計し、Firewall候補へ変換する際の公開用ガードレールです。

## 分類

- 内部通信: 宛先role、protocol、portで整理する。
- Internet通信: FQDNまたはservice単位を優先する。
- public IPしか分からない通信: 恒久許可せずobserve-onlyにする。
- block済み通信: allow候補へ自動変換しない。
- OSのephemeral宛先port: 応答通信や集計誤差の可能性があるためreview扱いにする。

特に高位portを単純に「内部で観測されたから必要」と判断すると、一時的なsession portを恒久allowへ昇格させる危険があります。

## 推奨する判定条件

- 通常端末は7 distinct days以上観測する。
- 低頻度端末は14 days以上観測する。
- 端末、FQDN/service、protocol、portが安定したものだけを候補にする。
- vendor資料または再現試験がない単発通信は許可しない。
- 観測期間中は既存の広いallowを急に削除せず、段階移行する。

## Privacy

公開artifactには次を出しません。

- source IP、MAC、実device名
- public destination IP
- policy ID、site ID、network ID
- credential、token、実ログ

sourceはhash aliasへ変換し、内部宛先も公開成果物ではrole名へ置き換えます。raw logと詳細reportはprivate cacheへ保存し、Git管理外にします。

## Firewall変更時の順序例

1. established/related
2. Gateway DNS
3. 必要な内部service
4. 内部横断block + logging
5. 端末別FQDN/service allow
6. 観測用Internet allow
7. 収束後のInternet default block + logging

変更後はDNS、主要IoT機能、Home Automation、監視、外部serviceを確認します。
