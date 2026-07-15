---
tags:
  - overview
  - swebok
  - software-quality
  - quality-assurance
  - verification-and-validation
  - dependability
---

# Software Quality — Overview

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 12 — Software Quality
> **Purpose:** Ensure that software systems meet their quality requirements through assurance processes, verification, validation, and defect management.

## What Is This?

Software Quality is the degree to which a software system satisfies its specified requirements and meets the needs and expectations of its stakeholders. Quality is not a single attribute but a multidimensional concept: a system can be functionally correct but unreliable, or reliable but unusable, or usable but insecure. SWEBOK v4 organizes quality around established frameworks (ISO 25010, McCall's quality model) that decompose quality into measurable characteristics like reliability, performance, maintainability, security, and usability.

Quality assurance (QA) is the set of activities that ensure the engineering processes are adequate to produce quality software. QA is process-focused — it asks "are we building the system right?" Verification and validation (V&V) are product-focused: verification asks "does the product meet its specifications?" and validation asks "does the product meet the user's actual needs?" These distinctions matter because they imply different activities, different responsible parties, and different points in the lifecycle where they are most effective.

Quality is an economic concern as well as a technical one. Defects found late cost exponentially more to fix than those found early. The cost of quality includes prevention costs (training, process improvement), appraisal costs (reviews, testing), and failure costs (rework, warranty claims, reputation damage). Investing in quality is not overhead — it is risk management with measurable returns.

## The 6 Topic Areas

### 1. [[Quality Fundamentals]]
- Quality models: ISO 25010 (product quality, quality in use), McCall's model, Boehm's model
- Quality characteristics: functionality, reliability, usability, efficiency, maintainability, portability, security
- Quality attributes vs. functional requirements
- Quality culture: embedding quality into engineering practices, not bolting it on
- Standards: ISO 9001, ISO/IEC 25010, CMMI
- **Book:** *Software Quality Engineering: A Practitioner's Approach* — Jeff Tian

### 2. [[Software Quality Assurance (SQA)]]
- SQA planning: defining quality objectives, standards, and processes
- Reviews and inspections: peer reviews, walkthroughs, formal inspections (Fagan)
- Process audits and compliance checking
- SQA in agile: built-in quality, definition of done, sprint reviews
- SQA tools and automation: static analysis, linting, continuous quality gates
- **Book:** *Practical Guide to Software Quality Management, 3rd Ed.* — John Horch

### 3. [[Verification and Validation]]
- **Verification:** Are we building the product right? Reviews, static analysis, testing against specifications
- **Validation:** Are we building the right product? User acceptance testing, prototyping, operational testing
- The V-model: linking requirements → design → code → test levels
- Independent V&V (IV&V): third-party verification for high-assurance systems
- V&V techniques: inspection, demonstration, test, analysis, formal verification
- **Book:** *Software Quality Assurance: From Theory to Implementation* — Daniel Galin

### 4. [[Defect Management]]
- Defect lifecycle: discovery → logging → triage → assignment → fix → verification → closure
- Defect classification: severity (critical/major/minor), priority, type (logic, interface, data, performance)
- Root cause analysis: 5 Whys, fishbone diagrams, fault tree analysis
- Defect prevention: code reviews, static analysis, pair programming, checklists
- Defect metrics: density, detection rate, removal efficiency, escaped defects
- **Book:** *How Google Tests Software* — Whittaker, Arbon & Carollo

### 5. [[Dependability]]
- Dependability attributes: availability, reliability, safety, security, maintainability, resilience
- Failure modes: crash, omission, timing, value, Byzantine
- Fault tolerance: redundancy, diversity, error detection and recovery
- Reliability engineering: MTBF, MTTF, MTTR, availability calculations
- Safety-critical systems: standards (DO-178C, IEC 61508, ISO 26262)
- **Book:** *Software Quality Engineering: A Practitioner's Approach* — Jeff Tian

### 6. [[Quality Economics]]
- Cost of Quality (CoQ): prevention, appraisal, internal failure, external failure costs
- Defect cost amplification: 10× rule (each phase multiplies fix cost by ~10)
- ROI of quality investments: testing tools, training, process improvement
- Technical debt as deferred quality cost
- Quality cost models: Jones's quality productivity model
- **Book:** *The Economics of Software Quality* — Capers Jones et al.

## Recommended Books (Priority Order)

| # | Book | Author(s) | Pages | Priority |
|---|------|-----------|:-----:|:--------:|
| 1 | *Software Quality Engineering: A Practitioner's Approach* (2018) | Jeff Tian | 560 | 🔴 Essential |
| 2 | *Practical Guide to Software Quality Management, 3rd Ed.* (2003) | John Horch | 288 | 🔴 Essential |
| 3 | *Software Quality Assurance: From Theory to Implementation* (2004) | Daniel Galin | 608 | 🟡 Recommended |
| 4 | *How Google Tests Software* (2012) | Whittaker, Arbon & Carollo | 304 | 🟡 Recommended |
| 5 | *The Economics of Software Quality* (2011) | Capers Jones et al. | 558 | 🟢 Supplementary |

## Relationship to Other KAs

- **[[Software Testing Overview|Software Testing]]** — Testing is the primary V&V activity. Test coverage, defect metrics, and test effectiveness are quality measures. Related QA notes exist in `programming-note/QA/`.
- **[[Software Requirements Overview|Software Requirements]]** — Non-functional requirements are quality attributes. Requirements validation ensures we are building the right product.
- **[[Software Security Overview|Software Security]]** — Security is a quality attribute. Security testing and secure coding are quality activities.
- **[[Software Construction Overview|Software Construction]]** — Code quality (complexity, readability, standards compliance) is a construction quality concern. Clean Code practices directly serve quality.
- **[[Software Maintenance Overview|Software Maintenance]]** — Technical debt degrades quality. Maintainability is a quality characteristic. Defect tracking links quality to maintenance.
- **[[Software Engineering Economics Overview|Software Engineering Economics]]** — Quality economics, cost of quality, and defect cost models connect quality to financial decisions.

## Related
- [[SWEBOK v4 - Overview]]
- [[05_Software_Testing/Software Testing Overview|Software Testing Overview]]
- [[13_Software_Security/Software Security Overview|Software Security Overview]]
