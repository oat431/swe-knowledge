---
title: "Audit: Project and Program Manager"
note_type: audit
career_path: project-and-program-manager
created: 2026-10-06
tags:
  - career-path
  - audit
  - project-management
  - program-management
---

# Audit: Project and Program Manager

> **Verdict:** A clean, rigorous treatment of **classic (predictive) project management**: charter, WBS, critical path, earned value, risk registers and reserves, change control, phase gates, audits, benefits, transition to operations, and post-project evaluation. Every topic includes an "…in Programs" section that scales it up. But for a path that *starts from software engineering*, it has a striking blind spot: **agile, adaptive, and hybrid delivery are never mentioned (0 notes)**, although PMBOK 7/8 put development approaches at the center. It also has **no project-team leadership topic**, no procurement/contract note, and its listed **"Technical context" capability has no notes behind it**.
>
> **Overall: 3 / 5**

## Snapshot

| Metric | Value |
|---|---|
| Notes | 57 (7 modules × 7 topics + 8 overviews) |
| Words | ~76k (avg ~1,330/note; modules 01–02 are lighter at ~960–1,240w) |
| Wikilinks / navigation | All resolve ✅, all topics reachable ✅ |
| Frontmatter | Consistent; only the role overview lacks `created` |
| Fill-in templates / checklists | 23/49 · 49/49 |
| Scenario exercises / examples | **0/49 · 1/49** |
| Sources | 1/49 topics names PMBOK/PMI; overview cites BLS + PMI program standard |

## Scorecard

| Dimension | Score | Why |
|---|---|---|
| Coverage | 3/5 | Predictive PM is complete; agile/hybrid, team leadership, procurement, and technical context are missing |
| Depth | 4/5 | Solid mechanics (EVM forecasting, compression decision framework, gate criteria by phase); early modules are lighter |
| Practice | 3/5 | Checklists everywhere; no exercises, and only one worked example |
| Progression | 3/5 | Good Project vs Program table; no certification guidance, which matters more for this path than any other |
| Sources | 1/5 | Draws on PMBOK concepts but rarely cites them |
| Vault hygiene | 5/5 | Clean |

## ✅ What's Good

- **Complete predictive toolkit.** Scope statements and WBS, CPM and float, cost baselines, **earned value management** with forecasting, variance thresholds, and schedule compression with a sponsor conversation.
- **Program-aware throughout.** Each topic closes with "…in Programs", and the role overview has a crisp Project Manager vs Program Manager table.
- **Honesty as a professional value.** "The PM's Role: Honest Recommendation" at gates, "The Honest Benefits Statement", the no-surprises sponsor discipline, and closing early-terminated projects.
- **Rarely-covered endings.** Transition to operations (including "When Operations Cannot Accept"), lessons learned turned into organizational learning, and post-project evaluation.
- **Strong sponsor relationship note** (`05_.../07_Sponsor_Relationship_Management`) with a weekly sponsor 1:1 operating model.

## ❌ What's Missing

1. **Agile, adaptive, and hybrid delivery (0 notes).** Software projects are rarely run purely predictively, and PMBOK 7/8 frame the choice of development approach (predictive, adaptive, hybrid) as a core PM decision. Missing: choosing an approach, rolling-wave planning, release planning with iterations, agile metrics for PMs, and working with Scrum teams and product owners.
2. **Project-team leadership (0 notes).** PMBOK's Team performance domain covers forming a project team without line authority, team agreements, motivation, and virtual teams. This path covers *resource planning* but not *leading people*.
3. **Procurement and contract management.** Mentioned in 5 notes but never taught: make-or-buy, SOWs, contract types (fixed price, T&M, cost-plus) and their risk allocation, vendor governance.
4. **Technical context (listed capability, no notes).** The overview's Capability Areas table has a "Technical context" row that links only to SWEBOK. Software PMs need SDLC literacy, estimation pitfalls in software, technical debt as schedule risk, and CI/CD's effect on release planning. The TPM path's module 02 could serve as the bridge.
5. **Certifications and career route.** PMP, PMI-ACP, PgMP, and PRINCE2 appear in only 4 notes, without guidance. For this path, certifications often gate hiring.
6. **AI (0 notes).** Neither AI-assisted planning and reporting, nor running AI projects with uncertain feasibility.

## ⚠️ What to Improve

- **Add the TPM boundary.** Three topics share exact names with the TPM path (`Managing_Stakeholder_Expectations`, `Risk_Response_Planning`, `Stakeholder_Conflict_Resolution`). Add a boundary table to both overviews: TPM = technical integration and cross-team engineering dependencies; PM = formal scope, schedule, cost, procurement, and governance.
- **Deepen modules 01–02.** Several notes have only 2–3 sections (e.g., `01_Scope_Definition`, `07_Project_Kickoff`).
- **Add one worked project.** Carry a single sample project through charter → WBS → schedule → EVM snapshot → change request → closure report.
- **Revisit `next_paths`** (currently EM and Product Manager). Consider Senior Program Manager, Portfolio Manager/PMO lead, and TPM.
- **Rename the stale heading** "Suggested Future Note Route" → "Background Reading"; add `created` to the role overview.

## 🎯 Recommended Additions (prioritized)

| Priority | Proposed note / module | What it should contain |
|---|---|---|
| 🎯 High | `08_Adaptive_and_Hybrid_Delivery/` | `01_Choosing_a_Development_Approach`, `02_Rolling_Wave_and_Release_Planning`, `03_Working_with_Scrum_Teams_and_Product_Owners`, `04_Agile_Metrics_for_PMs`, `05_Hybrid_Governance` |
| 🎯 High | `05_Stakeholder_Engagement/08_Leading_the_Project_Team.md` | Team formation without authority, team charter, motivation, virtual teams, conflict inside the team |
| 🎯 High | `09_Technical_Context_for_Software_PMs/` (or a cross-link to TPM module 02) | SDLC literacy, software estimation pitfalls, tech debt as schedule risk, CI/CD and release planning |
| Medium | `03_Schedule_and_Cost/08_Procurement_and_Contract_Management.md` | Make-or-buy, SOWs, contract types and risk allocation, vendor governance |
| Medium | `00_Certifications_and_Career_Route.md` | PMP / PMI-ACP / PgMP / PRINCE2: when each matters, and the path to program and portfolio roles |
| Low | `04_Risk_and_Issues/08_AI_in_Project_Management.md` | AI-assisted planning and reporting, and risks of AI projects |

**Suggested sources to cite:** PMI *PMBOK Guide* (7th/8th ed.) and *Agile Practice Guide*, PMI *The Standard for Program Management*, AXELOS *PRINCE2* and *Managing Successful Programmes*, *Software Estimation* (McConnell), *Making Things Happen* (Berkun), *Agile Estimating and Planning* (Cohn).

## Module-by-Module Notes

| Module | Verdict | Gap / suggestion |
|---|---|---|
| 01 Initiation & Charter | ✅ Good, lighter notes | Deepen kickoff and charter notes |
| 02 Scope & Planning | ✅ Good, lighter notes | Add rolling-wave/adaptive planning |
| 03 Schedule & Cost | ✅ Strong (EVM) | Add procurement and contracts |
| 04 Risk & Issues | ✅ Strong | Overlaps the TPM path; add a boundary note |
| 05 Stakeholder Engagement | ✅ Strong (sponsor note) | Add project-team leadership |
| 06 Governance & Change Control | ✅ Strong | Add hybrid governance |
| 07 Benefits & Closure | ✅ Excellent | — |

## Quick Fixes

- [ ] Add `created:` to the role overview
- [ ] Replace the "Technical context → SWEBOK" row with a real module or a TPM cross-link
- [ ] Add the TPM vs PM boundary table
- [ ] Rename "Suggested Future Note Route" → "Background Reading"

## Related

- [[career-path/13_Project_and_Program_Manager/00_overview|Project and Program Manager overview]]
- [[career-path/12_Technical_Program_Manager/audit|Technical Program Manager audit]]
- [[career-path/14_Product_Manager/audit|Product Manager audit]]
- [[career-path/audit|Career path audit (summary)]]
