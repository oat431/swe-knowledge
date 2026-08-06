---
title: "Remediation and Root Cause"
note_type: capability-topic
capability_area: data-quality
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - remediation
  - root-cause
---

# Remediation and Root Cause

> Systematically fixing data quality issues at their source, implementing preventive controls, and building feedback loops that reduce recurrence over time.

## Why This Is a Senior Skill

A mid-level engineer fixes bad data with an UPDATE statement. A senior engineer **traces the issue to its root cause, implements preventive controls at the source, and designs feedback loops** so that the same class of error cannot recur.

Correction without prevention is a treadmill. Teams that spend their time fixing data instead of preventing bad data from entering the system are in a reactive loop that never ends. The senior engineer breaks this cycle by investing in root-cause remediation and systemic prevention.

## Core Frameworks

### Root Cause Analysis Techniques

| Technique | How it works | Best for |
|---|---|---|
| Five Whys | Ask "why" iteratively until root cause emerges | Simple causal chains |
| Fishbone (Ishikawa) | Categorize causes by people, process, technology, environment | Multi-factor problems |
| Pareto analysis | 80/20 rule: focus on the few causes driving most issues | Prioritizing remediation effort |
| Track and trace | Follow data lineage backward from error to origin | Lineage-documented pipelines |
| Process analysis | Map the business process that creates the data | Data entry and workflow issues |

### Remediation Strategy Matrix

| Strategy | Description | When to use | Risk |
|---|---|---|---|
| Fix at source | Correct data in the originating system | Systemic issues, recurring errors | Requires source system access and cooperation |
| Pipeline correction | Add transformation logic to clean data in-flight | Source fix not feasible, known error pattern | Complexity, may mask root cause |
| Manual correction | Steward reviews and fixes individual records | Low volume, high-value records | Slow, expensive, not scalable |
| Automated correction | Rule-based fixes for known error patterns | High volume, well-defined error types | Must validate correction accuracy |
| Accept and document | Document known quality issues, adjust downstream | Cost of fix exceeds impact | Accumulating technical debt |

### Prevention vs Correction Investment

| Approach | Short-term cost | Long-term cost | Quality trajectory |
|---|---|---|---|
| Correction only | Low (quick fixes) | High (recurring issues) | Flat or declining |
| Prevention only | High (system changes) | Low (fewer issues) | Improving |
| Hybrid (recommended) | Medium (fix current + prevent recurrence) | Medium (sustained investment) | Steadily improving |

## In Practice

**Fix at source whenever possible.** Pipeline-level corrections mask the root cause and create maintenance burden. If customer addresses are consistently wrong because the entry form has poor validation, fix the form. If exchange rates are stale because the feed is unreliable, fix the feed. Source fixes prevent the issue across all consumers, not just one pipeline.

**Build feedback loops.** When you correct data downstream, feed the correction back to the source system. When you discover a pattern of errors, report it to the process owner. Quality improvement requires closing the loop between detection and the people who create the data.

**Accept that some issues are not worth fixing.** A 0.1% error rate in a non-critical field may cost more to prevent than the impact it causes. Document known quality issues, communicate them to consumers, and invest remediation effort where it has the highest business return.

## Practical Exercise

Analyze and remediate a data quality issue:

1. Problem: 15% of customer records have invalid postal codes, causing shipping failures and analytics errors
2. Investigation reveals: the e-commerce checkout form accepts free-text postal codes with no validation
3. Business impact: $200K/year in failed deliveries and manual corrections
4. Current workaround: data team runs a weekly script to standardize postal codes

Document:
- Your root cause analysis using the Five Whys technique
- Your remediation strategy (source fix, pipeline fix, or hybrid)
- The preventive controls you would implement
- How you would measure whether the fix is working over time
- Your feedback loop to the e-commerce team

## Knowledge Connections

- [[11_Data_Quality]] : DMBOK root cause analysis techniques and corrective actions
- [[career-path/09_Data_and_ML_Engineer/04_Data_Quality/02_Profiling_and_Anomaly_Detection]] : profiling reveals issues needing remediation
- [[career-path/09_Data_and_ML_Engineer/04_Data_Quality/03_Quality_Rules_and_Validation]] : rules as preventive controls
- [[career-path/09_Data_and_ML_Engineer/03_Data_Integration_and_Interoperability/06_Data_Lineage_and_Provenance]] : lineage for root-cause tracing

## Key Takeaways

- Correction without prevention is a treadmill: invest in root-cause fixes, not just symptom patches
- Fix at source whenever possible: it prevents the issue across all consumers, not just one pipeline
- Feedback loops close the gap between detection and the people who create the data
- Pareto analysis focuses remediation effort on the few causes driving most issues
- Some quality issues are not worth fixing: document, communicate, and invest where ROI is highest
- Prevention investment has a high short-term cost but dramatically lower long-term cost than correction-only approaches
