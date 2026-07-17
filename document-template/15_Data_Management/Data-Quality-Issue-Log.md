---
document_type: Data Quality Issue Log
version: "1.0"
status: Active
author: "[Data Steward]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [data-quality, issue-log, dmbok]
standard_ref:
  - DMBOK v2 — Data Quality
---

# Data Quality Issue Log

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Tracks data quality issues from discovery to resolution — ensuring nothing falls through the cracks.

## 2. Issue Register

| ID | Issue | Entity | Field | Count | Severity | Root Cause | Status |
|----|-------|--------|-------|-------|---------|-----------|--------|
| [DQI-001] | [Email format invalid] | [Customer] | [email] | [50] | 🟡 Medium | [No validation at input] | ✅ Resolved |
| [DQI-002] | [Phone format inconsistent] | [Customer] | [phone] | [200] | 🟡 Medium | [No E.164 validation] | ✅ Resolved |
| [DQI-003] | [Duplicate customers] | [Customer] | [email] | [100] | 🟡 Medium | [No dedup on registration] | ✅ Resolved |
| [DQI-004] | [Amount precision] | [Request] | [amount] | [500] | 🟢 Low | [No rounding rule] | ✅ Resolved |
| [DQI-005] | [Null phone numbers] | [Customer] | [phone] | [200] | 🟢 Low | [Optional field] | ✅ Accepted |

## 3. Issue Template

| Field | Value |
|-------|-------|
| **Issue ID** | [DQI-XXX] |
| **Date Discovered** | [YYYY-MM-DD] |
| **Discovered By** | [Name] |
| **Entity** | [Customer / Request / Transaction] |
| **Field** | [email / phone / amount / ...] |
| **Issue Description** | [What's wrong] |
| **Affected Records** | [Count] |
| **Severity** | 🟢 Low / 🟡 Medium / 🟠 High / 🔴 Critical |
| **Root Cause** | [Why it happened] |
| **Remediation** | [How to fix] |
| **Owner** | [Who fixes it] |
| **Due Date** | [YYYY-MM-DD] |
| **Status** | [Open / In Progress / Resolved / Accepted] |

## 4. Issue Metrics

| Metric | Value | Target |
|--------|-------|--------|
| [Open issues] | [0] | [0] |
| [Avg resolution time] | [3 days] | [< 5 days] |
| [Issues per month] | [1] | [< 5] |
| [Critical issues] | [0] | [0] |

## 5. Issue Trends

| Month | Opened | Resolved | Accepted | Net |
|-------|--------|---------|---------|-----|
| [Month 1] | [5] | [3] | [1] | [+1] |
| [Month 2] | [3] | [4] | [0] | [-1] |
| [Month 3] | [1] | [2] | [0] | [-1] |
| [Current] | [0] | [0] | [0] | [0] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Data-Quality-Scorecard]] | Quality metrics |
| [[Data-Cleansing-Specification]] | Cleansing rules |
| [[Data-Quality-Strategy]] | Quality framework |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Every quality issue gets logged. No exceptions. Track to resolution. Learn from patterns.
