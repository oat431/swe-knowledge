---
document_type: Data Quality Scorecard
version: "1.0"
status: Active
author: "[Data Steward]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [data-quality, scorecard, metrics, dmbok]
standard_ref:
  - DMBOK v2 — Data Quality
---

# Data Quality Scorecard

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Aggregated view of data quality metrics — tracking quality trends and identifying areas for improvement.

## 2. Overall Quality Score

| Metric | Score | Target | Status |
|--------|-------|--------|--------|
| [Overall DQ Score] | [99.1%] | [≥ 95%] | ✅ |
| [Rules Passing] | [18/18] | [100%] | ✅ |
| [Issues Open] | [0] | [0] | ✅ |
| [Issues Resolved (30d)] | [3] | — | ✅ |

## 3. Quality by Dimension

| Dimension | Score | Target | Trend | Status |
|----------|-------|--------|-------|--------|
| [Accuracy] | [99.5%] | [≥ 99%] | → | ✅ |
| [Completeness] | [97%] | [≥ 95%] | ↑ | ✅ |
| [Consistency] | [99%] | [≥ 99%] | → | ✅ |
| [Timeliness] | [99%] | [≥ 99%] | → | ✅ |
| [Uniqueness] | [99.8%] | [≥ 99%] | → | ✅ |
| [Validity] | [99.5%] | [≥ 99%] | → | ✅ |

## 4. Quality by Entity

| Entity | Accuracy | Completeness | Consistency | Timeliness | Uniqueness | Validity | Overall |
|--------|---------|-------------|------------|-----------|-----------|---------|--------|
| [Customer] | [99.5%] | [98%] | [99%] | [99%] | [99.8%] | [99.5%] | [99.1%] |
| [Request] | [99%] | [97%] | [99%] | [99%] | [100%] | [99%] | [98.8%] |
| [Transaction] | [99.9%] | [99%] | [99.5%] | [99%] | [100%] | [99.9%] | [99.5%] |

## 5. Quality Trends

| Month | Accuracy | Completeness | Consistency | Timeliness | Uniqueness | Validity | Overall |
|-------|---------|-------------|------------|-----------|-----------|---------|--------|
| [Month 1] | [98%] | [95%] | [98%] | [98%] | [99%] | [98%] | [97.7%] |
| [Month 2] | [99%] | [96%] | [99%] | [99%] | [99.5%] | [99%] | [98.6%] |
| [Month 3] | [99.5%] | [97%] | [99%] | [99%] | [99.8%] | [99.5%] | [99.0%] |
| [Current] | [99.5%] | [97%] | [99%] | [99%] | [99.8%] | [99.5%] | [99.1%] |

## 6. Quality Issues

| Status | Count | Trend |
|--------|-------|-------|
| [Open] | [0] | ↓ |
| [In Progress] | [0] | → |
| [Resolved (30d)] | [3] | — |
| [Total (all time)] | [15] | — |

## 7. Quality Alerts

| Alert | Condition | Status |
|-------|----------|--------|
| [DQ score < 95%] | [Any dimension below 95%] | 🟢 OK |
| [New issues] | [Any new quality issue] | 🟢 OK |
| [Rule failures] | [Any rule failing] | 🟢 OK |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Data-Quality-Strategy]] | Quality framework |
| [[Data-Quality-Rules]] | Quality rules |
| [[Data-Profiling-Report]] | Profiling results |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** The scorecard is the *health check*. Review weekly. If any dimension drops below 95%, escalate.
