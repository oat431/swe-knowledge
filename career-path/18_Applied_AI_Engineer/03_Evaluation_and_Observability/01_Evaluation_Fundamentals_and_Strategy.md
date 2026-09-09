---
title: "Evaluation Fundamentals and Strategy"
note_type: capability-topic
capability_area: evaluation-and-observability
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - evaluation
  - eval-strategy
---

# Evaluation Fundamentals and Strategy

> Designing the measurement system that decides whether an AI feature may ship — and knowing what each layer of measurement can and cannot prove.

## Why This Is a Senior Skill

A mid-level engineer treats evaluation as a QA step at the end of the project: run the prompt, eyeball the outputs, declare it good. A senior engineer writes the evaluation strategy before the feature: what "acceptable quality" means, how each failure mode is measured, what gates a release, and how the system re-measures itself after launch. The evaluation strategy is the design contract of the feature; code that ships without one is an experiment, not a product.

The senior challenge is sequencing measurement with the product. Evaluation costs money and latency, so the strategy must say *when* each measurement runs and *what it may conclude* — not merely that measurement exists. A strategy that cannot say which number blocks a release has not decided anything yet.

The payoff is organizational: with a strategy in place, prompt changes, model swaps, and pipeline edits all become comparable events instead of subjective bets.

## Core Frameworks

### The Three Evaluation Layers

| Layer | When It Runs | What It Proves | What It Cannot Prove |
|-------|-------------|----------------|---------------------|
| Offline evaluation | Before deploy | The system works on known, representative scenarios | That real users will behave like the test set |
| Runtime guardrails | Every request | Each request and response passes safety and validity checks | Overall quality trends (enforcement lives in [[career-path/18_Applied_AI_Engineer/04_AI_Security_and_Guardrails/00_overview|AI Security and Guardrails]]) |
| Online evaluation | In production | The system works for real traffic, with real feedback | *Why* a metric moved — that requires traces and offline replay |

### Strategy Decisions Every Team Must Make

| Decision | What to Do | Why |
|----------|-----------|-----|
| Success criteria | Define measurable targets before building (e.g., deflection >60%, faithfulness >95%) | You cannot know you arrived if you never defined the destination |
| Failure-mode mapping | List the ways the system can fail, then pick one metric per failure mode | A metric matching no failure mode produces numbers that reassure without informing |
| Cost tiers | Deterministic assertions on everything, LLM-as-judge on samples, humans on disagreements | Judge and human evaluation are expensive; spend where uncertainty is highest |
| Baselines | Record scores on the current system before any change | Without a baseline, "the new prompt is better" is unfalsifiable |
| Release authority | Define who may change a threshold and what evidence they need | Thresholds are product decisions with engineering consequences, not tuning knobs |
| Feedback loop policy | Every production incident ends as a new offline test case | A suite that never grows has stopped learning from users |

### Strategy Trade-Offs

| Trade-Off | The Pull | The Counterweight |
|-----------|----------|-------------------|
| More metrics vs more shipping | Each metric costs money and creates gate friction | Measure every failure mode you fear; tolerate measuring nothing else |
| Suite breadth vs suite freshness | Larger suites catch more but stale cases waste runs | Curate continuously; retire cases that no longer represent traffic |
| Judge depth vs judge cost | Granular rubrics score better but burn tokens | Granular only where disagreement is expensive; coarse elsewhere |
| Early measurement vs design speed | Measuring too early locks in premature targets | Write success criteria early, freeze thresholds only after baselines exist |

### Maturity Levels

| Level | What Exists | What It Can Catch |
|-------|-------------|-------------------|
| 1. Eyeballing | Developer runs the prompt, looks at outputs | Catastrophic breakage only |
| 2. Assertion suite | Deterministic checks on fixed cases | Format violations, price drift, broken refusals |
| 3. Judged suite | LLM-as-judge metrics with recorded baselines | Faithfulness and relevance regressions |
| 4. Gated CI | Suite runs on every PR with thresholds | Any change that regresses known quality |
| 5. Continuous loop | Online sampling feeds the suite; incidents become test cases | Drift and novel failure modes |

## In Practice

**Define success criteria before you build the feature.** The first deliverable of any AI project is the measurement plan, not the prompt. State targets in business terms the stakeholder recognizes — handoff rate, resolution rate, cost per reply — then derive the technical metrics that predict them. A feature whose success criteria cannot be written down cannot be evaluated, and the scale-or-kill decision in [[computing-foundation-note/Artificial_Intelligence/12_AI_ROI_and_Roadmap]] becomes guesswork.

**Map each metric to a failure mode it is meant to catch.** Faithfulness exists to catch hallucination against retrieved context; format validity exists to catch broken structured outputs; relevance exists to catch the model answering a different question. When a team cannot name the failure mode a metric detects, the metric is decorative — it will trend quietly while users absorb the real failures.

**Tier evaluation by cost and confidence.** Run cheap deterministic checks — schema validation, exact containment, refusal detection — on every evaluation run. Reserve LLM-as-judge scoring for open-ended qualities like helpfulness and tone, and reserve human review for the small sample where judge and stakeholder disagree. The tiers are a budget: a strategy that judges everything gets abandoned under cost pressure.

**Treat thresholds as product decisions, not engineering preferences.** Raising a faithfulness threshold means more releases blocked; lowering it means more user-facing errors. Both are product trade-offs, so the threshold and its rationale must be visible to the product owner, and changing one is a reviewable event — the same discipline as any contract change.

**Separate what offline can prove from what online can prove.** Offline evaluation proves the system handles known scenarios; it cannot prove real users are satisfied, because the test set is a snapshot of the team's assumptions. Online evaluation proves real-world performance but cannot isolate causes. A senior strategy uses both layers and, for each question, knows which one answers it.

**Every production failure becomes a new test case.** The eval-to-guardrail loop from [[computing-foundation-note/Artificial_Intelligence/13_LLM_Evaluation_and_Guardrails]] is a policy, not a habit: incident reviews end with a new offline case and, where possible, a runtime check. Incident review without suite growth is storytelling.

**Re-validate the strategy against production reality every quarter.** Traffic shifts, products change scope, and failure modes migrate. Every quarter, compare the suite's strata to live traffic, re-check judge calibration, and confirm that each metric still maps to a failure mode the team actually fears. A strategy written once is an assumption; a strategy revisited on schedule is a system.

## Practical Exercise

Write the evaluation strategy for one AI feature you own or want to build:
1. Write the business success criteria, one sentence each: metric, target, time frame (format from [[computing-foundation-note/Artificial_Intelligence/12_AI_ROI_and_Roadmap]])
2. Enumerate 5–10 concrete failure modes (e.g., hallucinated price, off-topic answer, refusal when context contains the answer)
3. Map each failure mode to one measurable check: deterministic assertion, LLM judge, or human review
4. Assign cost tiers: which checks run always, which on samples, which rarely
5. Define thresholds for each metric and name who is allowed to change them
6. Identify the three metrics that must be measured online after launch, and how (feedback capture, sampling, A/B)
7. Review the strategy against a recent production incident: would it have caught it? Add the missing case

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/13_LLM_Evaluation_and_Guardrails]]: the three-layer framework this topic turns into strategy
- [[computing-foundation-note/Artificial_Intelligence/12_AI_ROI_and_Roadmap]]: defining success before building; scale-or-kill
- [[02_Offline_Evaluation_Suites]]: the execution layer for offline strategy
- [[06_Regression_Gates_and_Continuous_Evaluation]]: turning thresholds into enforced gates
- [[career-path/09_Data_and_ML_Engineer/06_ML_Lifecycle_and_MLOps/03_Model_Training_and_Evaluation|Model Training and Evaluation]]: the trained-model counterpart of metric selection
