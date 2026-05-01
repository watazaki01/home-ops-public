# メンテナンス時間帯設計 / Maintenance Window Pattern

Public sanitized template for choosing safe maintenance windows in a home-lab.

自宅サーバーやNASを安全に更新するための、公開可能な時間帯設計テンプレートです。

## ルール / Rule

Do not pick a maintenance window by intuition. First inventory scheduled work.

勘で時間を決めず、先に定期ジョブを棚卸しします。

## 確認 / Check

- cron / systemd timers
- Docker backup containers
- Home Assistant automatic backups
- external monitoring heartbeat
- cloud sync jobs
- router or VPN update jobs
- family usage patterns

## 避けること / Avoid

- backup windows
- VPN/Tailscale update windows
- monitoring heartbeat transition windows
- just before daily reports
- times when no one can recover manually

## よい時間帯の形 / Good Window Shape

A good window has:

- recent backup before the change
- no backup job starting soon
- enough time before the next scheduled job
- a human available for rollback
- clear owner approval for reboot

## 承認テンプレート / Approval Template

```text
Can I start maintenance at <time>?

Expected impact:
- ...

Avoided windows:
- backup: ...
- monitoring: ...
- VPN/network: ...

Preflight:
- backup: ok
- disk/load: ok
- containers: ok
- remote access: ok

Rollback:
- ...
```
