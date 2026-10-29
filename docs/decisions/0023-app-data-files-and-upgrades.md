# ADR 0023: App data files and transactional instance upgrades

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
Apps need default records (pipeline stages, templates) and safe upgrades across many instances. Keep it simpler than Odoo's data files and pre/post/end scripts.

## Decision
- Three optional folders in an app, YAML records by table, each with a stable `key`:
  - `data/`: upserted on install and on every upgrade (records the app owns).
  - `seed/`: inserted on install only; users may edit them afterwards.
  - `demo/`: inserted only when the admin ticks "demo data" on install (also used by studio previews).
- Keys map to row ids in a per-instance `_data_keys(key, table, row_id)` table.
- Upgrade of one instance = one transaction: migrations, then `data/`, then the optional `OnUpgrade(fromVersion)` use case. Before it starts, the platform dumps that instance schema (`pg_dump --schema`).
- A failed upgrade rolls back. The instance is marked `needs_attention`: jobs paused, UI shows maintenance, other instances continue.

## Consequences
- No half-migrated instance; one bad instance does not block the rest.
- Long migrations hold locks for their duration; acceptable at this scale, but keep migrations small.
- A per-instance schema dump is the restore path for a bad upgrade.
