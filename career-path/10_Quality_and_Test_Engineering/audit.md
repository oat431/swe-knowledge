---
title: "Audit: Quality and Test Engineering"
note_type: audit
career_path: quality-and-test-engineering
created: 2026-10-06
tags:
  - career-path
  - audit
  - quality-engineering
---

# Audit: Quality and Test Engineering

> **Verdict:** Good *testing* content, but it reads like a **testing textbook rather than a career path**. Topics are written as "What X Is / The Technique / Examples" with no "what changes at specialist level" framing. The test-design module is genuinely strong (equivalence, boundaries, decision tables, state transitions, session-based exploratory testing). But **the role overview never links to its own modules** (all 6 module overviews are unreachable), the specialized-testing and measurement modules are **thin (415–880 words)**, and modern quality engineering is mostly absent: **testing AI features, shift-right, mutation/property testing, and the QA → SDET → quality-coach career shape**.
>
> **Overall: 2.5 / 5**

## Snapshot

| Metric | Value |
|---|---|
| Notes | 44 (6 modules, 37 topics + 7 overviews) |
| Words | ~68k including code samples; topic prose ranges 415–1,871 words |
| Wikilinks | All resolve ✅ |
| Role → module navigation | ❌ The role overview's Capability Areas table links to SWEBOK notes, **not** to the 6 module overviews, which have **no inbound links anywhere** |
| Module overviews | 3 of 6 are stubs (<400 words: 03, 04, 06); "Progress Tracker" lists items as "- Complete" next to unchecked boxes |
| Frontmatter | ✅ `created` present (except the role overview) |
| Exercises / examples | 12/37 · 16/37 |
| Checklists / templates | 16/37 · 11/37 |
| Sources | 0 classic testing references named |

## Scorecard

| Dimension | Score | Why |
|---|---|---|
| Coverage | 3/5 | Classic strategy, design, and automation covered; AI testing, shift-right, and advanced techniques missing |
| Depth | 3/5 | Test design notes are rich (1,200–1,700w with worked examples); specialized and measurement notes are thin |
| Practice | 3/5 | Good examples in test design; exercises in only a third of topics |
| Progression | 2/5 | No level framing, no career variants, no "specialist vs senior engineer" contrast in topics |
| Sources | 1/5 | ISTQB and the testing classics are never named |
| Vault hygiene | 2/5 | Modules unreachable from the role overview; stub overviews; contradictory progress trackers |

## ✅ What's Good

- **Test design module (02) is the best testing content in the vault.** It has seven technique notes with worked examples, coverage criteria, and "common defects found", plus a `07_Test_Design_Strategy` note on combining techniques.
- **Risk-first strategy.** `01_Risk_Based_Testing` (1,779 words) includes a planning example and covers communicating risk-based decisions, which is the core specialist skill.
- **Pragmatic automation.** It covers flaky-test management with a dashboard, an automation ROI framework, and **"When NOT to Automate"**.
- **Quality as culture, not gate.** `06_Quality_Culture` and `01_Defect_Prevention` push quality left into the team.
- **Accessibility is covered here** (the only path that does), and `06_Measurement_Pitfalls` covers Goodhart's Law and a measurement anti-pattern catalog.

## ❌ What's Missing

1. **Testing AI/LLM features and using AI in testing.** Only 2 notes touch AI, although the role overview lists "AI" under specialized testing. Missing: evaluation datasets, LLM-as-judge, regression testing of prompts and models, non-determinism handling, and AI-assisted test generation and its risks.
2. **Shift-right / testing in production (1 note).** Canary analysis, synthetic monitoring, feature-flag experiments, and observability as a test oracle.
3. **Advanced techniques.** Mutation testing (a key measure of test effectiveness), property-based testing, and fuzzing appear in only 2 notes.
4. **Career shape.** No note on QA analyst → SDET → quality engineer → test architect / quality coach / head of quality, or on how the role is changing (many organizations fold testing into development and keep quality engineers as coaches and platform builders). Moves to SRE or development are also not addressed.
5. **Specialist framing in topics.** Unlike every other path, topics never say what a *specialist or senior* quality engineer does differently from a developer who writes tests.
6. **Sources.** No ISTQB syllabi, and none of the classic books (see below).

## ⚠️ What to Improve

- **Fix top-level navigation.** Change the role overview's Capability Areas "Existing vault anchor" column to link the modules (`[[01_Test_Strategy/00_overview|Test Strategy]]`, …) and move the SWEBOK links to a separate column.
- **Expand the stub module overviews.** `03_Automation` (390w), `04_Quality_Engineering` (372w), and `06_Measurement` (364w).
- **Deepen thin topics.** `03_Reliability_Testing` (415w), `06_Measurement_Pitfalls` (439w), `04_Quality_Reporting` (499w), `05_Data_Analysis` (606w), `03_Process_Metrics` (619w), `02_Coverage_Metrics` (631w), `05_API_Testing` (668w), and `02_Security_Testing` (672w).
- **Fix the progress trackers.** Either check the boxes or drop the "- Complete" suffix.
- **Update accessibility.** `04_Accessibility_Testing` is built on WCAG 2.1; WCAG 2.2 has been the W3C Recommendation since October 2023.
- **Add a specialist-framing section to each topic** ("What a specialist does differently"), matching the Senior/Security/SRE pattern.
- **Cross-link the Security Engineer path.** `02_Security_Testing` covers the OWASP Top 10, which that path never names.

## 🎯 Recommended Additions (prioritized)

| Priority | Proposed note / module | What it should contain |
|---|---|---|
| 🎯 High | Fix role → module links and expand stub overviews | Prerequisite for using the path at all |
| 🎯 High | `05_Specialized_Testing/07_Testing_AI_and_LLM_Features.md` | Eval sets, LLM-as-judge with calibration, prompt/model regression, flaky-by-design outputs; link [[career-path/18_Applied_AI_Engineer/00_overview\|Applied AI Engineer]] module 03 |
| 🎯 High | `07_Quality_Engineering_Career/` | Role variants (SDET, QE, test architect, quality coach), levels, evidence, and transitions to dev/SRE |
| Medium | `03_Automation/07_Testing_in_Production_and_Shift_Right.md` | Canaries, synthetic monitoring, observability as oracle, feature-flag experiments |
| Medium | `02_Test_Design/08_Property_Based_and_Mutation_Testing.md` | When each pays off; mutation score as a coverage-quality check |
| Medium | `03_Automation/08_AI_Assisted_Test_Generation.md` | Where generated tests help, oracle problems, review discipline |
| Low | Deepen module 06 Measurement | Worked dashboards and analyses instead of lists |

**Suggested sources to cite:** ISTQB syllabi (Foundation, Advanced Test Analyst, Test Automation Engineer), *Lessons Learned in Software Testing* (Kaner, Bach & Pettichord), *Agile Testing* and *More Agile Testing* (Crispin & Gregory), *Explore It!* (Hendrickson), *xUnit Test Patterns* (Meszaros), *Software Engineering at Google* (testing chapters), the Modern Testing Principles (Page & Jensen), WCAG 2.2.

## Module-by-Module Notes

| Module | Verdict | Gap / suggestion |
|---|---|---|
| 01 Test Strategy | ✅ Strong | Add specialist framing |
| 02 Test Design | ✅ Excellent | Add property-based and mutation testing |
| 03 Automation | ✅ Good (flaky tests, ROI) | Overview is a stub; add shift-right and AI-assisted generation |
| 04 Quality Engineering | ✅ Good | Overview is a stub |
| 05 Specialized Testing | ⚠️ Thin (415–1,067w) | Add AI testing; deepen reliability, API, security; update to WCAG 2.2 |
| 06 Measurement | ⚠️ Thin (439–842w) | Overview is a stub; deepen with worked analyses |

## Quick Fixes

- [ ] Link all 6 module overviews from the role overview
- [ ] Add `created:` to the role overview
- [ ] Fix "- [ ] … - Complete" contradictions in Progress Trackers
- [ ] Update the accessibility note to WCAG 2.2

## Related

- [[career-path/10_Quality_and_Test_Engineering/00_overview|Quality and Test Engineering overview]]
- [[career-path/07_SRE_and_Platform_Engineer/audit|SRE and Platform Engineer audit]]
- [[career-path/08_Security_Engineer/audit|Security Engineer audit]]
- [[career-path/audit|Career path audit (summary)]]
