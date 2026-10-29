# ADR 0015: Single VPS with Docker Compose

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
Infrastructure cost matters over time.

## Decision
One VPS (target 2 vCPU / 4 GB) running Caddy, the server and PostgreSQL via Docker Compose; GitHub Actions deploys over SSH.

## Consequences
- Lowest cost; we own patching and backups (restore drills required).
- Same images run on a PaaS or cloud later.
