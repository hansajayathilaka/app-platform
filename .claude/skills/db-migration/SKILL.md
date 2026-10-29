---
name: db-migration
description: Create a database migration for the platform schema or for an app (applied per instance). Use for any table or column change.
---
# Database migration

Read first: `docs/architecture/data/postgres.md`.

- Platform tables: change the Drizzle schema in `packages/adapters/persistence-postgres`, then `pnpm db:generate`.
- App tables: add a numbered SQL file in `apps/<id>/migrations/`. It runs in every instance schema: no schema names, no `instance_id`.

Rules
1. Expand then contract: add new columns/tables first; remove old ones a release later.
2. Never edit an applied migration.
3. Large backfills run as a job, not in the migration.
4. `pnpm db:migrate` locally, run integration tests, check `pnpm check`.
