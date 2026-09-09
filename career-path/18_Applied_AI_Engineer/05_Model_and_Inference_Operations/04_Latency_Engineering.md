---
title: "Latency Engineering"
note_type: capability-topic
capability_area: model-and-inference-operations
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - latency
  - ttft
---

# Latency Engineering

> Engineering end-to-end response time for model-backed features — time to first token, streaming, parallelism, timeouts, and fallbacks — so the product feels fast under real traffic and degrades gracefully instead of failing.

## Why This Is a Senior Skill

Mid-level engineers set a timeout and hope, then tune p50s after users complain. Senior engineers design latency: they budget every stage of the request path, they understand that perceived latency (TTFT) and total latency are different products, they use streaming and parallel fan-out to restructure work, and they pre-define the degraded mode — the slower-but-alive answer — for when the fast path fails. Latency is a UX and revenue feature they own from design, not a number they observe.

The failure mode is optimizing the median while the tail strangles the product: the p95 experience is what users remember, and it is driven by queueing and retries that the happy path never sees.

## Core Frameworks

### The Latency Budget

| Stage | Typical Share | Levers |
|---|---|---|
| Retrieval / preprocessing | 10–30% | Parallel retrieval, index optimization, precomputation |
| Queueing / batching wait | 0–50%+ under load | Autoscaling, admission control, skip batching when idle |
| Time to first token (TTFT) | Dominates perceived latency | Smaller model, shorter prompt, prefill optimization |
| Generation (tokens/s) | Dominates total latency | Streaming, output constraints, shorter outputs |
| Post-processing / validation | 5–15% | Parallelize, cache, simplify schemas |

### The Technique Table

| Technique | How It Works | Effect |
|---|---|---|
| Streaming | Emit tokens as they generate | Perceived latency drops to roughly TTFT |
| Parallel fan-out | Independent calls (multiple retrievals, sub-tasks) run concurrently | Wall-clock of the slowest call, not the sum |
| Timeouts with tiered fallbacks | Strict budget per stage; on breach, retry a cheaper/faster path | Bounds the worst case instead of hanging |
| Hedging | Duplicate a slow call to an alternate provider after a threshold | Cuts tail latency at added cost |
| Caching | Serve repeats without a model call | Latency drops to cache lookup time |
| Precompute | Do retrieval/classification before the user waits | Moves work off the critical path |

### Latency vs Cost vs Quality Trade-offs

| Choice | Latency | Cost | Quality |
|---|---|---|---|
| Faster (smaller) model | Better | Lower | Usually lower |
| Parallel fan-out + merge | Better | Higher | Often higher |
| Hedged duplicate call | Better tails | Higher | Unchanged |
| Shorter output limit | Better | Lower | Lower if cut too hard |
| Timeout → degraded answer | Bounded | Lower | Lower on that request |

### Perceived vs Measured Latency

| Principle | Implication |
|---|---|
| Users notice TTFT, not tokens/s | Optimize first-token above all for chat-like UX |
| Streaming converts waiting into progress | A slow full answer feels fine if tokens flow steadily |
| Skeletons and partial results beat spinners | Show structure immediately; fill details as they arrive |

## In Practice

**Design the timeout and fallback ladder before writing the feature.** For every model call define: the primary timeout, the retry policy (and when retrying is safe — never for non-idempotent actions), and the degraded fallback: a smaller model, a cached answer, a graceful "try again" message. A system that hangs users for thirty seconds and then errors has no latency architecture; it has a wish.

**Stream by default on user-facing generation.** From the user's perspective, a response is fast if the first token arrives in under a second and tokens keep flowing — total duration matters far less. Reserve non-streaming calls for background jobs and internal pipelines where nobody is watching.

**Fan out anything independent.** Retrieval, classification, guardrail checks, and parallel sub-tasks are independent calls that should run concurrently, with a barrier at the merge point. Sequential dependence is usually an artifact of implementation, not of the problem; each serialized call adds its full latency to the user's wait.

**Budget latency per stage and alert on the budget, not on the average.** Set explicit budgets for retrieval, TTFT, and generation; alert when p95 breaches them, because p50 hides the experience of the unluckiest users. Tail latency is where products feel broken, and the tail is where provider queueing and rate-limit retries live.

**Treat provider queueing as your own problem even on managed APIs.** Provider-side queueing under load — rate-limit backoff, 429 retries — is invisible latency you did not budget for. Design admission control and circuit breakers around provider health ([[06_Provider_Management_and_Model_Routing]]) so that when the provider slows, your system fails fast and falls back instead of silently stacking retries.

**Instrument the full request path and own the merge points.** A latency trace that stops at "model call" cannot explain why the feature is slow. Trace from client to response including retrieval, routing decisions, and retries, so every millisecond is attributable — this is where this area hands off to the tracing tooling of [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]].

## Practical Exercise

Engineer the latency of one model-backed endpoint end to end:

1. Trace the current request path and break it into stages with measured p50/p95 per stage.
2. Write a latency budget: retrieval, TTFT, generation, post-processing, with a target total.
3. Implement the cheapest high-impact fix first — usually streaming and parallelizing retrieval.
4. Define the timeout and fallback ladder: primary, retry policy, degraded answer.
5. Load-test to the designed peak and measure p95 against the budget.
6. Add per-stage alerting on the p95 budget; document the degraded-mode behavior in the runbook.

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/10_LLM_Production_Patterns]]: agent loops and RAG define the latency structure
- [[career-path/18_Applied_AI_Engineer/00_overview|Applied AI Engineer]]: path positioning
- [[03_Cost_Optimization]]: latency levers and cost levers trade against each other
- [[05_Serving_Infrastructure_and_Scaling]]: queueing and batching are where latency is made or lost
- [[06_Provider_Management_and_Model_Routing]]: hedging and failover live in the routing layer
- [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]]: tracing tooling that makes budgets visible
