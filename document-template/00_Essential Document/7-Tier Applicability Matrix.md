---
tags: [tier-matrix, applicability, schema-v2, spec-driven, tailoring]
status: Generated — regenerate from frontmatter, never hand-edit
created: 2026-09-24
generator: phase3 mapping script (source of truth = template frontmatter)
tier_model: "checklist/release-checklist/release.md — Project Tier Scoping Matrix (7 tiers)"
---

# 7-Tier Applicability Matrix

> Generated view over the `schema_version: 2` frontmatter of all 363 templates. **Source of truth is each template's frontmatter** — regenerate this file, don't hand-edit.
> Tiers per [[release]] §Project Tier Scoping Matrix: 🧪 POC → 🔧 Prototype → 🏠 Internal → 🟢 Small-Prod → 🔵 Medium-Prod → 🟣 Prod-Grade → 🔴 Mission-Crit.
> `min_project_tier` = the lowest project tier at which this artifact becomes required/expected. Below that tier: skip (over-engineering).

## How to tailor a project

1. Determine your tier from the decision flow in `release.md` §"Which Tier Am I?".
2. Take every template with `min_project_tier <= your tier` AND (`applicability: universal` OR its `tier_trigger` condition is true).
3. `applicability: evidence` artifacts are produced by activities — create the record when the activity happens, not upfront.
4. `applicability: technique` artifacts live inside parent documents — never standalone paperwork.
5. Record omissions/combinations in the project's Tailoring-Justification (min tier 1 — always).

## Distribution

| Tier (min) | Count |
|---|---|
| 1 🧪 POC | 1 |
| 2 🔧 Prototype | 5 |
| 3 🏠 Internal | 15 |
| 4 🟢 Small-Prod | 207 |
| 5 🔵 Medium-Prod | 95 |
| 6 🟣 Prod-Grade | 21 |
| 7 🔴 Mission-Crit | 19 |

| Applicability | Count |
|---|---|
| conditional | 161 |
| universal | 133 |
| evidence | 55 |
| technique | 14 |

| Doc form | Count |
|---|---|
| light | 236 |
| record | 92 |
| heavy | 35 |

> Cumulative reading: a 🟢 Small-Prod project uses everything at tiers 1–4 ≈ 228 artifacts before conditional triggers; a 🧪 POC uses ≈ 1.

## 01_Business_Analysis_and_strategy

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Business-Objectives]] | 3 | universal | heavy |  |
| [[Benefits-Management-Plan]] | 4 | universal | heavy |  |
| [[Business-Analysis-Approach]] | 4 | universal | heavy |  |
| [[Business-Case]] | 4 | universal | heavy |  |
| [[Business-Requirements]] | 4 | universal | heavy | ⟷ `business-requirements` → SoT `01_Business_Analysis_and_strategy/Business-Requirements.md` |
| [[Change-Strategy]] | 4 | universal | heavy |  |
| [[Current-State-Description]] | 4 | universal | heavy |  |
| [[Future-State-Description]] | 4 | universal | heavy |  |
| [[Governance-Approach]] | 4 | universal | heavy |  |
| [[Information-Management-Approach]] | 4 | universal | heavy |  |
| [[Potential-Value]] | 4 | universal | heavy |  |
| [[Risk-Analysis-Results]] | 4 | evidence | record | Produced by the activity it records · ⟷ `risk` → SoT `05_Project_Management_Planning/Risk-Management-Plan.md` |
| [[Solution-Recommendation]] | 4 | universal | heavy |  |
| [[Solution-Scope]] | 4 | universal | heavy |  |
| [[BA-Performance-Assessment]] | 5 | universal | heavy |  |
| [[Design-Options]] | 5 | universal | heavy |  |
| [[Enterprise-Readiness-Assessment]] | 5 | universal | heavy |  |
| [[Gap-Analysis]] | 5 | universal | heavy |  |

## 02_Elicitation_and_Collaboration

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Elicitation-Results-Confirmed]] | 3 | evidence | record | Produced by the activity it records |
| [[Elicitation-Results-Unconfirmed]] | 3 | evidence | record | Produced by the activity it records |
| [[Elicitation-Activity-Plan]] | 4 | universal | heavy |  |
| [[Stakeholder-Engagement-Approach]] | 4 | universal | heavy |  |

## 03_Concept_and_Mission_Definition

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Concept-of-Operations]] | 4 | universal | heavy |  |
| [[Mission-Analysis-Report]] | 4 | evidence | record | Produced by the activity it records |
| [[Stakeholder-Needs-Document]] | 4 | universal | heavy |  |
| [[Stakeholder-Register]] | 4 | evidence | record | Produced by the activity it records |
| [[System-Requirements-Specification]] | 4 | universal | heavy | ⟷ `requirements-spec` → SoT `03_Concept_and_Mission_Definition/System-Requirements-Specification.md` |
| [[Feasibility-Study]] | 5 | universal | heavy |  |
| [[Market-Analysis-Technology-Assessment]] | 5 | universal | heavy |  |

## 04_Requirements_Engineering

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Assumption-Log]] | 2 | evidence | record | Produced by the activity it records |
| [[Definition-of-done]] | 2 | universal | light |  |
| [[Acceptance-Criteria]] | 3 | universal | heavy |  |
| [[Nonfunctional-Requirements-Catalog]] | 3 | evidence | record | Produced by the activity it records |
| [[User-Stories]] | 3 | universal | heavy |  |
| [[Business-Requirements-Document]] | 4 | universal | heavy | ⟷ `business-requirements` → SoT `01_Business_Analysis_and_strategy/Business-Requirements.md` |
| [[Decision-Tables-Trees]] | 4 | technique | light | Used inside parent artifacts when the modeling need arises |
| [[Project-Scope-Statement]] | 4 | universal | heavy |  |
| [[Requirements-Architecture]] | 4 | universal | heavy |  |
| [[Requirements-Change-Assessment]] | 4 | universal | light | ⟷ `change-control` → SoT `19_Configuration_Management/Change-Request.md` |
| [[Requirements-Change-Log]] | 4 | evidence | record | Produced by the activity it records · ⟷ `change-control` → SoT `19_Configuration_Management/Change-Request.md` |
| [[Requirements-Traceability-Matrix]] | 4 | universal | heavy | ⟷ `traceability` → SoT `04_Requirements_Engineering/Requirements-Traceability-Matrix.md` |
| [[Requirements-Validated]] | 4 | evidence | record | Produced by the activity it records |
| [[Requirements-Verified]] | 4 | evidence | record | Produced by the activity it records |
| [[Software-Requirements-Specification]] | 4 | universal | heavy | ⟷ `requirements-spec` → SoT `04_Requirements_Engineering/Software-Requirements-Specification.md` |
| [[Stakeholder-Analysis]] | 4 | universal | heavy |  |
| [[Activity-Diagrams]] | 5 | technique | light | Used inside parent artifacts when the modeling need arises |
| [[Functional-Size-Measurement]] | 5 | technique | light | Used inside parent artifacts when the modeling need arises |
| [[Prototypes]] | 5 | technique | heavy | Used inside parent artifacts when the modeling need arises |
| [[Use-Case-Specifications]] | 5 | technique | heavy | Used inside parent artifacts when the modeling need arises |

## 05_Project_Management_Planning

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Risk-Register]] | 2 | evidence | record | Produced by the activity it records · ⟷ `risk` → SoT `05_Project_Management_Planning/Risk-Management-Plan.md` |
| [[Project-Charter]] | 3 | universal | light |  |
| [[Activity-List]] | 4 | evidence | record | Produced by the activity it records |
| [[Communications-Management-Plan]] | 4 | universal | light |  |
| [[Cost-Baseline]] | 4 | evidence | record | Produced by the activity it records |
| [[Cost-Estimates]] | 4 | universal | light |  |
| [[Financial-Management-Plan]] | 4 | universal | light |  |
| [[Milestone-List]] | 4 | evidence | record | Produced by the activity it records · ⟷ `schedule` → SoT `05_Project_Management_Planning/Schedule-Management-Plan.md` |
| [[Project-Funding-Requirements]] | 4 | universal | light |  |
| [[Project-Management-Plan]] | 4 | universal | heavy |  |
| [[Project-Schedule]] | 4 | universal | light | ⟷ `schedule` → SoT `05_Project_Management_Planning/Schedule-Management-Plan.md` |
| [[Quality-Management-Plan]] | 4 | universal | light |  |
| [[Quality-Metrics]] | 4 | evidence | record | Produced by the activity it records |
| [[RACI-Matrix]] | 4 | universal | light |  |
| [[Resource-Management-Plan]] | 4 | universal | light |  |
| [[Resource-Requirements]] | 4 | universal | light |  |
| [[Risk-Management-Plan]] | 4 | universal | light | ⟷ `risk` → SoT `05_Project_Management_Planning/Risk-Management-Plan.md` |
| [[Schedule-Baseline]] | 4 | evidence | record | Produced by the activity it records |
| [[Schedule-Management-Plan]] | 4 | universal | light | ⟷ `schedule` → SoT `05_Project_Management_Planning/Schedule-Management-Plan.md` |
| [[Scope-Management-Plan]] | 4 | universal | light |  |
| [[Stakeholder-Engagement-Plan]] | 4 | universal | light |  |
| [[WBS-WBS-Dictionary]] | 4 | universal | light |  |
| [[Basis-of-Estimates]] | 5 | universal | light |  |
| [[Resource-Breakdown-Structure]] | 5 | universal | light |  |
| [[Risk-Report]] | 5 | evidence | record | Produced by the activity it records · ⟷ `risk` → SoT `05_Project_Management_Planning/Risk-Management-Plan.md` |
| [[Schedule-Network-Diagram]] | 5 | technique | light | Used inside parent artifacts when the modeling need arises |
| [[Skill-Matrix]] | 5 | universal | light |  |
| [[Stakeholder-Engagement-Assessment-Matrix]] | 5 | universal | light |  |
| [[Team-Charter]] | 5 | universal | light |  |

## 06_Project_Management_Executing_and_MC

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Issue-Log]] | 3 | evidence | record | Produced by the activity it records |
| [[Meeting-Minutes]] | 3 | evidence | record | Produced by the activity it records |
| [[Change-Log]] | 4 | evidence | record | Produced by the activity it records · ⟷ `change-control` → SoT `19_Configuration_Management/Change-Request.md` |
| [[Change-Requests]] | 4 | evidence | record | Produced by the activity it records · ⟷ `change-control` → SoT `19_Configuration_Management/Change-Request.md` |
| [[Cost-Forecasts]] | 4 | universal | light |  |
| [[Lessons-Learned-Register]] | 4 | evidence | record | Produced by the activity it records |
| [[Schedule-Forecasts]] | 4 | universal | light |  |
| [[Team-Assignments]] | 4 | evidence | record | Produced by the activity it records |
| [[Work-Performance-Data]] | 4 | evidence | record | Produced by the activity it records |
| [[Work-Performance-Information]] | 4 | evidence | record | Produced by the activity it records |
| [[Work-Performance-Reports]] | 4 | evidence | record | Produced by the activity it records |
| [[Earned-Value-Analysis]] | 5 | universal | light |  |
| [[Gantt-Chart-Schedule]] | 5 | universal | light | ⟷ `schedule` → SoT `05_Project_Management_Planning/Schedule-Management-Plan.md` |
| [[Variance-Analysis-Reports]] | 5 | evidence | record | Produced by the activity it records |

## 07_Project_Management_Closing

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Final-Report]] | 4 | evidence | record | Produced by the activity it records |
| [[Project-Closure-Document]] | 4 | universal | light |  |
| [[Verified-Deliverables]] | 4 | universal | light |  |

## 08_Procurement_and_Contracts

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Bid-Documents]] | 4 | conditional | light | External supplier, COTS, SaaS, or contract work involved |
| [[Contract-Agreement]] | 4 | conditional | light | External supplier, COTS, SaaS, or contract work involved |
| [[Procurement-Management-Plan]] | 4 | conditional | light | External supplier, COTS, SaaS, or contract work involved |
| [[Procurement-Statement-of-Work]] | 4 | conditional | light | External supplier, COTS, SaaS, or contract work involved |
| [[Source-Selection-Criteria]] | 4 | conditional | light | External supplier, COTS, SaaS, or contract work involved |
| [[Sourcing-Strategy-Plan]] | 4 | conditional | light | External supplier, COTS, SaaS, or contract work involved |

## 09_Systems_Architecture_and_Design

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Architecture-Decision-Records]] | 2 | universal | light |  |
| [[ASR-Catalog]] | 4 | evidence | record | Produced by the activity it records |
| [[Architecture-Patterns-Catalog]] | 4 | evidence | record | Produced by the activity it records |
| [[Architecture-Views-4-1]] | 4 | universal | light |  |
| [[CC-Views]] | 4 | universal | light |  |
| [[Functional-Architecture]] | 4 | universal | light |  |
| [[Interface-Control-Document]] | 4 | universal | light |  |
| [[Logical-Architecture]] | 4 | universal | light |  |
| [[Physical-Architecture]] | 4 | universal | light |  |
| [[Software-Architecture-Document]] | 4 | universal | light |  |
| [[System-Architecture-Description]] | 4 | universal | light |  |
| [[Trade-Study-Reports]] | 4 | evidence | record | Produced by the activity it records |
| [[Architecture-Evaluation-Report]] | 5 | evidence | record | Produced by the activity it records |
| [[Architecture-Metrics-Report]] | 5 | evidence | record | Produced by the activity it records |
| [[Module-Views]] | 5 | universal | light |  |
| [[System-Bill-of-Materials]] | 5 | universal | light |  |
| [[MBSE-Models]] | 6 | conditional | light | Systems-engineering, safety, formal-assurance, or organization-program context |
| [[QAW-Report]] | 6 | conditional | record | Systems-engineering, safety, formal-assurance, or organization-program context |
| [[Reference-Architecture]] | 6 | conditional | light | Systems-engineering, safety, formal-assurance, or organization-program context |
| [[Digital-Twin-Specification]] | 7 | conditional | light | Systems-engineering, safety, formal-assurance, or organization-program context |

## 10_Software_Design

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[API-Specification]] | 4 | universal | light |  |
| [[Activity-Diagrams-Design]] | 4 | technique | light | Used inside parent artifacts when the modeling need arises |
| [[Architecture-Overview]] | 4 | universal | light |  |
| [[Class-Diagrams]] | 4 | technique | light | Used inside parent artifacts when the modeling need arises |
| [[Data-Dictionary]] | 4 | universal | light |  |
| [[Database-Schema-DDL]] | 4 | universal | light |  |
| [[Design-Review-Records]] | 4 | universal | light |  |
| [[ERD]] | 4 | universal | light |  |
| [[High-Level-Design]] | 4 | universal | light |  |
| [[Low-Level-Design]] | 4 | universal | light |  |
| [[CRC-Cards]] | 5 | evidence | record | Produced by the activity it records |
| [[Communication-Diagrams]] | 5 | technique | light | Used inside parent artifacts when the modeling need arises |
| [[Component-Diagrams]] | 5 | technique | light | Used inside parent artifacts when the modeling need arises |
| [[Deployment-Diagrams]] | 5 | technique | light | Used inside parent artifacts when the modeling need arises |
| [[Design-Pattern-Catalog]] | 5 | evidence | record | Produced by the activity it records |
| [[Design-Rationale]] | 5 | universal | light |  |
| [[Pseudocode-PDL]] | 5 | technique | light | Used inside parent artifacts when the modeling need arises |
| [[Sequence-Diagrams]] | 5 | technique | light | Used inside parent artifacts when the modeling need arises |
| [[State-Diagrams]] | 5 | technique | light | Used inside parent artifacts when the modeling need arises |

## 11_UX_UI_Design

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Accessibility-Audit]] | 4 | conditional | light | Product has a user-facing interface |
| [[Analytics-Dashboard-Spec]] | 4 | conditional | light | Product has a user-facing interface |
| [[Asset-Export-Package]] | 4 | conditional | light | Product has a user-facing interface |
| [[Component-Library]] | 4 | conditional | light | Product has a user-facing interface |
| [[Design-Specifications]] | 4 | conditional | light | Product has a user-facing interface |
| [[Error-State-Specifications]] | 4 | conditional | light | Product has a user-facing interface |
| [[Information-Architecture]] | 4 | conditional | light | Product has a user-facing interface |
| [[Interactive-Prototype]] | 4 | conditional | light | Product has a user-facing interface |
| [[Journey-Map]] | 4 | conditional | light | Product has a user-facing interface |
| [[Responsive-Behavior-Spec]] | 4 | conditional | light | Product has a user-facing interface |
| [[Responsive-Specifications]] | 4 | conditional | light | Product has a user-facing interface |
| [[Sitemap]] | 4 | conditional | light | Product has a user-facing interface |
| [[State-Variations]] | 4 | conditional | light | Product has a user-facing interface |
| [[Style-Guide]] | 4 | conditional | light | Product has a user-facing interface |
| [[UI-Mockups]] | 4 | conditional | light | Product has a user-facing interface |
| [[Usability-Test-Plan]] | 4 | conditional | light | Product has a user-facing interface |
| [[Usability-Test-Report]] | 4 | conditional | record | Product has a user-facing interface |
| [[User-Flows]] | 4 | conditional | light | Product has a user-facing interface |
| [[User-Interview-Script]] | 4 | conditional | light | Product has a user-facing interface |
| [[User-Personas]] | 4 | conditional | light | Product has a user-facing interface |
| [[User-Research-Report]] | 4 | conditional | record | Product has a user-facing interface |
| [[AB-Test-Plan]] | 5 | conditional | light | Product has a user-facing interface |
| [[Brand-Guidelines]] | 5 | conditional | light | Product has a user-facing interface |
| [[Competitive-Analysis]] | 5 | conditional | light | Product has a user-facing interface |
| [[Content-Inventory]] | 5 | conditional | light | Product has a user-facing interface |
| [[Design-System]] | 5 | conditional | light | Product has a user-facing interface |
| [[Design-Tokens]] | 5 | conditional | light | Product has a user-facing interface |
| [[Empathy-Map]] | 5 | conditional | light | Product has a user-facing interface |
| [[Empty-State-Designs]] | 5 | conditional | light | Product has a user-facing interface |
| [[Heatmap-Report]] | 5 | conditional | record | Product has a user-facing interface |
| [[Icon-Library]] | 5 | conditional | light | Product has a user-facing interface |
| [[Interaction-Specifications]] | 5 | conditional | light | Product has a user-facing interface |
| [[Survey-Questionnaire]] | 5 | conditional | light | Product has a user-facing interface |
| [[Wireframes-Low-fi]] | 5 | conditional | light | Product has a user-facing interface |
| [[Wireframes-Mid-fi]] | 5 | conditional | light | Product has a user-facing interface |

## 12_Construction

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Coding-Standards]] | 3 | universal | light |  |
| [[Commit-Messages-Changelog]] | 3 | evidence | record | Produced by the activity it records |
| [[Dependency-Manifest]] | 3 | universal | light |  |
| [[README-Developer-Guide]] | 3 | universal | light |  |
| [[API-Documentation]] | 4 | universal | light |  |
| [[Build-Scripts]] | 4 | universal | light |  |
| [[Code-Review-Records]] | 4 | universal | light |  |
| [[Static-Analysis-Reports]] | 4 | evidence | record | Produced by the activity it records |
| [[Mock-Stub-Driver-Specifications]] | 5 | universal | light |  |
| [[SBOM]] | 5 | conditional | light | Distribution, supply-chain, contractual, or regulatory requirement |
| [[TDD-Test-Cases]] | 5 | universal | light |  |

## 13_Testing_and_Verification

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Defect-Report]] | 4 | evidence | record | Produced by the activity it records |
| [[Regression-Test-Suite]] | 4 | universal | light |  |
| [[Test-Cases]] | 4 | universal | light |  |
| [[Test-Completion-Report]] | 4 | evidence | record | Produced by the activity it records |
| [[Test-Data]] | 4 | evidence | record | Produced by the activity it records |
| [[Test-Plan]] | 4 | universal | light |  |
| [[Test-Report]] | 4 | evidence | record | Produced by the activity it records |
| [[Test-Scripts-Automated]] | 4 | universal | light |  |
| [[Test-Strategy]] | 4 | universal | light |  |
| [[Test-Suite]] | 4 | universal | light |  |
| [[Traceability-Matrix-Req-Tests]] | 4 | universal | light | ⟷ `traceability` → SoT `04_Requirements_Engineering/Requirements-Traceability-Matrix.md` |
| [[UAT-Sign-off]] | 4 | conditional | light | Separate business acceptance role exists; otherwise record acceptance decision elsewhere |
| [[Validation-Plan]] | 4 | universal | light |  |
| [[Validation-Reports]] | 4 | evidence | record | Produced by the activity it records |
| [[Verification-Plan]] | 4 | universal | light |  |
| [[Verification-Reports]] | 4 | evidence | record | Produced by the activity it records |
| [[Coverage-Report]] | 5 | evidence | record | Produced by the activity it records |
| [[Performance-Test-Report]] | 5 | evidence | record | Produced by the activity it records |
| [[Security-Test-Report]] | 5 | evidence | record | Produced by the activity it records |

## 14_Security

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Access-Control-Policy]] | 4 | conditional | light | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[Authentication-Standard]] | 4 | conditional | light | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[Compliance-Assessment-Report]] | 4 | conditional | record | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[Incident-Response-Plan]] | 4 | conditional | light | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[Network-Security-Architecture]] | 4 | conditional | light | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[Risk-Treatment-Plan]] | 4 | conditional | light | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[SAST-Report]] | 4 | conditional | record | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[SCA-Report]] | 4 | conditional | record | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[SSDLC-Process-Documentation]] | 4 | conditional | light | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[Secure-Coding-Guidelines]] | 4 | universal | light |  |
| [[Secure-Design-Review-Report]] | 4 | conditional | record | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[Security-Architecture]] | 4 | conditional | light | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[Security-Metrics-Dashboard]] | 4 | conditional | record | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[Security-Policy]] | 4 | universal | light |  |
| [[Security-Requirements-Specification]] | 4 | universal | light |  |
| [[Threat-Model]] | 4 | universal | light |  |
| [[Abuse-Misuse-Cases]] | 5 | conditional | light | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[Business-Continuity-Plan-BCP]] | 5 | conditional | light | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[DAST-Report]] | 5 | conditional | record | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[DevSecOps-Pipeline-Configuration]] | 5 | conditional | light | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[Penetration-Test-Report]] | 5 | conditional | record | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[Risk-Assessment-Report-Security]] | 5 | conditional | light | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[Vulnerability-Management-Report]] | 5 | conditional | record | Exposure, data sensitivity, supplier/distribution, or regulation triggers |
| [[Adversary-Emulation-Plan]] | 6 | conditional | light | Systems-engineering, safety, formal-assurance, or organization-program context |
| [[Digital-Forensics-Report]] | 6 | conditional | record | Systems-engineering, safety, formal-assurance, or organization-program context |
| [[ISMS-Documentation]] | 6 | conditional | light | Systems-engineering, safety, formal-assurance, or organization-program context |

## 15_Data_Management

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[API-Data-Contract]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Backup-Recovery-Plan]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Business-Glossary]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Content-Classification-Taxonomy]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Architecture-Blueprint]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Breach-Response-Plan]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Catalog]] | 4 | conditional | record | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Classification-Schema]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Cleansing-Specification]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Encryption-Standards]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Flow-Diagram]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Integration-Architecture]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Lineage-Documentation]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Masking-Anonymization-Rules]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Model-Review-Records]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Modeling-Standards]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Profiling-Report]] | 4 | conditional | record | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Quality-Issue-Log]] | 4 | conditional | record | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Quality-Rules]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Quality-Scorecard]] | 4 | conditional | record | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Quality-Strategy]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Replication-Synchronization-Spec]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Security-Audit-Report]] | 4 | conditional | record | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Standards]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Database-Operational-Runbook]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Dimensional-Model]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[ETL-ELT-Specification]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[High-Availability-DR-Configuration]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Privacy-Impact-Assessment]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Records-Retention-Schedule]] | 4 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Reference-Data-Catalog]] | 4 | conditional | record | Shared/master data, analytics workload, regulated or personal data |
| [[Regulatory-Compliance-Register]] | 4 | conditional | record | Shared/master data, analytics workload, regulated or personal data |
| [[Report-Dashboard-Catalog]] | 4 | conditional | record | Shared/master data, analytics workload, regulated or personal data |
| [[Analytics-Governance-Policy]] | 5 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Capacity-Plan-Data]] | 5 | conditional | record | Shared/master data, analytics workload, regulated or personal data |
| [[Conceptual-Data-Model-CDM]] | 5 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Access-Control-Policy]] | 5 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Governance-Charter]] | 5 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Governance-Operating-Framework]] | 5 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Governance-Strategy]] | 5 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Interface-Agreement-DIA]] | 5 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Model-Scorecard]] | 5 | conditional | record | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Policy]] | 5 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Retention-Archival-Policy]] | 5 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Stewardship-Assignment]] | 5 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Technology-Roadmap]] | 5 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Logical-Data-Model-LDM]] | 5 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Physical-Data-Model-PDM]] | 5 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[BI-Semantic-Layer-Definition]] | 6 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Asset-Valuation-Report]] | 6 | conditional | record | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Management-Maturity-Assessment]] | 6 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Virtualization-Specification]] | 6 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Data-Warehouse-Architecture]] | 6 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[ECM-Strategy]] | 6 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Enterprise-Data-Model-EDM]] | 6 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Golden-Record-Definition]] | 6 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[MDM-Strategy]] | 6 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Metadata-Repository]] | 6 | conditional | light | Shared/master data, analytics workload, regulated or personal data |
| [[Metadata-Standards]] | 6 | conditional | light | Shared/master data, analytics workload, regulated or personal data |

## 16_Deployment_and_Operations

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[CI-CD-Pipeline-Configuration]] | 3 | universal | light |  |
| [[Deployment-Plan]] | 4 | universal | light |  |
| [[Disaster-Recovery-Plan]] | 4 | conditional | light | Meaningful availability or data-loss requirement |
| [[Incident-Management-Process]] | 4 | universal | light | ⟷ `incident` → SoT `16_Deployment_and_Operations/Incident-Management-Process.md` |
| [[Operations-Manual-Runbook]] | 4 | universal | light |  |
| [[Release-Notes]] | 4 | evidence | record | Produced by the activity it records |
| [[Rollback-Plan]] | 4 | universal | light |  |
| [[Capacity-Plan]] | 5 | universal | light |  |
| [[Container-Configurations]] | 5 | universal | light |  |
| [[Infrastructure-as-Code]] | 5 | universal | light |  |
| [[Monitoring-Dashboard-Spec]] | 5 | universal | light |  |
| [[Operational-KPIs-Report]] | 5 | evidence | record | Produced by the activity it records |
| [[SLA]] | 5 | conditional | light | Software operated as a service with external commitments |
| [[SLO-SLI-Definitions]] | 5 | universal | light |  |

## 17_Maintenance_and_Support

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Impact-Analysis-Report]] | 4 | evidence | record | Produced by the activity it records |
| [[Incident-Problem-Reports]] | 4 | evidence | record | Produced by the activity it records · ⟷ `incident` → SoT `16_Deployment_and_Operations/Incident-Management-Process.md` |
| [[Maintenance-Log-Change-History]] | 4 | universal | light |  |
| [[Maintenance-Plan]] | 4 | universal | light |  |
| [[Maintenance-Metrics-Dashboard]] | 5 | evidence | record | Produced by the activity it records |
| [[Modification-Request]] | 5 | universal | light | ⟷ `change-control` → SoT `19_Configuration_Management/Change-Request.md` |
| [[SLA-Compliance-Report]] | 5 | evidence | record | Produced by the activity it records |
| [[Technical-Debt-Register]] | 5 | evidence | record | Produced by the activity it records |
| [[Logistics-Plan]] | 6 | conditional | light | Systems-engineering, safety, formal-assurance, or organization-program context |

## 18_Quality_Assurance

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Defect-Log-Metrics]] | 4 | universal | record |  |
| [[Review-Records]] | 4 | universal | light |  |
| [[SQAP]] | 4 | universal | light |  |
| [[Quality-Metrics-Dashboard]] | 5 | conditional | record | Formal QA program or certification context |
| [[RCA-Reports]] | 5 | conditional | record | Formal QA program or certification context · ⟷ `incident` → SoT `16_Deployment_and_Operations/Incident-Management-Process.md` |
| [[VandV-Plan]] | 5 | universal | light |  |
| [[Audit-Reports]] | 6 | conditional | record | Systems-engineering, safety, formal-assurance, or organization-program context |
| [[Process-Assessment-Report]] | 6 | conditional | record | Systems-engineering, safety, formal-assurance, or organization-program context |
| [[QMS-Documentation]] | 6 | conditional | light | Systems-engineering, safety, formal-assurance, or organization-program context |
| [[FMEA-FTA-Reports]] | 7 | conditional | record | Systems-engineering, safety, formal-assurance, or organization-program context |
| [[Integrity-Level-Assignments]] | 7 | conditional | record | Systems-engineering, safety, formal-assurance, or organization-program context |

## 19_Configuration_Management

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Baseline-Records]] | 4 | universal | light |  |
| [[Change-Request]] | 4 | universal | light | ⟷ `change-control` → SoT `19_Configuration_Management/Change-Request.md` |
| [[Configuration-Management-Plan]] | 4 | conditional | light | Formal configuration management / contractual CM required |
| [[SCMP]] | 4 | universal | light |  |
| [[Version-Description-Document]] | 4 | universal | light |  |
| [[Configuration-Status-Accounting-Reports]] | 5 | conditional | record | Formal configuration management / contractual CM required |
| [[Deviation-Waiver-Records]] | 7 | conditional | light | Systems-engineering, safety, formal-assurance, or organization-program context |
| [[FCA-Report]] | 7 | conditional | record | Systems-engineering, safety, formal-assurance, or organization-program context |
| [[PCA-Report]] | 7 | conditional | record | Systems-engineering, safety, formal-assurance, or organization-program context |

## 20_SE_Cross_Cutting

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Tailoring-Justification]] | 1 | universal | light |  |
| [[Decision-Records]] | 2 | universal | light |  |
| [[Measurement-Plan]] | 3 | universal | light |  |
| [[Implementation-Plan]] | 4 | universal | light |  |
| [[Integration-Plan-Reports]] | 4 | conditional | record | Multi-team / systems-engineering lifecycle context |
| [[Security-Plan]] | 4 | universal | light |  |
| [[Transition-Plan]] | 4 | conditional | light | Multi-team / systems-engineering lifecycle context |
| [[Capability-Upgrade-Plan]] | 5 | conditional | light | Multi-team / systems-engineering lifecycle context |
| [[Risk-Burn-Down-Chart]] | 5 | conditional | light | Multi-team / systems-engineering lifecycle context |
| [[SE-Performance-Dashboard]] | 5 | conditional | record | Multi-team / systems-engineering lifecycle context |
| [[As-Built-Documentation]] | 7 | conditional | light | Systems-engineering, safety, formal-assurance, or organization-program context |
| [[Hazard-Analysis-PHA-SHA-SSHA]] | 7 | conditional | light | Systems-engineering, safety, formal-assurance, or organization-program context |
| [[SEMP]] | 7 | conditional | light | Systems-engineering, safety, formal-assurance, or organization-program context |
| [[Standards-Compliance-Matrix]] | 7 | conditional | light | Systems-engineering, safety, formal-assurance, or organization-program context |
| [[System-Disposal-Retirement-Plan]] | 7 | conditional | light | Systems-engineering, safety, formal-assurance, or organization-program context |
| [[System-Safety-Plan]] | 7 | conditional | light | Systems-engineering, safety, formal-assurance, or organization-program context |
| [[Technical-Performance-Measures]] | 7 | conditional | light | Systems-engineering, safety, formal-assurance, or organization-program context |
| [[Technical-Review-Records]] | 7 | conditional | light | Systems-engineering, safety, formal-assurance, or organization-program context |

## 21_Solution_Evaluation

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Enterprise-Limitation]] | 4 | universal | light |  |
| [[Recommended-Actions]] | 4 | universal | light |  |
| [[Solution-Limitation]] | 4 | universal | light |  |
| [[Solution-Performance-Analysis]] | 4 | universal | light |  |
| [[Solution-Performance-Measures]] | 4 | universal | light |  |

## 22_Domain_Specific

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Medical-Device-File]] | 7 | conditional | light | Regulated/domain-specific context (medical, gov/defense, safety) |
| [[Security-Accreditation-Package]] | 7 | conditional | light | Regulated/domain-specific context (medical, gov/defense, safety) |
| [[Software-Assurance-Plan]] | 7 | conditional | light | Regulated/domain-specific context (medical, gov/defense, safety) |
| [[Software-Development-Plan]] | 7 | conditional | light | Regulated/domain-specific context (medical, gov/defense, safety) |

## 23_Project_Size

| Template | Tier | Applicability | Form | Trigger / Overlap |
|---|---|---|---|---|
| [[Profile-Medium-Enterprise-Checklist]] | 5 | evidence | record | Produced by the activity it records |
| [[Profile-Small-Startup-Checklist]] | 5 | evidence | record | Produced by the activity it records |
| [[Profile-Large-Safety-Critical-Checklist]] | 7 | conditional | record | Systems-engineering, safety, formal-assurance, or organization-program context |
