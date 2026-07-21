---
tags: [design-quality, reviews, audits, metrics, software-design, swebok]
---

# Design Quality Analysis — Reviews, Audits, and Metrics

> *Source: SWEBOK v4 Chapter 03 — Software Design*

## Purpose

A design isn't finished when it's written — it's finished when it's verified. Design quality analysis ensures the design meets its quality requirements before construction begins. Finding a design flaw during design review costs hours; finding it during testing costs days; finding it in production costs weeks (or more).

## Design Reviews

### Types of Reviews

| Type | Formality | Participants | Purpose |
|---|---|---|---|
| **Informal walkthrough** | Low | Designer + 1-2 peers | Quick sanity check, knowledge sharing |
| **Technical review** | Medium | Designer + technical peers | Find defects, evaluate alternatives |
| **Formal inspection (Fagan)** | High | Moderator, designer, reviewers, recorder | Rigorous defect detection against checklist |
| **Scenario-based review** | Medium | Designer + domain experts | Walk through specific use cases |
| **Requirements tracing review** | Medium | Designer + requirements engineer | Verify all requirements are addressed |

### The Inspection Process (Fagan)

1. **Planning** — Select reviewers, schedule, distribute materials
2. **Overview** — Designer presents the design to reviewers (optional)
3. **Preparation** — Reviewers study the design individually against checklists
4. **Inspection meeting** — Moderator leads systematic review; defects found, NOT fixed
5. **Rework** — Designer fixes all identified defects
6. **Follow-up** — Moderator verifies all defects are resolved

### Design Review Checklist

| Category | Questions |
|---|---|
| **Completeness** | Are all requirements addressed? Are all edge cases handled? |
| **Correctness** | Does the design satisfy functional requirements? Are algorithms correct? |
| **Consistency** | Are naming conventions consistent? Are interfaces compatible? |
| **Modularity** | Are responsibilities properly distributed? Is coupling low and cohesion high? |
| **Interfaces** | Are pre/postconditions clear? Are error conditions handled? |
| **Performance** | Are critical paths identified? Are performance targets achievable? |
| **Security** | Are inputs validated? Are sensitive data protected? |
| **Maintainability** | Can a new team member understand the design? |
| **Testability** | Can each component be tested independently? |

## Design Audits

**Definition:** A formal, independent assessment of whether the design conforms to organizational standards, regulatory requirements, and best practices.

**Difference from reviews:** Audits assess compliance; reviews find defects. Audits are typically more formal and may involve external assessors.

**What audits check:**
- Conformance to architectural standards
- Adherence to design guidelines and patterns
- Security and compliance requirements
- Documentation completeness and accuracy

## Design Metrics

### Structural Metrics

| Metric | What It Measures | Target |
|---|---|---|
| **Cyclomatic Complexity** | Number of independent paths through code | < 10 per function |
| **Depth of Inheritance Tree (DIT)** | Maximum inheritance depth | < 6 |
| **Number of Children (NOC)** | Number of immediate subclasses | Low (fewer is better) |
| **Coupling Between Objects (CBO)** | Number of classes a class is coupled to | < 14 |
| **Lack of Cohesion of Methods (LCOM)** | How well methods of a class relate | Low (higher cohesion) |
| **Fan-in** | Number of modules calling a module | High (reuse) |
| **Fan-out** | Number of modules a module calls | Low (focused) |

### Size Metrics

| Metric | What It Measures |
|---|---|
| **Lines of Code (LOC)** | Size; correlates with maintenance effort |
| **Number of Classes** | System size and complexity |
| **Number of Methods per Class** | Class complexity |
| **Function Points** | Functional size; language-independent |

### Quality Metrics

| Metric | What It Measures |
|---|---|
| **Defect Density** | Defects per KLOC; quality indicator |
| **Design Stability** | How much the design changes over time |
| **Technical Debt Ratio** | Effort to fix design issues / development cost |
| **Test Coverage** | Percentage of design elements covered by tests |

## Static Analysis

**Definition:** Automated analysis of design artifacts (models, code) without executing them.

**What static analysis can detect:**
- Violations of design rules (e.g., cyclic dependencies)
- Dead code and unreachable paths
- Potential null pointer dereferences
- Resource leaks
- Security vulnerabilities (e.g., SQL injection patterns)
- Coding standard violations

**Tools:** SonarQube, Checkstyle, PMD, FindBugs, ESLint, ArchUnit (architectural fitness functions)

## Verification vs. Validation

| Aspect | Verification | Validation |
|---|---|---|
| **Question** | "Did we design the system right?" | "Did we design the right system?" |
| **Focus** | Internal correctness, consistency | External fitness for purpose |
| **Method** | Reviews, inspections, static analysis | Prototyping, simulation, user testing |
| **Timing** | During design | Throughout, especially early |

## Essential Concepts

- **Reviews are cheaper than rework** — catching a design defect during review costs a fraction of fixing it in production.
- **Use checklists** — they prevent reviewers from missing common defect categories.
- **Metrics guide improvement** — they don't replace judgment. A "good" metric score doesn't mean "good" design.
- **Static analysis catches the obvious** — freeing reviewers to focus on the subtle.
- **Design quality is a continuous concern** — not a one-time gate.

## Related

- [[Software Design Note Overview]] — All design topics
- [[01_Design_Fundamentals_and_Principles]] — What makes a good design
- [[../05_Software_Testing/Software Testing Overview|Software Testing]] — Testing validates the design
- [[../02_Software_Architecture/09_Evaluation_and_Governance]] — Architecture evaluation (ATAM)
