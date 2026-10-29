---
summary: Users, principals, login methods and API tokens.
read_when: Working on login, invitations, OAuth, API tokens or service accounts.
related: [sessions.md, permissions.md, ../../decisions/0005-auth-and-sessions.md]
---
# Identity and authentication

- One organisation per deployment: `users`, `invitations`. No public signup; admins invite.
- **Principals**: `user`, `service_account` (API tokens), `app_instance` (the identity a job runs as). One permission check handles all three.
- Authentication is delegated to a self-hosted library (Better Auth) behind the `AuthProvider` port. The provider only proves identity; sessions, roles and permissions are ours.
- Day one: email + password (argon2id), magic links, Google and GitHub OAuth. Later: TOTP, passkeys, OIDC/SAML SSO.
- Rate-limit login, lock out after repeated failures, notify on new devices.
- API tokens: 32 random bytes with a visible prefix (`plt_…`), stored hashed, scoped to a permission subset, optional expiry.
- Each request sets `app.principal_id` on the DB connection for grants and audit.
