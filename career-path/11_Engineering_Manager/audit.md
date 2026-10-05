---
title: "Audit: Engineering Manager"
note_type: audit
career_path: engineering-manager
created: 2026-10-06
tags:
  - career-path
  - audit
  - engineering-manager
---

# Audit: Engineering Manager

> **Verdict:** A thorough, humane, and practical guide to **managing one team**. It covers 1:1s, SBI feedback, GROW coaching, calibrated promotions, performance arcs, a full hiring funnel (workforce plan → JD → sourcing → rubric-based loops → debriefs → offers → onboarding), reorgs, exits, and "politics as ethics". The problem is the **ends of the path**. There's **no first-90-days note for new managers** (Staff and Tech Lead both have one), and it **stops at one team**: managing managers and the Director level appear in the progression diagram but have no content, so the map's management ladder is one rung tall while the IC ladder has three. It also barely addresses **AI's impact on teams** (1 note).
>
> **Overall: 4 / 5**

## Snapshot

| Metric | Value |
|---|---|
| Notes | 57 (7 modules × 7 topics + 8 overviews) |
| Words | ~82k (avg ~1,430/note) |
| Wikilinks / navigation | All resolve ✅, all topics reachable ✅ |
| Frontmatter | Consistent; only the role overview lacks `created` |
| Fill-in templates / checklists | 44/49 · 49/49 ✅ |
| Scenario exercises | 3/49 |
| Example sections | 0/49 |
| Sources | 1/49 topics names a management classic; overview cites Engineering Ladders + BLS |

## Scorecard

| Dimension | Score | Why |
|---|---|---|
| Coverage | 4/5 | Single-team management is complete; entry transition, multi-team, and AI impact are missing |
| Depth | 5/5 | Specific mechanics: the underperformance arc, pre-boarding "abandonment window", interrupt budgets, cascade-landing checks |
| Practice | 3/5 | Templates and checklists everywhere; almost no scenarios, which matter most in management |
| Progression | 2/5 | No IC → EM onboarding; nothing past one team; `next_paths` are unusual |
| Sources | 1/5 | The well-known EM canon is essentially uncited |
| Vault hygiene | 5/5 | Clean |

## ✅ What's Good

- **People development is excellent.** One-to-ones ("their agenda first", cancelled 1:1s as a signal), SBI feedback, coaching vs mentoring vs therapy, calibration with peer managers, "working at level vs potential", and fair performance management with documentation discipline.
- **Hiring is the most complete in the map.** It runs end to end, from workforce planning and the headcount case to sourcing, rubric-anchored interviews, evidence-first debriefs, mis-hire retros, offer and closing dynamics, and 30/60/90 onboarding.
- **Honest about the hard parts.** It includes resignations and involuntary exits, morale repair, reorg grief handling, difficult announcements, managing up when your manager is weak, and "the manager's own emotions".
- **Clear TL–EM boundary.** It mirrors the Tech Lead path's partnership note and covers the combined-role case (`06_When_the_Manager_Is_Also_the_Tech_Lead`).
- **Modern team realities.** Remote and hybrid fairness, proximity bias, async-first communication, and productivity measurement (DORA/SPACE/DevEx in 13 notes).

## ❌ What's Missing

1. **Becoming a manager.** Staff (`07_First_90_Days_as_Staff`) and Tech Lead (`07_First_90_Days_as_Tech_Lead`) have entry playbooks, but EM does not. Only `07_Managing_Experienced_Engineers` touches "the former peer transition". The IC → EM switch is the riskiest transition in the map: identity change, letting go of code, and the first hard conversation.
2. **Managing managers and the Director level.** The progression diagram ends at "Manage multiple teams or managers → Engineering Director and beyond", and the root map has a Director node, but no notes exist on managing managers, skip-level systems, org design at group scale, or the director's portfolio (budget, multi-team strategy, hiring managers).
3. **AI's impact on teams (1 note: AI-assisted drafting ethics).** Missing: leading AI-tool adoption as change management, measuring real productivity effects, interview integrity in the age of AI, how AI changes team shape and junior-engineer development, and policy and data-handling rules.
4. **The manager ↔ IC pendulum.** Nothing helps an EM decide whether to stay, return to IC (Staff/TL), or move up, and `next_paths` lists Principal and TPM, which are unusual primary routes from EM.
5. **Worked scenarios.** Management is learned through situations, yet there are 0 example sections and only 3 exercises (e.g., "Your strongest engineer asks for a promotion you don't think they're ready for").

## ⚠️ What to Improve

- **Fix `next_paths`.** Add Senior EM / Director (when that path exists) and the IC pendulum (Staff, Tech Lead). Keep Principal only with an explanatory note.
- **Credit sources.** GROW, SBI, Tuckman, psychological safety (Edmondson / Project Aristotle), and calibration practices all come from identifiable sources.
- **Add a scenario bank.** One realistic case per module, with "what a good manager does" and "common wrong moves".
- **Rename the stale heading** "Suggested Future Note Route" → "Background Reading"; add `created` to the role overview.

## 🎯 Recommended Additions (prioritized)

| Priority | Proposed note / module | What it should contain |
|---|---|---|
| 🎯 High | `01_People_Development/08_First_90_Days_as_a_New_Manager.md` | Identity shift, the first 1:1s, letting go of code, first hard conversation, inherited vs new team |
| 🎯 High | New path or module: `Engineering_Director_and_Beyond` | Managing managers, skip-level systems, multi-team org design, budget ownership, hiring managers, director-level communication |
| 🎯 High | `06_Technical_Context_for_Managers/08_Leading_AI_Adoption_in_the_Team.md` | Rollout as change management, measuring impact honestly, interview integrity, policy and data rules, effects on junior growth |
| Medium | `05_Organizational_Awareness_and_Influence/08_The_Manager_IC_Pendulum.md` | Staying, moving up, or returning to IC, with signals and how to make the switch well |
| Medium | Scenario bank | 7 cases (one per module) with worked responses |
| Low | `02_Team_Formation_and_Health/08_Inclusive_Team_Practices.md` | Consolidates the scattered inclusion content (4 notes) into one practice note |

**Suggested sources to cite:** *The Manager's Path* (Fournier), *An Elegant Puzzle* (Larson), *Resilient Management* (Hogan), *The Making of a Manager* (Zhuo), *High Output Management* (Grove), *Radical Candor* (Scott), *Become an Effective Software Engineering Manager* (Stanier), *The Fearless Organization* (Edmondson), *Coaching for Performance* (Whitmore; GROW), *Who* (Smart & Street; hiring).

## Module-by-Module Notes

| Module | Verdict | Gap / suggestion |
|---|---|---|
| 01 People Development | ✅ Excellent | Add first-90-days for new managers |
| 02 Team Formation & Health | ✅ Strong (remote/hybrid, exits) | Consolidate inclusion practices |
| 03 Hiring & Staffing | ✅ Excellent (most complete in the map) | Add interview integrity with AI tools |
| 04 Delivery Leadership for Managers | ✅ Strong | — |
| 05 Org Awareness & Influence | ✅ Strong (ethics note) | Add manager ↔ IC pendulum |
| 06 Technical Context for Managers | ✅ Strong | Add AI adoption leadership |
| 07 Manager Communication | ✅ Strong | — |

## Quick Fixes

- [ ] Fix `next_paths` (add Director / pendulum options)
- [ ] Add `created:` to the role overview
- [ ] Add a Sources section to each module overview
- [ ] Rename "Suggested Future Note Route" → "Background Reading"

## Related

- [[career-path/11_Engineering_Manager/00_overview|Engineering Manager overview]]
- [[career-path/05_Tech_Lead/audit|Tech Lead audit]]
- [[career-path/12_Technical_Program_Manager/audit|Technical Program Manager audit]]
- [[career-path/audit|Career path audit (summary)]]
