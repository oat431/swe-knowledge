---
title: "Model Training and Evaluation"
note_type: capability-topic
capability_area: ml-lifecycle-and-mlops
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - model-training
  - evaluation
  - mlops
---

# Model Training and Evaluation

> Selecting, training, and evaluating models using metrics that reflect business outcomes and validation strategies that prevent overfitting.

## Why This Is a Senior Skill

Mid-level engineers optimize for accuracy on a test set. Senior engineers choose evaluation metrics that align with business value, design validation strategies that expose overfitting and data leakage, and make go/no-go decisions based on evidence rather than hope.

The senior challenge is that a model can be statistically excellent and business-useless. Accuracy of 95% means nothing if the 5% of errors are the high-value cases.

## Core Frameworks

### Metric Selection by Business Objective

| Business Objective | Primary Metric | Secondary Metric | Why |
|-------------------|---------------|-----------------|-----|
| Minimize false alarms | Precision | Recall at fixed precision threshold | False positives have high cost |
| Catch every case | Recall | Precision at fixed recall threshold | Missing positives has high cost |
| Rank items for users | NDCG, MAP | Coverage, diversity | Ranking quality matters more than classification |
| Predict continuous value | MAE or RMSE | MAPE for relative error | MAE is robust, RMSE penalizes large errors |
| Minimize financial loss | Expected value per prediction | Calibration error | Business impact is the real metric |
| Fair outcomes across groups | Equalized odds, demographic parity | Overall accuracy | Fairness constraints may reduce accuracy |

### Validation Strategy Decision Matrix

| Strategy | When to Use | Risk if Wrong |
|----------|------------|---------------|
| Random train-test split | Large i.i.d. data, no time component | Time leakage if data has temporal structure |
| Time-based split | Time series, any temporal data | Training on future data leaks into test |
| k-Fold cross-validation | Small datasets, model comparison | Leakage if folds share correlated samples |
| Group k-fold | Data with natural groups: users, sessions | Same group in train and test inflates metrics |
| Stratified split | Imbalanced classes | Rare class underrepresented in test set |
| Holdout with production simulation | Final evaluation before deployment | Offline metrics do not reflect online behavior |

### Hyperparameter Tuning Approaches

| Approach | Cost | When to Use |
|----------|------|------------|
| Grid search | High: evaluates all combinations | Small search space, 2-3 parameters |
| Random search | Medium: samples configurations | Large search space, many parameters |
| Bayesian optimization | Low-medium: learns from past trials | Expensive evaluations, 5+ parameters |
| Hyperband | Medium: adaptive resource allocation | Many configurations, uncertain budget |
| Population-based training | High: parallel, evolutionary | Distributed training, large-scale |

## In Practice

**Align metrics with business value before training starts.** If the business cares about revenue per prediction, define that metric before choosing a model. Optimizing accuracy and then translating to revenue after the fact creates misalignment.

**Always use time-based splits for production models.** Real-world data has temporal structure. A random split lets the model see "future" patterns in training that it must predict in test. This inflates offline metrics and disappoints in production.

**Watch for data leakage.** Features computed from the target variable, features that include future information, or duplicated records that appear in both train and test all inflate metrics. Senior engineers audit feature definitions for leakage before trusting results.

**Evaluate on the distribution you will serve.** If production data differs from training data, evaluate on a sample that matches production. A model that performs well on last year's data may fail on today's shifted distribution.

**Report confidence intervals, not point estimates.** A single accuracy number hides variance. Report metrics with confidence intervals from cross-validation or bootstrap. A model with 85% +/- 2% is a different decision than 85% +/- 0.1%.

## Practical Exercise

Evaluate a model for a business scenario of your choice:
1. Define the business objective and choose primary and secondary metrics
2. Design a validation strategy appropriate for the data's temporal structure
3. Run at least 3 model configurations and compare them on your chosen metrics
4. Check for data leakage: audit your features for any dependency on the target
5. Report results with confidence intervals
6. Make a go/no-go recommendation with evidence

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/AI Overview]]: ML algorithm fundamentals
- [[01_Experiment_Tracking_and_Reproducibility]]: tracking training runs for comparison
- [[02_Feature_Engineering_and_Feature_Store]]: feature quality determines model quality
- [[04_Model_Serving_and_Inference]]: evaluation informs serving decisions
- [[05_ML_Monitoring_and_Drift]]: evaluation metrics become monitoring baselines

## Common Pitfalls

- Random splits on temporal data: time leakage inflates offline metrics, model fails in production
- Optimizing a proxy metric that does not correlate with business value
- Ignoring class imbalance: overall accuracy hides poor performance on minority classes
- No confidence intervals: point estimates hide variance, making go/no-go decisions unreliable
- Data leakage through features: features computed from the target variable create circular models

## Key Takeaways

- Evaluation metrics must reflect business value, not just statistical accuracy
- Time-based validation splits prevent the most common form of data leakage
- Data leakage inflates offline metrics and creates false confidence in production readiness
- Confidence intervals communicate uncertainty: point estimates hide the real risk
- Senior engineers define success metrics before training, not after
- The go/no-go decision is the senior engineer's accountability: evidence-based, not intuition-based
