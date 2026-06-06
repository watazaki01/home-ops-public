# 一時障害に強いstatus refresh / Resilient Status Refresh Pattern

Public-safe pattern for refreshing related operational status artifacts without
turning a short service restart into a persistent false alarm.

短いservice再起動を長時間の誤警報にせず、関連するstatus artifactを正しい順序で更新するための公開パターンです。

## 問題 / Problem

Operational dashboards often combine several generated files:

- service health
- dependency readiness
- improvement proposals
- daily summary

If they are generated separately, a newer health file can coexist with an older
proposal or summary. The dashboard then continues to show an incident that has
already recovered.

個別に再生成すると、新しいhealthと古い改善提案が混在し、復旧済み障害が表示に残ることがあります。

## 原則 / Rules

1. Retry only transient connection failures.
2. Keep authentication, permission, and rate-limit failures classified separately.
3. Refresh dependent artifacts in a fixed order.
4. Load runtime credentials once at the process boundary.
5. Never print credential values.
6. Record which generators failed instead of silently serving stale files.
7. Re-run the refresh after a self-heal completes.

## 推奨順序 / Recommended Order

```text
service health
  -> dependency readiness
  -> improvement proposals
  -> daily summary
  -> dashboard API
```

The order matters. Improvement proposals should consume the latest health, and
the daily summary should consume all latest artifacts.

## 一時障害の再試行 / Transient Retry

Use a small bounded retry window:

```text
attempts: 3-6
delay: 2-5 seconds
total grace period: about 10-20 seconds
```

Retry connection refusal, timeout, and service-unavailable responses. Do not
hide persistent authentication or permission failures behind retries.

Record recovery evidence such as:

```json
{
  "state": "ok",
  "recovered_after_retries": 2
}
```

## Refresh wrapper example

```sh
#!/bin/sh
set -eu

failed=""

run_step() {
  name="$1"
  command_path="$2"

  if [ ! -x "$command_path" ]; then
    failed="${failed}${failed:+,}${name}:missing"
    return
  fi
  if ! "$command_path" >>status-refresh.log 2>&1; then
    failed="${failed}${failed:+,}${name}"
  fi
}

# Load an approved runtime environment here without echoing it.
run_step service_health ./generate-service-health
run_step dependency_readiness ./generate-dependency-readiness
run_step improvement_proposals ./generate-improvement-proposals
run_step daily_summary ./generate-daily-summary

printf '{"ok":%s,"failed_tools":"%s"}\n' \
  "$([ -z "$failed" ] && printf true || printf false)" \
  "$failed"

[ -z "$failed" ]
```

Use atomic writes inside every generator: write to a temporary file, validate
it, then replace the previous artifact.

## Self-heal integration

After a narrowly scoped self-heal:

1. Confirm the service is stable for several probes.
2. Run the complete refresh wrapper.
3. Confirm old active proposals moved to monitoring or completed state.
4. Confirm the daily summary no longer reports the recovered incident.
5. Stop if the service enters a restart loop.

The self-heal must not broaden into credential changes, network policy changes,
or a full stack restart unless those actions are separately approved.

## Public safety

Public examples should contain only:

- generic service names
- generic paths
- status levels and timestamps
- redacted error classes

Do not publish real endpoints, hostnames, internal addresses, entity IDs,
credential references, logs, or production directory layouts.
