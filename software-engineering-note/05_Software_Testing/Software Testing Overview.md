---
tags:
  - overview
  - swebok
  - software-testing
  - quality-assurance
  - test-driven-development
---

# Software Testing — Overview

> **Source:** SWEBOK v4 Chapter 05
> **Purpose:** Dynamically validate that a system under test provides expected behaviors on a finite set of suitably selected test cases.

## What Is This?

Software testing is the process of executing a system (the SUT — system under test) to evaluate whether it satisfies specified requirements and to identify defects. It reveals the gap between what we intended to build and what we actually built. Testing is far more than running the program — it is a disciplined engineering activity requiring strategic decisions about which tests to write, how to achieve maximum coverage with minimum effort, and when to stop testing.

SWEBOK v4 makes this the largest chapter (35 pages), reflecting testing's breadth. It distinguishes between faults (causes in the code) and failures (observed effects during execution), and establishes that exhaustive testing is impossible — even simple programs have near-infinite execution domains. The oracle problem (determining expected outcomes for comparison) remains one of testing's fundamental challenges.

The chapter covers test levels (unit → integration → system → acceptance), techniques (black-box, white-box, experience-based, mutation, usage-based), measures (coverage, fault density, reliability growth), processes (organizational → management → dynamic), and modern concerns: testing AI/ML systems, shift-left practices (TDD, ATDD, DevOps), fuzz testing for security, and metamorphic testing for non-deterministic systems.

## Knowledge Areas

### Software Testing Fundamentals
- Core concepts: SUT, faults vs. failures, test cases and test suites, dynamic vs. static validation
- Key issues: test selection criteria, adequacy criteria, the oracle problem, testability
- Dijkstra's aphorism: testing can show the presence of bugs but never their absence

### Test Levels
- Two orthogonal dimensions: *target* (unit, integration, system, acceptance) and *objectives* (conformance, regression, performance, security, usability, etc.)
- Each level-objective pair drives how test suites are composed and when testing is "enough"
- Regression testing: selective retesting after modifications to verify no unintended side effects

### Test Techniques
- Specification-based (black-box): equivalence partitioning, boundary value analysis, decision tables, state transition, combinatorial testing
- Structure-based (white-box): statement/branch/path coverage, MC/DC, data flow (all-DU-paths)
- Experience-based: error guessing, exploratory testing; fault-based: mutation testing; usage-based: operational profiles

### Test-Related Measures
- Measures evaluating the SUT: fault density, reliability growth models, deployment frequency, MTTR
- Measures evaluating the tests: coverage metrics, mutation score, fault injection effectiveness
- Measurement is fundamental to quality analysis and test optimization

### Test Process
- Multi-layered: organizational (policies, strategies) → management (planning, monitoring, control) → dynamic (design, implementation, execution, incident reporting)
- Practical considerations: team attitudes, documentation, staffing, test reusability, controlled experiments
- Shift-left testing: moving testing to early stages via Agile, TDD, ATDD, and DevOps CI

### Testing of Emerging Technologies
- AI/ML/DL systems: non-deterministic outputs, data-dependent behavior, metamorphic testing
- Blockchain, cloud, concurrent/distributed applications present unique testing challenges
- Emerging technologies (ML, simulation/HIL, crowdsourcing) also used to improve testing itself

### Testing Tools
- Test harnesses, generators, capture/replay, oracle/comparators, coverage analyzers
- Regression tools, security testing tools (fuzz, penetration), load/stress testing
- Defect tracking, cross-browser, mobile, API, and web testing tools

## My Notes

### Testing: A Craftsman's Approach (Jorgensen)
- [[01_Testing_Fundamentals]] — Definitions, test cases, discrete math, graph theory for testers
- [[02_Boundary_and_Equivalence]] — Boundary value analysis, equivalence class testing
- [[03_Decision_Table_and_Path]] — Decision tables, path testing, DD-paths, coverage metrics
- [[04_Data_Flow_and_Retrospective]] — Define/use testing, slice-based testing, spec vs code pendulum
- [[05_Integration_and_System]] — Integration strategies, system testing, use cases, risk-based testing
- [[06_Model_Based_and_Lifecycle]] — Model-based testing, life cycle-based testing
- [[07_OO_and_Complexity]] — OO testing, cyclomatic complexity, Halstead metrics
- [[08_Emerging_Topics]] — Feature-based, TDD, all pairs, mutation testing, reviews

### QA Practice
- [[QA/|QA]]

## Relationship to Other KAs

- **[[Software Requirements Overview|Software Requirements]]** — Requirements are the basis for test cases and acceptance criteria; traceability links requirements to test coverage
- **[[Software Construction Overview|Software Construction]]** — Unit testing and TDD are construction practices; developers are the first testers
- **[[Software Design Note Overview|Software Design]]** — Design for testability; interface specifications feed integration testing
- **[[Software Engineering Operations Overview|Software Engineering Operations]]** — Continuous testing in CI/CD pipelines and production monitoring blur the boundary between testing and operations

---

## SWEBOK v4 Coverage Map

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 05 | **Last analyzed:** 2026-07-21 | **Coverage:** ~80% (updated)

| # | SWEBOK Topic | Status | Vault File(s) | Notes |
|---|---|---|---|---|
| 1 | Testing Fundamentals | ✅ | `01_Testing_Fundamentals.md` (20 KB) | SUT, faults vs failures, oracle problem |
| 2 | Test Levels | ✅ | `05_Integration_and_System.md` (21 KB) | Integration strategies, system testing, risk-based |
| 3 | Test Techniques | ✅ | `02`, `03`, `04`, `07`, `08` | Boundary, equivalence, decision tables, path, data flow, OO, mutation |
| 4 | Test-Related Measures | ⚠️ | Scattered across 7 files | Coverage metrics mentioned but no consolidated treatment |
| 5 | Test Process | ⚠️ | `06` | SWEBOK's 3-layer process model not deeply covered |
| 6 | Testing in Dev Processes | ✅ | `06`, `08`, `10_Domain_Specific_Testing.md` (30 KB) | 10 domains: automotive, IoT, healthcare, mobile, avionics, finance, gaming, embedded, cloud, blockchain |
| 7 | Emerging Technologies | ✅ | `08`, `11_AI_ML_Testing_and_Emerging.md` (34 KB) | AI/ML testing, data quality, model validation, adversarial testing, chaos engineering |
| 8 | Testing Tools | ✅ | `09_Testing_Tools_and_Standards.md` (35 KB) | 15+ tool categories, ISO 29119, TMMi, CMMI, ODC |

### Gaps to Fill

| Priority | Gap | SWEBOK Topic | What's Missing |
|----------|-----|-------------|----------------|
| 🔴 High | Testing Tools (catalog) | Testing Tools | 15+ tool categories: harnesses, generators, coverage analyzers, etc. |
| 🟡 Medium | Test Process (3-layer model) | Test Process | Org → Management → Dynamic levels; test planning, monitoring, control |
| 🟡 Medium | Test-Related Measures (consolidated) | Test-Related Measures | Reliability growth models, fault density, mutation score |
| 🟡 Medium | AI/ML/DL System Testing | Emerging Technologies | Data quality, model validation, metamorphic relations, bias testing |
| 🟡 Medium | Domain-Specific Testing | Testing in Dev Processes | Automotive, IoT, healthcare, mobile, avionics, finance, embedded |
| 🟢 Low | Blockchain & Cloud Testing | Emerging Technologies | Testing blockchain apps and cloud deployments |
| 🟢 Low | Testing Standards | Testing Tools | ISO 29119, IEEE 1012, TMMi, CMMI testing maturity |
