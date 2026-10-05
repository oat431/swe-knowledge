---
title: "Audit: Forward Deployed Engineer"
note_type: audit
career_path: forward-deployed-engineer
created: 2026-10-06
tags:
  - career-path
  - audit
  - forward-deployed
---

# Audit: Forward Deployed Engineer

> **Verdict:** The newest path (added 2026-10-05) and one of the most *timely*. The overview is the best-researched in the map: role origin, the enterprise-AI "deployment gap", sharp boundaries with Solutions Architect / Consultant / Independent, and dated sources. The modules cover the real job end to end: immersion and shadowing, demo-driven development with an explicit hardening path, integration and field debugging in environments you don't own, AI on customer data with residency constraints, security reviews and procurement as engineering problems, executive communication, and field-to-product feedback. It's also the **only path with a "career paths beyond" note**. Gaps: **no ethics topic** (0 notes) for a role embedded in other organizations' data, **no worked deployment case**, and thin coverage of the role's **personal sustainability** (travel, context switching, "going native").
>
> **Overall: 4 / 5**

## Snapshot

| Metric | Value |
|---|---|
| Notes | 57 (7 modules × 7 topics + 8 overviews) |
| Words | ~73k (avg ~1,290/note) |
| Wikilinks / navigation | All resolve ✅, all topics reachable ✅ |
| Frontmatter | ✅ `created` present (except the role overview) |
| Stale label | "Capability Areas **(planned)**" although all 7 modules exist |
| Fill-in templates / checklists | 49/49 · 49/49 ✅ |
| Scenario exercises / examples | 3/49 · 0/49 |
| Sources | 5 in the overview (incl. 2026 hiring data); 0 in topics |

## Scorecard

| Dimension | Score | Why |
|---|---|---|
| Coverage | 4/5 | The whole deployment lifecycle plus the commercial side; ethics and sustainability missing |
| Depth | 4/5 | Specific and field-real ("verdict, not vibes", "why pilots fail to convert"), though many notes have only 2–3 sections |
| Practice | 3/5 | Templates and checklists everywhere; no worked deployment, almost no scenarios |
| Progression | 4/5 | Clear entry (Senior SWE / Applied AI), evidence list, and a dedicated career-beyond note |
| Sources | 3/5 | Good, dated overview sources; topics cite nothing |
| Vault hygiene | 4/5 | Clean, apart from the stale "(planned)" label |

## ✅ What's Good

- **Positioning is crisp.** The overview explains why FDE is a distinct category (vendor-paid, embedded with one customer, accountable for production outcomes there, and for feeding learning back), and how it differs from Solutions Architect, Consultant, and Independent.
- **Prototype → production honesty.** `02_.../06_From_Prototype_to_Production` (what hardening actually adds) and `07_Choosing_the_Right_Engineering_Bar` (upgrading and downgrading rigor consciously) address the FDE's central tension.
- **"Engineering in someone else's house" is covered properly:** field debugging without owning the environment, customer release trains and freeze calendars, handover with a fade-out plan, and incidents at customer sites.
- **Enterprise navigation as engineering.** Security reviews with evidence packs, least-privilege access people can approve, residency decision tables, procurement and vendor lists, and partnering with customer security teams.
- **Commercial awareness without becoming sales.** Deployment economics, why pilots fail to convert, engineering for the renewal clock, and roadmap influence backed by cross-deployment patterns.
- **`07_.../07_Career_Paths_Beyond_the_Field`** is a pattern every path should copy.

## ❌ What's Missing

1. **Ethics in the field (0 notes).** FDEs see sensitive customer data, sit between vendor and customer interests, and deploy AI into consequential workflows. Missing: conflicts of interest, honest claims about AI capability, refusing or escalating harmful use cases, data-handling boundaries, and confidentiality across customers (the "one customer's secrets in another's deployment" risk). DevRel, Staff, EM, and Independent all have ethics notes; this path needs one most.
2. **A worked deployment case.** One end-to-end story would tie the 49 topics together: discovery → demo → feasibility → integration → security review → field eval → launch → handover → renewal.
3. **Personal sustainability (4 passing mentions).** Travel and on-site load, context switching between customer and home team, "going native" (over-identifying with the customer), and avoiding burnout in a high-intensity role.
4. **FDE hiring and interviews (4 mentions).** FDE loops often include problem-decomposition and customer-scenario rounds. A preparation note would fit next to the career-beyond note.
5. **Scenario practice.** Only 3/49 topics have exercises. Tabletop scenarios suit this role well (e.g., "The customer's CISO blocks the deployment two days before go-live").

## ⚠️ What to Improve

- **Remove "(planned)"** from "Capability Areas (planned)", and add `created` to the role overview.
- **Present the headline statistic with caveats.** "Roughly 95% of enterprise AI pilots produced no measurable business impact" comes from a single report whose methodology was widely debated. Note the caveat, and link the primary source rather than a third-party PDF mirror.
- **Add topic-level sources.** The overview cites five sources, but topics cite none (e.g., cite the OWASP LLM Top 10 in module 04, and enterprise integration and change-management references in modules 03/06).
- **Cross-link neighbors.** Module 04 overlaps the Applied AI path (RAG, evaluation, cost/latency, agents) and module 01 overlaps DevRel/Consultant discovery. Add "product-side version" links to the Applied AI notes.

## 🎯 Recommended Additions (prioritized)

| Priority | Proposed note / module | What it should contain |
|---|---|---|
| 🎯 High | `05_Enterprise_Navigation_Security_and_Compliance/08_Field_Ethics_and_Conflicts_of_Interest.md` | Vendor vs customer interests, honest AI claims, harmful-use escalation, cross-customer confidentiality |
| 🎯 High | `08_Worked_Deployment_Case_Study/` | One realistic deployment carried through all seven capability areas, with artifacts |
| Medium | `01_Field_Discovery_and_Problem_Framing/08_Sustainability_in_the_Field.md` | Travel load, context switching, going native, recovery rhythms |
| Medium | `07_Field_to_Product_and_Commercial_Awareness/08_FDE_Hiring_and_Interviews.md` | Decomposition and customer-scenario interviews, portfolio evidence |
| Low | Tabletop scenarios | One per module |

**Suggested sources to add (topic level):** OWASP Top 10 for LLM Applications (module 04), *Enterprise Integration Patterns* (Hohpe & Woolf; module 03), *Leading Change* (Kotter; module 06 adoption), *The Trusted Advisor* (Maister, Green & Galford; module 06), and public writing by FDE organizations on their operating models.

## Module-by-Module Notes

| Module | Verdict | Gap / suggestion |
|---|---|---|
| 01 Field Discovery & Problem Framing | ✅ Strong (shadowing, political mapping) | Add sustainability note |
| 02 Solution Design & Rapid Prototyping | ✅ Excellent (engineering-bar note) | — |
| 03 Integration & Deployment Engineering | ✅ Excellent | Cite integration references |
| 04 AI Systems in Customer Environments | ✅ Strong | Cross-link Applied AI notes |
| 05 Enterprise Navigation, Security & Compliance | ✅ Excellent | Add field ethics |
| 06 Customer Communication & Executive Influence | ✅ Strong | Add scenarios |
| 07 Field to Product & Commercial Awareness | ✅ Strong (career-beyond note) | Add hiring/interviews |

## Quick Fixes

- [ ] Remove "(planned)" from the role overview
- [ ] Add `created:` to the role overview
- [ ] Add a caveat to the 95% statistic and link the primary source
- [ ] Cross-link module 04 to the Applied AI path

## Related

- [[career-path/19_Forward_Deployed_Engineer/00_overview|Forward Deployed Engineer overview]]
- [[career-path/18_Applied_AI_Engineer/audit|Applied AI Engineer audit]]
- [[career-path/15_Solutions_and_Enterprise_Architect/audit|Solutions and Enterprise Architect audit]]
- [[career-path/16_Developer_Advocate_and_Technical_Consultant/audit|Developer Advocate and Technical Consultant audit]]
- [[career-path/audit|Career path audit (summary)]]
