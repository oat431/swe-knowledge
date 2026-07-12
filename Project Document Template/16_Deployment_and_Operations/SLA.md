---
document_type: SLA (Service-Level Agreement)
version: "1.0"
status: Draft
author: "[DevOps Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [sla, service-level, uptime, swebok, sebok]
standard_ref:
  - SWEBOK v4 — Operations
  - SEBoK v2 — Operations
---

# SLA (Service-Level Agreement)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines the service levels — uptime, performance, support response times — that the team commits to.

## 2. SLA Summary

| Metric | Target | Measurement | Penalty |
|--------|--------|-----------|---------|
| [Uptime] | [99.9%] | [Monthly] | [Credit] |
| [Response Time — p95] | [< 2s] | [Monthly] | [Review] |
| [Error Rate] | [< 1%] | [Monthly] | [Review] |
| [Support Response — Critical] | [< 1 hour] | [Per incident] | [Escalation] |
| [Support Response — High] | [< 4 hours] | [Per incident] | [Review] |
| [Support Resolution — Critical] | [< 4 hours] | [Per incident] | [Escalation] |
| [Support Resolution — High] | [< 1 day] | [Per incident] | [Review] |

## 3. Uptime Calculation

```
Uptime = (Total Minutes - Downtime Minutes) / Total Minutes × 100

Monthly Uptime Target: 99.9%
Allowed Downtime: 43.8 minutes/month
```

| Uptime % | Monthly Downtime | Annual Downtime |
|---------|-----------------|----------------|
| [99.9%] | [43.8 min] | [8.77 hours] |
| [99.95%] | [21.9 min] | [4.38 hours] |
| [99.99%] | [4.38 min] | [52.6 min] |

## 4. Exclusions

| Exclusion | Rationale |
|----------|----------|
| [Scheduled maintenance] | [Planned, communicated in advance] |
| [Force majeure] | [Natural disasters, wars] |
| [Third-party outages] | [Cloud provider issues] |
| [Customer-caused issues] | [Misconfiguration, abuse] |

## 5. Support Response Times

| Severity | Response Time | Resolution Time | Business Hours |
|---------|-------------|----------------|---------------|
| 🔴 Critical | [1 hour] | [4 hours] | [24/7] |
| 🟡 High | [4 hours] | [1 day] | [Business hours] |
| 🟢 Medium | [1 day] | [3 days] | [Business hours] |
| ⚪ Low | [3 days] | [Next sprint] | [Business hours] |

## 6. Escalation Path

| Level | Contact | When |
|-------|---------|------|
| [L1 — Support] | [Support team] | [Initial response] |
| [L2 — Engineering] | [On-call engineer] | [After 1 hour] |
| [L3 — Management] | [Engineering manager] | [After 4 hours] |
| [L4 — Executive] | [CTO] | [After 8 hours] |

## 7. SLA Reporting

| Report | Frequency | Audience | Content |
|--------|----------|---------|---------|
| [SLA Dashboard] | [Real-time] | [Team] | [Current metrics] |
| [SLA Report] | [Monthly] | [Management] | [Monthly performance] |
| [Incident Report] | [Per incident] | [Stakeholders] | [Incident details] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SLO-SLI-Definitions]] | Detailed SLO/SLI |
| [[Operations-Manual-Runbook]] | Operational procedures |
| [[Incident-Management-Process]] | Incident handling |

---

> **Template Standard:** Based on SWEBOK v4, SEBoK v2
> **Usage:** The SLA is the *commitment*. Don't promise what you can't deliver. Measure continuously, report monthly.
