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

> **Source:** SWEBOK v4 Chapter 07
> **Purpose:** Cost-effectively support and evolve operational software throughout its post-delivery lifecycle.

## What Is This?

Software maintenance is the totality of activities required to keep software operational, relevant, and aligned with changing business needs after initial delivery. It is the most expensive phase of the software lifecycle — over 80% of total lifecycle cost is consumed by post-delivery evolution, not initial development. Despite this, maintenance receives far less attention in education and literature than development.

Maintenance is not merely "fixing bugs." It encompasses six standardized categories: corrective, preventive, adaptive, additive, perfective, and emergency. The majority of maintenance effort goes toward enhancements (adaptive and additive changes), not fault correction. Lehman's Laws of Software Evolution describe how real-world software must continually adapt or become progressively less satisfactory.

The central challenge is managing complexity and change in systems where limited understanding of existing code is the maintainer's primary obstacle. Effective maintenance requires program comprehension skills, disciplined impact analysis, regression testing, and organizational processes that support controlled change. Modern practices like CI/CD, DevOps, and continuous refactoring are collapsing the traditional boundary between development and maintenance.

## Knowledge Areas

### Software Maintenance Fundamentals
- Definitions, the nature and need for maintenance, and Lehman's eight laws of software evolution
- Six standardized categories: corrective, preventive, adaptive, additive, perfective, and emergency maintenance
- Maintenance as an evolutionary, continuous development activity — not a secondary concern

### Key Issues in Software Maintenance
- Limited understanding of code written by others as the primary challenge consuming maintenance effort
- Impact analysis, regression testing, maintainability as a quality attribute, and technical debt economics
- Staffing decisions (single team vs. separate maintenance team), outsourcing/offshoring, and SLA/SLO management

### Software Maintenance Processes
- ISO/IEC/IEEE 14764 process areas: prepare, perform, logistics support, and manage results
- Planning at four levels: business/organizational, maintenance transition, release/version, and individual modification request
- MR/PR acceptance/rejection workflows, help-desk operations, and program understanding activities

### Software Maintenance Techniques
- Program comprehension strategies and software visualization for understanding complex systems
- Reengineering (active restructuring) and refactoring (behavior-preserving internal reorganization) to reduce technical debt
- Reverse engineering for design recovery; CI/CD pipelines collapsing dev-maintenance boundaries

### Software Maintenance Tools
- Configuration management, code review tools, and software quality assessment platforms
- Static/dynamic analyzers, program slicers, cross-referencers, and dependency analyzers
- Reverse engineering and visualization tools for building mental models of existing systems

## My Notes

*(No additional notes yet)*

## What's Missing

- Maintenance Fundamentals (Lehman's Laws of Evolution, 6 maintenance categories)
- Key Issues (impact analysis, technical debt economics, maintainability, staffing)
- Maintenance Processes (ISO/IEC/IEEE 14764, program understanding, transition)
- Maintenance Techniques (reengineering, refactoring, reverse engineering, visualization)
- Maintenance Tools

## Relationship to Other KAs

- **[[Software Design Overview|Software Design]]** — Good design makes maintenance easier; poor design creates technical debt. Refactoring restores design quality.
- **[[Software Configuration Management Overview|Software Configuration Management]]** — SCM provides the traceability and change control infrastructure essential for controlled maintenance changes.
- **[[Software Testing Overview|Software Testing]]** — Regression testing is the safety net for maintenance. Without adequate tests, every change is a risk.
- **[[Software Quality Overview|Software Quality]]** — Maintainability is a quality characteristic; technical debt is a quality problem measured through sub-characteristics.
- **[[Software Engineering Economics Overview|Software Engineering Economics]]** — Maintenance cost dominates lifecycle economics. Build-vs-replace decisions require economic analysis.
- **[[Software Engineering Management Overview|Software Engineering Management]]** — Maintenance planning, cost estimation, staffing, and organizational design are management concerns.
