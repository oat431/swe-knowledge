# Database Checklist

> The **data layer** checklist — database design, operations, and performance across all engines.
> Complements [[Release]] (migration gates), [[Security]] (data protection), and the domain checklists (api, batch, microservice, infra).
> Engine-specific companions: [[PostgreSQL]] (relational), [[MongoDB]] (document), [[Valkey]] (cache/KV), [[Elasticsearch]] (search/analytics), [[ClickHouse]] (OLAP/columnar), [[SQLite]] (embedded), [[Cassandra]] (wide-column).
> Framework-agnostic — the principles are the same whether the engine is SQL or NoSQL, OLTP or OLAP, client-server or embedded.
> Last updated: 2026-08-07

---

## 1. Data Modeling & Schema Design

- [ ] **Entity-relationship model exists** — Entities, relationships, cardinality documented before schema DDL. Not "we'll figure it out in queries."
- [ ] **Normalization vs denormalization decided** — 3NF for transactional (OLTP) integrity. Deliberate denormalization for read-heavy (OLAP/reporting) or document-modeled data. Every denormalization has a documented reason.
- [ ] **Keys defined** — Natural vs surrogate keys chosen per table. UUIDs vs auto-increment decided (UUID for distributed/multi-writer, bigint for single-writer simplicity). Primary keys on every table.
- [ ] **Constraints everywhere** — NOT NULL, CHECK, UNIQUE, foreign keys. The database enforces integrity — application code forgets. FK indexes created (see §4).
- [ ] **Data types correct** — Timestamps with timezone (`timestamptz`), not strings. Money as decimal, not float. Dates as dates. Wrong types = silent bugs.
- [ ] **Soft delete vs hard delete** — Chosen per table (audit/regulatory → soft; high-volume/transient → hard). Consistent pattern, not ad-hoc.
- [ ] **Audit columns** — `created_at`, `updated_at` (and `created_by`/`updated_by` where relevant) on tables that need traceability.
- [ ] **Character encoding & collation deliberate** — UTF-8 end-to-end (DB, connection, client). Collation chosen consciously (`en-US` vs `th-TH`) — it affects sorting, comparisons, and UNIQUE constraints. Wrong collation = duplicate detection misses. Especially critical for Thai text.
- [ ] **Schema ownership & data contract** — Each table has a documented owner (which service/team writes it) and known consumers. Schema changes require consumer notification. Breaking changes gated via expand-contract (see §2). No surprise `ALTER TABLE` on shared tables.

## 2. Migrations & Schema Evolution

- [ ] **Migrations versioned** — Alembic / Flyway / Liquibase / Prisma migrations in version control. Applied in order, never edited after release → gate in [[Release]] §4.
- [ ] **No destructive auto-DDL** — No `ddl-auto: update` / `create-drop` / `db.sync()` in production. Migrations are explicit and reviewed.
- [ ] **Expand-contract for breaking changes** — Add column → deploy → backfill → switch code → drop old column. Never one-shot destructive migration.
- [ ] **Backfills are idempotent** — Re-running a backfill must not duplicate or corrupt data. Use `UPDATE ... WHERE NOT ...` patterns, checksums, or resumable batch processing. A crashed backfill at 80% must be safely restartable.
- [ ] **Downgrade path** — Every migration has a down-migration or documented restore procedure. Tested in staging.
- [ ] **Migrations tested in CI** — Apply migrations from scratch in CI. Catches ordering, missing defaults, and environment drift.
- [ ] **Migration rollback tested** — Down-migration actually executes and produces a valid prior state, not just "documented." Schema drift after rollback verified.
- [ ] **Test data strategy** — Factories/fixtures/synthetic data for tests, never production data. Testcontainers or ephemeral DB instances for integration tests — not mocks for query logic.
- [ ] **Data assertions in tests** — Tests verify constraints, triggers, cascades, and unique checks actually behave. Not just "the query runs without error."

## 3. Backup & Recovery

- [ ] **Backup strategy defined** — Full + incremental/WAL (point-in-time recovery) for relational. Snapshots + oplog/change-stream replay for document. RPO/RTO numbers documented.
- [ ] **Backups tested — restore actually works** — Scheduled restore drills (quarterly minimum). A backup that was never restored is a hope, not a backup.
- [ ] **Backups encrypted** — At rest (KMS/managed keys) and in transit. Off-site or cross-region copy.
- [ ] **Backup retention policy** — Daily/weekly/monthly retention defined (regulatory minimum if applicable). Purge old backups automatically.
- [ ] **Backup monitoring** — Backup job failure alerts. "Last successful backup" check on dashboards. Silent backup failure is a ticking bomb.

## 4. Indexing & Query Performance

- [ ] **Indexes on FK and hot WHERE columns** — Every foreign key indexed. Filter/join columns from real query patterns (not guesses).
- [ ] **Composite indexes match query shape** — Column order matters: equality first, range last. Leftmost-prefix rule understood.
- [ ] **N+1 queries hunted** — `EXPLAIN`/query logs reviewed. ORM lazy loading disabled where it causes N+1 → see engine checklists.
- [ ] **Index bloat managed** — Reindex/rebuild schedule for frequently-updated indexes. Monitor index size vs table size.
- [ ] **Covering indexes for hot paths** — Include columns to avoid table lookups on critical queries. Only for proven hot paths — every index costs writes.
- [ ] **Query plan review** — `EXPLAIN ANALYZE` on slow queries. Sequential scans on large tables flagged.
- [ ] **Slow query log enabled** — Threshold set (e.g. > 100ms), logged centrally, reviewed. The first place to look when something's slow.
- [ ] **Partitioning strategy for large tables** — Tables exceeding ~10M rows or time-series data have a partition strategy (range/list/hash) with a documented reason. Partition pruning verified — queries actually hit only relevant partitions. Retention via `DROP PARTITION` for old data instead of `DELETE` (no bloat, instant).

## 5. Connection Management

- [ ] **Connection pool configured** — Pool size deliberate (not default unlimited, not 1). `pool_size` / `max_connections` tuned: (CPU cores × 2) + spare is a sane starting point.
- [ ] **No connection leaks** — Sessions closed in `finally`/context managers. Pool exhaustion is a top-3 production outage cause.
- [ ] **Pool monitoring** — Active/idle/waiting connections tracked. Alerts at 80% pool utilization.
- [ ] **Timeouts set** — connect, read, write timeouts on all clients. `statement_timeout` on the server side as a safety net.
- [ ] **Transaction discipline** — Short transactions, no user I/O inside a transaction, `READ COMMITTED` default unless isolation level deliberately chosen.

## 6. Caching Strategy

- [ ] **Cache layers mapped** — Application cache (in-memory) → shared cache ([[Valkey]]/Redis) → database. Each layer's purpose documented.
- [ ] **Cache invalidation designed** — TTL-based, event-driven (pub/sub, CDC), or write-through. Stale data is a product bug — know your tolerance.
- [ ] **Cache stampede protection** — Locking or single-flight on cache miss. Thundering herd on popular keys takes the DB down.
- [ ] **Hot keys identified** — A few keys handling most traffic? Replica reads, local caches, or sharding for hot keys.
- [ ] **Cache hit rate monitored** — Dashboard + alert. Dropping hit rate = invalidation bug or TTL too short.

## 7. Replication & High Availability

- [ ] **HA architecture chosen** — Single-instance + backups (low tier) → primary + replica with failover (medium) → multi-region / active-active (high tier). Matches the tier matrix below.
- [ ] **Replication lag monitored** — Replica lag tracked; alert on drift. Read-your-writes handled (read from primary when consistency matters).
- [ ] **Failover tested** — Chaos/game-day: kill the primary, verify promotion, verify app recovers. Untested failover is fiction.
- [ ] **Split-brain prevented** — Quorum/fencing for multi-node setups. Never two primaries.
- [ ] **Writes scale plan** — If write throughput is the bottleneck: partition/shard BEFORE you need it (hard to retrofit).

## 8. Security & Access Control

- [ ] **Least-privilege accounts** — App connects with its own user, not `root`/`admin`/`postgres`. Read-only replicas for reporting. Grants, not blanket privileges.
- [ ] **No secrets in connection strings** — Credentials from env/vault, never hardcoded → see [[Security]] §4.
- [ ] **TLS for connections** — Client ↔ DB encrypted (`sslmode=require`/equivalent). Internal networks are not inherently trusted.
- [ ] **Network isolation** — DB not internet-exposed. Firewall/VPC/subnet rules. Admin ports closed.
- [ ] **Audit logging** — Who accessed what, when (pgAudit, audit log, change streams). Required for regulated tiers.
- [ ] **Encryption at rest** — Volume/disk encryption or engine-native (TDE). Backup encryption too → [[Security]] §8.

## 9. Privacy & Data Protection

- [ ] **PII inventory & classification** — Every sensitive column identified and labeled (email, phone, national ID, health data, financial data). Classification drives handling rules — you cannot protect what you have not identified.
- [ ] **Right to erasure / data subject requests** — Can you actually delete a user's data across *all* tables, foreign-key cascades, analytics copies, replicas, caches, logs, and backups? Documented procedure. Required for GDPR/CCPA/PDPA.
- [ ] **Data retention policy** — Not just backup retention, but *business data* retention. When can rows be archived or deleted? Automated archival/purge for stale data. Holding data longer than necessary is a legal liability.
- [ ] **Anonymization / pseudonymization** — Analytics copies, test environments, and data exports use masked or synthetic data. Never raw production PII in a dev database.
- [ ] **Consent & lawful basis tracked** — If collecting personal data: consent recorded, purpose documented, lawful basis identified (where applicable). Required for GDPR-regulated products.

## 10. Data Quality

- [ ] **Constraints as quality gates** — CHECK constraints, NOT NULL, enums, and domain types enforce validity at the DB level. The database is the last line of defense — application validation alone is insufficient.
- [ ] **Referential integrity verified** — Orphaned FK rows detected and remediated. Soft-deleted parents with active children handled deliberately (not accidentally).
- [ ] **Data profiling on load** — Null counts, cardinality, value distribution, and type conformance checked before and after migrations/backfills. Catches silent corruption that breaks downstream consumers.
- [ ] **Quality monitoring over time** — Row counts, null rates, uniqueness ratios, and value distributions tracked as time-series. Silent drift (dropping row counts, rising null rates) triggers alerts before users notice.
- [ ] **Natural key uniqueness across edge cases** — Uniqueness holds across soft-deleted rows, case variations, and whitespace differences. `UNIQUE` constraint alone may miss these — additional application or DB-level checks where needed.

## 11. Maintenance & Operations

- [ ] **Autovacuum / compaction tuned** — Default settings fail on write-heavy tables (PostgreSQL autovacuum, MongoDB compaction, etc.). Vacuum bloat kills performance silently. Tune thresholds per table, not globally.
- [ ] **Statistics kept fresh** — `ANALYZE` schedule or auto-statistics enabled. Stale stats = bad query plans = mysterious slowdowns.
- [ ] **Maintenance window defined** — Reindex, vacuum, stats refresh, and compaction run in a defined, low-impact window. Not "whenever someone remembers."
- [ ] **Deadlock detection & prevention** — Lock ordering documented for high-concurrency paths. Deadlock retry logic in application code. Monitor deadlock rate.
- [ ] **Long-running transaction monitoring** — Transactions open > N seconds flagged and killed. Long transactions block vacuum, bloat tables, and starve connections.

## 12. Observability & Monitoring

- [ ] **Key metrics collected** — Connections, queries/sec, cache hit ratio, replication lag, disk I/O, WAL/oplog growth, table/index bloat.
- [ ] **Alerts configured** — Disk space > 80%, connection pool > 80%, replication lag > threshold, slow query count spike, backup failure.
- [ ] **Dashboards exist** — Engine overview dashboard (Prometheus + Grafana or managed equivalent). Anyone can answer "is the DB healthy?" in 10 seconds.
- [ ] **Capacity planning** — Growth trend reviewed quarterly: disk, memory, connections. Plan before the alarm.

---

## Quick Sanity Check Before Launch

- [ ] Schema matches migration state (no drift)
- [ ] All FKs indexed, hot queries EXPLAIN-analyzed
- [ ] Backup running AND restore tested
- [ ] Connection pool sized and monitored
- [ ] Least-privilege app account, TLS on connections
- [ ] Slow query log on, key alerts armed
- [ ] Cache invalidation verified for the main flows
- [ ] Replication (if any) lag monitored, failover practiced
- [ ] PII columns identified and access-controlled
- [ ] Backup of production does not contain unmasked PII in dev/test environments
- [ ] Data quality constraints (CHECK, FK, NOT NULL) present and tested
- [ ] Autovacuum/compaction tuned for write-heavy tables

---

## Project Tier Scoping Matrix

> **How to use this table:** Pick your tier first, then focus only on the sections marked ✅ (required) or 🟡 (recommended). Skip ❌ sections entirely — they'd be over-engineering for your context.
>
> **Legend:** ✅ Required · 🟡 Recommended / partial · ❌ Skip

### Tier Descriptions

| # | Tier | Description | Typical Team | Users | Lifespan |
|---|---|---|---|---|---|
| 1 | 🧪 **POC / Spike** | Validate an idea. Throwaway data. SQLite is fine. | 1 dev | Internal only | Days–weeks |
| 2 | 🔧 **Prototype / MVP** | Waiting for integration or user validation. Might become real. | 1–2 devs | Beta testers | Weeks–months |
| 3 | 🏠 **Internal Tool** | Real users (employees), real data. No external exposure or paying customers. | 1–3 devs | Employees | Ongoing |
| 4 | 🟢 **Small Production** | Single engine, low traffic. Real users, maybe early revenue. | 1–2 devs | < 1K users | Ongoing |
| 5 | 🔵 **Medium Production** | Multiple engines or higher traffic. Real revenue or user base that matters. | 2–5 devs | 1K–100K users | Ongoing |
| 6 | 🟣 **Production Grade** | Full rigor — high-stakes SaaS, enterprise product, or large user base. | 5+ devs | 100K+ users | Long-term |
| 7 | 🔴 **Mission-Critical / Regulated** | Healthcare (HIPAA), finance (PCI-DSS), safety systems. Failure = severe harm. Adds formal verification, regulatory audit. | 10+ devs | Varies | Decades |

### Which Tier Am I?

```mermaid
flowchart TD
    A[Is this throwaway / exploratory?] -->|Yes| T1[🧪 Tier 1 or 2<br/>POC / Prototype]
    A -->|No| B[Are the users internal<br/>employees?]
    B -->|Yes| T3[🏠 Tier 3<br/>Internal Tool]
    B -->|No| C[Do paying users or real<br/>revenue depend on it?]
    C -->|No| T4[🟢 Tier 4<br/>Small Production]
    C -->|Yes| D[Multiple services or<br/>1K+ users?]
    D -->|No| T4
    D -->|Yes| E[Enterprise / high-stakes<br/>/ regulated industry?]
    E -->|No| T5[🔵 Tier 5<br/>Medium Production]
    E -->|Yes| F[Failure could cause<br/>severe harm?]
    F -->|No| T6[🟣 Tier 6<br/>Production Grade]
    F -->|Yes| T7[🔴 Tier 7<br/>Mission-Critical]
    
    style T1 fill:#e1f5ff
    style T3 fill:#fff4e1
    style T4 fill:#e8f5e9
    style T5 fill:#e3f2fd
    style T6 fill:#f3e5f5
    style T7 fill:#ffebee
```

### Checklist Applicability by Tier

| # | Section | 🧪 POC | 🔧 Prototype | 🏠 Internal | 🟢 Small Prod | 🔵 Medium Prod | 🟣 Production Grade | 🔴 Mission-Critical |
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 1 | Data Modeling | 🟡 minimal | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ + formal review |
| 2 | Migrations | ❌ SQLite auto | 🟡 simple | ✅ | ✅ | ✅ + expand-contract | ✅ + tested in CI | ✅ + approval + rollback test |
| 3 | Backup & Recovery | ❌ | 🟡 dump files | ✅ + scheduled | ✅ + restore tested | ✅ + PITR | ✅ + drills | ✅ + regulatory |
| 4 | Indexing & Performance | ❌ | 🟡 PK only | ✅ + hot queries | ✅ + EXPLAIN | ✅ + bloat mgmt | ✅ + partitioning | ✅ + capacity plan |
| 5 | Connection Management | ❌ | 🟡 default pool | ✅ + sized | ✅ + monitored | ✅ + timeouts | ✅ + pool alerts | ✅ + formal |
| 6 | Caching Strategy | ❌ | ❌ | 🟡 if needed | 🟡 | ✅ + invalidation | ✅ + stampede guard | ✅ + hot-key plan |
| 7 | Replication & HA | ❌ | ❌ | 🟡 if needed | 🟡 replica optional | ✅ + failover tested | ✅ + multi-region | ✅ + active-active |
| 8 | Security & Access | 🟡 local only | 🟡 app user | ✅ + least priv | ✅ + TLS | ✅ + audit logs | ✅ + full hardening | ✅ + regulatory |
| 9 | Privacy & Data Protection | ❌ | 🟡 mask in dev | 🟡 PII inventory | ✅ + erasure plan | ✅ + retention policy | ✅ + anonymization | ✅ + DSR automation |
| 10 | Data Quality | ❌ | 🟡 constraints | ✅ + FK checks | ✅ + profiling | ✅ + monitoring | ✅ + scorecards | ✅ + formal SLAs |
| 11 | Maintenance & Operations | ❌ | ❌ | 🟡 basic | ✅ + autovacuum | ✅ + deadlock monitor | ✅ + maintenance window | ✅ + formal |
| 12 | Observability | ❌ | ❌ | 🟡 basic | ✅ + alerts | ✅ + dashboards | ✅ + capacity planning | ✅ + full stack |

---

## Sources

- Complements [[Release]] (migration gates), [[Security]] (data protection), [[Infrastructure]] (homelab/AWS hosting).
- Engine-specific: [[PostgreSQL]], [[MongoDB]], [[Valkey]].
- Deep references: SQL standard, your Database vault (`F:\obsidian_note\swe-knowledge\computing-foundation-note\Database\`).
- Data quality dimensions and privacy engineering informed by DMBOK v2 and GDPR/CCPA/PDPA frameworks.
