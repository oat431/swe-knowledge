---
title: "Cost Optimization"
note_type: capability-topic
capability_area: model-and-inference-operations
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - cost-optimization
  - caching
---

# Cost Optimization

> Reducing the cost per successful task — not just per token — through caching, batching, prompt compression, small-model cascades, and routing, while holding quality and latency inside agreed budgets.

## Why This Is a Senior Skill

Mid-level engineers react to cost: the bill arrives, and they swap the model for a cheaper one or add a cache somewhere. Senior engineers design cost into the architecture: they optimize cost per completed task rather than per token, they know where each optimization lives on the quality–cost–latency triangle, and they track cost per feature so spending follows value ([[computing-foundation-note/Artificial_Intelligence/12_AI_ROI_and_Roadmap]]). The senior mindset: a token never generated is always cheaper than a token generated cheaply.

The failure mode is optimizing an input metric instead of the product metric: per-token cost falls while cost per task rises because retries, longer contexts, and re-runs ate the savings. Cost work that does not move cost-per-task is theater.

## Core Frameworks

### The Cost-Lever Table

| Lever | How It Works | Saves | Costs You |
|---|---|---|---|
| Semantic caching | Store responses keyed by meaning; serve repeats without a model call | Up to 100% on cache hits | Cache infrastructure, staleness risk |
| Prompt caching | Reuse the shared prefix (system prompt, context) across calls | 50–90% on input tokens | Only helps where the prefix is stable |
| Batching | Coalesce concurrent requests into one inference batch | Higher GPU throughput when self-hosting | Added latency while a batch fills |
| Prompt compression | Summarize or prune context before the model sees it | Input tokens shrink proportionally | Risk of losing task-relevant detail |
| Small-model cascades | Cheap model handles easy cases; escalate only hard ones | Majority of traffic on the cheap model | Two models to run and evaluate |
| Model routing | Send each request to the cheapest model that meets its quality bar | Volume-weighted savings | Routing errors cost quality |
| Shorter context | Constrain retrieved context and history aggressively | Linear input-token savings | Quality loss if the context was truly needed |
| Output discipline | Request only needed fields, small output schemas | Output-token savings | Over-constrained schemas degrade quality |

### Quality–Cost–Latency Trade-offs

| Optimization | Quality Risk | Latency Impact | Best Fit |
|---|---|---|---|
| Semantic caching | Low if eviction and validation are sound | Reduces (cache is faster than model) | Repeat and near-repeat queries |
| Small-model cascade | Depends on classifier quality | Often improves (small models are fast) | High-volume, mixed-difficulty traffic |
| Prompt compression | Medium — can drop needed facts | Improves (less to process) | Long-context tasks with padding |
| Batching (self-hosted) | None | Adds queueing delay | Bursty, latency-tolerant traffic |
| Aggressive context truncation | High if done blindly | Improves | Where precise retrieval already exists |

### The Measurement Discipline

| Metric | Why It Matters |
|---|---|
| Cost per task | The product metric; per-token cost hides task-length drift |
| Cache hit rate by cohort | Tells you which traffic actually repeats |
| Cost share by task class | Shows where the money goes before you optimize |
| Savings vs quality delta | Every optimization must be paired with an eval re-run |

## In Practice

**Optimize cost per successful task, not cost per token.** A cheaper model that needs two retries and longer context to succeed can cost more per completed unit of work than an expensive one that gets it right the first time. Report cost divided by completed, quality-passing tasks — [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]] supplies the quality side — and optimize that number.

**Measure before you optimize.** Instrument cost share by task class, cache hit rates, and token-length distributions first. Teams routinely build elaborate caching layers for traffic that never repeats, or cascade classifiers for tasks that are uniformly hard. The data tells you which lever has headroom before you spend engineering on it.

**Use small-model cascades as the default cost architecture for mixed-difficulty traffic.** Route each request to a cheap model; escalate to a stronger one only on low confidence, failed validation, or detected difficulty. The classifier or confidence threshold is itself an engineering artifact that needs evaluation — a cascade with a sloppy gate degrades quality while pretending to save money.

**Treat caching as a system with correctness semantics, not a database lookup.** A cache keyed on raw text misses near-duplicates; one keyed on embeddings risks serving stale or semantically wrong answers. Define eviction, invalidation, and staleness policy per use case, and spot-check served responses for correctness. A wrong cached answer is a cost optimization that converted into a quality incident.

**Run every cost lever as an experiment with an eval attached.** Before enabling a compression or cascade in production, run the candidate path through the offline eval suite and measure the quality delta alongside the projected savings. Ship only if the quality loss stays inside the agreed budget — the same discipline as a model swap in [[01_Model_Selection_and_Benchmarks]].

**Make the cost budget visible at the product decision level, not just to the infra team.** When cost per task is a dashboard metric reviewed alongside quality and latency, product teams start choosing "deterministic lookup instead of LLM" for tasks that never needed a model ([[computing-foundation-note/Artificial_Intelligence/10_LLM_Production_Patterns]]). The cheapest inference is the one that was never made.

## Practical Exercise

Run a structured cost-reduction pass on one AI feature:

1. Instrument: per-task-class token counts, cost, and quality pass-rate over a representative week.
2. Identify the top 3 cost drivers by task class, and check each against the cost-lever table.
3. Choose one lever (e.g., semantic caching) and define its success metric: a savings target plus an acceptable quality delta.
4. Implement it behind a feature flag; run the eval suite against the flagged path.
5. Ship to a cohort and measure actual savings and quality for a week against the pre-change baseline.
6. Decide keep/revert/iterate based on measured numbers, not estimates.
7. Document the lever's realized savings rate for reuse on the next feature.

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/10_LLM_Production_Patterns]]: patterns define where tokens are spent
- [[computing-foundation-note/Artificial_Intelligence/12_AI_ROI_and_Roadmap]]: cost-per-task targets and ROI framing
- [[career-path/18_Applied_AI_Engineer/00_overview|Applied AI Engineer]]: path positioning
- [[01_Model_Selection_and_Benchmarks]]: model choice sets the cost floor
- [[04_Latency_Engineering]]: every cost lever has a latency counterpart
- [[06_Provider_Management_and_Model_Routing]]: routing executes cost policy per request
