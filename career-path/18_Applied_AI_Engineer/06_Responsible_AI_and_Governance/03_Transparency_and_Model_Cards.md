---
title: "Transparency and Model Cards"
note_type: capability-topic
capability_area: responsible-ai-and-governance
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - transparency
  - model-cards
  - documentation
---

# Transparency and Model Cards

> Documenting what a model or AI system is, what it can and cannot do, and how it was evaluated, so users, auditors, and future engineers can make informed decisions without reading code.

## Why This Is a Senior Skill

Mid-level engineers write a README — or nothing. Senior engineers treat the model card as a release contract: limitations published before capabilities, evaluation evidence instead of claims, and generation wired into the eval pipeline so the documentation cannot drift from reality. A card is the difference between "trust us" and "verify this."

The senior challenge is honesty under pressure. Marketing wants the card to sell; the senior's job is to keep the negative results in — they are the point. A card that only lists strengths has zero credibility with auditors and zero utility for the next engineer.

## Core Frameworks

### Model Card Sections

| Section | Content | Why |
|---------|---------|-----|
| Model details | Provider, version, size, modality, license | Traceability across releases |
| Intended use | Scenarios and users the system is designed for | Defines the boundary of claimed safety |
| Out-of-scope use | Uses explicitly not supported | Legal and ethical hook; limits liability |
| Training data | Sources, timeframe, known gaps | Answers auditor questions about provenance |
| Evaluation results | Metrics, disaggregated by subgroup | Evidence, not claims |
| Limitations and biases | Known failure modes, bias findings | The honest core of the card |
| Maintenance and versioning | Cadence, owner, retirement plan | Card stays true over time |

### Card Types Compared

| Type | Scope | Audience | Maintenance Cost |
|------|-------|----------|------------------|
| Model card | A foundation model itself | Downstream engineers, researchers | Provider-maintained |
| System card | The full deployed system (RAG, agents, guardrails) | Users, auditors, regulators | Owner team, per release |
| Datasheet | A training or evaluation dataset | Data consumers | Low frequency |

### Transparency Maturity

| Level | What Exists | Signal |
|-------|-------------|--------|
| None | Nothing beyond code | Highest residual risk |
| Ad-hoc | README plus launch notes | Regulator interviews will hurt |
| Template | Standard card per release | Baseline defensibility |
| Auto-generated | Cards built from eval CI and traces | Documentation cannot drift from reality |
| Versioned contract | Card versioned with each release, reviewed at the gate | Senior standard |

### Tooling Examples

| Approach | Example | Strength | Weakness |
|----------|---------|----------|----------|
| Manual template | Markdown template in the repo | Full control, zero cost | Drifts from reality; needs discipline |
| Provider metadata | Hugging Face model-card metadata | Standard fields, ecosystem-readable | Model-level only, not system-level |
| Trace export | Export eval runs from tracing tools (e.g. LangSmith, Langfuse) | Auto-fills evaluation sections | Needs mapping to card structure |
| CI-generated | Generate the card in the eval pipeline; fail the release if stale | Documentation as code | Setup cost |

### Card Review Questions

| Question | What a Weak Card Does |
|----------|-----------------------|
| Can I state who this system is NOT for? | Avoids out-of-scope uses; risks silent misuse |
| Are evaluation numbers dated and versioned? | Shows a snapshot with no model version attached |
| Are results disaggregated by subgroup? | Reports a single aggregate accuracy |
| Who is accountable for keeping this card current? | Names no owner and no review cadence |

## In Practice

**Publish limitations before capabilities.** The card's credibility comes from documenting failure honestly: quantified error rates, known failure modes, the exact subgroups the system is weak on (from [[02_Bias_and_Fairness_Assessment]]). A card listing only strengths is marketing; a card with measured failure modes is evidence. Client and auditor trust correlates with candor, not polish.

**Disaggregate evaluation results by subgroup.** A single accuracy number hides who the system fails. Break results down by language, region, and the groups identified in the fairness assessment. Disaggregation is the difference between a benchmark report and a fairness record — and it is increasingly the difference between passing and failing regulatory review.

**State intended use and out-of-scope use explicitly.** These two sections carry legal weight under the EU AI Act and set expectations with customers. "Not for medical, legal, or safety-critical decisions" is one sentence that protects both the company and the user, and it makes the card a governance instrument rather than a formality.

**Generate cards from the evaluation pipeline, not from memory.** Manual documentation drifts the day after it is written. Wire card generation into eval CI (see [[computing-foundation-note/Artificial_Intelligence/13_LLM_Evaluation_and_Guardrails]]) so the numbers in the card are the numbers the release actually passed. When the card and the eval suite come from the same artifact, lying requires active effort.

**Write for two audiences: the auditor and the next engineer.** An auditor needs sources, dates, and version history. The next engineer needs known failure modes and maintenance state. A good card serves both without padding; a card written only for compliance is read by no one and maintained by no one.

**Version cards with the release, not the calendar.** Every model or prompt change should produce a card diff reviewed at the same gate as the code (see [[06_Governance_Operating_Model]]). The card is part of the release artifact — which is what makes it evidence rather than decoration.

## Practical Exercise

Build a model card for one deployed AI feature:
1. Pick the feature and gather its eval results, model versions, data sources, and known incidents
2. Fill a model-card template with real data — no field left as "TBD"
3. Write the limitations section first, from actual observed failures rather than hypotheticals
4. Have a reviewer who did not build the system verify every factual claim in the card
5. Publish the card alongside the next release notes and link it from the feature's documentation
6. Automate: add a CI step that regenerates the evaluation sections from the eval suite, and schedule a quarterly card review with the model owner

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/13_LLM_Evaluation_and_Guardrails]] — the eval pipeline that supplies the card's numbers
- [[computing-foundation-note/Artificial_Intelligence/08_AI_Ethics_and_Future]] — transparency and explainability demands
- [[02_Bias_and_Fairness_Assessment]] — subgroup results and trade-off decisions the card must carry
- [[04_Regulation_and_Compliance]] — cards as compliance evidence under the EU AI Act
- [[01_AI_Risk_Management]] — documented limitations as mitigation of misrepresentation risk
