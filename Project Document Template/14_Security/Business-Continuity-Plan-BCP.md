---
document_type: Business Continuity Plan (BCP)
version: "1.0"
status: Draft
author: "[Security Officer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Confidential"
tags: [bcp, business-continuity, resilience, cyberok, iso-22301]
standard_ref:
  - CyBOK v1 — Resilience
  - ISO 22301 — Business Continuity
---

# Business Continuity Plan (BCP)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Strategies for maintaining critical functions during and after a disruption — ensuring business survives adverse events.

## 2. Business Impact Analysis (BIA)

| Function | Criticality | RTO | RPO | Impact if Down |
|---------|-----------|-----|-----|---------------|
| [Request Processing] | 🔴 Critical | [1 hour] | [0] | [Revenue loss, SLA breach] |
| [Authentication] | 🔴 Critical | [15 min] | [0] | [All users locked out] |
| [Notifications] | 🟡 High | [4 hours] | [1 hour] | [Delayed communications] |
| [Reporting] | 🟢 Medium | [24 hours] | [4 hours] | [Delayed insights] |
| [File Storage] | 🟡 High | [4 hours] | [0] | [Document access lost] |

## 3. Continuity Strategies

| Threat | Strategy | Implementation | RTO |
|--------|---------|---------------|-----|
| [Server failure] | [Auto-failover] | [Multi-AZ, health checks] | [5 min] |
| [Database failure] | [Auto-failover] | [Multi-AZ replica] | [1 min] |
| [Region outage] | [Cross-region failover] | [DR region, DNS failover] | [4 hours] |
| [Data corruption] | [Point-in-time recovery] | [Continuous backups] | [1 hour] |
| [Ransomware] | [Restore from backup] | [Air-gapped backups] | [4 hours] |
| [DDoS attack] | [Traffic filtering] | [CDN + WAF + rate limiting] | [15 min] |

## 4. Recovery Procedures

### 4.1 Database Recovery

| Step | Action | Command | Duration |
|------|--------|---------|---------|
| 1 | [Assess damage] | [Check replication status] | [5 min] |
| 2 | [Promote replica] | [Automatic or manual failover] | [1 min] |
| 3 | [Verify data integrity] | [Run consistency checks] | [15 min] |
| 4 | [Resume operations] | [Update connection strings] | [5 min] |

### 4.2 Application Recovery

| Step | Action | Command | Duration |
|------|--------|---------|---------|
| 1 | [Deploy to DR region] | [CI/CD to DR] | [30 min] |
| 2 | [Update DNS] | [Failover DNS records] | [15 min] |
| 3 | [Verify functionality] | [Smoke tests] | [15 min] |
| 4 | [Notify stakeholders] | [Communication] | [5 min] |

## 5. Backup Strategy

| Data | Method | Frequency | Retention | Location |
|------|--------|----------|----------|---------|
| [Database] | [pg_dump + WAL] | [Daily full + continuous] | [30 days] | [Cross-region S3] |
| [Files] | [S3 replication] | [Real-time] | [Indefinite] | [Cross-region] |
| [Config] | [Git] | [Every change] | [Indefinite] | [GitHub] |
| [Secrets] | [Vault backup] | [Daily] | [30 days] | [Secure storage] |

## 6. Communication Plan

| Phase | Audience | Channel | Message |
|-------|---------|---------|---------|
| [Detection] | [Incident team] | [Slack #security] | [Incident detected] |
| [Containment] | [Management] | [Email + Phone] | [Impact assessment] |
| [Recovery] | [All stakeholders] | [Email + Status page] | [Recovery timeline] |
| [Resolution] | [All stakeholders] | [Email + Status page] | [Service restored] |

## 7. Testing Schedule

| Test Type | Frequency | Last Test | Next Test | Result |
|----------|----------|----------|----------|--------|
| [Backup restore] | [Monthly] | [YYYY-MM-DD] | [YYYY-MM-DD] | ✅ Pass |
| [Failover test] | [Quarterly] | [YYYY-MM-DD] | [YYYY-MM-DD] | ✅ Pass |
| [Full DR drill] | [Annually] | [YYYY-MM-DD] | [YYYY-MM-DD] | ✅ Pass |
| [Tabletop exercise] | [Quarterly] | [YYYY-MM-DD] | [YYYY-MM-DD] | ✅ Pass |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Disaster-Recovery-Plan]] | Technical DR procedures |
| [[Incident-Response-Plan]] | Security incident handling |
| [[SLA]] | Uptime commitments |

---

> **Template Standard:** Based on CyBOK v1, ISO 22301
> **Usage:** BCP is about *business survival*. Test quarterly. Document everything. When disaster strikes, follow the plan.
