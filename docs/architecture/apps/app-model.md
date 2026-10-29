---
summary: What an app is, build-time selection, folder structure and lifecycle.
read_when: Creating an app, or changing how apps are loaded or installed.
related: [manifest.md, instances.md, sdk.md, ../../guides/writing-an-app.md]
---
# App model

An app is a package `apps/<id>` with a manifest, use cases, page specs, `.proto` contracts and migrations, following the same layering as the core. Studio-generated apps have the same shape but are stored as versioned bundles and run in the sandbox.

## Build-time selection
- The deployment config lists enabled apps. Only those are bundled and migrated; disabled apps leave no code or tables (ADR 0014).
- A fresh system has **no apps installed**. An admin installs instances as needed.

## Lifecycle (per instance)
1. **Install**: admin names it, reviews requested permissions and imports (consent screen), binds imports to source instances, fills settings. Platform creates the schema and runs migrations.
2. **Enable / disable**: disabling stops jobs and hides UI, keeps data.
3. **Upgrade**: new app version migrates each instance; a failure marks only that instance degraded.
4. **Uninstall**: schema renamed `archived_…` for the retention window, then dropped.
Every step is audited.

## Isolation
- Data: schema per instance plus a DB role per instance (`instances.md`).
- Code: an app imports only other apps' published contracts; dependency-cruiser enforces it.
- Runtime: per-instance concurrency limits and timeouts on jobs.
