---
summary: The ctx contract every app codes against, in-process or over RPC from the sandbox.
read_when: Writing app use cases, or changing the SDK.
related: [cross-app-data.md, ../studio/sandbox.md]
---
# App SDK (`ctx`)

App code never imports the DB client, auth or queue. It receives `ctx`, scoped to the principal and the app instance:

| Member | Purpose |
| --- | --- |
| `ctx.db` | Connection with the instance's role, `search_path` = its schema, record-rule scopes set for the principal |
| `ctx.principal`, `ctx.instance` | Who is acting, which instance |
| `ctx.authorize(perm, resource?)` | Permission check, throws on deny |
| `ctx.audit(event)` | Audit record in the current transaction |
| `ctx.events.publish(type, payload)` | Outbox event, same transaction |
| `ctx.jobs.enqueue(job, input, opts)` | Queue a job, same transaction |
| `ctx.data.from(importName)` | Read another instance's exported read model via the binding |
| `ctx.call(importName, command, input)` | Call another app's exported command |
| `ctx.platform.contacts`, `ctx.platform.reference` | Shared contacts, countries, currencies, units (`../core/master-data.md`) |
| `ctx.i18n.t(key, args, locale?)` | Translated text for emails and job output (`../ui/i18n.md`) |
| `ctx.settings`, `ctx.secrets.get(name)` | Instance settings and encrypted secrets |
| `ctx.http` | Outbound fetch through the egress proxy (timeouts, allowlist, logging) |
| `ctx.log`, `ctx.clock` | Structured logs, injectable time |

The SDK is a serializable RPC contract with two implementations: in-process (hand-written apps) and RPC (sandboxed studio apps). Mock implementations back tests and studio previews.
