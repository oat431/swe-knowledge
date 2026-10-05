---
title: "Audit: SRE and Platform Engineer"
note_type: audit
career_path: sre-and-platform-engineer
created: 2026-10-06
tags:
  - career-path
  - audit
  - sre
  - platform-engineering
---

# Audit: SRE and Platform Engineer

> **Verdict:** A solid, tool-aware *introduction* to SRE: SLIs/SLOs/error budgets, RED/USE/golden signals, incident roles, progressive delivery, GitOps, DR, chaos, and golden paths, with **a practical exercise in every note**. But it is the **thinnest multi-module path in the map** (30 topics, ~1,050 words each), and **the specialist notes are often shorter than the generalist versions of the same topic** in the Senior and Tech Lead paths. Several defining SRE topics are missing (**toil, troubleshooting, production readiness reviews, overload and cascading failure**). **Platform engineering, half the path's title, gets one module out of six.**
>
> **Overall: 3 / 5**

## Snapshot

| Metric | Value |
|---|---|
| Notes | 37 (6 modules × 5 topics + 7 overviews) |
| Words | ~39k (avg ~1,050/note; several notes 745–870 words) |
| Wikilinks | All resolve ✅ |
| Module → topic navigation | ❌ 5 of 6 module overviews list topics as `` `file.md` `` text (25 topics not clickable); `02_Observability/05_Observability_Driven_Development` has **no inbound links at all** |
| Frontmatter | `created` missing in 32/37 |
| Exercises | **30/30** ✅ |
| Checklists / templates | 5/30 · 9/30 |
| Sources | 0/30 topics; Google's SRE books are never cited by name |

## Scorecard

| Dimension | Score | Why |
|---|---|---|
| Coverage | 3/5 | Core pillars are present; toil, troubleshooting, PRR/engagement, overload, runtime platform, security, and cost are missing |
| Depth | 3/5 | Concrete and tool-aware, but short; e.g., Senior's Chaos Engineering note is 1,327 words, SRE's is 1,060 |
| Practice | 4/5 | Exercise in every note and a self-assessment checklist in every module overview |
| Progression | 2/5 | No SRE vs DevOps vs Platform distinction, no engagement models, no levels, no evidence/capstone |
| Sources | 1/5 | No topic cites a source |
| Vault hygiene | 2/5 | Broken module navigation, one unreachable note, most notes lack `created` |

## ✅ What's Good

- **Service objectives done properly.** SLI design → SLO definition (with a template) → error-budget policy tiers and burn rate → SLA management → reliability reporting. This is the right order, and it treats error budgets as an investment tool, not just a gate.
- **Exercise-first.** Every one of the 30 notes ends with a practical exercise, which few other paths manage.
- **Tool-aware without being tool-bound.** For example, the CI/CD note maps each pipeline stage to tools (BuildKit, Bazel, Trivy, ArgoCD) *and* failure actions, and treats "pipelines as products with SLAs".
- **Developer-platform module uses current thinking:** platform as product with a maturity model, self-service, golden paths, DevEx measurement, and service catalogs.
- **Each module overview has a self-assessment checklist and an "Existing Vault Anchors" table** linking into `software-engineering-note/`.

## ❌ What's Missing

1. **Toil.** It's the defining SRE concept (measure it, cap it, automate it away), yet it appears only in 3 passing mentions and has no note.
2. **Systematic troubleshooting.** Hypothesis-driven debugging of production, using the USE method in practice, and narrowing the blast radius get 1 mention. This is a core SRE interview and on-the-job skill.
3. **Production readiness reviews and SRE engagement models (0 notes).** Onboarding a service to SRE support, readiness criteria, embedded vs consulting vs platform SRE teams, and handing the pager back.
4. **Overload and cascading failure.** Load shedding, backpressure, retries with budgets, and graceful degradation are scattered across 9 notes but never taught as a topic.
5. **Platform engineering depth.** Only 5 notes, with no runtime/orchestration platform (Kubernetes operations, multi-tenancy, upgrades), no platform architecture (IDP layers, APIs and abstractions), and no platform product management (roadmaps, adoption, internal marketing, deprecating paved roads, platform SLOs).
6. **Security and cost in the platform.** Supply-chain security (SLSA, signing, SBOM), secrets, and policy-as-code (3 notes in passing); FinOps and cost allocation (3 notes in passing).
7. **Operating AI workloads (1 mention).** GPU capacity, inference SLOs, model-serving reliability, and AIOps are increasingly common SRE/platform responsibilities in 2026.
8. **Career shape.** No note distinguishes SRE vs DevOps vs Platform Engineer, describes SRE levels, or provides an evidence/capstone module.

## ⚠️ What to Improve

- **Fix the depth inversion.** As the specialist path, SRE should be the *deepest* treatment of reliability topics, but it is often the shallowest:

  | Topic | Senior / Tech Lead version | SRE version |
  |---|---|---|
  | Chaos engineering | Senior `05/07`: 1,327w | `05/04`: 1,060w |
  | Observability / metrics | Senior `05/03`: 1,270w | `02/01`: 802w |
  | On-call | Tech Lead `07/05`: 1,589w | `03/01`: 1,184w |
  | Incident command | Tech Lead `07/01`: 1,347w | `03/02`: 1,269w |

  Either deepen the SRE notes (e.g., multi-window burn-rate alerting math, tail-latency SLIs, trace sampling strategies) or explicitly make the Senior/TL notes the "user" level and SRE the "builder" level with cross-links.
- **Reduce near-duplicate text.** Senior's `05_Quality_Reliability_Security/03_Observability` shares 22% of its text with `02_Observability/01_Metrics_and_Dashboards` and 14% with `02/02_Structured_Logging`.
- **Fix navigation.** Convert the "File" column in the topic tables of modules 01, 02, 03, 05, 06 to wikilinks, and link `05_Observability_Driven_Development` from its module overview.
- **Thicken module 04.** `01_CI_CD_Pipelines` through `04_GitOps` use a two-section "Core Concepts / Anti-Patterns" structure that is thinner than the rest.
- **Add `created`** to the 32 notes missing it.

## 🎯 Recommended Additions (prioritized)

| Priority | Proposed note / module | What it should contain |
|---|---|---|
| 🎯 High | `03_Incident_Response/06_Systematic_Troubleshooting.md` | Hypothesis loop, USE/RED in practice, bisecting the stack, and communicating while debugging |
| 🎯 High | `01_Service_Objectives/06_Toil_Measurement_and_Elimination.md` | Toil definition and taxonomy, measurement, the 50% cap, and an automation ROI template |
| 🎯 High | `07_SRE_Engagement_and_Production_Readiness/` | PRR checklist, engagement models (embedded / consulting / platform), service onboarding and pager hand-back, and SRE vs DevOps vs Platform roles |
| 🎯 High | `08_Platform_Engineering_Depth/` | `01_Runtime_Platform_and_Kubernetes_Operations`, `02_Platform_Architecture_and_Abstractions`, `03_Multi_Tenancy`, `04_Platform_Product_Management_and_Adoption`, `05_Platform_SLOs`; or split Platform Engineering into its own path |
| Medium | `05_Capacity_and_Resilience/06_Overload_and_Cascading_Failures.md` | Load shedding, backpressure, retry budgets, deadline propagation, graceful degradation |
| Medium | `04_Delivery_Automation/06_Supply_Chain_and_Secrets_Security.md` | SLSA, signing, SBOM, secrets management, policy-as-code |
| Medium | `05_Capacity_and_Resilience/07_Cost_and_FinOps.md` | Cost allocation, unit costs, rightsizing, and cost SLOs |
| Low | `05_Capacity_and_Resilience/08_Operating_AI_Workloads.md` | GPU capacity, inference latency SLOs, model rollout and rollback |

**Suggested sources to cite:** *Site Reliability Engineering* and *The Site Reliability Workbook* (Google; free online at sre.google), *Implementing Service Level Objectives* (Hidalgo), *Observability Engineering* (Majors, Fong-Jones & Miranda), *Team Topologies* (Skelton & Pais), the CNCF Platforms white paper and Platform Engineering Maturity Model, and *Accelerate* / DORA research.

## Module-by-Module Notes

| Module | Verdict | Gap / suggestion |
|---|---|---|
| 01 Service Objectives | ✅ Strong core | Add toil; deepen burn-rate alerting math; fix navigation |
| 02 Observability | ⚠️ Good but short (755–1,006w) | Add sampling and cardinality management; link the unreachable ODD note |
| 03 Incident Response | ✅ Good | Add systematic troubleshooting; cross-link to Tech Lead module 07 |
| 04 Delivery Automation | ⚠️ Thin structure in 4 of 5 notes | Add supply-chain security |
| 05 Capacity & Resilience | ✅ Good | Add overload/cascading failures, cost |
| 06 Developer Platform | ⚠️ Too small for half of the path's title | Expand into its own module set or path |

## Quick Fixes

- [ ] Convert backtick filenames to wikilinks in modules 01, 02, 03, 05, 06
- [ ] Link `02_Observability/05_Observability_Driven_Development` from its module overview
- [ ] Add `created:` to the 32 notes missing it
- [ ] Add a Sources section to each module overview (start with sre.google)

## Related

- [[career-path/07_SRE_and_Platform_Engineer/00_overview|SRE and Platform Engineer overview]]
- [[career-path/05_Tech_Lead/audit|Tech Lead audit]]
- [[career-path/06_Software_Architect/audit|Software Architect audit]]
- [[career-path/audit|Career path audit (summary)]]
