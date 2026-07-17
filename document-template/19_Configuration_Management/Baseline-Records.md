---
document_type: Baseline Records
version: "1.0"
status: Active
author: "[Configuration Manager]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [baseline, configuration-baseline, swebok, sebok]
standard_ref:
  - SWEBOK v4 — Configuration Management
  - SEBoK v2 — Configuration Management
---

# Baseline Records

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Records configuration baselines — approved snapshots of CIs at specific points in time. Baselines are the *reference points* for change control.

## 2. Baseline Definition

> A baseline is an approved snapshot of one or more configuration items that formally establishes a reference for subsequent changes. Changes to baselined items require formal change control.

## 3. Baseline Types

| Type | When | Contents | Approval |
|------|------|---------|---------|
| [Functional Baseline] | [After requirements] | [SRS, SRS, Requirements] | [CCB] |
| [Allocated Baseline] | [After design] | [HLD, LLD, Architecture] | [CCB] |
| [Development Baseline] | [During construction] | [Code, tests, configs] | [Tech Lead] |
| [Product Baseline] | [After testing] | [All deliverables] | [CCB] |

## 4. Baseline Register

| ID | Name | Date | Version | Contents | Approved By | Status |
|----|------|------|---------|---------|-----------|--------|
| [BL-001] | [Requirements Baseline] | [YYYY-MM-DD] | [v1.0] | [SRS, SRS, RTM] | [CCB] | ✅ Baselined |
| [BL-002] | [Design Baseline] | [YYYY-MM-DD] | [v1.0] | [HLD, LLD, SAD] | [CCB] | ✅ Baselined |
| [BL-003] | [Development Baseline — Sprint 1] | [YYYY-MM-DD] | [v1.0] | [Code, tests] | [Tech Lead] | ✅ Baselined |
| [BL-004] | [Development Baseline — Sprint 2] | [YYYY-MM-DD] | [v1.1] | [Code, tests] | [Tech Lead] | ✅ Baselined |
| [BL-005] | [Product Baseline — v1.0] | [YYYY-MM-DD] | [v1.0] | [All deliverables] | [CCB] | ✅ Baselined |

## 5. Baseline Contents

### BL-001: Requirements Baseline

| CI | Version | Location | Status |
|----|---------|---------|--------|
| [Software Requirements Specification] | [v1.0] | [docs/srs-v1.0.md] | ✅ Baselined |
| [System Requirements Specification] | [v1.0] | [docs/syrs-v1.0.md] | ✅ Baselined |
| [Requirements Traceability Matrix] | [v1.0] | [docs/rtm-v1.0.md] | ✅ Baselined |
| [Nonfunctional Requirements Catalog] | [v1.0] | [docs/nfr-v1.0.md] | ✅ Baselined |

### BL-005: Product Baseline — v1.0

| CI | Version | Location | Status |
|----|---------|---------|--------|
| [Source Code] | [v1.0.0] | [Git tag: v1.0.0] | ✅ Baselined |
| [Docker Image] | [v1.0.0] | [ghcr.io/org/project:v1.0.0] | ✅ Baselined |
| [Database Schema] | [v1.0] | [migrations/v1.0/] | ✅ Baselined |
| [Test Suite] | [v1.0] | [tests/] | ✅ Baselined |
| [Documentation] | [v1.0] | [docs/] | ✅ Baselined |

## 6. Baseline Change Log

| Date | Baseline | Change | Reason | Approved By |
|------|---------|--------|--------|-----------|
| [YYYY-MM-DD] | [BL-001] | [Added 2 requirements] | [Stakeholder feedback] | [CCB] |
| [YYYY-MM-DD] | [BL-003] | [Bug fix] | [DEF-001] | [Tech Lead] |

## 7. Baseline Comparison

| CI | BL-001 | BL-002 | BL-005 | Changes |
|----|--------|--------|--------|---------|
| [SRS] | [v1.0] | [v1.0] | [v1.0] | [None] |
| [HLD] | — | [v1.0] | [v1.0] | [None] |
| [Code] | — | — | [v1.0.0] | [New] |
| [Tests] | — | — | [v1.0] | [New] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SCMP]] | CM plan |
| [[Version-Description-Document]] | Version details |
| [[FCA-Report]] | Functional audit |
| [[PCA-Report]] | Physical audit |

---

> **Template Standard:** Based on SWEBOK v4, SEBoK v2
> **Usage:** Baselines are *frozen* — once approved, changes require formal change control. They're your "known good" reference points.
