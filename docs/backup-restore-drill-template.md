# 復元訓練テンプレート / Backup Restore Drill Template

Public sanitized template for checking whether backups are actually restorable.

バックアップが「ある」だけでなく「戻せる」ことを確認するための公開用テンプレートです。

## 原則 / Principle

A backup is not complete until restore has been rehearsed.

復元手順を確認して初めてバックアップと言えます。

## 台帳 / Inventory

| Data | Source |
|---|---|
| app source | Git repository |
| Docker compose | Git repository or config backup |
| runtime `.env` | password manager |
| private keys | password manager |
| service data | backup archive |
| external monitor config | password manager + sanitized scripts |

## 非破壊の復元訓練 / Non-Destructive Drill

1. Clone repositories.
2. Confirm password manager item names exist without printing values.
3. List backup archives without extracting into production.
4. Validate compose files with placeholder or runtime env.
5. Prepare a temporary restore directory.
6. Write down the exact production restore order.

## 承認境界 / Approval Boundary

Require human approval before:

- overwriting production files
- restarting services
- rotating secrets
- deleting old backups
- rebooting hosts

## 記録 / Record

```text
date:
operator:
repos checked:
password-manager items checked by name:
latest backup timestamps:
gaps:
next action:
```
