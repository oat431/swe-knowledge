---
tags:
  - overview
  - swebok
  - software-testing
  - quality-assurance
  - test-driven-development
---

# Software Testing — Overview

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 05 — Software Testing
> **Purpose:** Systematically evaluate software to find defects, verify behavior, and build confidence in system quality.

## What Is This?

Software testing is the process of executing a system or its components to evaluate whether it satisfies specified requirements and to identify defects. Testing is the primary mechanism through which we discover what is actually wrong with software — it reveals the gap between what we intended to build and what we actually built. No amount of careful design or elegant code eliminates the need for testing; complexity and human error guarantee that defects will exist.

Testing is far more than "running the program and seeing what happens." It is a disciplined engineering activity with its own techniques, tools, processes, and body of knowledge. Effective testing requires strategic thinking: which tests to write, how to achieve maximum coverage with minimum effort, when to stop testing, and how to prioritize what gets tested first.

SWEBOK v4 expands the testing chapter to address modern challenges: testing AI/ML systems (non-deterministic outputs, data-dependent behavior), continuous testing in CI/CD pipelines, and the relationship between TDD (a construction practice) and the broader test discipline. Testing is everyone's job, but it requires specialized knowledge to do well.

## The 6 Topic Areas

### 1. [[Test Levels]]
- **Unit Testing:** Testing individual functions, methods, or classes in isolation. Mocks, stubs, and test doubles. Fast feedback loop for developers.
- **Integration Testing:** Verifying interactions between components, modules, or services. Contract testing, API testing, database integration.
- **System Testing:** End-to-end testing of the complete system against requirements. Environment configuration, test data management.
- **Acceptance Testing:** Validation by stakeholders that the system meets business needs. UAT, alpha/beta testing, operational readiness testing.
- Test pyramid strategy: many unit tests, fewer integration tests, minimal end-to-end tests
- **Book:** *Software Testing: A Craftsman's Approach, 4th Ed.* — Paul Jorgensen

### 2. [[Test Techniques]]
- **Black-box techniques:** Equivalence partitioning, boundary value analysis, decision tables, state transition testing, use case testing
- **White-box techniques:** Statement coverage, branch coverage, path coverage, data flow testing
- **Experience-based techniques:** Error guessing, exploratory testing, checklist-based testing
- Technique selection criteria: risk level, defect type, available time
- **Book:** *The Art of Software Testing, 3rd Ed.* — Glenford Myers et al.

### 3. [[Test-Driven Development]]
- TDD as a design and testing discipline: Red-Green-Refactor
- Benefits: executable specifications, regression safety, emergent design
- BDD (Behavior-Driven Development): Given-When-Then, Cucumber, SpecFlow
- TDD vs. test-after: quality and design impact differences
- Relationship to [[04_Software_Construction/Software Construction Overview|Software Construction]]
- **Book:** *Lessons Learned in Software Testing* — Kaner, Bach & Pettichord

### 4. [[AI/ML System Testing]]
- Non-deterministic outputs: statistical testing, confidence intervals, tolerance thresholds
- Data quality testing: training data bias, distribution drift, data leakage
- Model evaluation: precision, recall, F1, AUC, confusion matrices
- Adversarial testing and robustness evaluation
- Continuous monitoring and A/B testing in production
- **Book:** *Explore It!* — Elisabeth Hendrickson

### 5. [[Test Measures]]
- Coverage metrics: statement, branch, requirement, path coverage
- Defect metrics: defect density, defect detection rate, defect removal efficiency
- Test progress: test cases executed, passed, failed, blocked
- Cost of quality: cost of finding defects vs. cost of not finding them
- Test ROI and risk-based test prioritization
- **Book:** *Software Testing: A Craftsman's Approach, 4th Ed.* — Paul Jorgensen

### 6. [[Test Process]]
- Test planning: scope, approach, resources, schedule, risks
- Test design: translating requirements and risks into test cases
- Test execution and environment management
- Test closure: reporting, lessons learned, archiving test assets
- Continuous testing in agile and DevOps contexts
- **Book:** *How Google Tests Software* — Whittaker, Arbon & Carollo

## Recommended Books (Priority Order)

| # | Book | Author(s) | Pages | Priority |
|---|------|-----------|:-----:|:--------:|
| 1 | *Software Testing: A Craftsman's Approach, 4th Ed.* (2014) | Paul Jorgensen | 508 | 🔴 Essential |
| 2 | *Lessons Learned in Software Testing* (2001) | Kaner, Bach & Pettichord | 320 | 🔴 Essential |
| 3 | *How Google Tests Software* (2012) | Whittaker, Arbon & Carollo | 304 | 🟡 Recommended |
| 4 | *Explore It!* (2013) | Elisabeth Hendrickson | 186 | 🟡 Recommended |
| 5 | *The Art of Software Testing, 3rd Ed.* (2011) | Glenford Myers et al. | 256 | 🟢 Supplementary |

## Relationship to Other KAs

- **[[Software Requirements Overview|Software Requirements]]** — Requirements are the basis for test cases and acceptance criteria. Every requirement should be traceable to at least one test.
- **[[Software Construction Overview|Software Construction]]** — Unit testing and TDD are construction practices. Developers are the first line of testing.
- **[[Software Quality Overview|Software Quality]]** — Testing is a core V&V activity within the quality framework. Defect management, reliability measurement, and quality metrics depend on testing data.
- **[[Software Security Overview|Software Security]]** — Security testing (penetration testing, fuzzing, vulnerability scanning) is a specialized testing discipline.
- **[[Software Engineering Operations Overview|Software Engineering Operations]]** — Continuous testing in CI/CD pipelines, canary deployments, and production monitoring blur the line between testing and operations.
- **[[Software Configuration Management Overview|Software Configuration Management]]** — Test environments, test data, and test scripts are configuration items that must be versioned and managed.

## Related
- [[SWEBOK v4 - Overview]]
- [[12_Software_Quality/Software Quality Overview|Software Quality Overview]]
- [[13_Software_Security/Software Security Overview|Software Security Overview]]
