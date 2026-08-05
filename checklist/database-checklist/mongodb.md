# MongoDB Checklist

> MongoDB companion to the general [[Database]] checklist.
> Covers MongoDB 8.x (current major) — the standard document database.
> Companion to [[PostgreSQL]] and [[Valkey]] for the other engines in the homelab.
> Last updated: 2026-08-05

---

## 1. Setup & Configuration

- [ ] **Version** — MongoDB 8.x (current major). `db.version()` recorded. Upgrade path known.
- [ ] **Deployment type chosen** — Standalone (dev only), Replica Set (minimum for production — transactions require it), or Sharded Cluster (horizontal scale, medium+ only).
- [ ] **Config file** — `mongod.conf` versioned: `storage.dbPath`, `net.port`, `security.authorization: enabled` (never default-open), `replication.replSetName` if replica set.
- [ ] **`mongod` never runs as root** — Dedicated `mongodb` user, restricted dbPath permissions.
- [ ] **Container/Docker notes** — Volume for `/data/db` (never container-local), auth enabled, healthcheck `mongosh --eval "db.runCommand({ping:1})"` → [[Infrastructure]].

## 2. Schema Design (Document Modeling)

- [ ] **Document model designed, not dumped** — Embed vs reference decided per relationship:
  - **Embed**: one-to-one, one-to-few, read-together data → fewer round trips
  - **Reference**: one-to-many/many-to-many, large or frequently-changing data
- [ ] **Anti-patterns avoided** — Unbounded arrays (grow forever), massive documents (>16MB limit), deep nesting (>100 levels), embedding data that changes independently.
- [ ] **Schema versioning** — Documents carry `schemaVersion` field for migrations. Rolling schema evolution (new fields optional, backfill in batches).
- [ ] **`_id` strategy** — Default ObjectId fine for most. Natural keys or UUIDs for distributed/sync scenarios. Shard key must be immutable and high-cardinality (see §5).
- [ ] **Indexes match query patterns** — Every query shape has an index. `explain("executionStats")` culture like `EXPLAIN ANALYZE` in Postgres → [[Database]] §4.
- [ ] **Data types deliberate** — `ISODate` for dates (not strings), `Decimal128` for money (not double), `NumberLong` for big integers.

## 3. Indexing (MongoDB-Specific)

- [ ] **Single-field indexes** — For equality filters. `db.coll.createIndex({ status: 1 })`.
- [ ] **Compound indexes** — Equality fields first, sort/range last. Leftmost-prefix rule applies (unlike Postgres — Mongo uses ESR: Equality, Sort, Range).
- [ ] **Multikey indexes** — For array fields. One multikey per compound index (limits).
- [ ] **Text indexes** — `{ $text: { $search: ... } }` for full-text. One text index per collection. Not for production search at scale — consider dedicated search.
- [ ] **TTL indexes** — `expireAfterSeconds` for automatic document expiry (sessions, logs, cache). Scheduled deletion without a cron.
- [ ] **Partial / sparse indexes** — `partialFilterExpression` for subsets, `sparse: true` for documents missing the field. Match Postgres partial-index use cases.
- [ ] **Index builds** — `createIndexes` with `background` behavior (builds are non-blocking since 4.2 but still resource-heavy). Rolling builds in sharded clusters.
- [ ] **Index bloat** — `compact` or rebuild indexes on fragmentation. `collStats` `size` vs `storageSize` reviewed.

## 4. Backup & Restore

- [ ] **`mongodump` for logical** — Scheduled for smaller DBs / partial restores. `--gzip --archive` for compressed portable backups.
- [ ] **`mongodump` with oplog** — `--oplog` flag during backup for point-in-time-ish consistency (replica sets).
- [ ] **`mongorestore` tested** — Restore drill quarterly: full restore + validation (`db.collection.validate()`) + app smoke test → [[Database]] §3.
- [ ] **Filesystem snapshot or Atlas/managed backup** — For medium+: consistent snapshots (LVM, EBS) or managed continuous backups.
- [ ] **Backup monitoring** — Backup job failure alerts. "Last successful backup" on dashboards.

## 5. Replication & Sharding

- [ ] **Replica Set for production** — 3 members minimum (primary + 2 secondaries), or 3-member with an arbiter for small deployments. Never standalone in production.
- [ ] **Write concern / read concern** — `writeConcern: majority` for durability of important writes. `readConcern: majority` where stale reads are unacceptable. `readPreference` secondary only for tolerance to lag.
- [ ] **Elections & failover** — Heartbeat config reviewed, election timeout understood. Failover drill: `rs.stepDown()` in staging, verify app reconnects.
- [ ] **Sharding only when needed** — Shard key choice is *permanent*: high-cardinality, immutable, evenly-distributed. Bad shard key = jumbo chunks + hotspots. Start unsharded; shard when write/scale demands (medium+ tier).
- [ ] **Balancer monitored** — Chunk migrations tracked; `balancer` window configured. Migrations on hot chunks cause latency spikes.

## 6. Aggregation & Performance

- [ ] **`$lookup` used sparingly** — It's a left outer join; on unsharded collections can be slow. Consider embedding or application-side joins for hot paths.
- [ ] **Aggregation pipeline stage order** — Filter early (`$match` first), project early, limit early. Never `$unwind` before filtering what you can.
- [ ] **`$group` memory** — `allowDiskUse: true` for large groupings; `100MB` memory limit per stage respected.
- [ ] **Cursor discipline** — Batched `batchSize`, `limit` on all queries, no unbounded `find()` returning millions of docs.
- [ ] **`explain` reviewed** — `executionStats` on slow queries: `totalDocsExamined` vs `nReturned` — high ratio = missing/wrong index.

## 7. Security (MongoDB)

- [ ] **Authorization enabled** — `security.authorization: enabled` + role-based accounts. App uses a role with least privilege (readWrite on its DB, no clusterAdmin) → [[Security]] §8.
- [ ] **Authentication** — SCRAM-SHA-256 (default) or x.509 / LDAP / Kerberos for enterprise. No plaintext credentials.
- [ ] **TLS enforced** — `net.tls.mode: requireTLS`. `ssl=true` in connection strings. Client certs for service accounts at high tiers.
- [ ] **Network isolation** — Bind to private interface, firewall rules, no internet-exposed mongod. `bindIp` explicit.
- [ ] **Encryption at rest** — MongoDB Enterprise/KMS-managed encryption, or volume-level encryption. Backups encrypted too.
- [ ] **Field-level encryption (FLE)** — For regulated PII: client-side field-level encryption with KMS. The server never sees plaintext.
- [ ] **Audit log** — `auditLog` enabled for regulated tiers: authentication events, DDL, DML on sensitive collections.

## 8. Observability (MongoDB)

- [ ] **`db.serverStatus()` metrics** — Opcounters, connections, cache (wiredTiger), page faults, queueing.
- [ ] **Prometheus exporter** — `mongodb_exporter` (percona-mongodb-exporter) or managed: ops/sec, latency, replication lag, cache hit ratio.
- [ ] **Slow query profiling** — `db.setProfilingLevel(1, 100)` or Atlas profiler. Slow queries logged centrally, reviewed weekly.
- [ ] **`currentOp` monitoring** — Long-running operations, lock waits. Alerts on: replication lag > threshold, oplog window shrinking, connections near max.
- [ ] **Capacity trend** — DB size, index size, oplog size trended. Oplog window must cover backup + replica downtime windows.

---

## Quick Sanity Check Before Launch

- [ ] Replica Set configured (3 members), never standalone
- [ ] Authorization enabled, app role least-privilege, no default-open mongod
- [ ] TLS required for connections
- [ ] Every hot query has an index (explain shows low docsExamined/nReturned)
- [ ] Document model reviewed: no unbounded arrays, schemaVersion present
- [ ] Backup scheduled AND restore tested this quarter
- [ ] Write concern majority on important writes
- [ ] Slow query profiling on, metrics exported, alerts armed
- [ ] Shard key (if sharded) immutable, high-cardinality, balanced

---

## Project Tier Scoping Matrix

> **How to use this table:** Pick your tier first, then focus only on the sections marked ✅ (required) or 🟡 (recommended). Skip ❌ sections entirely — they'd be over-engineering for your context.
>
> **Legend:** ✅ Required · 🟡 Recommended / partial · ❌ Skip

### Tier Descriptions

| # | Tier | Description | Typical Team | Users | Lifespan |
|---|---|---|---|---|---|
| 1 | 🧪 **POC / Spike** | Validate an idea. Standalone mongod is fine. | 1 dev | Internal only | Days–weeks |
| 2 | 🔧 **Prototype / MVP** | Waiting for integration or user validation. Might become real. | 1–2 devs | Beta testers | Weeks–months |
| 3 | 🏠 **Internal Tool** | Real users (employees), real data. No external exposure or paying customers. | 1–3 devs | Employees | Ongoing |
| 4 | 🟢 **Small Production** | Single replica set, low traffic. Real users, maybe early revenue. | 1–2 devs | < 1K users | Ongoing |
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
| 1 | Setup & Configuration | 🟡 standalone | ✅ + auth | ✅ | ✅ | ✅ | ✅ + full tuning | ✅ + formal |
| 2 | Schema Design | 🟡 minimal | ✅ | ✅ | ✅ | ✅ + schema versioning | ✅ + review | ✅ + formal |
| 3 | Indexing | ❌ PK only | 🟡 hot queries | ✅ + explain | ✅ + compound | ✅ + partial/TTL | ✅ + bloat mgmt | ✅ + capacity plan |
| 4 | Backup & Restore | ❌ | 🟡 mongodump | ✅ + scheduled | ✅ + restore tested | ✅ + snapshots | ✅ + continuous | ✅ + regulatory |
| 5 | Replication & Sharding | ❌ | ❌ | 🟡 replica set | ✅ + 3 members | ✅ + failover tested | ✅ + shard analysis | ✅ + multi-region |
| 6 | Aggregation & Performance | ❌ | 🟡 basic | ✅ + explain | ✅ + profiling | ✅ + pipeline review | ✅ + capacity trend | ✅ + formal |
| 7 | Security | 🟡 local only | 🟡 auth on | ✅ + least priv | ✅ + TLS | ✅ + FLE if PII | ✅ + audit log | ✅ + regulatory |
| 8 | Observability | ❌ | ❌ | 🟡 basic | ✅ + slow query log | ✅ + dashboards | ✅ + oplog watch | ✅ + full stack |

---

## Sources

- Complements [[Database]] (general), [[Release]] (migration gates), [[Security]] (data protection).
- MongoDB 8 docs — https://www.mongodb.com/docs/manual/
- Schema design patterns — https://www.mongodb.com/docs/manual/data-modeling/
