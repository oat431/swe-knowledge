---
tags:
  - profile
  - small-startup
  - essential-documents
  - software-engineering
---

# Project Profile — Small / Startup

> **Team:** 1–5 developers
> **Timeline:** Weeks to months
> **Methodology:** Agile / Lean
> **Regulatory:** Low / None
>
> **Key principle:** Just enough documentation to ship — and not get burned.

## When to Use This Profile

- Greenfield product or MVP with a small co-located or remote team
- No formal compliance or certification requirements
- Rapid iteration cycles (1–2 week sprints)
- Minimal dedicated QA or DevOps roles — developers wear multiple hats
- Budget or time constraints make heavyweight process impractical
- Startup environment where speed-to-market is the primary driver

## Sources

> This profile draws selections from the following Bodies of Knowledge:
>
> - [[SWEBOK Essential Documents]]
> - [[PMBOK Essential Documents]]
> - [[BABOK Essential Documents]]
> - [[SEBOK Essential Documents]]
> - [[CyBOK Essential Documents]]
> - [[DMBOK Essential Documents]]

## Priority Legend

| Symbol | Priority               | Meaning                                                     |
|--------|------------------------|-------------------------------------------------------------|
| 🔴     | **Must Have**          | Essential — produce before or during the phase              |
| 🟡     | **Nice to Have**       | Recommended for growing teams; add when capacity allows     |
| 🟢     | **Optional**           | Situational — produce only if the project context demands it|

---

## 1. Discovery & Requirements

> **Owner:** PO / Founder

| Document                              | Description                                                  | Priority | Source BOK         |
|---------------------------------------|--------------------------------------------------------------|----------|--------------------|
| Business Objectives                   | SMART goals defining product vision and success metrics      | 🔴       | BABOK              |
| User Stories                          | As a…I want…so that… format capturing user needs             | 🔴       | SWEBOK / BABOK     |
| Acceptance Criteria                   | BDD / Given-When-Then conditions for each story              | 🔴       | SWEBOK             |
| Stakeholder Analysis                  | Lightweight map of key stakeholders and their interests      | 🟡       | BABOK              |
| Product Backlog                       | Prioritized list of features, bugs, and tech debt            | 🔴       | PMBOK              |

## 2. Architecture & Design

> **Owner:** Tech Lead

| Document                              | Description                                                  | Priority | Source BOK         |
|---------------------------------------|--------------------------------------------------------------|----------|--------------------|
| ADR (Architecture Decision Records)   | Lightweight records of key technical decisions and trade-offs| 🔴       | SWEBOK             |
| API Specification                     | OpenAPI / Swagger contract for service interfaces            | 🔴       | SWEBOK             |
| Database Schema                       | DDL scripts defining tables, indexes, and constraints        | 🔴       | SWEBOK             |
| ERD (Entity-Relationship Diagram)     | Visual model of data entities and their relationships        | 🟡       | SWEBOK / DMBOK     |
| SAD (Software Architecture Document)  | Lightweight diagram of components, layers, and data flow     | 🟡       | SWEBOK             |

## 3. Construction

> **Owner:** Developer

| Document                              | Description                                                  | Priority | Source BOK         |
|---------------------------------------|--------------------------------------------------------------|----------|--------------------|
| Source Code                           | Well-structured code with inline documentation               | 🔴       | SWEBOK             |
| README / Developer Guide              | Setup instructions, architecture overview, contribution guide| 🔴       | SWEBOK             |
| Unit Test Code                        | Automated tests covering core business logic                 | 🔴       | SWEBOK             |
| Build Scripts                         | Repeatable build and packaging scripts                       | 🔴       | SWEBOK             |
| Dependency Manifest                   | package.json / go.mod / requirements.txt pinning versions    | 🔴       | SWEBOK             |
| Commit Messages / Changelog           | Conventional commits and auto-generated changelog            | 🔴       | SWEBOK             |
| Coding Standards                      | Short style guide or linter config the team agrees on        | 🟡       | SWEBOK             |
| Code Review Records                   | PR comments and approval history                             | 🟡       | SWEBOK             |

## 4. Testing

> **Owner:** Developer / QA

| Document                              | Description                                                  | Priority | Source BOK         |
|---------------------------------------|--------------------------------------------------------------|----------|--------------------|
| Test Plan                             | Lightweight plan covering scope, approach, and environments  | 🔴       | SWEBOK             |
| Test Cases                            | Documented scenarios with expected results                   | 🔴       | SWEBOK             |
| Defect Report                        | Tracked bugs with steps to reproduce and severity            | 🔴       | SWEBOK             |
| Regression Test Suite                 | Reusable test set to verify existing features after changes  | 🟡       | SWEBOK             |
| Coverage Report                       | Automated code coverage metrics from CI                      | 🟡       | SWEBOK             |

## 5. Deployment & Operations

> **Owner:** DevOps / Developer

| Document                              | Description                                                  | Priority | Source BOK         |
|---------------------------------------|--------------------------------------------------------------|----------|--------------------|
| CI/CD Pipeline Configuration          | Automated build, test, and deploy pipeline definition        | 🔴       | SWEBOK             |
| Deployment Plan                       | Step-by-step procedure for releasing to production           | 🔴       | SWEBOK             |
| Release Notes                         | Summary of changes, fixes, and new features per release      | 🔴       | SWEBOK             |
| Runbook                               | Basic operational procedures for common incidents            | 🟡       | SWEBOK             |
| Backup & Recovery Plan                | Database and file backup strategy with recovery steps        | 🟡       | DMBOK              |

## 6. Security

> **Owner:** Developer

| Document                              | Description                                                  | Priority | Source BOK         |
|---------------------------------------|--------------------------------------------------------------|----------|--------------------|
| Threat Model                          | Lightweight STRIDE analysis of key attack surfaces           | 🔴       | CyBOK              |
| Secure Coding Guidelines              | OWASP Top 10 reference and project-specific practices        | 🟡       | CyBOK              |
| SAST in CI/CD                         | Automated static application security testing in pipeline    | 🟡       | CyBOK              |

## 7. Project Management

> **Owner:** Founder / PO

| Document                              | Description                                                  | Priority | Source BOK         |
|---------------------------------------|--------------------------------------------------------------|----------|--------------------|
| Sprint Board / Task Tracker           | Visual board tracking work in progress and backlog           | 🔴       | PMBOK              |
| Risk Register                         | Simple list of identified risks with likelihood and impact   | 🟡       | PMBOK              |
| Meeting Minutes                       | Key decisions and action items from team syncs               | 🟡       | PMBOK              |

---

## Quick-Start Checklist

Print this table and check off each 🔴 item as you produce it.

| Phase                        | Document                             | ✓  |
|------------------------------|--------------------------------------|----|
| Discovery & Requirements     | Business Objectives                  | ☐  |
| Discovery & Requirements     | User Stories                         | ☐  |
| Discovery & Requirements     | Acceptance Criteria                  | ☐  |
| Discovery & Requirements     | Product Backlog                      | ☐  |
| Architecture & Design        | ADR (Architecture Decision Records)  | ☐  |
| Architecture & Design        | API Specification                    | ☐  |
| Architecture & Design        | Database Schema                      | ☐  |
| Construction                 | Source Code                          | ☐  |
| Construction                 | README / Developer Guide             | ☐  |
| Construction                 | Unit Test Code                       | ☐  |
| Construction                 | Build Scripts                        | ☐  |
| Construction                 | Dependency Manifest                  | ☐  |
| Construction                 | Commit Messages / Changelog          | ☐  |
| Testing                      | Test Plan                            | ☐  |
| Testing                      | Test Cases                           | ☐  |
| Testing                      | Defect Report                        | ☐  |
| Deployment & Operations      | CI/CD Pipeline Configuration         | ☐  |
| Deployment & Operations      | Deployment Plan                      | ☐  |
| Deployment & Operations      | Release Notes                        | ☐  |
| Security                     | Threat Model                         | ☐  |
| Project Management           | Sprint Board / Task Tracker          | ☐  |

---

## Related

### BOK Essential Documents
- [[SWEBOK Essential Documents]]
- [[PMBOK Essential Documents]]
- [[BABOK Essential Documents]]
- [[SEBOK Essential Documents]]
- [[CyBOK Essential Documents]]
- [[DMBOK Essential Documents]]

### Other Profiles
- [[Profile-Medium-Enterprise]]
- [[Profile-Large-Safety-Critical]]

### Overview
- [[Essential Documents - Overview]]
