---
title: "CI/CD for Data and ML"
note_type: capability-topic
capability_area: production-engineering
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - ci-cd
  - data-validation
  - mlops
---

# CI/CD for Data and ML

> Applying continuous integration and deployment practices to data pipelines and ML models, with data quality validation as a first-class CI concern.

## Why This Is a Senior Skill

Mid-level engineers run pipelines manually or on a schedule. Senior engineers build CI/CD systems that test data pipelines for correctness, validate data quality before deployment, and promote ML models through environments with automated gates.

The senior challenge is that data and ML systems have failure modes that application CI/CD does not catch: schema changes, data quality regressions, and model performance degradation. A pipeline that runs without errors may still produce wrong results.

## Core Frameworks

### Data Pipeline CI/CD Stages

| Stage | What Is Tested | Failure Consequence |
|-------|---------------|---------------------|
| Lint | Code style, SQL syntax, schema definitions | Code quality issues |
| Unit test | Transformation logic on sample data | Logic errors |
| Integration test | Pipeline end-to-end on test data | Integration failures |
| Data validation | Schema, null rates, distributions, referential integrity | Data quality issues |
| Performance test | Query execution time, resource usage | Performance regressions |
| Deployment | Promote to staging, then production | Deployment failures |

### Data Validation Checks

| Check Type | What It Catches | Example |
|------------|----------------|---------|
| Schema validation | Column added, removed, or type changed | Expected 10 columns, found 11 |
| Null rate | Unexpected nulls in required columns | email_address null rate jumped from 0.1% to 15% |
| Distribution | Value distribution shifted significantly | Age column mean shifted from 35 to 350 |
| Referential integrity | Foreign key violations | Order references non-existent customer |
| Volume | Record count outside expected range | Daily load dropped from 1M to 10K records |
| Freshness | Data is older than expected SLA | Data timestamp is 3 days old |

### ML Model Promotion Workflow

| Environment | Purpose | Gate Criteria |
|-------------|---------|---------------|
| Development | Experiment and training | Model trains successfully |
| Staging | Integration testing | Evaluation metrics meet threshold |
| Shadow | Compare with production model | No significant performance regression |
| Canary | Gradual production rollout | Business metrics stable for 24 hours |
| Production | Full production traffic | Monitoring confirms stability |

## In Practice

**Validate data in CI, not just in production.** A schema change that adds a column may break downstream pipelines. Run schema validation on every code change. A null rate spike in staging is cheaper to fix than in production.

**Test transformations on sample data.** Unit test your SQL and Python transformations on a small dataset with known inputs and expected outputs. A transformation that works on 100 rows likely works on 100 million rows, unless it has a scalability bug.

**Automate model promotion.** A model that meets evaluation criteria in staging should automatically move to shadow testing. Manual promotion creates delays and human error. Define gates as code: if metric X exceeds threshold Y, promote automatically.

**Version data pipelines like application code.** Pipeline code, schema definitions, and configuration should be in version control. Every change goes through code review and CI. A pipeline change that skips CI is a production incident waiting to happen.

**Run performance tests in CI.** A query that takes 5 seconds today may take 50 seconds after a data volume increase. Run performance tests on every change and fail the build if execution time exceeds the threshold. Catch performance regressions before they reach production.

## Practical Exercise

Build a CI/CD pipeline for a data pipeline or ML model:
1. Define CI stages: lint, unit test, integration test, data validation
2. Write 3 data validation checks: schema, null rate, and volume
3. Implement a performance test: measure query execution time and fail if it exceeds threshold
4. Design a promotion workflow: how does a change move from development to production?
5. Automate one gate: define a metric threshold that triggers automatic promotion or rollback
6. Write a runbook for the most common CI failure: what does the engineer check and fix?

## Knowledge Connections

- [[software-engineering-note/06_Software_Engineering_Operations/Software Engineering Operations Overview]]: CI/CD fundamentals
- [[04_Data_Quality/00_overview|Data Quality]]: data validation is quality enforcement
- [[06_ML_Lifecycle_and_MLOps/00_overview|ML Lifecycle and MLOps]]: model promotion is part of MLOps
- [[03_Reliability_and_Fault_Tolerance]]: CI catches reliability issues early
- [[06_Operational_Runbooks_and_On_Call]]: runbooks for CI failures

## Common Pitfalls

- No data validation in CI: schema changes and quality regressions reach production
- Testing only happy paths: CI passes but pipeline fails on edge cases in production
- Manual model promotion: delays and human error in moving models through environments
- Ignoring performance tests: a query that takes 5 seconds in CI takes 50 seconds with production data volume
- Pipeline code not in version control: changes made directly in production with no audit trail

## Key Takeaways

- Data validation belongs in CI: schema changes, null rates, and distributions should be tested before deployment
- Test transformations on sample data: unit tests catch logic errors before they reach production
- Automate model promotion: define gates as code, not as manual checklists
- Version data pipelines like application code: every change goes through code review and CI
- Performance tests in CI catch regressions before they affect production users
- Senior engineers treat data quality validation as a CI concern, not a production monitoring concern
