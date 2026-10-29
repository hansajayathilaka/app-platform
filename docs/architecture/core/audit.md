---
summary: Append-only audit log written in the same transaction as each change.
read_when: Writing any use case that changes state, or working on the audit viewer.
related: [permissions.md]
---
# Audit logging

- `audit_events(id, occurred_at, actor_type, actor_id, on_behalf_of, app_instance_id, action, target_type, target_id, request_id, ip, user_agent, changes jsonb, metadata jsonb)`.
- Written through `ctx.audit(...)` / `AuditWriter` **inside the same Unit of Work** as the change. No audited change commits without its record.
- Sensitive fields redacted by a per-entity allowlist.
- Append-only: app roles have `INSERT` only. Monthly partitions; old partitions exported per retention policy. Optional hash chain.
- Covers auth events, admin events (roles, installs, bindings), data changes, job runs, secret reads, studio actions.
- Application logs and traces are separate (pino, OpenTelemetry).
