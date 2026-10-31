# Docs index

Read only what your task needs. Each line: file, what it covers, when to read it.

## Start here
| File | Covers | Read when |
| --- | --- | --- |
| `product/overview.md` | Main goal, objectives with measures, non-goals | New to the project, or judging any design choice |
| `product/glossary.md` | App, instance, port, spec, catalog, principal... | A term is unclear |
| `roadmap.md` | Phases, current phase, open questions | Planning or picking work |

## Architecture (`architecture/README.md` has the full list)
| File | Covers | Read when |
| --- | --- | --- |
| `architecture/system-overview.md` | Big picture: shell, apps, SDK, core, Postgres | Any cross-cutting change |
| `architecture/code-architecture.md` | Layers, ports, dependency rule, folder layout | Writing any code |
| `architecture/core/*` | Identity, auth, sessions, permissions and record rules, audit, reference data | Touching login, access control, audit, reference data |
| `architecture/modules/*` | Supporting modules (contacts, record timeline) and how to write one | Writing or using a module |
| `architecture/apps/*` | App model, manifest, instances, SDK, cross-app data | Building or changing an app or the SDK |
| `architecture/jobs/jobs-runtime.md` | Triggers, scheduler, retries, long jobs, secrets | Jobs, schedules, automations |
| `architecture/ui/*` | Shell, layouts, server-driven UI, page spec, catalog, i18n | Any UI work |
| `architecture/studio/*` | App Studio agent, sandbox, review pipeline | Studio or generated apps |
| `architecture/api/connect-rpc.md` | Contracts, streaming, webhooks | Adding or changing an API |
| `architecture/data/postgres.md` | Schemas, migrations, jsonb, roles | DB or migration work |

## Decisions
| File | Covers | Read when |
| --- | --- | --- |
| `decisions/README.md` | Index of ADRs and their status | Before changing any architectural choice |

## How we work
| File | Covers | Read when |
| --- | --- | --- |
| `guides/dev-environment.md` | Dev Container, services, toolchain | Setting up or a tool misbehaves |
| `guides/versioning.md` | Conventional Commits, SemVer rules, releases | Writing a commit/PR title, changing a contract, releasing |
| `guides/testing.md` | Test pyramid, contract tests, fixtures | Writing tests |
| `guides/writing-an-app.md` | From empty folder to installed app | Creating an app by hand |
| `guides/ports-and-adapters.md` | Adding a port or a new adapter | Swapping or adding infrastructure |
| `guides/deployment.md` | Images, Compose on the VPS, backups | Releasing or operating |
| `workflows/README.md` | Feature, architecture change, release, AI-agent workflow | Starting any task |
