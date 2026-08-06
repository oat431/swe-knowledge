---
title: "Quality Dimensions and Metrics"
note_type: capability-topic
capability_area: data-quality
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - data-quality
  - metrics
---

# Quality Dimensions and Metrics

> Defining measurable quality targets across completeness, accuracy, timeliness, consistency, validity, and uniqueness so that data fitness for purpose can be quantified and tracked.

## Why This Is a Senior Skill

A mid-level engineer checks whether a column has nulls. A senior engineer **defines which dimensions matter for each consumer, sets measurable thresholds tied to business impact, and negotiates quality SLAs** that balance cost against risk.

Quality without measurement is aspiration. Quality with the wrong measurement is false confidence. The senior engineer chooses dimensions that align with actual business risk and defines metrics that are objective, reproducible, and actionable.

## Core Frameworks

### Core Quality Dimensions (DAMA UK)

| Dimension | Question it answers | Typical metric | Example threshold |
|---|---|---|---|
| Completeness | Are all required values present? | % non-null for required fields | >= 99.5% |
| Accuracy | Does the data reflect reality? | % matching verified source | >= 98% |
| Timeliness | Is data available when needed? | Hours between event and availability | <= 2 hours |
| Consistency | Do multiple representations agree? | % matching across systems | >= 99% |
| Validity | Does data conform to defined format/range? | % passing format/range rules | >= 99.9% |
| Uniqueness | Are there duplicate records? | Duplicate rate by key | <= 0.1% |

### Metric Design Criteria

| Criterion | Description | Anti-pattern |
|---|---|---|
| Measurable | Countable with quantifiable results | Subjective quality scores |
| Business-relevant | Connected to operational or financial impact | Measuring things nobody cares about |
| Acceptable | Compared against defined thresholds | Thresholds that are never reviewed |
| Accountable | Owned by a specific steward or team | Quality metrics with no owner |
| Controllable | Triggers action when out of range | Alerts that are always ignored |
| Trending | Enables measurement of improvement over time | Point-in-time snapshots only |

### Dimension Priority by Use Case

| Use case | Primary dimensions | Secondary | Low priority |
|---|---|---|---|
| Financial reporting | Accuracy, Completeness, Consistency | Timeliness | Uniqueness |
| Real-time fraud detection | Timeliness, Accuracy | Completeness | Validity |
| Customer analytics | Completeness, Uniqueness | Accuracy | Timeliness |
| Regulatory compliance | Accuracy, Completeness, Timeliness | Consistency | Uniqueness |
| ML feature store | Completeness, Timeliness, Consistency | Validity | Uniqueness |

## In Practice

**Not all dimensions matter equally for every consumer.** A real-time pricing engine cares about timeliness and accuracy above all else. A monthly compliance report cares about completeness and accuracy but can tolerate higher latency. Define dimension priorities per consumer, not globally.

**Accuracy is the hardest dimension to measure.** It requires comparison to a verified source of truth, which often does not exist at scale. Use sampling with manual verification for accuracy measurement, and be explicit about confidence intervals. Do not claim 99.9% accuracy based on automated checks that only validate format.

**Timeliness must be measured end-to-end.** The time between a business event occurring and the data being available to the consumer who needs it. Not the pipeline run time, not the ETL duration, but the full event-to-consumption latency.

## Practical Exercise

Define quality dimensions and metrics for an e-commerce orders data set:

1. Consumers: revenue reporting (daily), recommendation engine (real-time), customer service (on-demand)
2. Data: order_id, customer_id, product_id, quantity, price, order_timestamp, shipping_address

For each consumer:
- Rank the six dimensions by importance
- Define a measurable metric and threshold for the top 3 dimensions
- Explain what business impact occurs when each threshold is breached

## Knowledge Connections

- [[11_Data_Quality]] : DMBOK foundation for quality dimensions and frameworks
- [[career-path/09_Data_and_ML_Engineer/04_Data_Quality/02_Profiling_and_Anomaly_Detection]] : measuring current state against dimensions
- [[career-path/09_Data_and_ML_Engineer/04_Data_Quality/05_Quality_Scorecards_and_Reporting]] : reporting dimensions to stakeholders
- [[career-path/07_SRE_and_Platform_Engineer/01_Service_Objectives/02_SLO_Definition]] : SLO thinking applied to data quality

## Key Takeaways

- Quality dimensions must be defined per consumer, not globally: different use cases prioritize different dimensions
- Accuracy measurement requires a verified source of truth and is inherently sampling-based at scale
- Every metric must meet six criteria: measurable, business-relevant, acceptable, accountable, controllable, trending
- Timeliness is end-to-end event-to-consumption latency, not pipeline execution time
- Thresholds must be reviewed regularly: static thresholds become disconnected from business reality
- Dimension prioritization is a business decision informed by technical feasibility, not a technical decision alone
