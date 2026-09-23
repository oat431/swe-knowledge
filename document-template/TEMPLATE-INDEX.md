---
tags: [master-index, generated, template-library]
status: Generated — do not hand-edit; regenerate from frontmatter
created: 2026-09-24
generator: Phase 4 script — counts computed from disk
supersedes: TEMPLATE-CHECKLIST.md (archived to 99_Archive/)
---

# TEMPLATE-INDEX — Master Index (generated)

> Single source of truth = each template's `schema_version: 2` frontmatter. All counts on this page are **computed from disk at generation time** — they cannot drift.
> Supersedes `TEMPLATE-CHECKLIST.md` (archived; its hand-maintained counts contradicted themselves: 189 vs 357 vs 359).

## Totals

- **Templates on disk:** 360 (excludes `00_Essential Document/` catalog, `23_Project_Size/` checklists, `99_Archive/`, plan/audit docs)
- **Categories:** 22

| min_project_tier | Templates | Cumulative |
|---|---|---|
| 1 🧪 POC | 1 | 1 |
| 2 🔧 Prototype | 5 | 6 |
| 3 🏠 Internal | 15 | 21 |
| 4 🟢 Small-Prod | 207 | 228 |
| 5 🔵 Medium-Prod | 93 | 321 |
| 6 🟣 Prod-Grade | 21 | 342 |
| 7 🔴 Mission-Crit | 18 | 360 |

| Applicability | Count |
|---|---|
| conditional | 160 |
| universal | 133 |
| evidence | 53 |
| technique | 14 |

| Doc form | Count |
|---|---|
| light | 236 |
| record | 89 |
| heavy | 35 |

## Per-tier checklists (23_Project_Size/)

| Tier | Checklist | Artifacts |
|---|---|---|
| 1 🧪 POC | [[Tier-1-POC-Spike-Checklist]] | 1 |
| 2 🔧 Prototype | [[Tier-2-Prototype-MVP-Checklist]] | 6 |
| 3 🏠 Internal | [[Tier-3-Internal-Tool-Checklist]] | 21 |
| 4 🟢 Small-Prod | [[Tier-4-Small-Production-Checklist]] | 228 |
| 5 🔵 Medium-Prod | [[Tier-5-Medium-Production-Checklist]] | 321 |
| 6 🟣 Prod-Grade | [[Tier-6-Production-Grade-Checklist]] | 342 |
| 7 🔴 Mission-Crit | [[Tier-7-Mission-Critical-Checklist]] | 360 |

## Categories

| Category | Templates | Dominant tier range |
|---|---|---|
| 01_Business_Analysis_and_strategy | 18 | T3–T5 |
| 02_Elicitation_and_Collaboration | 4 | T3–T4 |
| 03_Concept_and_Mission_Definition | 7 | T4–T5 |
| 04_Requirements_Engineering | 20 | T2–T5 |
| 05_Project_Management_Planning | 29 | T2–T5 |
| 06_Project_Management_Executing_and_MC | 14 | T3–T5 |
| 07_Project_Management_Closing | 3 | T4 |
| 08_Procurement_and_Contracts | 6 | T4 |
| 09_Systems_Architecture_and_Design | 20 | T2–T7 |
| 10_Software_Design | 19 | T4–T5 |
| 11_UX_UI_Design | 35 | T4–T5 |
| 12_Construction | 11 | T3–T5 |
| 13_Testing_and_Verification | 19 | T4–T5 |
| 14_Security | 26 | T4–T6 |
| 15_Data_Management | 59 | T4–T6 |
| 16_Deployment_and_Operations | 14 | T3–T5 |
| 17_Maintenance_and_Support | 9 | T4–T6 |
| 18_Quality_Assurance | 11 | T4–T7 |
| 19_Configuration_Management | 9 | T4–T7 |
| 20_SE_Cross_Cutting | 18 | T1–T7 |
| 21_Solution_Evaluation | 5 | T4 |
| 22_Domain_Specific | 4 | T7 |

## Overlap groups (D4 alias-first)

| Group | Canonical source of truth | Views |
|---|---|---|
| `01_Business_Analysis_and_strategy` | [[Business-Requirements]] | [[Business-Requirements-Document]] |
| `04_Requirements_Engineering` | [[Requirements-Traceability-Matrix]] | [[Traceability-Matrix-Req-Tests]] |
| `05_Project_Management_Planning` | [[Risk-Management-Plan]] | [[Risk-Analysis-Results]], [[Risk-Register]], [[Risk-Report]] |
| `05_Project_Management_Planning` | [[Schedule-Management-Plan]] | [[Milestone-List]], [[Project-Schedule]], [[Gantt-Chart-Schedule]] |
| `16_Deployment_and_Operations` | [[Incident-Management-Process]] | [[Incident-Problem-Reports]], [[RCA-Reports]] |
| `19_Configuration_Management` | [[Change-Request]] | [[Requirements-Change-Assessment]], [[Requirements-Change-Log]], [[Change-Log]], [[Change-Requests]], [[Modification-Request]] |

## Related

- [[7-Tier Applicability Matrix]] — matrix view with tailoring procedure
- [[Essential Documents - Overview]] — BOK discipline checklists
- [[Template Restructure Plan - 2026-09-23]] — governing plan
- [[release]] — tier model source
