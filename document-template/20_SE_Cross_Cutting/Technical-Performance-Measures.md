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
| [Response Time] | [NFR-XXX] | [Target value] | [Current value] | [Margin] | [✅/⚠️/❌] |
| [Throughput] | [NFR-XXX] | [Target value] | [Current value] | [Margin] | [✅/⚠️/❌] |
| [Availability] | [NFR-XXX] | [Target value] | [Current value] | [Margin] | [✅/⚠️/❌] |
| [Concurrent Users] | [NFR-XXX] | [Target value] | [Current value] | [Margin] | [✅/⚠️/❌] |
| [Data Volume] | [NFR-XXX] | [Target value] | [Current value] | [Margin] | [✅/⚠️/❌] |
| [Recovery Time] | [NFR-XXX] | [Target value] | [Current value] | [Margin] | [✅/⚠️/❌] |

## 3. TPM Trend

| Month | Response Time | Throughput | Availability | Concurrent Users |
|-------|-------------|-----------|-------------|-----------------|
| [Month 1] | [Value] | [Value] | [Value] | [Value] |
| [Month 2] | [Value] | [Value] | [Value] | [Value] |
| [Month 3] | [Value] | [Value] | [Value] | [Value] |
| [Current] | [Value] | [Value] | [Value] | [Value] |

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
    Response Time: [[x], [y]]
    Throughput: [[x], [y]]
    Availability: [[x], [y]]
    Concurrent Users: [[x], [y]]
    Recovery Time: [[x], [y]]
```

## 5. Shortfall Analysis

| TPM | Shortfall | Impact | Corrective Action | Status |
|-----|----------|--------|------------------|--------|
| [None currently] | — | — | — | — |

## 6. Margin Analysis

| TPM | Margin | Risk if Margin Erodes | Monitoring |
|-----|--------|---------------------|-----------|
| [Response Time] | [Margin] | [User experience degrades] | [Real-time] |
| [Throughput] | [Margin] | [System overload] | [Real-time] |
| [Availability] | [Margin] | [SLA breach] | [Continuous] |
| [Concurrent Users] | [Margin] | [User rejection] | [Load testing] |

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
