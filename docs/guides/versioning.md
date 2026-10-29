---
summary: Commit message format, what gets a version, how SemVer bumps are decided, and how releases are cut.
read_when: Writing a commit or PR title, changing a public contract, or releasing.
related: [../decisions/0016-conventional-commits-semver.md, ../workflows/release.md]
---
# Commits and versioning

## Commit messages (Conventional Commits)

```text
<type>(<scope>)!: <summary in imperative, lower case, no period>

<body: why, not what>

BREAKING CHANGE: <what breaks and how to migrate>
```

| Type | Use for | Release effect |
| --- | --- | --- |
| `feat` | New capability | minor |
| `fix` | Bug fix | patch |
| `perf` | Performance improvement | patch |
| `refactor`, `test`, `docs`, `style`, `build`, `ci`, `chore` | Everything else | none |
| any type with `!` or a `BREAKING CHANGE:` footer | Incompatible change | major |

Scopes: `core`, `auth`, `rbac`, `audit`, `sdk`, `jobs`, `api`, `db`, `ui`, `shell`, `studio`, `sandbox`, `devcontainer`, `deps`, `docs`, or `app:<id>`.

Examples: `feat(jobs): add checkpointed long-running steps`, `fix(rbac): deny instance access when role is revoked`, `feat(sdk)!: rename ctx.call to ctx.command`.

PRs are squash-merged; the **PR title** must follow the same format.

## What is versioned (SemVer 2.0)

| Thing | Version lives in | Major bump when |
| --- | --- | --- |
| Platform release (server, shell, SDK) | git tag `vX.Y.Z`, `CHANGELOG.md` | SDK `ctx` contract, manifest or page-spec schema, platform proto, or upgrade needing manual steps breaks |
| Each app | `apps/<id>/app.yaml` `version` and tag `app-<id>-vX.Y.Z` | Its exported read models, commands or events break |
| Studio-generated apps | `app_versions.version` | Same rule, computed from the contract diff at publish |
| Proto packages | package suffix `v1`, `v2` | Any breaking change (`buf breaking`) |
| Page spec and manifest schemas | `version:` field (integer = major) | Any field removed or changed in meaning |

Minor = backward-compatible additions. Patch = fixes only. Pre-releases: `X.Y.Z-rc.N`. Until `1.0.0`, minor releases may still contain breaking changes, which are still marked `!`.

## Releasing
release-please (GitHub Action) reads commits on `main`, keeps a release PR with the next versions and changelogs, and tags on merge. Merging the release PR triggers the deploy (`../workflows/release.md`).
