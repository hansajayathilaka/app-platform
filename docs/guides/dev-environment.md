---
summary: Toolchain, local services, Dev Container and everyday commands.
read_when: Setting up, or when a tool or command misbehaves.
---
# Development environment

`pnpm dev` starts Postgres and Mailpit in Docker, then the server (`tsx watch`) and shell (Vite) on the host with hot reload. The same setup ships as a Dev Container for VS Code, Codespaces and cloud agents.

| Area | Tool |
| --- | --- |
| Versions | mise (`mise.toml`: Node LTS, pnpm, buf) |
| Monorepo | pnpm workspaces + Turborepo |
| Local services | Docker Compose: Postgres, Mailpit, optional Grafana LGTM |
| TypeScript | strict, `noUncheckedIndexedAccess`, project references |
| Lint/format | Biome |
| Architecture rules | dependency-cruiser |
| Contracts | buf (`lint`, `breaking`, `generate`, `curl`) |
| DB | Drizzle Kit, instance migration runner, Drizzle Studio, seed scripts |
| Specs | Red Hat YAML extension, `pnpm spec:check` |
| Tests | Vitest, Testcontainers, Playwright |
| UI | Storybook |
| Hooks | lefthook + commitlint |
| Deps | Renovate (exact pins for young libraries) |

Recommended VS Code extensions are in `.vscode/extensions.json`.
