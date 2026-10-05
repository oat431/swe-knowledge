---
title: "Audit: Solutions and Enterprise Architect"
note_type: audit
career_path: solutions-and-enterprise-architect
created: 2026-10-06
tags:
  - career-path
  - audit
  - solutions-architect
  - enterprise-architect
---

# Audit: Solutions and Enterprise Architect

> **Verdict:** A comprehensive, TOGAF/BABOK/DMBOK-aligned path with strong *enterprise* content: capability maps, value streams, current → target → transition architectures, application portfolio rationalization, migration and coexistence strategies, run-vs-change funding, and a first-90-days plan for building an EA practice. The *solutions* half is weaker on the market's version of the job. **Cloud architecture is nearly absent (3 notes)**, even though most "Solutions Architect" roles today are cloud or pre-sales roles. There's also **no enterprise AI content**, no **ArchiMate**, and almost no exercises or citations.
>
> **Overall: 3.5 / 5**

## Snapshot

| Metric | Value |
|---|---|
| Notes | 57 (7 modules × 7 topics + 8 overviews) |
| Words | ~70k (avg ~1,230/note) |
| Wikilinks / navigation | All resolve ✅, all topics reachable ✅ |
| Frontmatter | Consistent; only the role overview lacks `created` |
| Fill-in templates / checklists | 49/49 · 49/49 ✅ |
| Scenario exercises / examples | 5/49 · 1/49 |
| Sources | 1/49 topics; overview cites TOGAF + BOKs |

## Scorecard

| Dimension | Score | Why |
|---|---|---|
| Coverage | 3/5 | EA practice is complete; cloud, pre-sales, enterprise AI, and modeling standards are missing |
| Depth | 4/5 | Solid structure (e.g., disposition options for retirement, coexistence patterns, cutover design) |
| Practice | 3/5 | Templates and checklists everywhere; few scenarios |
| Progression | 3/5 | Good SA vs EA table and EA first 90 days; no certification guidance (TOGAF, cloud SA) |
| Sources | 1/5 | TOGAF is named in the overview; topics rarely cite |
| Vault hygiene | 5/5 | Clean |

## ✅ What's Good

- **Clear SA vs EA distinction** (scope, main question, horizon, stakeholders, artifacts), which helps readers choose a direction.
- **Business architecture done properly.** Business needs → capability mapping → value streams → requirements-to-architecture traceability → options analysis and business case.
- **Transformation is treated as survivable change.** Transition architectures, roadmap-as-decision-instrument, application portfolio management that makes retirement happen, migration and coexistence patterns, and cutover design.
- **EA as a practice, not a document.** `07_Building_the_EA_Practice` covers operating models, team shape, maturity, a first 90 days, an engagement model, and measuring the practice. Right-sized governance avoids the ivory-tower trap.
- **Vendor realism.** Third-party and fourth-party concentration risk, "Communicating Requirements, Not Wants", and "Evaluating Claims" in vendor conversations.

## ❌ What's Missing

1. **Cloud solution architecture (3 notes).** The market's dominant SA role. Missing: Well-Architected Frameworks (AWS/Azure/GCP), landing zones and account/subscription structure, migration strategies (the "R" models: rehost, replatform, refactor, …), cloud cost architecture, and hybrid/multi-cloud trade-offs. The Software Architect path's `07_.../04_Cloud_and_Infrastructure_Architecture` exists but is solution-scoped.
2. **Pre-sales and customer-facing SA work.** RFP/RFI responses, technical discovery calls, POCs, solution proposals and SOW inputs, and demo design. These are mentioned in 14 notes but never taught as the job. (The Forward Deployed Engineer and Developer Advocate/Consultant paths cover adjacent ground; cross-link them.)
3. **Enterprise AI (1 note).** AI use-case portfolio and capability mapping, AI platform reference architecture, AI governance and risk tiers (e.g., EU AI Act-style classification), and data readiness for AI.
4. **Modeling standards and tooling (0 notes).** ArchiMate (The Open Group's EA modeling language) and EA repositories/tools (LeanIX, Ardoq, Sparx, BizzDesign).
5. **Finance vocabulary.** CAPEX/OPEX, TCO, and depreciation appear in 1 note, although `05_Investment_and_Funding_Models` needs them.
6. **Certification route.** TOGAF and cloud architect certifications often gate SA/EA hiring; 6 notes mention certifications, but none gives guidance.

## ⚠️ What to Improve

- **Add scenario exercises.** Only 5/49 topics have one. This path suits case studies (e.g., "Rationalize a 300-application portfolio after a merger").
- **Credit the canon** (see below), especially for capability mapping, value streams, and integration patterns.
- **Clarify scope with the Software Architect path and Principal module 02.** Add a "Software Architect vs Solutions Architect vs Enterprise Architect" row set to the boundary table, since capability-based planning also appears in Principal `02_.../01_Enterprise_Systems_Thinking`.
- **Rename the stale heading** "Suggested Future Note Route" → "Background Reading"; add `created` to the role overview.

## 🎯 Recommended Additions (prioritized)

| Priority | Proposed note / module | What it should contain |
|---|---|---|
| 🎯 High | `08_Cloud_Solution_Architecture/` | `01_Well_Architected_Reviews`, `02_Landing_Zones_and_Account_Structure`, `03_Cloud_Migration_Strategies`, `04_Cloud_Cost_Architecture`, `05_Hybrid_and_Multi_Cloud_Trade_Offs` |
| 🎯 High | `03_Enterprise_Architecture_Practice/08_Enterprise_AI_Architecture_and_Governance.md` | AI use-case portfolio, platform reference architecture, risk tiers, data readiness |
| Medium | `02_Solution_Architecture_and_Design/08_Pre_Sales_Solution_Architecture.md` | Discovery calls, RFP responses, POCs, proposals, demo design; cross-link [[career-path/19_Forward_Deployed_Engineer/00_overview\|Forward Deployed Engineer]] |
| Medium | `07_Architecture_Communication/08_ArchiMate_and_EA_Tooling.md` | Core ArchiMate viewpoints, repository practices, tool selection |
| Medium | `06_Transformation_Roadmaps_and_Portfolio/08_Finance_for_Architects.md` | CAPEX/OPEX, TCO, depreciation, chargeback/showback |
| Low | `00_Certifications_and_Career_Route.md` | TOGAF, cloud SA certs, and when they matter |

**Suggested sources to cite:** The Open Group *TOGAF Standard* and *ArchiMate Specification*, *Enterprise Architecture as Strategy* (Ross, Weill & Robertson), *The Software Architect Elevator* (Hohpe), *Enterprise Integration Patterns* (Hohpe & Woolf), BIZBOK (Business Architecture Guild), AWS/Azure/GCP Well-Architected Frameworks, DMBOK v2.

## Module-by-Module Notes

| Module | Verdict | Gap / suggestion |
|---|---|---|
| 01 Business Analysis & Capability Mapping | ✅ Strong | Cite BIZBOK/BABOK per topic |
| 02 Solution Architecture & Design | ✅ Good | Add pre-sales SA and cloud-specific design |
| 03 Enterprise Architecture Practice | ✅ Excellent (EA first 90 days) | Add enterprise AI; ArchiMate |
| 04 Enterprise Data Architecture | ✅ Strong | Cross-link Data/ML and Software Architect module 06 |
| 05 Security, Risk & Compliance | ✅ Good (supply-chain risk) | Add AI risk tiers |
| 06 Transformation Roadmaps & Portfolio | ✅ Strong (migration/coexistence) | Add finance vocabulary |
| 07 Architecture Communication | ✅ Strong | Add ArchiMate viewpoints |

## Quick Fixes

- [ ] Add `created:` to the role overview
- [ ] Add a Sources section to each module overview
- [ ] Add a Software / Solutions / Enterprise Architect boundary table
- [ ] Rename "Suggested Future Note Route" → "Background Reading"

## Related

- [[career-path/15_Solutions_and_Enterprise_Architect/00_overview|Solutions and Enterprise Architect overview]]
- [[career-path/06_Software_Architect/audit|Software Architect audit]]
- [[career-path/04_Principal_and_Distinguished_Engineer/audit|Principal and Distinguished Engineer audit]]
- [[career-path/audit|Career path audit (summary)]]
