---
tags:
  - overview
  - swebok
  - software-maintenance
  - technical-debt
  - legacy-systems
  - refactoring
---

# Software Maintenance — Overview

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 07 — Software Maintenance
> **Purpose:** Modify delivered software to correct faults, improve performance, adapt to changing environments, and extend functionality.

## What Is This?

Software maintenance is the totality of activities performed on software after delivery to the customer. It is often the longest and most expensive phase of the software lifecycle — studies consistently show that 60–80% of total lifecycle cost is spent on maintenance. Yet it receives far less attention in education and literature than development. SWEBOK v4 addresses this imbalance by treating maintenance as a first-class engineering discipline.

Maintenance is not just "fixing bugs." It encompasses four distinct types: corrective (fixing defects), adaptive (adjusting to environmental changes like new OS versions or regulations), perfective (improving performance or maintainability), and preventive (reducing future maintenance effort through refactoring and technical debt reduction). Each type has different triggers, processes, and economic implications.

The central challenge of maintenance is managing complexity and change in systems that were often not designed for long-term evolvability. Legacy systems — large, poorly documented, built with obsolete technologies — are the most extreme case. Technical debt, the accumulated cost of shortcuts and suboptimal design decisions, is the invisible force that makes maintenance progressively harder over time. Effective maintenance requires both technical skills (reading unfamiliar code, making safe changes) and organizational discipline (impact analysis, change management, regression testing).

## The 5 Topic Areas

### 1. [[Maintenance Types]]
- **Corrective maintenance:** Fixing bugs and defects discovered in production. Reactive, often urgent.
- **Adaptive maintenance:** Modifying software to work in a changed environment (new OS, new database, new regulations). Driven by external change.
- **Perfective maintenance:** Enhancing functionality, improving performance, or improving usability. Driven by evolving user needs.
- **Preventive maintenance:** Refactoring, rewriting, re-documenting to reduce future maintenance cost. Proactive investment.
- **Emergency maintenance:** Rapid patches for critical production issues, often bypassing normal change processes.
- Maintenance distribution: typically 60% perfective, 20% adaptive, 15% corrective, 5% preventive
- **Book:** *Software Maintenance: Concepts and Practice, 2nd Ed.* — Grubb & Takang

### 2. [[Maintenance Processes]]
- Change request intake and triage
- Impact analysis: identifying affected components, estimating effort and risk
- Modification implementation: code changes, documentation updates
- Regression testing and release
- Maintenance metrics: mean time to repair (MTTR), change failure rate, maintenance effort ratio
- **Book:** *Working Effectively with Legacy Code* — Michael Feathers

### 3. [[Legacy System Management]]
- Legacy system assessment: technical condition, business value, strategic fit
- Migration strategies: big bang rewrite, incremental strangler fig, parallel running
- Reverse engineering and re-documentation of undocumented systems
- Legacy technology risks: end-of-life platforms, security vulnerabilities, talent scarcity
- When to maintain, migrate, or replace
- **Book:** *Object-Oriented Reengineering Patterns* — Demeyer et al.

### 4. [[Technical Debt]]
- Types: deliberate (conscious shortcuts), accidental (poor design), architectural (structural issues)
- Technical debt quadrant: prudent/reckless × deliberate/inadvertent (Martin Fowler)
- Measuring debt: code smells, complexity metrics, dependency analysis, code age
- Debt management: tracking, prioritization, interest payments (ongoing cost), repayment strategies
- Technical debt is not just code — it includes documentation, tests, infrastructure, and process debt
- **Book:** *Your Code as a Crime Scene* — Adam Tornhill

### 5. [[Impact Analysis]]
- Identifying change impact: static analysis, dependency graphs, call chains
- Traceability: requirements → design → code → tests
- Risk assessment for proposed changes
- Tools: IDE dependency analysis, architecture visualization, code search
- Impact analysis as a prerequisite for safe maintenance
- **Book:** *Refactoring: Improving the Design of Existing Code, 2nd Ed.* — Martin Fowler

## Recommended Books (Priority Order)

| #   | Book                                                                 | Author(s)                      | Pages |     Priority     |
| --- | -------------------------------------------------------------------- | ------------------------------ | :---: | :--------------: |
| 1   | *Software Maintenance: Concepts and Practice, 2nd Ed.* (2001)        | Penny Grubb & Armstrong Takang |  352  |   🔴 Essential   |
| 2   | *Working Effectively with Legacy Code* (2004)                        | Michael Feathers               |  464  |   🔴 Essential   |
| 3   | *Refactoring: Improving the Design of Existing Code, 2nd Ed.* (2018) | Martin Fowler                  |  448  |  🟡 Recommended  |
| 4   | *Your Code as a Crime Scene* (2013)                                  | Adam Tornhill                  |  218  |  🟡 Recommended  |
| 5   | *Object-Oriented Reengineering Patterns* (2002)                      | Serge Demeyer et al.           |  280  | 🟢 Supplementary |

## Relationship to Other KAs

- **[[Software Design Overview|Software Design]]** — Good design makes maintenance easier; poor design makes it painful. Refactoring restores design quality. Design patterns exist partly to support evolvability.
- **[[Software Configuration Management Overview|Software Configuration Management]]** — Change control, versioning, and baselines are essential for managing maintenance changes safely.
- **[[Software Testing Overview|Software Testing]]** — Regression testing is the safety net for maintenance. Without adequate tests, every change is a risk.
- **[[Software Quality Overview|Software Quality]]** — Technical debt is a quality problem. Dependability, maintainability, and portability are quality attributes that directly affect maintenance cost.
- **[[Software Engineering Economics Overview|Software Engineering Economics]]** — Maintenance cost dominates lifecycle economics. Build-vs-replace decisions require economic analysis.
- **[[Software Engineering Management Overview|Software Engineering Management]]** — Maintenance planning, resource allocation, and prioritization are management concerns.

## Related
- [[SWEBOK v4 - Overview]]
- [[03_Software_Design/Software Design Overview|Software Design Overview]]
- [[08_Software_Configuration_Management/Software Configuration Management Overview|SCM Overview]]
