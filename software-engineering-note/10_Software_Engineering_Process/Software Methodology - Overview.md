---
tags:
  - overview
  - swebok
  - software-process
  - methodology
  - software-engineering
---

# Software Engineering Process — Overview

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 10 — Software Engineering Process
> **Purpose:** How engineers organize work through life cycle models, process categories, process assessment, and continuous improvement.

## What Is This?

Software Engineering Process covers how software development work is organized, managed, and improved. A process is a set of activities, methods, practices, and transformations that people use to develop and maintain software and its associated products. The choice of process affects everything — quality, predictability, team morale, and ultimately whether the software meets its purpose.

SWEBOK Chapter 10 spans two major areas: **life cycle models** (the structural frameworks for organizing work — waterfall, spiral, Agile, etc.) and **process infrastructure** (how organizations assess, measure, and improve their processes through frameworks like CMMI and SPICE). Most engineers interact with life cycle models daily; process infrastructure is what engineering managers and organizational leaders use to level up the entire organization.

The vault's notes currently cover the life cycle models portion well — Agile (Scrum, XP), Lean, Kanban, Waterfall, and V-Model are all documented with practical detail. The process infrastructure topics (CMMI, SPICE, PDCA, process measurement) are not yet covered.

## The 6 Topic Areas

### 1. [[01_Life_Cycle_Models]]

- **Waterfall** — Sequential phases: requirements → design → implementation → testing → deployment
- **V-Model** — Verification at each level; requirements ↔ acceptance testing, design ↔ integration testing
- **Incremental** — Deliver the system in a series of increasing capability increments
- **Evolutionary/Prototyping** — Build prototypes to refine requirements, then evolve into production
- **Spiral** — Risk-driven; each cycle includes objectives, risk analysis, development, planning
- **Agile** — Iterative, incremental, adaptive. Scrum (sprints, roles, artifacts), XP (TDD, pair programming, CI), Lean (eliminate waste, amplify learning)
- **DevOps** — Integration of development and operations; continuous delivery, IaC, SRE
- **Book:** *Clean Agile: Back to Basics* (2019) — Robert C. Martin

### 2. [[02_Process_Categories]]

- **Primary processes:** Acquisition, supply, development, operation, maintenance
- **Supporting processes:** Documentation, configuration management, quality assurance, verification, validation, review, audit, problem resolution
- **Organizational processes:** Management, infrastructure, improvement, training
- **ISO/IEC/IEEE 12207** — International standard defining software life cycle processes
- **Book:** *The Rational Unified Process: An Introduction, 3rd Ed.* (2003) — Philippe Kruchten

### 3. [[03_Process_Assessment]]

- **CMMI (Capability Maturity Model Integration)** — 5 maturity levels: Initial → Managed → Defined → Quantitatively Managed → Optimizing
- **CMMI Practice Areas:** Requirements Development, Technical Solution, Product Integration, Verification, Validation, Organizational Process Focus, etc.
- **SPICE / ISO/IEC 15504** — International standard for process assessment
- **ISO/IEC 33000 series** — Updated process assessment standard replacing 15504
- Process assessment as a tool for organizational improvement, not just compliance
- **Book:** *CMMI: Guidelines for Process Integration and Product Improvement, 3rd Ed.* (2020) — Chrissis, Konrad & Shrum

### 4. [[04_Process_Measurement]]

- What to measure: effort, defects, productivity, cycle time, customer satisfaction
- GQM (Goal-Question-Metric) approach for defining meaningful measures
- Process performance baselines and models
- Statistical process control for software
- DORA metrics: deployment frequency, lead time, change failure rate, MTTR
- **Book:** *Accelerate* (2018) — Nicole Forsgren, Jez Humble & Gene Kim

### 5. [[05_Continuous_Process_Improvement]]

- **PDCA Cycle** (Plan-Do-Check-Act) — Deming's iterative improvement framework
- **IDEAL Model** (SEI) — Initiating, Diagnosing, Establishing, Acting, Learning
- Root cause analysis for process defects
- Retrospectives as the primary Agile improvement mechanism
- Process improvement vs. process innovation
- **Book:** *CMMI: Guidelines for Process Integration and Product Improvement, 3rd Ed.* (2020) — Chrissis et al.

### 6. [[06_Agile_and_Lean_Processes]]

- Scrum: roles (PO, SM, Dev Team), events (Sprint, Planning, Review, Retro, Daily), artifacts (Product Backlog, Sprint Backlog, Increment)
- XP: TDD, pair programming, continuous integration, collective ownership, sustainable pace
- Lean: 7 principles (eliminate waste, amplify learning, decide late, deliver fast, empower team, build integrity in, see the whole)
- Kanban: visualize workflow, limit WIP, manage flow, make policies explicit, implement feedback loops, improve collaboratively
- SAFe and scaling frameworks
- **Book:** *Clean Agile: Back to Basics* (2019) — Robert C. Martin
- **Book:** *Lean Software Development* (2003) — Mary & Tom Poppendieck
- **Book:** *Kanban: Successful Evolutionary Change for Your Technology Business* (2010) — David J. Anderson

---

## Recommended Books (Priority Order)

| #   | Book                                         | Author(s)                   | Pages |     Priority     |
| --- | -------------------------------------------- | --------------------------- | :---: | :--------------: |
| 1   | Clean Agile: Back to Basics (2019)           | Robert C. Martin            |  240  |     🔴 Core      |
| 2   | Lean Software Development (2003)             | Mary & Tom Poppendieck      |  233  |     🔴 Core      |
| 3   | Kanban (2010)                                | David J. Anderson           |  278  |     🔴 Core      |
| 4   | CMMI, 3rd Ed. (2020)                         | Chrissis, Konrad & Shrum    |  688  | 🟡 Supplementary |
| 5   | Accelerate (2018)                            | Forsgren, Humble & Gene Kim |  288  | 🟡 Supplementary |
| 6   | The Rational Unified Process, 3rd Ed. (2003) | Philippe Kruchten           |  352  |   🟢 Deep Dive   |

---

## Existing Vault Coverage

| Subtopic | Source | Files | Status |
|----------|--------|:-----:|:------:|
| Agile (Scrum, XP) | *Clean Agile* — Robert C. Martin | 1 (overview) | ✅ |
| Lean | *Lean Software Development* — Poppendieck | 1 | ✅ |
| Kanban | *Kanban* — David Anderson | 1 | ✅ |
| Waterfall & V-Model | Synthesized | 1 | ✅ |
| Methodologies Comparison | Synthesized | 1 | ✅ |
| CMMI | — | 0 | ❌ Missing |
| SPICE / ISO 15504 | — | 0 | ❌ Missing |
| PDCA / Process Improvement | — | 0 | ❌ Missing |
| Process Measurement | — | 0 | ❌ Missing |

**Total: 6 files** — Life cycle models well covered; process infrastructure missing.

---

## Relationship to Other KAs

- [[09_Software_Engineering_Management]] — Management plans and controls; Process defines the work structure
- [[14_Software_Engineering_Professional_Practice]] — Professional discipline is expressed through process adherence
- [[12_Software_Quality]] — Quality processes (reviews, audits, testing) are embedded in the SE process
- [[08_Software_Configuration_Management]] — SCM is a supporting process within every life cycle model
- [[06_Software_Engineering_Operations]] — DevOps is both a process model and an operational practice
- [[11_Software_Engineering_Models_and_Methods]] — Methods are the tools; processes are the frameworks that organize their use

## Related

- [[SWEBOK v4 - Overview]]
- [[Software Engineering Note Content]]
- [[02_Methodologies_Overview]]
