---
document_type: Operational KPIs Report
version: "1.0"
status: Active
author: "[DevOps Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [operational-kpis, metrics, swebok]
standard_ref:
  - SWEBOK v4 — Operations
---

# Operational KPIs Report

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Tracks operational health metrics — availability, performance, incidents, and efficiency.

## 2. Operational KPIs

| KPI | Definition | Target | Current | Status |
|-----|-----------|--------|---------|--------|
| [Uptime] | [Successful minutes / Total minutes] | [99.9%] | [X%] | 🟢🟡🔴 |
| [Response Time — p95] | [95th percentile latency] | [< 2s] | [Xs] | 🟢🟡🔴 |
| [Error Rate] | [5xx / Total requests] | [< 1%] | [X%] | 🟢🟡🔴 |
| [Deployment Frequency] | [Deployments per week] | [≥ 1] | [X] | 🟢🟡🔴 |
| [MTTR] | [Mean time to resolution] | [< 1 hour] | [X min] | 🟢🟡🔴 |
| [Change Failure Rate] | [Failed deploys / Total deploys] | [< 5%] | [X%] | 🟢🟡🔴 |
| [Incident Count] | [Incidents per month] | [< 5] | [X] | 🟢🟡🔴 |

## 3. Monthly Report Template

### [Month Year] Operational Report

| Metric | Target | Actual | Trend | Status |
|--------|--------|--------|-------|--------|
| [Uptime] | [99.9%] | [X%] | ↑↓→ | 🟢🟡🔴 |
| [Response Time p95] | [< 2s] | [Xs] | ↑↓→ | 🟢🟡🔴 |
| [Error Rate] | [< 1%] | [X%] | ↑↓→ | 🟢🟡🔴 |
| [Deployments] | [≥ 4] | [X] | ↑↓→ | 🟢🟡🔴 |
| [MTTR] | [< 1 hour] | [X min] | ↑↓→ | 🟢🟡🔴 |
| [Incidents] | [< 5] | [X] | ↑↓→ | 🟢🟡🔴 |
| [SLO Compliance] | [100%] | [X%] | ↑↓→ | 🟢🟡🔴 |

### Incidents This Month

| ID | Severity | Duration | Root Cause | Resolution |
|----|---------|---------|-----------|-----------|
| [INC-001] | [SEV-2] | [45 min] | [Database connection pool] | [Increased pool size] |

### Key Achievements

| # | Achievement |
|---|-----------|
| 1 | [Zero SEV-1 incidents] |
| 2 | [99.95% uptime — exceeding target] |
| 3 | [Reduced MTTR from 90 min to 45 min] |

### Action Items

| # | Action | Owner | Due Date |
|---|--------|-------|---------|
| 1 | [Add database connection monitoring] | [DevOps] | [YYYY-MM-DD] |
| 2 | [Implement auto-scaling for workers] | [DevOps] | [YYYY-MM-DD] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SLO-SLI-Definitions]] | SLO targets |
| [[SLA]] | External commitments |
| [[Monitoring-Dashboard-Spec]] | Dashboard specification |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Review operational KPIs monthly. Trends matter more than snapshots — a single bad day is noise; a worsening trend is a signal.
