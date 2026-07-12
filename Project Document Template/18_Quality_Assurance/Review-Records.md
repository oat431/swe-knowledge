---
document_type: Review Records
version: "1.0"
status: Active
author: "[QA Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [review-records, inspections, swebok, iso-20246]
standard_ref:
  - SWEBOK v4 — Quality Assurance
  - ISO/IEC 20246 — Work Product Reviews
---

# Review Records

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Records of all formal reviews — findings, actions, and dispositions.

## 2. Review Types

| Type | When | Participants | Purpose |
|------|------|-------------|---------|
| [Walkthrough] | [Informal] | [Author + peers] | [Find defects, share knowledge] |
| [Technical Review] | [Formal] | [Technical team] | [Evaluate technical quality] |
| [Inspection] | [Formal] | [Trained inspectors] | [Systematic defect detection] |

## 3. Review Register

| ID | Date | Type | Artifact | Reviewers | Findings | Actions | Disposition |
|----|------|------|---------|-----------|---------|---------|------------|
| [RV-001] | [YYYY-MM-DD] | [Technical Review] | [SAD v1.0] | [Architect, TL, QA] | [3] | [2] | ✅ Approved |
| [RV-002] | [YYYY-MM-DD] | [Technical Review] | [HLD v1.0] | [TL, Dev Team] | [5] | [4] | ✅ Approved |
| [RV-003] | [YYYY-MM-DD] | [Inspection] | [SRS v1.0] | [BA, QA, TL] | [4] | [3] | ✅ Approved |
| [RV-004] | [YYYY-MM-DD] | [Walkthrough] | [API Design] | [Dev Team] | [2] | [2] | ✅ Approved |

## 4. Review Statistics

| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| [Reviews conducted] | [X] | — | — |
| [Findings per review (avg)] | [X] | [< 5] | 🟢🟡🔴 |
| [Critical findings] | [X] | [0] | 🟢🟡🔴 |
| [Actions completed on time] | [X%] | [100%] | 🟢🟡🔴 |
| [Reviews with rework] | [X] | [0] | 🟢🟡🔴 |

## 5. Review Checklist Template

| # | Check | Status |
|---|-------|--------|
| 1 | [All requirements addressed] | ✅❌ |
| 2 | [No single points of failure] | ✅❌ |
| 3 | [Security controls adequate] | ✅❌ |
| 4 | [Performance targets achievable] | ✅❌ |
| 5 | [Error handling comprehensive] | ✅❌ |
| 6 | [Interfaces well-defined] | ✅❌ |
| 7 | [Design patterns appropriate] | ✅❌ |
| 8 | [Testability considered] | ✅❌ |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Design-Review-Records]] | Design-level reviews |
| [[Code-Review-Records]] | Code-level reviews |
| [[SQAP]] | Quality assurance plan |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC 20246
> **Usage:** Reviews catch defects when they're cheap to fix. Every major artifact gets reviewed before implementation.
