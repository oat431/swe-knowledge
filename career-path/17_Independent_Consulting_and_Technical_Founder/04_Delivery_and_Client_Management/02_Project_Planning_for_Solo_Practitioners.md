---
title: "Project Planning for Solo Practitioners"
role: Independent Consultant and Technical Founder
capability_area: Delivery and Client Management
topic: Project Planning for Solo Practitioners
status: complete
created: 2026-10-05
updated: 2026-10-05
tags:
  - career-path
  - independent-consulting
  - technical-founder
  - project-planning
  - capacity
  - estimation
---

# Project Planning for Solo Practitioners

> **Core skill:** Planning work realistically for one pair of hands — sequencing, buffers, and capacity limits that survive actual reality rather than an idealized calendar.

## Why This Matters

Solo planning fails differently from team planning. On a team, a wrong estimate is absorbed by collective capacity; there are others to review, to cover a sick day, to carry two workstreams in parallel. On your own, every plan is a plan for one person's finite week, and every assumption about availability is also an assumption about your health, focus, and admin load. A plan that assumes a perfect calendar is not optimistic — it is fiction, and fiction collapses at the first integration problem.

The core discipline is planning at realistic capacity. Independent work is roughly half billable at best: sales, invoicing, learning, support, and admin consume the rest, and none of it pauses for a delivery deadline. Estimates calibrated on your best days become deadlines missed on the merely human ones. Planning at around sixty to seventy percent of nominal capacity is not slack; it is the correction that makes commitments keepable.

The plan is also a communication instrument. Milestones that appear weekly keep the client oriented and provide early warning when something slips. One enormous milestone at the end hides all risk until it is too late to react. Designing the sequence — small checkpoints, visible progress, buffers attached to the risky parts — sets the engagement up to succeed in front of witnesses.

## Planning Assumptions for a Firm of One

| Assumption | Team Version | Solo Version |
|------------|--------------|--------------|
| Parallel workstreams | Several in flight at once | One, or two short alternations at most |
| Availability | Leave and sickness absorbed by the team | Single point of failure; plan around it |
| Estimation calibration | Collective correction over time | Self-calibration drifts toward optimism |
| Review capacity | Peers review incrementally | Self-review, which needs scheduled time |
| Interruptions | Teammates absorb the shock | Client support, sales, and admin are all you |
| Bus factor | Shared knowledge | One; mitigate with documentation as you go |

## The 60 Percent Rule

Plan no more than roughly sixty to seventy percent of nominal working hours as delivery time. The remainder is not waste — it is admin, sales, support, learning, and the buffer that absorbs estimation error. A forty-hour week yields about twenty-four to twenty-eight planned delivery hours, and plans built above that mark borrow from future weeks at a compounding interest rate.

## Estimating With Honest Buffers

| Component | How to Compute It | Example |
|-----------|-------------------|---------|
| Base build hours | From history, not from imagination | 60 hours |
| Integration and unknowns | 20 to 30 percent of base | 15 hours |
| Client dependency time | Waiting usually costs more than working | 10 hours of exposure |
| Admin and communication | Kickoff, updates, demos, invoicing | 8 hours |
| Contingency | 15 to 25 percent on the total | 14 hours |
| Committed estimate | Sum, rounded up | 110 hours |

The gap between the raw estimate and the committed one is not padding to be quietly consumed; it is the difference between a plan that communicates reality and one that performs precision.

## Milestone Design

| Principle | Why It Works | Anti-Pattern |
|-----------|--------------|--------------|
| Weekly or biweekly checkpoints | Progress is visible; drift is caught early | One milestone at the end |
| Each milestone has a client-visible output | The client participates in progress | Internal-only milestones |
| Dependencies named per milestone | Waiting is anticipated, not discovered | "Depends on client feedback" buried in prose |
| Riskiest work early | Unknowns shrink while cheap | Comfortable work first, risk last |
| Buffer attached to risky milestones | Uncertainty lives where it actually is | Uniform padding everywhere |

```mermaid
flowchart LR
    SCOPE["Scope from the statement of work"] --> BREAK["Break into deliverables"]
    BREAK --> EST["Estimate from historical hours"]
    EST --> BUFFER["Add admin and contingency buffer"]
    BUFFER --> PLAN["Milestone plan at sixty percent capacity"]
    PLAN --> TRACK["Track weekly and adjust early"]
```

## Practical Applications

```markdown
## Solo Project Plan
- Deliverables and sequence:
- Milestone table: week, output, client dependency, buffer

### Weekly Rhythm
- [ ] Monday: confirm the week's single priority deliverable
- [ ] Daily: one block of uninterrupted build time, calendar-protected
- [ ] Wednesday: check client dependencies and chase early
- [ ] Friday: update the plan, note slippage, communicate status
- [ ] Friday: adjust next week before it starts

### Capacity Check
- [ ] Total committed hours at or below 60 to 70 percent of capacity
- [ ] Admin, sales, and support hours explicitly reserved
- [ ] Two weeks of visible buffer in the calendar
```

## Common Pitfalls

| Pitfall | Why It Is a Problem | Better Approach |
|---------|---------------------|-----------------|
| **Planning at full capacity** | The first surprise cascades into every later week | Plan 60 to 70 percent; keep the rest for reality |
| **No buffer at all** | Estimates become promises with zero tolerance | Pad the estimate, not the promise |
| **Forgetting admin and sales** | Non-billable work silently consumes delivery time | Reserve recurring blocks for it |
| **One giant milestone** | All risk hides until the end | Weekly or biweekly client-visible checkpoints |
| **Unmanaged client dependencies** | Waiting is discovered, not planned | Name dependencies per milestone and chase early |
| **Estimating from the best day** | Consistent 6-hour days exist only in memory | Use trailing historical averages |
| **Plan changes kept private** | The client experiences drift without warning | Revised plan communicated the week it changes |

## Success Indicators

- Most milestones land on their scheduled week, and misses are small and early
- Client dependencies are chased before they become blockers
- Weeks end with the plan updated, not with a vague sense of slippage
- The workload feels demanding but sustainable across months, not just launch weeks
- Revisions of the plan are communicated the week they occur

## Related Topics

- [[01_Statements_of_Work_and_Commitments]]: the plan exists to deliver the committed scope
- [[04_Managing_Multiple_Clients]]: capacity planning across clients is this discipline applied to a portfolio
- [[06_Quality_Without_a_Safety_Net]]: review time is planned capacity, not leftover time
- [[career-path/12_Technical_Program_Manager/00_overview|Technical Program Manager]]: coordination craft for complex engagements
- [[career-path/13_Project_and_Program_Manager/00_overview|Project and Program Manager]]: scheduling discipline this solo version adapts

## Summary

Solo planning is capacity arithmetic done honestly: one pair of hands, roughly half of working time billable, buffers that live where the risk is, and milestones small enough to see. Estimate from history, commit at about sixty percent of nominal capacity, sequence the riskiest work early, and keep the client oriented with weekly visible progress. The plan is not a prediction — it is the correction that keeps commitments keepable.

