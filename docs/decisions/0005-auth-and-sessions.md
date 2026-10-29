# ADR 0005: Self-hosted auth library, opaque server-side sessions

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
Need email/password, OAuth, later SSO, with low cost and full control.

## Decision
Better Auth (self-hosted) behind an AuthProvider port for identity. Our own opaque DB sessions (hashed ids, HttpOnly cookie), not JWTs.

## Consequences
- Instant revocation and rotation.
- Swapping the auth library touches one adapter.
