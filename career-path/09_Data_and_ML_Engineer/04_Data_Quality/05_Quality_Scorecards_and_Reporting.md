---
title: "Quality Scorecards and Reporting"
note_type: capability-topic
capability_area: data-quality
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - scorecards
  - reporting
---

# Quality Scorecards and Reporting

> Building dashboards and reports that make data quality visible to stakeholders, track SLA conformance over time, and drive accountability through trend analysis.

## Why This Is a Senior Skill

A mid-level engineer runs quality checks and logs the results. A senior engineer **designs scorecards that connect quality metrics to business outcomes, defines reporting cadences for different audiences, and uses trend analysis to drive improvement investment decisions.**

Quality that is not visible is not managed. Scorecards translate technical quality metrics into business-relevant information that executives, data stewards, and consumers can act on. The senior engineer designs reporting that drives decisions, not just dashboards that collect dust.

## Core Frameworks

### Scorecard Audience and Content

| Audience | What they need | Reporting cadence | Format |
|---|---|---|---|
| Executive | Quality trend, business impact, investment ROI | Monthly | High-level dashboard, 3-5 metrics |
| Data steward | Rule pass rates, issue queue, remediation progress | Daily/weekly | Detailed dashboard with drill-down |
| Pipeline engineer | Pipeline health, failure rates, rule performance | Real-time | Operational dashboard with alerts |
| Data consumer | Freshness status, known issues, confidence levels | On-demand | Data catalog integration |

### Scorecard Design Principles

| Principle | Description | Anti-pattern |
|---|---|---|
| Actionable | Every metric has a clear owner and response | Metrics with no one accountable |
| Contextual | Show trends, not just point-in-time values | Static numbers with no history |
| Tiered | Different detail levels for different audiences | One-size-fits-all dashboard |
| Trust-weighted | Show confidence levels alongside data | Presenting uncertain data as fact |
| Connected | Link to lineage, rules, and issue tracking | Isolated numbers with no drill-down |

### SLA Tracking Metrics

| Metric | Definition | Target example |
|---|---|---|
| Data availability | % of time data meets freshness SLA | >= 99.5% |
| Quality pass rate | % of rows passing all quality rules | >= 99% |
| Issue resolution time | Median time from detection to fix | <= 4 hours for critical |
| Rule coverage | % of critical data assets with quality rules | 100% |
| Trend direction | Month-over-month quality improvement | Positive trend |

## In Practice

**Scorecards must drive action.** A dashboard that shows quality metrics but does not connect to remediation workflows is a reporting exercise, not a management tool. Every metric should link to an owner, a threshold, and a response procedure.

**Trend analysis is more valuable than point-in-time scores.** A quality score of 97% means little without context. Is it improving from 94% last month? Declining from 99%? Trend analysis reveals whether your quality program is working or degrading.

**Show confidence levels.** When accuracy is measured by sampling, report the confidence interval. When freshness depends on upstream systems you do not control, show the dependency chain. Consumers deserve to know how much to trust the numbers they see.

## Practical Exercise

Design a quality scorecard for a data warehouse serving three business units:

1. Finance: revenue, cost, and margin data (high compliance requirements)
2. Marketing: customer behavior and campaign data (moderate quality needs)
3. Operations: inventory and logistics data (real-time freshness critical)

Document:
- The metrics you would show to executives vs data stewards vs engineers
- Your SLA definitions for each business unit's critical data
- How you would visualize trends and highlight degradation
- Your escalation path when a metric breaches threshold

## Knowledge Connections

- [[11_Data_Quality]] : DMBOK quality reporting and metrics categories
- [[career-path/09_Data_and_ML_Engineer/04_Data_Quality/01_Quality_Dimensions_and_Metrics]] : dimensions that scorecards report
- [[career-path/09_Data_and_ML_Engineer/04_Data_Quality/04_Data_Observability]] : observability feeds scorecard data
- [[career-path/07_SRE_and_Platform_Engineer/01_Service_Objectives/04_SLA_Management]] : SLA management patterns

## Key Takeaways

- Scorecards must be designed per audience: executives need trends, stewards need detail, engineers need real-time
- Every metric needs an owner, a threshold, and a response procedure to be actionable
- Trend analysis reveals whether quality programs are improving or degrading over time
- SLA tracking connects quality metrics to business commitments and accountability
- Confidence levels must be shown alongside measured values: not all metrics are equally certain
- Scorecards that do not connect to remediation workflows are reporting exercises, not management tools
