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

> **Source:** SWEBOK v4 Chapter 09
> **Purpose:** Plan, measure, and control software engineering activities to deliver systems efficiently and effectively, integrating project management with measurement management.

## What Is This?

Software Engineering Management encompasses the planning, coordination, measurement, and control of software projects and processes. It argues that management without measurement lacks discipline, and measurement without management lacks purpose. The chapter addresses the unique challenges of managing software projects where intangible, malleable products, evolving requirements, and human-centric dynamics demand approaches distinct from traditional engineering management.

Management in software is uniquely difficult because software is invisible, its complexity is unbounded, and its requirements change constantly. Unlike physical construction, there is no "90% done" that looks like 90% done. This invisibility makes estimation unreliable, progress hard to measure, and communication critical. The best software managers create environments where engineers can do their best work, remove impediments, and make sound decisions under uncertainty.

SWEBOK v4 organizes management by activities (what happens) rather than phases (when it happens), making it applicable regardless of which SDLC is chosen. It covers the full spectrum from project-level initiation through closure, plus organizational measurement programs using ISO/IEC/IEEE 15939.

## Knowledge Areas

### Initiation and Scope Definition
- Establishing project need, scope, and feasibility through requirements determination and negotiation
- Feasibility analysis and defining processes for ongoing requirements review and revision
- Context diagrams and model-based systems engineering for defining system boundaries

### Software Project Planning
- Selecting an appropriate SDLC model (predictive to adaptive) and determining deliverables
- Estimating effort/schedule/cost using multiple approaches that must be reconciled
- Work Breakdown Structure (WBS), resource allocation, risk management, and quality planning

### Software Project Enactment (Execution)
- Implementing plans, managing software acquisition (COTS, custom, open source, SaaS)
- Enacting the measurement process and the ongoing cycle of monitoring, controlling, and reporting
- Dev/Sec/Ops as an integrated team culture focused on continuous incremental delivery

### Review and Evaluation
- Assessing stakeholder satisfaction against requirements and evaluating team performance
- Appraising effectiveness of tools, methods, and processes to drive corrective changes
- Earned value and variance analysis comparing actual vs. planned outcomes

### Closure
- Confirming completion criteria, archiving materials, and secure destruction of sensitive data
- Updating measurement databases and conducting retrospectives for organizational learning
- Feeding lessons learned back into process improvement

### Software Engineering Measurement
- ISO/IEC/IEEE 15939 measurement process: establish commitment → plan → perform → evaluate
- Applying measurement to organizations, projects, processes, and work products
- GQM (Goal-Question-Metric) approach and the RACI model for governance clarity

### Software Engineering Management Tools
- Project planning and tracking tools (estimation, Gantt charts, milestone tracking)
- Risk management tools (risk registers, decision trees, Monte Carlo simulation)
- Communication tools and measurement tools for data collection and analysis

## My Notes

### Peopleware (DeMarco & Lister)
- [[01_Managing_the_Human_Resource]] — Projects fail on people, Spanish vs. English theory, quality, Parkinson's Law
- [[02_The_Office_Environment]] — Furniture police, flow time, E-Factor, workspace design, Coding War Games
- [[03_The_Right_People]] — Hornblower factor, leadership, hiring, turnover, human capital
- [[04_Growing_Productive_Teams]] — Jelled teams, Black Team, teamicide, competition, chemistry
- [[05_Fertile_Soil_and_Fun]] — Self-healing systems, risk, meetings, email, change, organizational learning, fun

## Relationship to Other KAs

- **[[Software Requirements Overview|Software Requirements]]** — Requirements scope is the primary driver of planning and estimation. Scope changes are the biggest source of variance.
- **[[Software Engineering Economics Overview|Software Engineering Economics]]** — Estimation, cost-benefit analysis, and ROI calculations are economic activities embedded in management.
- **[[Software Quality Overview|Software Quality]]** — Quality planning, metrics, and defect management have both quality and management dimensions.
- **[[Software Configuration Management Overview|Software Configuration Management]]** — Change control, status accounting, and auditing provide management visibility during enactment and closure.
- **[[Software Maintenance Overview|Software Maintenance]]** — Maintenance planning, technical debt prioritization, and legacy system decisions are management concerns.
- **[[Software Engineering Models and Methods Overview|Software Engineering Models and Methods]]** — Methods and modeling approaches are selected during process planning.

---

## SWEBOK v4 Coverage Map

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 09 | **Last analyzed:** 2026-07-21 | **Coverage:** ~20%

| # | SWEBOK Topic | Status | Vault File(s) | Notes |
|---|---|---|---|---|
| 1 | Initiation and Scope Definition | ❌ | — | No coverage of project initiation, scope definition, feasibility |
| 2 | Software Project Planning | ❌ | Overview only | Overview references SDLC, WBS, estimation but no dedicated files |
| 3 | Software Project Enactment | ❌ | Overview only | Overview references Dev/Sec/Ops but no dedicated files |
| 4 | Review and Evaluation | ⚠️ | `04_Growing_Productive_Teams`, `05` | Team performance touched; no earned value, no stakeholder assessment |
| 5 | Closure | ❌ | `05` (organizational learning only) | No project closure processes, archiving, formal retrospectives |
| 6 | Software Engineering Measurement | ❌ | Overview only | Overview references ISO 15939, GQM, RACI but no dedicated files |
| 7 | Management Tools | ❌ | — | No coverage of project planning tools, risk registers, Gantt charts |

### Gaps to Fill

| Priority | Gap | SWEBOK Topic | What's Missing |
|----------|-----|-------------|----------------|
| 🔴 Critical | Project Initiation & Scope | Initiation & Scope | Project need, scope, feasibility analysis, context diagrams |
| 🔴 Critical | Estimation Techniques | Project Planning | COCOMO, function points, story points, parametric models |
| 🔴 Critical | WBS | Project Planning | Work Breakdown Structure creation, decomposition strategies |
| 🔴 Critical | Formal Risk Management | Project Planning | Risk registers, risk assessment, mitigation planning, Monte Carlo |
| 🔴 Critical | ISO/IEC/IEEE 15939 | Measurement | Measurement process: commit → plan → perform → evaluate |
| 🔴 Critical | GQM | Measurement | Goal-Question-Metric framework, goal setting, metric selection |
| 🔴 Critical | RACI | Measurement | Responsible, Accountable, Consulted, Informed governance |
| 🟡 High | Earned Value / Variance Analysis | Enactment | Comparing actual vs planned outcomes |
| 🟡 High | Software Acquisition Management | Enactment | COTS, custom-contracted, open source, SaaS management |
| 🟡 High | Monitoring, Controlling & Reporting | Enactment | Ongoing project progress tracking |
| 🟡 High | Project Closure | Closure | Completion criteria, archiving, retrospectives, lessons learned |
| 🟡 Medium | Resource Allocation | Project Planning | Allocating resources across project activities |
| 🟡 Medium | Quality Planning | Project Planning | Managing quality as part of project planning |

> **Note:** The vault's Peopleware content (human factors, team dynamics, culture) is valuable supplementary material but does not address SWEBOK's formal PM structure. This chapter has the largest gap of all 15 KAs.
