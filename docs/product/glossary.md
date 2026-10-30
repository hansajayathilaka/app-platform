---
summary: Definitions of the platform's core terms.
read_when: A term in another doc or in code is unclear.
---
# Glossary

| Term | Meaning |
| --- | --- |
| App | Code and version of a plugin module (`apps/<id>` or a studio-generated bundle) |
| App instance | A named install of an app, with its own slug, settings, secrets and Postgres schema. One app can have many |
| Binding | Which source instance an instance's import reads from (`instance_bindings`) |
| Catalog | The set of UI layouts and components (with Zod prop schemas) that page specs may use |
| Data file | YAML records an app ships in `data/`, `seed/` or `demo/`, matched by stable key |
| Descriptor registry | Store of each app version's protobuf descriptors, used for dynamic RPC dispatch |
| Field permission | A permission required to read or write one field; masked by the RPC dispatcher |
| Storage | A named file store (`default`, `public`, ...) behind the `FileStorage` port; backend chosen in config |
| Module | A supporting capability (contacts, record timeline, ...) installed once per deployment and plugged into apps via `ctx.module(id)`; not an app. A library module is stateless shared code imported directly |
| Reference data | Countries, currencies, rates, units, languages, timezones owned by the core |
| Message key | `{ t: key }` in place of literal text; resolved from i18n catalogs |
| Record rule | A named row scope (`own`, `team`) per table, chosen per role, enforced by Postgres RLS |
| Composition root | `services/server`: the only place that constructs adapters and wires them into use cases |
| Contract | A versioned, exported interface of an app: read model, command (`.proto`), or event |
| `ctx` | The SDK context passed to app code: db, authorize, audit, events, jobs, data, secrets |
| Page spec | A YAML description of a page, validated by JSON Schema, compiled to JSON, rendered by the shell |
| Port | An interface defined in the application layer (e.g. `UserRepository`, `JobQueue`) |
| Adapter | An implementation of a port (driven) or an entry point calling use cases (driving) |
| Record timeline | Comments, attachments and field-change history shown on a record |
| Principal | Who acts: a user, a service account, or an app instance (for jobs) |
| Permission | `<app>:<resource>:<action>`, declared in the manifest |
| Role assignment | Principal + role + scope (platform-wide or one instance) |
| Studio | The in-platform agent that generates apps as specs plus sandboxed code |
| Sandbox | V8 isolate runtime for studio-generated code steps |
| Use case | One application operation (command or query) |
