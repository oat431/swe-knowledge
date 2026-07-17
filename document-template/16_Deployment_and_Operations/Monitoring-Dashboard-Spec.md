---
document_type: Monitoring Dashboard Spec
version: "1.0"
status: Draft
author: "[DevOps Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [monitoring, dashboard, grafana, prometheus, swebok]
standard_ref:
  - SWEBOK v4 — Operations
---

# Monitoring Dashboard Specification

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines monitoring dashboards — what to monitor, how to visualize, and when to alert.

## 2. Monitoring Stack

| Component | Tool | Purpose |
|---------|------|---------|
| [Metrics] | [Prometheus] | [Time-series metrics collection] |
| [Visualization] | [Grafana] | [Dashboard and alerting] |
| [Logging] | [ELK Stack] | [Centralized log management] |
| [Tracing] | [Jaeger] | [Distributed tracing] |
| [Uptime] | [Pingdom] | [External uptime monitoring] |

## 3. Dashboard: Application Overview

```
┌─────────────────────────────────────────────────────────────┐
│  APPLICATION OVERVIEW                           [Last 1h ▼] │
├──────────┬──────────┬──────────┬──────────────────────────┤
│ Requests │ Errors   │ Latency  │ Uptime                   │
│ /sec     │ Rate     │ p95      │                          │
│   45     │  0.3%    │  1.8s    │  99.95%                  │
├──────────┴──────────┴──────────┴──────────────────────────┤
│  [Request Rate — Time Series]    [Error Rate — Time Series]│
├─────────────────────────────────────────────────────────────┤
│  [Response Time — Percentiles (p50/p95/p99)]              │
├──────────────────────────────┬──────────────────────────────┤
│  [Status Code Distribution]  │  [Top Endpoints by Latency] │
└──────────────────────────────┴──────────────────────────────┘
```

## 4. Dashboard: Infrastructure

```
┌─────────────────────────────────────────────────────────────┐
│  INFRASTRUCTURE                                 [Last 1h ▼] │
├──────────┬──────────┬──────────┬──────────────────────────┤
│ CPU      │ Memory   │ Disk     │ Network                  │
│ 45%      │ 55%      │ 35%      │ 120 Mbps                 │
├──────────┴──────────┴──────────┴──────────────────────────┤
│  [CPU Utilization — Per Pod]    [Memory Utilization — Per Pod]│
├─────────────────────────────────────────────────────────────┤
│  [Disk I/O]                     [Network I/O]              │
├──────────────────────────────┬──────────────────────────────┤
│  [Pod Status]                │  [Container Restarts]        │
└──────────────────────────────┴──────────────────────────────┘
```

## 5. Dashboard: Database

```
┌─────────────────────────────────────────────────────────────┐
│  DATABASE                                       [Last 1h ▼] │
├──────────┬──────────┬──────────┬──────────────────────────┤
│ Conn     │ Queries  │ Cache    │ Replication              │
│ Pool     │ /sec     │ Hit Rate │ Lag                      │
│ 25/100   │  120     │  95%     │  0.5s                    │
├──────────┴──────────┴──────────┴──────────────────────────┤
│  [Connection Pool Usage]        [Query Duration — p95]     │
├─────────────────────────────────────────────────────────────┤
│  [Slow Queries — Top 10]       [Table Sizes]              │
└─────────────────────────────────────────────────────────────┘
```

## 6. Alert Configuration

| Alert | Condition | Severity | Channel | Action |
|-------|----------|---------|--------|--------|
| [High Error Rate] | [> 5% for 5 min] | 🔴 | [PagerDuty] | [Investigate immediately] |
| [High Latency] | [p95 > 5s for 5 min] | 🟡 | [Slack] | [Investigate] |
| [Pod Restarting] | [> 3 restarts in 10 min] | 🟡 | [Slack] | [Check logs] |
| [Database Connections] | [> 80% pool] | 🟡 | [Slack] | [Scale connections] |
| [Disk Usage] | [> 80%] | 🟡 | [Slack] | [Expand storage] |
| [SLO Breach Risk] | [Error budget < 25%] | 🟡 | [Slack] | [Reduce risk] |
| [Uptime < 99.9%] | [Monthly calculation] | 🔴 | [Email] | [Incident review] |

## 7. Grafana Dashboard Links

| Dashboard | URL | Audience |
|----------|-----|---------|
| [Application Overview] | [grafana.project.com/d/app] | [Dev + Ops] |
| [Infrastructure] | [grafana.project.com/d/infra] | [Ops] |
| [Database] | [grafana.project.com/d/db] | [DBA + Ops] |
| [SLO Dashboard] | [grafana.project.com/d/slo] | [Engineering] |
| [Business Metrics] | [grafana.project.com/d/biz] | [PM + Management] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Operational-KPIs-Report]] | KPI definitions |
| [[SLO-SLI-Definitions]] | SLO targets |
| [[SLA]] | External commitments |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Dashboards should answer "Is the system healthy?" in 5 seconds. If you need to dig, the drill-down is one click away.
