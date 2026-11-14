# Data Pipeline Checklist

> Framework-agnostic checklist for data pipelines and analytics engineering: ingestion, transformation, contracts, quality, and operations.
> Batch or streaming, warehouse or lake — the disciplines are the same.
> Last updated: 2026-11-15 (created)

Master data layer: [[database]]. Streaming semantics: [[data-messaging]].

---

## 1. Architecture & Orchestration

- [ ] **Batch vs streaming chosen deliberately:** the freshness requirement drives the choice (hourly batch fine for finance close; streaming only when consumers actually need seconds) — not fashion, not resume padding
- [ ] **ELT over ETL for warehouse-scale data:** land raw data first, transform in the engine that scales (modern warehouses/lakes make in-place transforms cheaper than a separate transform tier)
- [ ] **Layered modeling:** staging (1:1 with sources, lightly cleaned) → intermediate (business logic, joins) → marts (consumable, dimensional). Consumers query marts, never staging → mirrors [[database]] §1
- [ ] **Orchestration with a real scheduler:** Airflow/Dagster/Prefect-class tool with dependency DAGs, retries with backoff, per-task timeouts, and catchup/backfill controls — not a wall of cron jobs
- [ ] **Idempotent stage design:** re-running any stage converges to the same state (upsert/merge semantics, deterministic keys, no blind appends) → same discipline as [[database]] §2 backfills
- [ ] **Pipeline DAG structure reviewed:** no circular dependencies; a failing upstream blocks downstream rather than producing half-truth data; critical path identified

---

## 2. Data Contracts & Schemas

- [ ] **Producer↔consumer contract defined:** schema + semantics (what a null means, units, currency) + freshness promise per published dataset
- [ ] **Schema registry in the path:** Avro/Protobuf/JSON Schema-class registry governs serialized event and table schemas; unregistered schemas don't reach production

```text
schema-registry check on CI: register new schema -> compatibility = BACKWARD passes -> deploy
```

- [ ] **Backward-compatible evolution only:** add fields with defaults, never repurpose or reinterpret an existing field; expand-contract for pipelines exactly as in [[database]] §2
- [ ] **Breaking-change process exists:** announce → dual-run old/new schema → verify consumers → cut over → deprecate. Never a silent rename on Friday
- [ ] **CDC change semantics explicit:** insert/update/delete/tombstone events handled distinctly (Debezium-class capture); deletes must not be silently ignored by downstream models
- [ ] **Contract version pinned per consumer:** consumers declare which schema version they tolerate; producers verify compatibility before deploying

---

## 3. Ingestion

- [ ] **CDC preferred over polling where available:** change data capture reads the transaction log, so updates and deletes are never missed (polling sees only rows it looks at) → source-side patterns in [[data-messaging]]
- [ ] **Exactly-what-happened capture:** before/after images retained where reconciliation or audit requires them; delete payloads not stripped by the capture layer
- [ ] **Deduplication by event identity:** unique event ID (log offset + change LSN, or source PK + version) enforced at the landing layer; replays don't double-count
- [ ] **Late-arriving data policy:** watermark + allowed-lateness window defined per source; data arriving after the window lands in quarantine with a repair path, not silently dropped
- [ ] **Clock-skew tolerance:** event time (source clock) vs processing time distinguished; ordering uses event time or monotonic change IDs, not ingestion timestamps
- [ ] **Source outage → catch-up plan:** ingestion resumes from last committed watermark/offset with a documented backfill; the pipeline state survives restarts

---

## 4. Transformation

- [ ] **Deterministic keys and joins:** stable surrogate keys (hash of natural key columns, not row order); **no floating-point join keys** — ever; timestamps normalized to UTC before joining
- [ ] **Slowly changing dimensions handled explicitly:** SCD2 (valid-from/valid-to + current flag) where history matters, SCD1 only where overwrite is truly acceptable; type chosen per dimension, not by default
```sql
-- incremental with watermark; deletes need CDC or reconciliation, not this alone
select * from source_events
where updated_at > (select max(watermark) from {{ this }})
```

- [ ] **Incremental models with watermark columns:** process only new/changed rows via `updated_at` or change version — **verified on deletes** (watermarking alone misses them; pair with CDC or periodic reconciliation) → [[database]] §10
- [ ] **Full-refresh where the dataset is small:** simplicity beats cleverness below ~1M rows; incremental logic is a cost paid only when it earns its keep
- [ ] **SQL reviewed like code:** models live in git, peer-reviewed via PR, naming conventions enforced; no hand-edited production tables or dashboard-invisible transforms
- [ ] **No hidden business logic in BI tools:** metric definitions live in the transformation layer, not in a chart's calculated field nobody can review

---

## 5. Data Quality

- [ ] **Tests at every layer:** not-null, unique, referential integrity, accepted values, freshness, volume-anomaly, and distribution checks (dbt-test/Great-Expectations-class) run on every load → constraints philosophy from [[database]] §10
- [ ] **Quarantine bad records visibly:** failed rows land in a quarantine table with the failure reason — never silent drops, never "filtered in the WHERE clause"
- [ ] **Quality SLAs per dataset:** freshness (data no older than X) + accuracy (quality-test pass rate) commitments per published dataset, with a named owner
- [ ] **Column-level lineage recorded:** upstream source → transformation → mart column mapping maintained (lineage-tooling or documented DAG) so incident archaeology is minutes, not days
- [ ] **Schema-drift alerts on sources:** unexpected column add/drop/type-change on any source fires an alert before it silently breaks models
- [ ] **Anomaly baselines, not just absolutes:** row counts and null rates compared against historical ranges (day-of-week, seasonality) — a "volume > 0" check catches nothing

---

## 6. Streaming Specifics

- [ ] **Consumer groups and offset commits understood:** at-least-once is the default; auto-commit after processing (never before) and idempotent consumers are mandatory → deep semantics in [[data-messaging]]
- [ ] **Rebalancing behavior understood:** what happens to in-flight partitions when a consumer joins/leaves; processing within `max.poll` limits; cooperative-sticky or equivalent where available
- [ ] **Dead-letter queues for poison messages:** unprocessable messages route to a DLQ after N retries with full context; DLQ is monitored and has a replay procedure
- [ ] **Consumer lag monitored with alerts:** lag per consumer group tracked as a time-series; alert before the topic's retention window is crossed (retention crossing = permanent data loss)
- [ ] **Backpressure and partition-count planning:** partition count sized for peak parallelism (changing it later re-keys the stream); slow consumers don't destabilize producers
- [ ] **Exactly-once only where genuinely offered:** exactly-once semantics claimed only within a single platform's transactional boundary (e.g. bounded consume-process-produce); across systems, design idempotent consumers → [[data-messaging]]

---

## 7. Storage & Tables

- [ ] **Lakehouse table format where history matters:** Iceberg/Delta/Hudi-class format gives snapshot isolation, time-travel, and safe compaction on object storage — schema evolution without rewrites
- [ ] **Partitioning/clustering keys match query patterns:** time-based partitioning for time-series access; clustering on high-cardinality filter columns; partition pruning verified on the top queries → parallels [[database]] §4
- [ ] **Small-files problem managed:** scheduled compaction jobs (or format-native compaction) keep file counts sane; unbounded tiny files silently destroy scan performance
- [ ] **Retention tiers per table:** hot (fast storage, full detail) → warm (compressed, older partitions) → archive (frozen, compliance minimum) with automated lifecycle moves → cost angle in [[finops]]
- [ ] **Time-travel window defined:** snapshot retention long enough for "undo bad backfill" but not so long it doubles storage; vacuum/cleanup jobs scheduled

---

## 8. Privacy & Governance in the Pipeline

- [ ] **PII tokenized/masked before wide marts:** raw zone access restricted to the ingestion team; downstream marts see pseudonymized values unless a documented use case requires raw → [[privacy]], [[database]] §9
- [ ] **Sensitive-table access audited:** who queried raw PII tables, when, why — audit log retained and reviewed, not just enabled → [[security]]
- [ ] **Erasure requests propagate end-to-end:** a DSAR/erasure in the source system cascades to marts, aggregates, and ML features — or a documented suppression strategy blocks re-import; verified by sampling after execution → [[privacy]]
- [ ] **Data classification labels on datasets:** every published dataset tagged (public / internal / confidential / restricted) in the catalog; classification drives access and export rules
- [ ] **Secrets never in pipeline code:** connection credentials from a secrets manager or environment injection, rotated, never committed → [[02 Secrets Management]]
- [ ] **Aggregates checked for re-identification:** small-cell suppression or k-anonymity on published aggregates exposing individuals

---

## 9. Operations

- [ ] **Pipeline freshness SLA per dataset with an owner:** "table X refreshed daily by 07:00" — a named human, not a team alias → incident routing in [[ops]]
- [ ] **Failed-run alerting pages the owner:** silent failure is worse than no pipeline; alert on failure, staleness, and SLA breach — not just on crash → [[ops]]
- [ ] **Retry policy distinct from rerun/backfill runbooks:** automatic retries (transient) vs manual reruns (bad input fixed) vs backfills (historical gap) each have documented procedures
- [ ] **Backfills planned, not improvised:** chunked by time/partition, throttled against source and warehouse load, cost-estimated before launch, resumable after interruption
- [ ] **Pipeline cost tracked per domain/dataset:** warehouse compute + storage attributed to teams/marts; runaway models and unattended dashboards are visible in the bill → [[finops]]
- [ ] **Run metadata recorded:** every run logs start/end, rows processed, watermark advanced — the audit trail for "when did this table last actually change?"

---

## 10. Testing & CI

- [ ] **Model/SQL tests run in CI on every change:** PR cannot merge with failing tests; pipeline compiles against the schema registry/warehouse catalog before deployment
- [ ] **Fixture-based pipeline tests:** known input → expected output on a small sample (golden dataset); catches logic regressions that schema tests miss
- [ ] **Data-diff before/after migration cutovers:** row counts, checksums, and column-level diffs between old and new pipeline outputs until parity is proven → cut-over discipline from [[database]] §2
- [ ] **Sampled-equality checks on replications:** continuous sampled comparison between source and warehouse for CDC pipelines; drift alerts before consumers notice
- [ ] **Contract tests between producers and consumers:** producer CI verifies schema compatibility against registered consumer contracts; consumer CI verifies against the producer's latest published schema

---

## 11. Access & Self-Serve

- [ ] **Semantic layer with single-sourced metrics:** "revenue" defined once, computed once, reused everywhere; two dashboards disagreeing on a KPI is a governance failure
- [ ] **BI tool governance:** certified/gold datasets are the only ones endorsed for decisions; personal scratch models clearly marked and not linked to exec dashboards
- [ ] **Export/egress controls on sensitive data:** bulk exports of confidential/restricted data require approval and are logged; CSV-to-laptop is a data breach vector → [[security]]
- [ ] **Access requests audited:** who got access to which dataset, approved by whom, reviewed on a schedule; standing access to restricted data expires

---

## Quick Sanity Check Before Release

- [ ] Freshness SLA per published dataset, with a named owner and a working alert
- [ ] Every model in git, tested in CI, peer-reviewed — zero hand-edited production tables
- [ ] Quality tests cover null/unique/referential/freshness; bad rows quarantined, never silently dropped
- [ ] Schema evolution is additive-only, or a dual-run cut-over plan exists; contract tests gate deploys
- [ ] CDC pipelines verified on deletes; streaming consumers idempotent with DLQ and lag alerts
- [ ] PII masked before wide marts; erasure requests demonstrably reach marts and aggregates
- [ ] Backfill/rerun runbooks tested; pipeline cost attributed per domain
- [ ] Data-diff parity proven for any migration or cut-over in flight

## Sources

- dbt docs (testing, incremental models, model contracts): https://docs.getdbt.com/docs/build/data-tests
- Debezium (CDC change semantics): https://debezium.io/documentation/
- Apache Iceberg (table format, snapshots, schema evolution): https://iceberg.apache.org/docs/latest/
- Apache Kafka (streaming semantics reference): https://kafka.apache.org/documentation/
- Master data layer: [[database]].
- Streaming and messaging semantics: [[data-messaging]].
- Privacy handling and erasure: [[privacy]]. Secrets: [[02 Secrets Management]]. Incidents: [[ops]]. Cost: [[finops]]. Hardening: [[security]].
