# PostgreSQL Checklist

> PostgreSQL companion to the general [[Database]] checklist.
> Covers PostgreSQL 18 (current major, Nov 2025) — the standard open-source relational engine.
> Companion to [[MongoDB]] and [[Valkey]] for the other engines in the homelab.
> Last updated: 2026-08-05

---

## 1. Setup & Configuration

- [ ] **Version** — PostgreSQL 18 (or current LTS-ish major). `SELECT version();` recorded. Upgrade path known before you need it.
- [ ] **Config tuned** — `shared_buffers` (25% of RAM typical), `work_mem`, `effective_cache_size`, `max_connections` deliberate. Defaults are for laptops → [[Database]] §5.
- [ ] **`hba.conf` locked down** — `pg_hba.conf`: scram-sha-256 auth, no `trust` for anything reachable, restricted source IPs.
- [ ] **`postgresql.conf` review** — `listen_addresses` bound to the right interface, `max_wal_size`/`checkpoint_completion_target` sane, logging configured (see §8).
- [ ] **Extensions pinned** — `CREATE EXTENSION` list documented with versions (pgvector, postgis, uuid-ossp, pg_stat_statements as needed). Extension upgrades tested.
- [ ] **Container/Docker notes** — Volume for `/var/lib/postgresql/data` (never container-local), `POSTGRES_PASSWORD` from env/vault, healthcheck `pg_isready` → [[Infrastructure]].

## 2. Schema & DDL

- [ ] **Migrations via Alembic/Flyway** — Versioned, ordered, reviewed. No `CREATE TABLE IF NOT EXISTS` hacks as "migrations" → [[Database]] §2.
- [ ] **`timestamptz` for all timestamps** — Never `timestamp without time zone` unless the app is single-TZ and you've documented why.
- [ ] **UUIDs vs bigint decided** — `gen_random_uuid()` for distributed/multi-writer; `bigint GENERATED ALWAYS AS IDENTITY` for single-writer. `SERIAL` is legacy.
- [ ] **Enums vs lookup tables** — Native `CREATE TYPE ... AS ENUM` for stable small sets (ALTER TYPE ADD VALUE is transactional-safe since PG12). Lookup tables when values grow or carry metadata.
- [ ] **Generated columns** — `GENERATED ALWAYS AS (...)` for derived data (e.g. `price * quantity`). Never store what you can compute — unless it's a hot path.
- [ ] **Partitioning for large tables** — Native `PARTITION BY RANGE/HASH` when tables exceed ~100GB or queries always filter by the partition key. Partition pruning verified with EXPLAIN.
- [ ] **`pgvector` for AI workloads** — If RAG/embeddings: `vector(1536)` columns + HNSW/IVFFlat indexes. See [[AI]] checklist for the application side.

## 3. Indexing (PostgreSQL-Specific)

- [ ] **B-tree defaults verified** — Default index type. Works for equality + range. Composite order: equality → range → sort.
- [ ] **`EXPLAIN ANALYZE` culture** — Every slow query gets explained. Look for: Seq Scan on large tables, Nested Loop on hot paths, missing index hints (pg_hint_plan in dev).
- [ ] **Partial indexes** — `WHERE` clause on the index for hot subsets (e.g. `WHERE status = 'active'` on 5% of rows). Big win for skewed data.
- [ ] **Expression indexes** — `LOWER(email)`, `(data->>'field')` for JSONB hot paths. Must match the query expression exactly.
- [ ] **JSONB with GIN** — `GIN` index for JSONB containment (`@>`) and array operators. B-tree won't help JSONB.
- [ ] **`pg_stat_statements` enabled** — The #1 tool: normalized query stats, identify top-by-total-time queries, track down N+1 and repeated queries.
- [ ] **Index bloat** — `pgstattuple`/`pg_repack` or scheduled `REINDEX CONCURRENTLY` for hot tables. `autovacuum` tuning checked.

## 4. Vacuum & Maintenance

- [ ] **Autovacuum running and tuned** — `autovacuum_vacuum_scale_factor`, `autovacuum_analyze_scale_factor` tuned for table size (defaults cause huge tables to vacuum rarely). Check `pg_stat_all_tables` for overdue vacuum.
- [ ] **`vacuum` debt monitored** — `n_dead_tup` vs `n_live_tup` per table. Dead tuple explosion = bloat + index bloat + frozen XID risk.
- [ ] **XID wraparound watch** — `datfrozenxid` age monitored. Emergency vacuum at 70% of 2^31. This takes the DB down if ignored.
- [ ] **`ANALYZE` up to date** — Stats drive the planner. `autoanalyze` on, or scheduled `ANALYZE` after bulk loads.
- [ ] **Bulk load pattern** — `COPY` over INSERT loops, drop indexes before mass load + recreate after, `synchronous_commit = off` only for non-durable bulk.

## 5. Backup & Point-in-Time Recovery

- [ ] **`pg_dump` for logical** — Schema + data dumps scheduled for smaller DBs / partial restores. `pg_dump -Fc` (custom format) for compressible portable backups.
- [ ] **`pg_basebackup` + WAL archiving for PITR** — Physical base backup + continuous `archive_command`/`pgBackRest`/`barman` WAL shipping → restore to any point in time.
- [ ] **pgBackRest or barman considered** — For medium+ tiers, dedicated backup tools beat hand-rolled scripts: retention, verification, parallelism.
- [ ] **Restore tested** — Quarterly restore drill: full restore + `pg_controldata` + consistency checks + app smoke test → [[Database]] §3.
- [ ] **`pg_verifybackup`** — Verify base backups after creation (PG13+). Cheap insurance against silently corrupt backups.

## 6. Replication & HA

- [ ] **Streaming replication** — Primary + hot standby (synchronous_commit = remote_apply for zero-loss if needed). `pg_basebackup` to bootstrap the replica.
- [ ] **Replication slots monitored** — `pg_replication_slots`: lag + `restart_lsn` vs current WAL. A stalled slot grows WAL forever (disk full).
- [ ] **Failover solution chosen** — Patroni (self-managed, standard), repmgr, or managed (Cloud SQL/RDS/Aurora). Automatic failover with quorum; never manual-only for medium+.
- [ ] **Read scaling** — Replica for read-only traffic; app routes reads appropriately (read-your-writes handled). Connection pooling per replica.
- [ ] **Failover drill** — Kill primary in staging, verify promotion, verify app reconnect, verify no data loss (or documented acceptable loss).

## 7. Connection Pooling

- [ ] **PgBouncer or managed pool** — PostgreSQL's per-connection process model makes pooling mandatory at scale. Transaction-mode pooling for most apps (session-mode only when session state needed).
- [ ] **Pool sizing** — `max_connections` + PgBouncer `default_pool_size`: (cores × 2) + spare per app instance. Too many connections = context-switch thrash.
- [ ] **`statement_timeout` set** — Server-side safety net (`statement_timeout = '30s'` typical, tune per workload). Kills runaway queries before they pile up.
- [ ] **`idle_in_transaction_session_timeout`** — Prevents abandoned transactions holding locks forever. Production must-have.

## 8. Observability (PostgreSQL)

- [ ] **`pg_stat_statements` on** — Top queries by total time / calls / rows. The first dashboard panel.
- [ ] **`pg_stat_activity` monitoring** — Active queries, long-running transactions, blocked sessions (`pg_locks` joins). Alerts on: long-running query > threshold, lock waits.
- [ ] **Metrics to Prometheus** — `postgres_exporter` (or managed): connections, cache hit ratio, WAL size, replication lag, dead tuples, bloat.
- [ ] **Slow query log** — `log_min_duration_statement = 100` + central log shipping. Reviewed weekly at minimum.
- [ ] **Capacity trend** — Disk, WAL growth, table growth trended quarterly. `pg_database_size()`/`pg_total_relation_size()` snapshots.

## 9. Security (PostgreSQL)

- [ ] **Roles least-privilege** — App role has only needed grants (SELECT/INSERT/UPDATE/DELETE on its schema, no DDL). Read-only reporting role. No app using `postgres` superuser → [[Security]] §8.
- [ ] **TLS enforced** — `ssl = on`, `ssl_min_protocol_version = 'TLSv1.2'`, `hostssl` entries in `pg_hba.conf`. `sslmode=require` in app connection strings.
- [ ] **`pgAudit` for audit trails** — Regulated tiers: `pgaudit` extension logging DDL/DML per role. Logs shipped centrally.
- [ ] **Row-level security (RLS)** — Multi-tenant: `CREATE POLICY` + `SET row_security` — the database enforces tenant isolation, not just the app.
- [ ] **Secrets via vault/env** — Connection strings from env/KMS, never config files in git → [[Security]] §4.

---

## Quick Sanity Check Before Launch

- [ ] `pg_stat_statements` enabled and top queries reviewed
- [ ] `EXPLAIN ANALYZE` done on all hot queries — no Seq Scan on large tables
- [ ] Autovacuum healthy: no table overdue, XID age < 70%
- [ ] Backup runs nightly AND restore tested this quarter
- [ ] Replication lag (if replica) < threshold, slots not stalled
- [ ] PgBouncer sized, `statement_timeout` + `idle_in_transaction_session_timeout` set
- [ ] `pg_hba.conf`: scram auth, no trust, restricted sources
- [ ] TLS enforced, app role least-privilege
- [ ] Slow query log on, disk/WAL growth trend recorded

---

## Project Tier Scoping Matrix

> **How to use this table:** Pick your tier first, then focus only on the sections marked ✅ (required) or 🟡 (recommended). Skip ❌ sections entirely — they'd be over-engineering for your context.
>
> **Legend:** ✅ Required · 🟡 Recommended / partial · ❌ Skip

### Tier Descriptions

| # | Tier | Description | Typical Team | Users | Lifespan |
|---|---|---|---|---|---|
| 1 | 🧪 **POC / Spike** | Validate an idea. Docker Postgres is fine. | 1 dev | Internal only | Days–weeks |
| 2 | 🔧 **Prototype / MVP** | Waiting for integration or user validation. Might become real. | 1–2 devs | Beta testers | Weeks–months |
| 3 | 🏠 **Internal Tool** | Real users (employees), real data. No external exposure or paying customers. | 1–3 devs | Employees | Ongoing |
| 4 | 🟢 **Small Production** | Single instance, low traffic. Real users, maybe early revenue. | 1–2 devs | < 1K users | Ongoing |
| 5 | 🔵 **Medium Production** | Higher traffic or multi-service. Real revenue or user base that matters. | 2–5 devs | 1K–100K users | Ongoing |
| 6 | 🟣 **Production Grade** | Full rigor — high-stakes SaaS, enterprise product, or large user base. | 5+ devs | 100K+ users | Long-term |
| 7 | 🔴 **Mission-Critical / Regulated** | Healthcare (HIPAA), finance (PCI-DSS), safety systems. Failure = severe harm. | 10+ devs | Varies | Decades |

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
| 1 | Setup & Configuration | 🟡 defaults | ✅ + tuned basics | ✅ | ✅ | ✅ | ✅ + full tuning | ✅ + formal |
| 2 | Schema & DDL | 🟡 minimal | ✅ | ✅ | ✅ | ✅ + partitioning | ✅ + pgvector | ✅ + formal review |
| 3 | Indexing | ❌ PK only | 🟡 hot queries | ✅ + EXPLAIN | ✅ + stat_statements | ✅ + partial/expr | ✅ + bloat mgmt | ✅ + capacity plan |
| 4 | Vacuum & Maintenance | ❌ | ❌ | 🟡 autovacuum on | ✅ + monitored | ✅ + bloat control | ✅ + XID watch | ✅ + formal |
| 5 | Backup & PITR | ❌ | 🟡 pg_dump | ✅ + scheduled | ✅ + restore tested | ✅ + PITR | ✅ + pgBackRest/barman | ✅ + regulatory |
| 6 | Replication & HA | ❌ | ❌ | 🟡 if needed | 🟡 replica optional | ✅ + failover tested | ✅ + Patroni | ✅ + multi-region |
| 7 | Connection Pooling | ❌ | ❌ | 🟡 if needed | ✅ + PgBouncer | ✅ + sized | ✅ + timeouts | ✅ + formal |
| 8 | Observability | ❌ | ❌ | 🟡 basic | ✅ + slow query log | ✅ + dashboards | ✅ + capacity trend | ✅ + full stack |
| 9 | Security | 🟡 local only | 🟡 app user | ✅ + least priv | ✅ + TLS | ✅ + RLS + audit | ✅ + pgAudit | ✅ + regulatory |

---

## Sources

- Complements [[Database]] (general), [[Release]] (migration gates), [[Security]] (data protection).
- PostgreSQL 18 docs — https://www.postgresql.org/docs/current/
- pgBackRest — https://pgbackrest.org/ · Patroni — https://patroni.readthedocs.io/
