---
summary: API contracts with Connect RPC - proto conventions, dynamic dispatch for app methods, streaming limits, webhooks.
read_when: Adding or changing any API method, or calling the API from a client.
related: [../../decisions/0009-connect-rpc.md, ../../decisions/0019-dynamic-rpc-descriptor-registry.md, ../../guides/ports-and-adapters.md]
---
# API: Connect RPC

- Contracts in `proto/` (platform) and `apps/<app>/proto/`, managed with buf. One Fastify server speaks gRPC, gRPC-Web and Connect (JSON over HTTP).
- Workflow: edit `.proto` -> `pnpm buf:lint` -> `pnpm buf:breaking` -> `pnpm buf:generate` -> implement the handler in `packages/interfaces/rpc` calling a use case.
- Handlers are thin: map DTO -> use case input, call, map result. No business logic.
- Validation: protovalidate annotations on messages; domain invariants stay in the domain.
- Mark read-only methods `idempotency_level = NO_SIDE_EFFECTS` so Connect can serve them as cacheable GET.
- Streaming: browsers get unary and **server streaming** only (job logs, run status). Client and bidi streaming are server-to-server. Two-way browser features will use a WebSocket adapter later.
- Protobuf rules: never reuse or renumber fields; breaking changes go in a new package version (`v2`).
- Use JSON encoding in development for readable network logs.
- Inbound webhooks are plain HTTP endpoints (`packages/interfaces/webhooks`) with signature checks.

## Dynamic dispatch for app methods (ADR 0019)
App methods (hand-written and studio) are not compiled into the shell. They go through one dynamic path.

- Each app version stores its `FileDescriptorSet` (`buf build`). `DescriptorRegistry` port: (app, version) -> descriptors.
- Server: one dispatcher for `/<package>.<Service>/<Method>`. It reads the `Platform-Instance` header, loads that instance's app-version descriptors, decodes the request (protobuf-es dynamic messages), applies protovalidate and field permissions from descriptor options, calls the handler in-process or in the sandbox (`AppRuntime`), encodes the reply. New versions swap in without a restart.
- Browser: `invoke(instance, method, input)` creates a connect-es client from runtime descriptors, JSON encoding. Descriptors come with the page and are cached by app-version ETag.
- A page cached for an older app version gets `failed_precondition`; the shell reloads it.
- Platform services (identity, pages, admin) use statically generated clients.
