---
document_type: Backup & Recovery Plan
version: "1.0"
status: Draft
author: "[DBA]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [backup, recovery, disaster-recovery, dmbok]
standard_ref:
  - DMBOK v2 — Database Operations
---

# Backup & Recovery Plan

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines backup strategy, retention, and recovery procedures — ensuring data can be restored after loss or corruption.

## 2. Backup Strategy

| Backup Type | Frequency | Retention | Method | Storage |
|------------|----------|----------|--------|---------|
| [Full backup] | [Daily] | [30 days] | [pg_dump] | [S3 cross-region] |
| [WAL archiving] | [Continuous] | [7 days] | [archive_command] | [S3] |
| [Logical backup] | [Weekly] | [90 days] | [pg_dumpall] | [S3] |
| [Config backup] | [On change] | [Indefinite] | [Git] | [GitHub] |

## 3. RPO/RTO Targets

| Metric | Target | Current |
|--------|--------|---------|
| [RPO (Recovery Point Objective)] | [< 5 minutes] | [Continuous WAL] |
| [RTO (Recovery Time Objective)] | [< 1 hour] | [~30 minutes] |
| [Backup verification] | [Monthly] | [Automated] |

## 4. Backup Procedures

### 4.1 Full Backup (Daily)

```bash
#!/bin/bash
# Daily full backup
BACKUP_DATE=$(date +%Y%m%d)
pg_dump -Fc -Z9 project_db > /backups/project_db_${BACKUP_DATE}.dump
aws s3 cp /backups/project_db_${BACKUP_DATE}.dump s3://backups-bucket/daily/
# Cleanup old backups (keep 30 days)
aws s3 ls s3://backups-bucket/daily/ | while read -r line; do
    createDate=$(echo $line | awk '{print $1" "$2}')
    createDate=$(date -d "$createDate" +%s)
    olderThan=$(date -d "30 days ago" +%s)
    if [[ $createDate -lt $olderThan ]]; then
        fileName=$(echo $line | awk '{print $4}')
        aws s3 rm s3://backups-bucket/daily/$fileName
    fi
done
```

### 4.2 WAL Archiving (Continuous)

```bash
# postgresql.conf
archive_mode = on
archive_command = 'aws s3 cp %p s3://backups-bucket/wal/%f'
```

## 5. Recovery Procedures

### 5.1 Point-in-Time Recovery (PITR)

| Step | Action | Command | Duration |
|------|--------|---------|---------|
| 1 | [Stop database] | `systemctl stop postgresql` | [1 min] |
| 2 | [Restore base backup] | `pg_restore -Fc backup.dump` | [15 min] |
| 3 | [Configure recovery] | `recovery_target_time = 'YYYY-MM-DD HH:MM:SS'` | [1 min] |
| 4 | [Start recovery] | `systemctl start postgresql` | [5 min] |
| 5 | [Verify data] | `SELECT count(*) FROM requests;` | [2 min] |
| 6 | [Promote to primary] | `SELECT pg_promote();` | [1 min] |

### 5.2 Full Restore

| Step | Action | Command | Duration |
|------|--------|---------|---------|
| 1 | [Create new database] | `createdb project_db_restored` | [1 min] |
| 2 | [Restore from backup] | `pg_restore -d project_db_restored backup.dump` | [15 min] |
| 3 | [Verify integrity] | [Run consistency checks] | [10 min] |
| 4 | [Switch application] | [Update connection strings] | [5 min] |

## 6. Backup Verification

| Test | Frequency | Method | Success Criteria |
|------|----------|--------|-----------------|
| [Restore test] | [Monthly] | [Restore to test env] | [Data integrity verified] |
| [PITR test] | [Quarterly] | [Restore to specific time] | [Data at target time correct] |
| [Full DR test] | [Annually] | [Complete recovery] | [RTO met] |

## 7. Backup Monitoring

| Metric | Threshold | Alert |
|--------|----------|-------|
| [Backup age] | [< 24 hours] | [PagerDuty] |
| [Backup size] | [± 20% of avg] | [Slack] |
| [WAL lag] | [< 5 minutes] | [PagerDuty] |
| [Backup success] | [100%] | [PagerDuty] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Database-Operational-Runbook]] | Operational procedures |
| [[High-Availability-DR-Configuration]] | HA/DR setup |
| [[Disaster-Recovery-Plan]] | Overall DR plan |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Backups are *useless* if you've never tested recovery. Test monthly. Document everything.
