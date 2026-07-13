---
document_type: Technical Review Records (SRR, PDR, CDR, TRR)
version: "1.0"
status: Active
author: "[Systems Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [technical-reviews, srr, pdr, cdr, trr, sebok]
standard_ref:
  - SEBoK v2 — Technical Reviews
  - ISO/IEC/IEEE 15288
---

# Technical Review Records (SRR, PDR, CDR, TRR)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Records outcomes of formal technical reviews — ensuring technical baselines are established and approved.

## 2. Technical Review Lifecycle

```mermaid
flowchart LR
    SRR[SRR<br>Requirements] --> PDR[PDR<br>Design]
    PDR --> CDR[CDR<br>Implementation]
    CDR --> TRR[TRR<br>Test Readiness]
    TRR --> FCA[FCA<br>Functional]
    FCA --> PCA[PCA<br>Physical]

    style SRR fill:#2196F3,color:#fff
    style PDR fill:#4CAF50,color:#fff
    style CDR fill:#FF9800,color:#fff
    style TRR fill:#9C27B0,color:#fff
    style FCA fill:#f44336,color:#fff
    style PCA fill:#607D8B,color:#fff
```

## 3. Review Register

| Review | Date | Chair | Result | Action Items | Status |
|--------|------|-------|--------|-------------|--------|
| [SRR] | [YYYY-MM-DD] | [Chief Engineer] | ✅ Approved | [2] | ✅ Closed |
| [PDR] | [YYYY-MM-DD] | [Chief Engineer] | ✅ Approved | [3] | ✅ Closed |
| [CDR] | [YYYY-MM-DD] | [Chief Engineer] | ✅ Approved | [1] | ✅ Closed |
| [TRR] | [YYYY-MM-DD] | [Chief Engineer] | ✅ Approved | [0] | ✅ Closed |

## 4. Review Template

### [Review Name] — [Date]

| Field | Detail |
|-------|--------|
| **Review** | [SRR / PDR / CDR / TRR] |
| **Date** | [YYYY-MM-DD] |
| **Chair** | [Name] |
| **Attendees** | [List] |
| **Purpose** | [What was reviewed] |
| **Result** | [Approved / Approved with conditions / Not approved] |

### Entry Criteria

| # | Criterion | Met | Evidence |
|---|----------|-----|---------|
| 1 | [Criterion 1] | ✅ | [Evidence] |
| 2 | [Criterion 2] | ✅ | [Evidence] |

### Findings

| # | Finding | Severity | Owner | Due Date | Status |
|---|--------|---------|-------|---------|--------|
| 1 | [Finding description] | [Major/Minor] | [Name] | [YYYY-MM-DD] | ✅ Closed |

### Disposition

- [ ] ✅ Approved — proceed to next phase
- [ ] ⚠️ Approved with conditions — complete action items before proceeding
- [ ] ❌ Not approved — address findings and re-review

## 5. Review Statistics

| Review | Findings (Major) | Findings (Minor) | Action Items | Closure Rate |
|--------|-----------------|-----------------|-------------|-------------|
| [SRR] | [0] | [2] | [2] | [100%] |
| [PDR] | [1] | [2] | [3] | [100%] |
| [CDR] | [0] | [1] | [1] | [100%] |
| [TRR] | [0] | [0] | [0] | [N/A] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SEMP]] | SE management context |
| [[Design-Review-Records]] | Design reviews |
| [[Decision-Records]] | Decision documentation |

---

> **Template Standard:** Based on SEBoK v2, ISO/IEC/IEEE 15288
> **Usage:** Technical reviews are *gatekeepers*. No review = no progress. Document everything.
