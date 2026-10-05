---
title: "Audit: Software Architect"
note_type: audit
career_path: software-architect
created: 2026-10-06
tags:
  - career-path
  - audit
  - software-architect
---

# Audit: Software Architect

> **Verdict:** A rigorous, method-driven path built on the established architecture canon: quality-attribute workshops and scenarios, tactics, ATAM-style utility trees, sensitivity and trade-off points, views and viewpoints, C4, fitness functions, and secure-by-design. The "Architect vs Security / Data / SRE" boundary sections are a model the rest of the map should copy. The gaps are **modern core architect topics**: **API and integration architecture, domain-driven boundaries, legacy modernization, and architecting AI-enabled systems** (0 notes). It also has **no practice mechanism** (0 exercises, no katas), and the ideas it builds on are rarely credited.
>
> **Overall: 4 / 5**

## Snapshot

| Metric | Value |
|---|---|
| Notes | 57 (7 modules × 7 topics + 8 overviews) |
| Words | ~81k (avg ~1,430/note) |
| Wikilinks / navigation | All resolve ✅, all topics reachable ✅ |
| Frontmatter | Consistent; only the role overview lacks `created` |
| Fill-in templates / checklists | 49/49 · 49/49 ✅ |
| Scenario exercises | **0/49** |
| Example sections | 13/49 (mostly quality-attribute scenarios) |
| Sources | 0/49 topics have a sources section |

## Scorecard

| Dimension | Score | Why |
|---|---|---|
| Coverage | 4/5 | Classic architecture is fully covered; several modern core topics are missing |
| Depth | 5/5 | Method-level detail (utility trees, tactic catalogs, viewpoint selection tables, fitness functions) |
| Practice | 3/5 | Templates everywhere, but nothing to practice on: no katas or design exercises |
| Progression | 3/5 | Strong evidence list; the Senior → Architect transition is not layered (topics repeat Senior's module 03) |
| Sources | 2/5 | ATAM/SEI and C4 are named; Ford/Parsons/Kua, Nygard, Richards/Ford, Kleppmann, and Dehghani are not |
| Vault hygiene | 5/5 | Clean |

## ✅ What's Good

- **Quality attributes drive architecture.** Module 02 turns stakeholder needs into measurable scenarios, then into tactics for performance, availability, security, modifiability, and deployability, then into explicit trade-offs. This is the most valuable habit an architect can learn.
- **Evaluation produces evidence, not opinion.** Module 04 covers a method catalog, utility trees, sensitivity and trade-off points, an anti-pattern catalog, continuous evaluation with **fitness functions**, and reports that document *non-risks* too.
- **Clear role boundaries.** The security, data, and operations modules each include an "Architect vs Security Engineer / Data and ML Engineer / SRE" table, which prevents scope confusion with paths 07–09.
- **Architecture stays tied to running systems.** Module 07 covers deployment views, resilience decision records, observability as an architecture contract, IaC as executable architecture, and **FinOps as architecture governance** (the map's best cost coverage).
- **Realistic about the role.** "The Architect as Enabler, Not Gatekeeper" and "Just-Enough Architecture" counter the ivory-tower failure mode.

## ❌ What's Missing

1. **AI-enabled system architecture (0 notes).** Every "ML" mention is a link to the Data/ML path. In 2026, architects must handle model gateways, RAG and agent architectures, non-determinism as a quality attribute, evaluation suites as fitness functions, inference cost and latency, and guardrails as structural components.
2. **API and integration architecture.** APIs are mentioned in 9 notes but never taught: REST/gRPC/GraphQL/async trade-offs, contract-first design, versioning and deprecation, gateways, and backward-compatibility policy.
3. **Domain-driven boundaries.** DDD, bounded contexts, context mapping, and event storming appear in only 2 notes, although drawing boundaries is the architect's core act.
4. **Legacy modernization and migration.** "Migration and modernization strategy" is in the overview's evidence list, but no note teaches it (strangler fig, branch by abstraction, monolith decomposition, data migration sequencing).
5. **Distributed-systems consistency as a decision.** Sagas, idempotency, and consistency models are scattered across 10 notes but never framed as an architect's decision guide.
6. **Practice.** There are no exercises and no architecture katas. Staff's `06_Teaching_Architecture_Thinking` recommends katas, but this path, where they belong most, has none.

## ⚠️ What to Improve

- **Layer the path over Senior's module 03.** The Senior path's `03_Architecture_and_Design_Judgment` already covers ADRs, quality-attribute trade-offs, architecture evaluation, governance, and communication. Add a "Senior version → Architect version" line to each overlapping note so the jump in rigor is explicit.
- **Credit the canon.** The methods come from identifiable works (listed below). A Sources section per module would make the path auditable and point readers to the originals.
- **Move the ADR material to one home.** ADRs are taught in Senior 03/04, Tech Lead 03/02, Architect 03/05, and Architect 05/06 (security ADRs). Pick a canonical note and link to it from the others.
- **Rename the stale heading** "Suggested Future Note Route" → "Background Reading"; add `created` to the role overview.

## 🎯 Recommended Additions (prioritized)

| Priority | Proposed note / module | What it should contain |
|---|---|---|
| 🎯 High | `08_AI_Enabled_System_Architecture/` | `01_AI_Components_as_Architecture_Elements`, `02_Non_Determinism_as_a_Quality_Attribute`, `03_Evaluation_as_Fitness_Function`, `04_Model_Gateway_and_Inference_Cost`, `05_Guardrails_as_Structure`; cross-link to [[career-path/18_Applied_AI_Engineer/00_overview\|Applied AI Engineer]] |
| 🎯 High | `01_Architecture_Fundamentals/08_Domain_Driven_Boundaries.md` | Bounded contexts, context maps, event storming, and aligning domains with teams (Conway) |
| 🎯 High | `06_Data_Architecture/08_API_and_Integration_Architecture.md` (or a new module) | Interface styles, contract-first design, versioning and deprecation policy, gateways, compatibility guarantees |
| Medium | `04_Architecture_Evaluation_and_Trade_Offs/08_Modernization_and_Migration_Strategy.md` | Strangler fig, branch by abstraction, decomposition sequencing, data migration, and when *not* to modernize |
| Medium | `02_Quality_Attribute_Analysis/08_Consistency_and_Distribution_Decisions.md` | Consistency models, sagas vs 2PC, idempotency, and a decision guide |
| Medium | Architecture katas | One kata per module (requirements brief → candidate architecture → evaluation) |

**Suggested sources to cite:** *Software Architecture in Practice* (Bass, Clements & Kazman), *Documenting Software Architectures: Views and Beyond* (Clements et al.), *Fundamentals of Software Architecture* (Richards & Ford), *Building Evolutionary Architectures* (Ford, Parsons, Kua & Sadalage), *Designing Data-Intensive Applications* (Kleppmann), *Domain-Driven Design* (Evans), *Data Mesh* (Dehghani), Michael Nygard's original ADR post, and Simon Brown's c4model.com.

## Module-by-Module Notes

| Module | Verdict | Gap / suggestion |
|---|---|---|
| 01 Architecture Fundamentals | ✅ Strong | Add domain-driven boundaries |
| 02 Quality Attribute Analysis | ✅ Excellent | Add consistency/distribution decisions and AI non-determinism |
| 03 Architecture Description & Views | ✅ Good (shortest notes; ADR note is 905 words) | Make one ADR note canonical across the map |
| 04 Evaluation & Trade-Offs | ✅ Excellent | Add modernization strategy |
| 05 Security Architecture | ✅ Strong | Cross-link to Security Engineer `02_Secure_Architecture_and_Design` |
| 06 Data Architecture | ✅ Strong | Add API/integration architecture |
| 07 Operations & Infrastructure | ✅ Strong (best FinOps coverage in the map) | — |

## Quick Fixes

- [ ] Add `created:` to the role overview
- [ ] Add a Sources section to each module overview
- [ ] Add "Senior version" links to overlapping notes (ADRs, trade-offs, evaluation)
- [ ] Rename "Suggested Future Note Route" → "Background Reading"

## Related

- [[career-path/06_Software_Architect/00_overview|Software Architect overview]]
- [[career-path/02_Senior_Software_Engineer/audit|Senior Software Engineer audit]]
- [[career-path/15_Solutions_and_Enterprise_Architect/audit|Solutions and Enterprise Architect audit]]
- [[career-path/audit|Career path audit (summary)]]
