---
summary: How apps share data - read models, commands, events - and the versioning rules.
read_when: An app needs data from, or must notify, another app.
related: [instances.md, sdk.md]
---
# Cross-app data sharing

The owning app keeps control: others **read** exported views, **write** via exported commands, **react** to events. Never another app's tables.

| Mechanism | Owner exports | Consumer uses | Implemented as |
| --- | --- | --- | --- |
| Read model | Versioned entity, `crm.Customer v1` | `ctx.data.from('customers').list({ where, limit })` | SQL view in the **bound source instance's** schema, granted to the consumer instance's role |
| Command | `.proto` RPC, e.g. `CreateCustomer` | `ctx.call('customers', 'CreateCustomer', input)` | SDK call; owner's validation, authorize and audit run |
| Event | `crm.customer.created.v1` + payload schema | `subscribes` in manifest, or an automation trigger | Transactional outbox -> queued jobs, at-least-once, idempotent handlers |
| Platform entity | Users, tags, reference data | `ctx.platform.*` | Core schema (`../core/reference-data.md`) |
| Module API | Contacts, record timeline, ... | `ctx.module(id)` | Module schema `m_<id>` (`../modules/README.md`) |

Rules
- Contracts are versioned; breaking change = new `v2` beside `v1`. `v1` goes when no instance imports it.
- Permissions apply: a consumer acting for a user sees only what that user may see in the source instance. Views are `security_invoker`, so the source tables' record rules (RLS) run for the caller, and the `ReadModelGateway` masks fields the caller may not read (`../core/permissions.md`).
- Cross-app references are `{ app, instance, type, id }`, not foreign keys.
- Heavy cross-app reporting goes to a read replica or warehouse later, not wider views.
