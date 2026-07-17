# Batch Processing Checklist

> Practical, no-fluff checklist for production batch jobs.
> Scheduled tasks, ETL pipelines, bulk operations, event-driven workers — the principles are the same. The orchestrator changes.
> Last updated: 2026-07-17

---

## 1. Job Design & Architecture

- [ ] **Job type clarity** — Scheduled cron (time-triggered), event-driven (message/queue-triggered), or on-demand (manual/API-triggered). Each has different failure characteristics.
- [ ] **Single responsibility** — One job, one purpose. "Sync users and send emails and generate reports" is three jobs. Compose them in an orchestrator if they depend on each other.
- [ ] **Idempotent execution** — Running the same job twice with the same input produces the same result. No duplicate records, no double-sends. Use idempotency keys, upserts, or deduplication checks.
- [ ] **Resumable from failure** — If the job crashes at record 5,000 of 50,000, it can resume from 5,000 — not restart from 0. Checkpointing, cursor-based progress, or chunked processing with committed offsets.
- [ ] **Chunk/partition strategy** — Process data in bounded chunks (100–10,000 records per batch). Never `SELECT * FROM million_row_table` into memory. Stream, paginate, or partition by key range.
- [ ] **Stateless workers** — Job state lives in the database or queue, not in the worker process. Workers can be killed and replaced without losing progress.
- [ ] **Concurrency control** — Can multiple instances of this job run simultaneously? If not: distributed lock (Redis SETNX, DB advisory lock, leader election). If yes: partition work so instances don't overlap.
- [ ] **Dependency ordering** — If Job B depends on Job A's output, make it explicit. DAG orchestration (Airflow, Step Functions, Temporal) or event-driven (Job A emits completion event → triggers Job B).

## 2. Scheduling & Orchestration

- [ ] **Scheduler choice** — Simple cron (K8s CronJob, systemd timer) for single jobs. Orchestrator (Airflow, Prefect, Dagster, Temporal, Step Functions, Bull/BullMQ) for workflows with dependencies, retries, and monitoring.
- [ ] **Cron expression correctness** — Timezone-aware. DST handling (2 AM runs twice or never during DST transitions). Test with `crontab.guru` or equivalent. Document the schedule in the job itself.
- [ ] **No overlapping runs** — If a job takes longer than its interval, the next run must wait or skip — not stack. `concurrencyPolicy: Forbid` (K8s), `misfire_grace_time` (Airflow), or explicit lock.
- [ ] **Job timeout** — Every job has a maximum runtime. Kill it if exceeded. Alert on timeout. A stuck job is worse than a failed one — it holds resources indefinitely.
- [ ] **Manual trigger capability** — Every scheduled job can also be triggered manually (for debugging, backfills, or re-runs). Same code path, same parameters, same logging.
- [ ] **Backfill support** — Can you run this job for a past date range? Historical re-processing without affecting current data. Parameterize by date/range, don't hardcode "today."
- [ ] **Calendar/business-day awareness** — If applicable: skip weekends, holidays, or non-business hours. Configurable calendar, not hardcoded dates.

## 3. Data Processing

- [ ] **Input validation** — Validate source data before processing. Schema checks, null checks, type coercion. Reject or quarantine invalid records — don't let garbage propagate.
- [ ] **Partial failure handling** — 1 bad record out of 10,000 should not kill the entire batch. Skip-and-log, quarantine to error table/DLQ, or collect errors and report at end. Configurable: fail-fast vs skip-on-error.
- [ ] **Transaction boundaries** — Commit per chunk, not per record (too slow) and not per entire job (too risky — OOM, lock timeout). Chunk size = transaction size. Rollback one chunk doesn't lose everything.
- [ ] **Cursor/offset tracking** — Track "where I left off" in durable storage. Database column, Redis key, or checkpoint file. Updated after each successful chunk commit.
- [ ] **Data deduplication** — Source data may have duplicates. Process may run twice. Use unique constraints, UPSERT/ON CONFLICT, or dedup table with processed IDs.
- [ ] **Ordering guarantees** — Does processing order matter? If yes: sort explicitly, process sequentially within partitions. If no: parallelize freely.
- [ ] **Memory management** — Stream large datasets, don't load all into memory. Use cursors (DB), streaming parsers (CSV/JSON), or paginated API calls. Monitor RSS/heap during runs.
- [ ] **Rate limiting & backpressure** — If calling external APIs in batch: respect rate limits. Token bucket, fixed delay between calls, or adaptive throttling on 429s. Don't hammer a downstream service with 100K requests/second.
- [ ] **File processing specifics** — Large files: stream line-by-line or chunk-by-chunk. CSV: handle encoding (UTF-8 BOM), quoted fields, newlines in values. JSON: streaming parser (`jq`, `ijson`, `JsonParser`). XML: SAX over DOM for large files.

## 4. Error Handling & Recovery

- [ ] **Retry strategy** — Transient errors (network timeout, deadlock, rate limit): retry with exponential backoff + jitter. Permanent errors (validation, business rule): no retry, route to error handling. Max retries per record AND per job.
- [ ] **Dead letter / error table** — Failed records go somewhere inspectable. Include: original payload, error message, timestamp, attempt count, job run ID. Support manual review and replay.
- [ ] **Poison message detection** — A single bad record that causes repeated failures should be identified and quarantined, not retried forever. After N failures for the same record: move to DLQ, continue processing.
- [ ] **Compensation / rollback** — If a partially-completed job must be undone: compensation logic (reverse transactions, soft-delete created records). Document what "undo" means for each job.
- [ ] **Alerting on failure** — Job failed → immediate alert (PagerDuty, Slack, email). Job succeeded with errors → warning alert with error count. Job didn't run at all (missed schedule) → alert. Silent failures are the worst.
- [ ] **Error classification** — Distinguish: infrastructure errors (DB down, network), data errors (bad input), logic errors (bug). Each has different response: retry, quarantine, or fix-and-redeploy.
- [ ] **Graceful shutdown** — SIGTERM → finish current chunk → commit progress → exit. Don't abandon a half-processed chunk. K8s `terminationGracePeriodSeconds` must exceed chunk processing time.

## 5. Observability & Monitoring

- [ ] **Structured logging** — Every log line: job name, run ID, chunk number, records processed, timestamp. JSON format. Correlate all logs from one job run with a single ID.
- [ ] **Progress tracking** — For long jobs: log/emit progress (e.g., "processed 5,000/50,000 records — 10%"). Queryable: dashboard or API shows current progress of running jobs.
- [ ] **Job metrics** — Duration, records processed, records failed, records skipped, throughput (records/sec). Per-run and trended over time. Prometheus/Datadog/CloudWatch.
- [ ] **SLO for batch jobs** — "Daily sync completes within 2 hours." "Error rate < 0.1% of records." "No missed runs in 30 days." Measure and alert on SLO breach.
- [ ] **Stale job detection** — Job should have finished by now but hasn't. Heartbeat pattern: worker updates "last alive" timestamp every N seconds. Monitor detects staleness → alert → kill.
- [ ] **Run history** — Every job run recorded: start time, end time, status (success/failure/partial), records in/out/error, error summary. Queryable dashboard. Retention policy.
- [ ] **Distributed tracing** — If batch job calls other services: propagate trace context. Correlate batch processing with downstream API calls.
- [ ] **Resource monitoring** — Memory usage, CPU, disk I/O, DB connection count during job runs. Batch jobs often spike resources — know when you're approaching limits.

## 6. Data Integrity & Consistency

- [ ] **Exactly-once vs at-least-once** — Know which guarantee you have. Most systems are at-least-once. If exactly-once matters (financial): idempotency + dedup + transactional outbox.
- [ ] **Source-destination reconciliation** — After processing: count records in source, count records in destination. They should match (or the difference is explained by filtered/errored records). Automated reconciliation report.
- [ ] **Audit trail** — Who triggered the job, when, with what parameters, what changed. Immutable audit log. Required for compliance (SOX, GDPR, HIPAA) and debugging.
- [ ] **Data validation post-processing** — After the job completes: run validation queries. Row counts match? Aggregates reasonable? No orphaned records? No nulls where unexpected?
- [ ] **Temporal consistency** — If reading from a changing source: snapshot or read with consistent timestamp. Don't process half before a source update and half after.
- [ ] **Referential integrity** — Batch inserts/updates must respect foreign keys. Process parents before children. Delete children before parents. Or use deferred constraints within a transaction.

## 7. Performance & Scalability

- [ ] **Parallelization strategy** — Partition data by key (tenant, date range, ID range) → distribute partitions across workers. Map-reduce pattern. Embarrassingly parallel where possible.
- [ ] **Connection pooling** — Batch jobs can exhaust DB connections. Pool size appropriate for concurrency. Release connections between chunks, not held for entire job duration.
- [ ] **Bulk operations** — Batch INSERT (multi-row), bulk UPDATE, COPY/LOAD for large imports. Not row-by-row. 10x–100x performance difference.
- [ ] **Index management for bulk loads** — For massive inserts: consider disabling indexes → bulk load → rebuild indexes. Or use staging tables → swap. Depends on volume and acceptable downtime.
- [ ] **Resource scheduling** — Run heavy batch jobs during off-peak hours. Don't compete with production traffic for DB connections, CPU, or I/O. If cloud: use spot instances / preemptible VMs for cost savings.
- [ ] **Horizontal scaling** — Can you add more workers to process faster? Partitioned work supports this. Shared cursor/single-threaded processing does not. Design for scale-out.
- [ ] **Backpressure to downstream** — If writing to another service/queue: monitor its capacity. Don't overwhelm downstream with burst writes. Throttle output rate if needed.

## 8. Security

- [ ] **Least privilege** — Job runs with minimum required permissions. Read-only access to source if only reading. Write access only to specific target tables/buckets. No admin credentials for batch jobs.
- [ ] **Secrets management** — DB credentials, API keys, encryption keys: from vault/secrets manager, not environment variables in plaintext config. Rotatable without redeployment.
- [ ] **Data encryption** — Sensitive data encrypted at rest (storage) and in transit (TLS for DB/API connections). If processing PII: ensure intermediate files/temp storage are also encrypted.
- [ ] **PII handling** — If processing personal data: data minimization (only fetch what's needed), retention policies, right-to-erasure compliance, audit logging of access. Mask PII in logs.
- [ ] **Network isolation** — Batch workers in private subnet. Access to data sources via private endpoints (VPC endpoints, private link). No public internet exposure unless calling external APIs.
- [ ] **Temporary file cleanup** — If job creates temp files (CSV exports, staging files): delete after processing. Don't leave sensitive data on disk. Use `tmpfs` or encrypted ephemeral storage.

## 9. Testing

- [ ] **Unit tests** — Business logic, transformation functions, validation rules. Fast, no I/O. Test edge cases: empty input, single record, max chunk size, malformed data.
- [ ] **Integration tests** — Full job run against test database with realistic data. Testcontainers for DB/queue. Verify: correct records created, errors handled, progress tracked.
- [ ] **Idempotency tests** — Run the same job twice with the same input. Second run should produce no changes (no duplicates, no side effects). This is the most important batch test.
- [ ] **Failure & recovery tests** — Simulate crash mid-batch (kill process after N records). Restart. Verify: resumes from checkpoint, no duplicates, no gaps.
- [ ] **Large dataset tests** — Test with production-scale data volume (or 10% of it). Verify: memory stays bounded, duration is acceptable, no timeouts, no lock contention.
- [ ] **Edge cases** — Empty source (zero records to process — should succeed, not error). Single record. Exactly chunk-size records. Source with all invalid records.
- [ ] **Backfill tests** — Run job with historical date range. Verify it processes correctly without affecting current data.

## 10. Deployment & Operations

- [ ] **Containerized** — Same container image as other services. Multi-stage build. Non-root user. Health check for long-running workers.
- [ ] **Infrastructure as code** — Job definitions (CronJob YAML, Airflow DAGs, Step Functions JSON, scheduled tasks) versioned in git. Not hand-configured in production.
- [ ] **Environment parity** — Same job code runs in dev, staging, production. Only config (connection strings, schedules, chunk sizes) differs. Dev can run against synthetic/anonymized data.
- [ ] **Rollback plan** — If new job version produces bad data: how to revert? Rollback the code AND compensate for bad data already written. Document the procedure.
- [ ] **Capacity planning** — Know your data growth rate. Job that takes 1 hour today with 1M records — how long with 10M? Plan scaling triggers before you hit the wall.
- [ ] **Runbook** — How to: manually trigger, check status, kill a stuck job, replay failed records from DLQ, backfill a date range, scale workers up/down. Written for the 3 AM oncall.
- [ ] **Feature flags** — Enable/disable job processing without redeployment. Useful for: pausing during incidents, gradual rollout of new logic, A/B processing paths.
- [ ] **Config without redeploy** — Chunk size, concurrency, rate limits, skip-on-error threshold: configurable at runtime (env vars, config service, feature flags). Tune without rebuilding.

## 11. Specific Patterns

### ETL / Data Pipeline
- [ ] **Extract** — Paginated/cursor-based reads. Don't lock source tables. Read replicas if available.
- [ ] **Transform** — Pure functions. Testable in isolation. Schema mapping documented. Handle schema evolution (new columns, type changes).
- [ ] **Load** — Bulk insert to staging → validate → swap/merge to target. Not direct insert to production table during transform.
- [ ] **Lineage tracking** — Where did this record come from? Which source, which run, which transformation? Metadata columns: `source_system`, `batch_run_id`, `loaded_at`.

### Queue Consumer / Worker
- [ ] **Acknowledgment after processing** — Don't ACK before processing completes. Process → commit → ACK. If crash between process and ACK: message redelivered (at-least-once).
- [ ] **Visibility timeout / lease** — Set longer than max processing time. If processing takes 5 min, visibility timeout must be > 5 min. Extend lease for long operations.
- [ ] **Consumer group / competing consumers** — Multiple workers consuming from the same queue. Messages distributed across workers. Scaling: add workers. No coordination needed.
- [ ] **Poison message circuit breaker** — If the same message fails N times across all consumers: stop retrying, move to DLQ, alert. Don't let one bad message block the entire queue.

### Scheduled Report / Export
- [ ] **Output to object storage** — S3, GCS, Azure Blob. Not local filesystem. Presigned URL for download. Lifecycle policy for cleanup.
- [ ] **Incremental over full** — Export only what changed since last run (CDC, updated_at filter). Full export only when necessary (reconciliation, initial load).
- [ ] **Format choice** — CSV for human consumption. Parquet/ORC for analytics. JSON/NDJSON for downstream systems. Avro for schema evolution.
- [ ] **Compression** — gzip for text formats (CSV, JSON). Snappy/LZ4 for Parquet (already columnar-compressed). Reduces storage cost and transfer time.

---

## Quick Sanity Check Before Launch

- [ ] Job is idempotent — tested by running twice, verified no duplicates
- [ ] Resumable from failure — tested by killing mid-run, verified it resumes
- [ ] Partial failure handled — 1 bad record doesn't kill 10,000 good ones
- [ ] Timeout configured — job can't run forever
- [ ] No overlapping runs — concurrent execution prevented or safely partitioned
- [ ] Memory bounded — tested with production-scale data, no OOM
- [ ] Progress visible — can see how far along a running job is
- [ ] Alerting works — failed job triggers alert within minutes
- [ ] Missed schedule detected — if job didn't run, someone knows
- [ ] DLQ/error table exists — failed records are inspectable and replayable
- [ ] Secrets not in code or logs — verified, not assumed
- [ ] Graceful shutdown tested — SIGTERM mid-chunk, verified clean exit
- [ ] Backfill tested — can re-process historical data without side effects
- [ ] Runbook exists — oncall can operate the job without reading source code

---

## Orchestrator Quick Reference

### Kubernetes CronJob
- **Best for:** Simple scheduled jobs, no dependencies between jobs
- **Strengths:** Native K8s, no extra infra, job history, pod resource limits
- **Limitations:** No DAG, no built-in retry/backoff, limited monitoring

### Apache Airflow / Prefect / Dagster
- **Best for:** Complex DAGs, data pipelines, ETL with dependencies
- **Strengths:** DAG visualization, scheduling, retry, backfill, monitoring
- **Trade-offs:** Operational overhead (Airflow), learning curve, separate infra

### AWS Step Functions / Azure Durable Functions / GCP Workflows
- **Best for:** Serverless batch, cloud-native, event-driven workflows
- **Strengths:** Managed, pay-per-execution, built-in retry/error handling, visual
- **Trade-offs:** Vendor lock-in, execution time limits, cold starts

### Temporal / Inngest
- **Best for:** Long-running workflows, saga patterns, durable execution
- **Strengths:** Code-first, resumable, versioning, visibility, language SDKs
- **Trade-offs:** Self-host complexity (Temporal), newer ecosystem

### BullMQ / Celery / Sidekiq
- **Best for:** Queue-based workers, background jobs, real-time processing
- **Strengths:** Simple, language-native, dashboard, priority queues, rate limiting
- **Trade-offs:** Not designed for DAGs, less built-in for complex workflows

### Spring Batch
- **Best for:** Java/Kotlin enterprise batch, chunk-oriented processing
- **Strengths:** Chunk/tasklet model, restart/skip/retry built-in, job repository
- **Trade-offs:** Java-only, heavy framework, steep learning curve
