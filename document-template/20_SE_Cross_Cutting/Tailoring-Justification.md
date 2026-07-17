---
document_type: Tailoring Justification
version: "1.0"
status: Draft
author: "[Systems Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [tailoring, justification, process, sebok]
standard_ref:
  - SEBoK v2 — Systems Engineering Management
  - ISO/IEC/IEEE 15288
---

# Tailoring Justification

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Justifies deviations from standard processes — documenting why standard practices were adapted for this project.

## 2. Tailoring Principles

| # | Principle | Description |
|---|----------|-------------|
| 1 | [Justified] | [Every deviation has a documented reason] |
| 2 | [Approved] | [Deviations approved by appropriate authority] |
| 3 | [Risk-based] | [Deviations driven by risk assessment] |
| 4 | [Documented] | [All deviations recorded with rationale] |

## 3. Tailoring Register

| # | Standard Process | Tailored Process | Rationale | Risk | Approved By |
|---|-----------------|-----------------|-----------|------|------------|
| 1 | [Formal requirements review] | [Peer review + BA sign-off] | [Small team, agile approach] | 🟢 Low | [Chief Engineer] |
| 2 | [Formal design review] | [Tech lead review + ADR] | [Iterative design] | 🟢 Low | [Chief Engineer] |
| 3 | [Formal test plan] | [Test strategy + automated tests] | [TDD approach] | 🟢 Low | [QA Lead] |
| 4 | [Formal configuration audit] | [CI/CD + automated checks] | [Automated pipeline] | 🟢 Low | [Chief Engineer] |
| 5 | [Formal change control board] | [PR-based review + tech lead approval] | [Small team, fast decisions] | 🟢 Low | [PM] |

## 4. Justification Details

### Tailoring #1: Requirements Review

| Field | Detail |
|-------|--------|
| **Standard** | [Formal requirements review with stakeholders] |
| **Tailored** | [Peer review + BA sign-off] |
| **Rationale** | [Small team (5 people), co-located, agile methodology. Formal review would add overhead without value.] |
| **Risk** | [Low — team has direct access to BA, requirements reviewed in sprint planning] |
| **Mitigation** | [BA validates requirements with stakeholders before sprint] |
| **Approval** | [Chief Engineer — YYYY-MM-DD] |

### Tailoring #2: Design Review

| Field | Detail |
|-------|--------|
| **Standard** | [Formal design review with architecture board] |
| **Tailored** | [Tech lead review + ADR documentation] |
| **Rationale** | [Iterative design, continuous refinement. Formal review would delay iteration.] |
| **Risk** | [Low — tech lead is experienced architect, ADRs document decisions] |
| **Mitigation** | [ADRs reviewed in sprint retrospectives] |
| **Approval** | [Chief Engineer — YYYY-MM-DD] |

## 5. Tailoring Impact Assessment

| Process Area | Tailored | Risk Level | Mitigation |
|-------------|---------|-----------|-----------|
| [Requirements] | [30%] | 🟢 Low | [BA involvement, peer review] |
| [Design] | [40%] | 🟢 Low | [Tech lead oversight, ADRs] |
| [Testing] | [20%] | 🟢 Low | [TDD, automated tests] |
| [Configuration] | [50%] | 🟢 Low | [CI/CD automation] |
| [Change Control] | [60%] | 🟢 Low | [PR-based workflow] |

## 6. Review Schedule

| Review | Frequency | Purpose |
|--------|----------|---------|
| [Tailoring review] | [Quarterly] | [Assess if tailoring still appropriate] |
| [Risk reassessment] | [Monthly] | [Ensure risks still acceptable] |
| [Process improvement] | [Semi-annually] | [Identify improvement opportunities] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SEMP]] | SE management context |
| [[Decision-Records]] | Decision documentation |
| [[Standards-Compliance-Matrix]] | Standards compliance |

---

> **Template Standard:** Based on SEBoK v2, ISO/IEC/IEEE 15288
> **Usage:** Tailoring is *not* cutting corners. It's adapting processes to context. Document why. Get approval. Review regularly.
