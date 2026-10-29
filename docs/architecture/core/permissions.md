---
summary: Permission strings, roles, instance-scoped role assignments, record rules, field permissions and the single authorize check.
read_when: Adding permissions, guarding a use case, or changing roles.
related: [../apps/manifest.md, ../apps/instances.md, ../../decisions/0006-rbac.md, ../../decisions/0018-record-rules-and-field-access.md]
---
# Permissions (RBAC)

- Permission = `<app>:<resource>:<action>`, e.g. `platform:users:invite`, `crm:customer:write`. Apps declare theirs in the manifest.
- Role = named set of permissions, defined by admins. Built-in: Owner, Admin, Member, Viewer. Apps can suggest roles (`CRM Editor`).
- `role_assignments(principal_id, role_id, scope)`: scope is `platform` or one app instance ("editor of Sales CRM"). Instance-scoped is the normal case. Every instance is deny-by-default.
- One check everywhere: `ctx.authorize('crm:customer:write', resource?)`, called inside use cases, never only in the UI.
- The server filters page specs by permission (`../ui/server-driven-ui.md`); the client also hides controls, but the server is the authority.

## Record rules (row-level, ADR 0018)
The manifest declares named scopes per table as structured filters; a role picks one scope per permission.

```yaml
recordRules:
  leads:
    own:  { field: owner_id, op: eq, value: $principal.id }
    team: { field: team_id,  op: in, value: $principal.teams }
```

- Role editor: "CRM Editor: `crm:lead:read` = team, `crm:lead:write` = own". Unset = `all`. Several roles combine with OR.
- Migrations generate an RLS policy per ruled table; the SDK sets `SET LOCAL app.scope.<table>` per transaction. No setting = no rows.
- Variables: `$principal.id`, `$principal.teams`. Operators: `eq`, `in`, `is_null`, combined with `all`/`any`.
- Jobs: a run `on_behalf_of` a user gets that user's scopes; a system run gets `all` in its own instance.
- The instance role never owns its tables (owners bypass RLS); migrations run as a separate owner role.

## Field permissions
- `fieldPermissions` in the manifest (`leads.margin: crm:lead:read_margin`) and the matching proto field option `(platform.requires)`.
- The RPC dispatcher masks unreadable fields and rejects writes to unwritable ones; `GetPage` drops them from specs; `ReadModelGateway` masks them in cross-app reads.

## Later
- Per-record sharing between named users: a relationship layer (e.g. OpenFGA) behind the same `authorize`.
