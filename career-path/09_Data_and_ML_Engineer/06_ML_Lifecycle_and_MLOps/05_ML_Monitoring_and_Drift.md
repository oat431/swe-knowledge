---
title: "ML Monitoring and Drift"
note_type: capability-topic
capability_area: ml-lifecycle-and-mlops
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - ml-monitoring
  - drift-detection
  - mlops
---

# ML Monitoring and Drift

> Detecting when a deployed model's performance degrades due to changes in data distribution or the relationship between features and outcomes.

## Why This Is a Senior Skill

Mid-level engineers set up dashboards. Senior engineers design monitoring systems that distinguish meaningful drift from noise, choose detection methods appropriate for the data type and volume, and define retraining triggers that balance model freshness against retraining cost.

The senior challenge is that drift is inevitable. The question is not whether it will happen but how quickly you detect it and how confidently you act.

## Core Frameworks

### Types of Drift

| Type | What Changes | Detection Method | Response |
|------|-------------|-----------------|----------|
| Data drift | Input feature distribution shifts | Statistical tests: KS, PSI, KL divergence | Retrain with new data |
| Concept drift | Relationship between features and target changes | Monitor prediction accuracy over time | Retrain with new labels |
| Feature drift | Individual feature distribution changes | Per-feature statistical tests | Investigate source, retrain if significant |
| Prediction drift | Output distribution shifts | Compare prediction histograms | May indicate data or concept drift |
| Upstream data drift | Source system changes data format or semantics | Schema validation, null rate monitoring | Fix upstream, reprocess if needed |

### Drift Detection Methods

| Method | Data Type | Sensitivity | Computational Cost |
|--------|-----------|-------------|-------------------|
| Kolmogorov-Smirnov test | Continuous, univariate | Medium | Low |
| Population Stability Index | Continuous, binned | Medium | Low |
| Chi-squared test | Categorical | Medium | Low |
| Wasserstein distance | Continuous, multivariate | High | Medium |
| KL divergence | Probability distributions | High | Medium |
| Page-Hinkley test | Time series, streaming | High for sudden drift | Low |
| ADWIN | Streaming, adaptive | High for gradual drift | Medium |
| Model-based detection | Any, uses a classifier | Very high | High |

### Alerting Strategy

| Alert Level | Trigger | Response Time | Action |
|-------------|---------|---------------|--------|
| Info | PSI 0.1 to 0.2: mild drift | Next business day | Log for review |
| Warning | PSI 0.2 to 0.3: moderate drift | Within 24 hours | Investigate root cause |
| Critical | PSI > 0.3 or accuracy drop > 5% | Within 4 hours | Retrain or rollback |
| Emergency | Model serving errors > 1% | Immediate | Investigate infrastructure |

## In Practice

**Monitor features, not just predictions.** Prediction drift tells you something changed. Feature drift tells you what changed. Monitor the top 10 features by importance and the features most likely to drift based on domain knowledge.

**Use PSI for tabular data, KS test for continuous distributions.** PSI is intuitive: 0-0.1 is stable, 0.1-0.2 is mild drift, above 0.2 is significant. KS test is more statistically rigorous for continuous features. Use both for comprehensive coverage.

**Set baselines from training data, compare against serving data.** The training data distribution is your baseline. Every batch of serving data should be compared against this baseline. Drift is the divergence between what the model was trained on and what it sees in production.

**Do not retrain on every drift signal.** Some drift is noise. Define thresholds that trigger investigation, not automatic retraining. A senior engineer investigates root cause before retraining: maybe the drift is a data pipeline bug, not a real distribution change.

**Build a retraining pipeline that is triggerable, not scheduled.** Scheduled retraining wastes resources when the model is stable and is too slow when drift is rapid. Build a pipeline that triggers on confirmed drift signals and can be run on demand.

## Practical Exercise

Design a monitoring system for a deployed model:
1. List the top 5 features to monitor and the drift detection method for each
2. Define alerting thresholds: what PSI or accuracy drop triggers each alert level?
3. Design the comparison: what is the baseline, how often do you compare, what window size?
4. Write a runbook for the most common alert: "PSI exceeds 0.25 on feature X"
5. Design the retraining trigger: what conditions initiate automatic retraining?
6. Plan the rollback criteria: when do you revert to the previous model version?

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/AI Overview]]: model lifecycle management
- [[02_Feature_Engineering_and_Feature_Store]]: feature store provides monitoring data
- [[04_Model_Serving_and_Inference]]: monitoring deployed models
- [[03_Model_Training_and_Evaluation]]: retraining uses the same evaluation framework
- [[07_Production_Engineering/03_Reliability_and_Fault_Tolerance|Reliability and Fault Tolerance]]: monitoring as part of reliability

## Common Pitfalls

- Monitoring predictions only: prediction drift tells you something changed but not what or why
- Alerting on every statistical fluctuation: noise triggers alert fatigue, real drift gets ignored
- Automatic retraining on any drift signal: retraining on noise wastes resources and may worsen the model
- No baseline comparison: comparing today against yesterday instead of against the training distribution
- Ignoring upstream data drift: a source system schema change causes silent model degradation

## Key Takeaways

- Drift is inevitable: the senior skill is detecting it quickly and responding appropriately
- Monitor features and predictions: feature drift tells you what changed, prediction drift tells you something changed
- PSI is the most intuitive drift metric for tabular data with clear thresholds
- Do not retrain on every signal: investigate root cause before committing to retraining
- Retraining should be triggerable on confirmed drift, not on a fixed schedule
- Alerting thresholds must be calibrated to your domain: generic thresholds create alert fatigue
