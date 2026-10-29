# AI agent workflow

How Claude Code (and other agents) should work in this repo while keeping context small.

## Context loading (progressive disclosure)
1. `AGENTS.md` (root) loads automatically: rules, commands, map. Under 150 lines.
2. Nested `AGENTS.md` files load when working in that folder: local rules only.
3. `docs/README.md` is the index; read only the docs whose "read when" matches. Every doc starts with `summary` and `read_when` front matter, so reading the first 5 lines is enough to decide.
4. Skills (`.claude/skills/*/SKILL.md`) load only when the task matches their description.
5. Subagents (`.claude/agents/*.md`) run in their own context; give them a focused task and take back a summary, so the main session stays small.

## Which tool for which job
| Job | Use |
| --- | --- |
| Plan a feature, check against ADRs | `architect` subagent |
| Repeatable procedure (new app, use case, adapter, RPC method, page spec, migration, component, ADR) | Matching skill |
| Commit message, PR title, version impact | `commit-and-version` skill |
| Draft tests and contract suites | `test-engineer` subagent |
| Review a diff | `code-reviewer` subagent |
| Auth, permissions, sandbox, secrets, egress changes | `security-reviewer` subagent |
| Keep docs in sync after a change | `docs-keeper` subagent |

## Rules
- Prefer reading a doc over guessing; prefer a skill over improvising.
- Keep docs small: one topic per file, under about 150 lines. Split rather than grow.
- When code and docs disagree, fix whichever is wrong in the same PR.
