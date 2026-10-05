---
title: "Audit: Senior Software Engineer"
note_type: audit
career_path: senior-software-engineer
created: 2026-10-06
tags:
  - career-path
  - audit
  - senior-engineer
---

# Audit: Senior Software Engineer

> **Verdict:** The largest path in the map and one of the most practical: 9 modules, 61 topic notes, exercises in two-thirds of topics, and the map's only capstone module. Two structural problems hold it back. **(1) The promotion module targets Senior → Staff, so nothing in the map covers getting promoted *to* Senior**, the "immediate promotion target" according to the root overview. **(2) It teaches senior *judgment* well but not senior *technical depth*:** there are no notes on system design, distributed systems, API evolution, data migrations, or performance.
>
> **Overall: 4 / 5**

## Snapshot

| Metric | Value |
|---|---|
| Notes | 71 (9 module overviews + 61 topics + role overview) |
| Words | ~114k (avg ~1,600/note) |
| Wikilinks | All resolve ✅ |
| Module → topic navigation | ❌ Modules 01, 02, 08 list topics as `` `file.md` `` text, not links (20 topics not clickable) |
| Frontmatter | ⚠️ **7 different schemas** in one folder; 41/71 notes lack `created`; 4 notes have `created: 2026-01-05` (git says 2026-08-05) |
| Exercises | 40/61 topics (strong: every note in modules 01–05) |
| Example / scenario sections | 20/61 |
| External sources | 1/61 topics has a sources section; 2 source links in the whole folder (both Engineering Ladders; other URLs are code samples) |

## Scorecard

| Dimension | Score | Why |
|---|---|---|
| Coverage | 4/5 | All senior *behaviors* are covered; senior *technical depth* is missing |
| Depth | 4/5 | Specific and actionable (e.g., confidence-range forecasting, stakeholder pushback scripts) |
| Practice | 4/5 | Exercises in modules 01–05, templates, and a full year-long capstone; modules 06–08 have few exercises |
| Progression | 3/5 | The promotion module points to the *next* level (Staff), not this one |
| Sources | 1/5 | Almost no citations of books, ladders, or research |
| Vault hygiene | 2/5 | Broken module navigation, a malformed table, 7 frontmatter schemas, wrong dates |

## ✅ What's Good

- **Well-chosen modules.** Ownership → problem framing → architecture judgment → delivery → quality/reliability/security → communication → mentoring → economics → evidence follows how seniority is actually assessed.
- **"Why This Is a Senior Skill" contrasts.** Each topic opens with a mid-level vs senior comparison, which makes the level jump explicit.
- **Practical, not theoretical.** Estimation, for example, covers five techniques with worked numbers, 50/75/90% confidence forecasting, an anti-pattern table, stakeholder pushback scripts, and an exercise that calibrates against your last five estimates.
- **Capstone module (09) is unique in the map.** It includes a 10-point capstone checklist, a quarter-by-quarter execution plan, an evidence-collection plan, and example projects. No other path has an equivalent.
- **Economics module (08).** Build vs buy, TCO, tech-debt ROI, and business cases are rarely taught to engineers and are a strong differentiator.
- **Low duplication.** Content is original, apart from the Observability note, which shares 14–22% of its text with the SRE path's Metrics and Logging notes.

## ❌ What's Missing

1. **Mid → Senior promotion.** Module 09 says it builds "compelling cases for advancement to Staff/Principal levels", and `05_Career_Ladders` centers on "My Delta: Senior to Staff". That overlaps the Staff path and leaves the most common promotion in the industry uncovered.
2. **Senior technical depth.** No dedicated notes on:
   - System design fundamentals (scalability, caching, queues, consistency, idempotency); "distributed system" appears in only 1 note
   - API design and evolution (versioning, backward compatibility, deprecation)
   - Data and schema migrations (expand/contract, zero-downtime, backfills)
   - Performance engineering (profiling, latency budgets, load testing)
   - Large-scale refactoring and legacy modernization (strangler fig, incremental migration)
3. **AI-assisted engineering at senior level.** Setting team norms for coding agents, reviewing AI-generated PRs, and judging when AI output is trustworthy. Nothing in the path covers this.
4. **Interviewing as a senior.** Seniors usually join hiring loops (technical interviews, calibrated feedback, bar raising). This is covered only in the EM path.
5. **Self-assessment checklists in module overviews.** Only 3 of 9 module overviews have one (01, 02, 09).
6. **Sources.** Missing the canonical references for this level (see below).
7. **Non-functional breadth.** Accessibility, privacy, and i18n never appear (0 notes mention accessibility).

## ⚠️ What to Improve

- **Fix navigation.** Convert the "File" column in the topic tables of `01_Technical_Ownership`, `02_Problem_Framing_and_Requirements`, and `08_Engineering_Economics_and_Trade_Offs` from `` `01_System_Ownership.md` `` to `[[01_System_Ownership]]`.
- **Fix the role overview table.** The Capability Areas header starts with `||`, giving 4 header cells against a 3-cell separator; under GFM rules it will not render as a table. The rows are also out of module order (02 is listed fifth).
- **Unify the frontmatter.** Seven key sets are in use (`note_type` vs `type`, `career_path` vs `role`, `status`/`updated` on some notes only). Pick one schema so Dataview queries work.
- **Fix dates.** `09_Promotion_Evidence_and_Capstone/00_overview`, `01`, `02`, and `03` say `created: 2026-01-05`; git history shows 2026-08-05.
- **Fix the module 09 title.** `title: "09_Promotion_Evidence_and_Capstone"` should be human-readable ("Promotion Evidence and Capstone").
- **Align the templates.** Modules 01–05 use the "Practical Exercise / Key Takeaways" template and modules 06–08 use the "Practical Applications / Summary" template, which has almost no exercises (3/20 topics). Add an exercise to each note in 06–08.
- **Rename the stale heading.** "Suggested Future Note Route" now lists SWEBOK links that duplicate the modules; rename it to "Background Reading".

## 🎯 Recommended Additions (prioritized)

| Priority | Proposed note / module | What it should contain |
|---|---|---|
| 🎯 High | `09_Promotion_Evidence_and_Capstone/00_Mid_to_Senior_Promotion.md` (or reframe the module) | What committees expect for Senior (sustained ownership, independent ambiguity handling), a mid → senior delta template, and the common reasons people get "not yet". Move Senior → Staff content to the Staff path, or label it as a "look ahead" |
| 🎯 High | `10_Technical_Depth_for_Seniors/` | `01_System_Design_Fundamentals`, `02_API_Design_and_Evolution`, `03_Data_and_Schema_Migrations`, `04_Performance_Engineering`, `05_Large_Scale_Refactoring_and_Legacy_Modernization`, `06_Distributed_Systems_Failure_Modes` |
| 🎯 High | `05_Quality_Reliability_Security/08_AI_Assisted_Development_Practices.md` | Team norms for AI coding tools, reviewing generated code, provenance/licensing, security, and productivity measurement |
| Medium | `07_Mentoring_and_Team_Leadership/08_Interviewing_and_Hiring_Participation.md` | Structured interviews, writing calibrated feedback, avoiding bias, and system-design interviewing |
| Medium | Self-assessment checklists | Add to module overviews 03–08 (the pattern already exists in 01, 02, 09) |
| Low | `05_Quality_Reliability_Security/09_Accessibility_Privacy_and_Compliance_Basics.md` | Non-functional requirements seniors should flag in design reviews |

**Suggested sources to cite:** *Designing Data-Intensive Applications* (Kleppmann), *Software Engineering at Google* (Winters, Manshreck & Wright), *Accelerate* (Forsgren, Humble & Kim), *Release It!* (Nygard), *Software Estimation: Demystifying the Black Art* (McConnell), *The Staff Engineer's Path* (Reilly; useful for the senior → staff look-ahead), *Crucial Conversations* (Patterson et al.), and public ladders such as progression.fyi.

## Module-by-Module Notes

| Module | Verdict | Gap / suggestion |
|---|---|---|
| 01 Technical Ownership | ✅ Strong | Navigation links broken; add "Ownership Handoff" checklist as a template |
| 02 Problem Framing & Requirements | ✅ Strong (largest: 8 topics) | Navigation links broken |
| 03 Architecture & Design Judgment | ✅ Good on judgment | No hands-on system design; overlaps the Software Architect path (cross-link explicitly) |
| 04 Delivery & Execution | ✅ Strong | Add probabilistic (Monte Carlo/throughput) forecasting to Estimation |
| 05 Quality, Reliability, Security | ✅ Good | Add AI-assisted development and accessibility/privacy; Observability shares 14–22% of its text with SRE notes |
| 06 Communication & Influence | ✅ Good content | Only 2/7 topics have exercises |
| 07 Mentoring & Team Leadership | ✅ Good content | No exercises (0/7); add interviewing |
| 08 Engineering Economics | ✅ Differentiator | Navigation links broken; 1/6 topics has an exercise |
| 09 Promotion Evidence & Capstone | ⚠️ Excellent mechanics, wrong target level | Reframe to mid → senior; fix dates and title |

## Quick Fixes

- [ ] Convert backtick filenames to wikilinks in modules 01, 02, 08
- [ ] Fix the `||` header in the role overview's Capability Areas table
- [ ] Change `created: 2026-01-05` → `2026-08-05` (4 files in module 09)
- [ ] Add `created:` to the 41 notes missing it
- [ ] Standardize on one frontmatter schema
- [ ] Rename "Suggested Future Note Route" → "Background Reading"

## Related

- [[career-path/02_Senior_Software_Engineer/00_overview|Senior Software Engineer overview]]
- [[career-path/01_Software_Engineer/audit|Software Engineer audit]]
- [[career-path/03_Staff_Engineer/audit|Staff Engineer audit]]
- [[career-path/audit|Career path audit (summary)]]
