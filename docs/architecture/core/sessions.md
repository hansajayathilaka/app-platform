---
summary: Opaque server-side sessions - cookie, storage, timeouts, rotation, CSRF.
read_when: Changing session handling, cookies, logout or CSRF.
related: [identity-and-auth.md]
---
# Sessions

- Opaque random id in an `HttpOnly; Secure; SameSite=Lax` cookie. Only its hash is stored: `sessions(id_hash, user_id, created_at, last_seen_at, expires_at, ip, user_agent)`.
- Sliding idle timeout plus absolute lifetime (default 7 days idle, 30 days absolute; configurable).
- Rotate on login and privilege change. Logout, password change and admin revoke delete rows: revocation is immediate.
- CSRF: SameSite cookie plus double-submit token on state-changing calls.
- Users can list and revoke their sessions.
- Port: `SessionStore`. Not JWTs (ADR 0005).
