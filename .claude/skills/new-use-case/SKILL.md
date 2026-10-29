---
name: new-use-case
description: Add a command or query use case in the core or an app, test-first, with authorize, unit of work and audit. Use for any new business operation.
---
# New use case

Read first: `docs/architecture/code-architecture.md`.

1. Name it as an intent: `InviteUser`, `ListOverdueInvoices`.
2. Write the test first in `application/<Name>.test.ts` using in-memory adapters from `src/testing/`.
3. Implement `application/<Name>.ts`:
   - constructor takes ports only;
   - `execute(input)`: validate input -> `authorize(permission)` -> inside `unitOfWork.run(...)`: load, call domain, save, `audit(...)`, publish events / enqueue jobs.
   - queries skip the unit of work and audit unless they read sensitive data.
4. Declare any new permission in the manifest.
5. Expose it through a handler (skill `add-rpc-method`) if callers need it.
6. Wire it in the composition root.
7. `pnpm test` then `pnpm check`.
