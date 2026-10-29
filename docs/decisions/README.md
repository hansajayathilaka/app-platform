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

Template: `template.md`.
