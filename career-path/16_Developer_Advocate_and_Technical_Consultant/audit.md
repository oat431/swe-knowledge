---
title: "Audit: Developer Advocate and Technical Consultant"
note_type: audit
career_path: developer-advocate-and-technical-consultant
created: 2026-10-06
tags:
  - career-path
  - audit
  - developer-relations
  - consulting
---

# Audit: Developer Advocate and Technical Consultant

> **Verdict:** A well-rounded, ethically grounded path. It covers explaining at the right "altitude", documentation as a product, workshops and curriculum design, support-signal archaeology, DX friction audits, community health and moderation, and the consultant's "guide, don't build" boundary. Its gaps are about **2026 realities** and **the business side**. It has **no AI-era DevRel** (developers increasingly meet a product through AI assistants and agents, so docs must serve them too), **none of the consultancy mechanics** (utilization, SOWs, engagement management), and it **merges two different jobs** without helping the reader choose between them.
>
> **Overall: 3.5 / 5**

## Snapshot

| Metric | Value |
|---|---|
| Notes | 57 (7 modules × 7 topics + 8 overviews) |
| Words | ~71k (avg ~1,240/note) |
| Wikilinks / navigation | All resolve ✅, all topics reachable ✅ |
| Frontmatter | Consistent; only the role overview lacks `created` |
| Fill-in templates / checklists | 49/49 · 49/49 ✅ |
| Scenario exercises / examples | 2/49 · 2/49 |
| Sources | 2/49 topics; the documentation framework (Diátaxis) is used but not credited |

## Scorecard

| Dimension | Score | Why |
|---|---|---|
| Coverage | 4/5 | Communication, docs, facilitation, research, community, and feedback complete; AI-era DevRel and consulting business missing |
| Depth | 4/5 | Concrete loops and taxonomies (altitude model, participation ladder, metric ladder, evidence packages) |
| Practice | 3/5 | Templates and checklists everywhere; almost no exercises (a teaching-heavy role should have the most) |
| Progression | 3/5 | Good evidence list; no Advocate vs Consultant choice, no DevRel org/career model |
| Sources | 1/5 | DevRel, documentation, and consulting classics are barely cited |
| Vault hygiene | 5/5 | Clean |

## ✅ What's Good

- **Accuracy and ethics as first-class topics.** `01_.../07_Communication_Ethics_and_Accuracy` (corrections protocol, pressure situations) and `05_.../07_Consulting_Boundaries_and_Ethics` protect the role's core asset: trust.
- **Documentation as a product.** The four-type documentation model, information architecture and search, troubleshooting pages mined from real failures, migration guides, and keeping examples alive.
- **Research discipline.** Interview craft with bias countermeasures, observing real usage, support-signal archaeology with "quantification without nonsense", and developer segmentation.
- **Measurement that resists vanity.** `06_.../07_Measuring_Community_Impact` and `07_.../06_Advocacy_Metrics_and_Reporting` separate health metrics from vanity metrics and address attribution honestly.
- **Field-to-product influence.** Evidence packages, roadmap-season rhythm, and DX advocacy make the advocate a credible internal voice rather than a complaint channel.

## ❌ What's Missing

1. **AI-era developer relations (1 mention).** In 2026, developers often discover and integrate products *through* AI coding assistants and agents. Missing: documentation for LLM consumption (structured, chunkable docs; `llms.txt`-style indexes), agent integrations (e.g., MCP servers) as a new developer surface, measuring adoption that happens inside AI tools, and the ethics of AI-generated content in advocacy.
2. **Consulting business mechanics (0 notes).** For a Technical Consultant at a consultancy or vendor: utilization and billability, SOW scoping, engagement management, escalations with account teams, and the consulting career ladder. (The Independent Consulting path covers this for solo practitioners only.)
3. **Choosing between the two jobs.** Developer Advocate (content, community, feedback) and Technical Consultant (discovery, solution guidance, implementation oversight) have different days, metrics, and next steps. No note helps the reader decide or describes hybrid roles (Solutions Engineer, Customer Engineer).
4. **DevRel as an organization.** Where DevRel reports (marketing vs product vs engineering) and how that shapes metrics, team structures, the DevRel ladder, and sustainable travel and on-stage load. These are mentioned in about 11 notes but never consolidated.
5. **Docs-as-code and technical writing as a career (0 notes).** Doc toolchains, review workflows, and a note on Technical Writer as an adjacent path.
6. **Exercises.** For the map's most teaching-oriented role, only 2/49 topics have one.

## ⚠️ What to Improve

- **Credit the frameworks.** The "Documentation Quadrant" (tutorials / how-to / reference / explanation) is Diátaxis (Procida). Credit it and the other classics listed below.
- **Add a practice project per module,** e.g., write a tutorial and run a usability test of it; run a 60-minute workshop for mixed levels; produce a field-signal synthesis.
- **Cross-link neighbors.** `05_Solution_Guidance` overlaps the Forward Deployed Engineer path's modules 01–03, and `04_.../01_Developer_and_Customer_Research` overlaps the Product Manager path's discovery module. Add boundary notes.
- **Rename the stale heading** "Suggested Future Note Route" → "Background Reading"; add `created` to the role overview.

## 🎯 Recommended Additions (prioritized)

| Priority | Proposed note / module | What it should contain |
|---|---|---|
| 🎯 High | `08_AI_Era_Developer_Relations/` | `01_Docs_for_Humans_and_Agents`, `02_Agent_Integrations_as_Developer_Surface`, `03_Measuring_Adoption_Through_AI_Tools`, `04_AI_Generated_Content_Ethics` |
| 🎯 High | `00_Advocate_vs_Consultant_vs_Solutions_Engineer.md` | Day-to-day work, metrics, org placement, ladders, and next steps for each |
| Medium | `05_Solution_Guidance/08_Consulting_Engagement_Mechanics.md` | Utilization, SOW scoping, engagement management, account-team partnership |
| Medium | `02_Documentation_and_Learning_Materials/08_Docs_as_Code_Toolchain.md` | Docs in repos, review flows, link checking, versioned docs, doc testing |
| Medium | `06_Community_and_Ecosystem/08_DevRel_Team_and_Sustainability.md` | Reporting lines and metrics, team models, travel load, burnout prevention |
| Low | Practice projects | One per module |

**Suggested sources to cite:** Diátaxis (Procida), *Docs for Developers* (Bhatti et al.), Google Developer Documentation Style Guide, *The Business Value of Developer Relations* (Thengvall), *Developer Relations* (Lewko & Parton), *People Powered* and *The Art of Community* (Bacon), *Working in Public* (Eghbal), *The Trusted Advisor* (Maister, Green & Galford), *Flawless Consulting* (Block).

## Module-by-Module Notes

| Module | Verdict | Gap / suggestion |
|---|---|---|
| 01 Technical Communication | ✅ Strong (ethics note) | Add AI-generated content ethics |
| 02 Documentation & Learning Materials | ✅ Strong | Credit Diátaxis; add docs-as-code and docs for agents |
| 03 Facilitation & Enablement | ✅ Strong (curriculum design) | — |
| 04 Customer & Developer Understanding | ✅ Excellent (support-signal archaeology) | Boundary with PM discovery |
| 05 Solution Guidance | ✅ Good | Add engagement mechanics; boundary with FDE |
| 06 Community & Ecosystem | ✅ Strong (moderation, impact) | Add team sustainability |
| 07 Product Feedback & Ecosystem Strategy | ✅ Strong | Add adoption via AI tools |

## Quick Fixes

- [ ] Credit Diátaxis in `02_Documentation_and_Learning_Materials/00_overview`
- [ ] Add `created:` to the role overview
- [ ] Add boundary notes to FDE and PM where topics overlap
- [ ] Rename "Suggested Future Note Route" → "Background Reading"

## Related

- [[career-path/16_Developer_Advocate_and_Technical_Consultant/00_overview|Developer Advocate and Technical Consultant overview]]
- [[career-path/19_Forward_Deployed_Engineer/audit|Forward Deployed Engineer audit]]
- [[career-path/17_Independent_Consulting_and_Technical_Founder/audit|Independent Consulting and Technical Founder audit]]
- [[career-path/audit|Career path audit (summary)]]
