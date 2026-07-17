---
document_type: Data Interface Agreement (DIA)
version: "1.0"
status: Draft
author: "[Data Architect]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [data-interface, integration, dmbok]
standard_ref:
  - DMBOK v2 — Data Integration
---

# Data Interface Agreement (DIA)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Formal agreement between systems for data exchange — defining format, frequency, responsibility, and SLAs.

## 2. Interface Register

| # | Interface | Provider | Consumer | Direction | Status |
|---|----------|---------|---------|----------|--------|
| 1 | [Customer Data] | [Application] | [Data Warehouse] | [Outbound] | ✅ Active |
| 2 | [Request Data] | [Application] | [Data Warehouse] | [Outbound] | ✅ Active |
| 3 | [ERP Sync] | [ERP] | [Application] | [Inbound] | ✅ Active |
| 4 | [Payment Data] | [Payment Gateway] | [Application] | [Inbound] | ✅ Active |

## 3. Interface Agreement Template

### DIA-[XXX]: [Interface Name]

| Field | Detail |
|-------|--------|
| **DIA ID** | [DIA-001] |
| **Interface Name** | [Customer Data Sync] |
| **Provider** | [Application Database] |
| **Consumer** | [Data Warehouse] |
| **Direction** | [Outbound] |
| **Data** | [Customer records] |
| **Format** | [JSON / CSV / Parquet] |
| **Frequency** | [Nightly at 02:00] |
| **Volume** | [~10,000 records/day] |
| **Protocol** | [S3 + dbt] |
| **SLA** | [Data available by 03:00] |
| **Owner** | [Data Architect] |
| **Status** | [Active] |

### Data Elements

| Element | Type | Required | Description |
|---------|------|---------|-----------|
| [customer_id] | [UUID] | [Yes] | [Unique identifier] |
| [name] | [VARCHAR] | [Yes] | [Customer name] |
| [email] | [VARCHAR] | [Yes] | [Email address] |
| [type] | [VARCHAR] | [Yes] | [Customer type] |
| [created_at] | [TIMESTAMP] | [Yes] | [Creation time] |
| [updated_at] | [TIMESTAMP] | [Yes] | [Last update] |

### Responsibilities

| Responsibility | Provider | Consumer |
|---------------|---------|---------|
| [Data quality] | [✅] | [Verify] |
| [Format compliance] | [✅] | [Validate] |
| [SLA adherence] | [✅] | [Monitor] |
| [Error handling] | [Alert] | [Retry] |
| [Schema changes] | [Notify 7d] | [Adapt] |

### Change Management

| Change Type | Notification | Lead Time | Approval |
|------------|-------------|----------|---------|
| [Schema change] | [Written notice] | [7 days] | [Both parties] |
| [Frequency change] | [Written notice] | [3 days] | [Both parties] |
| [Format change] | [Written notice] | [14 days] | [Both parties] |
| [Volume change] | [Written notice] | [3 days] | [Provider] |

## 4. Interface Monitoring

| Metric | Threshold | Alert |
|--------|----------|-------|
| [Data freshness] | [< 24 hours] | [PagerDuty] |
| [Record count] | [± 20% of avg] | [Slack] |
| [Error rate] | [< 1%] | [Slack] |
| [SLA compliance] | [100%] | [Slack if < 100%] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Data-Integration-Architecture]] | Architecture context |
| [[API-Data-Contract]] | API contracts |
| [[ETL-ELT-Specification]] | Pipeline implementation |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** DIAs are *contracts*. When something breaks, the DIA defines who's responsible. Document everything.
