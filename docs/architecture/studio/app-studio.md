---
summary: The App Studio - agent, what it generates, the prompt-to-install pipeline and human review.
read_when: Working on the studio, the agent's tools, or generated apps.
related: [sandbox.md, ../ui/page-spec.md, ../../decisions/0013-studio-apps.md]
---
# App Studio

Generated apps use the same manifest, SDK, page specs and install flow as hand-written apps. The studio must be able to generate **everything a hand-written app has**.

## What the agent produces
| Part | Form |
| --- | --- |
| Data model | Entity definitions; the platform generates migrations (agent never writes DDL) |
| UI | YAML page specs from the catalog |
| Automations | Workflow graph: trigger, steps (SDK actions, commands, HTTP, conditions, loops) |
| Custom logic | TypeScript code steps, run only in the sandbox |
| Custom UI | New components, run in a sandboxed iframe on a separate origin |
| Manifest | Permissions, imports, events, schedules, egress hosts, secrets |

## Agent
- Server-side, acting as a studio principal **for the requesting user**: can only request what that user could grant; never publishes or installs.
- Tools: read contracts, catalog, SDK docs; write and validate specs; run tests in the sandbox with sample data; open a preview.
- Claude via the Claude API with tool use; every prompt, tool call and output stored with the draft version and audited.
- Previews run use cases against **mock adapters** (in-memory repos, stubbed HTTP, fake clock).

## Pipeline
Prompt -> agent drafts -> checks (schema, types, policy; fail -> agent fixes) -> sandbox tests -> **human approves** a diff of permissions, data access, egress and code (changes -> agent) -> publish immutable version -> install as an instance.

Tables: `studio_projects`, `app_versions(app_id, version, spec, code_bundle_hash, status)`, `app_reviews`. Quotas on generations and sandbox CPU.
