---
title: "Pattern Selection and Fallback Design"
note_type: capability-topic
capability_area: llm-application-patterns
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - pattern-selection
  - graceful-degradation
---

# Pattern Selection and Fallback Design

> The discipline of choosing the cheapest pattern that meets the measured quality bar, and designing failure paths so that when the chosen pattern degrades, the user still gets a correct — or at least honest — answer.

## Why This Is a Senior Skill

A mid-level engineer picks patterns by excitement: the demo wants an agent, so the system gets an agent. A senior engineer selects by measured need and designs the failure story up front: what happens when retrieval comes back empty, when the schema fails three times, when the loop exhausts its budget, when the provider is down. Senior pattern selection is a written, evidence-backed decision — and senior fallback design is graceful degradation practiced under fault injection, not discovered by users.

The senior challenge: the right pattern is a moving target (corpus grows, models change, budgets shift), and every pattern has failure modes that must degrade into something safer — never into silence, and never into fabrication.

## Core Frameworks

### Selection Criteria

| Criterion | Question It Answers | Pattern Implication |
|-----------|--------------------|--------------------|
| Knowledge size | Does all needed knowledge fit the context window? | Injection vs RAG |
| Freshness | Is the data static or per-request live? | Injection/RAG vs tool use |
| Step complexity | Is the path to the answer known in advance? | Pipeline vs loop |
| Latency budget | What is the user-facing SLA? | Fewer calls, smaller contexts |
| Cost budget | What does a query cost now, at 10x traffic? | Cheapest sufficient pattern |
| Predictability | How often may the answer vary in form? | Structured outputs, pipelines |
| Risk | What breaks if the answer is wrong? | Confirmation gates, fail-closed |

### The Complexity Ladder

| Level | Pattern | Capability Gained | New Failure Modes | Relative Cost |
|-------|---------|-------------------|-------------------|---------------|
| 0 | Deterministic code | None needed | Ordinary bugs | Baseline |
| 1 | Context injection (single call) | Language understanding, small static knowledge | Hallucination, injection | ~1x |
| 2 | Plus RAG | Large or updating corpora | Retrieval misses, stale index | ~2–4x |
| 3 | Plus tool use | Live data, controlled actions | Bad calls, side effects | ~3–6x |
| 4 | Agent loop | Unknown paths, multi-step work | Runaway loops, cost spikes | ~5–20x |

Climb only when the level below fails the quality bar in evaluation — and descend when a level above proves unnecessary.

### Fallback Design per Failure Class

| Failure Class | Detection | Fallback Strategy |
|---------------|-----------|-------------------|
| Provider outage / rate limit | 5xx, 429 | Retry with backoff → failover model/provider → cached or canned answer |
| Retrieval empty or irrelevant | Low similarity scores | Re-query with expanded terms → "not found in our knowledge" → human handoff |
| Schema parse/validation failure | Validator rejects after retries | Downgrade schema → deterministic path → fail honest |
| Loop budget exhausted | Iteration/token/time breach | Best partial result plus status flag → escalation |
| Unsafe or uncertain output | Guardrail flags, low confidence | Refuse honestly → handoff to human |
| Tool failure mid-task | Error result from execution | Re-plan without the tool → structured apology plus next steps |

### Degradation Design Rules

| Rule | Meaning |
|------|---------|
| Downgrade capability before correctness | Lose citations, formatting, or speed before guessing facts |
| Fail honest, not silent | "I don't know / I can't do that" beats a confident wrong answer |
| Fail closed on irreversible actions | No confirmation gate → no write, ever |
| Make fallbacks observable | Every fallback path emits a metric so degradation is visible |
| Test fallbacks like features | Fault injection in CI/staging, not manual hope |

## In Practice

**Choose the simplest pattern that meets the quality bar — measured, not assumed.** The burden of proof runs upward: before adding RAG to a small corpus, tools to deterministic data, or a loop to a known path, show the simpler pattern failing an evaluation. The cheapest pattern that passes is the right one, and "simplest" is a property of the system, not a statement about the team.

**Design fallbacks per failure class, not one catch-all.** "If anything fails, show an error" is not a design. Each class — provider down, retrieval empty, schema broken, budget burned — needs its own detection signal and its own degradation path, because the user-facing outcome should differ: a cached answer for an outage, an honest refusal for missing knowledge, a partial result for a spent budget.

**Fail honest: when the system is uncertain, it says so or escalates.** The worst failure mode of an AI feature is confident fabrication. Degradation must converge on honest refusal or human handoff, and the refusal must be a designed, on-brand response — not a panic path. Honesty under degradation is what separates a dependable AI feature from a demo.

**Test the degradation paths with fault injection.** Kill the vector store, break the schema validator, cut the tool, exhaust the loop budget — in staging, on a schedule. If the behavior in those scenarios is discovered in production, the fallback design was fiction. Fault injection for AI features is the same discipline as chaos engineering for services.

**Downgrade capability before correctness.** When the rich path fails, the degraded path keeps the promises that matter: a RAG answer that loses its citations is acceptable; one that invents sources is not. Order the fallback ladder by which properties are sacred — correctness, honesty, no unconfirmed writes — and let everything else fall away first.

**Record the selection decision with evidence, and revisit it on a schedule.** An ADR-style note — chosen pattern, alternatives considered, eval numbers, cost model, revisit triggers (corpus size, traffic, model change) — turns pattern choice from a vibe into an engineering decision the next engineer can audit. Without the rationale, every new hire re-litigates the architecture from scratch.

## Practical Exercise

1. Take a feature you own (or a proposed one) and write the pattern-selection decision: criteria, candidates, eval evidence, chosen level on the complexity ladder.
2. Enumerate every failure class for the chosen pattern and map each to detection signal plus fallback behavior.
3. Implement the top three fallbacks (e.g., empty-retrieval answer, schema downgrade, provider failover).
4. Build fault injection: simulate each failure in staging and verify the user-facing behavior matches the design.
5. Add metrics for every fallback path that fires, so degradation is visible in dashboards.
6. Document the revisit triggers (corpus size, traffic, cost thresholds) and the decision record in the team's log.
7. Review whether any fallback could be replaced by climbing down the ladder permanently (e.g., a loop that always times out becomes a pipeline).

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/10_LLM_Production_Patterns]]: the four-pattern decision framework this extends
- [[computing-foundation-note/Artificial_Intelligence/12_AI_ROI_and_Roadmap]]: cost framing for pattern economics
- [[03_Agent_Loops_and_Orchestration]]: the top rung of the ladder, where the costs concentrate
- [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]]: the evidence that pattern decisions are made from
- [[career-path/18_Applied_AI_Engineer/05_Model_and_Inference_Operations/00_overview|Model and Inference Operations]]: provider failover and caching live there

## Common Pitfalls

- Pattern choice by hype: agents for tasks that pipelines handle better
- No fallback design: the first outage reveals the failure story
- Silent failures: empty answers and dropped requests instead of honest refusal
- One generic error path for all failure classes
- Selection rationale never written down: the next engineer re-litigates the decision without the evidence

## Key Takeaways

- Selection is evidence-driven and revisitable: the complexity ladder is climbed only on measured need
- Fallbacks are designed per failure class, with detection signals and metrics
- Honest failure is a feature: refusal and handoff beat fabrication every time
- Fault injection turns fallback fiction into verified behavior
- The sacred properties — correctness, honesty, no unconfirmed writes — survive every degradation step
