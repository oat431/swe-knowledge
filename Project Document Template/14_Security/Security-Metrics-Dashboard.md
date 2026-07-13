---
document_type: Security Metrics Dashboard
version: "1.0"
status: Active
author: "[Security Officer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [security-metrics, dashboard, kpis, cyberok]
standard_ref:
  - CyBOK v1 — Security Governance
---

# Security Metrics Dashboard

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Centralized view of security posture — vulnerability density, remediation time, compliance status, and incident metrics.

## 2. Security KPIs

| KPI | Definition | Target | Current | Status |
|-----|-----------|--------|---------|--------|
| [Vulnerability Density] | [Open vulns / Total assets] | [< 0.1] | [X] | 🟢🟡🔴 |
| [Mean Time to Remediate] | [Avg days to fix critical] | [< 48h] | [X hours] | 🟢🟡🔴 |
| [SLA Compliance] | [Vulns fixed within SLA / Total] | [≥ 95%] | [X%] | 🟢🟡🔴 |
| [Critical Vulns Open] | [Count of open critical] | [0] | [X] | 🟢🟡🔴 |
| [Security Incidents] | [Incidents this month] | [< 3] | [X] | 🟢🟡🔴 |
| [Phishing Click Rate] | [Users who clicked / Tested] | [< 5%] | [X%] | 🟢🟡🔴 |
| [MFA Adoption] | [Users with MFA / Total] | [100%] | [X%] | 🟢🟡🔴 |
| [Compliance Score] | [Controls compliant / Total] | [≥ 95%] | [X%] | 🟢🟡🔴 |

## 3. Dashboard Layout

```
┌─────────────────────────────────────────────────────────────┐
│  SECURITY DASHBOARD                             [Last 30d ▼]│
├──────────┬──────────┬──────────┬──────────────────────────┤
│ Vuln     │ MTTR     │ SLA      │ Critical                 │
│ Density  │          │ Compliance│ Vulns                    │
│ 0.05     │ 36h      │ 97%      │ 0                        │
├──────────┴──────────┴──────────┴──────────────────────────┤
│  [Vulnerability Trends — Line]    [Open by Severity — Bar] │
├─────────────────────────────────────────────────────────────┤
│  [Remediation SLA — Gauge]        [Incident Trend — Line]  │
├──────────────────────────────┬──────────────────────────────┤
│  [Top Vulnerability Sources]  │  [Compliance Scorecard]     │
└──────────────────────────────┴──────────────────────────────┘
```

## 4. Vulnerability Trends

| Month | Critical | High | Medium | Low | Total | MTTR |
|-------|---------|------|--------|-----|-------|------|
| [Month 1] | [2] | [5] | [10] | [15] | [32] | [72h] |
| [Month 2] | [1] | [3] | [8] | [12] | [24] | [60h] |
| [Month 3] | [0] | [2] | [5] | [8] | [15] | [48h] |
| [Month 4] | [0] | [1] | [4] | [6] | [11] | [36h] |
| **Current** | **[0]** | **[1]** | **[3]** | **[5]** | **[9]** | **[36h]** |

## 5. Incident Summary

| Month | SEV-1 | SEV-2 | SEV-3 | SEV-4 | Total | MTTR |
|-------|-------|-------|-------|-------|-------|------|
| [Month 1] | [0] | [1] | [2] | [3] | [6] | [4h] |
| [Month 2] | [0] | [0] | [2] | [2] | [4] | [3h] |
| [Month 3] | [0] | [1] | [1] | [2] | [4] | [2h] |
| [Month 4] | [0] | [0] | [1] | [2] | [3] | [2h] |

## 6. Compliance Scorecard

| Framework | Controls | Compliant | Score | Status |
|---------|---------|----------|-------|--------|
| [ISO 27001] | [12] | [12] | [100%] | ✅ |
| [OWASP Top 10] | [10] | [10] | [100%] | ✅ |
| [GDPR] | [7] | [7] | [100%] | ✅ |

## 7. Alerts

| Alert | Condition | Status | Action |
|-------|----------|--------|--------|
| [Critical vuln open > 48h] | [MTTR > 48h] | 🟢 OK | None |
| [SLA compliance < 95%] | [SLA < 95%] | 🟢 OK | None |
| [Security incident] | [SEV-1 or SEV-2] | 🟢 OK | None |
| [MFA adoption < 100%] | [MFA < 100%] | 🟢 OK | None |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Vulnerability-Management-Report]] | Vulnerability details |
| [[Compliance-Assessment-Report]] | Compliance details |
| [[Incident-Response-Plan]] | Incident handling |

---

> **Template Standard:** Based on CyBOK v1
> **Usage:** Review security metrics weekly. Trends matter more than snapshots. If critical vulns > 0, it's a priority 1 issue.
