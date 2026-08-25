# Tailscale最小権限化の段階移行パターン

既存のremote accessを止めずに、広いtailnet policyを段階的に縮小するための公開用パターンです。実IP、machine名、Tailnet詳細は扱いません。

## Roleを先に分ける

| Role | 代表的な許可 |
|---|---|
| general client | 内部reverse proxyのHTTPSと専用DNS |
| admin client | general clientに加え、限定した管理port |
| runtime | 承認済み外部APIへのApp Connector通信 |
| deploy | 単一runtimeへのdeploy port |
| connector | 管理者からの限定SSHとApp Connector転送 |

identity全体へのallow-allは、端末1台の侵害を全machine・subnetへ拡大させます。端末tagまたはprivate policy内のstable addressでroleを分離します。

## 段階

1. 現行通信を変えず、accept/denyのpolicy testを追加する。
2. 未使用SSH rule、不要なtag ownerなど影響の小さい権限から除去する。
3. owner allow-allを、HTTPS、DNS、限定管理portへ分割する。
4. Internet/App Connector grantを利用runtimeだけへ限定する。
5. 24時間以上観測し、問題がなければ次の段階へ進む。

各段階を別保存にし、直前policyへ戻せるようにします。DNS、route、ACL、認証key、Firewallを同時に変更しないことが重要です。

## 最低限のpostflight

- 一般clientから内部HTTPS/DNS
- admin clientから限定管理port
- deploy元からdeploy先だけ
- App Connectorの対象APIと冗長性
- 一般clientから管理portがdeny
- runtime、監視、Portalなど既存サービス

Exit Node利用者がいる場合、Internet grant削除は一般通信断につながります。変更前に利用中clientを確認してください。
