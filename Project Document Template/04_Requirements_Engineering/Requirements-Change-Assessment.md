---
document_type: Requirements Change Assessment
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
ba_owner: "[Business Analyst Name]"
classification: "Internal / Confidential"
tags: [requirements-change-assessment, impact-analysis, babok, iso-29148]
standard_ref:
  - BABOK v3 — Requirements Life Cycle Management
  - ISO/IEC/IEEE 29148 — Requirements Engineering
  - IEEE 828 — Configuration Management
---

# Requirements Change Assessment

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Business Analyst | [Name / Role] |
| Change Authority | [PM / CCB / Steering Committee] |

---

## 1. Purpose

> This document provides a structured template for assessing proposed changes to baselined requirements. Each assessment evaluates impact on scope, schedule, cost, quality, and risk before a decision is made.

## 2. Change Assessment Template

### CR-XXX: [Change Title]

| Field | Detail |
|-------|--------|
| **Change Request ID** | [CR-XXX] |
| **Date Submitted** | [YYYY-MM-DD] |
| **Requestor** | [Name / Role] |
| **Affected Requirement(s)** | [FR-XXX, NFR-XXX] |
| **Change Type** | [Add / Modify / Delete] |
| **Classification** | [Minor / Moderate / Major] |

#### Change Description

| What is changing | [Clear description of the proposed change] |
|-----------------|------------------------------------------|
| **Why** | [Business justification for the change] |
| **Urgency** | [🔴 Immediate / 🟡 Next Sprint / 🟢 Next Phase / ⚪ Future] |

#### Impact Analysis

| Dimension | Impact | Details |
|-----------|--------|---------|
| **Scope** | [🔴 High / 🟡 Medium / 🟢 Low / None] | [What must be added, removed, or modified] |
| **Schedule** | [X days added / No impact] | [Which milestones are affected] |
| **Cost** | [$X additional / No impact] | [Development, testing, infrastructure] |
| **Quality** | [🔴 High / 🟡 Medium / 🟢 Low / None] | [Effect on performance, security, usability] |
| **Risk** | [🔴 High / 🟡 Medium / 🟢 Low / None] | [New risks introduced or existing risks changed] |
| **Other Requirements** | [X requirements affected] | [Which other requirements are impacted] |
| **Design** | [X design elements affected] | [Which design documents need updating] |
| **Test Cases** | [X test cases affected] | [Which tests need updating or creating] |
| **Documentation** | [X documents affected] | [Which docs need updating] |

#### Downstream Impact

| Artifact | Impact | Action Required |
|----------|--------|----------------|
| [[Software-Requirements-Specification]] | [Section X.X modified] | [Update requirement] |
| [[Requirements-Traceability-Matrix]] | [New links added] | [Update RTM] |
| [[Acceptance-Criteria]] | [New ACs for modified requirement] | [Write new ACs] |
| [[User-Stories]] | [Story split or modified] | [Update story] |
| [[High-Level-Design]] | [Component X affected] | [Update design] |
| [[Acceptance-Criteria]] | [TC-XX modified, new TC created] | [Update/create tests] |
| [[Usability-Test-Plan]] | [Test scope expanded] | [Update test plan] |

#### Recommendation

| Decision | Recommendation | Rationale |
|----------|---------------|-----------|
| **Approve** | [Recommended / Not Recommended] | [Why] |
| **Reject** | [With rationale] | [Why not] |
| **Defer** | [To which phase] | [Why defer] |

#### Decision

| Field | Detail |
|-------|--------|
| **Decision** | ✅ Approved / ❌ Rejected / ⏸️ Deferred |
| **Decision Date** | [YYYY-MM-DD] |
| **Decision Authority** | [PM / CCB / Steering Committee] |
| **Conditions** | [Any conditions attached] |
| **Decision Rationale** | [Summary of decision reasoning] |

## 3. Change Assessment Register

| CR ID | Date | Title | Classification | Scope | Schedule | Cost | Decision | Authority |
|-------|------|-------|---------------|-------|----------|------|----------|-----------|
| CR-001 | [YYYY-MM-DD] | [Add save draft] | Minor | +1 feature | +3 days | +$2K | ✅ Approved | PM |
| CR-002 | [YYYY-MM-DD] | [Change response time] | Minor | None | None | +$5K | ✅ Approved | PM |
| CR-003 | [YYYY-MM-DD] | [Add ML anomaly detection] | Major | +large feature | +3 months | +$50K | ⏸️ Deferred | Steering |
| CR-004 | [YYYY-MM-DD] | [Add bulk upload] | Moderate | +1 feature | +5 days | +$3K | ✅ Approved | CCB |
| CR-005 | | | | | | | | |

## 4. Change Impact Summary

| Category | Approved Changes | Total Impact |
|----------|-----------------|-------------|
| [Features added] | [X] | [Y story points] |
| [Schedule impact] | [X changes] | [Y days total] |
| [Cost impact] | [X changes] | [$Y total] |
| [Requirements modified] | [X] | — |
| [Requirements added] | [X] | — |
| [Requirements removed] | [X] | — |

## 5. Change Assessment Statistics

| Metric | Value | Target |
|--------|-------|--------|
| [Total change requests] | [X] | — |
| [Average processing time] | [X days] | [≤5 days] |
| [Approval rate] | [X%] | — |
| [Rejection rate] | [X%] | — |
| [Deferral rate] | [X%] | — |
| [Requirements stability index] | [X%] | [≥85%] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Requirements-Change-Log]] | Change log tracks all changes |
| [[Software-Requirements-Specification]] | Requirements being assessed |
| [[Requirements-Traceability-Matrix]] | Impact on traceability |
| [[Governance-Approach]] | Change authority definitions |
| [[Change-Requests]] | Version control for requirements |

---

> **Template Standard:** Based on BABOK v3, ISO/IEC/IEEE 29148, IEEE 828
> **Usage:** Use this template for *every* proposed change to a baselined requirement. The impact analysis must be thorough — missed downstream impacts cause rework. The decision must be documented with rationale for audit purposes.
