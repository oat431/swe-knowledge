---
document_type: Medical Device File
version: "1.0"
status: Draft
author: "[Regulatory Affairs]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Confidential"
tags: [medical-device, fda, iec-62304, sebok]
standard_ref:
  - SEBoK v2 — Software Engineering
  - IEC 62304 — Medical Device Software Life Cycle
  - FDA 21 CFR Part 820
---

# Medical Device File

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]
>
> ⚠️ **Domain-Specific:** This document applies to medical device software projects.

---

## 1. Purpose

> Maintains the regulatory documentation required for medical device software — per IEC 62304 and FDA requirements.

## 2. Regulatory Framework

| Standard | Scope | Applicability |
|---------|-------|-------------|
| [IEC 62304] | [Software life cycle processes] | [Required] |
| [ISO 14971] | [Risk management] | [Required] |
| [FDA 21 CFR 820] | [Quality system regulation] | [Required (US)] |
| [EU MDR] | [Medical device regulation] | [Required (EU)] |

## 3. Software Safety Classification

| Class | Description | Impact | Requirements |
|-------|-----------|--------|-------------|
| [Class A] | [No injury possible] | [None] | [Basic lifecycle] |
| [Class B] | [Non-serious injury possible] | [Minor] | [Enhanced lifecycle] |
| [Class C] | [Death or serious injury possible] | [Critical] | [Full lifecycle + verification] |

### Project Classification

| Field | Detail |
|-------|--------|
| [Software Safety Class] | [Class B] |
| [Rationale] | [Incorrect data could lead to non-serious injury] |
| [Regulatory Class] | [Class II] |
| [Predicate Device] | [Device Name, 510(k) number] |

## 4. Required Documentation

| # | Document | IEC 62304 Clause | Status | Location |
|---|---------|-----------------|--------|---------|
| 1 | [Software Development Plan] | [5.1] | ✅ | [[Software-Development-Plan]] |
| 2 | [Software Requirements Specification] | [5.2] | ✅ | [[Software-Requirements-Specification]] |
| 3 | [Software Architecture Design] | [5.3] | ✅ | [[Software-Architecture-Document]] |
| 4 | [Software Detailed Design] | [5.4] | ✅ | [[Low-Level-Design]] |
| 5 | [Software Unit Implementation] | [5.5] | ✅ | [Source code] |
| 6 | [Software Unit Verification] | [5.6] | ✅ | [[Test-Cases]] |
| 7 | [Software Integration Testing] | [5.7] | ✅ | [[Test-Report]] |
| 8 | [Software System Testing] | [5.8] | ✅ | [[Test-Report]] |
| 9 | [Risk Management File] | [ISO 14971] | ✅ | [[Risk-Register]] |
| 10 | [Software Maintenance Plan] | [6.1] | ✅ | [[Maintenance-Plan]] |
| 11 | [Problem Resolution] | [6.2] | ✅ | [[Defect-Report]] |
| 12 | [Configuration Management] | [8.1] | ✅ | [[SCMP]] |

## 5. Traceability

| Requirement | Design | Implementation | Verification | Risk |
|-------------|--------|---------------|-------------|------|
| [SRS-001] | [SAD §3.1] | [Module X] | [TC-001] | [R-001] |
| [SRS-002] | [SAD §3.2] | [Module Y] | [TC-002] | [R-002] |

## 6. Risk Management Summary

| Hazard | Cause | Severity | Probability | Risk Level | Mitigation |
|--------|-------|---------|-----------|-----------|-----------|
| [Incorrect calculation] | [Software defect] | [Major] | [Low] | [Medium] | [Verification, validation] |
| [Data loss] | [System failure] | [Major] | [Low] | [Medium] | [Backup, recovery] |
| [Unauthorized access] | [Security breach] | [Major] | [Low] | [Medium] | [Authentication, audit] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Software-Development-Plan]] | Development approach |
| [[Software-Assurance-Plan]] | Assurance activities |
| [[Risk-Register]] | Risk management |

---

> **Template Standard:** Based on SEBoK v2, IEC 62304, FDA 21 CFR 820
> **Usage:** Medical device software requires *complete traceability* — requirement → design → code → test → risk. No gaps allowed.
