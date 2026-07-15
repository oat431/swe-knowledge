---
tags:
  - overview
  - swebok
  - software-engineering-management
  - project-management
  - estimation
  - risk-management
---

# Software Engineering Management — Overview

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 09 — Software Engineering Management
> **Purpose:** Plan, measure, and control software engineering activities to deliver systems on time, within budget, and at required quality.

## What Is This?

Software Engineering Management encompasses the planning, coordination, measurement, and control of software projects and processes. It addresses the managerial and organizational dimensions of building software — not the technical act of coding or designing, but the human and economic systems that make engineering work possible. Poor management can doom a technically excellent project; good management can rescue a technically challenged one.

Management in software is uniquely difficult because software is invisible, its complexity is unbounded, and its requirements change constantly. Unlike physical construction, there is no "90% done" that looks like 90% done — software is either working or it isn't. This invisibility makes estimation unreliable, progress hard to measure, and communication critical. The best software managers are those who create environments where engineers can do their best work, remove impediments, and make sound decisions under uncertainty.

SWEBOK v4's management chapter covers the full spectrum: from project-level planning and estimation to organizational process improvement and people management. It integrates with every other KA because management touches all engineering activities — requirements scope, architectural decisions, testing schedules, release planning, and maintenance prioritization all have management dimensions.

## The 6 Topic Areas

### 1. [[SE Planning]]
- Project initiation: defining scope, objectives, constraints, and success criteria
- Work breakdown structure (WBS): decomposing work into manageable tasks
- Scheduling: task dependencies, critical path, milestones, buffers
- Resource allocation: team composition, skills matching, capacity planning
- Planning in predictive (waterfall) vs. adaptive (agile) contexts
- Rolling wave planning: detail near-term, outline long-term
- **Book:** *Software Engineering Project Management, 3rd Ed.* — Richard Thayer

### 2. [[Estimation]]
- Estimation challenges: the cone of uncertainty, optimism bias, anchoring
- Techniques: expert judgment, analogy-based, parametric (COCOMO, Function Points), planning poker
- Estimation in agile: velocity, story points, throughput-based forecasting
- Estimation accuracy vs. precision: ranges, confidence intervals
- Re-estimation as a project progresses (progressive elaboration)
- **Book:** *Rapid Development* — Steve McConnell

### 3. [[Measurement and Analysis]]
- Goal-Question-Metric (GQM) approach to measurement
- Process metrics: velocity, lead time, cycle time, defect density
- Product metrics: size (LOC, function points), complexity, code coverage
- Project metrics: effort, schedule variance, budget burn rate
- Measurement pitfalls: Goodhart's Law, perverse incentives, data quality
- Dashboards and actionable reporting
- **Book:** *Managing the Software Engineering Process* — Watts Humphrey

### 4. [[Risk Management]]
- Risk identification: technical risks, schedule risks, resource risks, external risks
- Risk analysis: probability × impact, risk exposure, risk categorization
- Risk response strategies: avoid, mitigate, transfer, accept
- Risk monitoring: risk registers, trigger conditions, periodic review
- Technical debt as a form of risk accumulation
- **Book:** *Rapid Development* — Steve McConnell

### 5. [[Control and Tracking]]
- Earned Value Management (EVM): planned value, earned value, actual cost
- Agile tracking: burndown/burnup charts, cumulative flow diagrams, cycle time tracking
- Variance analysis and corrective action
- Change control: scope creep, change request management
- Status reporting: what to report, how often, to whom
- **Book:** *Software Engineering Project Management, 3rd Ed.* — Richard Thayer

### 6. [[People Management]]
- Team formation: Tuckman's model (forming, storming, performing, adjourning)
- Motivation: autonomy, mastery, purpose (Pink), intrinsic vs. extrinsic motivation
- Communication: Brooks's Law, communication overhead, information radiators
- Hiring, onboarding, and knowledge transfer
- Technical leadership and the role of the tech lead / engineering manager
- Psychological safety and team culture
- **Book:** *Peopleware, 3rd Ed.* — DeMarco & Lister

## Recommended Books (Priority Order)

| # | Book | Author(s) | Pages | Priority |
|---|------|-----------|:-----:|:--------:|
| 1 | *Managing the Software Engineering Process* (1997) | Watts Humphrey | 320 | 🔴 Essential |
| 2 | *Software Engineering Project Management, 3rd Ed.* (2009) | Richard Thayer | 592 | 🔴 Essential |
| 3 | *Peopleware, 3rd Ed.* (2013) | Tom DeMarco & Timothy Lister | 272 | 🟡 Recommended |
| 4 | *Rapid Development* (1996) | Steve McConnell | 680 | 🟡 Recommended |
| 5 | *The Mythical Man-Month, Anniversary Ed.* (1995) | Fred Brooks | 336 | 🟢 Supplementary |

## Relationship to Other KAs

- **[[Software Requirements Overview|Software Requirements]]** — Requirements scope is the primary driver of project planning, estimation, and resource allocation. Scope changes are the biggest source of schedule and budget variance.
- **[[Software Engineering Economics Overview|Software Engineering Economics]]** — Estimation, cost-benefit analysis, and ROI calculations are economic activities embedded in management. Budget management requires economic thinking.
- **[[Software Quality Overview|Software Quality]]** — Quality planning, quality metrics, and defect management have both quality and management dimensions.
- **[[Software Configuration Management Overview|Software Configuration Management]]** — Change control, release management, and status accounting are SCM activities that management depends on for visibility.
- **[[Software Maintenance Overview|Software Maintenance]]** — Maintenance planning, technical debt prioritization, and legacy system decisions are management concerns.
- **[[Software Engineering Operations Overview|Software Engineering Operations]]** — Operational metrics (uptime, incident rates) feed into project management dashboards. DevOps culture requires management support.

## Related
- [[SWEBOK v4 - Overview]]
- [[15_Software_Engineering_Economics/Software Engineering Economics Overview|SE Economics Overview]]
- [[11_Software_Engineering_Models_and_Methods/Software Engineering Models and Methods Overview|Models and Methods Overview]]
