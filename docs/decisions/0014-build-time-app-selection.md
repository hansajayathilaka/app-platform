# ADR 0014: Apps enabled at build time; empty install by default

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
Unused apps should not ship code or tables.

## Decision
The deployment config lists enabled hand-written apps; only those are bundled and migrated. A fresh system has no instances installed.

## Consequences
- Smaller builds and databases.
- Enabling a hand-written app needs a redeploy (studio apps do not).
