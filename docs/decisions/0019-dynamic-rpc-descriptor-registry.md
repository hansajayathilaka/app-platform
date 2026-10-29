# ADR 0019: Dynamic RPC through a descriptor registry

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
Studio apps are published at runtime. There is no `buf generate` step and no generated client in the static shell bundle, yet page specs call app methods by name. Hand-written apps should use the same browser path so there is one way to call an app.

## Decision
- Every app version stores its compiled `FileDescriptorSet` (from `buf build`; the studio publish pipeline runs the same step) beside the version.
- Server: a `DescriptorRegistry` port maps (app, version) to descriptors. One dynamic Connect dispatcher routes `/<package>.<Service>/<Method>`, resolves the instance from the `Platform-Instance` header, decodes with the instance's app-version descriptors (protobuf-es dynamic messages), runs the handler in-process or in the sandbox (`AppRuntime`), and encodes the response. New versions swap in without a restart.
- Browser: one generic `invoke(instance, method, input)` built on connect-es clients created from runtime descriptors. Descriptors are fetched with the page and cached by app-version ETag. JSON encoding.
- Page specs keep short method names (`ListCustomers`); `spec:check` resolves them to fully qualified names in the compiled JSON.
- Platform services (auth, pages, admin) keep statically generated clients in the shell. Server-side app code and tests keep generated TypeScript types.

## Consequences
- Studio apps go live with no shell rebuild, and hand-written apps take the same path.
- A version mismatch between a cached page and the server returns `failed_precondition`; the shell reloads the page.
- Field permissions (ADR 0018) and protovalidate run in the dispatcher from descriptor options, so they apply to every app.
- `buf` ships in the server image for the studio pipeline.
