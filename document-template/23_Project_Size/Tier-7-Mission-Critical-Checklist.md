---
document_type: Project Tier Checklist — Tier 7 Mission-Critical / Regulated
schema_version: 2
canonical_name: Tier 7 Mission-Critical / Regulated Checklist
doc_form: record
applicability: evidence
min_project_tier: 7
minimum_form: This checklist itself
version: "1.0"
status: Active
created: "2026-09-24"
last_updated: "2026-09-24"
tags: [tier-checklist, tier-7, project-size, generated]
generator: Generated from template frontmatter — do not hand-edit; regenerate instead
---

# 🔴 Tier 7 — Mission-Critical / Regulated — Document Checklist

> **Description:** Healthcare (HIPAA), finance (PCI-DSS), safety systems. Failure = severe harm.
> **Team:** 10+ devs | **Users:** Varies | **Lifespan:** Decades
> **Tier source:** `checklist/release-checklist/release.md` §Project Tier Scoping Matrix
>
> **Key principle:** Everything a Tier-7 project needs = every template with `min_project_tier ≤ 7` whose trigger applies. Anything above tier 7 is over-engineering for this context.
>
> **Scope:** 360 artifacts (18 new at this tier, 342 inherited from lower tiers).

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
| 4 | [[Tier-4-Small-Production-Checklist|🟢 Small Production]] | 228 |
| 5 | [[Tier-5-Medium-Production-Checklist|🔵 Medium Production]] | 321 |
| 6 | [[Tier-6-Production-Grade-Checklist|🟣 Production Grade]] | 342 |
| 7 | **→ this tier** | 360 |

## Business Analysis and strategy

> **Owner:** PO / BA · 18 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[BA-Performance-Assessment]] | 🔴 | heavy | T5 | always | ☐ |
| [[Benefits-Management-Plan]] | 🔴 | heavy | T4 | always | ☐ |
| [[Business-Analysis-Approach]] | 🔴 | heavy | T4 | always | ☐ |
| [[Business-Case]] | 🔴 | heavy | T4 | always | ☐ |
| [[Business-Objectives]] | 🔴 | heavy | T3 | always | ☐ |
| [[Business-Requirements]] | 🔴 | heavy | T4 | always | ☐ |
| [[Change-Strategy]] | 🔴 | heavy | T4 | always | ☐ |
| [[Current-State-Description]] | 🔴 | heavy | T4 | always | ☐ |
| [[Design-Options]] | 🔴 | heavy | T5 | always | ☐ |
| [[Enterprise-Readiness-Assessment]] | 🔴 | heavy | T5 | always | ☐ |
| [[Future-State-Description]] | 🔴 | heavy | T4 | always | ☐ |
| [[Gap-Analysis]] | 🔴 | heavy | T5 | always | ☐ |
| [[Governance-Approach]] | 🔴 | heavy | T4 | always | ☐ |
| [[Information-Management-Approach]] | 🔴 | heavy | T4 | always | ☐ |
| [[Potential-Value]] | 🔴 | heavy | T4 | always | ☐ |
| [[Risk-Analysis-Results]] | 🟡 | record | T4 | Produced by the activity it records · view of `Risk-Management-Plan` | ☐ |
| [[Solution-Recommendation]] | 🔴 | heavy | T4 | always | ☐ |
| [[Solution-Scope]] | 🔴 | heavy | T4 | always | ☐ |

## Elicitation and Collaboration

> **Owner:** BA / PO · 4 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Elicitation-Activity-Plan]] | 🔴 | heavy | T4 | always | ☐ |
| [[Elicitation-Results-Confirmed]] | 🟡 | record | T3 | Produced by the activity it records | ☐ |
| [[Elicitation-Results-Unconfirmed]] | 🟡 | record | T3 | Produced by the activity it records | ☐ |
| [[Stakeholder-Engagement-Approach]] | 🔴 | heavy | T4 | always | ☐ |

## Concept and Mission Definition

> **Owner:** Systems Engineer / Sponsor · 7 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Concept-of-Operations]] | 🔴 | heavy | T4 | always | ☐ |
| [[Feasibility-Study]] | 🔴 | heavy | T5 | always | ☐ |
| [[Market-Analysis-Technology-Assessment]] | 🔴 | heavy | T5 | always | ☐ |
| [[Mission-Analysis-Report]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Stakeholder-Needs-Document]] | 🔴 | heavy | T4 | always | ☐ |
| [[Stakeholder-Register]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[System-Requirements-Specification]] | 🔴 | heavy | T4 | always | ☐ |

## Requirements Engineering

> **Owner:** PO / BA · 20 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Acceptance-Criteria]] | 🔴 | heavy | T3 | always | ☐ |
| [[Activity-Diagrams]] | 🟢 | light | T5 | Used inside parent artifacts when the modeling need arises | ☐ |
| [[Assumption-Log]] | 🟡 | record | T2 | Produced by the activity it records | ☐ |
| [[Business-Requirements-Document]] | 🔴 | heavy | T4 | always · view of `Business-Requirements` | ☐ |
| [[Decision-Tables-Trees]] | 🟢 | light | T4 | Used inside parent artifacts when the modeling need arises | ☐ |
| [[Definition-of-done]] | 🔴 | light | T2 | always | ☐ |
| [[Functional-Size-Measurement]] | 🟢 | light | T5 | Used inside parent artifacts when the modeling need arises | ☐ |
| [[Nonfunctional-Requirements-Catalog]] | 🟡 | record | T3 | Produced by the activity it records | ☐ |
| [[Project-Scope-Statement]] | 🔴 | heavy | T4 | always | ☐ |
| [[Prototypes]] | 🟢 | heavy | T5 | Used inside parent artifacts when the modeling need arises | ☐ |
| [[Requirements-Architecture]] | 🔴 | heavy | T4 | always | ☐ |
| [[Requirements-Change-Assessment]] | 🔴 | light | T4 | always · view of `Change-Request` | ☐ |
| [[Requirements-Change-Log]] | 🟡 | record | T4 | Produced by the activity it records · view of `Change-Request` | ☐ |
| [[Requirements-Traceability-Matrix]] | 🔴 | heavy | T4 | always | ☐ |
| [[Requirements-Validated]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Requirements-Verified]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Software-Requirements-Specification]] | 🔴 | heavy | T4 | always | ☐ |
| [[Stakeholder-Analysis]] | 🔴 | heavy | T4 | always | ☐ |
| [[Use-Case-Specifications]] | 🟢 | heavy | T5 | Used inside parent artifacts when the modeling need arises | ☐ |
| [[User-Stories]] | 🔴 | heavy | T3 | always | ☐ |

## Project Management Planning

> **Owner:** PM · 29 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Activity-List]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Basis-of-Estimates]] | 🔴 | light | T5 | always | ☐ |
| [[Communications-Management-Plan]] | 🔴 | light | T4 | always | ☐ |
| [[Cost-Baseline]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Cost-Estimates]] | 🔴 | light | T4 | always | ☐ |
| [[Financial-Management-Plan]] | 🔴 | light | T4 | always | ☐ |
| [[Milestone-List]] | 🟡 | record | T4 | Produced by the activity it records · view of `Schedule-Management-Plan` | ☐ |
| [[Project-Charter]] | 🔴 | light | T3 | always | ☐ |
| [[Project-Funding-Requirements]] | 🔴 | light | T4 | always | ☐ |
| [[Project-Management-Plan]] | 🔴 | heavy | T4 | always | ☐ |
| [[Project-Schedule]] | 🔴 | light | T4 | always · view of `Schedule-Management-Plan` | ☐ |
| [[Quality-Management-Plan]] | 🔴 | light | T4 | always | ☐ |
| [[Quality-Metrics]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[RACI-Matrix]] | 🔴 | light | T4 | always | ☐ |
| [[Resource-Breakdown-Structure]] | 🔴 | light | T5 | always | ☐ |
| [[Resource-Management-Plan]] | 🔴 | light | T4 | always | ☐ |
| [[Resource-Requirements]] | 🔴 | light | T4 | always | ☐ |
| [[Risk-Management-Plan]] | 🔴 | light | T4 | always | ☐ |
| [[Risk-Register]] | 🟡 | record | T2 | Produced by the activity it records · view of `Risk-Management-Plan` | ☐ |
| [[Risk-Report]] | 🟡 | record | T5 | Produced by the activity it records · view of `Risk-Management-Plan` | ☐ |
| [[Schedule-Baseline]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Schedule-Management-Plan]] | 🔴 | light | T4 | always | ☐ |
| [[Schedule-Network-Diagram]] | 🟢 | light | T5 | Used inside parent artifacts when the modeling need arises | ☐ |
| [[Scope-Management-Plan]] | 🔴 | light | T4 | always | ☐ |
| [[Skill-Matrix]] | 🔴 | light | T5 | always | ☐ |
| [[Stakeholder-Engagement-Assessment-Matrix]] | 🔴 | light | T5 | always | ☐ |
| [[Stakeholder-Engagement-Plan]] | 🔴 | light | T4 | always | ☐ |
| [[Team-Charter]] | 🔴 | light | T5 | always | ☐ |
| [[WBS-WBS-Dictionary]] | 🔴 | light | T4 | always | ☐ |

## Project Management Executing and MC

> **Owner:** PM · 14 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Change-Log]] | 🟡 | record | T4 | Produced by the activity it records · view of `Change-Request` | ☐ |
| [[Change-Requests]] | 🟡 | record | T4 | Produced by the activity it records · view of `Change-Request` | ☐ |
| [[Cost-Forecasts]] | 🔴 | light | T4 | always | ☐ |
| [[Earned-Value-Analysis]] | 🔴 | light | T5 | always | ☐ |
| [[Gantt-Chart-Schedule]] | 🔴 | light | T5 | always · view of `Schedule-Management-Plan` | ☐ |
| [[Issue-Log]] | 🟡 | record | T3 | Produced by the activity it records | ☐ |
| [[Lessons-Learned-Register]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Meeting-Minutes]] | 🟡 | record | T3 | Produced by the activity it records | ☐ |
| [[Schedule-Forecasts]] | 🔴 | light | T4 | always | ☐ |
| [[Team-Assignments]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Variance-Analysis-Reports]] | 🟡 | record | T5 | Produced by the activity it records | ☐ |
| [[Work-Performance-Data]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Work-Performance-Information]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Work-Performance-Reports]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |

## Project Management Closing

> **Owner:** PM · 3 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Final-Report]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Project-Closure-Document]] | 🔴 | light | T4 | always | ☐ |
| [[Verified-Deliverables]] | 🔴 | light | T4 | always | ☐ |

## Procurement and Contracts

> **Owner:** PM / Procurement · 6 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Bid-Documents]] | 🟡 | light | T4 | External supplier, COTS, SaaS, or contract work involved | ☐ |
| [[Contract-Agreement]] | 🟡 | light | T4 | External supplier, COTS, SaaS, or contract work involved | ☐ |
| [[Procurement-Management-Plan]] | 🟡 | light | T4 | External supplier, COTS, SaaS, or contract work involved | ☐ |
| [[Procurement-Statement-of-Work]] | 🟡 | light | T4 | External supplier, COTS, SaaS, or contract work involved | ☐ |
| [[Source-Selection-Criteria]] | 🟡 | light | T4 | External supplier, COTS, SaaS, or contract work involved | ☐ |
| [[Sourcing-Strategy-Plan]] | 🟡 | light | T4 | External supplier, COTS, SaaS, or contract work involved | ☐ |

## Systems Architecture and Design

> **Owner:** Architect · 20 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[ASR-Catalog]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Architecture-Decision-Records]] | 🔴 | light | T2 | always | ☐ |
| [[Architecture-Evaluation-Report]] | 🟡 | record | T5 | Produced by the activity it records | ☐ |
| [[Architecture-Metrics-Report]] | 🟡 | record | T5 | Produced by the activity it records | ☐ |
| [[Architecture-Patterns-Catalog]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Architecture-Views-4-1]] | 🔴 | light | T4 | always | ☐ |
| [[CC-Views]] | 🔴 | light | T4 | always | ☐ |
| [[Digital-Twin-Specification]] | 🟡 | light | T7 🆕 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[Functional-Architecture]] | 🔴 | light | T4 | always | ☐ |
| [[Interface-Control-Document]] | 🔴 | light | T4 | always | ☐ |
| [[Logical-Architecture]] | 🔴 | light | T4 | always | ☐ |
| [[MBSE-Models]] | 🟡 | light | T6 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[Module-Views]] | 🔴 | light | T5 | always | ☐ |
| [[Physical-Architecture]] | 🔴 | light | T4 | always | ☐ |
| [[QAW-Report]] | 🟡 | record | T6 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[Reference-Architecture]] | 🟡 | light | T6 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[Software-Architecture-Document]] | 🔴 | light | T4 | always | ☐ |
| [[System-Architecture-Description]] | 🔴 | light | T4 | always | ☐ |
| [[System-Bill-of-Materials]] | 🔴 | light | T5 | always | ☐ |
| [[Trade-Study-Reports]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |

## Software Design

> **Owner:** Architect / Sr. Dev · 19 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[API-Specification]] | 🔴 | light | T4 | always | ☐ |
| [[Activity-Diagrams-Design]] | 🟢 | light | T4 | Used inside parent artifacts when the modeling need arises | ☐ |
| [[Architecture-Overview]] | 🔴 | light | T4 | always | ☐ |
| [[CRC-Cards]] | 🟡 | record | T5 | Produced by the activity it records | ☐ |
| [[Class-Diagrams]] | 🟢 | light | T4 | Used inside parent artifacts when the modeling need arises | ☐ |
| [[Communication-Diagrams]] | 🟢 | light | T5 | Used inside parent artifacts when the modeling need arises | ☐ |
| [[Component-Diagrams]] | 🟢 | light | T5 | Used inside parent artifacts when the modeling need arises | ☐ |
| [[Data-Dictionary]] | 🔴 | light | T4 | always | ☐ |
| [[Database-Schema-DDL]] | 🔴 | light | T4 | always | ☐ |
| [[Deployment-Diagrams]] | 🟢 | light | T5 | Used inside parent artifacts when the modeling need arises | ☐ |
| [[Design-Pattern-Catalog]] | 🟡 | record | T5 | Produced by the activity it records | ☐ |
| [[Design-Rationale]] | 🔴 | light | T5 | always | ☐ |
| [[Design-Review-Records]] | 🔴 | light | T4 | always | ☐ |
| [[ERD]] | 🔴 | light | T4 | always | ☐ |
| [[High-Level-Design]] | 🔴 | light | T4 | always | ☐ |
| [[Low-Level-Design]] | 🔴 | light | T4 | always | ☐ |
| [[Pseudocode-PDL]] | 🟢 | light | T5 | Used inside parent artifacts when the modeling need arises | ☐ |
| [[Sequence-Diagrams]] | 🟢 | light | T5 | Used inside parent artifacts when the modeling need arises | ☐ |
| [[State-Diagrams]] | 🟢 | light | T5 | Used inside parent artifacts when the modeling need arises | ☐ |

## UX UI Design

> **Owner:** Designer / UX · 35 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[AB-Test-Plan]] | 🟡 | light | T5 | Product has a user-facing interface | ☐ |
| [[Accessibility-Audit]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[Analytics-Dashboard-Spec]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[Asset-Export-Package]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[Brand-Guidelines]] | 🟡 | light | T5 | Product has a user-facing interface | ☐ |
| [[Competitive-Analysis]] | 🟡 | light | T5 | Product has a user-facing interface | ☐ |
| [[Component-Library]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[Content-Inventory]] | 🟡 | light | T5 | Product has a user-facing interface | ☐ |
| [[Design-Specifications]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[Design-System]] | 🟡 | light | T5 | Product has a user-facing interface | ☐ |
| [[Design-Tokens]] | 🟡 | light | T5 | Product has a user-facing interface | ☐ |
| [[Empathy-Map]] | 🟡 | light | T5 | Product has a user-facing interface | ☐ |
| [[Empty-State-Designs]] | 🟡 | light | T5 | Product has a user-facing interface | ☐ |
| [[Error-State-Specifications]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[Heatmap-Report]] | 🟡 | record | T5 | Product has a user-facing interface | ☐ |
| [[Icon-Library]] | 🟡 | light | T5 | Product has a user-facing interface | ☐ |
| [[Information-Architecture]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[Interaction-Specifications]] | 🟡 | light | T5 | Product has a user-facing interface | ☐ |
| [[Interactive-Prototype]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[Journey-Map]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[Responsive-Behavior-Spec]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[Responsive-Specifications]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[Sitemap]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[State-Variations]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[Style-Guide]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[Survey-Questionnaire]] | 🟡 | light | T5 | Product has a user-facing interface | ☐ |
| [[UI-Mockups]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[Usability-Test-Plan]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[Usability-Test-Report]] | 🟡 | record | T4 | Product has a user-facing interface | ☐ |
| [[User-Flows]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[User-Interview-Script]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[User-Personas]] | 🟡 | light | T4 | Product has a user-facing interface | ☐ |
| [[User-Research-Report]] | 🟡 | record | T4 | Product has a user-facing interface | ☐ |
| [[Wireframes-Low-fi]] | 🟡 | light | T5 | Product has a user-facing interface | ☐ |
| [[Wireframes-Mid-fi]] | 🟡 | light | T5 | Product has a user-facing interface | ☐ |

## Construction

> **Owner:** Developer · 11 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[API-Documentation]] | 🔴 | light | T4 | always | ☐ |
| [[Build-Scripts]] | 🔴 | light | T4 | always | ☐ |
| [[Code-Review-Records]] | 🔴 | light | T4 | always | ☐ |
| [[Coding-Standards]] | 🔴 | light | T3 | always | ☐ |
| [[Commit-Messages-Changelog]] | 🟡 | record | T3 | Produced by the activity it records | ☐ |
| [[Dependency-Manifest]] | 🔴 | light | T3 | always | ☐ |
| [[Mock-Stub-Driver-Specifications]] | 🔴 | light | T5 | always | ☐ |
| [[README-Developer-Guide]] | 🔴 | light | T3 | always | ☐ |
| [[SBOM]] | 🟡 | light | T5 | Distribution, supply-chain, contractual, or regulatory requirement | ☐ |
| [[Static-Analysis-Reports]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[TDD-Test-Cases]] | 🔴 | light | T5 | always | ☐ |

## Testing and Verification

> **Owner:** QA · 19 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Coverage-Report]] | 🟡 | record | T5 | Produced by the activity it records | ☐ |
| [[Defect-Report]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Performance-Test-Report]] | 🟡 | record | T5 | Produced by the activity it records | ☐ |
| [[Regression-Test-Suite]] | 🔴 | light | T4 | always | ☐ |
| [[Security-Test-Report]] | 🟡 | record | T5 | Produced by the activity it records | ☐ |
| [[Test-Cases]] | 🔴 | light | T4 | always | ☐ |
| [[Test-Completion-Report]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Test-Data]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Test-Plan]] | 🔴 | light | T4 | always | ☐ |
| [[Test-Report]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Test-Scripts-Automated]] | 🔴 | light | T4 | always | ☐ |
| [[Test-Strategy]] | 🔴 | light | T4 | always | ☐ |
| [[Test-Suite]] | 🔴 | light | T4 | always | ☐ |
| [[Traceability-Matrix-Req-Tests]] | 🔴 | light | T4 | always · view of `Requirements-Traceability-Matrix` | ☐ |
| [[UAT-Sign-off]] | 🟡 | light | T4 | Separate business acceptance role exists; otherwise record acceptance decision elsewhere | ☐ |
| [[Validation-Plan]] | 🔴 | light | T4 | always | ☐ |
| [[Validation-Reports]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Verification-Plan]] | 🔴 | light | T4 | always | ☐ |
| [[Verification-Reports]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |

## Security

> **Owner:** Security · 26 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Abuse-Misuse-Cases]] | 🟡 | light | T5 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Access-Control-Policy]] | 🟡 | light | T4 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Adversary-Emulation-Plan]] | 🟡 | light | T6 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[Authentication-Standard]] | 🟡 | light | T4 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Business-Continuity-Plan-BCP]] | 🟡 | light | T5 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Compliance-Assessment-Report]] | 🟡 | record | T4 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[DAST-Report]] | 🟡 | record | T5 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[DevSecOps-Pipeline-Configuration]] | 🟡 | light | T5 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Digital-Forensics-Report]] | 🟡 | record | T6 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[ISMS-Documentation]] | 🟡 | light | T6 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[Incident-Response-Plan]] | 🟡 | light | T4 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Network-Security-Architecture]] | 🟡 | light | T4 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Penetration-Test-Report]] | 🟡 | record | T5 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Risk-Assessment-Report-Security]] | 🟡 | light | T5 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Risk-Treatment-Plan]] | 🟡 | light | T4 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[SAST-Report]] | 🟡 | record | T4 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[SCA-Report]] | 🟡 | record | T4 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[SSDLC-Process-Documentation]] | 🟡 | light | T4 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Secure-Coding-Guidelines]] | 🔴 | light | T4 | always | ☐ |
| [[Secure-Design-Review-Report]] | 🟡 | record | T4 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Security-Architecture]] | 🟡 | light | T4 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Security-Metrics-Dashboard]] | 🟡 | record | T4 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |
| [[Security-Policy]] | 🔴 | light | T4 | always | ☐ |
| [[Security-Requirements-Specification]] | 🔴 | light | T4 | always | ☐ |
| [[Threat-Model]] | 🔴 | light | T4 | always | ☐ |
| [[Vulnerability-Management-Report]] | 🟡 | record | T5 | Exposure, data sensitivity, supplier/distribution, or regulation triggers | ☐ |

## Data Management

> **Owner:** Data / DBA · 59 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[API-Data-Contract]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Analytics-Governance-Policy]] | 🟡 | light | T5 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[BI-Semantic-Layer-Definition]] | 🟡 | light | T6 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Backup-Recovery-Plan]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Business-Glossary]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Capacity-Plan-Data]] | 🟡 | record | T5 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Conceptual-Data-Model-CDM]] | 🟡 | light | T5 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Content-Classification-Taxonomy]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Access-Control-Policy]] | 🟡 | light | T5 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Architecture-Blueprint]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Asset-Valuation-Report]] | 🟡 | record | T6 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Breach-Response-Plan]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Catalog]] | 🟡 | record | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Classification-Schema]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Cleansing-Specification]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Encryption-Standards]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Flow-Diagram]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Governance-Charter]] | 🟡 | light | T5 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Governance-Operating-Framework]] | 🟡 | light | T5 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Governance-Strategy]] | 🟡 | light | T5 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Integration-Architecture]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Interface-Agreement-DIA]] | 🟡 | light | T5 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Lineage-Documentation]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Management-Maturity-Assessment]] | 🟡 | light | T6 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Masking-Anonymization-Rules]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Model-Review-Records]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Model-Scorecard]] | 🟡 | record | T5 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Modeling-Standards]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Policy]] | 🟡 | light | T5 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Profiling-Report]] | 🟡 | record | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Quality-Issue-Log]] | 🟡 | record | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Quality-Rules]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Quality-Scorecard]] | 🟡 | record | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Quality-Strategy]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Replication-Synchronization-Spec]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Retention-Archival-Policy]] | 🟡 | light | T5 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Security-Audit-Report]] | 🟡 | record | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Standards]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Stewardship-Assignment]] | 🟡 | light | T5 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Technology-Roadmap]] | 🟡 | light | T5 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Virtualization-Specification]] | 🟡 | light | T6 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Data-Warehouse-Architecture]] | 🟡 | light | T6 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Database-Operational-Runbook]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Dimensional-Model]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[ECM-Strategy]] | 🟡 | light | T6 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[ETL-ELT-Specification]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Enterprise-Data-Model-EDM]] | 🟡 | light | T6 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Golden-Record-Definition]] | 🟡 | light | T6 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[High-Availability-DR-Configuration]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Logical-Data-Model-LDM]] | 🟡 | light | T5 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[MDM-Strategy]] | 🟡 | light | T6 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Metadata-Repository]] | 🟡 | light | T6 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Metadata-Standards]] | 🟡 | light | T6 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Physical-Data-Model-PDM]] | 🟡 | light | T5 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Privacy-Impact-Assessment]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Records-Retention-Schedule]] | 🟡 | light | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Reference-Data-Catalog]] | 🟡 | record | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Regulatory-Compliance-Register]] | 🟡 | record | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |
| [[Report-Dashboard-Catalog]] | 🟡 | record | T4 | Shared/master data, analytics workload, regulated or personal data | ☐ |

## Deployment and Operations

> **Owner:** DevOps / SRE · 14 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[CI-CD-Pipeline-Configuration]] | 🔴 | light | T3 | always | ☐ |
| [[Capacity-Plan]] | 🔴 | light | T5 | always | ☐ |
| [[Container-Configurations]] | 🔴 | light | T5 | always | ☐ |
| [[Deployment-Plan]] | 🔴 | light | T4 | always | ☐ |
| [[Disaster-Recovery-Plan]] | 🟡 | light | T4 | Meaningful availability or data-loss requirement | ☐ |
| [[Incident-Management-Process]] | 🔴 | light | T4 | always | ☐ |
| [[Infrastructure-as-Code]] | 🔴 | light | T5 | always | ☐ |
| [[Monitoring-Dashboard-Spec]] | 🔴 | light | T5 | always | ☐ |
| [[Operational-KPIs-Report]] | 🟡 | record | T5 | Produced by the activity it records | ☐ |
| [[Operations-Manual-Runbook]] | 🔴 | light | T4 | always | ☐ |
| [[Release-Notes]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Rollback-Plan]] | 🔴 | light | T4 | always | ☐ |
| [[SLA]] | 🟡 | light | T5 | Software operated as a service with external commitments | ☐ |
| [[SLO-SLI-Definitions]] | 🔴 | light | T5 | always | ☐ |

## Maintenance and Support

> **Owner:** Support / Maintainer · 9 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Impact-Analysis-Report]] | 🟡 | record | T4 | Produced by the activity it records | ☐ |
| [[Incident-Problem-Reports]] | 🟡 | record | T4 | Produced by the activity it records · view of `Incident-Management-Process` | ☐ |
| [[Logistics-Plan]] | 🟡 | light | T6 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[Maintenance-Log-Change-History]] | 🔴 | light | T4 | always | ☐ |
| [[Maintenance-Metrics-Dashboard]] | 🟡 | record | T5 | Produced by the activity it records | ☐ |
| [[Maintenance-Plan]] | 🔴 | light | T4 | always | ☐ |
| [[Modification-Request]] | 🔴 | light | T5 | always · view of `Change-Request` | ☐ |
| [[SLA-Compliance-Report]] | 🟡 | record | T5 | Produced by the activity it records | ☐ |
| [[Technical-Debt-Register]] | 🟡 | record | T5 | Produced by the activity it records | ☐ |

## Quality Assurance

> **Owner:** QA Lead · 11 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Audit-Reports]] | 🟡 | record | T6 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[Defect-Log-Metrics]] | 🔴 | record | T4 | always | ☐ |
| [[FMEA-FTA-Reports]] | 🟡 | record | T7 🆕 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[Integrity-Level-Assignments]] | 🟡 | record | T7 🆕 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[Process-Assessment-Report]] | 🟡 | record | T6 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[QMS-Documentation]] | 🟡 | light | T6 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[Quality-Metrics-Dashboard]] | 🟡 | record | T5 | Formal QA program or certification context | ☐ |
| [[RCA-Reports]] | 🟡 | record | T5 | Formal QA program or certification context · view of `Incident-Management-Process` | ☐ |
| [[Review-Records]] | 🔴 | light | T4 | always | ☐ |
| [[SQAP]] | 🔴 | light | T4 | always | ☐ |
| [[VandV-Plan]] | 🔴 | light | T5 | always | ☐ |

## Configuration Management

> **Owner:** Config Manager · 9 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Baseline-Records]] | 🔴 | light | T4 | always | ☐ |
| [[Change-Request]] | 🔴 | light | T4 | always | ☐ |
| [[Configuration-Management-Plan]] | 🟡 | light | T4 | Formal configuration management / contractual CM required | ☐ |
| [[Configuration-Status-Accounting-Reports]] | 🟡 | record | T5 | Formal configuration management / contractual CM required | ☐ |
| [[Deviation-Waiver-Records]] | 🟡 | light | T7 🆕 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[FCA-Report]] | 🟡 | record | T7 🆕 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[PCA-Report]] | 🟡 | record | T7 🆕 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[SCMP]] | 🔴 | light | T4 | always | ☐ |
| [[Version-Description-Document]] | 🔴 | light | T4 | always | ☐ |

## SE Cross Cutting

> **Owner:** Systems Engineer · 18 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[As-Built-Documentation]] | 🟡 | light | T7 🆕 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[Capability-Upgrade-Plan]] | 🟡 | light | T5 | Multi-team / systems-engineering lifecycle context | ☐ |
| [[Decision-Records]] | 🔴 | light | T2 | always | ☐ |
| [[Hazard-Analysis-PHA-SHA-SSHA]] | 🟡 | light | T7 🆕 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[Implementation-Plan]] | 🔴 | light | T4 | always | ☐ |
| [[Integration-Plan-Reports]] | 🟡 | record | T4 | Multi-team / systems-engineering lifecycle context | ☐ |
| [[Measurement-Plan]] | 🔴 | light | T3 | always | ☐ |
| [[Risk-Burn-Down-Chart]] | 🟡 | light | T5 | Multi-team / systems-engineering lifecycle context | ☐ |
| [[SE-Performance-Dashboard]] | 🟡 | record | T5 | Multi-team / systems-engineering lifecycle context | ☐ |
| [[SEMP]] | 🟡 | light | T7 🆕 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[Security-Plan]] | 🔴 | light | T4 | always | ☐ |
| [[Standards-Compliance-Matrix]] | 🟡 | light | T7 🆕 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[System-Disposal-Retirement-Plan]] | 🟡 | light | T7 🆕 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[System-Safety-Plan]] | 🟡 | light | T7 🆕 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[Tailoring-Justification]] | 🔴 | light | T1 | always | ☐ |
| [[Technical-Performance-Measures]] | 🟡 | light | T7 🆕 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[Technical-Review-Records]] | 🟡 | light | T7 🆕 | Systems-engineering, safety, formal-assurance, or organization-program context | ☐ |
| [[Transition-Plan]] | 🟡 | light | T4 | Multi-team / systems-engineering lifecycle context | ☐ |

## Solution Evaluation

> **Owner:** BA / PO · 5 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Enterprise-Limitation]] | 🔴 | light | T4 | always | ☐ |
| [[Recommended-Actions]] | 🔴 | light | T4 | always | ☐ |
| [[Solution-Limitation]] | 🔴 | light | T4 | always | ☐ |
| [[Solution-Performance-Analysis]] | 🔴 | light | T4 | always | ☐ |
| [[Solution-Performance-Measures]] | 🔴 | light | T4 | always | ☐ |

## Domain Specific

> **Owner:** Domain Owner · 4 artifacts

| Document | Priority | Form | Applies at tier | Trigger / Note | Status |
|---|---|---|---|---|---|
| [[Medical-Device-File]] | 🟡 | light | T7 🆕 | Regulated/domain-specific context (medical, gov/defense, safety) | ☐ |
| [[Security-Accreditation-Package]] | 🟡 | light | T7 🆕 | Regulated/domain-specific context (medical, gov/defense, safety) | ☐ |
| [[Software-Assurance-Plan]] | 🟡 | light | T7 🆕 | Regulated/domain-specific context (medical, gov/defense, safety) | ☐ |
| [[Software-Development-Plan]] | 🟡 | light | T7 🆕 | Regulated/domain-specific context (medical, gov/defense, safety) | ☐ |

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
