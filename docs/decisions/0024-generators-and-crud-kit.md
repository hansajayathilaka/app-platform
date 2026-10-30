# ADR 0024: Generators, CRUD kit, test kit and starter templates

- **Status:** Proposed
- **Date:** 2026-10-30
- **Deciders:** Hansa

## Context
The main goal is to build small to large apps quickly without rebuilding the same things. Today one CRUD entity touches about 9 kinds of artefact (migration, Drizzle schema, domain, repository port and adapter, use cases, proto, handlers, page specs, i18n, manifest entries): 20 to 30 files of mostly identical shape. A runtime entity model (Odoo's `ir.model`) was declined on 2026-10-29; the answer here has to be ordinary code the engineer owns.

## Decision
- **Generators** (`pnpm gen:<kind>`, templates in `tools/generators/`):
  - `gen:app <id>` from a starter template: `crud`, `scheduled-job`, `webhook-integration`.
  - `gen:entity <app> <name>` from a short YAML (`fields`, types, relations, `owner` field, `list`/`form` columns) writes the migration, Drizzle schema, domain entity, repository, use cases, proto messages and methods, handlers, list/form/detail specs, i18n keys and manifest entries, plus tests.
  - `gen:use-case`, `gen:page`, `gen:job`, `gen:rpc` for single pieces.
  - Output is plain committed code. Regenerating never overwrites edited files; it reports a diff.
- **CRUD kit** (`packages/sdk/crud`): `defineCrud({ table, entity, permissions, events })` gives List, Get, Create, Update, Archive with `authorize`, `audit`, outbox events, record rules and `ListQuery` (ADR 0025) built in. Apps override hooks (`validate`, `beforeCreate`, `afterUpdate`) or replace a use case. Plain CRUD entities skip a hand-written domain layer; entities with real rules keep full hexagonal layering.
- **Test kit** (`packages/sdk/testing`): `createTestCtx()` with in-memory adapters, record factories, a fake clock, and assertion helpers for audit and events.

## Consequences
- A new CRUD entity takes about an hour instead of one to two days, and every app looks the same to people and agents.
- The Studio agent can call the same generators, which keeps its output consistent with hand-written apps.
- Templates are a maintained product: CI generates a fresh app from each template and runs `pnpm check` on it.
