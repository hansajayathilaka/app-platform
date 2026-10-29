---
summary: How to add a port, add an adapter, or swap infrastructure.
read_when: Introducing new infrastructure or replacing an existing one.
related: [../architecture/code-architecture.md]
---
# Ports and adapters

## Add a port
1. Define the interface in `packages/core-application/src/ports/<Name>.ts`, in domain terms (no library types).
2. Write the contract suite `test/contracts/<Name>.contract.ts`.
3. Write an in-memory adapter that passes it.

## Add or swap an adapter
1. Create `packages/adapters/<tech>-<purpose>/` implementing the port.
2. Run the port's contract suite against it.
3. Wire it in `services/server/src/composition-root.ts` (config-selected if both stay).
4. Nothing outside the adapter package and the composition root should change. If it must, the port leaks: fix the port.
