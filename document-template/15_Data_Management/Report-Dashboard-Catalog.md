---
document_type: Report & Dashboard Catalog
version: "1.0"
status: Active
author: "[Data Architect]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [reports, dashboards, analytics, dmbok]
standard_ref:
  - DMBOK v2 — Business Intelligence
---

# Report & Dashboard Catalog

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Catalogs all reports and dashboards — what exists, who uses it, how often it refreshes.

## 2. Report Catalog

| # | Report | Audience | Data Source | Refresh | Owner | Status |
|---|--------|---------|-----------|--------|-------|--------|
| 1 | [Executive Summary] | [Management] | [fact_requests] | [Daily] | [Data Architect] | ✅ Active |
| 2 | [Request Volume Report] | [Operations] | [fact_requests] | [Hourly] | [Operations Steward] | ✅ Active |
| 3 | [Financial Summary] | [Finance] | [fact_transactions] | [Daily] | [Financial Steward] | ✅ Active |
| 4 | [Customer Analytics] | [Marketing] | [fact_requests, dim_customer] | [Weekly] | [Customer Steward] | ✅ Active |
| 5 | [SLA Compliance] | [Operations] | [fact_requests] | [Daily] | [Operations Steward] | ✅ Active |
| 6 | [Staff Performance] | [HR] | [fact_requests, dim_staff] | [Weekly] | [HR Steward] | ✅ Active |

## 3. Dashboard Catalog

| # | Dashboard | Audience | Metrics | Refresh | Owner | Status |
|---|----------|---------|---------|--------|-------|--------|
| 1 | [Operations Dashboard] | [Operations] | [Request count, SLA, backlog] | [Real-time] | [Operations Steward] | ✅ Active |
| 2 | [Financial Dashboard] | [Finance] | [Revenue, costs, trends] | [Daily] | [Financial Steward] | ✅ Active |
| 3 | [Customer Dashboard] | [Marketing] | [Customers, segments, retention] | [Daily] | [Customer Steward] | ✅ Active |
| 4 | [Executive Dashboard] | [Management] | [KPIs, trends, forecasts] | [Daily] | [Data Architect] | ✅ Active |

## 4. Report Details

### Executive Summary

| Field | Detail |
|-------|--------|
| [Description] | [High-level summary of key metrics] |
| [Audience] | [Management, Executives] |
| [Metrics] | [Request count, amount, SLA, customer count] |
| [Dimensions] | [Date, Category, Status] |
| [Filters] | [Date range, Category] |
| [Refresh] | [Daily at 06:00] |
| [Format] | [PDF + Web] |
| [Access] | [Management role] |

### Operations Dashboard

| Field | Detail |
|-------|--------|
| [Description] | [Real-time operational metrics] |
| [Audience] | [Operations team] |
| [Metrics] | [Open requests, SLA compliance, staff workload] |
| [Dimensions] | [Date, Staff, Category, Status] |
| [Filters] | [Date range, Staff, Category] |
| [Refresh] | [Real-time (5 min)] |
| [Format] | [Web dashboard] |
| [Access] | [Operations role] |

## 5. Access Control

| Report/Dashboard | Access Level | Approval |
|-----------------|-------------|---------|
| [Executive Dashboard] | [Management] | [CFO/COO] |
| [Financial Reports] | [Finance] | [CFO] |
| [Operations Reports] | [Operations] | [Operations Manager] |
| [Customer Analytics] | [Marketing] | [Marketing Manager] |

## 6. Report Metrics

| Metric | Value |
|--------|-------|
| [Total reports] | [6] |
| [Total dashboards] | [4] |
| [Active users] | [X] |
| [Avg report load time] | [< 5 sec] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[BI-Semantic-Layer-Definition]] | Semantic layer |
| [[Data-Warehouse-Architecture]] | DW architecture |
| [[Analytics-Governance-Policy]] | Analytics governance |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** If a report exists but nobody uses it, kill it. Catalog everything. Review usage quarterly.
