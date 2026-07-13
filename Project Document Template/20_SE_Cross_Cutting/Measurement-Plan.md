---
document_type: Measurement Plan
version: "1.0"
status: Draft
author: "[Systems Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [measurement, metrics, sebok]
standard_ref:
  - SEBoK v2 — Systems Engineering Management
  - ISO/IEC/IEEE 15939 — Measurement Process
---

# Measurement Plan

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines what to measure, how to measure, and how to use measurements — enabling data-driven decision-making.

## 2. Measurement Objectives

| # | Objective | Questions | Measures |
|---|----------|----------|---------|
| 1 | [Track project progress] | [Are we on schedule?] | [Schedule variance, milestone completion] |
| 2 | [Monitor quality] | [Is quality improving?] | [Defect density, test coverage] |
| 3 | [Control costs] | [Are we on budget?] | [Cost variance, earned value] |
| 4 | [Manage risk] | [Are risks being mitigated?] | [Risk count, risk velocity] |
| 5 | [Ensure performance] | [Does system meet targets?] | [TPMs, SLA compliance] |

## 3. Measures

### Process Measures

| Measure | Definition | Formula | Target | Frequency | Owner |
|---------|-----------|---------|--------|----------|-------|
| [Schedule Variance] | [Planned vs actual] | [(Planned - Actual) / Planned] | [± 10%] | [Weekly] | [PM] |
| [Cost Variance] | [Budget vs actual] | [(Budget - Actual) / Budget] | [± 10%] | [Monthly] | [PM] |
| [Defect Density] | [Defects per KLOC] | [Defects / KLOC] | [< 5] | [Per release] | [QA] |
| [Test Coverage] | [Code coverage] | [Covered / Total] | [≥ 80%] | [Per build] | [QA] |
| [Risk Velocity] | [Risk mitigation rate] | [Mitigated / Total] | [≥ 3/month] | [Monthly] | [SE] |

### Product Measures

| Measure | Definition | Formula | Target | Frequency | Owner |
|---------|-----------|---------|--------|----------|-------|
| [Response Time] | [API latency] | [p95 latency] | [< 2s] | [Real-time] | [DevOps] |
| [Availability] | [Uptime] | [Uptime / Total time] | [≥ 99.9%] | [Continuous] | [DevOps] |
| [Throughput] | [Requests per second] | [req/sec] | [≥ 100] | [Real-time] | [DevOps] |
| [Error Rate] | [Failed requests] | [Errors / Total] | [< 1%] | [Real-time] | [DevOps] |

## 4. Measurement Collection

| Measure | Source | Method | Automation |
|---------|--------|--------|-----------|
| [Schedule Variance] | [Project management tool] | [Calculated] | ✅ |
| [Cost Variance] | [Financial system] | [Calculated] | ✅ |
| [Defect Density] | [Issue tracker] | [Counted] | ✅ |
| [Test Coverage] | [CI/CD pipeline] | [Tool-generated] | ✅ |
| [Response Time] | [Monitoring] | [Measured] | ✅ |

## 5. Measurement Reporting

| Report | Audience | Frequency | Content |
|--------|---------|----------|---------|
| [Project Dashboard] | [PM, Management] | [Weekly] | [All process measures] |
| [Quality Report] | [QA, Dev] | [Per release] | [Quality measures] |
| [Performance Report] | [DevOps, SE] | [Daily] | [Product measures] |
| [Executive Summary] | [Management] | [Monthly] | [Key metrics + trends] |

## 6. Measurement Analysis

| Measure | Trend | Analysis | Action |
|---------|-------|---------|--------|
| [Schedule Variance] | [Within ±10%] | [On track] | [Continue] |
| [Defect Density] | [Decreasing] | [Quality improving] | [Continue] |
| [Response Time] | [Stable] | [Performance acceptable] | [Monitor] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SEMP]] | SE management context |
| [[Technical-Performance-Measures]] | Technical measures |
| [[Quality-Metrics-Dashboard]] | Quality metrics |

---

> **Template Standard:** Based on SEBoK v2, ISO/IEC/IEEE 15939
> **Usage:** Measure what matters. Don't measure everything — measure what helps you make decisions.
