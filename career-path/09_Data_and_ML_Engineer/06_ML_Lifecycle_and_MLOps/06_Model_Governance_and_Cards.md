---
title: "Model Governance and Cards"
note_type: capability-topic
capability_area: ml-lifecycle-and-mlops
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - model-governance
  - model-cards
  - fairness
---

# Model Governance and Cards

> Documenting, reviewing, and controlling models to ensure they meet fairness, regulatory, and organizational standards.

## Why This Is a Senior Skill

Mid-level engineers document models when asked. Senior engineers design governance workflows that make documentation automatic, establish fairness assessment criteria before training begins, and create approval processes that balance speed with accountability.

The senior challenge is that model governance is where technical, legal, and ethical responsibilities converge. A model that performs well but discriminates against a protected group is a liability, not an asset.

## Core Frameworks

### Model Card Sections

| Section | Content | Audience |
|---------|---------|----------|
| Model details | Name, version, type, training date, owner | Everyone |
| Intended use | What the model is for and what it is not for | Product teams, legal |
| Training data | Data sources, size, preprocessing, known biases | Data scientists, auditors |
| Evaluation | Metrics, test methodology, results by subgroup | Technical reviewers |
| Limitations | Known failure modes, out-of-scope scenarios | Product teams, legal |
| Ethical considerations | Fairness assessment, bias mitigation, residual risks | Legal, compliance, ethics |
| Caveats | Conditions under which the model may not perform as expected | Deployers, downstream teams |

### Fairness Assessment Framework

| Metric | Definition | When to Use |
|--------|-----------|-------------|
| Demographic parity | Equal positive rate across groups | When equal outcomes are the goal |
| Equalized odds | Equal TPR and FPR across groups | When equal accuracy is the goal |
| Predictive parity | Equal precision across groups | When equal trust in predictions is the goal |
| Individual fairness | Similar individuals get similar predictions | When case-by-case fairness matters |
| Counterfactual fairness | Prediction unchanged if protected attribute flipped | When causal fairness is required |
| Calibration | Equal predictive value within risk scores | When score interpretation must be consistent |

### Approval Workflow

| Stage | Gate | Approver | Evidence Required |
|-------|------|---------|-------------------|
| Development | Code review | Tech lead | Test coverage, documentation |
| Evaluation | Performance review | ML lead | Evaluation report, fairness metrics |
| Pre-production | Governance review | Governance board | Model card, risk assessment |
| Production | Deployment approval | Engineering manager | Rollback plan, monitoring setup |
| Ongoing | Periodic review | Model owner | Drift report, retraining history |

## In Practice

**Write the model card before training, not after.** Defining intended use, limitations, and fairness criteria before training forces clarity. If you cannot articulate what the model should not be used for, you are not ready to train it.

**Assess fairness on subgroups, not just overall.** A model with 90% overall accuracy may have 60% accuracy on a minority group. Evaluate performance across all relevant subgroups: gender, age, ethnicity, geography, and any domain-specific segments.

**Make governance proportional to risk.** A model that recommends articles needs lighter governance than a model that determines loan eligibility. Define risk tiers and match governance rigor to the potential harm.

**Automate what you can.** Model cards should pull evaluation metrics, training data statistics, and fairness scores from the experiment tracking system. Manual documentation goes stale. Automated documentation stays current.

**Plan for model retirement.** Every model has an end of life. Define retirement criteria in the model card: when accuracy drops below a threshold, when the use case is no longer valid, or when a successor model replaces it. Retirement includes removing the model from serving and archiving its artifacts.

## Practical Exercise

Create a model card for a model you have built or are building:
1. Define intended use and explicit non-use cases
2. Document training data: sources, size, known biases, preprocessing steps
3. Run a fairness assessment: evaluate performance across at least 3 subgroups
4. List limitations and known failure modes
5. Design an approval workflow: who reviews, what evidence, what gates
6. Define retirement criteria: when should this model be decommissioned?

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/AI Overview]]: AI ethics and governance
- [[01_Experiment_Tracking_and_Reproducibility]]: experiment records feed model cards
- [[03_Model_Training_and_Evaluation]]: evaluation results populate the model card
- [[05_ML_Monitoring_and_Drift]]: monitoring data feeds ongoing governance reviews
- [[05_Data_Security_and_Privacy/04_Privacy_Engineering|Privacy Engineering]]: privacy considerations in model governance

## Common Pitfalls

- Writing model cards after deployment: documentation goes stale and is never updated
- Fairness assessed only on overall metrics: subgroup performance hides discriminatory patterns
- Governance as a document, not a workflow: approval gates that are bypassed under deadline pressure
- No retirement plan: deprecated models continue serving, accumulating technical debt
- Treating all models the same: a recommendation model does not need the same governance as a credit scoring model

## Key Takeaways

- Model governance is proportional to risk: high-impact models need rigorous review, low-impact models need basic documentation
- Write model cards before training to force clarity on intended use and limitations
- Fairness assessment requires subgroup evaluation: overall metrics hide discriminatory patterns
- Automate model card population from experiment tracking: manual documentation goes stale
- Model retirement is part of governance: define end-of-life criteria before deployment
- Senior engineers make governance a workflow, not a document: approval gates are enforced, not optional
