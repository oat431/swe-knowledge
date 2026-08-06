---
title: "Profiling and Anomaly Detection"
note_type: capability-topic
capability_area: data-quality
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - profiling
  - anomaly-detection
---

# Profiling and Anomaly Detection

> Using statistical techniques to discover the true structure and content of data, detect distribution drift, and identify outliers before they cause downstream failures.

## Why This Is a Senior Skill

A mid-level engineer runs a profiling tool and reads the output. A senior engineer **designs continuous profiling that detects drift over time, distinguishes expected variation from true anomalies, and triggers investigation** before quality degrades below acceptable thresholds.

Data changes over time. Distributions shift, new patterns emerge, and edge cases that were rare become common. The senior engineer builds systems that detect these changes automatically and distinguish signal from noise.

## Core Frameworks

### Profiling Statistics and Their Purpose

| Statistic | What it reveals | Quality dimension |
|---|---|---|
| Null count and percentage | Missing data patterns | Completeness |
| Min/max values | Outliers, invalid ranges | Validity, Accuracy |
| Value length distribution | Format inconsistencies | Validity |
| Frequency distribution | Defaulted values, skew | Reasonability |
| Distinct value count | Cardinality changes | Uniqueness |
| Pattern matching | Format conformance | Validity |
| Cross-column correlation | Embedded dependencies | Consistency |

### Drift Detection Methods

| Method | How it works | Sensitivity | Use case |
|---|---|---|---|
| Statistical tests (KS, chi-squared) | Compare distributions mathematically | High, may over-alert | Numeric columns with stable distributions |
| PSI (Population Stability Index) | Measures distribution shift magnitude | Medium, interpretable | Feature monitoring, model drift |
| Windowed comparison | Compare recent window to baseline | Configurable | Trending detection |
| Z-score on aggregates | Detect outlier aggregate values | Low, robust | Volume, count monitoring |
| ML-based (isolation forest, autoencoders) | Learn normal patterns, flag deviations | High, complex | High-dimensional, complex patterns |

### Anomaly Classification

| Type | Characteristics | Response |
|---|---|---|
| True anomaly | Genuine data quality issue | Investigate and remediate |
| Expected variation | Seasonal, business-cycle driven | Document and adjust baseline |
| Schema change | New column, type change, value range expansion | Update profiling baseline |
| Data pipeline issue | Missing data, duplicates, ordering problems | Fix pipeline, reprocess |
| Source system change | Upstream behavior changed without notice | Coordinate with source team |

## In Practice

**Profile continuously, not just at discovery.** One-time profiling during initial data assessment is valuable but insufficient. Run profiling statistics on every pipeline execution and compare to historical baselines. Drift that accumulates slowly is invisible to point-in-time checks.

**Distinguish drift from anomaly.** A 10% increase in average order value during holiday season is expected variation, not an anomaly. A 10% increase in null customer_id values on a random Tuesday is an anomaly. Build seasonal baselines and business-calendar awareness into your detection.

**PSI is your friend for feature monitoring.** Population Stability Index is interpretable (PSI < 0.1 is stable, 0.1-0.25 is moderate drift, > 0.25 is significant drift) and works well for ML feature monitoring. It catches distribution shifts that mean and variance alone miss.

## Practical Exercise

Design a profiling and anomaly detection system for a customer transactions table:

1. Data: 50M rows, 20 columns, daily partitioned, used for ML features and reporting
2. Known patterns: weekday/weekend volume variation, seasonal spending patterns, occasional bulk imports
3. Requirements: detect drift in key features, alert on anomalies, minimize false positives

Document:
- Which columns you would profile and which statistics matter most
- Your drift detection method for the transaction_amount column
- How you handle expected seasonal variation
- Your alerting thresholds and escalation path

## Knowledge Connections

- [[11_Data_Quality]] : DMBOK profiling concepts and techniques
- [[career-path/09_Data_and_ML_Engineer/04_Data_Quality/01_Quality_Dimensions_and_Metrics]] : dimensions that profiling measures
- [[career-path/09_Data_and_ML_Engineer/04_Data_Quality/04_Data_Observability]] : continuous monitoring of profiling results
- [[career-path/09_Data_and_ML_Engineer/06_ML_Lifecycle_and_MLOps/00_overview]] : feature drift detection for ML

## Key Takeaways

- Profiling must be continuous, not a one-time discovery exercise
- Drift detection requires baselines that account for expected variation (seasonal, cyclical)
- PSI is the most interpretable drift metric for ML feature monitoring
- Anomaly classification prevents alert fatigue: distinguish true issues from expected variation
- Cross-column profiling reveals hidden dependencies that single-column checks miss
- Profiling statistics should feed directly into quality rules and observability dashboards
