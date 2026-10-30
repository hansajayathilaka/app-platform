---
summary: Supporting modules - what a module is, module.yaml, ctx.module, schemas, providers, dependencies and boundaries.
read_when: Writing a module, using one from an app, or deciding whether something is core, a module or an app.
related: [../apps/app-model.md, ../apps/manifest.md, ../../decisions/0024-pluggable-supporting-modules.md]
---
# Supporting modules

A module is a reusable capability that apps plug in when they need it (ADR 0024). It is not an app: it is installed once per deployment, has no instances, and serves every app that requires it.

| Question | Core | Module | App |
| --- | --- | --- | --- |
| Needed by every deployment? | Yes | No | No |
| Installed per instance? | No | No | Yes |
| Exposes | Ports, `ctx` | One typed API via `ctx.module()` | Versioned contracts |

## Modules today
| Module | Covers | Doc |
| --- | --- | --- |
| `contacts` | People and organisations shared by all apps | `contacts.md` |
| `record-timeline` | Comments, attachments, change history on records | `record-timeline.md` |

## Library modules
A module can be just a library: `library: true` in `module.yaml`, no schema, no API, no `ctx`, no I/O. Apps and modules import it directly (`modules/money`, `modules/state-machine`). Use it to share code between apps instead of copying it.

Candidates, built only when a real app needs them (decided 2026-10-30: no module is built up front): notifications, email, files (UI on top of the core `FileStorage` port), connections (OAuth), outbound webhooks, search, documents.

## module.yaml
```yaml
# yaml-language-server: $schema=https://<platform>/schemas/module/v1.json
id: notifications
version: 1.0.0
requires: { email: ^1 }        # other modules
api: contract/                 # typed, serializable API -> ctx.module('notifications')
schema: true                   # owns Postgres schema m_notifications
permissions: [notifications:preference:write]
settings: {}                   # deployment-level settings page (JSON Schema subset)
appConfig: {}                  # JSON Schema for per-app config in app manifests
providers:                     # adapters chosen in deployment config
  channel: [in-app, email]
contributes:
  services: proto/notifications/v1/service.proto
  pages: specs/pages/
  components: ui/              # namespaced: notifications.Bell
  jobs: { digest: { handler: SendDigest, schedule: "0 8 * * *" } }
events: [notifications.sent.v1]
i18n: i18n/
```

## Using a module from an app
```yaml
# app.yaml
modules:
  contacts: ^1
  record-timeline:
    version: ^1
    records: { lead: { table: leads, comments: true, history: [stage] } }
```
```ts
const contact = await ctx.module('contacts').get(contactId)
```
- Types come from the module's `contract` package. Studio apps use the same API over RPC from the sandbox.
- The build includes every module that an enabled app requires; installing an app whose modules are missing is refused.

## Inside a module
- Folders: `module.yaml`, `contract/`, `domain/`, `application/`, `adapters/`, `proto/`, `specs/`, `ui/`, `migrations/`, `i18n/`.
- Runs in-process as trusted code with a module context: `db` (schema `m_<id>`, owner rules as for instances), `authorize`, `audit`, `events`, `jobs`, other modules' APIs.
- App data only through core ports, for example `RecordAccess.canRead(principal, ref)`, which runs the check under the app's record rules.
- Migrations run once per deployment at deploy, in one transaction, after a schema dump (ADR 0023).

## Boundaries
- The core never imports a module. Apps and modules import only a module's `contract` package, or a library module whole; dependency-cruiser enforces it. Library modules import nothing but other library modules.
- A module API is a platform contract: SemVer, and a breaking change is a new major version.
