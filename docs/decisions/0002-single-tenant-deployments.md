# ADR 0002: One deployment per organisation (no multi-tenancy)

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
The platform is not public. Separation inside an organisation is achievable with app instances and instance-scoped permissions.

## Decision
No tenant ids, no RLS on app tables, no tenant switcher. A separate customer gets a separate deployment.

## Consequences
- Simpler schema, code and UI; subdomain-based tenant routing is not needed.
- Everyone in a deployment shares one user directory and audit log; admins see everything.
- Groups that must never see each other need separate deployments.
