# ADR 0004: One Postgres schema per app instance

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
Apps can be installed several times under different names. Options: shared schema with instance_id, or schema per instance.

## Decision
Each instance gets its own schema and DB role. Imports are bound to a chosen source instance (instance_bindings), with platform-wide default instances and an Odoo-style Settings/Integrations page.

## Consequences
- Strong isolation; easy per-instance backup and drop; app code has no instance_id.
- Migrations run per instance; cross-instance reports need UNION. Acceptable at tens to hundreds of instances.
