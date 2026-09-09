---
title: "Metrics for LLM Outputs"
note_type: capability-topic
capability_area: evaluation-and-observability
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - llm-metrics
  - llm-as-judge
  - faithfulness
  - hallucination
---

# Metrics for LLM Outputs

> Scoring what a model produced — correctness, faithfulness, relevance, format — with a deliberate mix of deterministic checks, LLM judges, and human review.

## Why This Is a Senior Skill

A mid-level engineer reports that "the outputs look good." A senior engineer decomposes "good" into axes that each map to a failure mode: correctness, faithfulness to context, relevance to the query, and format validity. They know LLM-as-judge is biased in predictable ways and calibrate judges against human labels before trusting any score, and they know when a cheap deterministic check beats an expensive model.

The senior challenge is calibration: every judge and every threshold must be validated against a human-labeled sample, or the numbers are theater that will be trusted in exactly the moments they are wrong.

## Core Frameworks

### Quality Axes

| Axis | Definition | Example Check | Detection Method |
|------|-----------|---------------|-----------------|
| Correctness | Answer matches ground truth | "Product X costs ฿500" must state ฿500 | Assertion or judge against reference |
| Faithfulness | Every claim is supported by provided context | No fact absent from the retrieved documents | Claim decomposition + context comparison |
| Relevance | Answer addresses the question asked | Password-reset answer for a password-reset question | Judge or embedding similarity |
| Format | Output matches the required schema | Valid JSON with required fields and types | Deterministic schema validation |
| Safety | No harmful or policy-violating content | No medical advice, no PII disclosure | Guardrail checks (enforcement in [[career-path/18_Applied_AI_Engineer/04_AI_Security_and_Guardrails/00_overview|AI Security and Guardrails]]) |

### Measurement Methods and Their Limits

| Method | Measures | Misses | Cost |
|--------|----------|--------|------|
| Exact match / assertions | Deterministic properties | Anything paraphrased | ~Zero |
| LLM-as-judge | Open-ended quality on a rubric | Rare, subtle failures the rubric omits | Medium |
| Human review | True quality as users perceive it | Coverage — only samples are affordable | High |
| Reference-based (ROUGE, BLEU, BERTScore) | Surface similarity to references | Factual correctness; rewards fluent nonsense | Low |
| Retrieval metrics (Recall@k, MRR, NDCG) | Whether the right context was found | Generation quality on top of it | Low |

### LLM-as-Judge: Known Failure Modes

| Failure Mode | What Happens | Mitigation |
|--------------|--------------|------------|
| Position bias | Judge prefers the first-presented answer | Randomize order; judge each answer independently |
| Self-preference | Judge favors its own style or model family | Use a stronger, different-family judge; calibrate against humans |
| Verbosity bias | Longer answers score higher regardless of quality | Rubric penalizes length; add length controls to the judge prompt |
| Rubric vagueness | Coarse rubric produces noisy scores | Granular per-criterion rubrics with concrete examples |
| Judge drift | Judge model updates change scoring behavior | Pin judge version; re-calibrate on every update |

### RAG Quality Targets (starting points, not universals)

| Metric | Meaning | Common Target |
|--------|---------|---------------|
| Recall@k | Fraction of relevant docs in top-k retrieval | >90% |
| Faithfulness | % of claims supported by the provided context | >95% |
| Answer relevance | Response addresses the question | >90% |
| Context precision | Retrieved docs are relevant to the query | >80% |

These are baselines to argue from, not laws: a support bot can tolerate lower relevance than a medical assistant. Targets must be re-derived per use case and per risk tolerance.

### Hallucination Detection Signals

| Signal | Meaning |
|--------|---------|
| Fact in response absent from retrieved context | Likely hallucination |
| Price/stock/date in response mismatches the source | High-confidence hallucination — critical |
| "I don't know" while the context contains the answer | Retrieval failure, not model laziness |
| Confident and completely wrong | Most dangerous — needs a human-review trigger |

## In Practice

**Detect hallucination by comparing decomposed claims to the context, never by vibes.** Split the answer into atomic claims; for each, ask whether the retrieved context supports it; report supported-over-total. This converts "the model feels truthful" into a number and is the backbone of faithfulness measurement in RAG systems.

**Calibrate every LLM judge against human labels before trusting it.** On a pilot batch of 50–100 outputs, have humans score with the same rubric, then measure judge–human agreement. Where they diverge, fix the rubric or the judge prompt — not the numbers. An uncalibrated judge turns CI into a source of false confidence.

**Use deterministic checks wherever the property is deterministic.** JSON validity, schema completeness, price containment, refusal keywords: these are exact and free. Spending judge tokens on a property a parser can verify is waste, and exact checks never suffer position bias.

**Choose the metric by the failure you fear.** Faithfulness for grounding failures, format validity for structured-output failures, relevance for retrieval-or-intent failures. Metric selection is a statement about what the team believes can go wrong — make that statement explicit rather than defaulting to a generic quality score.

**Reference-based metrics measure surface, not truth.** ROUGE and BLEU reward word overlap with a reference; a fluent, confidently wrong summary can score well. Use them for phrasing-adjacent tasks like summarization compression, and pair them with a faithfulness measure whenever facts matter.

**Report distributions and disagreement, not just averages.** A mean faithfulness of 0.94 that hides a stratum at 0.70 is a lie by aggregation. Report per-stratum scores, score distributions, and judge confidence, and treat rising judge–human disagreement as a signal that the measurement itself needs work.

## Practical Exercise

Implement and calibrate a faithfulness metric:
1. Take 20 outputs from your system and decompose each into atomic claims
2. Human-label each claim as supported, unsupported, or irrelevant to the provided context
3. Write an LLM-judge prompt that performs the same claim-by-claim check with a granular rubric
4. Run the judge on the same 20 outputs and compute claim-level agreement against the human labels
5. Fix the rubric where agreement is low; re-run until agreement reaches an acceptable level
6. Pin the judge's model and version, and document the calibration sample
7. Add the calibrated metric to the offline suite from [[02_Offline_Evaluation_Suites]]

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/13_LLM_Evaluation_and_Guardrails]]: metric families, targets, and hallucination signals
- [[computing-foundation-note/Artificial_Intelligence/10_LLM_Production_Patterns]]: grounding and hallucination as the patterns' failure modes
- [[02_Offline_Evaluation_Suites]]: where these metrics run
- [[06_Regression_Gates_and_Continuous_Evaluation]]: where these metrics gate
- [[career-path/18_Applied_AI_Engineer/06_Responsible_AI_and_Governance/00_overview|Responsible AI and Governance]]: bias and fairness metrics, intentionally out of scope here
