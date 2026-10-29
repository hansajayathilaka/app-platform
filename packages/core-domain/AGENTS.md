# core-domain

- Pure TypeScript. **No imports** from other packages or libraries (Zod included). dependency-cruiser enforces it.
- Entities protect their invariants; constructors/factories validate. No public setters.
- Value objects are immutable and compared by value.
- Domain events are plain data, raised by entities, dispatched by the application layer.
- No I/O, no `Date.now()`, no randomness: take values as arguments.
- Docs: `docs/architecture/code-architecture.md`.
