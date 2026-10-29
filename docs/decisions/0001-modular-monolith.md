# ADR 0001: Modular monolith with plugin apps

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
Small automation apps on a shared core, one developer, one VPS. Microservices would cost ops time and RAM.

## Decision
One deployable (API, scheduler, workers) plus PostgreSQL. Apps are plugin modules that reach the core and each other only through the SDK. Studio apps run out of process in a sandbox with the same SDK.

## Consequences
- One deploy, easy refactors, low cost.
- Boundaries must be enforced by tooling (dependency-cruiser, per-instance DB roles), not convention.
