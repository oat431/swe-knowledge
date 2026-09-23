---
document_type: ASR Catalog
schema_version: 2
canonical_name: ASR Catalog
doc_form: record
applicability: evidence
min_project_tier: 4  # Small-Prod
tier_trigger: Produced by the activity it records
minimum_form: Entries in the project tracker or repo
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
architect: "[Solution Architect]"
classification: "Internal / Confidential"
tags: [asr, architecturally-significant, requirements, swebok, iso-42010]
standard_ref:
  - SWEBOK v4 — Architecture
  - ISO/IEC/IEEE 42010 — Architecture Description
---

# ASR Catalog (Architecturally Significant Requirements)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This catalog identifies and tracks Architecturally Significant Requirements (ASRs) — requirements that have a profound effect on the architecture. Not all requirements drive architecture; this catalog focuses on those that do.

## 2. What Makes a Requirement Architecturally Significant?

| Criterion | Description | Example |
|----------|-------------|---------|
| [Quality Attribute] | [NFR that constrains architecture] | [99.9% availability] |
| [Structural] | [Affects component decomposition] | [Independent deployment] |
| [Integration] | [External system connectivity] | [Must integrate with ERP] |
| [Constraint] | [Technology or organizational limitation] | [Must use existing cloud] |
| [High Risk] | [Requirement with significant risk] | [Data migration 500K records] |
| [High Business Value] | [Critical business function] | [Auto-approval engine] |

## 3. ASR Register

| ASR ID | Requirement | Category | Priority | Architecture Impact | Design Decision |
|--------|-------------|----------|----------|-------------------|----------------|
| ASR-[XXX] | [System shall ...] | [Quality / Integration / Constraint / Functional] | 🔴 | [Architecture impact] | ADR-[XXX] |

## 4. ASR to Architecture Mapping

| ASR | Architecture Component | Design Pattern | Trade-off |
|-----|----------------------|---------------|----------|
| ASR-[XXX] | [Architecture Component] | [Design Pattern] | [Trade-off] |

## 5. ASR Quality Attribute Scenarios

| ASR | Quality Attribute | Scenario | Response Measure |
|-----|------------------|---------|-----------------|
| ASR-[XXX] | [Performance / Availability / Scalability] | [Stimulus and environment] | [Measurable response] |

## 6. ASR Traceability

| ASR | Business Req | System Req | Design Element | Test Case |
|-----|-------------|-----------|---------------|-----------|
| ASR-[XXX] | [BR-XX / NFR-XXX] | [SYRS-XXX] | [Design Element] | [TC-ID] |

## 7. ASR Risk Assessment

| ASR | Risk | Probability | Impact | Mitigation |
|-----|------|-----------|--------|-----------|
| ASR-[XXX] | [Risk description] | [Low / Medium / High] | [Low / Medium / High] | [Mitigation] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Software-Requirements-Specification]] | All requirements including ASRs |
| [[Software-Architecture-Document]] | Architecture driven by ASRs |
| [[Architecture-Decision-Records]] | Decisions driven by ASRs |
| [[Trade-Study-Reports]] | Analysis of ASR-driven alternatives |
| [[Nonfunctional-Requirements-Catalog]] | NFRs that become ASRs |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC/IEEE 42010
> **Usage:** Not all requirements are architecturally significant. ASRs are the ones that *shape* the architecture — they determine component structure, technology choices, and quality attributes. Review this catalog at every architecture review.