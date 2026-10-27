---
title: "Audit: Applied AI Engineer"
note_type: audit
career_path: applied-ai-engineer
created: 2026-10-06
updated: 2026-10-27
tags:
  - career-path
  - audit
  - applied-ai
---

# Audit: Applied AI Engineer

> **Status (2026-10-27):** This audit was taken 2026-10-06. Since then the High/Medium/Low additions below have been **delivered** (see [[#Remediation Log]]): the `07_Agentic_Systems` module — including the missing agent-evaluation coverage — plus eval-data flywheels, multimodal/voice, AI UX, and reasoning-model budgeting. **Still open:** the AI-engineering career-map note, NIST AI RMF mapping in module 06, and the `last_reviewed` currency mechanism. Findings below are annotated with their current status.

> **Verdict (as audited 2026-10-06):** A well-designed path that is honest about its field. It says there is no mature body of knowledge, deliberately teaches *principles* rather than churning tools, and anchors on the OWASP Top 10 for LLM Applications and *AI Engineering* (Huyen). The core of LLM application engineering is solid: RAG, tool contracts, structured outputs, context assembly, eval suites with regression gates, layered guardrails, inference cost and latency, and governance, with an exercise in every note. To stay current in 2026 it needs **agentic depth**: agent evaluation (0 notes), multi-agent and memory design (2 each), tool protocols such as MCP (3 mentions), and computer-use agents (0). **Multimodal/voice** is also thin.
>
> **Overall: 4 / 5** *(unchanged; the agentic gap is now closed — see the scorecard notes)*

## Snapshot (updated 2026-10-27)

| Metric | Value |
|---|---|
| Notes | 54 (7 modules: 4 × 7 topics + 3 × 6 topics + 7 overviews) — was 43 at audit |
| Words | ~62k (was ~47k at audit) |
| Wikilinks / navigation | All resolve ✅, all topics reachable ✅ |
| Frontmatter | `created` present on all notes ✅ (was missing 43/43; fixed in Phase 0) |
| Stale labels | Removed ✅ (was "(planned)" + "phase 2" wording) |
| Exercises | **46/46** ✅ |
| Checklists / templates in topics | 10/46 (all new notes; older topics still lack them) |
| Sources | Module overviews have Sources; the 11 newest topics add topic-level Sources |

## Scorecard

| Dimension | Score | Why |
|---|---|---|
| Coverage | 4/5 | LLM-application core complete **plus agentic systems and agent evals (added 2026-10-27)**; career-map note remains open |
| Depth | 4/5 | Principle-level by design, with clear senior framing (e.g., "retrieval quality, not model quality, is the binding constraint in most RAG failures") |
| Practice | 4/5 | Exercise in every note; the 10 newest notes add checklists/templates (older topics do not) |
| Progression | 3/5 | Strong evidence list and scope boundaries; no levels or SWE to AI engineer transition guide (career-map note still open) |
| Sources | 3/5 | Module-level sources plus new topic-level Sources; NIST AI RMF / EU AI Act mapping still unwritten |
| Vault hygiene | 5/5 | Clean links, `created` present, stale labels removed (was 3/5 at audit) |

## ✅ What's Good

- **Right scope, clearly bounded.** The overview separates Applied AI (consumes models) from Data/ML (trains and serves models) and SRE/Platform (operates them), and every module overview has a "Scope Boundary" section.
- **Evaluation as engineering.** Module 03 goes from eval fundamentals to offline suites to output metrics to tracing to online monitoring to **regression gates**. "Evidence, not demos" is the right core value.
- **Security assumes the model can be tricked.** LLM threat modeling, prompt-injection defense, data leakage, input/output guardrails, model and tool supply chain, and incident response for AI abuse.
- **Operations with economics.** Model selection and benchmarks, API vs self-hosted, cost, latency, serving, and provider routing and fallbacks.
- **Honest product judgment.** "Say 'no AI needed here' when a deterministic solution is better" is a moving-forward signal, and module 01 covers pattern selection *and* fallback design.

## ❌ What's Missing (as audited 2026-10-06; resolved items marked)

1. ✅ **Resolved 2026-10-27 — Agent evaluation.** Delivered as `07_Agentic_Systems/04_Agent_Evaluation_and_Trajectories` (trajectory and tool-call correctness, task success over episodes, cost and step budgets, simulation environments), with follow-through in `07_Agentic_Systems/02_Agent_Memory_and_State` and regression-set practice in `03_Evaluation_and_Observability/07_Eval_Data_and_Feedback_Flywheels`.
2. ✅ **Resolved 2026-10-27 — Agentic system design depth.** New `07_Agentic_Systems/` module: `01_Agent_Architectures_and_Orchestration` (workflows vs agents, orchestrator-worker, multi-agent topologies), `02_Agent_Memory_and_State`, `03_Tool_Protocols_and_Integration` (tool contracts, MCP, least-privilege tooling), `05_Human_in_the_Loop_and_Autonomy_Levels`, `06_Computer_Use_and_Browser_Agents`. The existing `01_.../03_Agent_Loops_and_Orchestration` is deliberately kept as the loop-mechanics note and cross-linked from the module (not moved).
3. ✅ **Resolved 2026-10-27 — Multimodal and voice.** `01_LLM_Application_Patterns/07_Multimodal_and_Voice_Applications` (document/vision pipelines, voice pipelines and turn-taking, per-modality latency and evaluation).
4. ✅ **Resolved 2026-10-27 — AI UX.** `02_Context_and_Prompt_Engineering/07_AI_UX_Patterns` (streaming, uncertainty display, review-queue UX, graceful failure, correction affordances, overtrust).
5. ✅ **Resolved 2026-10-27 — Data flywheels.** `03_Evaluation_and_Observability/07_Eval_Data_and_Feedback_Flywheels` (golden sets, synthetic data, annotation workflows, production-failure-to-eval-case pipeline, eval-set drift).
6. ⏳ **Open — Career shape.** No note covers Applied AI Engineer vs ML Engineer vs Research Engineer vs FDE, how a software engineer transitions, or what a portfolio should show (`00_AI_Engineering_Career_Map.md` — intentionally deferred to a guided, career-coach-authored note).
7. ✅ **Resolved 2026-10-27 — Cross-links to the Security path.** Wired both ways: `04_AI_Security_and_Guardrails/00_overview` links the Security Engineer path (already present), and Security's role overview now links back to this path's guardrails module.

## ⚠️ What to Improve

- ✅ **Stale labels removed** ("Capability Areas (planned)", "phase 2" wording) — done in Phase 0.
- ✅ **`created` added** to all notes — done in Phase 0 (2026-10-21).
- ⏳ **Partially done — checklists and templates per topic.** All 10 new topics carry a "Practical Applications" checklist or template; the 36 pre-existing topics still lack them.
- ⏳ **Open — currency review mechanism.** A `last_reviewed:` field and quarterly review checklist on the role overview remain unwritten; given this field's pace, this is the most important remaining hygiene item.

## 🎯 Recommended Additions (prioritized) — delivery status

| Priority | Proposed note / module | What it should contain | Status |
|---|---|---|---|
| 🎯 High | `07_Agentic_Systems/` | `01_Agent_Architectures_and_Orchestration`, `02_Agent_Memory_and_State`, `03_Tool_Protocols_and_Integration` (e.g., MCP), `04_Agent_Evaluation_and_Trajectories`, `05_Human_in_the_Loop_and_Autonomy_Levels`, `06_Computer_Use_and_Browser_Agents` | ✅ Done 2026-10-27 |
| 🎯 High | `03_Evaluation_and_Observability/07_Eval_Data_and_Feedback_Flywheels.md` | Golden sets, synthetic data, annotation workflows, turning production failures into tests | ✅ Done 2026-10-27 |
| Medium | `01_LLM_Application_Patterns/07_Multimodal_and_Voice_Applications.md` | Document/vision pipelines, voice agents, latency and eval specifics | ✅ Done 2026-10-27 |
| Medium | `02_Context_and_Prompt_Engineering/07_AI_UX_Patterns.md` | Streaming, uncertainty display, review queues, graceful failure UX | ✅ Done 2026-10-27 |
| Medium | `00_AI_Engineering_Career_Map.md` | Applied AI vs ML vs Research vs FDE; SWE transition; portfolio evidence | ⏳ Open |
| Low | `05_Model_and_Inference_Operations/07_Reasoning_Models_and_Compute_Budgets.md` | When extra reasoning pays off, budgeting thinking/compute, latency trade-offs | ✅ Done 2026-10-27 |

**Suggested sources to add:** OWASP Top 10 for LLM Applications, *AI Engineering* / *Designing Machine Learning Systems* (Huyen), and the Model Context Protocol specification are now cited in the new notes. Still open: OWASP GenAI Security Project (beyond the Top 10), NIST AI RMF 1.0 and its Generative AI Profile, EU AI Act summaries (for module 06), provider engineering guides on agents and evaluation.

## Module-by-Module Notes

| Module | Verdict | Gap / suggestion |
|---|---|---|
| 01 LLM Application Patterns | ✅ Strong | Multimodal/voice ✅ added (07). Agent-loops note deliberately kept here as loop mechanics, cross-linked to new module 07 |
| 02 Context & Prompt Engineering | ✅ Good | AI UX ✅ added (07) |
| 03 Evaluation & Observability | ✅ Excellent | Flywheels ✅ added (07); agent evals delivered in `07_Agentic_Systems/04` |
| 04 AI Security & Guardrails | ✅ Excellent (the map's only AI security) | Security cross-link ✅ completed both ways |
| 05 Model & Inference Operations | ✅ Strong | Reasoning-model budgeting ✅ added (07); prompt caching still in only 2 notes |
| 06 Responsible AI & Governance | ✅ Strong | Add NIST AI RMF mapping (open) |
| 07 Agentic Systems | ✅ New 2026-10-27 | Agentic depth the audit called for: architectures, memory, tool protocols, trajectory evals, autonomy, computer use |

## Quick Fixes

- [x] Remove "(planned)" and "phase 2" wording from the role overview (done 2026-10-21)
- [x] Add `created:` to all notes (done 2026-10-21; `last_reviewed:` still open)
- [x] Cross-link module 04 ↔ Security Engineer path (done 2026-10-27)
- [x] Add a checklist to each new topic note (done for the 10 new notes; the 36 older topics still open)

## Remediation Log

**2026-10-27 — Agentic depth and AI-thread additions** (commits `433c759`, `bfd9e61`):
- New module `07_Agentic_Systems/`: 1 contract overview + 6 topic notes (~10.6–11.9 KB each), covering all six topics the audit specified, including agent evaluation (the audit's single biggest gap).
- New topics: `01/07_Multimodal_and_Voice_Applications`, `02/07_AI_UX_Patterns`, `03/07_Eval_Data_and_Feedback_Flywheels`, `05/07_Reasoning_Models_and_Compute_Budgets`.
- Wiring: topic-table rows added in modules 01/02/03/05, `07_Agentic_Systems` registered on the role overview (file counts and totals updated), Security ↔ Applied AI cross-links completed both ways.
- Verification: 10/10 new notes pass the path-family contract (byte-locked definitions, section set, mermaid rules, whitelist-only wikilinks, no invented statistics/URLs); path totals now 54 notes / ~62k words; zero broken links, zero orphan topics.

**2026-10-21 — Phase 0 hygiene** (commits `c14b005`, `40d9bf1`, `71abc47`, `7dbf142`): `created` added to all notes, "(planned)" / "phase 2" stale labels removed.

**Remaining work:** `00_AI_Engineering_Career_Map.md`; NIST AI RMF mapping for module 06; `last_reviewed:` currency mechanism; checklists/templates for the 36 pre-existing topics.

## Related

- [[career-path/18_Applied_AI_Engineer/00_overview|Applied AI Engineer overview]]
- [[career-path/09_Data_and_ML_Engineer/audit|Data and ML Engineer audit]]
- [[career-path/08_Security_Engineer/audit|Security Engineer audit]]
- [[career-path/19_Forward_Deployed_Engineer/audit|Forward Deployed Engineer audit]]
- [[career-path/audit|Career path audit (summary)]]
