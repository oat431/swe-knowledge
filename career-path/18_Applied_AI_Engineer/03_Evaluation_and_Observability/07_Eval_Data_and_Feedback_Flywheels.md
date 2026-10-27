---
title: "Eval Data and Feedback Flywheels"
note_type: capability-topic
capability_area: evaluation-and-observability
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
created: 2026-10-21
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - eval-data
  - feedback-loops
---

# Eval Data and Feedback Flywheels

> Building the data flywheel behind evaluation: golden sets, synthetic data, annotation workflows, and turning production failures into permanent test cases.

## Why This Is a Senior Skill

A mid-level engineer assembles a handful of examples, runs them a few times, and trusts the number. A senior engineer treats eval data as a maintained product with its own pipeline: golden cases chosen to cover known failure modes rather than happy paths, synthetic data validated before it enters the suite, annotation workflows that surface disagreement instead of hiding it, and a routine that converts every production incident into a permanent test case within days. The suite in [[02_Offline_Evaluation_Suites]] is the machinery; this note is about the fuel and the refinery.

The senior question is not "do we have evals?" but "does every failure this system is capable of producing have a case that would catch it, and does new evidence from production reach the suite before the same failure ships twice?"

## Core Frameworks

### Golden-Set Design

| Design Axis | Choice | Why It Matters |
|-------------|--------|----------------|
| Coverage source | Production failure modes first, happy paths second | The golden set exists to prevent repeats, not to celebrate what works |
| Strata | By topic, difficulty, language, risk tier | Averages hide collapse in the slices that matter most |
| High-stakes slice | Verified cases with hard pass/fail: pricing, policy, compliance | These absorb no threshold slack; they gate releases outright |
| Size discipline | Small enough to review by hand, large enough to cover strata | An unreviewable golden set becomes unowned and silently stale |
| Provenance | Every case records where it came from and why it is included | Cases without provenance get deleted in the first cleanup and nobody knows what was lost |
| Versioning | Cases change only through review, with the rubric in the same commit | A score means nothing without dataset, rubric, and system version recorded together |

### Synthetic Eval Data: Generation and Validation

| Generation Method | Strength | Validation Required Before Use |
|-------------------|----------|-------------------------------|
| Model-generated variants of real cases | Cheap expansion of known patterns | Check label consistency with the source case; sample human review |
| Templated cases from schemas and rules | Deterministic, controllable difficulty | Ensure templates do not leak into prompts the system was tuned on |
| Adversarial generation (injection, boundary probes) | Covers attack surface deliberately | Cap its share of the suite; verify cases still reflect real threat models |
| Paraphrase and difficulty laddering | Same semantics, varied surface form | Confirm semantic equivalence; paraphrase drift is a common false case |
| Back-translation or cross-lingual generation | Language coverage beyond the team's own | Native-speaker sampling for naturalness and label fidelity |

### Annotation Workflow and the Failure-to-Case Pipeline

| Stage | What Happens | Quality Control |
|-------|--------------|-----------------|
| Capture | Log the failing input, system output, and context verbatim from production | Retention rules applied at capture; consent-checked fields only |
| Minimize | Reduce to the smallest input that reproduces the failure | Re-run to confirm the minimal case still fails |
| Classify | Tag failure mode: grounding, extraction, policy, format, prompt injection | Shared taxonomy; multi-label allowed when the failure is compound |
| Annotate | Define the expected behavior and the check that asserts it | Two independent annotations on ambiguous cases; disagreement is data |
| Adjudicate | Resolve disagreement through a written rubric, not a coin flip | Record the ruling and the rubric clause it cites |
| Add | Merge into the suite with provenance and stratum tags | CI runs the case before merge; the case must fail the old system |
| Retire | Remove cases that now fail only because the product changed | Review note on retirement, like any test-suite change |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Production Traffic"] --> B["Failure Capture"]
    B --> C["Minimize and Classify"]
    C --> D["Annotation and Adjudication"]
    D --> E["Golden Set and Eval Suite"]
    E --> F["Regression Gates"]
    F --> G["Improved System"]
    G --> A
```

## In Practice

**Curate golden sets for failure-mode coverage, not for comfort.** The cases that feel representative are the ones the system already handles; the value lives in the modes the team has been burned by. Seed each stratum from production incidents and support escalations, then fill gaps deliberately. A golden set with no cases that currently fail is a golden set that has stopped looking.

**Validate synthetic data before it earns suite membership.** Synthetic cases are cheap and unlimited, which is exactly why they quietly dominate suites that skip validation. Check label consistency against the rule or source case that generated them, sample a human review batch per generator, and track per-generator failure rates; a generator that produces ambiguous labels is a metric distortion, not a coverage win.

**Treat annotation disagreement as signal, not noise to average away.** When two annotators disagree, either the case is genuinely ambiguous or the rubric is underspecified; both are findings. Measure disagreement per label and per stratum, route contested cases to adjudication with a written rubric, and revise the rubric when the same disagreement recurs. The calibration discipline behind [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/02_Offline_Evaluation_Suites|Offline Evaluation Suites]] depends on this.

**Run the production-failure-to-eval-case pipeline on a clock.** Capture, minimize, classify, add: four steps, done the same week, with the added case required to fail the system that produced the incident. An incident that is not converted into a case is an incident waiting to repeat, and a backlog of uncaptured failures is the strongest predictor of a repeat outage.

**Fight eval-set drift with freshness, not nostalgia.** User behavior, product scope, and attack patterns move; the suite stands still unless it is re-stratified against fresh production samples and retired cases are actually deleted. Schedule a re-derivation of strata at a fixed cadence, compare suite distribution to current traffic, and treat a widening gap as a defect in the suite.

**Keep privacy and consent inside the eval pipeline, not beside it.** Eval data is production data: capture with the same consent, retention, and access rules, minimize identifiers before annotation, and keep raw user content out of shared golden sets where a redacted or derived form asserts the same check. Annotation vendors see the data too; their access is part of the review surface in [[career-path/18_Applied_AI_Engineer/04_AI_Security_and_Guardrails/00_overview|AI Security and Guardrails]].

**Avoid benchmark-overfitting your own golden set.** If the team tunes prompts and models against the same suite that gates releases, scores rise while real quality stands still. Hold back a rotating audit slice never used for tuning, periodically run cases the team has not seen, and treat a growing gap between suite scores and production feedback as proof of overfitting.

## Practical Exercise

Stand up the flywheel for a customer-support RAG assistant:
1. Define the strata for the golden set (topic, difficulty, language, risk tier) and the target case count per stratum
2. Collect 10 real failure or near-failure cases from production logs or support escalations
3. For two cases, run the full pipeline: capture, minimize to the smallest reproducing input, classify the failure mode, write the expected behavior and the asserting check
4. Generate 10 synthetic variants from existing cases and define the validation step each must pass before inclusion
5. Write the annotation rubric for one ambiguous case type and specify how annotator disagreement is recorded and adjudicated
6. Design the freshness routine: cadence for re-stratification against live traffic, and the retirement rule for stale cases
7. Define the privacy gates: what is redacted at capture, who may see raw data, and where consent is checked

## Practical Applications

- [ ] Golden set is stratified by failure mode, difficulty, language, and risk tier
- [ ] Every case carries provenance: where it came from and why it is included
- [ ] Synthetic generators each have a validation step and a tracked error rate
- [ ] Failure-to-case pipeline has an owner and a same-week service level
- [ ] Annotation disagreements are measured, adjudicated against a written rubric, and feed rubric revisions
- [ ] Re-stratification against live traffic runs on a fixed cadence; stale cases are retired with notes
- [ ] Eval data obeys the same consent, retention, and redaction rules as production data
- [ ] A held-back audit slice exists that is never used for prompt or model tuning

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Golden set full of happy-path examples | Measures what already worked; regressions in real failure modes pass silently | Seed strata from incidents and escalations first |
| Synthetic data merged without validation | Ambiguous labels distort scores worse than missing coverage | Per-generator validation, human sampling, tracked error rates |
| Averaging away annotator disagreement | Hides rubric gaps and genuinely ambiguous cases | Measure disagreement; adjudicate with a written rubric |
| Production failures queue up "for later" | The same failures ship again; the suite never learns | Same-week capture, minimize, classify, add pipeline |
| Tuning prompts against the suite that gates releases | Scores inflate while production quality stands still | Held-back audit slice never used for tuning |
| Eval copies of raw production data shared freely | Consent and retention violations travel with the dataset | Redact at capture; minimize identifiers; restrict access |
| Suite never re-stratified as traffic shifts | Coverage drifts from what users actually do | Scheduled re-derivation of strata from fresh samples |

## Knowledge Connections

- [[02_Offline_Evaluation_Suites]]: the suite machinery this data flywheel feeds
- [[05_Online_Evaluation_and_Monitoring]]: production sampling that replenishes the flywheel
- [[06_Regression_Gates_and_Continuous_Evaluation]]: where new cases become release gates
- [[computing-foundation-note/Artificial_Intelligence/13_LLM_Evaluation_and_Guardrails|LLM Evaluation and Guardrails]]: the eval-to-guardrail lifecycle for recurring failures
- [[career-path/18_Applied_AI_Engineer/04_AI_Security_and_Guardrails/00_overview|AI Security and Guardrails]]: consent, redaction, and access rules for eval data

## Sources

- AI Engineering (Chip Huyen)
- Designing Machine Learning Systems (Chip Huyen)
- Designing Data-Intensive Applications (Martin Kleppmann)
- OWASP Top 10 for LLM Applications
