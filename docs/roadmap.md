---
summary: Delivery phases, current phase, and open questions.
read_when: Planning work or deciding what to build next.
---
# Roadmap

**Current phase:** 0 - repository not created yet.

1. **Foundation**: monorepo, toolchain, CI, Docker Compose, Postgres, migrations, logging, `@platform/ui` skeleton with Storybook, docs and agent files.
2. **Core identity**: users, invitations, sign-in, sessions, RBAC with instance scopes, audit writer and viewer. Shell, `AppShellLayout`, `AuthLayout`, members and roles pages.
3. **App SDK and jobs**: YAML manifest, SDK as RPC-ready contract, instances (schema per instance, settings, bindings), Graphile Worker, scheduler, checkpointed long jobs, runs page, secrets, page specs + json-render. First real automation app.
4. **Cross-app data**: read views, commands, outbox events, event triggers, Integrations page. Second app consuming the first.
5. **Spec runtime and sandbox**: entities and workflow graphs from specs, isolate sandbox, egress proxy, app versions, review screen.
6. **App studio**: agent, tools, mock-adapter previews, quotas.
7. **Hardening**: API tokens, TOTP/passkeys, SSO, rate limits, OpenTelemetry, backups and restore drill, audit retention.

## Open questions
- Who approves studio apps before install: the requester or an admin?
- Will other developers hand-write apps?
- Rough job volume.
- GitHub repository name.
