# ADR 0026: Platform building-block services

- **Status:** Proposed
- **Date:** 2026-10-30
- **Deciders:** Hansa

## Context
Most apps need the same infrastructure: files, notifications, email, connections to external services, outbound webhooks and search. If the platform does not provide them, each app builds its own and the copies diverge.

## Decision
Each service is a port with one adapter, exposed to apps through `ctx` and audited:

| Service | `ctx` | First adapter | Notes |
| --- | --- | --- | --- |
| Files | `ctx.files` | Local disk volume; S3-compatible later | Upload via signed URL, size and type limits, thumbnails as a job |
| Notifications | `ctx.notify(user, key, args, link)` | Postgres + shell bell | Per-user preferences: in-app, email digest |
| Email | `ctx.mail.send(template, to, args)` | SMTP (Mailpit in dev) | Templates per app with i18n, queued and retried |
| Connections | `ctx.connections.get(name)` | OAuth 2 client + encrypted token store | Per instance; consent flow in instance settings; auto refresh; Google, Microsoft, Slack, GitHub first; API-key connections too |
| Outbound webhooks | `ctx.webhooks.emit(event)` | Job queue | HMAC-signed, retried, delivery log |
| Search | `ctx.search.index(ref, text)` + global search bar | Postgres full-text (`tsvector`) per instance | Results filtered by record rules |
| Documents (later) | `ctx.documents.render(template, data)` | Typst or a pdfkit-style renderer, as a job | No headless Chromium on 4 GB |

Each service is built when the first real app needs it, never inside an app.

## Consequences
- Apps stop reimplementing infrastructure; retries, audit and permissions behave the same everywhere.
- More platform surface to maintain and version; each service gets a port contract suite.
