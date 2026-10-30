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

Candidates, built only when a real app needs them (decided 2026-10-30: no module is built up front): notifications, email, files (UI on top of the `storage` type), connections (OAuth), outbound webhooks, search, documents.

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
uses:                          # module types this module needs (see Module types)
  email: { type: ^1, bindings: { default: email-smtp } }
contributes:
  services: proto/notifications/v1/service.proto
  pages: specs/pages/
  components: ui/              # namespaced: notifications.Bell
  jobs: { digest: { handler: SendDigest, schedule: "0 8 * * *" } }
events: [notifications.sent.v1]
i18n: i18n/
```

## Module types and providers
- A **type** is a versioned interface with a contract test suite, in `packages/module-types/<type>`: `storage`, later `email`, `sms`, `payments`, `llm`, `search`.
- A **provider module** implements one: `provides: storage@1.2.0` in its `module.yaml` (`storage-local`, `storage-s3`). Several can be installed together. Their configuration lives in connections (below).
- Apps and modules bind providers by name under `uses` and code against the type only: `ctx.use('storage', 'archive')`.
- **Connections** hold provider configuration and credentials (next section).
- **Compatibility check** (build and install): bound providers and required modules must satisfy the declared SemVer ranges, and providers must pass the type's contract suite in CI. A mismatch fails with app, type, expected range and found version.
- **Runtime switching (later, room kept):** bindings are stored per instance (`instance_module_bindings`) and `ctx.use` resolves on each call, so an admin override can be added without app changes.

| Type | Providers | Doc |
| --- | --- | --- |
| `storage@1` | `storage-local`, `storage-s3` | `storage.md` |

## Provider connections (configuration and credentials)
A provider declares what it needs; admins create named connections; bindings point at one.

```yaml
# modules/storage-gcs/module.yaml
provides: storage@1.0.0
configSchema:
  bucket: { type: string }
  projectId: { type: string }
  prefix: { type: string, default: "" }
  serviceAccount: { type: string, format: file, secret: true, accept: application/json }
```
- **Create:** Admin, Modules, `storage-gcs`, New connection (`gcs-archive`). The form is generated from `configSchema`. Secret fields are write-only.
- **Store:** `module_connections(id, provider, name, config jsonb, secret_refs, created_by, updated_at)`. Secrets are envelope-encrypted like instance secrets, never returned to the UI, and every read is audited. Rotate by replacing the value; `ctx.use` resolves per call, so new credentials apply on the next call.
- **Check:** `validateConfig` (schema plus provider rules) and `testConnection` (for storage: write, read, delete a probe object) run on save and at install. Install does not finish while a binding's connection fails.
- **Bind:** in the manifest `archive: { provider: storage-gcs, connection: gcs-archive }`, or leave `connection` out and the install screen asks the admin to pick or create one.
- **Automate:** deployment config may predefine connections, with secrets referenced from the secret store: `serviceAccount: ${secret:GCS_SA_JSON}`.
- **Many connections per provider:** one provider module can have any number of connections (`gcs-archive`, `gcs-media`: two buckets, two service accounts). The platform creates one provider instance per (provider, connection), cached and rebuilt when the connection changes, so an app can bind both at once.
- **Isolation:** a provider instance only receives its own connection's config; apps never see the values.

## Using a module from an app
```yaml
# app.yaml
modules:
  contacts: ^1
  record-timeline:
    version: ^1
    records: { lead: { table: leads, comments: true, history: [stage] } }
```
uses:
  storage: { type: ^1, bindings: { default: storage-local } }
```
```ts
const contact = await ctx.module('contacts').get(contactId)
const files = ctx.use('storage')
```
- Types come from the module's `contract` package. Studio apps use the same API over RPC from the sandbox.
- The build includes every module and provider that an enabled app requires, after the compatibility check; installing an app whose modules are missing or incompatible is refused.

## Inside a module
- Folders: `module.yaml`, `contract/`, `domain/`, `application/`, `adapters/`, `proto/`, `specs/`, `ui/`, `migrations/`, `i18n/`.
- Runs in-process as trusted code with a module context: `db` (schema `m_<id>`, owner rules as for instances), `authorize`, `audit`, `events`, `jobs`, other modules' APIs.
- App data only through core ports, for example `RecordAccess.canRead(principal, ref)`, which runs the check under the app's record rules.
- Migrations run once per deployment at deploy, in one transaction, after a schema dump (ADR 0023).

## Boundaries
- The core never imports a module. Apps and modules import only a module's `contract` package, or a library module whole; dependency-cruiser enforces it. Library modules import nothing but other library modules.
- A module API is a platform contract: SemVer, and a breaking change is a new major version.
