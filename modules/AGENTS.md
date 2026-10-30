# modules

- Supporting modules: reusable capabilities apps plug in (`contacts`, `record-timeline`). Not apps: one per deployment, no instances.
- Each module: `module.yaml`, `contract/` (the only thing others may import), `domain/`, `application/`, `adapters/`, `proto/`, `specs/`, `ui/`, `migrations/`, `i18n/`.
- Tables live in schema `m_<id>`. App data only through core ports (e.g. `RecordAccess`).
- The `contract` API must stay serializable (Studio apps call it over RPC). Breaking change = new major version.
- Docs: `docs/architecture/modules/`.
