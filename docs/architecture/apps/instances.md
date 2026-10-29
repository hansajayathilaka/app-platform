---
summary: Installing an app more than once - schema per instance, migrations, settings, integrations and default instances.
read_when: Working on install/uninstall, migrations, instance settings or bindings between apps.
related: [cross-app-data.md, ../data/postgres.md, ../../decisions/0004-schema-per-instance.md]
---
# App instances

An app can be installed many times under different names. **Each instance gets its own Postgres schema.**

- `app_instances(id, app_id, slug, name, version, schema_name, status, settings)`. Schema `a_<app>_<short id>` (under the 63-char limit).
- Install: create schema + DB role, run the app's migrations there. Uninstall: rename to `archived_…`, drop after retention.
- Migrations run per instance; each schema has its own migrations table. A failure marks only that instance degraded.
- The SDK sets `search_path` per transaction (`SET LOCAL`), so app code has no `instance_id` column and is identical for one instance or ten.
- URLs, permissions, schedules, run history and audit are keyed by instance slug: `/apps/sales-crm/…`.
- `multiInstance: false` in the manifest for apps that must exist once.
- Trade-offs: strongest isolation and easy per-instance backup/drop; deploys slow as instances grow; cross-instance reports need `UNION`. Fine at tens to hundreds of instances.

## Settings and integrations (Odoo-style)
- Every instance has a Settings page generated from the manifest `settings` schema.
- **Integrations** section: one dropdown per import listing compatible instances. Changing it updates `instance_bindings`, regrants views, and is audited.
- Platform-wide **default instances** ("default CRM = Sales CRM") pre-fill bindings on install.
- If a bound instance is uninstalled, consumers show "needs setup" and their jobs pause.
