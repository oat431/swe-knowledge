---
tags: [overview, software-engineering, sdlc, swebok, essential-documents]
---

# SDLC — Standard Documents by Phase

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4 (IEEE Computer Society, 2024)]]
> Organized by standard SDLC phases: Requirements → Architecture → Design → Construction → Testing → Deployment/Operations → Maintenance
> Plus cross-cutting documents (Management, Quality, Security, SCM)
>
> ⚠️ **This is a document-only extract.** For the full body of knowledge, see:
> `F:\projects\orlita_md\software-engineering-note\Body of Knowledge\SWEBOK\`
>
> 📄 **Original file:** [[../Software Engineering Note Content|Software Engineering Note Content.md]]

**Priority Legend:**

| Icon | Level | Meaning |
|---|---|---|
| 🔴 | **Must Have** | Essential for virtually all projects; skipping creates significant risk |
| 🟡 | **Nice to Have** | Recommended for medium+ complexity; adds substantial value |
| 🟢 | **Optional** | Situational — depends on domain, scale, regulatory needs, or methodology |

---

## 1. Requirements

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Acceptance Criteria** (ATDD/BDD Specs) | Given/When/Then scenarios; precise, testable requirement statements | 🔴 Must Have | ISO/IEC/IEEE 29119 |
| **BRD** (Business Requirements Document) | High-level business needs, objectives, and scope | 🔴 Must Have | — |
| **Nonfunctional Requirements Catalog** | QoS constraints: performance, security, reliability, usability targets | 🔴 Must Have | ISO/IEC 25010 (SQuaRE) |
| **Requirements Change Log** | History of requirement changes with rationale | 🔴 Must Have | ISO/IEC/IEEE 29148 |
| **Requirements Traceability Matrix** (RTM) | Forward/backward links: requirement → design → code → test | 🔴 Must Have | ISO/IEC/IEEE 29148 |
| **SRS** (Software Requirements Specification) | Formal document capturing functional and nonfunctional requirements | 🔴 Must Have | ISO/IEC/IEEE 29148 |
| **Stakeholder Analysis** | Identified stakeholder classes, their concerns, and influence | 🔴 Must Have | ISO/IEC/IEEE 29148 |
| **User Stories** | "As a… I want… so that…" format; Agile/backlog items | 🔴 Must Have (Agile) | — |
| **Activity Diagrams** | Business process / workflow models | 🟡 Nice to Have | ISO/IEC 19501 (UML) |
| **Data Flow Diagrams** (DFD) — Context level | System boundary and external entity interactions | 🟡 Nice to Have | — |
| **Decision Tables / Decision Trees** | Complex business rule specifications | 🟡 Nice to Have | — |
| **Functional Size Measurement** (FSM) | Quantified requirements volume (function points, story points) | 🟡 Nice to Have | ISO/IEC 20926 (IFPUG), ISO/IEC 24570 (NESMA) |
| **Prototypes** (low/high fidelity) | Exploratory mockups for requirements elicitation/validation | 🟡 Nice to Have | — |
| **Sequence Diagrams** (System-level) | Interaction sequences between actors and system | 🟡 Nice to Have | ISO/IEC 19501 (UML) |
| **State Diagrams** | System state transitions and lifecycle | 🟡 Nice to Have | ISO/IEC 19501 (UML) |
| **Use Case Diagrams / Use Case Specifications** | Actor-goal modeling; structured templates with pre/post conditions | 🟡 Nice to Have | ISO/IEC 19501 (UML) |
| **QFD House of Quality** | Quality function deployment — mapping customer needs to technical requirements | 🟢 Optional | — |

---

## 2. Architecture

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **ADR** (Architecture Decision Records) | Rationale for each significant decision: alternatives, trade-offs, rejected options | 🔴 Must Have | ISO/IEC/IEEE 42010 |
| **ASR Catalog** (Architecturally Significant Requirements) | Requirements that shape the architecture | 🔴 Must Have | ISO/IEC/IEEE 42010 |
| **Architecture Views** (4+1 View Model) | Logical, Development, Process, Physical views + Scenarios | 🔴 Must Have | ISO/IEC/IEEE 42010 |
| **SAD** (Software Architecture Document) | Architecture description of fundamental concepts and properties | 🔴 Must Have | ISO/IEC/IEEE 42010 |
| **ADL Models** (UML / ArchiMate / SysML) | Formal architecture description language representations | 🟡 Nice to Have | ISO/IEC 19501 (UML), ISO/IEC 42010 |
| **Allocation Views** | Mapping software to environment: deployment, installation, work assignment | 🟡 Nice to Have | ISO/IEC/IEEE 42010 |
| **Architecture Evaluation Report** (ATAM / SAAM) | Quality-attribute trade-off analysis using utility trees and scenarios | 🟡 Nice to Have | — |
| **Architecture Metrics Report** | Coupling, cohesion, cyclomatic complexity, dependency analysis | 🟡 Nice to Have | — |
| **Architecture Patterns & Styles Catalog** | Chosen patterns: layered, microservices, event-driven, CQRS, etc. | 🟡 Nice to Have | — |
| **Component-and-Connector (C&C) Views** | Runtime structure: components, connectors, ports, protocols | 🟡 Nice to Have | ISO/IEC/IEEE 42010 |
| **Module Views** | Development-time structure: layers, packages, subsystems | 🟡 Nice to Have | ISO/IEC/IEEE 42010 |
| **QAW Report** (Quality Attribute Workshop) | Stakeholder-elicited architecture-driving quality requirements | 🟢 Optional | — |
| **Reference Architecture** | Domain-specific standardized architecture guidance | 🟢 Optional | — |

---

## 3. Design

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **API Specifications** (OpenAPI / Swagger / IDL) | Interface contracts: endpoints, methods, request/response schemas | 🔴 Must Have | OpenAPI Spec (OAS 3.x) |
| **Class Diagrams** | Static structure: classes, attributes, methods, relationships | 🔴 Must Have (OOP) | ISO/IEC 19501 (UML) |
| **Database Schema** (DDL) | Physical database design: tables, indexes, constraints | 🔴 Must Have | — |
| **Design Review Records** | Review findings, action items, sign-off | 🔴 Must Have | ISO/IEC 20246 |
| **ERD** (Entity-Relationship Diagram) | Data model: entities, attributes, relationships, cardinality | 🔴 Must Have | — |
| **HLD** (High-Level Design Document) | Top-level structure: major components, interfaces, data formats, protocols | 🔴 Must Have | — |
| **LLD** (Low-Level / Detailed Design Document) | Module internals: algorithms, data structures, class details, state machines | 🔴 Must Have | — |
| **Activity Diagrams** | Detailed process flows and control logic | 🟡 Nice to Have | ISO/IEC 19501 (UML) |
| **Component Diagrams** | Component interfaces, ports, and wiring | 🟡 Nice to Have | ISO/IEC 19501 (UML) |
| **Data Dictionary** | Formal definitions of all data elements | 🟡 Nice to Have | — |
| **Deployment Diagrams** | Physical deployment topology | 🟡 Nice to Have | ISO/IEC 19501 (UML) |
| **Design Rationale** | Why decisions were made: assumptions, alternatives, trade-offs | 🟡 Nice to Have | — |
| **Sequence Diagrams** (Design-level) | Object interactions for specific scenarios | 🟡 Nice to Have | ISO/IEC 19501 (UML) |
| **State Diagrams / Statecharts** | Component lifecycle and state transitions | 🟡 Nice to Have | ISO/IEC 19501 (UML) |
| **CRC Cards** | Class-Responsibility-Collaboration — lightweight OO design | 🟢 Optional | — |
| **Communication Diagrams** | Object collaboration with message numbering | 🟢 Optional | ISO/IEC 19501 (UML) |
| **Design Pattern Catalog** | Applied GoF patterns and their context | 🟢 Optional | — |
| **Feature Models** | Variability modeling for product lines | 🟢 Optional | — |
| **Pseudocode / PDL** (Program Design Language) | Algorithm specifications at human-readable level | 🟢 Optional | — |

---

## 4. Construction

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **API Documentation** (OpenAPI / JSDoc / Javadoc / godoc) | Auto-generated or manually written interface docs | 🔴 Must Have | — |
| **Build Scripts** (Makefile / Gradle / Maven / Cargo / etc.) | Automated compilation, linking, packaging | 🔴 Must Have | — |
| **Code Review Records** | Peer review findings, comments, approvals | 🔴 Must Have | ISO/IEC 20246 |
| **Coding Standards / Style Guide** | Language-specific conventions, naming, formatting | 🔴 Must Have | CERT Secure Coding Standards |
| **Commit Messages / Changelog** | Granular change tracking at VCS level | 🔴 Must Have | — |
| **Dependency Manifest** (package.json / go.mod / Cargo.toml / pom.xml) | Declared dependencies with versions | 🔴 Must Have | — |
| **Integration Test Code** | Cross-component interaction tests | 🔴 Must Have | ISO/IEC/IEEE 29119 |
| **README / Developer Guide** | Setup, build, run, test instructions | 🔴 Must Have | — |
| **SBOM** (Software Bill of Materials) | Component inventory for supply-chain transparency | 🔴 Must Have | ISO/IEC 5230 (OpenChain), SPDX |
| **Source Code** | The primary deliverable — with inline documentation | 🔴 Must Have | — |
| **Unit Test Code** | Isolated component test cases | 🔴 Must Have | ISO/IEC/IEEE 29119 |
| **Mock/Stub/Driver Specifications** | Test scaffolding for isolated testing | 🟡 Nice to Have | — |
| **Static Analysis Reports** | Linting, complexity, security, and style violation results | 🟡 Nice to Have | — |
| **TDD Test Cases** (Test-First) | Tests written before implementation code | 🟡 Nice to Have | — |

---

## 5. Testing

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Defect Report / Bug Report** | Fault documentation: steps to reproduce, severity, priority, status | 🔴 Must Have | IEEE 1044 (Defect Classification) |
| **Regression Test Suite** | Selective retest suite for change verification | 🔴 Must Have | ISO/IEC/IEEE 29119-4 |
| **Test Cases** | Specific inputs, conditions, procedures, expected outcomes | 🔴 Must Have | ISO/IEC/IEEE 29119-4 |
| **Test Completion Report** | Summary: coverage achieved, defects found/fixed, residual risk | 🔴 Must Have | ISO/IEC/IEEE 29119-3 |
| **Test Data** | Curated datasets for test execution | 🔴 Must Have | ISO/IEC/IEEE 29119-4 |
| **Test Plan** | Overall strategy: scope, objectives, resources, schedule, risks | 🔴 Must Have | ISO/IEC/IEEE 29119-3 |
| **Test Report** | Execution results: pass/fail counts, coverage metrics, defect summary | 🔴 Must Have | ISO/IEC/IEEE 29119-3 |
| **Test Scripts** | Automated test execution code | 🔴 Must Have | ISO/IEC/IEEE 29119-4 |
| **Test Suite** | Organized collection of test cases | 🔴 Must Have | ISO/IEC/IEEE 29119-4 |
| **Traceability Matrix** (Requirements ↔ Tests) | Coverage mapping: each requirement traced to test cases | 🔴 Must Have | ISO/IEC/IEEE 29119-4 |
| **UAT Sign-off** (User Acceptance Testing) | Stakeholder approval of system readiness | 🔴 Must Have | ISO/IEC/IEEE 29119-4 |
| **Coverage Report** | Statement, branch, path, MC/DC, mutation coverage metrics | 🟡 Nice to Have | ISO/IEC/IEEE 29119-4 |
| **Performance Test Report** | Load, stress, scalability, volume testing results | 🟡 Nice to Have | ISO/IEC/IEEE 29119-4 |
| **Security Test Report** | Penetration test, fuzz test, vulnerability scan results | 🟡 Nice to Have | ISO/IEC/IEEE 29119-4 |
| **Test Strategy** | Organizational-level approach: levels, techniques, tools, entry/exit criteria | 🟡 Nice to Have | ISO/IEC/IEEE 29119-2 |
| **Usability Test Report** | UX evaluation findings | 🟢 Optional | ISO/IEC 25010 (SQuaRE), ISO 9241 |

---

## 6. Deployment & Operations (DevOps)

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **CI/CD Pipeline Configuration** | Build, test, deploy automation (Jenkinsfile, GitHub Actions, GitLab CI) | 🔴 Must Have | ISO/IEC/IEEE 32675 (DevOps) |
| **Deployment Plan** | Step-by-step release procedure, rollback strategy | 🔴 Must Have | ISO/IEC/IEEE 32675 (DevOps) |
| **Disaster Recovery Plan** | Failover, backup/restore procedures, RPO/RTO targets | 🔴 Must Have | ISO/IEC 27031 (ICT Readiness) |
| **Incident Management Process** | Triage, escalation, post-mortem, RCA template | 🔴 Must Have | ISO/IEC/IEEE 20000-1 |
| **Operations Manual / Runbook** | Routine procedures: startup, shutdown, backup, monitoring | 🔴 Must Have | ISO/IEC/IEEE 20000-1 |
| **Release Notes / Version Description Document** (VDD) | What's new, fixed, known issues per release | 🔴 Must Have | IEEE 828 (CM Plans) |
| **Rollback Plan** | Reversion procedure with data migration scripts | 🔴 Must Have | ISO/IEC/IEEE 32675 (DevOps) |
| **SLA** (Service-Level Agreement) | Availability, performance, response-time commitments | 🔴 Must Have | ISO/IEC/IEEE 20000-1 |
| **Capacity Plan** | Sizing, workload estimates, scaling strategy | 🟡 Nice to Have | ISO/IEC/IEEE 20000-1 |
| **Container Configurations** (Dockerfile, docker-compose, K8s manifests) | Application packaging and orchestration | 🟡 Nice to Have | — |
| **Infrastructure-as-Code** (Terraform / Ansible / Pulumi) | Provisioning scripts for environments | 🟡 Nice to Have | — |
| **Monitoring Dashboard Configuration** | Telemetry, alerts, KPIs | 🟡 Nice to Have | ISO/IEC/IEEE 20000-1 |
| **Operational KPIs Report** | Deployment frequency, lead time, MTTR, change failure rate | 🟡 Nice to Have | ISO/IEC/IEEE 32675 (DevOps) |
| **SLO / SLI Definitions** | Service-level objectives and indicators | 🟡 Nice to Have | ISO/IEC/IEEE 20000-1 |

---

## 7. Maintenance

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Impact Analysis Report** | Affected components, estimated effort, risks, downstream effects | 🔴 Must Have | ISO/IEC/IEEE 14764 |
| **MR/PR** (Modification Request / Problem Report) | Formal change or fix request with categorization | 🔴 Must Have | ISO/IEC/IEEE 14764 |
| **Maintenance Log / Change History** | Record of all post-release modifications | 🔴 Must Have | ISO/IEC/IEEE 14764 |
| **Maintenance Plan** | Strategy for corrective, adaptive, perfective, preventive, emergency maintenance | 🔴 Must Have | ISO/IEC/IEEE 14764 |
| **Maintenance Metrics Dashboard** | Effort by category, request volume, MTTR, backlog | 🟡 Nice to Have | ISO/IEC/IEEE 14764 |
| **Reengineering Documentation** | Updated design/docs after significant restructuring | 🟡 Nice to Have | ISO/IEC/IEEE 14764 |
| **SLA Compliance Report** | Maintenance performance against service commitments | 🟡 Nice to Have | ISO/IEC/IEEE 20000-1 |
| **Technical Debt Register** | Known shortcuts, their cost, and remediation plan | 🟡 Nice to Have | — |
| **Refactoring Log** | Internal restructuring changes (no behavioral change) | 🟢 Optional | — |
| **Reverse Engineering Artifacts** | Recovered specifications, design descriptions from legacy code | 🟢 Optional | — |

---

## Cross-Cutting Documents

### Project Management

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Meeting Minutes** | Decision records from key meetings | 🔴 Must Have | — |
| **Project Charter** | Authorization, scope, objectives, stakeholders | 🔴 Must Have | — |
| **Project Closure Report** | Lessons learned, retrospective, archival | 🔴 Must Have | ISO/IEC/IEEE 12207 |
| **Project Plan / SMP** (Software Management Plan) | Schedule, resources, budget, milestones | 🔴 Must Have | ISO/IEC/IEEE 12207 |
| **Risk Register** | Identified risks, probability, impact, mitigation, status | 🔴 Must Have | ISO 31000 (Risk Management) |
| **WBS** (Work Breakdown Structure) | Hierarchical task decomposition | 🔴 Must Have | — |
| **Earned Value / Variance Reports** | Cost and schedule performance tracking | 🟡 Nice to Have | — |
| **Gantt Chart / Schedule** | Task dependencies and timeline | 🟡 Nice to Have | — |
| **RACI Matrix** | Role clarity: Responsible, Accountable, Consulted, Informed | 🟡 Nice to Have | — |
| **Stakeholder Communication Plan** | Who gets what information, when, how | 🟡 Nice to Have | — |

### Configuration Management

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Baseline Records** | Formally approved CI versions at life-cycle milestones | 🔴 Must Have | IEEE 828 |
| **Change Request** (CR/SCR) | Formal modification proposal with impact analysis | 🔴 Must Have | IEEE 828 |
| **FCA Report** (Functional Configuration Audit) | Verification that SCI matches functional specs | 🔴 Must Have | IEEE 828 |
| **PCA Report** (Physical Configuration Audit) | Verification that docs match as-built product | 🔴 Must Have | IEEE 828 |
| **SCMP** (SCM Plan) | Organization, responsibilities, activities, tools, schedules | 🔴 Must Have | IEEE 828 |
| **Version Description Document** (VDD) | Contents and changes in a specific release | 🔴 Must Have | IEEE 828 |
| **Configuration Status Accounting Reports** | CI status, change traffic, metrics | 🟡 Nice to Have | IEEE 828 |
| **Deviation / Waiver Records** | Approved departures from requirements | 🟡 Nice to Have | IEEE 828 |

### Quality Assurance

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Defect Log / Defect Metrics** | Categorized defect data with trend analysis | 🔴 Must Have | IEEE 1044 |
| **QMS Documentation** (Quality Management System) | Organizational quality policies, processes, procedures | 🔴 Must Have | ISO 9001 |
| **Review Records** (all types) | Inspection, walkthrough, technical review findings | 🔴 Must Have | ISO/IEC 20246 |
| **SQAP** (Software Quality Assurance Plan) | Quality activities, tasks, standards, measures | 🔴 Must Have | IEEE 730 |
| **V&V Plan** (Verification & Validation) | V&V strategy and activities across the life cycle | 🔴 Must Have | IEEE 1012 |
| **Audit Reports** | Compliance assessment against standards and plans | 🟡 Nice to Have | ISO 9001, IEEE 730 |
| **Quality Metrics Dashboard** | Defect density, error density, failure rate, reliability growth | 🟡 Nice to Have | ISO/IEC 25010 (SQuaRE), IEEE 1633 |
| **RCA Reports** (Root Cause Analysis) | Underlying cause identification for significant defects | 🟡 Nice to Have | — |
| **FMEA / FTA Reports** | Failure Mode Effects Analysis; Fault Tree Analysis (safety-critical) | 🟢 Optional | IEC 60812 (FMEA), IEC 61025 (FTA) |
| **Integrity Level Assignments** | Risk-based rigor classification per component | 🟢 Optional | DO-178C (avionics), EN 50128 (railways) |
| **Process Assessment Report** (CMMI / SPICE) | Organizational process maturity evaluation | 🟢 Optional | ISO/IEC 33000 (SPICE), CMMI V2.0 |

### Security

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **SAST Report** (Static Application Security Testing) | Source code vulnerability scan results | 🔴 Must Have | ISO/IEC 27001 |
| **Secure Coding Guidelines** | CERT Top 10, language-specific secure coding rules | 🔴 Must Have | CERT Secure Coding Standards |
| **Security Architecture** | Access control, cryptography, defense-in-depth design | 🔴 Must Have | ISO/IEC 27001 |
| **Security Requirements Specification** | Derived from threat modeling and compliance needs | 🔴 Must Have | ISO/IEC 27001 |
| **Threat Model** | Attack surface analysis: STRIDE, attack trees, misuse cases | 🔴 Must Have | — |
| **Abuse / Misuse Cases** | Negative scenarios: how the system could be attacked | 🟡 Nice to Have | — |
| **DAST Report** (Dynamic Application Security Testing) | Runtime penetration test and fuzzing results | 🟡 Nice to Have | ISO/IEC 27001 |
| **DevSecOps Pipeline Configuration** | Security automation in CI/CD | 🟡 Nice to Have | — |
| **ISMS Documentation** (Information Security Management) | Information security management system | 🟡 Nice to Have | ISO/IEC 27001 |
| **Penetration Test Report** | Controlled attack simulation findings | 🟡 Nice to Have | ISO/IEC 27001 |
| **Vulnerability Assessment** | CVE/CWE/CVSS-catalogued findings with severity scores | 🟡 Nice to Have | CVSS v4.0, CVE, CWE |
| **Common Criteria Evaluation** | Security functionality and assurance evaluation | 🟢 Optional | ISO/IEC 15408 (Common Criteria) |
| **Security Patterns Catalog** | Applied proven security solutions | 🟢 Optional | — |

---

## Life Cycle Models Reference

Per SWEBOK v4, the six generic life cycle stages are:

1. **Concept** → Feasibility study, business case, project charter
2. **Development** → All Requirements through Testing documents above
3. **Production** → Deployment plan, release notes, operations manual
4. **Utilization** → SLA, monitoring dashboards, operational KPIs
5. **Support** → Maintenance plan, MR/PR, impact analysis, incident reports
6. **Retirement** → Data migration plan, archival records, decommissioning report

> **Note:** Priority ratings are default recommendations. Adjust based on: life cycle model (waterfall → more formal; Agile → lighter), domain criticality (safety-critical → upgrade to 🔴), team size/distribution, and contractual/regulatory requirements. The SWEBOK emphasizes that *documentation choice depends on context, not dogma*.

### Key Standards Referenced

| Standard | Title |
|---|---|
| ISO/IEC/IEEE 12207 | Systems and Software Engineering — Software Life Cycle Processes |
| ISO/IEC/IEEE 15288 | Systems and Software Engineering — System Life Cycle Processes |
| ISO/IEC/IEEE 29148 | Systems and Software Engineering — Requirements Engineering |
| ISO/IEC/IEEE 42010 | Systems and Software Engineering — Architecture Description |
| ISO/IEC/IEEE 29119-1/2/3/4 | Software and Systems Engineering — Software Testing |
| ISO/IEC/IEEE 14764 | Software Engineering — Software Life Cycle Processes — Maintenance |
| ISO/IEC/IEEE 32675 | Information Technology — DevOps |
| ISO/IEC/IEEE 20000-1 | Information Technology — Service Management |
| ISO/IEC 25010 | Systems and Software Engineering — SQuaRE (Quality Model) |
| ISO/IEC 19501 | Information Technology — Unified Modeling Language (UML) |
| ISO/IEC 20246 | Software and Systems Engineering — Work Product Reviews |
| ISO/IEC 5230 | Information Technology — OpenChain (Open Source Compliance) |
| ISO/IEC 33000 | Information Technology — Process Assessment (SPICE) |
| ISO/IEC 27001 | Information Security, Cybersecurity and Privacy Protection |
| ISO/IEC 27031 | Information Technology — ICT Readiness for Business Continuity |
| ISO/IEC 15408 | Information Technology — Common Criteria (Security Evaluation) |
| ISO 9001 | Quality Management Systems — Requirements |
| ISO 31000 | Risk Management — Guidelines |
| ISO 9241 | Ergonomics of Human-System Interaction (Usability) |
| IEEE 730 | Software Quality Assurance Processes |
| IEEE 828 | Configuration Management in Systems and Software Engineering |
| IEEE 1012 | System, Software, and Hardware Verification and Validation |
| IEEE 1044 | Standard Classification for Software Anomalies |
| IEEE 1633 | Recommended Practice for Software Reliability |
| IEC 60812 | Failure Modes and Effects Analysis (FMEA) |
| IEC 61025 | Fault Tree Analysis (FTA) |
| IEC 61508 | Functional Safety of Electrical/Electronic/Programmable Electronic Systems |
| DO-178C | Software Considerations in Airborne Systems and Equipment Certification |
| EN 50128 | Railway Applications — Software for Railway Control and Protection |
| ISO/IEC 20926 (IFPUG) | Software and Systems Engineering — Function Point Measurement |
| ISO/IEC 24570 (NESMA) | Software Engineering — Functional Size Measurement |
| CERT | Secure Coding Standards (C, C++, Java, Perl, etc.) |
| CVE / CWE / CVSS | Common Vulnerabilities and Exposures / Weakness Enumeration / Scoring System |

---

## Related

- [[../Software Engineering Note Content|Software Engineering Note Content]] — Original file
- [[PMBOK Essential Documents]] — Project management documents by phase
- [[SEBOK Essential Documents]] — Systems engineering documents by phase
- [[SWEBOK v4 - Overview]] — Full SWEBOK knowledge base
