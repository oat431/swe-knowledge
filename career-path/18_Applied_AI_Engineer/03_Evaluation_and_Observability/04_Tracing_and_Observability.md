---
title: "Tracing and Observability"
note_type: capability-topic
capability_area: evaluation-and-observability
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - tracing
  - observability
  - spans
  - cost-tracking
  - latency
---

# Tracing and Observability

> Instrumenting every LLM call and the pipeline around it so any production behavior can be reconstructed, attributed to its cause, and costed.

## Why This Is a Senior Skill

A mid-level engineer prints prompts to application logs and greps for them after incidents. A senior engineer designs a span structure that mirrors the pipeline — retrieval, prompt assembly, model, parsing, tool, answer — attaches configuration and version metadata at the right scope, and makes cost and latency attributable per feature, per user query, and per span. When quality degrades, the trace shows *where* the degradation entered the chain, which is usually before the model.

The senior challenge is designing instrumentation that survives refactors: spans must follow the logical pipeline rather than this quarter's code structure, and metadata must attach automatically rather than by discipline.

## Core Frameworks

### What Every Trace Must Capture

| Field | Example | Why |
|-------|---------|-----|
| Prompt template + version | `support_v3.2` | Regressions are usually prompt changes |
| Model + parameters | Model id, temperature, max tokens | Behavior is configuration-dependent |
| Full input/output | User query, final answer | Replay and audit |
| Intermediate steps | Retrieved docs with scores, tool calls, agent steps | Quality failures originate before generation |
| Tokens and cost | Input/output tokens, per-span cost | Cost attribution and anomaly detection |
| Latency | Time-to-first-token, total, per span | Locating the slow leg |
| Lineage | Session, user, feature, span parent/child | Rolling up and slicing |
| Feedback hooks | Thumbs, handoff events | Connecting behavior to quality |

### Observability Layers

| Layer | Typical Metrics | Alert Example |
|-------|-----------------|---------------|
| Application golden signals | Error rate, latency percentiles, throughput | p95 latency breaches the SLA |
| LLM-specific | Tokens per query, cost per query, TTFT, judge-flagged hallucination rate | Cost per query doubles overnight |
| Business | Deflection rate, resolution rate, CSAT | Deflection drops below target |

### Example Tooling (examples, not prescriptions)

| Tool | Strength | Watch Out | Fits |
|------|----------|-----------|------|
| Langfuse | Open source, self-hostable, tracing + eval in one | Hosting and upgrades on you | Teams wanting control and price predictability |
| LangSmith | Deep LangChain integration, mature eval tooling | Commercial cost at scale | LangChain-centric stacks |
| Braintrust | Eval-first design with dataset management | Younger ecosystem | Teams where evals are the center of gravity |
| W&B Weave | Versioned objects and traces in one SDK | Heavier abstraction | Research-to-production workflows |
| OpenLLMetry / Arize Phoenix | OpenTelemetry-native LLM spans | More assembly required | OTel-standardized platforms |

Capabilities matter more than products: all of the above do spans, cost, and evals. The real choice is integration depth, hosting model, and the evaluation workflow each tool forces.

### Span Design Decisions

| Decision | Sound Default | Why |
|----------|---------------|-----|
| Span granularity | One span per logical pipeline step, not per function call | Refactors survive; trace trees stay readable |
| Attribute placement | Static config on the root span, dynamic data on the child span | Config is constant per request; payloads are not |
| Error capture | Span status + exception payload + input snapshot on failure | Failures are the traces most likely to be replayed |
| Retention | Keep failure and flagged traces longest; aggregate the rest | The investigation set is small; the aggregate set is large |

## In Practice

**Trace the whole chain, not just the model call.** RAG failures are usually retrieval failures: the wrong documents enter the prompt and the model faithfully answers a different question. Spans for embedding, retrieval with scores, reranking, and generation let you see the wrong turn as it happens instead of blaming the model.

**Attach config and version to every span, automatically.** Prompt version, model, temperature, feature flags, and code hash must ride on the span without the engineer remembering to add them — inject them at the tracing-client level. A trace without config metadata cannot explain a regression that appeared after a deploy.

**Make cost and latency attributable per feature and per query.** Roll token counts and dollars up the span tree so the team can answer "what does feature X cost per request" from tracing data alone. Attribution turns cost from an opaque monthly bill into a per-feature engineering signal — the measurement side of what [[career-path/18_Applied_AI_Engineer/05_Model_and_Inference_Operations/00_overview|Model and Inference Operations]] optimizes.

**Sample deliberately, not by default.** Tracing 100% of traffic is expensive and unnecessary; tracing 1% uniformly is blind. Capture all failures and all judge-flagged or user-flagged interactions, plus a stratified sample of successes. The worst traces are always in the set you kept.

**Use traces as the raw material for the offline suite.** Production traces contain real queries with real contexts and real outcomes. Export interesting and failing traces into the eval-suite pipeline from [[02_Offline_Evaluation_Suites]] — tracing feeds evaluation, and evaluation decides which changes ship.

**Alert on golden signals; investigate on traces.** Traces are for investigation, not alerting. Alert on aggregates — latency, error rate, cost anomalies — and when an alert fires, the trace of the worst offender is the first thing anyone opens.

## Practical Exercise

Instrument a RAG chain end to end:
1. Add spans for embedding, retrieval (with scores and top-k docs), prompt assembly, generation, and answer parsing
2. Attach metadata automatically at the client level: prompt version, model, temperature, feature flag, code hash
3. Log tokens and computed cost per span, and roll up per query
4. Run 20 varied queries and inspect the resulting trace trees
5. Identify the slowest span and one retrieval miss purely from trace data
6. Export the 5 most interesting traces as candidate cases for the offline suite
7. Write the two aggregate alerts you would configure (latency, cost) with draft thresholds

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/13_LLM_Evaluation_and_Guardrails]]: monitoring stack and what-to-watch source
- [[computing-foundation-note/Artificial_Intelligence/10_LLM_Production_Patterns]]: the pipelines being traced (RAG, agents, tool use)
- [[05_Online_Evaluation_and_Monitoring]]: traces underpin production quality monitoring
- [[02_Offline_Evaluation_Suites]]: traces become test cases
- [[career-path/07_SRE_and_Platform_Engineer/00_overview|SRE and Platform Engineer]]: golden signals and alerting discipline
- [[career-path/09_Data_and_ML_Engineer/04_Data_Quality/04_Data_Observability|Data Observability]]: observability mindset from the data platform world
