---
summary: Step-by-step - from empty folder to an installed app instance.
read_when: Creating a hand-written app. (Agents: use skill new-app.)
related: [../architecture/apps/app-model.md, ../architecture/apps/manifest.md]
---
# Writing an app

1. `pnpm gen:app <id>` scaffolds `apps/<id>/{domain,application,adapters,specs,proto}` and `app.yaml`.
2. Model the domain: entities and value objects in `domain/`, with unit tests.
3. Write use cases in `application/`, each with a test using in-memory adapters. Call `ctx.authorize` and `ctx.audit` inside.
4. Declare permissions, settings, imports, exports, jobs in `app.yaml`.
5. Define the service in `proto/<id>/v1/service.proto`; `pnpm buf:generate`; implement thin handlers.
6. Write migrations for the app's tables (they run per instance).
7. Write page specs in `specs/pages/*.yaml`; `pnpm spec:check`.
8. Enable the app in the deployment config; `pnpm dev`; install an instance from the admin UI.
9. `pnpm check`, then open a PR.
