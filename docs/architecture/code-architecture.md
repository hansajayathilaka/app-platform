---
summary: Hexagonal / clean architecture - layers, ports, adapters, the dependency rule, folder layout and practices.
read_when: Before writing or moving any code.
related: [../guides/ports-and-adapters.md, ../decisions/0007-hexagonal-architecture.md]
---
# Code architecture

Domain and use cases sit in the middle with no framework or infrastructure imports. Every database, auth provider, queue or transport plugs in from outside through an interface (port). No framework owns the code.

## Layers

| Layer | Contains | May depend on |
| --- | --- | --- |
| Domain | Entities, value objects, domain services, domain events, invariants | Nothing |
| Application | Use cases (commands, queries) and **ports**: `UserRepository`, `SessionStore`, `AuthProvider`, `JobQueue`, `UnitOfWork`, `AuditWriter`, `Clock`, `IdGenerator`, `PageRenderer`... | Domain |
| Adapters (driven) | Port implementations: Postgres (Drizzle), Better Auth, Graphile Worker, isolate sandbox, SMTP, LLM | Application, Domain, their library |
| Interfaces (driving) | Connect RPC handlers, webhook endpoints, CLI; DTO mapping and validation | Application |
| UI | Static shell and `@platform/ui`, using generated Connect clients | Contracts only |
| Composition root | `services/server`: builds adapters, injects them into use cases | Everything |

## Folder layout

```text
proto/                    # .proto contracts (buf)
packages/
  core-domain/
  core-application/       # use cases + ports
  adapters/
    persistence-postgres/ # repositories, UnitOfWork, InstanceStorageProvisioner, ReadModelGateway
    auth-better-auth/     # AuthProvider
    queue-graphile/       # JobQueue, Scheduler
    sandbox-isolates/     # AppRuntime for studio apps
    mail-smtp/  llm-claude/
  interfaces/
    rpc/                  # Connect handlers -> use cases
    webhooks/  cli/
  sdk/                    # ctx contract + in-process and RPC implementations
  ui/                     # @platform/ui components, layouts, catalog
modules/<id>/{contract,domain,application,adapters,specs,proto,ui} # supporting modules
apps/<app>/{domain,application,adapters,specs,proto}
services/
  server/                 # composition root
  web/                    # static shell
```

## Practices
- Constructor injection; a hand-written composition root. No decorators, no reflection containers.
- Repository + Unit of Work; domain events for side effects; CQRS-lite where reads differ from writes.
- **Contract tests per port**: one shared suite every adapter of that port must pass.
- Ports only where a swap is plausible: database, auth, queue, file storage, email, LLM, sandbox, page renderer.
- `dependency-cruiser` fails CI on any layer, app-boundary or module-boundary violation (core never imports modules; apps and modules import only a module's `contract`).

## Swap cost
- Auth provider: one adapter; our sessions and RBAC stay.
- Interface (REST, gRPC, CLI, MCP): a new driving adapter.
- Database: repositories swap easily; Postgres-specific features (schema per instance, outbox, queue, cross-app views) sit behind `InstanceStorageProvisioner`, `UnitOfWork`, `JobQueue`, `ReadModelGateway`, so a different DB means rewriting that adapter set, not the domain.
