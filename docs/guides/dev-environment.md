---
summary: The Dev Container setup (the only supported dev environment), its services, toolchain and everyday commands.
read_when: Setting up, changing the toolchain, or when a tool or command misbehaves.
related: [../decisions/0017-devcontainers.md]
---
# Development environment (Dev Container)

All development happens inside the repo's Dev Container (ADR 0017). Open the repo in VS Code and choose "Reopen in Container", or open it in Codespaces. Cloud AI agents use the same image.

## Layout
```text
.devcontainer/
  devcontainer.json   # features, extensions, post-create (pnpm install, git hooks)
  compose.yaml        # workspace + postgres + mailpit (+ optional otel-lgtm profile)
  Dockerfile          # Node LTS, pnpm (corepack), buf, mise, Docker CLI
```
- `workspace`: where code runs. Ports forwarded: 3000 (server), 5173 (web), 6006 (Storybook), 8025 (Mailpit UI).
- `postgres`: PostgreSQL with a persistent volume; `DATABASE_URL` preset.
- `mailpit`: catches all outgoing email.
- Testcontainers uses the host Docker through docker-outside-of-docker.

Inside the container, `pnpm dev` runs the server (`tsx watch`) and shell (Vite) with hot reload.

## Toolchain
| Area | Tool |
| --- | --- |
| Versions | `mise.toml` (Node LTS, pnpm, buf), baked into the image |
| Monorepo | pnpm workspaces + Turborepo |
| TypeScript | strict, `noUncheckedIndexedAccess`, project references |
| Lint/format | Biome |
| Architecture rules | dependency-cruiser |
| Contracts | buf (`lint`, `breaking`, `generate`, `curl`) |
| DB | Drizzle Kit, instance migration runner, Drizzle Studio, seed scripts |
| Specs | Red Hat YAML extension, `pnpm spec:check` |
| Tests | Vitest, Testcontainers, Playwright (browsers in the image) |
| UI | Storybook |
| Commits | lefthook + commitlint (Conventional Commits, `guides/versioning.md`) |
| Releases | release-please |
| Deps | Renovate (exact pins for young libraries) |

## Rules
- Do not install tools on the host for this project; change `.devcontainer/` or `mise.toml` instead, in a `build(devcontainer): …` commit.
- CI builds the same image, so "passes in the container" means "passes in CI".
