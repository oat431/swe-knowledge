---
tags:
  - overview
  - swebok
  - software-process
  - methodology
  - software-engineering
---

# Software Engineering Process — Overview

> **Source:** SWEBOK v4 Chapter 10
> **Purpose:** Examine how software engineers organize and execute work through life cycle models, process categories, assessment, and continuous improvement.

## What Is This?

Software Engineering Process covers how software development work is organized, managed, and improved. A process is a set of interrelated activities that transforms inputs into outputs. The choice of process affects everything — quality, predictability, team morale, and ultimately whether the software meets its purpose. This knowledge area provides the structural frameworks for organizing work and the mechanisms for measuring and improving those frameworks.

SWEBOK Chapter 10 spans two major areas: life cycle models (waterfall, spiral, Agile, DevOps, etc.) and process infrastructure (how organizations assess, measure, and improve their processes through CMMI, SPICE, and PDCA). It organizes all processes into four categories: technical processes (building the product), technical management processes (planning and controlling), organizational project-enabling processes (infrastructure and resource management), and agreement processes (acquisition and supply).

A key insight is that no ideal process exists universally — processes must be tailored by selecting appropriate standards, development strategies, stages, and processes for each project. The chapter also emphasizes that process and product assessment must be done jointly using a holistic, empirical approach, and that unrealistic estimations lead to failure.

## Knowledge Areas

### Software Engineering Process Fundamentals
- Defines what a process is (interrelated activities transforming inputs into outputs) and the project context
- Establishes the relationship between processes, projects, and other SWEBOK knowledge areas
- Four process categories: technical, technical management, organizational project-enabling, and agreement

### Development Life Cycle Paradigms
- Predictive (requirements fixed early), iterative (scope early, time/cost adjusted), incremental (successive capability additions)
- Evolutionary (continuous product change over lifetime) and continuous development (frequent releases via automation)
- No single paradigm is best — selection depends on project characteristics and uncertainty levels

### Specific Life Cycle Models
- Waterfall (sequential, document-driven), V-model (verification at each level), spiral (risk-driven, evolutionary)
- Unified Process / RUP / OpenUP (iterative, architecture-centric), rapid prototyping
- Agile (embracing change, face-to-face communication, small incremental deliveries, PDCA-based)

### Management of Software Life Cycle Processes
- Six generic stages: Concept → Development → Production → Utilization → Support → Retirement
- Three management levels: technical execution, project coordination, and executive/organizational strategy
- Life cycle adaptation and tailoring for each project's unique characteristics

### Process Infrastructure, Tools, and Methods
- Notations: natural language, data-flow diagrams, IDEF0, Petri nets, UML activity diagrams, BPMN
- Integrated tools: version control, testing, configuration management, ticket management
- DevOps as principles and practices enabling continuous delivery and stakeholder collaboration

### Software Process Assessment and Improvement
- PDCA (Plan-Do-Check-Act) as the foundational continuous improvement paradigm
- GQM (Goal-Question-Metric) for measurement-based improvement
- Framework-based methods: CMM/CMMI, ISO/IEC 33000 (SPICE), and Agile retrospectives as continuous learning loops

## My Notes

- [[00_Agile_Methodology]]
- [[01_Lean_Methodology]]
- [[02_Methodologies_Overview]]
- [[03_Kanban_and_Flow]]
- [[04_Waterfall_and_V-Model]]

## What's Missing

- Process Fundamentals (process definition, 4 process categories)
- Specific Life Cycle Models (Spiral, RUP/UP, rapid prototyping)
- Process Infrastructure & Tools (BPMN, IDEF0, Petri nets)
- Process Assessment & Improvement (PDCA, CMMI, ISO/IEC 33000/SPICE)

## Relationship to Other KAs

- **[[Software Engineering Management Overview|Software Engineering Management]]** — Management plans and controls; Process defines the work structure that management operates within.
- **[[Software Engineering Models and Methods Overview|Software Engineering Models and Methods]]** — Methods are the tools; processes are the frameworks that organize their use.
- **[[Software Quality Overview|Software Quality]]** — Quality processes (reviews, audits, testing) are embedded in the SE process. Process quality directly affects product quality.
- **[[Software Configuration Management Overview|Software Configuration Management]]** — SCM is a foundational supporting process within every life cycle model.
- **[[Software Engineering Operations Overview|Software Engineering Operations]]** — DevOps is both a process model and an operational practice bridging development and operations.
- **[[Software Engineering Professional Practice Overview|Software Engineering Professional Practice]]** — Professional discipline is expressed through process adherence.
