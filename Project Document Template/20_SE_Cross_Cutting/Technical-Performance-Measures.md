---
document_type: Technical Performance Measures (TPMs)
version: "1.0"
status: Active
author: "[Systems Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [tpm, technical-performance, measures, sebok]
standard_ref:
  - SEBoK v2 — Technical Management
---

# Technical Performance Measures (TPMs)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Tracks technical performance against requirements — identifying shortfalls early and enabling corrective action.

## 2. TPM Summary

| TPM | Requirement | Target | Current | Margin | Status |
|-----|-----------|--------|---------|--------|--------|
| [Response Time] | [NFR-001] | [< 2 seconds] | [1.2 seconds] | [+40%] | ✅ |
| [Throughput] | [NFR-002] | [100 req/sec] | [150 req/sec] | [+50%] | ✅ |
| [Availability] | [NFR-003] | [99.9%] | [99.95%] | [+0.05%] | ✅ |
| [Concurrent Users] | [NFR-004] | [200] | [250] | [+25%] | ✅ |
| [Data Volume] | [NFR-005] | [1M records] | [1.5M records] | [+50%] | ✅ |
| [Recovery Time] | [NFR-006] | [< 1 hour] | [30 minutes] | [+50%] | ✅ |

## 3. TPM Trend

| Month | Response Time | Throughput | Availability | Concurrent Users |
|-------|-------------|-----------|-------------|-----------------|
| [Month 1] | [2.5s] | [80/sec] | [99.5%] | [100] |
| [Month 2] | [1.8s] | [120/sec] | [99.8%] | [150] |
| [Month 3] | [1.5s] | [140/sec] | [99.9%] | [200] |
| [Current] | [1.2s] | [150/sec] | [99.95%] | [250] |

## 4. TPM Analysis

```mermaid
quadrantChart
    title TPM Status
    x-axis "Low Margin" --> "High Margin"
    y-axis "Low Priority" --> "High Priority"
    quadrant-1 "Monitor Closely"
    quadrant-2 "Maintain"
    quadrant-3 "Accept Risk"
    quadrant-4 "Celebrate"
    Response Time: [0.8, 0.9]
    Throughput: [0.85, 0.7]
    Availability: [0.9, 1.0]
    Concurrent Users: [0.75, 0.6]
    Recovery Time: [0.85, 0.8]
```

## 5. Shortfall Analysis

| TPM | Shortfall | Impact | Corrective Action | Status |
|-----|----------|--------|------------------|--------|
| [None currently] | — | — | — | — |

## 6. Margin Analysis

| TPM | Margin | Risk if Margin Erodes | Monitoring |
|-----|--------|---------------------|-----------|
| [Response Time] | [+40%] | [User experience degrades] | [Real-time] |
| [Throughput] | [+50%] | [System overload] | [Real-time] |
| [Availability] | [+0.05%] | [SLA breach] | [Continuous] |
| [Concurrent Users] | [+25%] | [User rejection] | [Load testing] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SEMP]] | SE management context |
| [[Nonfunctional-Requirements-Catalog]] | Requirements source |
| [[Performance-Test-Report]] | Performance testing |

---

> **Template Standard:** Based on SEBoK v2
> **Usage:** TPMs track *technical health*. If margins erode, act before they become shortfalls.
