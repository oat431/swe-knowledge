---
title: "Audit: Career Path (Summary)"
note_type: audit
scope: career-path
created: 2026-10-06
tags:
  - career-path
  - audit
  - summary
---

# Audit: Career Path (Summary)

> **Short answer: yes, this is a good guide, and in places an excellent one.** Across 19 paths, 124 modules, and 821 topic notes (~1.28M words), it consistently teaches *what changes at the next level*, gives you artifacts to use, and has **zero broken links**. The leadership and IC-progression paths (Staff, Tech Lead, Engineering Manager, TPM, Principal) are genuinely strong.
>
> **Where it falls short:** the **entry point** (Software Engineer is a single page, and "mid-level" is used 218 times but never defined), **three specialist paths** that are thin or structurally off (Quality, SRE, Data/ML), and **2026 currency**, since AI is nearly absent outside the two AI paths. Practice material and sources are uneven across the map, and six paths have navigation bugs.
>
> **Overall: 3.5 / 5** (average of the 19 path scores; range 2 to 4)

## Scoreboard

| # | Path | Score | Strongest point | Biggest gap | Audit |
|---|---|---|---|---|---|
| 01 | Software Engineer | 2 | Correct lifecycle framing | Overview only; mid-level undefined | [[career-path/01_Software_Engineer/audit\|audit]] |
| 02 | Senior Software Engineer | 4 | Capstone module; exercises in two-thirds of topics | Promotion module targets Staff; no system-design depth | [[career-path/02_Senior_Software_Engineer/audit\|audit]] |
| 03 | Staff Engineer | 4 | Reads like an operator's manual | Staying technical; AI adoption; Staff promotion | [[career-path/03_Staff_Engineer/audit\|audit]] |
| 04 | Principal & Distinguished | 4 | Board-level, M&A, geopolitics | Hands-on depth; enterprise AI; empty `next_paths` | [[career-path/04_Principal_and_Distinguished_Engineer/audit\|audit]] |
| 05 | Tech Lead | 4 | Player-coach, TL–EM split, incident command | Security, cost, and AI norms for the team | [[career-path/05_Tech_Lead/audit\|audit]] |
| 06 | Software Architect | 4 | Rigorous quality-attribute analysis and evaluation | AI architecture; API; DDD; no katas | [[career-path/06_Software_Architect/audit\|audit]] |
| 07 | SRE & Platform | 3 | SLO stack; exercise in every note | Thinnest path; toil/troubleshooting/PRR; platform half | [[career-path/07_SRE_and_Platform_Engineer/audit\|audit]] |
| 08 | Security Engineer | 3.5 | Best role overview in the map | AI/LLM security, OWASP, cloud | [[career-path/08_Security_Engineer/audit\|audit]] |
| 09 | Data & ML Engineer | 3 | DMBOK breadth; exercises | Shortest notes; ML is 1 of 7 modules | [[career-path/09_Data_and_ML_Engineer/audit\|audit]] |
| 10 | Quality & Test | 2.5 | Test-design module | Textbook framing; modules unreachable | [[career-path/10_Quality_and_Test_Engineering/audit\|audit]] |
| 11 | Engineering Manager | 4 | Hiring end to end | New-manager onboarding; managing managers | [[career-path/11_Engineering_Manager/audit\|audit]] |
| 12 | Technical Program Manager | 4 | Technical integration + benefits | Agile cadence; launches; AI | [[career-path/12_Technical_Program_Manager/audit\|audit]] |
| 13 | Project & Program Manager | 3 | EVM, CPM, gates, programs | Agile/hybrid (0 notes); team leadership | [[career-path/13_Project_and_Program_Manager/audit\|audit]] |
| 14 | Product Manager | 3 | Resources in every topic | SWE→PM; platform PM; AI PM; navigation | [[career-path/14_Product_Manager/audit\|audit]] |
| 15 | Solutions & Enterprise Architect | 3.5 | EA practice | Cloud SA; enterprise AI; ArchiMate | [[career-path/15_Solutions_and_Enterprise_Architect/audit\|audit]] |
| 16 | Developer Advocate & Consultant | 3.5 | Ethics; docs as a product | AI-era DevRel; consulting mechanics | [[career-path/16_Developer_Advocate_and_Technical_Consultant/audit\|audit]] |
| 17 | Independent & Founder | 3.5 | Solo-firm economics | Technical-founder journey; exits | [[career-path/17_Independent_Consulting_and_Technical_Founder/audit\|audit]] |
| 18 | Applied AI Engineer | 4 | Evals and guardrails; honest scope | Agentic systems; agent evals | [[career-path/18_Applied_AI_Engineer/audit\|audit]] |
| 19 | Forward Deployed Engineer | 4 | Best-researched overview; career-beyond note | Field ethics; worked case | [[career-path/19_Forward_Deployed_Engineer/audit\|audit]] |

Scores combine six dimensions rated in each path audit: coverage, depth, practice, progression, sources, and vault hygiene.

## ✅ What the Map Does Well (keep these)

1. **Level-delta framing.** Most paths say what changes at the next level: "mid-level vs senior", "Staff vs Principal", "What changes from Tech Lead to EM", "Senior vs Architect". This is the main thing that makes it a *career* guide rather than a textbook.
2. **Consistent architecture.** Role overview → 6–9 modules → 5–8 numbered topics, so the same reading habit works everywhere.
3. **Integrated with the vault.** Overviews anchor to the SWEBOK, PMBOK, BABOK, DMBOK, CyBOK, and SEBoK notes, and **all 6,449 wikilinks resolve**.
4. **Usable artifacts.** 631/821 topics include a checklist and 537 include a fill-in template.
5. **Honest, ethical tone.** Staff, EM, DevRel, and Independent each have ethics notes; risk is accepted *in writing*; status and benefits reporting is honest.
6. **Original content.** Only 2 cross-path near-duplicate pairs exist (>10% shared 8-word sequences).
7. **Best-in-map patterns to spread to every path:**
   - Security's role overview structure (mid vs senior table, evidence-linked checklist, role-boundary table, "artifact → what it demonstrates")
   - FDE's `07_Career_Paths_Beyond_the_Field`
   - Product Manager's per-topic Resources
   - Senior's capstone module
   - Staff's and Tech Lead's first-90-days notes
   - The exercise in every note in SRE, Security, Data/ML, and Applied AI
   - Software Architect's "Architect vs X" boundary tables

## ❌ Systemic Gaps (each affects many paths)

### 1. The entry point and level definitions

- `01_Software_Engineer` is a 509-word overview, while every other path has 37–71 notes.
- "Mid-level" is the comparison baseline in 218 places (Senior, Security, Data/ML, Applied AI) but is never defined. The only level map in the vault is Tech Lead's `05_.../02_Growing_Engineers_at_Levels`.
- Senior's `09_Promotion_Evidence_and_Capstone` targets **Senior → Staff**, so **the most common promotion (mid → senior) is covered nowhere**.

### 2. AI currency (2026)

| Topic | Coverage today |
|---|---|
| AI-assisted development as a daily engineering skill | **0 notes anywhere** (only EM's "AI-Assisted Drafting Ethics" section) |
| Security Engineer: AI/LLM attack surface | 0 notes (Applied AI has the only AI-security module) |
| Software Architect: AI-enabled system architecture | 0 notes |
| Tech Lead / TPM / Project Manager | 0 notes each |
| Staff / Principal / EM: leading AI adoption | 1 passing mention each, or regulation/foresight only |
| Product Manager | AI appears only as example features |
| Quality: testing AI features | 2 notes in passing |
| Applied AI: agentic systems, agent evals | Thin (agent evals: 0) |

**Recommendation:** an "AI thread" with one level-appropriate note per path. Each path audit names the exact note.

### 3. Practice material is split by authoring batch

| Pattern | Paths | Has | Lacks |
|---|---|---|---|
| Exercise-first | SRE, Security, Data/ML, Applied AI | An exercise in ~every note | Checklists and templates in topics |
| Template-first | Staff, Principal, Tech Lead, Architect, EM, TPM, Project Mgr, SA/EA, DevRel, Independent, FDE | A checklist and template in ~every note | Exercises (0–10% of topics) |
| Mixed | Senior, Quality, Product Manager | Some of both | Consistency |

Worked examples and case studies are rare everywhere: no path walks one realistic case end to end. **Target state:** every topic has a fill-in artifact *and* a scenario exercise, and every path has one worked case study.

### 4. Sources are almost absent

Excluding code-sample URLs (`example.com`, `localhost`), there are only 52 external links across 965 notes. Only Product Manager cites resources per topic (as book titles rather than links). Ideas from well-known works are used without credit: Larson and Reilly (Staff), Google SRE (SRE), OWASP (Security), Diátaxis (DevRel), Kleppmann (Data, Architect), Edmondson (EM), Nygard's ADRs (four paths). **Minimum fix:** a Sources section in every module overview. Each path audit lists suggested sources.

### 5. Career mechanics (getting in, starting well, moving on)

- **First 90 days:** Staff and Tech Lead only (plus an EA-practice section). Missing for EM, where the IC → manager switch makes it most needed.
- **Interviewing for the role:** no path covers it (EM's interview note is about hiring others).
- **Career paths beyond:** FDE only.
- **Certification guidance:** none, although it matters for Project Manager, SA/EA, Security, and cloud roles.

### 6. The progression graph is inconsistent

- **59 one-directional links** between `entry_from` and `next_paths`. Senior is the hub, yet its `next_paths` lists 6 paths while 15 paths list Senior as their entry. Staff lists only Senior as an entry, though 10 paths point to Staff.
- **Empty `next_paths`:** Principal and Independent.
- **Odd primary routes:** EM → Principal/TPM, TPM → Project Manager, Product Manager → Project Manager.
- **Root mermaid diagram:** missing edges (Applied AI → FDE, Staff → SA/EA, Product Manager → Founder), and an "Engineering Director and beyond" node with no folder.

### 7. Missing or candidate paths

The map is missing paths on two axes:

- **Role paths** the current map already implies: **Engineering Director / VP / CTO** (the management ladder has 1 rung while the IC ladder has 3), **Platform Engineer** and **ML Engineer** (each is a minor part of a combined path today), and **Solutions & Sales Engineer** (pre-sales appears in only 2 notes).
- **Domain tracks**, which the map doesn't model at all: **Frontend & UI**, **Mobile**, and **Embedded & Safety-Critical**. These describe *where* engineers apply their skills, so they belong in a separate layer, not alongside Staff or EM.

The prioritized proposal, with module outlines, graph wiring, and rules for agents, is in [[#Handoff for Agents]].

### 8. Repeated topics without a progression view

The same topics recur at different levels, which is fine because text duplication is low. But readers can't see the progression between versions.

- **ADRs:** Senior 03/04, Tech Lead 03/02, Architect 03/05, Architect 05/06
- **Incident response:** Senior 05/04, Tech Lead 07, SRE 03, Security 06, FDE 03/07
- **Estimation:** Senior 04/01, Tech Lead 04/02, Quality 01/04, Product Manager 03/02
- **Stakeholders:** Senior 02/03, TPM 05, Project Manager 05, SA/EA 01/05, Product Manager 03/04
- **Mentoring:** Senior 07, Tech Lead 05, Staff 07, Principal 06, EM 01
- **Staff ↔ Principal:** 6 near-identical topic pairs at larger scope

**Recommendation:** add `career-path/00_Topic_Progression_Index.md`, mapping each recurring topic to its level-specific notes and the "delta" between them.

## ⚠️ Vault Hygiene Summary

| Issue | Where | Count | Fix |
|---|---|---|---|
| Topics not clickable from their module overview (`` `file.md` `` instead of `[[link]]`) | Senior 01/02/08, SRE, Security, Data/ML | **87 topics** | Convert to wikilinks |
| Module overviews not linked from the role overview | Quality (6), Product Manager (7) | 13 | Link from the Capability Areas table |
| Notes with **no inbound links at all** | Quality 6, Product Manager 3, SRE 1, Security 1 | 11 | Link them |
| `created` missing | All 19 role overviews + root; all of Security, Data/ML, Product Manager, Applied AI; most of SRE and Senior | **279 / 965** | Add `created:` |
| Mixed frontmatter schemas | Senior alone uses 7 key sets (`note_type` vs `type`, `career_path` vs `role`, …) | — | Adopt one schema |
| Date typo | Senior `09_...` (4 notes): `2026-01-05` → git says `2026-08-05` | 4 | Correct |
| Absolute paths to a folder outside the vault | Security (`F:\obsidian_note\document_template\...`) | 89 refs / 33 notes | `obsidian://` URIs or in-vault copies |
| Wikilink targets with spaces (vault convention) | 161 notes → 53 targets in BOK / SWEBOK folders | 472 links (7%) | Rename the *target* files vault-wide (Obsidian updates links); outside `career-path` |
| Stale labels | Root says "career overviews only"; "Suggested Future Note Route" in 18/19 role overviews; "(planned)" in Applied AI and FDE; Quality trackers show "- [ ] … - Complete" | — | Rename or remove |
| Malformed table | Senior role overview (`\|\|` header → 4 cells vs 3) | 1 | Fix header |
| Outdated reference | Quality accessibility note uses WCAG 2.1 (2.2 is current since Oct 2023) | 1 | Update |

## 🎯 Recommended Roadmap

**Phase 0: Hygiene sprint.** Mostly scriptable; one session. Everything in the table above.

**Phase 1: Highest-impact content**
1. Build out `01_Software_Engineer`: junior → mid leveling, daily engineering craft, AI-assisted engineering, and a **mid → senior promotion** guide (and re-target Senior's module 09).
2. **AI thread** across paths: AI-assisted dev (SWE/Senior/TL), AI security (Security), AI architecture (Architect), AI adoption (Staff/EM/Principal), AI PM, AI testing, AI-era DevRel, agentic systems (Applied AI).
3. **Security currency:** OWASP and vulnerability classes, cloud and Kubernetes security, mapping to industry standards.

**Phase 2: Depth and restructuring**

4. SRE: toil, troubleshooting, PRR and engagement models, overload; expand or split Platform Engineering.
5. Data/ML: deepen modules 03–04, add ML-engineering foundations, consider a split.
6. Quality: fix navigation, add specialist framing, AI testing, shift-right, and career variants.
7. Project/Program Manager: an adaptive and hybrid delivery module, team leadership, and technical context.

**Phase 3: Practice and credibility (map-wide)**

8. Scenario exercises for template-first paths; checklists and templates for exercise-first paths; one worked case study per path.
9. A Sources section in every module overview.
10. Career mechanics per path: first 90 days, interviewing, career-beyond, certifications where relevant.

**Phase 4: Structure**

11. New role paths and domain tracks, starting with **Engineering Director / VP / CTO** (see [[#Handoff for Agents]]).
12. Topic Progression Index, a symmetric `entry_from`/`next_paths` graph, and an updated root mermaid diagram.

## 💡 Standard Role-Overview Template

This template is assembled from the best patterns already in the vault:

1. Positioning and **What This Path Is**, with a **role-boundary table** (from Security and Architect)
2. **Level N vs Level N+1** table (from Security)
3. Capability Areas, **linking every module**
4. Typical Progression and Signals for Moving Forward
5. **Self-assessment checklist with linked evidence** (from Security)
6. **Evidence table: artifact → what it demonstrates** (from Security)
7. **First 90 Days** (from Staff and Tech Lead)
8. **Getting Hired**: interview formats, portfolio, certifications (new)
9. **Career Paths Beyond** (from FDE)
10. **Sources**, per module (from Product Manager and Applied AI)
11. Related

## Handoff for Agents

> For agents extending this map. Before changing a path, read this section, that path's own `audit.md`, and the Standard Role-Overview Template above. Path-specific additions are listed in each path audit; this section covers **new paths** and the **rules every change should follow**.

### Ground rules

1. **Never move or rename existing folders from the filesystem.** Notes link by full vault path: 51 notes link into `07_SRE_and_Platform_Engineer/` and 44 into `09_Data_and_ML_Engineer/`, including notes outside `career-path`. Create new folders and cross-link. If a move is unavoidable, do it inside Obsidian so links are rewritten.
2. **Numbering.** New role paths continue at `20_`. Domain tracks go under `30_Domain_Tracks/`.
3. **Structure.** Role overview → modules (`01_Name/00_overview.md`) → numbered topics (`01_Name.md`). Build role overviews from the Standard Role-Overview Template. Module overviews must link every topic with `[[wikilinks]]`, never with backtick filenames.
4. **Frontmatter.** Copy the Staff path's schema: `title, role, capability_area, topic, status, created, updated, tags` for topics (module overviews omit `topic`). Role overviews keep `note_type, career_family, level, entry_from, next_paths, source_frameworks, tags` and must add `created`. Dates use `YYYY-MM-DD`.
5. **Links.** Use path-qualified targets with underscores and no spaces, e.g. `[[career-path/20_Engineering_Director_VP_and_CTO/00_overview|Engineering Director, VP, and CTO]]`. Inside table cells, escape the alias pipe as `\|`.
6. **Register every new path.** Add it to [[career-path/00_Career_Path_Overview|the root overview]] (Career Families table and mermaid diagram), set its `entry_from` and `next_paths`, and add the reverse entry in each neighbor's overview so the graph stays symmetric.
7. **Every topic note includes** a level-delta section ("what changes at this level"), a fill-in template or checklist, a scenario exercise, a Related section, and sources. These are the map's weakest areas today (Systemic Gaps 3 and 4).
8. **Verify before finishing.** Every wikilink resolves, every topic is linked from its module overview, and every module is linked from its role overview.
9. **Order of work.** Do the Phase 0 hygiene sprint first (it doesn't conflict with new paths), then follow the build order at the end of this section.

### Candidate role paths

| Order | Folder | Family | Why it's missing | Build from |
|---|---|---|---|---|
| 1 | `20_Engineering_Director_VP_and_CTO` | People leadership | Management stops at EM; the root diagram already shows "Engineering Director and beyond" with no folder | EM path; Principal `04_.../01_Executive_Communication`; PMBOK finance domain |
| 2 | `21_Platform_Engineer` | Specialist engineering | Platform is half of path 07's title but only 5 of its 30 topics; different customer (internal developers) and a product mindset | Path 07 `06_Developer_Platform`; `checklist/infra-checklist` |
| 3 | `22_ML_Engineer` | Specialist engineering | ML is 6 of path 09's 42 topics; a distinct career between Data Engineer and Applied AI | Path 09 `06_ML_Lifecycle_and_MLOps`; `computing-foundation-note/Artificial_Intelligence`; `checklist/ai-checklist` |
| 4 | `23_Solutions_and_Sales_Engineer` | Enterprise and customer-facing | A large pre-sales job family; "pre-sales" appears in only 2 notes; Independent's sales module sells your own services and FDE is post-sale | FDE modules 01 and 06; DevRel module 03; SA/EA module 02 |

**Suggested modules and graph wiring**

- **20 Engineering Director / VP / CTO:** `01_Managing_Managers` · `02_Org_Design_and_Team_Topologies` · `03_Engineering_Strategy_and_Annual_Planning` · `04_Budget_Headcount_and_Vendors` · `05_Executive_and_Board_Communication` · `06_Engineering_Health_and_Metrics_at_Scale` · `07_VP_vs_CTO_and_Company_Stage`. Entry from EM (and Principal, for the CTO route); next: Independent/Founder (17). Add 20 to EM's `next_paths`.
- **21 Platform Engineer:** `01_Platform_as_Product` · `02_Internal_Developer_Platform_Architecture` · `03_Runtime_and_Kubernetes_Operations` · `04_Golden_Paths_and_Self_Service` · `05_Developer_Experience_and_Productivity` · `06_Platform_Adoption_and_Deprecation` · `07_Platform_Reliability_Security_and_Cost`. Entry from Senior and SRE; next: Staff, Software Architect, EM. Keep the 07 folder name, retitle its text as SRE-focused, and cross-link its module 06 here.
- **22 ML Engineer:** `01_ML_Foundations_for_Engineers` · `02_Experimentation_and_Evaluation` · `03_Training_at_Scale` · `04_Serving_and_Inference_Optimization` · `05_MLOps_and_Model_Lifecycle` · `06_AI_Infrastructure_and_GPUs` · `07_ML_Career_Variants`. Entry from Senior and path 09; next: Staff, Applied AI, Software Architect. Keep path 09 focused on data engineering and cross-link.
- **23 Solutions & Sales Engineer:** `01_The_Pre_Sales_Role_and_Technical_Win` · `02_Technical_Discovery` · `03_Demos_and_Proofs_of_Concept` · `04_RFPs_and_Security_Questionnaires` · `05_Competitive_Positioning` · `06_Partnering_with_Account_Executives` · `07_Handoff_to_Delivery`. Entry from Senior, DevRel/Consultant, and FDE; next: Solutions & Enterprise Architect, Product Manager, Independent.

### Candidate domain tracks (`30_Domain_Tracks/`)

Domains describe *where* an engineer applies their skills, not a level they grow into. Keeping them out of the role list avoids every combination ("Senior Mobile", "Staff Frontend") becoming its own path. Each track is an overview plus 4–6 modules, linked from the Software Engineer, Senior, and Staff overviews.

| Order | Track | Why it's missing | Build from |
|---|---|---|---|
| 1 | `01_Frontend_and_UI` | No path, module, or topic covers client-side work | `checklist/web-checklist`; `computing-foundation-note/HCI Simplify`; `UX UI Essential Documents.md`; QA's accessibility note |
| 2 | `02_Mobile` | Only QA's Mobile Testing note exists | `checklist/mobile-checklist`; QA `05_.../06_Mobile_Testing` |
| 3 | `03_Embedded_and_Safety_Critical` | Nothing today, yet the vault is unusually ready for it | SEBoK (`body-of-knowledge/System Engineer BOK`); `Profile-Large-Safety-Critical.md`; Computer Organization and Operating Systems notes |
| Later | Systems software, games and graphics, robotics | Niche; add only if wanted | Programming Language Theory, Operating Systems, and Database notes |

**Suggested modules**

- **Frontend & UI:** rendering and web performance · accessibility (WCAG 2.2) · design systems · state and data-fetching architecture · frontend testing and observability · frontend at Senior/Staff scope
- **Mobile:** platform lifecycle and constraints · offline and sync · store release and staged rollout · performance, battery, and device fragmentation · mobile security and privacy · mobile at Senior/Staff scope
- **Embedded & Safety-Critical:** real-time and resource constraints · hardware/software interfaces · firmware and OTA updates · safety standards (ISO 26262, IEC 62304, DO-178C) · verification for safety-critical systems · embedded at Senior/Staff scope

### Variants: add inside existing paths, not as new folders

Add each as a short career-variants note or a section in the target path's overview.

| Variant | Add to |
|---|---|
| Privacy Engineer | 08 Security Engineer |
| Analytics Engineer | 09 Data and ML Engineer (data side) |
| AI Research Engineer, AI Infrastructure Engineer | 22 ML Engineer (once created) |
| Developer Productivity / DevEx Engineer | 21 Platform Engineer (once created) |
| Product Engineer, Growth Engineer | 02 Senior Software Engineer, 14 Product Manager |
| Technical Writer | 16 Developer Advocate and Technical Consultant |
| Support / Customer Reliability Engineer | 19 Forward Deployed Engineer |
| Engineering Chief of Staff | 12 Technical Program Manager |
| Fractional CTO | 17 Independent Consulting and Technical Founder |

### Not recommended

- **Backend / full-stack:** already the default Software Engineer path.
- **DevOps Engineer:** a practice that SRE and Platform cover.
- **Cloud Engineer:** fix the cloud gap in Solutions & Enterprise Architect and Platform instead.
- **Prompt Engineer:** now part of Applied AI.
- **Blockchain / Web3:** too volatile for a durable map.

### Build order

Engineering Director / VP / CTO → Platform Engineer → ML Engineer → Frontend & UI track → Solutions & Sales Engineer → Mobile track → Embedded & Safety-Critical track.

## How This Audit Was Done

- **Automated scan of all 965 notes:** wikilink resolution (6,449 links), inbound-link and orphan analysis, frontmatter keys, section patterns, counts of exercises/templates/checklists, 8-word near-duplicate detection, the `entry_from`/`next_paths` graph, and keyword probes for each suspected gap.
- **Reading:** every role overview and module overview (purpose, topic list, structure), plus full reads of sampled topic notes in each path.
- **Limits:** topic notes were sampled rather than all 821 read end to end. Scores are editorial judgments calibrated across paths. Keyword counts show *presence*, not quality.

## Related

- [[career-path/00_Career_Path_Overview|Career Path Overview]]
