---
document_type: Project Tier Checklist — Tier 4 Small Production
schema_version: 2
canonical_name: Tier 4 Small Production Checklist
doc_form: record
applicability: evidence
min_project_tier: 4
minimum_form: This checklist itself
version: "1.0"
status: Active
created: "2026-09-24"
last_updated: "2026-09-24"
tags: [tier-checklist, tier-4, project-size, generated]
generator: Generated from template frontmatter — do not hand-edit; regenerate instead
---

# 🟢 Tier 4 — Small Production — Document Checklist

> **Description:** Single service/app, low traffic. Real users, maybe early revenue.
> **Team:** 1–2 devs | **Users:** < 1K users | **Lifespan:** Ongoing
> **Tier source:** `checklist/release-checklist/release.md` §Project Tier Scoping Matrix
>
> **Key principle:** Everything a Tier-4 project needs = every template with `min_project_tier ≤ 4` whose trigger applies. Anything above tier 4 is over-engineering for this context.
>
> **Scope:** 228 artifacts (207 new at this tier, 21 inherited from lower tiers).

## How to use

1. Confirm your tier with the decision flow in `release.md` §"Which Tier Am I?".
2. Tick ☐ → ✅ as each document is created. **Skip rows whose trigger condition doesn't apply** and note the omission in [[Tailoring-Justification]].
3. `Form` column: **heavy** = full controlled document · **light** = lean doc · **record** = produced by an activity (create when the activity happens, not upfront).
4. Priority: 🔴 universal · 🟡 conditional/evidence (check trigger) · 🟢 technique (embed in parent docs, never standalone).

## Tier ladder

| Tier | Checklist | Artifacts |
|---|---|---|
| 1 | [[Tier-1-POC-Spike-Checklist|🧪 POC / Spike]] | 1 |
| 2 | [[Tier-2-Prototype-MVP-Checklist|🔧 Prototype / MVP]] | 6 |
| 3 | [[Tier-3-Internal-Tool-Checklist|🏠 Internal Tool]] | 21 |
| 4 | **→ this tier** | 228 |
| 5 | [[Tier-5-Medium-Production-Checklist|🔵 Medium Production]] | 321 |
| 6 | [[Tier-6-Production-Grade-Checklist|🟣 Production Grade]] | 342 |
| 7 | [[Tier-7-Mission-Critical-Checklist|🔴 Mission-Critical / Regulated]] | 360 |

## Business Analysis and strategy

> **Owner:** PO / BA · 14 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Benefits-Management-Plan]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Business-Analysis-Approach]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Business-Case]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Business-Objectives]] | 🔴 | heavy | T3 | always | ☐ |
| [[Business-Requirements]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Change-Strategy]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Current-State-Description]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Future-State-Description]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Governance-Approach]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Information-Management-Approach]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Potential-Value]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Risk-Analysis-Results]] | 🟡 | record | T4 🆕 | Produced by the activity it records · view of `Risk-Management-Plan` | ☐ |
| [[Solution-Recommendation]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Solution-Scope]] | 🔴 | heavy | T4 🆕 | always | ☐ |

## Elicitation and Collaboration

> **Owner:** BA / PO · 4 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Elicitation-Activity-Plan]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Elicitation-Results-Confirmed]] | 🟡 | record | T3 | Produced by the activity it records | ☐ |
| [[Elicitation-Results-Unconfirmed]] | 🟡 | record | T3 | Produced by the activity it records | ☐ |
| [[Stakeholder-Engagement-Approach]] | 🔴 | heavy | T4 🆕 | always | ☐ |

## Concept and Mission Definition

> **Owner:** Systems Engineer / Sponsor · 5 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Concept-of-Operations]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Mission-Analysis-Report]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Stakeholder-Needs-Document]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Stakeholder-Register]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[System-Requirements-Specification]] | 🔴 | heavy | T4 🆕 | always | ☐ |

## Requirements Engineering

> **Owner:** PO / BA · 16 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Acceptance-Criteria]] | 🔴 | heavy | T3 | always | ☐ |
| [[Assumption-Log]] | 🟡 | record | T2 | Produced by the activity it records | ☐ |
| [[Business-Requirements-Document]] | 🔴 | heavy | T4 🆕 | always · view of `Business-Requirements` | ☐ |
| [[Decision-Tables-Trees]] | 🟢 | light | T4 🆕 | Used inside parent artifacts when the modeling need arises | ☐ |
| [[Definition-of-done]] | 🔴 | light | T2 | always | ☐ |
| [[Nonfunctional-Requirements-Catalog]] | 🟡 | record | T3 | Produced by the activity it records | ☐ |
| [[Project-Scope-Statement]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Requirements-Architecture]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Requirements-Change-Assessment]] | 🔴 | light | T4 🆕 | always · view of `Change-Request` | ☐ |
| [[Requirements-Change-Log]] | 🟡 | record | T4 🆕 | Produced by the activity it records · view of `Change-Request` | ☐ |
| [[Requirements-Traceability-Matrix]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Requirements-Validated]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Requirements-Verified]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Software-Requirements-Specification]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Stakeholder-Analysis]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[User-Stories]] | 🔴 | heavy | T3 | always | ☐ |

## Project Management Planning

> **Owner:** PM · 22 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Activity-List]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Communications-Management-Plan]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Cost-Baseline]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Cost-Estimates]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Financial-Management-Plan]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Milestone-List]] | 🟡 | record | T4 🆕 | Produced by the activity it records · view of `Schedule-Management-Plan` | ☐ |
| [[Project-Charter]] | 🔴 | light | T3 | always | ☐ |
| [[Project-Funding-Requirements]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Project-Management-Plan]] | 🔴 | heavy | T4 🆕 | always | ☐ |
| [[Project-Schedule]] | 🔴 | light | T4 🆕 | always · view of `Schedule-Management-Plan` | ☐ |
| [[Quality-Management-Plan]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Quality-Metrics]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[RACI-Matrix]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Resource-Management-Plan]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Resource-Requirements]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Risk-Management-Plan]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Risk-Register]] | 🟡 | record | T2 | Produced by the activity it records · view of `Risk-Management-Plan` | ☐ |
| [[Schedule-Baseline]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Schedule-Management-Plan]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Scope-Management-Plan]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Stakeholder-Engagement-Plan]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[WBS-WBS-Dictionary]] | 🔴 | light | T4 🆕 | always | ☐ |

## Project Management Executing and MC

> **Owner:** PM · 11 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Change-Log]] | 🟡 | record | T4 🆕 | Produced by the activity it records · view of `Change-Request` | ☐ |
| [[Change-Requests]] | 🟡 | record | T4 🆕 | Produced by the activity it records · view of `Change-Request` | ☐ |
| [[Cost-Forecasts]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Issue-Log]] | 🟡 | record | T3 | Produced by the activity it records | ☐ |
| [[Lessons-Learned-Register]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Meeting-Minutes]] | 🟡 | record | T3 | Produced by the activity it records | ☐ |
| [[Schedule-Forecasts]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Team-Assignments]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Work-Performance-Data]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Work-Performance-Information]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Work-Performance-Reports]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |

## Project Management Closing

> **Owner:** PM · 3 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Final-Report]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Project-Closure-Document]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Verified-Deliverables]] | 🔴 | light | T4 🆕 | always | ☐ |

## Procurement and Contracts

> **Owner:** PM / Procurement · 6 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Bid-Documents]] | 🟡 | light | T4 🆕 | External supplier, COTS, SaaS, or contract work involved | ☐ |
| [[Contract-Agreement]] | 🟡 | light | T4 🆕 | External supplier, COTS, SaaS, or contract work involved | ☐ |
| [[Procurement-Management-Plan]] | 🟡 | light | T4 🆕 | External supplier, COTS, SaaS, or contract work involved | ☐ |
| [[Procurement-Statement-of-Work]] | 🟡 | light | T4 🆕 | External supplier, COTS, SaaS, or contract work involved | ☐ |
| [[Source-Selection-Criteria]] | 🟡 | light | T4 🆕 | External supplier, COTS, SaaS, or contract work involved | ☐ |
| [[Sourcing-Strategy-Plan]] | 🟡 | light | T4 🆕 | External supplier, COTS, SaaS, or contract work involved | ☐ |

## Systems Architecture and Design

> **Owner:** Architect · 12 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[ASR-Catalog]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Architecture-Decision-Records]] | 🔴 | light | T2 | always | ☐ |
| [[Architecture-Patterns-Catalog]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Architecture-Views-4-1]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[CC-Views]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Functional-Architecture]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Interface-Control-Document]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Logical-Architecture]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Physical-Architecture]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Software-Architecture-Document]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[System-Architecture-Description]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Trade-Study-Reports]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |

## Software Design

> **Owner:** Architect / Sr. Dev · 10 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[API-Specification]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Activity-Diagrams-Design]] | 🟢 | light | T4 🆕 | Used inside parent artifacts when the modeling need arises | ☐ |
| [[Architecture-Overview]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Class-Diagrams]] | 🟢 | light | T4 🆕 | Used inside parent artifacts when the modeling need arises | ☐ |
| [[Data-Dictionary]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Database-Schema-DDL]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Design-Review-Records]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[ERD]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[High-Level-Design]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Low-Level-Design]] | 🔴 | light | T4 🆕 | always | ☐ |

## UX UI Design

> **Owner:** Designer / UX · 21 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Accessibility-Audit]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[Analytics-Dashboard-Spec]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[Asset-Export-Package]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[Component-Library]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[Design-Specifications]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[Error-State-Specifications]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[Information-Architecture]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[Interactive-Prototype]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[Journey-Map]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[Responsive-Behavior-Spec]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[Responsive-Specifications]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[Sitemap]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[State-Variations]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[Style-Guide]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[UI-Mockups]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[Usability-Test-Plan]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[Usability-Test-Report]] | 🟡 | record | T4 🆕 | Product has a user-facing interface | ☐ |
| [[User-Flows]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[User-Interview-Script]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[User-Personas]] | 🟡 | light | T4 🆕 | Product has a user-facing interface | ☐ |
| [[User-Research-Report]] | 🟡 | record | T4 🆕 | Product has a user-facing interface | ☐ |

## Construction

> **Owner:** Developer · 8 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[API-Documentation]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Build-Scripts]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Code-Review-Records]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Coding-Standards]] | 🔴 | light | T3 | always | ☐ |
| [[Commit-Messages-Changelog]] | 🟡 | record | T3 | Produced by the activity it records | ☐ |
| [[Dependency-Manifest]] | 🔴 | light | T3 | always | ☐ |
| [[README-Developer-Guide]] | 🔴 | light | T3 | always | ☐ |
| [[Static-Analysis-Reports]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |

## Testing and Verification

> **Owner:** QA · 16 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Defect-Report]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Regression-Test-Suite]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Test-Cases]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Test-Completion-Report]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Test-Data]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Test-Plan]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Test-Report]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Test-Scripts-Automated]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Test-Strategy]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Test-Suite]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Traceability-Matrix-Req-Tests]] | 🔴 | light | T4 🆕 | always · view of `Requirements-Traceability-Matrix` | ☐ |
| [[UAT-Sign-off]] | 🟡 | light | T4 🆕 | Separate business acceptance role exists; otherwise record acceptance decision elsewhere | ☐ |
| [[Validation-Plan]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Validation-Reports]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Verification-Plan]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Verification-Reports]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |

## Security

> **Owner:** Security · 16 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Access-Control-Policy]] | 🟡 | light | T4 🆕 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Authentication-Standard]] | 🟡 | light | T4 🆕 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Compliance-Assessment-Report]] | 🟡 | record | T4 🆕 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Incident-Response-Plan]] | 🟡 | light | T4 🆕 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Network-Security-Architecture]] | 🟡 | light | T4 🆕 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Risk-Treatment-Plan]] | 🟡 | light | T4 🆕 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[SAST-Report]] | 🟡 | record | T4 🆕 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[SCA-Report]] | 🟡 | record | T4 🆕 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[SSDLC-Process-Documentation]] | 🟡 | light | T4 🆕 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Secure-Coding-Guidelines]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Secure-Design-Review-Report]] | 🟡 | record | T4 🆕 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Security-Architecture]] | 🟡 | light | T4 🆕 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Security-Metrics-Dashboard]] | 🟡 | record | T4 🆕 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Security-Policy]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Security-Requirements-Specification]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Threat-Model]] | 🔴 | light | T4 🆕 | always | ☐ |

## Data Management

> **Owner:** Data / DBA · 33 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[API-Data-Contract]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Backup-Recovery-Plan]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Business-Glossary]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Content-Classification-Taxonomy]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Architecture-Blueprint]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Breach-Response-Plan]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Catalog]] | 🟡 | record | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Classification-Schema]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Cleansing-Specification]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Encryption-Standards]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Flow-Diagram]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Integration-Architecture]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Lineage-Documentation]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Masking-Anonymization-Rules]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Model-Review-Records]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Modeling-Standards]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Profiling-Report]] | 🟡 | record | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Quality-Issue-Log]] | 🟡 | record | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Quality-Rules]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Quality-Scorecard]] | 🟡 | record | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Quality-Strategy]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Replication-Synchronization-Spec]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Security-Audit-Report]] | 🟡 | record | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Standards]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Database-Operational-Runbook]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Dimensional-Model]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[ETL-ELT-Specification]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[High-Availability-DR-Configuration]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Privacy-Impact-Assessment]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Records-Retention-Schedule]] | 🟡 | light | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Reference-Data-Catalog]] | 🟡 | record | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Regulatory-Compliance-Register]] | 🟡 | record | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Report-Dashboard-Catalog]] | 🟡 | record | T4 🆕 | Shared/master data, analytics workload, regulated or personal data | ☐ |

## Deployment and Operations

> **Owner:** DevOps / SRE · 7 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[CI-CD-Pipeline-Configuration]] | 🔴 | light | T3 | always | ☐ |
| [[Deployment-Plan]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Disaster-Recovery-Plan]] | 🟡 | light | T4 🆕 | Meaningful availability or data-loss requirement | ☐ |
| [[Incident-Management-Process]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Operations-Manual-Runbook]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Release-Notes]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Rollback-Plan]] | 🔴 | light | T4 🆕 | always | ☐ |

## Maintenance and Support

> **Owner:** Support / Maintainer · 4 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Impact-Analysis-Report]] | 🟡 | record | T4 🆕 | Produced by the activity it records | ☐ |
| [[Incident-Problem-Reports]] | 🟡 | record | T4 🆕 | Produced by the activity it records · view of `Incident-Management-Process` | ☐ |
| [[Maintenance-Log-Change-History]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Maintenance-Plan]] | 🔴 | light | T4 🆕 | always | ☐ |

## Quality Assurance

> **Owner:** QA Lead · 3 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Defect-Log-Metrics]] | 🔴 | record | T4 🆕 | always | ☐ |
| [[Review-Records]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[SQAP]] | 🔴 | light | T4 🆕 | always | ☐ |

## Configuration Management

> **Owner:** Config Manager · 5 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Baseline-Records]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Change-Request]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Configuration-Management-Plan]] | 🟡 | light | T4 🆕 | Formal configuration management / contractual CM required | ☐ |
| [[SCMP]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Version-Description-Document]] | 🔴 | light | T4 🆕 | always | ☐ |

## SE Cross Cutting

> **Owner:** Systems Engineer · 7 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Decision-Records]] | 🔴 | light | T2 | always | ☐ |
| [[Implementation-Plan]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Integration-Plan-Reports]] | 🟡 | record | T4 🆕 | Multi-team / systems-engineering lifecycle context | ☐ |
| [[Measurement-Plan]] | 🔴 | light | T3 | always | ☐ |
| [[Security-Plan]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Tailoring-Justification]] | 🔴 | light | T1 | always | ☐ |
| [[Transition-Plan]] | 🟡 | light | T4 🆕 | Multi-team / systems-engineering lifecycle context | ☐ |

## Solution Evaluation

> **Owner:** BA / PO · 5 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Enterprise-Limitation]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Recommended-Actions]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Solution-Limitation]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Solution-Performance-Analysis]] | 🔴 | light | T4 🆕 | always | ☐ |
| [[Solution-Performance-Measures]] | 🔴 | light | T4 🆕 | always | ☐ |

---

## Tailoring record

| # | Document omitted / combined | Reason | Approved by |
|---|---|---|---|
| 1 | [Document] | [Why not needed at this tier] | [Name] |

> Copy omissions into the project's [[Tailoring-Justification]].

## Related

- [[7-Tier Applicability Matrix]] — full matrix view over all 363 templates
- [[release]] — tier model source (Project Tier Scoping Matrix)
- [[TEMPLATE-INDEX]] — regenerated master index
