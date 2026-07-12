---
document_type: SLA Compliance Report
version: "1.0"
status: Active
author: "[DevOps Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [sla-compliance, uptime, performance, swebok]
standard_ref:
  - SWEBOK v4 — Maintenance
---

# SLA Compliance Report

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Tracks SLA compliance — uptime, performance, and support response times against commitments.

## 2. Monthly SLA Report — [Month Year]

### Uptime

| Metric | SLA Target | Actual | Status |
|--------|-----------|--------|--------|
| [Uptime] | [99.9%] | [X%] | ✅❌ |
| [Allowed downtime] | [43.8 min] | [X min] | ✅❌ |
| [Actual downtime] | — | [X min] | — |
| [Downtime incidents] | — | [X] | — |

### Performance

| Metric | SLA Target | Actual | Status |
|--------|-----------|--------|--------|
| [Response time — p95] | [< 2s] | [Xs] | ✅❌ |
| [Error rate] | [< 1%] | [X%] | ✅❌ |
| [Throughput] | [≥ 100 req/s] | [X req/s] | ✅❌ |

### Support Response

| Severity | SLA Response | Actual Response | SLA Resolution | Actual Resolution | Status |
|---------|-------------|----------------|---------------|------------------|--------|
| 🔴 Critical | [< 15 min] | [X min] | [< 4 hours] | [X hours] | ✅❌ |
| 🟡 High | [< 1 hour] | [X hours] | [< 1 day] | [X days] | ✅❌ |
| 🟢 Medium | [< 4 hours] | [X hours] | [< 3 days] | [X days] | ✅❌ |

## 3. SLA Trend

| Month | Uptime | Response p95 | Error Rate | Support Response | Overall |
|-------|--------|-------------|-----------|-----------------|---------|
| [Month 1] | [99.95%] | [1.8s] | [0.3%] | [100%] | ✅ |
| [Month 2] | [99.92%] | [1.9s] | [0.4%] | [98%] | ✅ |
| [Month 3] | [99.85%] | [2.1s] | [0.6%] | [95%] | ❌ |
| [Month 4] | [99.98%] | [1.7s] | [0.2%] | [100%] | ✅ |
| **Current** | **[X%]** | **[Xs]** | **[X%]** | **[X%]** | **✅❌** |

## 4. SLA Breaches

| Date | Duration | Severity | Root Cause | Resolution |
|------|---------|---------|-----------|-----------|
| [YYYY-MM-DD] | [X min] | [Uptime] | [Database failover] | [Auto-recovered] |
| [YYYY-MM-DD] | [X hours] | [Support] | [After-hours incident] | [On-call rotation added] |

## 5. Error Budget

| SLO | Budget | Used | Remaining | Status |
|-----|--------|------|----------|--------|
| [99.9% uptime] | [43.8 min] | [X min] | [X min] | 🟢🟡🔴 |
| [< 1% error rate] | [~4,320] | [X] | [X] | 🟢🟡🔴 |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SLA]] | SLA definitions |
| [[SLO-SLI-Definitions]] | SLO targets |
| [[Operational-KPIs-Report]] | Operational metrics |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** SLA compliance is the *trust metric*. Report monthly. Address breaches immediately.
