---
summary: Jobs and automations - triggers, scheduler, guarantees, long-running jobs, run history and secrets.
read_when: Adding a job, schedule, event trigger or webhook, or changing the queue.
related: [../apps/sdk.md, ../../decisions/0012-jobs-graphile-no-temporal.md]
---
# Jobs and automation runtime

Graphile Worker (Postgres-backed) behind the `JobQueue` port. Enqueue happens in the same transaction as the business write. Workers run in the server process on a small box and can be split out later.

## Triggers
- **Cron**: manifest defaults or user-created `schedules(id, app_instance_id, job, cron, timezone, payload, enabled, created_by)`. Timezone-aware; missed-run policy (skip or catch up once).
- **Event**: any exported event.
- **Webhook**: inbound URL per trigger with a signing secret (plain HTTP, `../api/connect-rpc.md`).
- **Manual / API**: "Run now", or `POST /api/apps/:instance/jobs/:job/run` with a token.

## Scheduler
One loop, kept single by a Postgres advisory lock, turns due schedules into queue rows keyed `schedule_id + fire_time`, so a double tick cannot run twice.

## Guarantees
- At-least-once; handlers get an idempotency key.
- Exponential backoff with jitter, max attempts, then dead-letter (inspect, replay).
- Timeout, heartbeat, cancellation, concurrency keys (e.g. one run per instance per job).
- Runs as the **app instance** principal, `on_behalf_of` the schedule creator. Audit shows both.

## Long-running jobs (over 15 minutes)
No Temporal (RAM cost, ADR 0012). Long jobs are **checkpointed workflows**: each step's result is persisted; after a crash or deploy the job resumes from the last finished step. Steps must be idempotent. Waiting on humans = a step that parks the run until a signal arrives.

## Run history
`job_runs(id, app_instance_id, job, trigger, status, attempt, started_at, finished_at, duration_ms, input, output, error, log_ref)`. Runs page: filters, streaming logs, rerun, cancel. Alerts on repeated failure.

## Secrets
Per instance, envelope-encrypted (libsodium master key on the VPS). Only via `ctx.secrets`; never returned to the UI after saving; every read audited.
