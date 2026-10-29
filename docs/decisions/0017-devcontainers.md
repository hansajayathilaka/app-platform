# ADR 0017: Dev Containers as the development environment

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
The same toolchain must work on any laptop, in Codespaces and for cloud AI agents, without "works on my machine" drift.

## Decision
Development happens inside the repo's Dev Container (`.devcontainer/`): a Compose-based setup with a `workspace` container (Node LTS, pnpm, buf, mise, Docker CLI) plus `postgres` and `mailpit` services. Running on the bare host is unsupported. CI builds and uses the same image.

## Consequences
- One definition of the toolchain; new machines are ready after "Reopen in Container".
- Docker is required on the host. Testcontainers inside the container uses the host Docker socket (docker-outside-of-docker).
- Toolchain changes are made in `.devcontainer/` and `mise.toml` and reviewed like code.
