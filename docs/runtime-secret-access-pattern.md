# runtime secret access pattern / Runtime Secret Access Pattern

This public pattern describes how to let automation use secrets without turning
source control, chat, or logs into a secret store.

## Goal

Use the password manager as the source of truth, but avoid repeated direct API
reads from every local job. Prefer a local broker or Connect-style service for
runtime automation.

## Recommended order

1. Existing non-secret status artifacts: health, freshness, timestamps, counts.
2. Local broker / Connect-style service for runtime secret reads.
3. Service account CLI fallback for bootstrap and recovery.
4. Interactive desktop approval for one-off human work.

## What can be cached

Safe to cache:

- `ok`, `watch`, `failed` states
- timestamps
- item names or reference names when they are not sensitive
- last successful check metadata
- redacted error classes

Do not cache:

- token values
- API keys
- private keys
- webhook URLs
- OAuth client secrets
- raw `.env` contents

## Rate limit behavior

If the password manager returns rate-limit or transient errors:

- stop retrying in a tight loop
- keep the last known non-secret status
- mark the runtime state as `watch`, not `failed`, when service impact is unknown
- retry with exponential backoff
- create one attention issue instead of repeated notifications

## Apply boundary

Secret reads can be part of an autonomous production apply only when:

- the secret value is never printed
- the target command is narrow and known
- temporary files are owner-only and deleted afterward
- the runbook records preflight, rollback, postflight, and reporting
- credential lifecycle changes are out of scope

Credential issue, revoke, rotation, permission expansion, or secret display still
requires explicit owner approval.
