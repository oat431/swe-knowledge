---
title: "Audit: Product Manager"
note_type: audit
career_path: product-manager
created: 2026-10-06
tags:
  - career-path
  - audit
  - product-manager
---

# Audit: Product Manager

> **Verdict:** Covers the classic PM core (discovery, strategy, prioritization, roadmapping, analytics, requirements, technical partnership) and does two things better than any other path: **every topic cites real resources** (*The Mom Test*, *Interviewing Users*, …) and **every topic has a "Senior-Level" section**. But the notes are **short** (482–1,146 words) and **nearly isolated** (1.2 wikilinks per note), the **role overview doesn't link its own modules**, and topics are duplicated *inside* the path. For an engineer moving into product, the most relevant topics are missing: **the SWE → PM transition, technical/platform/API product management, and AI product management** (AI appears only as example feature names).
>
> **Overall: 3 / 5**

## Snapshot

| Metric | Value |
|---|---|
| Notes | 49 (7 modules, 41 topics + 7 overviews + role overview) |
| Words | ~59k including code blocks; topic prose ranges 482–1,146 words |
| Wikilinks | All resolve ✅, but topics average **1.2 links each** (no "Related" sections) |
| Role → module navigation | ❌ The Capability Areas table links BABOK/PMBOK/DMBOK notes, **not** the modules; `02_Strategy`, `04_Roadmapping`, and `06_Requirements` overviews have **no inbound links at all** |
| Module overviews | 2 stubs (`01_Problem_Discovery` 329w, `03_Prioritization` 285w) |
| Frontmatter | `created` missing in **49/49** |
| Resources per topic | **41/41** ✅ (the best in the map) |
| Checklists / exercises / examples | 41/41 · 13/41 · 11/41 |

## Scorecard

| Dimension | Score | Why |
|---|---|---|
| Coverage | 3/5 | Classic core is present; AI PM, technical/platform PM, GTM, and the transition are missing |
| Depth | 2/5 | Many notes are a sequence of short "Why / Mistakes / Senior-Level" sections |
| Practice | 3/5 | Checklist in every note; exercises and examples in about a quarter |
| Progression | 2/5 | "Evidence for Promotion" in module overviews, but no transition guide or PM ladder |
| Sources | 4/5 | Real books per topic, the best in the map |
| Vault hygiene | 2/5 | Modules unreachable from the role overview, isolated notes, `created` missing, in-path duplication |

## ✅ What's Good

- **Sourced by default.** Each topic ends with Resources, e.g., `01_Customer_Interviews` → BABOK elicitation, *The Mom Test* (Fitzpatrick), *Interviewing Users* (Portigal). Other paths should copy this.
- **Senior framing on every topic.** "Senior-Level Practices / Senior-Level Prioritization / …" sections, plus "Evidence for Promotion" in each module overview.
- **Good framework coverage where it matters.** `03_Prioritization_Frameworks` covers RICE, MoSCoW, Value/Effort, Kano, WSJF, and Cost of Delay with selection guidance. `06_Analytics_Pitfalls` covers vanity metrics, survivorship bias, and Simpson's paradox.
- **Roadmapping without false precision.** Three-horizon roadmaps and a 7-item roadmap anti-pattern catalog (feature factory, wish list, false precision, …).
- **Technical partnership module** is written from the PM side of the TL/EM relationships, which complements the Tech Lead and EM paths.

## ❌ What's Missing

1. **The SWE → PM transition (1 mention).** The path's own `entry_from` is Senior Software Engineer, yet nothing covers making the switch: how to stop solutioning, building credibility with former peers, the first 90 days as a PM, or common failure modes of ex-engineer PMs.
2. **Technical, platform, and API product management (0 notes).** For an engineer this is the most natural first PM role: internal platforms, developer products, APIs as products, and DevEx metrics. It would also link to the SRE path's Developer Platform module and the Developer Advocate path.
3. **AI product management.** AI appears only as example features ("AI recommendations"). Missing: probabilistic UX, eval metrics as product metrics, cost per interaction, trust and safety, human-in-the-loop design, and AI-feature discovery.
4. **Go-to-market, launch, pricing, and packaging.** Mentioned across 22 notes but never taught as a topic.
5. **Continuous discovery.** Jobs-to-be-Done, opportunity solution trees, and assumption testing with prototypes appear in only 4 notes.
6. **Business acumen depth.** Business models, unit economics (LTV/CAC), and P&L thinking appear in 9 notes, mostly in passing.
7. **PM career ladder.** APM → PM → Senior → Group PM → Director → VP/CPO is mentioned in 2 notes, and `next_paths` (Project Manager, Solutions Architect) misses the natural routes, including Founder ([[career-path/17_Independent_Consulting_and_Technical_Founder/00_overview|Independent Consulting and Technical Founder]] lists PM as an entry).

## ⚠️ What to Improve

- **Fix top-level navigation.** Link the 7 module overviews from the role overview, and move the BOK anchors to a second column.
- **Remove in-path duplication.** `03_Prioritization/05_Roadmap_Planning` duplicates the whole `04_Roadmapping` module (11% shared text with its overview). `03_Prioritization/04_Stakeholder_Management` and `06_Requirements/05_Requirements_Prioritization` overlap other modules. Merge or cross-reference them.
- **Add "Related" sections.** Topic notes average 1.2 links; link each to sibling topics and to matching notes in the Senior (02 Problem Framing), Tech Lead, and EM paths.
- **Deepen the shortest notes:** `04_Acceptance_Criteria` (482w), `03_User_Stories` (564w), `03_Technical_Trade_Offs` (568w), `06_Collaboration_with_Tech_Leads` (587w), `05_Architecture_Understanding` (618w), `03_Dependencies_and_Sequencing` (632w).
- **Expand the stub overviews** for Problem Discovery and Prioritization; add `created` to all 49 notes.

## 🎯 Recommended Additions (prioritized)

| Priority | Proposed note / module | What it should contain |
|---|---|---|
| 🎯 High | Fix role → module links and remove in-path duplication | Prerequisite for using the path |
| 🎯 High | `00_From_Engineer_to_Product_Manager.md` | Switching paths, first 90 days, unlearning solutioning, ex-engineer PM failure modes, using technical depth well |
| 🎯 High | `08_Technical_and_Platform_Product_Management/` | Internal platforms as products, API product management, developer experience metrics, platform roadmaps, deprecations |
| 🎯 High | `09_AI_Product_Management/` | AI-feature discovery, probabilistic UX, evals as product metrics, cost per interaction, trust and safety, human-in-the-loop |
| Medium | `02_Strategy/06_Go_To_Market_Pricing_and_Launch.md` | Positioning, pricing and packaging, launch tiers, sales/CS enablement |
| Medium | `01_Problem_Discovery/07_Continuous_Discovery_and_JTBD.md` | Opportunity solution trees, JTBD interviews, assumption tests |
| Low | `00_PM_Career_Ladder.md` | APM → CPO expectations and evidence per level |

**Suggested sources to add (beyond the existing Resources):** *Inspired* and *Empowered* (Cagan), *Continuous Discovery Habits* (Torres), *Escaping the Build Trap* (Perri), *Competing Against Luck* (Christensen; JTBD), *Monetizing Innovation* (Ramanujam & Tacke; pricing), *Lean Analytics* (Croll & Yoskovitz), *Trustworthy Online Controlled Experiments* (Kohavi et al.).

## Module-by-Module Notes

| Module | Verdict | Gap / suggestion |
|---|---|---|
| 01 Problem Discovery | ✅ Good, sourced | Overview is a stub; add continuous discovery/JTBD |
| 02 Strategy | ✅ Good | Add go-to-market and pricing; overview unreachable |
| 03 Prioritization | ⚠️ Overlaps 04 and 06 | Overview is a stub; merge roadmap and stakeholder topics |
| 04 Roadmapping | ✅ Good (anti-pattern catalog) | Overview unreachable; deepen 02–03 |
| 05 Product Analytics | ✅ Good (pitfalls) | Deepen experimentation (818w) |
| 06 Requirements | ⚠️ Short notes (482–986w) | Overview unreachable; cross-link Senior module 02 |
| 07 Technical Partnership | ⚠️ Short notes (568–951w) | Add platform/API PM as a module |

## Quick Fixes

- [ ] Link all 7 module overviews from the role overview
- [ ] Add `created:` to all 49 notes
- [ ] Add a "Related" section to every topic
- [ ] Merge `03_Prioritization/05_Roadmap_Planning` into module 04
- [ ] Revisit `next_paths` (add Group PM / Director of Product / Founder)

## Related

- [[career-path/14_Product_Manager/00_overview|Product Manager overview]]
- [[career-path/13_Project_and_Program_Manager/audit|Project and Program Manager audit]]
- [[career-path/16_Developer_Advocate_and_Technical_Consultant/audit|Developer Advocate and Technical Consultant audit]]
- [[career-path/audit|Career path audit (summary)]]
