---
title: "Audit: Staff Engineer"
note_type: audit
career_path: staff-engineer
created: 2026-10-06
tags:
  - career-path
  - audit
  - staff-engineer
---

# Audit: Staff Engineer

> **Verdict:** One of the strongest paths in the map. Its 49 topics read like an operator's manual (listening tours, Impact × Leverage × Readiness filtering, arc scoping, influence budgets, written risk acceptance), and the scope matches the major Staff+ literature. The gaps are about the *edges* of the role: **staying technical**, **leading AI adoption**, **running a large project end to end**, and **getting promoted or hired as Staff**. Most templates are blank and few worked examples exist.
>
> **Overall: 4 / 5** (content quality alone would be 5)

## Snapshot

| Metric | Value |
|---|---|
| Notes | 57 (7 modules × 7 topics + 8 overviews) |
| Words | ~79k (avg ~1,380/note) |
| Wikilinks / navigation | All resolve ✅, every topic reachable from its module ✅ |
| Frontmatter | Consistent; only the role overview lacks `created` |
| Fill-in template + checklist | In nearly every topic ✅ |
| Scenario exercises | 4/49 topics |
| Worked examples | 1/49 |
| Sources | 3/49 topics; Larson credited once; *The Staff Engineer's Path* never cited |

## Scorecard

| Dimension | Score | Why |
|---|---|---|
| Coverage | 4/5 | Complete on influence, strategy, systems, risk, and multiplication; missing the edges listed below |
| Depth | 5/5 | Specific methods with reject criteria, e.g., "an arc without success criteria is a hobby; an arc without a duration is a committee" |
| Practice | 3/5 | Every note has a template and checklist, but templates are blank and there are few scenarios to practice on |
| Progression | 3/5 | Good Senior → Staff scope note and first-90-days plan; no consolidated promotion/evidence module (that content sits in the Senior path) |
| Sources | 2/5 | Ideas from Larson, Reilly, Rumelt, Meadows, and Team Topologies are used but rarely credited |
| Vault hygiene | 5/5 | Clean links, consistent schema, all topics reachable |

## ✅ What's Good

- **Module 01 alone is worth the folder.** It covers the four archetypes (credited to Larson), Senior vs Staff scope, finding staff work, the staff calendar, working without authority, a trap catalog, and a first-90-days plan. Most career guides skip the "how do I actually start" part; this one doesn't.
- **Realistic about organizations.** Notes like `04_Saying_No_at_Scale`, `07_Sunset_and_Exit_Strategy`, `05_Risk_Pricing_and_Acceptance`, and `07_Influence_Ethics_for_Staff` deal with the uncomfortable parts of the job (declining, killing things, accepting risk in writing, the line between influence and manipulation).
- **Systems thinking grounded in practice.** Stocks/flows and leverage points lead into Conway's Law, incentive audits, Goodhart's Law, and coordination-cost math.
- **Multiplication is concrete.** `05_Growing_the_Next_Staff` separates sponsorship from mentorship, and `07_Knowledge_Continuity` treats bus factor as risk management.
- **Each topic has a usable artifact,** such as the Candidate Problem Log, strategy-document anatomy, and the written acceptance record.

## ❌ What's Missing

1. **Staying technical.** No note covers keeping hands-on credibility (protected coding time, prototypes, deep dives, staying current). The Solver and Tech Lead archetypes depend on it, and losing it is a top reason staff engineers stall.
2. **Leading AI adoption.** AI appears once (LLM APIs as a vendor dependency). In 2026, staff engineers are routinely asked to evaluate AI tooling, set cross-team norms, and decide where AI belongs in the architecture. This fits `03_Technical_Strategy`.
3. **Running a large cross-team project end to end.** Migration leadership is covered, but general project execution is not: kickoff, design doc, milestones, getting unstuck, and declaring done.
4. **Staff promotion and hiring.** There is no consolidated "staff packet". The Senior path's module 09 holds the Senior → Staff promotion content, which belongs here. Staff interview loops (architecture deep dive, leadership behavioral questions, project retrospective) are also absent.
5. **Partnering with managers.** The Right Hand archetype and the staff + EM pairing get only 3 passing mentions; working effectively with EMs and directors deserves its own note.
6. **Worked examples.** A filled-in strategy document, an adopted RFC, or a risk acceptance record would teach far more than blank templates.

## ⚠️ What to Improve

- **Scenario exercises.** Only 4/49 topics have one. A "Try it" scenario per topic would help (e.g., "Three teams each built a caching layer; draft the standard and the exception process").
- **Credit sources.** Larson's archetypes are credited once. Reilly's *The Staff Engineer's Path* is the closest single source to this folder and is never named. Add a Sources section to each module overview.
- **Fix role overview ordering.** The Capability Areas table lists module 07 before 06.
- **Rename the stale heading.** "Suggested Future Note Route" should become "Background Reading".
- **Add `created`** to the role overview.

## 🎯 Recommended Additions (prioritized)

| Priority | Proposed note / module | What it should contain |
|---|---|---|
| 🎯 High | `01_The_Staff_Role_and_Scope/08_Staying_Technical.md` | Protected maker time, prototype-first influence, deep-dive rotations, reading code you don't own, and recognizing when you've drifted too far from the code |
| 🎯 High | `03_Technical_Strategy/08_AI_Adoption_Strategy.md` | Evaluating AI tooling, cross-team usage norms, where AI fits architecturally, risk and governance hand-offs, and measuring real impact |
| 🎯 High | `08_Staff_Promotion_Evidence_and_Hiring/` | Staff packet, scope evidence over time, sponsor map, staff interview loops; move Senior → Staff content here from the Senior path's module 09 |
| Medium | `02_Cross_Team_Technical_Leadership/08_Leading_Large_Projects.md` | Kickoff, design doc, milestones, unblocking, and finishing (*The Staff Engineer's Path*, Part II) |
| Medium | `04_Influence_and_Alignment/08_Partnering_With_Engineering_Managers.md` | Staff + EM pairing, division of responsibilities, and the Right Hand archetype in practice |
| Medium | Worked examples | A filled-in strategy document (03), RFC (04), and risk acceptance record (06) |
| Low | "Try it" scenarios | One per topic, starting with modules 03 and 04 |

**Suggested sources to cite:** *Staff Engineer: Leadership Beyond the Management Track* (Larson) and staffeng.com, *The Staff Engineer's Path* (Reilly), *Good Strategy Bad Strategy* (Rumelt), *Thinking in Systems* (Meadows), *Team Topologies* (Skelton & Pais), *An Elegant Puzzle* (Larson), *The Software Architect Elevator* (Hohpe).

## Module-by-Module Notes

| Module | Verdict | Gap / suggestion |
|---|---|---|
| 01 The Staff Role & Scope | ✅ Excellent | Add "Staying Technical" |
| 02 Cross-Team Technical Leadership | ✅ Strong | Add general large-project execution |
| 03 Technical Strategy | ✅ Strong | Add AI adoption strategy; credit Rumelt |
| 04 Influence & Alignment | ✅ Excellent (ethics note is rare) | Add EM partnership |
| 05 Systems Thinking & Org Design | ✅ Strong | Credit Meadows / Team Topologies consistently |
| 06 Technical Risk & Judgment | ✅ Strong | Add a filled-in risk acceptance example |
| 07 Org Learning & Mentoring | ✅ Strong | — |

## Quick Fixes

- [ ] Add `created:` to the role overview
- [ ] Reorder the Capability Areas table (06 before 07)
- [ ] Rename "Suggested Future Note Route" → "Background Reading"
- [ ] Add a Sources section to each module overview

## Related

- [[career-path/03_Staff_Engineer/00_overview|Staff Engineer overview]]
- [[career-path/02_Senior_Software_Engineer/audit|Senior Software Engineer audit]]
- [[career-path/04_Principal_and_Distinguished_Engineer/audit|Principal and Distinguished Engineer audit]]
- [[career-path/audit|Career path audit (summary)]]
