---
title: "Bias and Fairness Assessment"
note_type: capability-topic
capability_area: responsible-ai-and-governance
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - bias
  - fairness
  - algorithmic-fairness
---

# Bias and Fairness Assessment

> Measuring whether an AI system harms different groups unequally, then choosing mitigations with eyes open to what each one trades away.

## Why This Is a Senior Skill

Mid-level engineers know "bias is bad" and add "use diverse data" to a checklist. Senior engineers know that fairness metrics are mathematically incompatible with each other, that measurement requires defining groups and harms before touching data, and that in LLM applications most bias enters through retrieval, prompts, and deployment context — not model weights. They also know the difference between a disparity that causes real harm and a statistical difference that does not.

The senior challenge is precision: every mitigation trades something — accuracy, scope, cost, or another group's outcome. The senior's job is to measure, choose, and document the trade-off so the decision survives scrutiny later.

## Core Frameworks

### Bias Sources in LLM Applications

| Source | Mechanism | Example |
|--------|-----------|---------|
| Training data | Model absorbs statistical patterns of its pretraining text | Stereotypical associations for professions |
| Retrieval and context | Biased corpus or ranking surfaces skewed content | RAG over historical hiring records |
| Prompt and template design | Wording steers outputs systematically | "Describe a typical engineer" priming a single profile |
| Deployment context | Identical output harms differently across users | Casual tone fine in one market, unprofessional in another |
| Feedback loops | Users reinforce model behavior, skewing future data | Preference data collected from a non-representative user base |

### Fairness Metrics and Their Trade-Offs

| Metric | Definition | Trade-Off / When To Use |
|--------|-----------|-------------------------|
| Demographic parity | Same selection or positive-outcome rate across groups | Ignores merit differences; use when outcomes should not depend on group membership |
| Equalized odds | Equal false-positive and false-negative rates across groups | Can conflict with parity; use when errors themselves are the harm |
| Calibration by group | Predictions mean the same thing across groups | Hardest to satisfy alongside others; use when confidence scores drive decisions |
| Individual fairness | Similar individuals treated similarly | "Similar" is hard to define; useful for recommendation-like systems |

These metrics are provably not all satisfiable at once — the impossibility theorems. Choosing one is a policy decision made with stakeholders, not a technical detail to be optimized away.

### Assessment Workflow

| Stage | What To Do | Why |
|-------|-----------|-----|
| Define | Choose groups, harms, and success criteria with stakeholders | Measurement is meaningless without named groups |
| Measure | Disaggregate evaluation metrics by group; build subgroup test sets | Aggregate accuracy hides group-level failures |
| Triage | Rank disparities by harm severity and likelihood | Not every statistical difference matters equally |
| Mitigate | Select from data, model, or system options | Each option trades accuracy, cost, or scope |
| Monitor | Track subgroup metrics in production | Usage shifts change the exposure over time |

### Mitigation Options Compared

| Option | Example | Effectiveness | Cost / Risk |
|--------|---------|---------------|-------------|
| Data-level | Filtering, augmentation, synthetic balancing of eval sets | High for training bias; limited for already-deployed LLMs | Pipeline cost; risk of overcorrection |
| Model-level | Fine-tuning, preference data shaping behavior | High for tone and behavior; careful work needed to avoid new skew | Training cost; regression risk |
| System-level | Prompt constraints, output guardrails, human review for sensitive cases | Fast and deployable today; limited depth | Latency; coverage gaps |

### Measurement Pitfalls

| Pitfall | Why It Misleads | Correct Practice |
|---------|-----------------|------------------|
| Aggregate-only metrics | 95% overall accuracy hides 80% for one group | Always report subgroup-level metrics |
| Proxy groups | Gender-coded names or language are proxies, not the real attribute | Use the actual protected attribute where legally available |
| One-shot audits | A clean launch-day audit says nothing about next quarter | Re-run stratified evals at every significant release |
| Sampling bias in eval sets | Synthetic or convenience samples miss real user distribution | Build eval sets from production traffic |

## In Practice

**Define protected groups and harms before touching data.** The groups to protect come from the legal and social context — under Thailand's PDPA, sensitive data includes race, religion, health, and political opinion, so features processing such attributes carry a legal duty on top of an ethical one. Decide who could be harmed, how, and how you will detect the harm before building any test set.

**Fairness metrics conflict — pick the one matching the business harm.** You cannot satisfy demographic parity, equalized odds, and calibration simultaneously, so the choice must follow the harm: a hiring tool's worst failure is rejecting qualified candidates (equalized odds); a marketing tool's worst failure is systematically excluding a community (parity). Name the harm, then choose the metric, then document the choice.

**Disaggregate every headline metric.** A system with 95% overall accuracy can score 80% for Thai-language non-native speakers while nobody notices — the headline number hides it. Disaggregation by language, region, and demographic is the single highest-value fairness practice an applied AI engineer can adopt, and it costs nothing beyond stratified eval sets.

**In RAG systems, bias usually enters through retrieval and prompts, not weights.** Before fine-tuning anything, audit what the retriever surfaces and what the system prompt asks the model to do — fixing a skewed corpus or a leading prompt is faster and more durable than retraining. Model-level fixes are the last resort, not the first.

**Bias audits are ongoing, not launch-day events.** Usage drift, model swaps, new markets, and new training data all change the exposure. Re-run the stratified eval suite at every significant release (see [[01_AI_Risk_Management]] for change-triggered re-assessment) and alert on subgroup metric drift, the same way you alert on latency.

**Document the trade-off decision.** Record which metric was prioritized, why, and which disparities were accepted as residual — that record is what regulators, auditors, and successors will ask for. The write-up belongs in the [[03_Transparency_and_Model_Cards]] model card, not in a chat thread.

## Practical Exercise

Assess fairness for one AI feature — a support chatbot, resume screener, or content classifier:
1. Choose the system and define the protected groups relevant to its market (e.g. Thai vs non-Thai language users, age, region) and the concrete harms to watch
2. Build a stratified test set with 50+ cases per group, mirroring real user distribution
3. Run the system and compute at least two fairness metrics, disaggregated by group
4. Identify the largest disparity and trace its root cause: data, retrieval, prompt, or model
5. Choose and implement one mitigation (prompt constraint, guardrail, corpus fix, or fine-tune)
6. Re-measure: verify the target disparity shrank and no new disparity appeared elsewhere
7. Write the findings — metric choice, root cause, mitigation, residual risk — into the model card

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/08_AI_Ethics_and_Future]] — fairness definitions and impossibility theorems
- [[computing-foundation-note/Artificial_Intelligence/13_LLM_Evaluation_and_Guardrails]] — eval suites and guardrails that carry the measurement
- [[01_AI_Risk_Management]] — bias as a scored risk class
- [[03_Transparency_and_Model_Cards]] — where the trade-off decision gets documented
- [[04_Regulation_and_Compliance]] — PDPA sensitive-data duties and EU AI Act bias obligations
