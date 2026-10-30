---
summary: The big picture - static shell, apps, SDK, platform core, workers, PostgreSQL.
read_when: Making a change that crosses components, or orienting yourself.
related: [code-architecture.md, ../decisions/0001-modular-monolith.md]
---
# System overview

A **modular monolith on PostgreSQL**. One Node process (API, scheduler, workers) plus Postgres, behind Caddy, on one VPS.

```text
Browser: static React shell ---- Connect RPC (+ webhooks) ----+
                                                               |
  Apps (hand-written in-process, studio-generated in sandbox)  |
        |  only through                                         |
  App SDK (ctx): db, authorize, audit, events, jobs, data, secrets
        |
  Supporting modules (plugged in when needed): contacts | record timeline | ...
        |
  Platform core: identity & sessions | RBAC | audit | data contracts | event outbox | scheduler & queue
        |
  PostgreSQL: platform schema, audit, queue, one schema per app instance
```

- The shell is static; every app page is a spec returned by the server (`ui/server-driven-ui.md`).
- Apps never call each other or the core directly; everything goes through `ctx` (`apps/sdk.md`).
- Workers are the same codebase; on a small box they run in the same process as the API.
- One deployment serves one organisation (ADR 0002).
