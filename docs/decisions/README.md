# Architecture decision records

Read the ADR before changing anything it covers. To change a decision, write a new ADR that supersedes it (skill `write-adr`); never edit an accepted one except its Status line.

| ADR | Decision | Status |
| --- | --- | --- |
| 0001 | Modular monolith with plugin apps | Accepted |
| 0002 | One deployment per organisation, no multi-tenancy | Accepted |
| 0003 | PostgreSQL only, jsonb for flexibility | Accepted |
| 0004 | One Postgres schema per app instance | Accepted |
| 0005 | Self-hosted auth library, opaque server-side sessions | Accepted |
| 0006 | RBAC, app-declared permissions, instance-scoped grants | Accepted |
| 0007 | Hexagonal architecture without a framework | Accepted |
| 0008 | TypeScript on Node end to end | Accepted |
| 0009 | Connect RPC for APIs | Accepted |
| 0010 | Server-driven UI, YAML specs validated by JSON Schema | Accepted |
| 0011 | json-render as the web renderer | Accepted |
| 0012 | Graphile Worker, checkpointed long jobs, no Temporal | Accepted |
| 0013 | Studio apps: specs plus sandboxed code, human-approved | Accepted |
| 0014 | Apps enabled at build time, empty install by default | Accepted |
| 0015 | Single VPS with Docker Compose | Accepted |
| 0016 | Conventional Commits and Semantic Versioning | Accepted |
| 0017 | Dev Containers as the development environment | Accepted |
| 0018 | Record rules (RLS) and field-level access on top of RBAC | Accepted |
| 0019 | Dynamic RPC through a descriptor registry | Accepted |
| 0020 | Translatable text (message keys) from the first spec | Accepted |
| 0021 | Shared master and reference data in the core | Accepted |
| 0022 | Comments, attachments and change history on records | Accepted |
| 0023 | App data files and transactional instance upgrades | Accepted |
| 0024 | Generators, CRUD kit, test kit and starter templates | Proposed |
| 0025 | Standard list query and common contract types | Proposed |
| 0026 | Platform building-block services (files, notifications, email, connections, webhooks, search) | Proposed |
| 0027 | Shared stateless kits apps may import | Proposed |
| 0028 | App-scoped UI components for hand-written apps (amends 0010) | Proposed |

Template: `template.md`.
