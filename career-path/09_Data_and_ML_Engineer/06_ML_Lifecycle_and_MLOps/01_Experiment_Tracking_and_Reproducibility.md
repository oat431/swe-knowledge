---
title: "Experiment Tracking and Reproducibility"
note_type: capability-topic
capability_area: ml-lifecycle-and-mlops
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - experiment-tracking
  - reproducibility
  - mlops
---

# Experiment Tracking and Reproducibility

> Recording every input, parameter, and output of an ML experiment so it can be reproduced exactly at any future point.

## Why This Is a Senior Skill

Mid-level engineers log metrics to a dashboard. Senior engineers design experiment tracking systems where any model in production can be retrained from its recorded state, where experiments are comparable across teams, and where artifact storage scales without bankrupting the organization.

The senior challenge is making reproducibility effortless rather than aspirational: if it requires heroic effort to reproduce a result, the system has failed.

## Core Frameworks

### What to Track

| Category | Examples | Why It Matters |
|----------|----------|---------------|
| Code version | Git commit, branch, diff | Reproduce the exact training logic |
| Data version | Dataset hash, snapshot ID, data lineage | Reproduce the exact training data |
| Hyperparameters | Learning rate, batch size, architecture config | Reproduce the exact configuration |
| Environment | Python version, library versions, GPU type | Eliminate environment drift |
| Metrics | Accuracy, loss, custom business metrics | Compare experiments objectively |
| Artifacts | Model weights, feature importances, plots | Inspect and deploy without retraining |
| Metadata | Author, timestamp, purpose, hypothesis | Find and understand past experiments |

### Tool Comparison

| Tool | Strengths | Weaknesses | Best For |
|------|-----------|-----------|----------|
| MLflow | Open source, framework-agnostic, simple API | Limited collaboration features, self-hosted | Teams starting MLOps |
| Weights and Biases | Rich visualization, collaboration, hyperparameter sweeps | Commercial, expensive at scale | Research-heavy teams |
| Neptune.ai | Flexible metadata, good for complex pipelines | Smaller community | Custom pipeline tracking |
| Vertex AI Experiments | Integrated with GCP ML platform | Vendor lock-in | GCP-native teams |
| DVC + Git | Data versioning alongside code | Manual integration, less automation | Small teams, cost-sensitive |

### Reproducibility Levels

| Level | What Is Reproducible | Effort to Achieve |
|-------|---------------------|-------------------|
| None | Cannot retrain the same model | Default without tracking |
| Approximate | Same algorithm, similar results | Basic parameter logging |
| Exact | Same code, data, environment, same result | Full tracking with environment pinning |
| Deterministic | Bit-identical output across runs | Seed control, deterministic algorithms |

## In Practice

**Track data version, not just data path.** A path like "s3://data/train.csv" means nothing if the file changes. Hash the dataset or use a data versioning tool. When you record an experiment, the data version must be unambiguous.

**Pin the environment.** A model trained with scikit-learn 1.2 may produce different results with scikit-learn 1.3. Record the full environment: library versions, Python version, CUDA version. Use containers or virtual environments with frozen dependencies.

**Automate tracking, do not rely on discipline.** If tracking requires manual steps, it will be skipped under deadline pressure. Integrate tracking into the training code so that every run is logged automatically. MLflow autolog or W&B callbacks handle this.

**Design for comparison.** The value of tracking is comparing experiments. Use consistent metric names, tag experiments with their purpose, and organize runs by project. A thousand untagged runs are less useful than a hundred well-organized ones.

**Plan artifact retention.** Model weights for every experiment consume storage fast. Define a retention policy: keep all artifacts for 90 days, keep production model artifacts indefinitely, keep only metrics for failed experiments.

## Practical Exercise

Set up experiment tracking for a model you are working on:
1. Choose a tool: MLflow for self-hosted, W&B for managed
2. Configure automatic logging: every training run should record code version, data version, parameters, and metrics
3. Run 3 experiments with different hyperparameters
4. Compare the results using the tool's comparison view
5. Verify reproducibility: pick one past experiment and retrain it from its recorded state
6. Define a retention policy for your project's artifacts

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/AI Overview]]: ML fundamentals
- [[02_Feature_Engineering_and_Feature_Store]]: feature versions are part of experiment tracking
- [[03_Model_Training_and_Evaluation]]: experiments feed into evaluation
- [[06_Model_Governance_and_Cards]]: experiment records feed model cards
- [[04_Data_Quality/00_overview|Data Quality]]: data version tracking overlaps with quality tracking

## Common Pitfalls

- Tracking metrics but not data version: cannot reproduce without knowing which data was used
- Manual tracking: under deadline pressure, engineers skip manual logging steps
- Storing every artifact forever: model weights for thousands of experiments consume terabytes
- Inconsistent metric naming: "accuracy" in one experiment, "acc" in another makes comparison impossible
- Ignoring environment pinning: library version changes silently alter model behavior

## Key Takeaways

- Reproducibility requires tracking code, data, parameters, and environment: missing any one breaks reproduction
- Automate tracking: manual logging fails under pressure
- Data versioning is as important as code versioning: the same code on different data produces different models
- Design for comparison: consistent naming, tagging, and organization make experiments useful
- Artifact retention must be planned: unlimited storage of model weights is not sustainable
- Senior engineers make reproducibility effortless, not aspirational
