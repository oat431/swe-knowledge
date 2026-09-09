---
title: "Model Selection and Benchmarks"
note_type: capability-topic
capability_area: model-and-inference-operations
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - model-selection
  - benchmarks
---

# Model Selection and Benchmarks

> Choosing which model class and size to serve each task, based on measured evidence from public benchmarks and — decisively — your own evaluation suite.

## Why This Is a Senior Skill

Mid-level engineers pick the top of the current leaderboard and call it done. Senior engineers treat model selection as an evidence-based, reversible, continuously re-evaluated decision: they know what public benchmarks do and do not measure, they benchmark candidates against their own workload before switching, and they design the system so that swapping models is configuration, not a migration.

The senior question is never "which model is best?" — it is "best for what task, at what cost and latency budget, measured how?"

The failure mode on both sides is identical: choosing by leaderboard without your own measurements, or ignoring evidence entirely — both end with the wrong model in production. Selection is a measurement problem before it is an opinion problem.

## Core Frameworks

### Model Classes and When to Use Them

| Class | Typical Use | When to Avoid |
|---|---|---|
| Frontier general-purpose | Complex reasoning, writing, novel tasks | Simple classification/extraction; cost-sensitive volume traffic |
| Reasoning models (long chain-of-thought) | Math, multi-step logic, hard code problems | Latency-critical paths, simple lookup tasks |
| Small / medium models | High-volume tasks: classification, extraction, routing, summarization | Tasks genuinely needing frontier capability |
| Specialist models | Embeddings, code completion, structured-output-first models | Tasks outside the specialization |

### Size vs Capability Trade-off

| Dimension | Large Model | Small Model |
|---|---|---|
| Quality on hard tasks | Higher | Lower |
| Latency per token | Slower | Faster |
| Cost per token | Higher | Lower |
| Context fidelity on long inputs | Often better | Degrades with length |
| Best fit | Complex, low-volume tasks | Simple, high-volume tasks |

### What Public Benchmarks Do and Do Not Tell You

| Benchmark Type | What It Measures | What It Does Not Measure |
|---|---|---|
| Public leaderboards (knowledge, coding, math) | Broad capability ordering of models | Your task, your data, your prompt patterns |
| Reasoning/agentic evals | Multi-step tool-use ability | Production cost and latency of that behavior |
| Safety/alignment suites | Refusal rates, bias | How refusals affect your user experience |

| Risk | Description | Mitigation |
|---|---|---|
| Contamination and saturation | Public test data leaks into training; leaderboard gains stop meaning real gains | Treat small leaderboard deltas as noise |
| Prompt sensitivity | The same model scores differently under different prompting styles | Bench with your production prompts, not the benchmark's |

### The Selection Framework

| Step | Question | Output |
|---|---|---|
| 1. Characterize the task | How hard, how frequent, how latency-sensitive? | Task classes, not one "AI feature" |
| 2. Filter candidates | Which model classes are plausibly sufficient? | Shortlist including one cheaper and one stronger candidate |
| 3. Benchmark on your workload | Run candidates through your eval suite with production prompts | Head-to-head quality numbers |
| 4. Check budget constraints | Do candidates meet cost-per-task and latency budgets? | Feasible set |
| 5. Deploy behind an abstraction | Is the model ID configuration, not code? | Reversibility |

## In Practice

**Start from the simplest model that plausibly passes your eval, not from the frontier.** The frontier model is the fallback, not the default. Put the small model in production behind your eval suite; escalate traffic — or upgrade the model — only when measured quality on your data says you must. This inverts the common pattern where the expensive model is the baseline and savings are an afterthought.

**Treat public benchmarks as a screening signal, never as the decision.** Leaderboards tell you which models are worth trying, not which to deploy. Scores are prompt-sensitive, occasionally contaminated, and measured on tasks that overlap with your workload only by accident. A model that is three points behind on a leaderboard but five points ahead on your private eval is the right model.

**Pin the exact model version, not the brand.** A brand name is not a model; a dated snapshot with a specific context window, pricing, and behavior is. Providers change defaults, deprecate versions, and silently retune behavior over time. Record the exact model ID in configuration alongside the eval score that justified it, so you can detect drift when a provider changes what that ID serves.

**Own a private eval suite before you allow model swaps.** Without one, every model change is a leap of faith. The suite — built per [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]] — is the gate: no model enters production without passing it, and no model leaves without a successor that does.

**Make selection reversible by keeping the model ID in configuration.** A model swap that requires a code change is a migration and will not happen; one that requires a config change is an experiment and will. This discipline also enables per-task model choice — classification on the small model, reasoning on the large one — instead of one model for everything.

**Re-run selection on a cadence, not on a crisis.** Model economics and capabilities move fast: a model that was cost-prohibitive last quarter may be the sensible default today. Schedule re-benchmarking tied to the cost review rhythm of [[computing-foundation-note/Artificial_Intelligence/12_AI_ROI_and_Roadmap]], rather than waiting for a problem to force the decision.

## Practical Exercise

Run a model-selection cycle for one real task in your application:

1. Characterize one task: define its difficulty, request volume, and cost/latency budgets in numbers.
2. Shortlist 3 candidates across at least two size classes (e.g., one frontier, one mid-size, one small).
3. Run each candidate through your existing eval suite — or a 20–50 case golden set if you have none — with production prompts.
4. Record quality, cost per 1k requests, and p50/p95 latency for each in a comparison table.
5. Select the cheapest model that meets your quality threshold; note which leaderboard ranking would have misled you.
6. Deploy the choice behind a config-driven model ID and document the eval results that justify it.
7. Set a reminder to re-run the cycle in 90 days.

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/10_LLM_Production_Patterns]]: the patterns whose difficulty drives model choice
- [[career-path/18_Applied_AI_Engineer/00_overview|Applied AI Engineer]]: path positioning
- [[02_API_vs_Self_Hosted_Tradeoffs]]: selection feeds the make-vs-buy decision
- [[03_Cost_Optimization]]: selection sets the cost baseline
- [[06_Provider_Management_and_Model_Routing]]: selection gates live in the promotion workflow
- [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]]: the eval suite that makes selection evidence-based
