---
title: "Audit: Tech Lead"
note_type: audit
career_path: tech-lead
created: 2026-10-06
tags:
  - career-path
  - audit
  - tech-lead
---

# Audit: Tech Lead

> **Verdict:** An excellent, operationally grounded path. It handles the hard parts of tech leadership honestly (the player-coach dilemma, the TL–EM split, delegating without abdicating, leading an incident), and each topic is written as a deliberate **step up from the Senior version** ("from personal practice to team gate", "from personal debt to team register"). The gaps are domains a TL owns *for the team's system* but that the path never covers: **security, cost, and AI tooling norms**. Hiring has no dedicated note, and no topic cites a source.
>
> **Overall: 4 / 5**

## Snapshot

| Metric | Value |
|---|---|
| Notes | 57 (7 modules × 7 topics + 8 overviews) |
| Words | ~85k (avg ~1,490/note) |
| Wikilinks / navigation | All resolve ✅, all topics reachable ✅ |
| Frontmatter | Consistent; only the role overview lacks `created` |
| Fill-in templates / checklists | 47/49 · 49/49 ✅ |
| Scenario exercises | 4/49 |
| Example sections | 2/49 |
| Sources | **0/49 topics**; overview cites Engineering Ladders only |

## Scorecard

| Dimension | Score | Why |
|---|---|---|
| Coverage | 4/5 | Team operation is fully covered; security, cost, AI norms, and hiring are missing |
| Depth | 5/5 | Concrete formats: mandate document, weekly TL–EM sync, first-fifteen-minutes incident playbook, delegation ladder |
| Practice | 3/5 | Templates and checklists everywhere; almost no scenarios |
| Progression | 4/5 | First 90 days, succession, and growing future leaders; TL → Staff vs TL → EM decision is not covered |
| Sources | 1/5 | No topic cites a source |
| Vault hygiene | 5/5 | Clean |

## ✅ What's Good

- **Module 01 defines the role precisely.** It covers the mandate triangle, mandate vs job description, the TL–EM division principle with a weekly sync format, the dual-role case, the player-coach dilemma with time-allocation models, and a 30-60-90 plan.
- **Deliberate level-up framing.** Most topics explicitly convert a Senior-level personal practice into a team system: readiness checklist → team release gate, debt notes → team debt register, reviews → review SLAs and load fairness.
- **`05_Team_Development_and_Mentoring_Leadership/02_Growing_Engineers_at_Levels`** has a level map (juniors, mids, seniors, future leads). It's the closest thing in the map to a junior/mid definition, and the Software Engineer path should link to it.
- **Incident module is production-grade.** It covers command vs doing, severity activation, the first fifteen minutes, update templates, blameless postmortems, remediation funding, humane on-call design, and game days.
- **Process "fits context".** Workflow choice, DoD evolution, and process scaling by team-growth stage avoid dogma.

## ❌ What's Missing

1. **Security ownership of the team's system.** Only 1 note touches threat modeling, security reviews, or vulnerabilities. A TL is usually accountable for threat-modeling the team's system, including security in design reviews, setting vulnerability-remediation SLAs, and handling secrets hygiene.
2. **AI tooling norms for the team (0 notes).** In 2026 the TL sets the team's rules for coding agents: where they're allowed, how AI-generated PRs are reviewed, and how test and quality gates adapt. This fits `06_Process_and_Quality_Stewardship`.
3. **Cost of the team's system (0 notes).** Cloud spend, cost budgets, and cost as a design dimension are frequently a TL accountability, especially for platform and data-heavy teams.
4. **Hiring and interviewing.** Mentioned in 5 notes but never taught: designing the technical interview, calibrating feedback, and shaping team composition. TLs often own the technical half of hiring.
5. **The TL → Staff vs TL → EM decision.** The progression diagram points to Staff or Architect, and `next_paths` includes EM, but no note helps the reader choose (the "pendulum" question).
6. **Sources.** None of the 49 topics cites a source (see below).

## ⚠️ What to Improve

- **Add scenario exercises.** Only 4/49 topics have one. TL topics suit scenarios well, e.g., "Your EM wants to commit to a date the team estimated at 60% confidence; run the conversation".
- **Cross-link overlapping notes deliberately.** Estimation, ADRs, code review, production readiness, tech debt, and incident response exist in Senior, Tech Lead, and SRE. The TL versions frame the step up well; add a "Senior version → TL version → SRE version" link line to each so readers see the progression.
- **Rename the stale heading** "Suggested Future Note Route" → "Background Reading"; add `created` to the role overview.

## 🎯 Recommended Additions (prioritized)

| Priority | Proposed note / module | What it should contain |
|---|---|---|
| 🎯 High | `02_System_Ownership_and_Production_Responsibility/08_Security_Ownership_for_the_Team.md` | Threat-modeling the team's system, security in design review, vulnerability SLAs, secrets, and partnering with the security team |
| 🎯 High | `06_Process_and_Quality_Stewardship/08_AI_Tooling_Norms_for_the_Team.md` | Where AI tools are allowed, review expectations for AI-generated changes, gate adjustments, and measuring effect on delivery and quality |
| Medium | `05_Team_Development_and_Mentoring_Leadership/08_Technical_Hiring_and_Interviewing.md` | Interview design, rubrics, calibrated feedback, and team composition |
| Medium | `02_System_Ownership_and_Production_Responsibility/09_Cost_Ownership.md` | Cost budgets, cost dashboards, and cost trade-offs in design reviews |
| Medium | `01_The_Tech_Lead_Role_and_Operating_Model/08_Next_Step_Staff_or_EM.md` | Decision guide for TL → Staff vs TL → EM, plus switching back (the pendulum) |
| Low | Scenario exercises | One per topic, starting with modules 01 and 07 |

**Suggested sources to cite:** *Talking with Tech Leads* (Patrick Kua), *The Manager's Path* (Fournier; tech lead chapter), *Debugging Teams* (Fitzpatrick & Collins-Sussman), *Accelerate* (Forsgren, Humble & Kim), *Site Reliability Engineering* (Beyer et al.; incident management chapters), PagerDuty's public Incident Response documentation.

## Module-by-Module Notes

| Module | Verdict | Gap / suggestion |
|---|---|---|
| 01 Role & Operating Model | ✅ Excellent | Add TL → Staff/EM decision note |
| 02 System Ownership & Production | ✅ Strong | Add security ownership and cost ownership |
| 03 Technical Direction & Architecture | ✅ Strong | Link ADR/design-review notes to the Software Architect path |
| 04 Team Delivery & Execution | ✅ Strong (longest module) | Link Estimation to the Senior version |
| 05 Team Development & Mentoring | ✅ Strong | Add hiring/interviewing; cross-link the level map to Software Engineer |
| 06 Process & Quality Stewardship | ✅ Strong | Add AI tooling norms |
| 07 Incident Leadership & Production Excellence | ✅ Excellent | Cross-link to SRE `03_Incident_Response` |

## Quick Fixes

- [ ] Add `created:` to the role overview
- [ ] Add a Sources section to each module overview
- [ ] Link `05_.../02_Growing_Engineers_at_Levels` from the Software Engineer overview
- [ ] Rename "Suggested Future Note Route" → "Background Reading"

## Related

- [[career-path/05_Tech_Lead/00_overview|Tech Lead overview]]
- [[career-path/02_Senior_Software_Engineer/audit|Senior Software Engineer audit]]
- [[career-path/11_Engineering_Manager/audit|Engineering Manager audit]]
- [[career-path/audit|Career path audit (summary)]]
