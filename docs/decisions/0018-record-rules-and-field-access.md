# ADR 0018: Record rules and field-level access on top of RBAC

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
Instance-scoped RBAC (ADR 0006) answers "may this user read leads in Sales CRM" but not "only their own leads", and it cannot hide a field such as margin. These are among the most common needs of internal tools (Odoo solves them with `ir.rule` and field groups). Cross-instance read views also need them: a granted view returns every row, whatever the end user may see in the source instance.

Options considered: filters in every repository (easy to forget), OpenFGA (heavy, a second store), Postgres row-level security driven by per-transaction settings.

## Decision
- **Record rules.** The manifest declares named row scopes per table (`own`, `team`, ...) as structured filters, never raw SQL. A role grants, per permission, which scope applies (`all` when unset). Scopes from several roles combine with OR.
- Migrations generate a Postgres **RLS policy** per ruled table (`USING` for read, `WITH CHECK` for write). The SDK computes the caller's effective scope and sets it with `SET LOCAL app.scope.<table>` beside `app.principal_id`. A missing setting matches no rows (fail closed).
- **Field permissions.** The manifest marks sensitive fields with a permission. The RPC layer masks them in responses and rejects writes to them, `GetPage` drops them from specs, and the `ReadModelGateway` masks them in cross-app reads.
- Cross-app views are created `WITH (security_invoker = true)` so the source tables' RLS applies to the caller.
- OpenFGA stays the option for per-record sharing between named users, behind the same `authorize`.

## Consequences
- "Own records only" is configuration, not code, and is enforced by Postgres even if app code forgets.
- Every query runs under RLS: the instance role must not own its tables (owner bypasses RLS), so migrations run as a separate owner role.
- RLS predicates must be indexed (`owner_id`, `team_id`); the policy generator adds indexes.
- Field masking depends on the descriptor registry (ADR 0019) to find marked fields.
