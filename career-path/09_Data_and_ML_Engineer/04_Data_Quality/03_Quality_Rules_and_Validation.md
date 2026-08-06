---
title: "Quality Rules and Validation"
note_type: capability-topic
capability_area: data-quality
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - validation
  - data-contracts
---

# Quality Rules and Validation

> Encoding data quality expectations as declarative, versioned, and executable rules that run automatically within pipelines to prevent bad data from propagating.

## Why This Is a Senior Skill

A mid-level engineer adds a `WHERE` clause to filter bad rows. A senior engineer **designs a rule framework that is declarative, version-controlled, owned by business stakeholders, and integrated into pipeline execution** so that quality checks are infrastructure, not afterthoughts.

Quality rules are the executable specification of what good data looks like. When they are ad-hoc, scattered across pipeline code, and undocumented, quality becomes tribal knowledge. The senior engineer makes quality rules a first-class, governed artifact.

## Core Frameworks

### Rule Taxonomy

| Rule type | Example | Dimension | Enforcement point |
|---|---|---|---|
| Schema validation | Column exists, type is INTEGER | Validity | Ingestion |
| Null check | customer_id IS NOT NULL | Completeness | Post-transform |
| Range check | price BETWEEN 0 AND 100000 | Validity | Post-transform |
| Referential integrity | order.customer_id IN customers.id | Consistency | Post-join |
| Cross-record | SUM(line_items) = order.total | Consistency | Post-aggregation |
| Freshness | MAX(updated_at) > NOW() - INTERVAL '2 hours' | Timeliness | Pre-consumption |
| Volume | row_count BETWEEN 10000 AND 50000 | Reasonability | Post-load |
| Custom business | discount_rate < 0.5 OR requires_approval | Accuracy | Post-transform |

### Rule Enforcement Patterns

| Pattern | How it works | Impact on pipeline | Use case |
|---|---|---|---|
| Hard stop (circuit breaker) | Pipeline halts on rule failure | Blocks downstream | Critical compliance rules |
| Warn and continue | Rule failure logged, pipeline proceeds | Downstream aware but not blocked | Advisory checks |
| Quarantine (dead-letter) | Failing rows diverted to separate store | Clean downstream, recoverable failures | High-volume with known error patterns |
| Sampling | Rules run on sample, not full set | Reduced overhead, probabilistic | Large data sets with stable distributions |

### Great Expectations-Style Framework Concepts

| Concept | Purpose | Implementation |
|---|---|---|
| Expectation | Declarative quality assertion | `expect_column_values_to_be_between("price", 0, 100000)` |
| Expectation suite | Collection of expectations for a data asset | Version-controlled YAML or code |
| Validation result | Pass/fail with observed values and context | Structured JSON output |
| Data docs | Human-readable validation reports | Auto-generated HTML from expectations |
| Checkpoint | Scheduled or triggered validation run | Integrated with orchestration |

## In Practice

**Rules should be declarative and version-controlled.** Business stakeholders should be able to read and understand quality rules without reading Python. Use YAML or domain-specific languages for rule definition, and store rules in version control alongside pipeline code so that rule changes are reviewed and auditable.

**Not every rule deserves a hard stop.** A missing optional field should warn, not halt the pipeline. A negative price in a financial report should halt. Classify rules by severity and design enforcement accordingly. Over-aggressive circuit breakers cause pipeline failures that are worse than the data issues they prevent.

**Custom business rules are where the real value lives.** Schema validation catches type errors. Range checks catch obvious outliers. But custom rules like "discount_rate < 0.5 OR requires_approval flag is set" encode business knowledge that prevents expensive downstream mistakes. Invest time in eliciting these rules from domain experts.

## Practical Exercise

Design a quality rule suite for a patient records data set in a healthcare system:

1. Data: patient_id, name, date_of_birth, diagnosis_code, treatment_date, provider_id, insurance_id
2. Consumers: clinical analytics, billing, regulatory reporting
3. Requirements: compliance-grade validation, quarantine for fixable errors, hard stop for critical issues

Document:
- 8-10 quality rules with type, dimension, and enforcement pattern
- Which rules are hard stops vs quarantine vs warnings
- How you handle a rule that changes (e.g., new valid diagnosis codes)
- Your strategy for testing rules before deploying them

## Knowledge Connections

- [[11_Data_Quality]] : DMBOK business rule types and validation approaches
- [[career-path/09_Data_and_ML_Engineer/04_Data_Quality/01_Quality_Dimensions_and_Metrics]] : dimensions that rules enforce
- [[career-path/09_Data_and_ML_Engineer/03_Data_Integration_and_Interoperability/04_API_and_Contract_Design]] : contracts as quality rules at boundaries
- [[career-path/09_Data_and_ML_Engineer/04_Data_Quality/04_Data_Observability]] : monitoring rule pass rates over time

## Key Takeaways

- Quality rules must be declarative, version-controlled, and owned by business stakeholders
- Enforcement severity (hard stop, warn, quarantine) must be chosen deliberately per rule
- Schema validation is necessary but insufficient: custom business rules prevent the expensive failures
- Great Expectations-style frameworks make rules testable, versionable, and reportable
- Rule changes are code changes: they require review, testing, and rollback capability
- Quarantine patterns let pipelines proceed while preserving bad data for investigation and repair
