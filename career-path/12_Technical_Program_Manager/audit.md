---
title: "Audit: Technical Program Manager"
note_type: audit
career_path: technical-program-manager
created: 2026-10-06
tags:
  - career-path
  - audit
  - technical-program-manager
---

# Audit: Technical Program Manager

> **Verdict:** A deep and well-differentiated path (avg ~1,660 words/note, among the longest in the map). The **Technical Integration & Architecture** module is what separates a *TPM* from a generic program manager: reading architecture diagrams for integration risk, integration sequencing, working with architects, and integration test strategy. The **Benefits** module (output → outcome → benefit, realization, sustainment) is rare and valuable. Gaps: it never addresses **agile delivery cadences** (0 mentions of Agile/Scrum/Kanban), has **no launch / go-live note**, has **no AI coverage**, and has **zero exercises or worked examples**.
>
> **Overall: 4 / 5**

## Snapshot

| Metric | Value |
|---|---|
| Notes | 57 (7 modules × 7 topics + 8 overviews) |
| Words | ~94k (avg ~1,660/note) |
| Wikilinks / navigation | All resolve ✅, all topics reachable ✅ |
| Frontmatter | Consistent; only the role overview lacks `created` |
| Fill-in templates / checklists | 35/49 · 49/49 ✅ |
| Scenario exercises / examples | **0/49 · 0/49** |
| Sources | 1/49 topics; overview cites Engineering Ladders + PMI Standard for Program Management |
| Overlap with Project/Program Manager | 3 identical topic names (`Managing_Stakeholder_Expectations`, `Risk_Response_Planning`, `Stakeholder_Conflict_Resolution`) |

## Scorecard

| Dimension | Score | Why |
|---|---|---|
| Coverage | 4/5 | Program management plus technical integration fully covered; agile cadence, launches, and AI missing |
| Depth | 5/5 | Thorough: escalation packages, decision countdowns, contingency sizing, benefit variance analysis |
| Practice | 3/5 | Checklists everywhere, templates in most notes; no scenarios or worked programs |
| Progression | 3/5 | Senior-TPM framing throughout; no TPM ladder, interview prep, or clear next steps |
| Sources | 1/5 | Program-management standards and TPM literature are barely cited |
| Vault hygiene | 5/5 | Clean |

## ✅ What's Good

- **Technical depth that's honest about its limits.** `01_Technical_Fluency_for_TPMs` covers "What to Know, What to Delegate" and "Knowing When You Do Not Know", plus the technical questions a TPM always asks. This is the right calibration for the role.
- **Integration as a first-class concern.** Interface risk by type, progressive integration milestones, parallel vs sequential integration, integration environments and defect triage cadence.
- **Decision facilitation as a module.** Decision rights, framing documents that "price, don't split" options, pre-read contracts, decision countdowns, recording dissent, and follow-through. Most TPM guides treat this as a soft skill; here it's a system.
- **Value, not just delivery.** Module 07 separates outputs, outcomes, and benefits, assigns benefit owners, and covers sustainment after handoff and comparing actuals to the business case.
- **External dependencies.** A five-phase vendor and partner lifecycle, which many guides skip.

## ❌ What's Missing

1. **Working with agile teams (0 mentions).** TPMs coordinate teams running Scrum, Kanban, or their own cadences, and many organizations use scaled frameworks (SAFe PI planning, quarterly planning). Missing: aligning milestones to team cadences, translating sprint signals into program signals, and when scaled frameworks help or hurt.
2. **Launch and go-live management.** "Launch", "go/no-go", and "cutover" appear across 21 notes but are never taught: launch readiness reviews, go/no-go criteria, cutover runbooks, hypercare, and rollback decisions. TPMs often *own* the launch.
3. **AI programs and AI in TPM work (0 notes).** Running AI initiatives (evaluation milestones, uncertain feasibility, data dependencies) differs from classic programs, and AI-assisted status synthesis and risk detection is changing the TPM's own workflow.
4. **TPM career mechanics.** No note on TPM levels (TPM → Senior → Principal TPM), TPM interviews (program sense, system design for TPMs, behavioral), or choosing between TPM, PM (path 13), and EM.
5. **Worked program example.** A single end-to-end case (charter → integration plan → RAID → decision log → benefits review) would tie the 49 topics together.

## ⚠️ What to Improve

- **Clarify the TPM vs Project/Program Manager boundary.** Three topics share exact names with path 13, and both paths have risk, stakeholder, governance, and benefits modules. Text overlap is low, but add a boundary table to both role overviews (TPM: technical integration, cross-team dependencies, technical decisions; PM: formal scope, schedule, cost, change control, procurement).
- **Fix `next_paths`.** Currently Project/Program Manager, EM, Staff. TPM → Project Manager is unusual as a primary route; consider Senior/Principal TPM, Director of TPM, or Product Manager.
- **Add exercises.** None of the 49 topics has one. TPM skills suit tabletop scenarios ("A critical vendor slips 6 weeks two weeks before integration; run the escalation").
- **Rename the stale heading** "Suggested Future Note Route" → "Background Reading"; add `created` to the role overview.

## 🎯 Recommended Additions (prioritized)

| Priority | Proposed note / module | What it should contain |
|---|---|---|
| 🎯 High | `01_Program_Structure_and_Charter/08_Working_with_Agile_Teams_and_Scaled_Planning.md` | Cadence alignment, sprint → program signal translation, PI/quarterly planning, when frameworks help or hurt |
| 🎯 High | `02_Technical_Integration_and_Architecture/08_Launch_Readiness_and_Go_Live.md` | Readiness reviews, go/no-go criteria, cutover runbooks, hypercare, rollback decisions |
| 🎯 High | `08_Worked_Program_Case_Study/` | One realistic program run end to end through all 7 capability areas |
| Medium | `04_Risk_and_Issue_Leadership/08_Running_AI_Programs.md` | Feasibility uncertainty, eval-gated milestones, data dependencies, responsible-AI checkpoints |
| Medium | `00_TPM_Career_Ladder_and_Interviews.md` | Levels, interview formats, TPM vs PM vs EM decision guide |
| Low | Tabletop exercises | One per module |

**Suggested sources to cite:** PMI *The Standard for Program Management* (already in the overview; cite per module), AXELOS *Managing Successful Programmes*, *The Art of Project Management* (Berkun), *Accelerate* (Forsgren et al.; delivery metrics), Amazon's public writing on working-backwards documents and narratives (decision framing), *Thinking in Bets* (Duke; decision quality).

## Module-by-Module Notes

| Module | Verdict | Gap / suggestion |
|---|---|---|
| 01 Program Structure & Charter | ✅ Strong | Add agile/scaled cadence alignment |
| 02 Technical Integration & Architecture | ✅ Excellent (defining module) | Add launch readiness and go-live |
| 03 Dependency Management | ✅ Strong (external dependencies) | — |
| 04 Risk & Issue Leadership | ✅ Strong | Add AI program risk patterns; distinguish from PM path module 04 |
| 05 Stakeholder Alignment | ✅ Strong | Shares 2 topic names with the PM path; add a boundary note |
| 06 Decision Facilitation | ✅ Excellent | — |
| 07 Benefits & Outcome Measurement | ✅ Excellent | — |

## Quick Fixes

- [ ] Add `created:` to the role overview
- [ ] Add a TPM vs Project/Program Manager boundary table to both overviews
- [ ] Revisit `next_paths`
- [ ] Rename "Suggested Future Note Route" → "Background Reading"

## Related

- [[career-path/12_Technical_Program_Manager/00_overview|Technical Program Manager overview]]
- [[career-path/13_Project_and_Program_Manager/audit|Project and Program Manager audit]]
- [[career-path/11_Engineering_Manager/audit|Engineering Manager audit]]
- [[career-path/audit|Career path audit (summary)]]
