---
title: "SLO Definition"
note_type: capability-topic
capability_area: service-objectives
career_path: sre-and-platform-engineer
prerequisite:
  - "[[01_SLI_Design]]"
tags:
  - career-path
  - sre
  - platform-engineering
  - SLO
  - service-objectives
---

# SLO Definition

> **One-line definition:** Setting realistic, measurable reliability targets that balance user expectations with engineering cost and business constraints.

## Why This Is a Specialist Skill

A senior software engineer may work within existing SLOs. An SRE or platform engineer **defines SLOs that the organization can commit to**, **negotiates trade-offs with product and business stakeholders**, and **adjusts SLOs as systems and expectations evolve**.

SLO definition is not just picking a number. It is **aligning engineering effort with user value and business risk**.

## SLO vs SLI vs SLA

```mermaid
flowchart LR
    SLI["SLI\n(what we measure)"] --> SLO["SLO\n(what we aim for)"]
    SLO --> SLA["SLA\n(what we promise)"]
    SLA --> CONSEQUENCE["Consequence\n(what happens if we miss)"]
```

| Term | Definition | Example | Who defines it |
|---|---|---|---|
| **SLI** | Service Level Indicator: what you measure | "Percentage of successful requests" | Engineering |
| **SLO** | Service Level Objective: target value for SLI | "99.9% of requests succeed" | Engineering + Product |
| **SLA** | Service Level Agreement: contractual commitment | "If availability < 99.9%, we refund 10%" | Business + Legal |

**Key insight:** SLOs should be **tighter** than SLAs. If your SLA promises 99.9%, your SLO should target 99.95% or higher to provide a safety margin.

## Setting SLO Targets

### Starting points

| Approach | When to use | Example |
|---|---|---|
| **User expectations** | When you know what users tolerate | "Users tolerate 500ms latency; beyond that, they abandon" |
| **Historical performance** | When you have existing data | "Our p99 latency has been 200ms; let's commit to 300ms" |
| **Competitive benchmark** | When competitors set expectations | "Competitor X offers 99.99% availability; we need to match" |
| **Business impact** | When downtime has clear cost | "Each minute of downtime costs $10K; 99.99% is justified" |

### The 100% trap

**Never set an SLO of 100%.** It is mathematically impossible and economically unjustified.

| SLO | Allowed downtime per month (30 days) | Allowed downtime per year |
|---|---|---|
| 99% | 7.2 hours | 3.65 days |
| 99.9% | 43.2 minutes | 8.76 hours |
| 99.95% | 21.6 minutes | 4.38 hours |
| 99.99% | 4.32 minutes | 52.6 minutes |
| 99.999% | 25.9 seconds | 5.26 minutes |

**Each additional nine costs 10x more engineering effort.** Ask: "Is the user experience difference worth 10x the cost?"

## SLO Measurement Windows

SLOs can be measured over different time windows:

| Window | Pros | Cons | Use when |
|---|---|---|---|
| **Rolling (e.g., 30 days)** | Smooths out short-term spikes; always current | Harder to reason about "budget remaining" | Most services; continuous improvement |
| **Calendar (e.g., per month)** | Easy to report and budget | Resets at month boundary; can mask trends | Contractual SLAs; monthly reporting |
| **Per-request** | Immediate feedback | No smoothing; noisy | Real-time alerting; synthetic probes |

**Recommendation:** Use rolling windows for internal SLOs, calendar windows for external SLAs.

## Multi-Tier SLOs

Not all traffic is equal. Define different SLOs for different tiers:

| Tier | Definition | SLO | Rationale |
|---|---|---|---|
| **Critical** | Paid users, core transactions | 99.95% | High business impact |
| **Standard** | Free users, non-critical features | 99.9% | Lower business impact |
| **Background** | Batch jobs, analytics | 99% | Tolerates delays |

## SLO Definition Template

```markdown
## Service: [Service Name]
## SLO: [SLO Name]

**SLI:** [Formula for measuring the indicator]
**Target:** [e.g., 99.9%]
**Measurement window:** [e.g., rolling 30 days]
**Measurement point:** [e.g., server-side access logs]

**Rationale:**
- [Why this target? User expectations, business impact, historical performance]

**Error budget:**
- Allowed failures: [e.g., 0.1% = 43.2 minutes per month]
- Policy: [What happens when budget is low/exhausted]

**Review cadence:**
- [e.g., quarterly review with product and engineering]
```

## SLO Anti-Patterns

| Anti-pattern | Problem | What to do instead |
|---|---|---|
| **100% SLO** | Impossible; zero error budget | Set realistic targets (99.9%, 99.95%) |
| **SLO without SLI** | Can't measure; meaningless | Define SLI first, then SLO |
| **SLO too tight** | Error budget exhausted; feature work blocked | Adjust SLO or improve system reliability |
| **SLO too loose** | Users unhappy before SLO is breached | Tighten SLO or add tiered SLOs |
| **No review cadence** | SLO becomes stale; misaligned with reality | Review SLOs quarterly with stakeholders |

## Practical Exercise

**For a service you own:**

1. **Review your current SLOs:**
   - Do you have SLOs for all critical SLIs?
   - Are they realistic (not 100%)?
   - Do they have clear measurement windows?

2. **Calculate error budgets:**
   - For each SLO, calculate allowed downtime per month.
   - Is this acceptable to your product team?

3. **Define a new SLO:**
   - Pick a critical user journey without an SLO.
   - Use the SLO Definition Template above.
   - Share with your product manager for alignment.

**Bonus:** Review a recent incident. How much error budget did it consume? Was your SLO tight enough to catch it early?

## Knowledge Connections

- [[01_SLI_Design]] : SLIs are the foundation for SLOs
- [[03_Error_Budget_Policy]] : error budgets turn SLOs into decision tools
- [[04_SLA_Management]] : SLOs inform SLA commitments
- [[software-engineering-note/02_Software_Architecture/Microservice/05 Observability/054 SLOs & Error Budgets]] : SLI/SLO foundations
- [[career-path/02_Senior_Software_Engineer/05_Quality_Reliability_Security/02_SRE_Principles]] : SRE principles from Senior SWE path

## Key Takeaways

- SLOs must be realistic, measurable, and aligned with user expectations
- Never set 100% SLOs; each additional nine costs 10x more effort
- SLOs should be tighter than SLAs to provide a safety margin
- Use rolling windows for internal SLOs, calendar windows for external SLAs
- Define tiered SLOs for different traffic types (critical, standard, background)
- Review SLOs quarterly with product and engineering stakeholders
