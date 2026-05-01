# home-ops-public

Sanitized public reference templates for home operations with Docker, Home Assistant, monitoring, and Codex handoff.

This repository is intentionally public. Do not copy private operational files here.

## Purpose

Share reusable ideas without exposing the real home environment:

- Codex handoff templates
- Secret handling templates
- Docker monitoring patterns
- Home Assistant + AI architecture notes
- GitHub Actions concepts
- Rule-based checks before paid API calls
- Public-safe runbooks

## Never Publish

- Real IP addresses
- Real hostnames
- Tailnet details
- SSH usernames tied to production access
- API keys, tokens, private keys, or webhook URLs
- Real `.env` files
- Home Assistant entity IDs that expose private home layout
- Real logs, reports, screenshots, or backup files
- Exact private production paths if they increase attack usefulness

## Suggested Structure

```text
docs/
  codex-handoff-template.md
  secret-handling-template.md
  docker-monitoring-pattern.md
  home-assistant-ai-pattern.md
examples/
  env.example
  docker-compose.example.yml
  github-actions-example.yml
```

## Publishing Rule

Draft privately first, sanitize manually, then publish here only after review.
