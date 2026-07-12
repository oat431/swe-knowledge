---
document_type: QMS Documentation
version: "1.0"
status: Draft
author: "[QA Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [qms, quality-management, iso-9001, swebok]
standard_ref:
  - SWEBOK v4 — Quality Assurance
  - ISO 9001 — Quality Management System
---

# QMS Documentation (Quality Management System)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines the Quality Management System — policies, procedures, and work instructions that ensure consistent quality.

## 2. QMS Structure

```mermaid
flowchart TD
    QMS[Quality<br>Management System] --> POLICY[Quality<br>Policy]
    QMS --> MANUAL[Quality<br>Manual]
    QMS --> PROCEDURES[Procedures]
    QMS --> RECORDS[Quality<br>Records]
    
    PROCEDURES --> P1[Document Control]
    PROCEDURES --> P2[Change Control]
    PROCEDURES --> P3[Review Process]
    PROCEDURES --> P4[Testing Process]
    PROCEDURES --> P5[Defect Management]
    PROCEDURES --> P6[Corrective Action]

    style QMS fill:#1a237e,color:#fff
    style POLICY fill:#4CAF50,color:#fff
    style MANUAL fill:#2196F3,color:#fff
    style PROCEDURES fill:#FF9800,color:#fff
    style RECORDS fill:#9C27B0,color:#fff
```

## 3. Quality Policy

> [Organization] is committed to delivering software that meets customer requirements and exceeds expectations. We achieve this through:
> - Continuous improvement
> - Customer focus
> - Process adherence
> - Team empowerment

## 4. QMS Procedures

| # | Procedure | Owner | Frequency | Status |
|---|----------|-------|----------|--------|
| 1 | [Document Control] | [QA Lead] | [Ongoing] | ✅ Active |
| 2 | [Change Control] | [PM] | [Ongoing] | ✅ Active |
| 3 | [Review Process] | [QA Lead] | [Per artifact] | ✅ Active |
| 4 | [Testing Process] | [QA Lead] | [Per sprint] | ✅ Active |
| 5 | [Defect Management] | [QA Lead] | [Ongoing] | ✅ Active |
| 6 | [Corrective Action] | [QA Lead] | [As needed] | ✅ Active |

## 5. Quality Records

| Record | Retention | Location | Owner |
|--------|----------|---------|-------|
| [Review Records] | [3 years] | [Repository] | [QA Lead] |
| [Test Reports] | [3 years] | [Repository] | [QA Lead] |
| [Defect Reports] | [3 years] | [Defect tracking] | [QA Lead] |
| [Audit Reports] | [5 years] | [Repository] | [QA Lead] |
| [Corrective Actions] | [3 years] | [Repository] | [QA Lead] |

## 6. Document Control

| Rule | Implementation |
|------|---------------|
| [Version control] | [Git repository] |
| [Review required] | [All documents reviewed before approval] |
| [Change history] | [Documented in each document] |
| [Approval signatures] | [Required for baselines] |
| [Distribution] | [Controlled via repository access] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SQAP]] | Quality assurance plan |
| [[Review-Records]] | Review evidence |
| [[Audit-Reports]] | Audit evidence |

---

> **Template Standard:** Based on SWEBOK v4, ISO 9001
> **Usage:** The QMS is the *quality backbone*. If it's not documented, it's not a process.
