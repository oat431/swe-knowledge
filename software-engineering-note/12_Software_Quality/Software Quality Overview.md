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

> **Source:** SWEBOK v4 Chapter 12
> **Purpose:** Ensure that software systems meet their quality requirements through assurance processes, verification, validation, and defect management.

## What Is This?

Software Quality is the degree to which a software product meets established requirements that accurately represent stakeholder needs, wants, and expectations. It encompasses both product quality (the attributes of the delivered software) and process quality (the adequacy of the engineering processes that produce it). Quality is not a single dimension — a system can be functionally correct but unreliable, or reliable but unmaintainable.

The discipline operates through the error → defect → failure chain: a human error injects a defect into a work product; when executed, that defect produces an externally visible failure. Understanding this chain drives quality engineering — preventing errors, detecting defects early, and containing failures. The cost of quality includes conformance costs (prevention + appraisal) and nonconformance costs (internal + external failure). Fixing defects earlier in the lifecycle is exponentially cheaper.

Quality management requires organizational commitment: a Quality Management System (QMS) with defined policies, processes, measurement, and feedback loops. For safety-critical systems, integrity levels demand increasingly rigorous assurance activities, potentially including independent V&V. Agile and DevOps practices contribute through quick feedback loops, CI/CD automation, and continuous review — but the fundamentals remain: verification asks "are we building it right?" and validation asks "are we building the right thing?"

## Knowledge Areas

### Software Quality Fundamentals
- Foundational definitions, culture, and ethics supporting quality
- Economics of quality: cost of conformance vs. nonconformance (CoSQ)
- Standards (ISO 25010, ISO 9001, CMMI), certifications, and integrity levels for safety-critical systems

### Software Quality Management Process
- Quality Management System (QMS): planning, evaluation through measurement, continual improvement
- Process improvement models: PDCA, Six Sigma, Lean, Kaizen, QFD
- Corrective/preventive actions, defect characterization, and root cause analysis

### Software Quality Assurance Process
- SQA Plan (SQAP): defining activities, standards, measures, and reporting structures
- Process assurance (auditing adherence) and product assurance (verifying work products)
- V&V activities: static analysis, dynamic testing, formal methods, and technical reviews/audits

### Software Quality Tools
- Static analysis tools, code review tools, defect tracking systems
- CI/CD pipelines, automated testing frameworks, measurement visualization
- Safety analysis techniques: FMEA, Fault Tree Analysis, assurance cases

## My Notes

*(No additional notes yet — only this overview)*

## What's Missing

- Quality Fundamentals (ISO 25010, Cost of Quality, integrity levels)
- Quality Management Process (QMS, PDCA, Six Sigma, root cause analysis)
- Quality Assurance Process (SQAP, V&V, static analysis, reviews, inspections)
- Quality Tools

## Relationship to Other KAs

- **[[Software Testing Overview|Software Testing]]** — Testing is the primary dynamic V&V activity; test coverage and defect metrics are quality measures.
- **[[Software Requirements Overview|Software Requirements]]** — Non-functional requirements are quality attributes; requirements validation ensures we build the right product.
- **[[Software Security Overview|Software Security]]** — Security is a quality attribute; security testing and secure coding are quality activities.
- **[[Software Construction Overview|Software Construction]]** — Code quality (complexity, readability, standards compliance) is a construction concern.
- **[[Software Maintenance Overview|Software Maintenance]]** — Technical debt degrades quality; maintainability is a quality characteristic.
- **[[Software Engineering Economics Overview|Software Engineering Economics]]** — Cost of quality models quantify the financial impact of quality investments.
