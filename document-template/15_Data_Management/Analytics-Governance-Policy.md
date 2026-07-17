---
document_type: Analytics Governance Policy
version: "1.0"
status: Draft
author: "[Data Governance Officer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [analytics, governance, bi, dmbok]
standard_ref:
  - DMBOK v2 — Business Intelligence
---

# Analytics Governance Policy

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines governance for analytics and BI — ensuring reports are accurate, consistent, and trustworthy.

## 2. Analytics Principles

| # | Principle | Description |
|---|----------|-------------|
| 1 | [Single source of truth] | [All reports use the data warehouse] |
| 2 | [Certified metrics] | [Metrics defined and approved by data stewards] |
| 3 | [Access control] | [Reports access based on role and classification] |
| 4 | [Audit trail] | [All report access and changes logged] |
| 5 | [Data quality] | [Reports only use quality-verified data] |

## 3. Metric Governance

| Metric | Definition | Owner | Approval | Status |
|--------|-----------|-------|---------|--------|
| [Request Count] | [Total requests submitted] | [Operations Steward] | [DGO] | ✅ Certified |
| [Total Amount] | [Sum of request amounts] | [Financial Steward] | [DGO] | ✅ Certified |
| [SLA Compliance] | [Requests within SLA / Total] | [Operations Steward] | [DGO] | ✅ Certified |
| [Customer Count] | [Distinct customers] | [Customer Steward] | [DGO] | ✅ Certified |

## 4. Report Governance

| Rule | Description | Enforcement |
|------|-----------|-----------|
| [Use certified metrics] | [Reports must use approved metric definitions] | [Review gate] |
| [Use DW as source] | [Reports must query DW, not operational DB] | [Technical enforcement] |
| [Document data sources] | [Reports must document which tables/views used] | [Documentation requirement] |
| [Access control] | [Reports classified by data sensitivity] | [RBAC] |
| [Version control] | [Report definitions version controlled] | [Git] |

## 5. Analytics Access Control

| Report Type | Access | Approval | Classification |
|------------|--------|---------|---------------|
| [Executive reports] | [Management] | [CFO/COO] | 🔴 L1 |
| [Financial reports] | [Finance team] | [CFO] | 🔴 L1 |
| [Operational reports] | [Operations team] | [Ops Manager] | 🟡 L2 |
| [Customer analytics] | [Marketing team] | [Marketing Manager] | 🟡 L2 |
| [Ad-hoc queries] | [Data team] | [Data Architect] | 🟡 L2 |

## 6. Self-Service Analytics

| Capability | Who | Governance | Guardrails |
|-----------|-----|-----------|-----------|
| [View dashboards] | [All authorized users] | [RBAC] | [View only] |
| [Create reports] | [Data team, trained users] | [Review required] | [Certified metrics only] |
| [Ad-hoc queries] | [Data team] | [Query logging] | [No PII exposure] |
| [Export data] | [Data team, managers] | [Approval required] | [Classification-based] |

## 7. Analytics Quality

| Check | Frequency | Method | Action on Fail |
|-------|----------|--------|---------------|
| [Metric accuracy] | [Monthly] | [Cross-check with source] | [Alert + investigate] |
| [Report freshness] | [Daily] | [Check refresh status] | [Alert if stale] |
| [Data consistency] | [Weekly] | [Compare reports] | [Alert if inconsistent] |
| [Access review] | [Quarterly] | [Review access logs] | [Adjust permissions] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[BI-Semantic-Layer-Definition]] | Semantic layer |
| [[Report-Dashboard-Catalog]] | Report catalog |
| [[Data-Governance-Charter]] | Governance framework |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Analytics without governance is *dangerous*. Bad metrics lead to bad decisions. Certify everything.
