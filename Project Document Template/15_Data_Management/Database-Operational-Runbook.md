---
document_type: Database Operational Runbook
version: "1.0"
status: Draft
author: "[DBA]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [database-runbook, operations, dmbok]
standard_ref:
  - DMBOK v2 — Database Operations
---

# Database Operational Runbook

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Operational procedures for database management — daily tasks, maintenance, troubleshooting, and emergency procedures.

## 2. Database Information

| Field | Detail |
|-------|--------|
| [RDBMS] | [PostgreSQL 16] |
| [Host] | [db.project.internal] |
| [Port] | [5432] |
| [Database] | [project_db] |
| [Schema] | [public] |
| [Connection Pool] | [PgBouncer, max 100] |

## 3. Daily Operations

| Task | Frequency | Command | Owner |
|------|----------|---------|-------|
| [Check connections] | [Daily] | `SELECT count(*) FROM pg_stat_activity;` | [DBA] |
| [Check replication lag] | [Daily] | `SELECT lag FROM pg_stat_replication;` | [DBA] |
| [Check long queries] | [Daily] | `SELECT * FROM pg_stat_activity WHERE state = 'active' AND query_start < now() - interval '5 minutes';` | [DBA] |
| [Check disk usage] | [Daily] | `SELECT pg_database_size(current_database());` | [DBA] |
| [Check deadlocks] | [Daily] | `SELECT * FROM pg_stat_activity WHERE wait_event_type = 'Lock';` | [DBA] |

## 4. Maintenance Tasks

| Task | Frequency | Command | Purpose |
|------|----------|---------|---------|
| [VACUUM ANALYZE] | [Weekly] | `VACUUM ANALYZE;` | [Reclaim space, update statistics] |
| [REINDEX] | [Monthly] | `REINDEX DATABASE project_db;` | [Rebuild indexes] |
| [Update statistics] | [Weekly] | `ANALYZE;` | [Query planner optimization] |
| [Check table bloat] | [Monthly] | [Check pg_stat_user_tables] | [Identify bloated tables] |

## 5. Performance Monitoring

| Metric | Query | Threshold | Alert |
|--------|-------|----------|-------|
| [Active connections] | `SELECT count(*) FROM pg_stat_activity;` | [< 80] | [PagerDuty] |
| [Cache hit ratio] | `SELECT sum(blks_hit)/sum(blks_hit+blks_read) FROM pg_stat_database;` | [> 99%] | [Slack] |
| [Transaction throughput] | `SELECT xact_commit + xact_rollback FROM pg_stat_database;` | [Baseline] | [Dashboard] |
| [Dead tuples] | `SELECT n_dead_tup FROM pg_stat_user_tables;` | [< 10000] | [Slack] |

## 6. Troubleshooting

### Slow Queries

| Step | Action | Command |
|------|--------|---------|
| 1 | [Identify slow queries] | `SELECT pid, query, query_start FROM pg_stat_activity WHERE state = 'active' AND query_start < now() - interval '5 minutes';` |
| 2 | [Check query plan] | `EXPLAIN ANALYZE <query>;` |
| 3 | [Kill if necessary] | `SELECT pg_terminate_backend(<pid>);` |
| 4 | [Optimize query] | [Add indexes, rewrite query] |

### Connection Issues

| Step | Action | Command |
|------|--------|---------|
| 1 | [Check connection count] | `SELECT count(*) FROM pg_stat_activity;` |
| 2 | [Check connection limits] | `SHOW max_connections;` |
| 3 | [Check PgBouncer] | `SHOW POOLS;` (via pgbouncer) |
| 4 | [Restart if necessary] | `systemctl restart postgresql` |

### Disk Space

| Step | Action | Command |
|------|--------|---------|
| 1 | [Check database size] | `SELECT pg_database_size(current_database());` |
| 2 | [Check table sizes] | `SELECT pg_size_pretty(pg_total_relation_size(relid)) FROM pg_stat_user_tables ORDER BY pg_total_relation_size(relid) DESC LIMIT 10;` |
| 3 | [VACUUM FULL if needed] | `VACUUM FULL <table>;` |
| 4 | [Archive old data] | [Move to archive tables] |

## 7. Emergency Contacts

| Role | Name | Phone | Email |
|------|------|-------|-------|
| [Primary DBA] | [Name] | [Phone] | [Email] |
| [Backup DBA] | [Name] | [Phone] | [Email] |
| [Cloud Provider Support] | — | [Phone] | [Email] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Backup-Recovery-Plan]] | Backup procedures |
| [[High-Availability-DR-Configuration]] | HA/DR setup |
| [[Operations-Manual-Runbook]] | General operations |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** The runbook is the *DBA's bible*. When something breaks, open the runbook. Don't improvise under pressure.
