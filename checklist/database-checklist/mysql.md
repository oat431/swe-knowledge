# MySQL Checklist

> Covers MySQL 8.4 LTS / 9.x and MariaDB (current majors), the most common open-source relational engine for web and retail systems.
> Companion to [[database]] (general contract) and [[postgresql]] (relational sibling).
> Last updated: 2026-11-15

---

## 1. Engine & Distribution Choice

- [ ] **MySQL vs MariaDB decided deliberately:** The fork divergence is real — replication stacks (Group Replication / InnoDB Cluster vs Galera),
  optimizer behavior, and SQL surface drift apart every release; moving between them later is a migration project, not a command. Record why you picked one → [[database]] §1.
- [ ] **Release track chosen (MySQL):** LTS series for production (long premier support; behavior stable within a series)
  vs Innovation releases (features first, behavior churn expected). LTS for anything with real users.
- [ ] **InnoDB everywhere:** The default engine and the only sensible one for transactional tables; legacy engines (MyISAM-class)
  only with a documented reason (no transactions, no crash-safety) → [[database]] §1.
- [ ] **Routing layer picked early:** ProxySQL (query rules, pooling, read/write split) or MySQL Router (InnoDB Cluster companion)
  between app and DB — not "we'll add one when we need it."
- [ ] **Edition and licensing reviewed:** MySQL (Oracle; GPL/commercial dual) vs MariaDB (community fork, own licensing).
  Audit, thread pool, and encryption features differ per edition — know what you actually get.
- [ ] **Connector compatibility tested:** Your app driver against your exact fork/version before deploy (auth handshake changes bite here — see §9).

## 2. Schema & Storage Model

- [ ] **Clustered primary key kept narrow:** InnoDB stores rows in PK order and every secondary index ends with the PK values —
  a wide PK (UUID string, composite) bloats *every* index and slows *every* lookup. Prefer `BIGINT` or `BINARY(16)` UUIDs → [[database]] §1.
- [ ] **utf8mb4 end-to-end:** Server, schema, tables, columns, and connection (`SET NAMES utf8mb4`) all agree;
  the legacy 3-byte `utf8` silently mangles emoji and supplementary characters.
- [ ] **Collation chosen deliberately:** `_ci`/`_ai` collations fold case and accents — UNIQUE constraints and duplicate detection change meaning.
  Thai text sorting/comparison needs explicit collation review with real Thai strings → [[database]] §1.
- [ ] **`ROW_FORMAT=DYNAMIC`:** The modern InnoDB default (long VARCHAR/TEXT stored off-page efficiently);
  REDUNDANT/COMPACT only for legacy imports — convert them, don't propagate them.
- [ ] **Case sensitivity known:** Table names are case-sensitive on Linux (`lower_case_table_names` defaults to case-sensitive there,
  insensitive on Windows/macOS) and the setting is fixed at initialization — standardize on lowercase names across all environments.
- [ ] **Generated columns and expression indexes:** Derived data via `GENERATED ALWAYS AS (...) STORED/VIRTUAL`;
  expression access indexed via functional indexes (MySQL) or indexed generated columns (MariaDB — confirm which mechanism your fork supports).
- [ ] **Partitioning for very large tables:** RANGE partitioning by time with verified partition pruning when a table outgrows single-node comfort;
  retention via `DROP PARTITION`/`REORGANIZE` instead of mass DELETEs → [[database]] §4.
- [ ] **`sql_mode` asserted, not assumed:** Confirm strict mode is active (`STRICT_TRANS_TABLES`, `ONLY_FULL_GROUP_BY`, `NO_ZERO_DATE`)
  so truncation and invalid aggregates become errors, not silent corruption → [[database]] §10.

## 3. Indexing & Queries

- [ ] **`EXPLAIN` on every slow query:** `EXPLAIN` / `EXPLAIN ANALYZE` (`FORMAT=TREE`/`JSON` for detail).
  Flag full scans on large tables, temp-table-to-disk, and rows-estimated-vs-actual blowups → [[database]] §4.
- [ ] **Implicit type conversions hunted:** Comparing a `VARCHAR` column to a number (`WHERE zip = 10110`)
  or joining columns with mismatched charsets/collations silently disables the index — match types exactly.
- [ ] **Covering indexes for hot paths:** Remember the clustered-PK quirk — an index on `(status, created_at)` already carries the PK;
  add only truly needed columns to avoid row lookups → [[database]] §4.
- [ ] **Leftmost-prefix rule respected:** Composite `(a, b, c)` serves `a`, `a,b`, `a,b,c` — not `b, c`.
  Order: equality columns first, range/sort last.
- [ ] **Expression access indexed:** `WHERE LOWER(email) = ...` or JSON extraction in predicates needs a functional/generated-column index;
  a plain B-tree on the raw column will not be used.
- [ ] **Selectivity checked before adding indexes:** Low-cardinality columns (status, booleans) rarely justify standalone indexes —
  verify with `EXPLAIN`, since every index costs writes and buffer pool space.
- [ ] **Hints as last resort:** `FORCE INDEX` / `STRAIGHT_JOIN` only after `ANALYZE TABLE` and index fixes fail,
  with a comment saying why. Hints rot as data distributions change.

## 4. Transactions & Locking

- [ ] **REPEATABLE READ implications understood:** InnoDB's default isolation (vs Postgres's READ COMMITTED default)
  fixes the snapshot at the transaction's first read and locks differently on UPDATE/DELETE —
  retry and consistency logic must be written for *this* behavior → [[postgresql]].
- [ ] **Gap/next-key locks respected:** Under REPEATABLE READ, secondary-index updates and range UPDATE/DELETE lock gaps
  and block *inserts* into the range; deadlock spikes on hot secondary indexes are usually this.
- [ ] **Lock wait and deadlock handling:** `innodb_lock_wait_timeout` set sanely (the default is far too long for web requests),
  deadlock victim retry in app code; `SHOW ENGINE INNODB STATUS` for the deadlock trace.
- [ ] **Hot-row contention designed around:** One counter/last-ID row serializes all writers regardless of cores —
  batch, shard, or queue writes to hot rows → [[database]] §5.
- [ ] **Short transactions, no external I/O:** Never call an API or wait on a user inside a transaction;
  long transactions pile up undo history and stall purge.

## 5. Replication & HA

- [ ] **Async primary-replica with GTID:** Row-based binlog (`binlog_format=ROW`) and GTID auto-positioning
  for robust replica bootstrap and failover; statement-based replication only with documented exceptions → [[database]] §7.
- [ ] **Replication lag treated as staleness:** Async replicas are stale by design — read-your-writes must route to the primary
  (or wait for the write's GTID to apply), never to a lagging replica.
- [ ] **Semi-sync where loss matters:** Semi-synchronous replication (replica ACK before commit) shrinks the data-loss window
  at the cost of commit latency — decide per service, not globally.
- [ ] **HA stack matches the fork:** MySQL: Group Replication / InnoDB Cluster + MySQL Router.
  MariaDB: Galera (multi-primary) or primary-replica with external failover. Don't mix recipes across forks.
- [ ] **DDL applied once, on the primary:** Schema changes replicate through the binlog — running DDL per node
  (or with tools blind to replication) manufactures schema drift. Verify replicas after every migration.
- [ ] **Failover tooling + drill:** Orchestrator-class external failover or the cluster's built-in election;
  kill the primary in staging, verify promotion and app reconnect → [[database]] §7.
- [ ] **Vitess when writes outgrow one primary:** The YouTube-proven sharding path (VTGate routing, online resharding).
  Sharding bolted on late is a rewrite.
- [ ] **Replica drift checked:** Checksum-class tooling (pt-table-checksum) on a schedule —
  replicas silently diverge after failovers and errant writes.

## 6. Backup & PITR

- [ ] **`mysqldump` for logical backups:** Smaller DBs and schema copies; `--single-transaction` gives a consistent InnoDB snapshot
  without locking, but it is not the tool for large production volumes.
- [ ] **Physical hot backups:** Percona XtraBackup (MySQL) or `mariabackup` (MariaDB) — copy data plus redo/binlog while writes continue.
- [ ] **Binlog-based point-in-time recovery:** Base backup + retained binlogs = restore to any moment;
  binlog retention sized to the recovery window and its disk growth watched.
- [ ] **Restore actually tested:** Quarterly drill: full restore + replay to a chosen point in time + app smoke test.
  The restore procedure is the deliverable, not the backup file → [[database]] §3.
- [ ] **Replica snapshots positioned:** Backing up from a replica is fine only if the snapshot records its binlog/GTID position —
  otherwise it cannot participate in PITR.
- [ ] **Backups encrypted, retained, off-site:** Per policy, with restore verification → [[database]] §3.

## 7. Schema Changes & Migrations

- [ ] **Online DDL limits understood:** Many `ALTER TABLE` operations rebuild the table or block writes for the duration —
  check whether the specific operation supports `ALGORITHM=INSTANT`/`INPLACE` before running anything on a big table.
- [ ] **Shadow-table tools for big tables:** pt-online-schema-change or gh-ost (trigger-free, throttled, pausable)
  to alter/rebuild large tables under controlled load.
- [ ] **Expand-contract migrations:** Add column → deploy → backfill → switch code → drop old column in a later release.
  Never one-shot destructive DDL → [[database]] §2.
- [ ] **Backfills batched and idempotent:** Small batches with progress tracking, safe to re-run after a crash;
  a multi-hour single UPDATE holds locks and dies un-restartably → [[database]] §2.
- [ ] **Migration framework exercised in CI:** Flyway/Liquibase-class versioned migrations applied from scratch in CI,
  and timed against a production-sized copy so the real DDL duration is known → [[qa]].
- [ ] **Locking DDL budgeted:** Every migration reviewed for lock scope and expected duration;
  anything beyond the agreed budget goes through the shadow-table path or a maintenance window.

## 8. Performance

- [ ] **Buffer pool sized:** `innodb_buffer_pool_size` is *the* knob (roughly 50–70% of RAM on a dedicated DB host;
  leave room for connections and temp tables on shared hosts). Tune against measured hit rate, not vibes.
- [ ] **Durability/flush trade-off documented:** `innodb_flush_log_at_trx_commit=1` (default: durable per commit)
  vs `2` (faster, can lose roughly a second on OS crash) — if changed, write down what loss was accepted.
- [ ] **Temp/derived tables watched:** Complex joins, derived tables, and UNIONs materialize into temp tables;
  when they exceed `tmp_table_size`/`max_heap_table_size` they land on disk — the `Created_tmp_disk_tables` counter is the tell.
- [ ] **Slow query log armed:** `long_query_time` set (e.g. 100ms) plus `log_queries_not_using_indexes` for new-code review;
  shipped centrally and actually read → [[database]] §12.
- [ ] **`performance_schema` + `sys` schema used:** Built-in statement digests, wait analysis,
  and unused/missing index reports (sys schema views). The poor person's APM.
- [ ] **Connections pooled, `max_connections` a ceiling not a target:** Thread-per-connection means every idle connection
  costs memory and OS threads — ProxySQL (or the driver pool) in front, alerts at 80% of the ceiling → [[database]] §5.
- [ ] **Thread handling confirmed per distribution:** MariaDB ships a thread pool (`thread_pool_size`);
  on MySQL the thread pool is not part of the community server. Confirm what your build actually provides before tuning for it.

## 9. Security

- [ ] **Grants least-privilege, roles used:** App user owns only its tables' DML; no application with admin/superuser rights;
  reporting on read-only accounts → [[security]].
- [ ] **Auth plugins current:** `caching_sha2_password` is the default; the legacy `mysql_native_password` is deprecated
  and no longer enabled by default in current MySQL (re-enabling is a migration bridge, not a config choice).
  Old clients that only speak the legacy plugin will break — verify drivers first.
- [ ] **TLS enforced:** `require_secure_transport=ON` (or per-user `REQUIRE SSL`), TLS 1.2+ as the floor;
  internal networks are not trusted networks → [[database]] §8.
- [ ] **Encryption at rest:** InnoDB tablespace encryption (keyring plugin) where sensitive;
  managed offerings encrypt by default. Backups encrypted too → [[database]] §8.
- [ ] **Audit trail where regulated:** Your edition's audit plugin (MariaDB Audit Plugin, MySQL Enterprise Audit,
  or a community alternative) shipping to central logs → [[database]] §8.
- [ ] **Credentials out of config files:** DSNs from env/vault, rotated, never committed in `my.cnf` or scripts → [[02 Secrets Management]].

## 10. Managed Options & Scale

- [ ] **Managed option evaluated:** RDS/Aurora MySQL, Cloud SQL, or a PlanetScale-class Vitess service trade control
  for operational labor — usually right below "Production Grade" team size.
- [ ] **Know what you lose:** Restricted superuser/variable privileges, limited binlog and topology control,
  no arbitrary plugins/engines, vendor-specific SQL behavior. Design inside the cage instead of fighting it.
- [ ] **Semantics differ from vanilla:** Aurora-class storage changes durability and replication mechanics
  (redo shipping, quorum commits) — re-derive RPO/RTO from the managed engine's docs, don't inherit self-hosted assumptions.
- [ ] **Migration path mapped both ways:** Self-hosted ↔ managed (dump-and-reload, replication-based, or vendor tooling)
  planned before traffic makes it urgent → [[database]] §7.

## 11. Monitoring

- [ ] **Replication lag is a first-class alert:** Seconds-behind *plus* a heartbeat/GTID-position check
  (the built-in counter can read zero on an idle, broken replica).
- [ ] **Buffer pool health:** Hit rate, free buffers, and disk-read counters —
  a falling hit rate is the leading indicator of the slow-spiral-before-outage.
- [ ] **Locks and deadlocks tracked:** Deadlock rate and long lock waits from `SHOW ENGINE INNODB STATUS`
  and performance_schema lock tables; alert on waits, not just errors.
- [ ] **Connection saturation watched:** `Threads_connected` vs `max_connections`, `Threads_running` spikes —
  pool exhaustion announces itself only after users feel it.
- [ ] **Disk and binlog growth trended:** Data dir, binlog, temp/undo growth forecast quarterly;
  disk-full is the most common MySQL outage and it is entirely predictable → [[database]] §12.

---

## Quick Sanity Check Before Release

- [ ] Fork + release track recorded (`SELECT VERSION()`), every deviation from defaults documented
- [ ] `sql_mode` strict (`STRICT_TRANS_TABLES`, `ONLY_FULL_GROUP_BY`) confirmed on the server
- [ ] Hot queries `EXPLAIN`ed: no full scans, no implicit type conversions eating indexes
- [ ] Backup + binlog retention running AND a restore-to-point-in-time actually tested
- [ ] Replication lag alerted; read-your-writes routing verified against a lagging replica
- [ ] App user least-privilege, TLS enforced, no clients propped up on the legacy auth plugin
- [ ] Buffer pool sized, slow query log shipping, big ALTERs routed through gh-ost/pt-osc

---

## Sources

- Master checklist: [[database]].
- MySQL Reference Manual: https://dev.mysql.com/doc/
- MySQL Releases: Innovation and LTS: https://dev.mysql.com/doc/refman/8.4/en/mysql-releases.html
- MariaDB Server Knowledge Base: https://mariadb.com/kb/en/
- Percona XtraBackup: https://docs.percona.com/percona-xtrabackup/ · gh-ost: https://github.com/github/gh-ost
- ProxySQL: https://proxysql.com/ · Vitess: https://vitess.io/
