---
summary: The platform's main goal, its seven objectives with measures, what is not strict, and non-goals.
read_when: You are new to the project, or need the "why" behind a requirement or design choice.
---
# Product overview: app-platform (working name)

Owner: Hansa. Goal and objectives agreed 2026-10-31. Judge every design choice against them; where an older ADR conflicts, propose a new ADR.

## Main goal

A ready-made platform where a software engineer, working with an AI coding agent such as Claude Code or Codex, turns an idea into a running business app, automation or AI workflow in minutes. The platform already provides the UI, the backend plumbing, sign-in, permissions and isolated data, so the engineer and the agent write only the business logic.

## Objectives

1. **Minutes from idea to a running app.** A new app can be created, started and opened in the dev environment within a few minutes, with no setup beyond the app itself. The dev environment is a hosted environment where developers test apps, not a local machine.
   *Measure:* from an empty folder, `create app` plus one prompt to the agent gives a working page with sign-in and a table, deployed to the hosted dev environment, in under 5 minutes.
2. **Built for AI coding agents.** Claude Code is first-class; Codex and other agents are best effort. The agent can build a correct app on the first try thanks to clear conventions, a typed SDK, docs and skills it reads, and fast checks that catch its mistakes.
   *Measure:* given a one-paragraph description, Claude Code produces an app that passes all platform checks (types, lint, tests) without a human fixing the code.
3. **Ready-made UI.** Layouts, navigation, headers, theme and colours, tables, forms, buttons and dialogs come from the platform. Pages are React (TSX) files composed from platform components, not YAML or JSON specs (ADR 0028). Custom CSS is allowed but discouraged; platform components and the theme come first.
4. **Ready-made backend.** API calls, data access, validation, background and scheduled jobs and AI steps are provided, so an app does not wire its own server code. AI steps are first-class; their exact definition will be settled once the platform basics are in place.
5. **Apps are isolated.** Each app has its own database schema and its own DB user, so it can be changed, broken or removed without touching any other app's data. Apps share one runtime for now; the design leaves room to isolate runtimes later when needed.
   *Measure:* a broken migration or dropped schema in one app leaves every other app's data and the platform login working.
6. **Share an app with someone.** An admin adds a person (for example a friend trying an app) and gives them access to one app only, with permissions handled by the platform. For now users are added manually and sign in with a username and password; there is no public sign-up, invitation link or SSO.
   *Measure:* an admin creates a user and grants one app and a role; that person can sign in and use only that app, and revoking access takes effect immediately.
7. **Real code, never cornered.** It is a tool for engineers, not a no-code or vibe-coding tool. The app is real code the engineer owns. Bypassing a platform building block is strongly discouraged: look for a supported way first. The room to bypass still exists, behind loud warnings (lint errors that need an explicit, commented opt-out, flagged in review).
   *Measure:* every platform building block (UI component, data access, jobs, auth checks) can be bypassed inside an app without forking the platform, and every bypass is visible and warned.

## Not strict (decided per need, may change)

- Specific technologies and frameworks.
- Installing the same app more than once.
- Single-tenant deployments.

## Non-goals

- A no-code tool for non-developers.
- Multi-company inside one deployment.

## Related decisions

- Build our own platform rather than adopt an existing product (2026-10-31). Odoo, ERPNext, NocoBase and PocketBase are reference points only.

## Users

- **Admin**: adds users, grants app access and roles, manages apps.
- **Builder**: a software engineer who builds apps with an AI coding agent.
- **Member**: uses the apps they were granted.
