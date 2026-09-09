---
title: "Online Evaluation and Monitoring"
note_type: capability-topic
capability_area: evaluation-and-observability
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - online-evaluation
  - production-monitoring
  - feedback
---

# Online Evaluation and Monitoring

> Measuring the system against real traffic after deploy — quality, performance, cost, and usage — and converting the signals into alerts, test cases, and product decisions.

## Why This Is a Senior Skill

A mid-level engineer ships and watches the error dashboard. A senior engineer designs the feedback capture itself: explicit signals (ratings, reports) and implicit signals (copy-after-answer, re-ask, handoff, abandonment), samples production outputs for continuous offline scoring, and separates the three jobs online monitoring does — detect degradation, prove launch impact, and replenish the offline suite. They also know which signals to trust: users rarely rate, and the users who do are not representative.

The senior challenge is noise control. Production signals are biased, sparse, and lagging; thresholds set without baselines produce alert fatigue or silence, and both are failure states.

## Core Frameworks

### What to Monitor

| Category | Metrics | Alert Trigger |
|----------|---------|---------------|
| Performance | Handoff rate, resolution rate, user satisfaction | Degradation below threshold |
| Quality | Sampled hallucination rate, sampled relevance | Spike in sampled failures |
| Latency | Time to first token, total response time | SLA breach (e.g., p95 > 2s) |
| Cost | Cost per query, tokens per query | Budget overrun or anomaly |
| Usage | Queries per minute/day, active users | Anomalous spike or drop |
| Safety | Injection attempts blocked, harmful outputs flagged | Spike in attack traffic |

### Feedback Signal Types

| Signal | Example | Strength | Weakness |
|--------|---------|----------|----------|
| Explicit | 👍/👎, ratings, user reports | Direct quality statement | Sparse, selection-biased, gamed |
| Implicit — behavior | Copy-after-answer, re-ask, abandonment, edit-before-accept | Unfiltered, abundant | Needs interpretation; causal claims risky |
| Implicit — process | Human handoff, escalation | Strong failure proxy in support flows | Exists only where handoff is possible |
| Experiment | A/B versus baseline, shadow deploy | Controlled comparison | Requires traffic volume and experiment hygiene |
| Sampled scoring | Periodic offline judging of production samples | Grounded quality number over time | Lagging; biased if not stratified |

### Example Monitoring Stack

| Tool | What It Does |
|------|--------------|
| Langfuse / LangSmith | LLM tracing, cost tracking, evaluation on production data |
| Arize / Galileo | LLM observability, drift detection, guardrail monitoring |
| Evidently AI | Data and quality reports over time |
| WhyLabs / whylogs | Statistical profiling of LLM inputs and outputs |
| Custom logging | Full query + context + response capture for offline analysis |

Boundary note: statistical drift monitoring for *trained* models belongs to [[career-path/09_Data_and_ML_Engineer/06_ML_Lifecycle_and_MLOps/05_ML_Monitoring_and_Drift|ML Monitoring and Drift]]. Here the focus is LLM *applications*: their outputs, their traffic, and their feedback.

### Monitoring Artifacts and Their Jobs

| Artifact | Job | Failure Mode |
|----------|-----|--------------|
| Dashboard | Answer "how are we doing" in aggregate | Becomes wall decoration if nobody rotates ownership |
| Alert | Interrupt a human when action is needed | Fatigue if thresholds are guesses; silence if thresholds are politics |
| Weekly quality report | Force a recurring look at sampled scores and feedback | Written by nobody if it has no audience and no decisions |
| Investigation trace set | Answer "what changed and why" after any alert | Useless if config metadata is missing from spans |

## In Practice

**Sample production outputs continuously and score them offline.** The strongest online quality signal is a running judge score over a stratified sample of real traffic. It closes the loop from [[computing-foundation-note/Artificial_Intelligence/13_LLM_Evaluation_and_Guardrails]]: online evaluation catches what offline missed, and the caught cases join the offline suite.

**Instrument implicit feedback; treat explicit ratings as a weak signal.** A user who copies the answer and ends the session has voted with behavior; a user who clicks 👎 has voted with a button that 1% of users ever touch. Weight behavior over buttons, but validate interpretations before acting — a re-ask can mean "wrong answer" or "want more detail."

**Set thresholds only after a baseline period.** Launch with monitoring in collection mode; after one to four weeks, set thresholds from observed distributions with room for normal variance. Thresholds invented before launch are guesses that produce alert fatigue — the same define-before-you-measure discipline as [[computing-foundation-note/Artificial_Intelligence/12_AI_ROI_and_Roadmap]], applied to operations.

**Watch hallucination signals where they are detectable.** Fact-in-answer-but-not-in-context, price mismatches, "I don't know" when the context holds the answer — these are cheap to detect on sampled traffic and are leading indicators of retrieval or grounding regressions. Confident-but-wrong cases should route to human review, never silently to the user.

**Own business metrics and quality metrics together, and distrust them when they diverge.** Deflection rate and CSAT are business metrics; faithfulness and relevance are quality metrics. If deflection rises while sampled quality falls, something is being gamed — usually the model learned to say "I can't help" or to hand off faster. Divergence between the two families is itself an alert.

**Every alert must end in a fix or a test case.** An alert that resolves to "monitoring noise" twice gets ignored the third time. The runbook for every alert ends with one of two outputs: a code or config change, or a new case in the offline suite. Anything else is documentation of learned helplessness.

## Practical Exercise

Set up online evaluation for a deployed AI feature:
1. Define 8 metrics across the six categories above, with owners and definitions
2. Implement output sampling (1–5%, stratified by feature) with storage for offline scoring
3. Add explicit feedback capture and two implicit signals (e.g., copy-after-answer, re-ask)
4. Run a one-week collection period and compute baseline distributions
5. Set alert thresholds with variance margins; document each alert's runbook ending
6. Score one week of samples with the offline suite and compare against the offline baseline
7. Export the 10 worst sampled interactions into the offline suite as new cases

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/13_LLM_Evaluation_and_Guardrails]]: the online layer's metrics and stack
- [[computing-foundation-note/Artificial_Intelligence/12_AI_ROI_and_Roadmap]]: business metrics and success criteria
- [[04_Tracing_and_Observability]]: the instrumentation that carries these signals
- [[02_Offline_Evaluation_Suites]]: the destination for sampled failures
- [[career-path/09_Data_and_ML_Engineer/06_ML_Lifecycle_and_MLOps/05_ML_Monitoring_and_Drift|ML Monitoring and Drift]]: classic drift monitoring boundary
