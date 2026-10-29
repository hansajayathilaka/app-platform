---
summary: Permission strings, roles, instance-scoped role assignments and the single authorize check.
read_when: Adding permissions, guarding a use case, or changing roles.
related: [../apps/manifest.md, ../apps/instances.md, ../../decisions/0006-rbac.md]
---
# Permissions (RBAC)

- Permission = `<app>:<resource>:<action>`, e.g. `platform:users:invite`, `crm:customer:write`. Apps declare theirs in the manifest.
- Role = named set of permissions, defined by admins. Built-in: Owner, Admin, Member, Viewer. Apps can suggest roles (`CRM Editor`).
- `role_assignments(principal_id, role_id, scope)`: scope is `platform` or one app instance ("editor of Sales CRM"). Instance-scoped is the normal case. Every instance is deny-by-default.
- One check everywhere: `ctx.authorize('crm:customer:write', resource?)`, called inside use cases, never only in the UI.
- The server filters page specs by permission (`../ui/server-driven-ui.md`); the client also hides controls, but the server is the authority.
- Per-record sharing, if ever needed: add a relationship layer (e.g. OpenFGA) behind the same `authorize`.
