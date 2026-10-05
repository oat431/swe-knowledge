---
title: "Audit: Applied AI Engineer"
note_type: audit
career_path: applied-ai-engineer
created: 2026-10-06
tags:
  - career-path
  - audit
  - applied-ai
---

# Audit: Applied AI Engineer

> **Verdict:** A well-designed path that is honest about its field. It says there is no mature body of knowledge, deliberately teaches *principles* rather than churning tools, and anchors on the OWASP Top 10 for LLM Applications and *AI Engineering* (Huyen). The core of LLM application engineering is solid: RAG, tool contracts, structured outputs, context assembly, eval suites with regression gates, layered guardrails, inference cost and latency, and governance, with an exercise in every note. To stay current in 2026 it needs **agentic depth**: agent evaluation (0 notes), multi-agent and memory design (2 each), tool protocols such as MCP (3 mentions), and computer-use agents (0). **Multimodal/voice** is also thin.
>
> **Overall: 4 / 5**

## Snapshot

| Metric | Value |
|---|---|
| Notes | 43 (6 modules × 6 topics + 7 overviews) |
| Words | ~47k (avg ~1,080/note) |
| Wikilinks / navigation | All resolve ✅, all topics reachable ✅ |
| Frontmatter | `created` missing in **43/43** |
| Stale labels | "Capability Areas **(planned)**" and "Detailed area notes (phase 2 folders…)" although all 6 modules exist |
| Exercises | **36/36** ✅ |
| Checklists / templates in topics | 0/36 · 0/36 |
| Sources | Every module overview has a Sources section; most external source links of any path (10) |

## Scorecard

| Dimension | Score | Why |
|---|---|---|
| Coverage | 4/5 | LLM-application core complete; agentic systems, agent evals, and multimodal are thin |
| Depth | 4/5 | Principle-level by design, with clear senior framing (e.g., "retrieval quality, not model quality, is the binding constraint in most RAG failures") |
| Practice | 4/5 | Exercise in every note; no templates or checklists |
| Progression | 3/5 | Strong evidence list and scope boundaries; no levels or SWE → AI engineer transition guide |
| Sources | 3/5 | Module-level sources, the best in the map's engineering paths |
| Vault hygiene | 3/5 | Clean links, but `created` missing everywhere and stale "planned" labels |

## ✅ What's Good

- **Right scope, clearly bounded.** The overview separates Applied AI (consumes models) from Data/ML (trains and serves models) and SRE/Platform (operates them), and every module overview has a "Scope Boundary" section.
- **Evaluation as engineering.** Module 03 goes from eval fundamentals → offline suites → output metrics → tracing → online monitoring → **regression gates**. "Evidence, not demos" is the right core value.
- **Security assumes the model can be tricked.** LLM threat modeling, prompt-injection defense, data leakage, input/output guardrails, model and tool supply chain, and incident response for AI abuse.
- **Operations with economics.** Model selection and benchmarks, API vs self-hosted, cost, latency, serving, and provider routing and fallbacks.
- **Honest product judgment.** "Say 'no AI needed here' when a deterministic solution is better" is a moving-forward signal, and module 01 covers pattern selection *and* fallback design.

## ❌ What's Missing

1. **Agent evaluation (0 notes).** Evaluating multi-step agents differs from evaluating single outputs: trajectory and tool-call correctness, task success over episodes, cost and step budgets, and simulation environments. This is the biggest gap given how much 2026 work is agentic.
2. **Agentic system design depth.** One note covers agent loops, while multi-agent and sub-agent orchestration (2), agent memory and long-running state (2), tool protocols such as MCP (3 mentions), and computer-use/browser agents (0) are thin or missing.
3. **Multimodal and voice (3 mentions).** Document and vision pipelines, speech-to-speech and voice agents, and their latency and eval problems.
4. **AI UX.** Streaming, communicating uncertainty, human-in-the-loop review flows, and recovery UX appear in about 8 notes but are never taught as a topic.
5. **Data flywheels.** Golden datasets, synthetic eval data, annotation workflows, and turning production feedback into eval cases appear in 5 notes in passing.
6. **Career shape.** No note covers Applied AI Engineer vs ML Engineer vs Research Engineer vs FDE, how a software engineer transitions, or what a portfolio should show.
7. **Cross-links to the Security path.** This path has the map's only AI-security module, but the Security Engineer path never mentions AI. Link the two both ways.

## ⚠️ What to Improve

- **Remove the stale labels.** Change "Capability Areas (planned)" → "Capability Areas", and drop "Detailed area notes (phase 2 folders under this path)" from the reading route.
- **Add `created`** to all 43 notes (the path was added 2026-09-09).
- **Add a checklist and a template to each topic** (e.g., an eval-suite spec, guardrail review checklist, model-selection memo). The Staff/TL paths show the pattern.
- **Schedule a currency review.** Given the field's pace, add a `last_reviewed:` field and a quarterly review checklist to the role overview.

## 🎯 Recommended Additions (prioritized)

| Priority | Proposed note / module | What it should contain |
|---|---|---|
| 🎯 High | `07_Agentic_Systems/` | `01_Agent_Architectures_and_Orchestration`, `02_Agent_Memory_and_State`, `03_Tool_Protocols_and_Integration` (e.g., MCP), `04_Agent_Evaluation_and_Trajectories`, `05_Human_in_the_Loop_and_Autonomy_Levels`, `06_Computer_Use_and_Browser_Agents` |
| 🎯 High | `03_Evaluation_and_Observability/07_Eval_Data_and_Feedback_Flywheels.md` | Golden sets, synthetic data, annotation workflows, turning production failures into tests |
| Medium | `01_LLM_Application_Patterns/07_Multimodal_and_Voice_Applications.md` | Document/vision pipelines, voice agents, latency and eval specifics |
| Medium | `02_Context_and_Prompt_Engineering/07_AI_UX_Patterns.md` | Streaming, uncertainty display, review queues, graceful failure UX |
| Medium | `00_AI_Engineering_Career_Map.md` | Applied AI vs ML vs Research vs FDE; SWE transition; portfolio evidence |
| Low | `05_Model_and_Inference_Operations/07_Reasoning_Models_and_Compute_Budgets.md` | When extra reasoning pays off, budgeting thinking/compute, latency trade-offs |

**Suggested sources to add:** OWASP GenAI Security Project (beyond the Top 10), NIST AI RMF 1.0 and its Generative AI Profile, EU AI Act summaries (for module 06), *Designing Machine Learning Systems* (Huyen; boundary with path 09), provider engineering guides (Anthropic, OpenAI, Google) on agents, tool use, and evaluation, and the Model Context Protocol specification.

## Module-by-Module Notes

| Module | Verdict | Gap / suggestion |
|---|---|---|
| 01 LLM Application Patterns | ✅ Strong | Add multimodal/voice; move agents into a dedicated module |
| 02 Context & Prompt Engineering | ✅ Good | Add AI UX patterns |
| 03 Evaluation & Observability | ✅ Excellent | Add eval data flywheels and agent evals |
| 04 AI Security & Guardrails | ✅ Excellent (the map's only AI security) | Cross-link Security Engineer path |
| 05 Model & Inference Operations | ✅ Strong | Add reasoning-model budgeting; prompt caching appears in only 2 notes |
| 06 Responsible AI & Governance | ✅ Strong | Add NIST AI RMF mapping |

## Quick Fixes

- [ ] Remove "(planned)" and "phase 2" wording from the role overview
- [ ] Add `created:` (and `last_reviewed:`) to all 43 notes
- [ ] Cross-link module 04 ↔ Security Engineer path
- [ ] Add a checklist to each topic note

## Related

- [[career-path/18_Applied_AI_Engineer/00_overview|Applied AI Engineer overview]]
- [[career-path/09_Data_and_ML_Engineer/audit|Data and ML Engineer audit]]
- [[career-path/08_Security_Engineer/audit|Security Engineer audit]]
- [[career-path/19_Forward_Deployed_Engineer/audit|Forward Deployed Engineer audit]]
- [[career-path/audit|Career path audit (summary)]]
