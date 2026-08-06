---
title: "Operational Runbooks and On-Call"
note_type: capability-topic
capability_area: production-engineering
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - runbooks
  - on-call
  - incident-response
---

# Operational Runbooks and On-Call

> Designing runbooks and on-call processes that enable engineers to respond to data system incidents quickly and confidently without heroics.

## Why This Is a Senior Skill

Mid-level engineers respond to alerts and improvise solutions. Senior engineers design runbooks that guide any engineer through incident response, establish escalation paths that match severity to expertise, and build systems where on-call is sustainable rather than heroic.

The senior challenge is that data system incidents have unique characteristics: data corruption may not be visible until downstream systems fail, backfill operations are expensive and error-prone, and the blast radius of a data incident can span the entire organization.

## Core Frameworks

### Runbook Structure

| Section | Purpose | Example |
|---------|---------|---------|
| Alert description | What triggered the alert, what it means | "Pipeline X failed: source table schema changed" |
| Impact assessment | What is broken, who is affected | "Dashboard Y is stale, affecting Z team's decisions" |
| Diagnostic steps | How to investigate the root cause | "Check source table schema, compare with expected" |
| Resolution steps | How to fix the issue | "Update pipeline schema, reprocess last 24 hours" |
| Verification | How to confirm the fix worked | "Check dashboard Y shows current data" |
| Escalation | When and how to escalate | "If schema change is breaking, escalate to data platform team" |
| Post-incident | What to document after resolution | "Log root cause, update schema validation in CI" |

### Incident Severity Levels

| Level | Response Time | Escalation | Example |
|-------|--------------|-----------|---------|
| P1: Critical | Immediate, page on-call | Escalate to manager after 30 minutes | Production pipeline down, data corruption |
| P2: High | Within 1 hour, page during business hours | Escalate to team lead after 2 hours | Pipeline delayed, dashboard stale |
| P3: Medium | Within 4 hours, no page | Escalate to team lead next business day | Non-critical pipeline failed, data quality warning |
| P4: Low | Next business day, no page | Handle in regular work | Cosmetic issue, minor data quality deviation |

### On-Call Sustainability Metrics

| Metric | Healthy Target | Warning Sign |
|--------|---------------|--------------|
| Pages per week per engineer | Less than 2 | More than 5: alert fatigue |
| Time to resolution | Less than 1 hour for P1 | More than 4 hours: runbook gap |
| Repeat incidents | Less than 10% of total | More than 30%: root cause not fixed |
| On-call sleep disruption | Less than 1 night per rotation | More than 2 nights: unsustainable |
| Runbook coverage | 100% of alerts have runbooks | Less than 80%: engineers improvising |

## In Practice

**Write runbooks for every alert.** An alert without a runbook is a 3 AM puzzle. Every alert should have a runbook that any on-call engineer can follow, not just the expert who built the system. If an alert does not have a runbook, either write one or remove the alert.

**Design runbooks for the 3 AM context.** At 3 AM, the engineer is tired and stressed. Runbooks should be step-by-step with copy-paste commands, not conceptual explanations. "Run this command, check this output, if X then do Y" is better than "investigate the root cause."

**Automate recovery where possible.** If a pipeline fails due to a transient error, auto-retry. If a node fails, auto-replace. Reserve human intervention for incidents that require judgment. Every automated recovery reduces on-call burden.

**Conduct blameless post-incident reviews.** After every P1 or P2 incident, conduct a review focused on system improvement, not blame. Document what happened, why, and what changes prevent recurrence. Update runbooks and monitoring based on lessons learned.

**Rotate on-call sustainably.** A rotation longer than one week causes burnout. A rotation shorter than one week does not allow context building. One week with a secondary on-call for escalation is a common sustainable pattern. Track sleep disruption and adjust if engineers are regularly losing sleep.

## Practical Exercise

Design an on-call system for a data platform you manage:
1. List the top 5 alerts and write a runbook for each
2. Define severity levels: what is P1, P2, P3, P4?
3. Design the escalation path: who is called when, at what severity?
4. Write a post-incident review template: what sections does it include?
5. Identify one incident that could be auto-recovered: design the automation
6. Plan the rotation: how long, how many engineers, how is secondary on-call handled?

## Knowledge Connections

- [[software-engineering-note/06_Software_Engineering_Operations/Software Engineering Operations Overview]]: incident management
- [[03_Reliability_and_Fault_Tolerance]]: fault tolerance reduces incidents
- [[05_ML_Monitoring_and_Drift]]: monitoring alerts trigger runbooks
- [[05_CI_CD_for_Data_and_ML]]: CI catches issues before they become incidents
- [[04_Cost_Optimization_for_Data_Systems]]: incidents have cost impact

## Common Pitfalls

- Alerts without runbooks: 3 AM puzzle-solving instead of step-by-step recovery
- Runbooks written for experts: only the original author can follow them, on-call rotation fails
- No post-incident reviews: the same incident recurs because root causes are not addressed
- Heroic on-call: engineers lose sleep regularly, burnout and turnover follow
- Blame-focused reviews: engineers hide incidents instead of reporting them for improvement

## Key Takeaways

- Every alert needs a runbook: an alert without a runbook is a 3 AM puzzle
- Runbooks must be step-by-step with copy-paste commands, designed for the 3 AM context
- Automate recovery for known transient failures: reduce human intervention to cases requiring judgment
- Blameless post-incident reviews drive system improvement: focus on what, not who
- On-call sustainability requires metrics: track pages, resolution time, and sleep disruption
- Senior engineers design on-call systems that are sustainable, not heroic
