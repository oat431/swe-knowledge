---
document_type: Capacity Plan (Infrastructure)
version: "1.0"
status: Draft
author: "[DevOps Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [capacity-planning, infrastructure, scaling, swebok]
standard_ref:
  - SWEBOK v4 — Operations
---

# Capacity Plan (Infrastructure)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Plans infrastructure capacity — ensuring the system can handle current and projected load without performance degradation.

## 2. Current Capacity

| Resource | Current | Utilization | Threshold | Status |
|---------|---------|-----------|----------|--------|
| [CPU] | [4 vCPU] | [45%] | [80%] | 🟢 |
| [Memory] | [8 GB] | [55%] | [80%] | 🟢 |
| [Storage] | [500 GB] | [35%] | [80%] | 🟢 |
| [Database] | [4 vCPU, 16 GB] | [40%] | [80%] | 🟢 |
| [Network] | [1 Gbps] | [20%] | [80%] | 🟢 |

## 3. Growth Projections

| Period | Users | Requests/Day | Storage | CPU | Memory |
|--------|-------|-------------|---------|-----|--------|
| [Current] | [1,000] | [500] | [175 GB] | [45%] | [55%] |
| [6 months] | [5,000] | [2,500] | [350 GB] | [65%] | [70%] |
| [12 months] | [10,000] | [5,000] | [500 GB] | [80%] | [80%] |
| [24 months] | [25,000] | [12,500] | [1 TB] | [Scale needed] | [Scale needed] |

## 4. Scaling Plan

| Resource | Current | 6-Month | 12-Month | 24-Month |
|---------|---------|---------|---------|---------|
| [App Servers] | [2 pods] | [4 pods] | [6 pods] | [10 pods] |
| [Database] | [4 vCPU, 16 GB] | [4 vCPU, 16 GB] | [8 vCPU, 32 GB] | [Read replicas] |
| [Cache] | [2 GB] | [4 GB] | [8 GB] | [Cluster mode] |
| [Storage] | [500 GB] | [1 TB] | [2 TB] | [5 TB] |

## 5. Cost Projections

| Period | Monthly Cost | Annual Cost | Change |
|--------|-------------|------------|--------|
| [Current] | [$X] | [$Y] | — |
| [6 months] | [$X] | [$Y] | [+Z%] |
| [12 months] | [$X] | [$Y] | [+Z%] |
| [24 months] | [$X] | [$Y] | [+Z%] |

## 6. Auto-Scaling Configuration

| Resource | Min | Max | Trigger | Cooldown |
|---------|-----|-----|---------|---------|
| [App Pods] | [2] | [10] | [CPU > 70%] | [300s] |
| [Workers] | [2] | [6] | [Queue > 100] | [300s] |

## 7. Capacity Alerts

| Alert | Threshold | Action |
|-------|----------|--------|
| [CPU > 70%] | [Warning] | [Investigate] |
| [CPU > 85%] | [Critical] | [Scale up] |
| [Memory > 80%] | [Warning] | [Investigate] |
| [Storage > 80%] | [Warning] | [Expand storage] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Operational-KPIs-Report]] | Utilization metrics |
| [[Infrastructure-as-Code]] | Infrastructure definition |
| [[Container-Configurations]] | Container specs |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Plan capacity *before* you need it. Last-minute scaling is expensive and risky.
