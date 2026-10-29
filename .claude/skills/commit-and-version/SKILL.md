---
name: commit-and-version
description: Write Conventional Commit messages and PR titles and decide the SemVer impact of a change. Use before every commit or PR, and whenever a public contract changes.
---
# Commit and version

Read: `docs/guides/versioning.md`.

1. Pick the type from what the change does for users: `feat`, `fix`, `perf`, or a non-release type (`refactor`, `test`, `docs`, `build`, `ci`, `chore`).
2. Pick the scope from the package or area (`sdk`, `jobs`, `ui`, `app:<id>`, ...).
3. Breaking? Check: SDK `ctx`, manifest or page-spec schema, `.proto` (`pnpm buf:breaking`), exported read models/commands/events, migrations needing manual steps. If yes, add `!` and a `BREAKING CHANGE:` footer with migration steps.
4. Summary: imperative, lower case, no trailing period, under 72 characters.
5. Never edit versions or changelogs by hand; release-please does it.
6. The PR title uses the same format (it becomes the squash commit).
