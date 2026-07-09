---
tags:
  - profile
  - medium-enterprise
  - essential-documents
  - software-engineering
---

# Project Profile — Medium / Enterprise

> **Team Size:** 10–50
> **Timeline:** 6–18 months
> **Methodology:** Hybrid (Agile + formal gates)
> **Regulatory Exposure:** Moderate
>
> **Key Principle:** *Structured enough for governance, flexible enough for delivery.*

---

## When to Use This Profile

- Mid-size product or platform development with cross-functional teams
- Enterprise applications requiring audit trails and compliance evidence
- Projects with formal release cycles, change control boards, or external stakeholders
- Teams transitioning from startup agility toward organizational maturity
- Systems with moderate regulatory or contractual documentation obligations
- Multi-team programs needing shared architecture and coordination artifacts

---

## Sources

> This profile draws its recommended documents from six bodies of knowledge:
>
> - [[BABOK — Business Analysis Body of Knowledge]]
> - [[PMBOK — Project Management Body of Knowledge]]
> - [[SWEBOK — Software Engineering Body of Knowledge]]
> - [[CyBOK — Cyber Security Body of Knowledge]]
> - [[DMBOK — Data Management Body of Knowledge]]
> - [[SEBOK — Systems Engineering Body of Knowledge]]

---

## Priority Legend

| Icon | Priority | Meaning |
|------|----------|---------|
| 🔴 | **Must Have** | Required — produce before the phase gate or release |
| 🟡 | **Nice to Have** | Recommended — create when scope, risk, or compliance justifies it |
| 🟢 | **Optional** | Produce only if project-specific conditions demand it |

---

## 1. Business Analysis

> **Owner:** BA / PO

| Document | Description | Priority | Source BOK |
|----------|-------------|----------|------------|
| Business Requirements Document (BRD) | High-level business needs, drivers, and constraints | 🔴 | BABOK |
| Business Objectives (SMART) | Specific, measurable goals the solution must achieve | 🔴 | BABOK |
| Current State Description | As-is process, system, and organizational landscape | 🔴 | BABOK |
| Future State Description | Target operating model and desired capabilities | 🔴 | BABOK |
| Change Strategy | Approach for transitioning from current to future state | 🔴 | BABOK |
| Solution Scope | Boundaries, inclusions, exclusions, assumptions | 🔴 | BABOK |
| Stakeholder Analysis | Identification, influence, interest, and engagement needs | 🔴 | BABOK |
| Gap Analysis | Delta between current and future state; remediation path | 🟡 | BABOK |
| Business Case | Cost-benefit justification and ROI analysis | 🔴 | PMBOK |

---

## 2. Requirements

> **Owner:** BA / PO

| Document | Description | Priority | Source BOK |
|----------|-------------|----------|------------|
| SRS (Software Requirements Specification) | Formal functional and nonfunctional requirements | 🔴 | SWEBOK |
| User Stories (Agile backlog items) | Epics, stories, tasks in product backlog | 🔴 | SWEBOK |
| Acceptance Criteria | Testable conditions defining story completion | 🔴 | SWEBOK |
| Nonfunctional Requirements Catalog | Performance, security, availability, scalability specs | 🔴 | SWEBOK |
| Requirements Traceability Matrix (RTM) | Bidirectional links: requirements ↔ design ↔ tests | 🔴 | SWEBOK |
| Requirements Change Log | History of requirement additions, modifications, removals | 🔴 | SWEBOK |
| Use Case Specifications | Actor-goal-scenario descriptions with alternate flows | 🟡 | SWEBOK |

---

## 3. Project Management

> **Owner:** PM

| Document | Description | Priority | Source BOK |
|----------|-------------|----------|------------|
| Project Charter | Formal authorization, objectives, authority, constraints | 🔴 | PMBOK |
| Project Management Plan | Integrated plan: scope, schedule, cost, quality, risk, etc. | 🔴 | PMBOK |
| WBS (Work Breakdown Structure) | Hierarchical decomposition of deliverables and work | 🔴 | PMBOK |
| Project Schedule / Baseline | Gantt, milestones, critical path, baseline for variance | 🔴 | PMBOK |
| Cost Baseline | Approved budget distributed across work packages | 🔴 | PMBOK |
| Risk Register | Identified risks, probability, impact, response strategies | 🔴 | PMBOK |
| Change Management Plan | Process for submitting, evaluating, approving changes | 🔴 | PMBOK |
| Stakeholder Register + Engagement Plan | Stakeholder list with power/interest grid and strategy | 🔴 | PMBOK |
| Communications Management Plan | Who gets what info, when, how, frequency | 🔴 | PMBOK |
| Quality Management Plan | Quality standards, metrics, assurance/control activities | 🔴 | PMBOK |
| RACI Matrix | Responsible / Accountable / Consulted / Informed mapping | 🟡 | PMBOK |

---

## 4. Architecture & Design

> **Owner:** Architect / Tech Lead

| Document | Description | Priority | Source BOK |
|----------|-------------|----------|------------|
| SAD (Software Architecture Document) | Overall architecture, components, connectors, rationale | 🔴 | SWEBOK |
| ADR (Architecture Decision Records) | Key design choices with context, options, consequences | 🔴 | SWEBOK |
| Architecture Views (4+1) | Logical, process, development, physical, use-case views | 🔴 | SWEBOK |
| ASR Catalog | Architecturally significant requirements and tactics | 🔴 | SWEBOK |
| HLD (High-Level Design) | System modules, interfaces, data flow, tech stack | 🔴 | SWEBOK |
| LLD (Low-Level Design) | Class diagrams, algorithms, DB tables, API contracts | 🔴 | SWEBOK |
| API Specification (OpenAPI) | RESTful / gRPC contracts, schemas, error codes | 🔴 | SWEBOK |
| ERD | Entity-relationship diagram for core data model | 🔴 | SWEBOK / DMBOK |
| Database Schema (DDL) | Table definitions, indexes, constraints, migrations | 🔴 | SWEBOK |
| Data Dictionary | Canonical definitions of data elements and types | 🟡 | SWEBOK / DMBOK |
| Design Review Records | Peer review outcomes, findings, resolutions | 🔴 | SWEBOK |
| Deployment Diagrams | Runtime node topology, artifact placement | 🟡 | SWEBOK |

---

## 5. Construction

> **Owner:** Developer

| Document | Description | Priority | Source BOK |
|----------|-------------|----------|------------|
| Source Code | Production codebase in version control | 🔴 | SWEBOK |
| README / Developer Guide | Setup, build, contribution, local dev instructions | 🔴 | SWEBOK |
| Unit Test Code | Automated tests for individual functions/classes | 🔴 | SWEBOK |
| Integration Test Code | Tests verifying component interaction and contracts | 🔴 | SWEBOK |
| Build Scripts | CI build definitions, compilation, packaging steps | 🔴 | SWEBOK |
| Dependency Manifest | Package lock files, pinned versions, license metadata | 🔴 | SWEBOK |
| SBOM | Software Bill of Materials — transitive dependency inventory | 🔴 | SWEBOK |
| API Documentation | Auto-generated or hand-written API reference | 🔴 | SWEBOK |
| Coding Standards / Style Guide | Naming, formatting, patterns, anti-patterns | 🔴 | SWEBOK |
| Code Review Records | PR/MR review comments, approvals, resolutions | 🔴 | SWEBOK |
| Commit Messages / Changelog | Conventional commits, release changelogs | 🔴 | SWEBOK |
| Static Analysis Reports | Linter, complexity, duplication, code smell findings | 🟡 | SWEBOK |

---

## 6. Testing

> **Owner:** QA

| Document | Description | Priority | Source BOK |
|----------|-------------|----------|------------|
| Test Plan | Scope, approach, resources, schedule, pass/fail criteria | 🔴 | SWEBOK |
| Test Strategy | Organization-wide testing philosophy, levels, types | 🟡 | SWEBOK |
| Test Cases | Step-by-step scenarios with expected results | 🔴 | SWEBOK |
| Test Suite | Grouped test cases by feature, cycle, or priority | 🔴 | SWEBOK |
| Test Data | Datasets, fixtures, synthetic data for test execution | 🔴 | SWEBOK |
| Test Scripts (automated) | E2E, regression, smoke automation code | 🔴 | SWEBOK |
| Defect Report | Bug log: steps, severity, priority, status, owner | 🔴 | SWEBOK |
| Regression Test Suite | Tests ensuring existing functionality unbroken | 🔴 | SWEBOK |
| Traceability Matrix (Req ↔ Tests) | Mapping each requirement to covering test cases | 🔴 | SWEBOK |
| Test Report | Execution summary: pass/fail, defects, observations | 🔴 | SWEBOK |
| Test Completion Report | Final exit criteria assessment, residual risk | 🔴 | SWEBOK |
| UAT Sign-off | Stakeholder acceptance of delivered solution | 🔴 | SWEBOK |
| Coverage Report | Code/requirement coverage metrics | 🟡 | SWEBOK |
| Performance Test Report | Load, stress, soak, spike test findings | 🟡 | SWEBOK |
| Security Test Report | Vulnerability scan, pen test, security regression results | 🟡 | SWEBOK |

---

## 7. Security

> **Owner:** Security Engineer

| Document | Description | Priority | Source BOK |
|----------|-------------|----------|------------|
| Threat Model | STRIDE / PASTA analysis of attack surfaces and mitigations | 🔴 | CyBOK |
| Security Requirements Specification | Authentication, authorization, crypto, data protection | 🔴 | CyBOK |
| Secure Coding Guidelines | OWASP-aligned coding practices and anti-patterns | 🔴 | CyBOK |
| SAST Report | Static application security testing findings | 🔴 | CyBOK |
| SCA Report (Software Composition Analysis) | Vulnerability scan of third-party/open-source dependencies | 🔴 | CyBOK |
| DAST Report | Dynamic application security testing findings | 🟡 | CyBOK |
| Access Control Policy | RBAC/ABAC definitions, least-privilege rules | 🔴 | CyBOK |
| Penetration Test Report | Third-party or internal ethical hacking findings | 🟡 | CyBOK |
| Secure Design Review Report | Architecture-level security assessment and findings | 🟡 | CyBOK |

---

## 8. Data Management

> **Owner:** Data Architect / DBA

| Document | Description | Priority | Source BOK |
|----------|-------------|----------|------------|
| Conceptual Data Model (CDM) | High-level business entities and relationships | 🟡 | DMBOK |
| Logical Data Model (LDM) | Normalized entity-attribute-relationship model | 🔴 | DMBOK |
| Physical Data Model (PDM) | Platform-specific tables, indexes, storage optimization | 🔴 | DMBOK |
| Data Dictionary | Authoritative definitions, types, constraints, owners | 🔴 | DMBOK |
| Data Quality Rules | Validation, completeness, consistency, timeliness rules | 🟡 | DMBOK |
| Backup & Recovery Plan | Backup schedule, retention, RPO/RTO, restore procedures | 🔴 | DMBOK |
| Data Retention Policy | Lifecycle, archival, deletion rules per data class | 🟡 | DMBOK |
| Data Classification Schema | Sensitivity tiers (public, internal, confidential, restricted) | 🟡 | DMBOK |

---

## 9. Deployment & Operations

> **Owner:** DevOps / SRE

| Document | Description | Priority | Source BOK |
|----------|-------------|----------|------------|
| CI/CD Pipeline Configuration | Build, test, deploy automation definitions | 🔴 | SWEBOK |
| Deployment Plan | Steps, environments, dependencies, rollback triggers | 🔴 | SWEBOK |
| Rollback Plan | Procedure to revert to last known good state | 🔴 | SWEBOK |
| SLA | Uptime, response time, throughput commitments | 🔴 | SWEBOK |
| Operations Manual / Runbook | Day-2 operational procedures, troubleshooting guides | 🔴 | SWEBOK |
| Release Notes | New features, fixes, known issues, upgrade instructions | 🔴 | SWEBOK |
| Incident Management Process | Severity levels, escalation, communication, postmortem | 🔴 | SWEBOK |
| Disaster Recovery Plan | RPO/RTO targets, failover procedures, DR site details | 🔴 | SWEBOK |
| Monitoring Dashboard Configuration | Metrics, alerts, SLOs, dashboard definitions | 🟡 | SWEBOK |
| Container Configurations (Docker/K8s) | Dockerfiles, Helm charts, manifests, resource limits | 🟡 | SWEBOK |
| Infrastructure-as-Code | Terraform, Pulumi, CloudFormation definitions | 🟡 | SWEBOK |

---

## 10. Maintenance

> **Owner:** Support / Maintainer

| Document | Description | Priority | Source BOK |
|----------|-------------|----------|------------|
| Maintenance Plan | Ongoing support scope, schedules, resource allocation | 🔴 | SWEBOK |
| Impact Analysis Report | Change impact on existing system, risk, effort estimate | 🔴 | SWEBOK |
| MR/PR (Modification Request) | Formal change request with justification and priority | 🔴 | SWEBOK |
| Maintenance Log / Change History | Chronological record of all post-release modifications | 🔴 | SWEBOK |
| Technical Debt Register | Known tech debt items with impact, priority, remediation | 🟡 | SWEBOK |

---

## 11. Quality Assurance

> **Owner:** QA Lead

| Document | Description | Priority | Source BOK |
|----------|-------------|----------|------------|
| SQAP (Software Quality Assurance Plan) | QA processes, standards, audits, reporting | 🔴 | SWEBOK |
| V&V Plan | Verification & validation strategy, methods, milestones | 🔴 | SWEBOK |
| Review Records | Inspection/checklist outcomes, findings, corrective actions | 🔴 | SWEBOK |
| Defect Log / Metrics | Defect density, trend, aging, severity distribution | 🔴 | SWEBOK |
| Quality Metrics Dashboard | Real-time quality KPIs and trend visualization | 🟡 | SWEBOK |

---

## 12. Configuration Management

> **Owner:** DevOps / Config Manager

| Document | Description | Priority | Source BOK |
|----------|-------------|----------|------------|
| SCMP (SCM Plan) | Config identification, control, status accounting, auditing | 🔴 | SWEBOK |
| Baseline Records | Approved baselines: requirements, design, code, test | 🔴 | SWEBOK |
| Change Request (CR/SCR) | Formal configuration change submission and approval | 🔴 | SWEBOK |
| Version Description Document | What changed between versions, dependencies, build info | 🔴 | SWEBOK |

---

## 13. Cross-Cutting: Systems Engineering (Light)

> **Owner:** Systems Engineer

| Document | Description | Priority | Source BOK |
|----------|-------------|----------|------------|
| ConOps (Concept of Operations) | High-level operational scenario and user interaction model | 🟡 | SEBOK |
| Interface Control Document (ICD) | System-to-system interface specs, protocols, data formats | 🟡 | SEBOK |
| Verification Plan | How the system will be verified against requirements | 🟡 | SEBOK |
| Validation Plan | How the system will be validated against user needs | 🟡 | SEBOK |
| Transition Plan | Migration, cutover, data conversion, training approach | 🟡 | SEBOK |

---

---

## Quick-Start Checklist

> Print this table and check off 🔴 documents as your team produces them.

| Phase | Document | ✓ |
|-------|----------|---|
| **Business Analysis** | Business Requirements Document (BRD) | ☐ |
| | Business Objectives (SMART) | ☐ |
| | Current State Description | ☐ |
| | Future State Description | ☐ |
| | Change Strategy | ☐ |
| | Solution Scope | ☐ |
| | Stakeholder Analysis | ☐ |
| | Business Case | ☐ |
| **Requirements** | SRS (Software Requirements Specification) | ☐ |
| | User Stories (Agile backlog items) | ☐ |
| | Acceptance Criteria | ☐ |
| | Nonfunctional Requirements Catalog | ☐ |
| | Requirements Traceability Matrix (RTM) | ☐ |
| | Requirements Change Log | ☐ |
| **Project Management** | Project Charter | ☐ |
| | Project Management Plan | ☐ |
| | WBS (Work Breakdown Structure) | ☐ |
| | Project Schedule / Baseline | ☐ |
| | Cost Baseline | ☐ |
| | Risk Register | ☐ |
| | Change Management Plan | ☐ |
| | Stakeholder Register + Engagement Plan | ☐ |
| | Communications Management Plan | ☐ |
| | Quality Management Plan | ☐ |
| **Architecture & Design** | SAD (Software Architecture Document) | ☐ |
| | ADR (Architecture Decision Records) | ☐ |
| | Architecture Views (4+1) | ☐ |
| | ASR Catalog | ☐ |
| | HLD (High-Level Design) | ☐ |
| | LLD (Low-Level Design) | ☐ |
| | API Specification (OpenAPI) | ☐ |
| | ERD | ☐ |
| | Database Schema (DDL) | ☐ |
| | Design Review Records | ☐ |
| **Construction** | Source Code | ☐ |
| | README / Developer Guide | ☐ |
| | Unit Test Code | ☐ |
| | Integration Test Code | ☐ |
| | Build Scripts | ☐ |
| | Dependency Manifest | ☐ |
| | SBOM | ☐ |
| | API Documentation | ☐ |
| | Coding Standards / Style Guide | ☐ |
| | Code Review Records | ☐ |
| | Commit Messages / Changelog | ☐ |
| **Testing** | Test Plan | ☐ |
| | Test Cases | ☐ |
| | Test Suite | ☐ |
| | Test Data | ☐ |
| | Test Scripts (automated) | ☐ |
| | Defect Report | ☐ |
| | Regression Test Suite | ☐ |
| | Traceability Matrix (Req ↔ Tests) | ☐ |
| | Test Report | ☐ |
| | Test Completion Report | ☐ |
| | UAT Sign-off | ☐ |
| **Security** | Threat Model | ☐ |
| | Security Requirements Specification | ☐ |
| | Secure Coding Guidelines | ☐ |
| | SAST Report | ☐ |
| | SCA Report (Software Composition Analysis) | ☐ |
| | Access Control Policy | ☐ |
| **Data Management** | Logical Data Model (LDM) | ☐ |
| | Physical Data Model (PDM) | ☐ |
| | Data Dictionary | ☐ |
| | Backup & Recovery Plan | ☐ |
| **Deployment & Operations** | CI/CD Pipeline Configuration | ☐ |
| | Deployment Plan | ☐ |
| | Rollback Plan | ☐ |
| | SLA | ☐ |
| | Operations Manual / Runbook | ☐ |
| | Release Notes | ☐ |
| | Incident Management Process | ☐ |
| | Disaster Recovery Plan | ☐ |
| **Maintenance** | Maintenance Plan | ☐ |
| | Impact Analysis Report | ☐ |
| | MR/PR (Modification Request) | ☐ |
| | Maintenance Log / Change History | ☐ |
| **Quality Assurance** | SQAP (Software Quality Assurance Plan) | ☐ |
| | V&V Plan | ☐ |
| | Review Records | ☐ |
| | Defect Log / Metrics | ☐ |
| **Configuration Management** | SCMP (SCM Plan) | ☐ |
| | Baseline Records | ☐ |
| | Change Request (CR/SCR) | ☐ |
| | Version Description Document | ☐ |

---

## Related

- [[BABOK Essential Documents]] — Business Analysis Body of Knowledge
- [[PMBOK Essential Documents]] — Project Management Body of Knowledge
- [[SWEBOK Essential Documents]] — Software Engineering Body of Knowledge
- [[CyBOK Essential Documents]] — Cyber Security Body of Knowledge
- [[DMBOK Essential Documents]] — Data Management Body of Knowledge
- [[SEBOK Essential Documents]] — Systems Engineering Body of Knowledge
- [[Profile-Small-Startup]]
- [[Profile-Large-Safety-Critical]]
- [[Essential Documents - Overview]]
