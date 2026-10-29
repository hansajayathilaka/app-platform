# server (composition root)

- The only place that constructs adapters and injects them into use cases (`src/composition-root.ts`).
- Starts interfaces (Connect server, webhooks), scheduler and workers in one process; flags allow splitting later.
- Config via environment, validated at boot. No business logic here.
