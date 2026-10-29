# Feature development workflow

1. **Frame**: one issue with acceptance criteria. Identify the docs it touches via `docs/README.md`.
2. **Plan**: for anything beyond a small change, the `architect` subagent writes a short plan: affected layers, ports, contracts, specs, migrations, tests. If it conflicts with an ADR, stop and follow `architecture-change.md`.
3. **Branch**: `feat/<short-name>`.
4. **Contract first**: `.proto` and manifest/spec changes, then `buf:breaking`.
5. **Test first**: use case tests with in-memory adapters; contract tests for any new adapter (`test-engineer` subagent can draft them).
6. **Implement** inside out: domain -> use case -> adapter -> handler -> spec. Use the matching skill.
7. **Check**: `pnpm check` locally (lefthook runs the fast part on commit).
8. **Review**: `code-reviewer` subagent, plus `security-reviewer` for auth, permissions, sandbox, secrets or egress changes.
9. **Docs**: `docs-keeper` subagent updates affected docs in the same PR.
10. **PR**: small, conventional title, CI green, then merge.
