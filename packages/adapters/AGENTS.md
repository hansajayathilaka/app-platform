# adapters

- One package per technology and purpose: `persistence-postgres`, `auth-better-auth`, `queue-graphile`, `sandbox-isolates`, `mail-smtp`, `llm-claude`.
- An adapter implements ports from `core-application` and must pass their contract suites.
- Library types never leak out of the adapter.
- Only `services/server` imports adapters.
- Postgres: one Unit of Work per use case; `SET LOCAL search_path` for instance schemas. See `docs/architecture/data/postgres.md`.
