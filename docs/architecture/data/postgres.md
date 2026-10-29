---
summary: PostgreSQL usage - schemas, roles, migrations, jsonb, Postgres-specific ports.
read_when: Writing migrations, repositories, or anything touching DB roles or schemas.
related: [../apps/instances.md, ../../decisions/0003-postgresql-only.md]
---
# PostgreSQL

- One database. Schemas: `platform` (core tables), `audit`, `queue` (Graphile Worker), one `a_<app>_<id>` per app instance.
- Roles: one per instance, limited to its schema plus granted views. Audit is `INSERT`-only for app roles.
- Platform migrations: Drizzle Kit. Instance migrations: our runner applies each app's migrations to every instance schema, tracking versions per schema.
- `jsonb` for flexible data: studio entity custom fields, workflow specs, settings, job input/output. Index what you query.
- Postgres-specific behaviour stays behind ports: `InstanceStorageProvisioner`, `UnitOfWork`, `JobQueue`, `ReadModelGateway`.
- Transactions: one Unit of Work per use case; audit, outbox and enqueue join it.
- Small-VPS tuning: modest `shared_buffers`, connection pool capped (the app uses one pool, `SET LOCAL search_path` per transaction).
