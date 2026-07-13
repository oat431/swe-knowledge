---
document_type: Risk Burn-Down Chart
version: "1.0"
status: Active
author: "[Systems Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [risk-burndown, risk-management, sebok]
standard_ref:
  - SEBoK v2 — Risk Management
---

# Risk Burn-Down Chart

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Tracks risk reduction over time — visualizing progress from identified risks to mitigated risks.

## 2. Risk Burn-Down Summary

| Metric | Value |
|--------|-------|
| [Total risks identified] | [15] |
| [Risks mitigated] | [12] |
| [Risks open] | [3] |
| [Risk reduction rate] | [80%] |

## 3. Risk Burn-Down Chart

```mermaid
xychart-beta
    title "Risk Burn-Down"
    x-axis [Month 1, Month 2, Month 3, Month 4, Current]
    y-axis "Open Risks" 0 --> 15
    line [15, 12, 8, 5, 3]
```

## 4. Risk Trend

| Month | Opened | Mitigated | Accepted | Net Open |
|-------|--------|---------|---------|---------|
| [Month 1] | [15] | [3] | [0] | [12] |
| [Month 2] | [2] | [4] | [0] | [10] |
| [Month 3] | [1] | [4] | [1] | [6] |
| [Month 4] | [0] | [3] | [0] | [3] |
| [Current] | [0] | [0] | [0] | [3] |

## 5. Remaining Open Risks

| ID | Risk | Severity | Mitigation | Owner | Status |
|----|------|---------|-----------|-------|--------|
| [R-013] | [Third-party API reliability] | 🟡 Medium | [Fallback mechanism] | [Dev Team] | 🔄 In Progress |
| [R-014] | [Data migration complexity] | 🟡 Medium | [Phased migration] | [Data Architect] | 🔄 In Progress |
| [R-015] | [User adoption] | 🟡 Medium | [Training program] | [Change Manager] | 🔄 In Progress |

## 6. Risk Velocity

| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| [Avg risks mitigated/month] | [3] | [≥ 3] | ✅ |
| [Avg time to mitigate] | [30 days] | [< 45 days] | ✅ |
| [Risk backlog] | [3] | [< 5] | ✅ |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Risk-Register]] | Risk details |
| [[Risk-Management-Plan]] | Risk management approach |
| [[Technical-Performance-Measures]] | Technical metrics |

---

> **Template Standard:** Based on SEBoK v2
> **Usage:** The burn-down chart shows *progress*. If the line isn't going down, your risk mitigation isn't working.
