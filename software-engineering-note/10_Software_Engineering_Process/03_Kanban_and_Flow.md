---
tags: [kanban, flow, wip, continuous-delivery, methodology, software-methodology]
---

# Kanban and Flow

> *Source: Kanban: Successful Evolutionary Change for Your Technology Business by David J. Anderson (2010)*

## Purpose

Kanban is a Lean-based method for managing knowledge work. Unlike Scrum's timeboxed sprints, Kanban uses continuous flow with explicit work-in-progress (WIP) limits. It's particularly effective for operations, maintenance, and continuous delivery teams.

## Core Practices

### 1. Visualize the Workflow
- Map all states work passes through (e.g., Backlog → Analysis → Development → Review → Done)
- Use a Kanban board with columns for each state
- Every work item is visible on the board
- Makes bottlenecks, queues, and blockers immediately apparent

### 2. Limit Work in Progress (WIP)
- Set explicit limits on how many items can be in each state simultaneously
- WIP limits are constraints that force flow: if a column is full, no new work can enter until something leaves
- Start with current WIP as the limit, then gradually reduce
- Reducing WIP increases throughput (Little's Law)

### 3. Manage Flow
- Track items through the system
- Measure lead time (request to delivery) and cycle time (start to done)
- Optimize for smooth, predictable flow — not maximum utilization
- Use cumulative flow diagrams (CFD) to visualize throughput

### 4. Make Policies Explicit
- Define what "done" means for each state
- Document entry/exit criteria for each column
- Policies are visible to everyone on the team
- Examples: "Code review required before merge", "Tests must pass before deployment"

### 5. Implement Feedback Loops
- Daily standup (15 min) — review the board, identify blockers
- Replenishment meeting — decide what to pull next
- Delivery planning — coordinate releases
- Operations review — monthly review of flow metrics
- Risk review — assess items at risk of delay

### 6. Improve Collaboratively, Evolve Experimentally
- Use models (Theory of Constraints, Lean Thinking) to identify improvements
- Run small experiments to test changes
- Measure impact; keep what works, revert what doesn't
- No big-bang transformations — incremental, evolutionary change

## Little's Law

> **L = λW**

- **L** = Average number of items in the system (WIP)
- **λ** = Throughput (items completed per unit time)
- **W** = Average lead time (time in system)

**Implication:** To reduce lead time, reduce WIP. Adding more work items doesn't speed things up — it slows everything down.

## Kanban vs. Scrum

| Aspect | Kanban | Scrum |
|---|---|---|
| **Cadence** | Continuous flow | Fixed sprints |
| **WIP limits** | Explicit per column | Implicit per sprint |
| **Roles** | No prescribed roles | Product Owner, Scrum Master, Team |
| **Changes** | Can be added anytime | Only at sprint boundaries |
| **Metrics** | Lead time, cycle time, throughput | Velocity, burndown |
| **Planning** | On-demand replenishment | Sprint planning |
| **Best for** | Operations, support, continuous delivery | Product development |
| **Change approach** | Evolutionary | Iterative |

## Cumulative Flow Diagram (CFD)

A stacked area chart showing the number of items in each state over time:
- **Rising bands** = items entering a state faster than leaving (bottleneck forming)
- **Flat bands** = stable flow
- **Gap between bands** = WIP — narrower is better
- **Slope of Done line** = throughput

## When to Use Kanban

- **Continuous delivery** — Deploy multiple times per day
- **Operations/Support** — Unpredictable incoming work
- **Maintenance** — Mix of bug fixes and small features
- **Mixed work** — Combining planned and unplanned work
- **Teams transitioning from Waterfall** — Less disruptive than switching to Scrum
- **Mature teams** — Want flow optimization over process structure

## Related

- [[00_Agile_Methodology]] — Scrum as an alternative with timeboxed sprints
- [[01_Lean_Methodology]] — Lean principles that underpin Kanban
- [[02_Methodologies_Overview]] — Comparison across all methodologies
