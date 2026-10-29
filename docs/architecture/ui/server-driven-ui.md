---
summary: How pages reach the browser - GetPage RPC, permission filtering, caching, renderer port.
read_when: Changing how pages are served or rendered, or adding a mobile client.
related: [page-spec.md, ui-catalog.md, ../../decisions/0010-server-driven-ui-yaml.md, ../../decisions/0011-json-render.md]
---
# Server-driven UI

The shell is static; every app page comes from the server. This is what lets studio apps go live with no frontend rebuild.

- `PageService.GetPage(instance, route)` returns the compiled page spec (JSON) for the instance's app version, **filtered by the caller's permissions**: actions the user cannot run, and fields they may not read, are removed server-side.
- The response also carries the app version's RPC descriptors and the i18n messages the spec uses for the user's locale (`i18n.md`).
- Specs are cached by the browser with an ETag on app version and locale. Data comes from separate query RPCs through the generic `invoke` (`../api/connect-rpc.md`).
- The spec format is **ours** (versioned JSON Schema in `core-application`). Rendering sits behind the `PageRenderer` port.
- Renderer adapter: **json-render** (React) against the `@platform/ui` catalog. Pinned exact versions.
- Mobile (possible, unconfirmed): React Native via json-render's RN renderer and a mobile registry; or, for fully native, the server compiles our spec to A2UI messages.
- Escape hatch: a page needing something the catalog cannot express gets a new reviewed catalog component, never page-specific React.
