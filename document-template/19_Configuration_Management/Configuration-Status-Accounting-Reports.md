---
document_type: Configuration Status Accounting Reports
version: "1.0"
status: Active
author: "[Configuration Manager]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [configuration-status, status-accounting, swebok, sebok]
standard_ref:
  - SWEBOK v4 — Configuration Management
  - SEBoK v2 — Configuration Management
---

# Configuration Status Accounting Reports

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Provides visibility into the status of all configuration items — what exists, what version, what changed, and what state it's in.

## 2. CI Status Summary

| CI Category | Total | Baselined | In Change | Current | Status |
|------------|-------|----------|----------|---------|--------|
| [Source Code] | [X files] | [v1.0.0] | [0] | [v1.2.0] | ✅ |
| [Documents] | [X files] | [v1.0] | [1] | [v1.1] | 🟡 |
| [Infrastructure] | [X files] | [v1.0] | [0] | [v1.0] | ✅ |
| [Tests] | [X files] | [v1.0] | [0] | [v1.2] | ✅ |
| [Docker Images] | [X images] | [v1.0.0] | [0] | [v1.2.0] | ✅ |

## 3. CI Version Status

| CI | Baseline | Current | Pending Changes | Status |
|----|---------|---------|----------------|--------|
| [Source Code] | [v1.0.0] | [v1.2.0] | [0] | ✅ |
| [SRS] | [v1.0] | [v1.0] | [0] | ✅ |
| [HLD] | [v1.0] | [v1.0] | [0] | ✅ |
| [LLD] | [v1.0] | [v1.1] | [1] | 🟡 |
| [Test Plan] | [v1.0] | [v1.1] | [0] | ✅ |
| [Docker Image] | [v1.0.0] | [v1.2.0] | [0] | ✅ |
| [Database Schema] | [v1.0] | [v1.2] | [0] | ✅ |

## 4. Change Status

| Metric | Value |
|--------|-------|
| [Total CRs submitted] | [X] |
| [CRs approved] | [X] |
| [CRs implemented] | [X] |
| [CRs pending] | [X] |
| [CRs rejected] | [X] |

## 5. Baseline Status

| Baseline | Date | Version | CIs | Changes Since | Status |
|---------|------|---------|-----|-------------|--------|
| [BL-001] | [YYYY-MM-DD] | [v1.0] | [4] | [2] | ✅ Baselined |
| [BL-002] | [YYYY-MM-DD] | [v1.0] | [3] | [1] | ✅ Baselined |
| [BL-003] | [YYYY-MM-DD] | [v1.0] | [5] | [3] | ✅ Baselined |
| [BL-004] | [YYYY-MM-DD] | [v1.1] | [5] | [1] | ✅ Baselined |
| [BL-005] | [YYYY-MM-DD] | [v1.2] | [6] | [0] | ✅ Baselined |

## 6. Configuration Trends

| Month | CIs Added | CIs Modified | CIs Deleted | Baselines |
|-------|----------|-------------|------------|----------|
| [Month 1] | [10] | [5] | [0] | [1] |
| [Month 2] | [5] | [8] | [1] | [2] |
| [Month 3] | [3] | [12] | [0] | [1] |
| [Month 4] | [2] | [6] | [0] | [1] |

## 7. Status Accounting Report

### Monthly Report — [Month Year]

| Section | Summary |
|---------|--------|
| [CIs under management] | [X] |
| [Baselines established] | [X] |
| [Changes processed] | [X] |
| [Audits completed] | [X] |
| [Open issues] | [X] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SCMP]] | CM plan |
| [[Baseline-Records]] | Baseline details |
| [[Change-Request]] | Change details |

---

> **Template Standard:** Based on SWEBOK v4, SEBoK v2
> **Usage:** Status accounting answers "What do we have, what version, and what changed?" Report monthly.
