---
tags:
  - profile
  - large-safety-critical
  - essential-documents
  - software-engineering
---

# Project Profile — Large / Safety-Critical

> **Team:** 50+ members
> **Timeline:** 18+ months
> **Methodology:** V-Model / Waterfall with formal gates
> **Regulatory:** Heavy (aerospace, medical, defense, automotive)
> **Key Principle:** *If it's not documented, it didn't happen.*

---

## When to Use This Profile

Use this profile when your project meets **any** of the following criteria:

- Team size exceeds 50 members or spans multiple organizations
- Timeline extends beyond 18 months with formal milestone gates
- Regulatory or certification requirements apply, including:
  - **Aerospace** — DO-178C (Software Considerations in Airborne Systems and Equipment Certification)
  - **Medical Devices** — IEC 62304 (Medical Device Software — Software Life Cycle Processes)
  - **Automotive** — ISO 26262 (Road Vehicles — Functional Safety)
  - **Defense** — MIL-STD-882, DEF STAN 00-55, NATO AQAP standards
  - **Railway** — EN 50128 (Railway Applications — Software for Railway Control and Protection Systems)
  - **Nuclear** — IEC 61513, IEC 62138
  - **Industrial Automation** — IEC 61508 (Functional Safety of E/E/PE Safety-Related Systems)
- Independent Verification & Validation (IV&V) is mandated
- Safety integrity levels (SIL 1–4 or DAL A–E) must be demonstrated
- Formal audits by certification authorities are expected
- Contractual obligations require extensive deliverable evidence

> [!warning]
> This profile is **comprehensive**. For every document listed, ask: *"Does our regulator, customer, or contract require this?"* Tailor ruthlessly — but never omit what the certification authority expects.

---

## Sources

> This profile draws from six Bodies of Knowledge:
>
> - [[BABOK v3]] — Business Analysis Body of Knowledge
> - [[PMBOK 7]] — Project Management Body of Knowledge
> - [[SWEBOK v4]] — Software Engineering Body of Knowledge
> - [[SEBOK v2]] — Systems Engineering Body of Knowledge (INCOSE)
> - [[DMBOK 2]] — Data Management Body of Knowledge
> - [[CyBOK 1.1]] — Cyber Security Body of Knowledge
> - [[UX UI Essential Documents]] — UX/UI Design Documents
>
> Plus applicable regulatory standards: DO-178C, IEC 62304, ISO 26262, EN 50128, IEC 61508, IEEE 730, IEEE 828, IEEE 1012, IEEE 1044, ISO/IEC/IEEE 12207, ISO/IEC/IEEE 15288, ISO/IEC/IEEE 29119, ISO/IEC/IEEE 29148, ISO/IEC/IEEE 42010.

---

## Priority Legend

| Icon | Priority | Meaning |
|------|----------|---------|
| 🔴 | **Must Have** | Required by regulation, contract, or certification. Absence is a blocking audit finding. |
| 🟡 | **Nice to Have** | Strongly recommended for project success; enhances quality and traceability. |
| 🟢 | **Optional** | Beneficial if resources permit; not typically mandated. |

---

## 1. Business Analysis & Strategy

> **Owner:** BA / Sponsor

| Document | Description | Priority | ISO/IEEE Reference |
|----------|-------------|----------|--------------------|
| Business Case | Justification for investment including ROI, strategic alignment, and regulatory drivers | 🔴 | PMBOK |
| Business Requirements | High-level business needs that the solution must address | 🔴 | BABOK |
| Business Objectives | Measurable goals aligned with organizational strategy | 🔴 | BABOK |
| Current State Description | As-is analysis of existing systems, processes, and capabilities | 🔴 | BABOK |
| Future State Description | Target vision of the solution and desired outcomes | 🔴 | BABOK |
| Change Strategy | Approach for transitioning from current to future state | 🔴 | BABOK |
| Solution Scope | Boundaries of the solution including in-scope and out-of-scope items | 🔴 | BABOK |
| Gap Analysis | Differences between current and future state; drives requirements | 🔴 | BABOK |
| Enterprise Readiness Assessment | Organizational preparedness for change including training and resource needs | 🟡 | BABOK |
| Potential Value | Quantified expected benefits including safety and compliance value | 🔴 | BABOK |
| Risk Analysis Results | Business-level risks including regulatory, safety, and market risks | 🔴 | BABOK |

---

## 2. Concept & Mission Definition

> **Owner:** Systems Engineer / Sponsor

| Document | Description | Priority | ISO/IEEE Reference |
|----------|-------------|----------|--------------------|
| Mission Analysis Report | Operational need statement, mission objectives, and environmental context | 🔴 | SEBOK (ISO/IEC/IEEE 15288) |
| Stakeholder Needs Document | Captured needs and expectations from all stakeholders including regulators | 🔴 | SEBOK |
| Concept of Operations (ConOps) | Operational scenarios, user interactions, and system behavior in the operational environment | 🔴 | SEBOK |
| System Requirements Specification (SyRS) | Top-level system requirements derived from stakeholder needs and mission | 🔴 | SEBOK |
| Stakeholder Register | Identification of all stakeholders, roles, authority, and communication needs | 🔴 | SEBOK |
| Feasibility Study | Technical, economic, and regulatory feasibility assessment | 🟡 | SEBOK |
| Market Analysis / Technology Assessment | Competitive landscape and technology readiness evaluation | 🟡 | SEBOK |

---

## 3. Requirements Engineering

> **Owner:** BA / Systems Engineer

| Document | Description | Priority | ISO/IEEE Reference |
|----------|-------------|----------|--------------------|
| SRS (Software Requirements Specification) | Complete software requirements with functional, interface, and constraint specifications | 🔴 | SWEBOK (ISO/IEC/IEEE 29148) |
| Nonfunctional Requirements Catalog | Performance, reliability, safety, security, and maintainability requirements | 🔴 | SWEBOK (ISO/IEC 25010) |
| Requirements Traceability Matrix | Bidirectional traceability from business needs to tests | 🔴 | SWEBOK/SEBOK (ISO/IEC/IEEE 29148) |
| Requirements Change Log | History of all requirement changes with rationale and impact | 🔴 | SWEBOK |
| Requirements Architecture | Structured organization of requirements by feature, component, and priority | 🔴 | BABOK |
| Stakeholder Analysis | Influence/interest mapping and engagement strategy per stakeholder | 🔴 | SWEBOK |
| User Stories (if Agile components exist) | Lightweight requirement expressions for iterative components | 🟡 | SWEBOK |
| Acceptance Criteria | Measurable conditions that each requirement must satisfy for acceptance | 🔴 | SWEBOK |
| Use Case Specifications | Detailed behavioral specifications with actors, flows, and exceptions | 🟡 | SWEBOK |

---

## 4. Project Management

> **Owner:** PM

| Document | Description | Priority | ISO/IEEE Reference |
|----------|-------------|----------|--------------------|
| Project Charter | Formal authorization, objectives, high-level scope, and PM authority | 🔴 | PMBOK (ISO 21502) |
| Project Management Plan | Baseline plan integrating all subsidiary plans for scope, schedule, cost, quality, risk, and resources | 🔴 | PMBOK |
| WBS + WBS Dictionary | Hierarchical decomposition of deliverables with descriptions and milestones | 🔴 | PMBOK (ISO 21511) |
| Schedule Baseline | Approved project schedule with critical path, milestones, and gate dates | 🔴 | PMBOK |
| Cost Baseline | Approved budget with time-phased expenditure plan and reserves | 🔴 | PMBOK |
| Risk Management Plan | Risk strategy, thresholds, categories, and response planning approach | 🔴 | PMBOK/SEBOK (ISO 31000) |
| Risk Register | Identified risks with probability, impact, owner, and response plan | 🔴 | PMBOK/SEBOK |
| Change Management Plan | Change control board, procedures, and escalation paths | 🔴 | PMBOK |
| Configuration Management Plan | CM policies, tools, baselines, and change control procedures | 🔴 | PMBOK/SEBOK (IEEE 828) |
| Stakeholder Engagement Plan | Strategies for managing stakeholder expectations and communication | 🔴 | PMBOK |
| Communications Management Plan | Reporting cadence, distribution matrix, and escalation protocols | 🔴 | PMBOK |
| Quality Management Plan | Quality standards, assurance activities, and control procedures | 🔴 | PMBOK (ISO 9001) |
| Procurement Management Plan + SOW + Contracts | Procurement strategy, vendor selection, contract terms, and deliverable acceptance | 🔴 | PMBOK |
| RACI Matrix | Responsibility assignment matrix for all project roles and deliverables | 🔴 | PMBOK |
| Earned Value Analysis | Performance measurement integrating scope, schedule, and cost | 🟡 | PMBOK (ISO 21508) |
| Lessons Learned Register | Continuous capture of insights, decisions, and improvement opportunities | 🔴 | PMBOK |

---

## 5. Systems Architecture & Design

> **Owner:** Systems Architect

| Document | Description | Priority | ISO/IEEE Reference |
|----------|-------------|----------|--------------------|
| Functional Architecture | Decomposition of system functions independent of implementation | 🔴 | SEBOK (ISO/IEC/IEEE 15288) |
| Logical Architecture | Logical grouping of components, interfaces, and data flows | 🔴 | SEBOK |
| Physical Architecture | Hardware/software allocation, network topology, and deployment structure | 🔴 | SEBOK |
| System Architecture Description | Comprehensive architectural documentation with rationale and constraints | 🔴 | SEBOK (ISO/IEC/IEEE 42010) |
| Interface Control Document (ICD) | Detailed specification of all internal and external interfaces | 🔴 | SEBOK |
| Trade Study Reports | Alternatives analysis with evaluation criteria, options, and recommendation | 🔴 | SEBOK |
| SAD (Software Architecture Document) | Software-level architecture with components, connectors, and configurations | 🔴 | SWEBOK (ISO/IEC/IEEE 42010) |
| ADR (Architecture Decision Records) | Captured architectural decisions with context, options, and rationale | 🔴 | SWEBOK |
| Architecture Views (4+1) | Logical, process, development, physical, and scenario views | 🔴 | SWEBOK |
| ASR Catalog | Architecturally significant requirements driving design decisions | 🔴 | SWEBOK |
| Architecture Evaluation Report (ATAM) | Formal architecture assessment for quality attribute trade-offs | 🟡 | SWEBOK |
| MBSE Models (SysML) | Model-based systems engineering artifacts for requirements, structure, and behavior | 🟡 | SEBOK |
| System Bill of Materials | Complete listing of hardware, software, and COTS components | 🟡 | SEBOK |

---

## 6. Software Design

> **Owner:** Architect / Sr. Developer

| Document | Description | Priority | ISO/IEEE Reference |
|----------|-------------|----------|--------------------|
| HLD (High-Level Design) | Module decomposition, interfaces, data flow, and technology decisions | 🔴 | SWEBOK |
| LLD (Low-Level Design) | Detailed class, method, and algorithm specifications | 🔴 | SWEBOK |
| API Specification | Contract definitions for all APIs including endpoints, schemas, and error handling | 🔴 | SWEBOK |
| ERD | Entity-relationship diagram showing data structures and relationships | 🔴 | SWEBOK/DMBOK |
| Database Schema (DDL) | Physical database schema with tables, indexes, constraints, and stored procedures | 🔴 | SWEBOK |
| Data Dictionary | Standardized definitions of all data elements, types, and business rules | 🔴 | SWEBOK/DMBOK |
| Design Review Records | Formal review outcomes for HLD and LLD with action items and dispositions | 🔴 | SWEBOK (ISO/IEC 20246) |
| Class Diagrams | Static structure showing classes, attributes, methods, and relationships | 🔴 | SWEBOK |
| Sequence Diagrams | Interaction diagrams showing message flows between objects over time | 🟡 | SWEBOK |
| State Diagrams | Behavioral models showing state transitions and triggering events | 🟡 | SWEBOK |
| Deployment Diagrams | Physical deployment of artifacts on infrastructure nodes | 🟡 | SWEBOK |
| Design Rationale | Justification for design choices including trade-offs and constraints considered | 🟡 | SWEBOK |

---

## 6b. UX/UI Design

> **Owner:** UX Researcher / UI Designer

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| User Personas | Fictional characters representing user segments with goals, behaviors, frustrations | 🔴 | ISO 9241-210 |
| User Interview Script | Structured questions for user interviews | 🔴 | ISO 9241-210 |
| Journey Map | Visual timeline of user experience with pain points and opportunities | 🔴 | ISO 9241-210 |
| Information Architecture (IA) | Content structure, hierarchy, navigation model | 🔴 | ISO 9241-110 |
| Sitemap | Visual map of all pages/sections | 🔴 | — |
| User Flows | Step-by-step paths users take to complete tasks | 🔴 | ISO 9241-210 |
| Wireframes (Low-fi) | Basic page layouts showing structure and content placement | 🔴 | — |
| Wireframes (Mid-fi) | More detailed wireframes with real content | 🔴 | — |
| Interactive Prototype | Clickable wireframes for user testing | 🔴 | ISO 9241-210 |
| UI Mockups | High-fidelity screen designs with final visual design | 🔴 | — |
| Component Library | Reusable UI components with specifications | 🔴 | — |
| Design System | Complete system: tokens, components, patterns, guidelines | 🔴 | — |
| Responsive Specifications | Desktop, tablet, mobile layouts | 🔴 | ISO 9241-112 |
| State Variations | Hover, active, disabled, error, loading states | 🔴 | — |
| Accessibility Audit | WCAG compliance check | 🔴 | ISO/IEC 40500 (WCAG 2.0) |
| Usability Test Plan | Tasks, participants, metrics, schedule | 🔴 | ISO 9241-11 |
| Usability Test Report | Findings, severity ratings, recommendations | 🔴 | ISO 9241-11 |
| Design Specifications | Measurements, colors, typography for developers | 🔴 | — |
| Interaction Specifications | Animations, transitions, micro-interactions | 🟡 | — |
| Design Tokens | CSS variables for colors, spacing, typography | 🟡 | — |

---

## 7. Construction

> **Owner:** Developer

| Document | Description | Priority | ISO/IEEE Reference |
|----------|-------------|----------|--------------------|
| Source Code | Implementation artifacts under version control with review gates | 🔴 | SWEBOK |
| README / Developer Guide | Setup instructions, architecture overview, and contribution guidelines | 🔴 | SWEBOK |
| Unit Test Code | Automated unit tests with coverage targets per safety level | 🔴 | SWEBOK |
| Integration Test Code | Automated integration tests verifying component interactions | 🔴 | SWEBOK |
| Build Scripts | Reproducible, automated build processes with environment specification | 🔴 | SWEBOK |
| Dependency Manifest | Complete listing of direct and transitive dependencies with versions | 🔴 | SWEBOK |
| SBOM | Software Bill of Materials for supply chain transparency and vulnerability tracking | 🔴 | SWEBOK (ISO/IEC 5230) |
| API Documentation | Auto-generated or maintained API reference with examples | 🔴 | SWEBOK |
| Coding Standards / Style Guide | Enforced coding conventions including safety-critical rules (MISRA, CERT) | 🔴 | SWEBOK (CERT) |
| Code Review Records | Formal peer review records with findings, dispositions, and sign-off | 🔴 | SWEBOK (ISO/IEC 20246) |
| Commit Messages / Changelog | Traceable change history linking commits to requirements and issues | 🔴 | SWEBOK |
| Static Analysis Reports | Tool-generated reports on code quality, complexity, and violations | 🔴 | SWEBOK |
| TDD Test Cases | Test-driven development test artifacts with red-green-refactor evidence | 🟡 | SWEBOK |

---

## 8. Testing & Verification

> **Owner:** QA / V&V Engineer

| Document | Description | Priority | ISO/IEEE Reference |
|----------|-------------|----------|--------------------|
| Test Plan | Master test plan covering scope, approach, resources, schedule, and pass/fail criteria | 🔴 | SWEBOK (ISO/IEC/IEEE 29119) |
| Test Strategy | Organization-level testing approach aligned with safety and quality objectives | 🔴 | SWEBOK |
| Test Cases | Detailed test cases with preconditions, steps, expected results, and traceability | 🔴 | SWEBOK |
| Test Suite | Grouped test cases organized by feature, level, or risk | 🔴 | SWEBOK |
| Test Data | Test data sets including boundary, negative, and safety-critical scenarios | 🔴 | SWEBOK |
| Test Scripts (automated) | Executable automated test scripts with environment setup | 🔴 | SWEBOK |
| Defect Report | Structured defect lifecycle tracking from discovery to resolution | 🔴 | SWEBOK (IEEE 1044) |
| Regression Test Suite | Curated test suite for validating unchanged functionality after changes | 🔴 | SWEBOK |
| Traceability Matrix (Req ↔ Tests) | Bidirectional mapping between requirements and test cases proving coverage | 🔴 | SWEBOK |
| Test Report | Aggregated test execution results with metrics and trend analysis | 🔴 | SWEBOK |
| Test Completion Report | Final test summary with exit criteria assessment and residual risk | 🔴 | SWEBOK |
| UAT Sign-off | Formal stakeholder acceptance of tested deliverables | 🔴 | SWEBOK |
| Coverage Report | Code coverage (statement, branch, MC/DC) per safety level requirements | 🔴 | SWEBOK |
| Performance Test Report | Load, stress, and scalability test results against NFR targets | 🔴 | SWEBOK |
| Security Test Report | Vulnerability assessment, penetration testing, and security validation results | 🔴 | SWEBOK |
| Usability Test Report | User-centered evaluation of interface effectiveness and learnability | 🟡 | SWEBOK |
| Verification Plan | Systematic verification approach mapping requirements to methods (test, analysis, inspection, demonstration) | 🔴 | SEBOK (ISO/IEC/IEEE 15288) |
| Verification Reports | Results of all verification activities with evidence and disposition | 🔴 | SEBOK |
| Validation Plan | Validation approach ensuring the system meets intended use in the operational environment | 🔴 | SEBOK |
| Validation Reports | Validation results with stakeholder confirmation of operational suitability | 🔴 | SEBOK |

---

## 9. Security

> **Owner:** Security Engineer / CISO

| Document | Description | Priority | ISO/IEEE Reference |
|----------|-------------|----------|--------------------|
| ISMS Documentation | Information Security Management System framework and policies | 🔴 | CyBOK (ISO/IEC 27001) |
| Security Policy | Organizational security directives, responsibilities, and enforcement | 🔴 | CyBOK |
| Risk Assessment Report | Systematic identification and evaluation of security risks | 🔴 | CyBOK (ISO/IEC 27005) |
| Risk Treatment Plan | Selected controls and implementation plan for identified security risks | 🔴 | CyBOK |
| Threat Model | Structured analysis of attack surfaces, threat actors, and attack vectors | 🔴 | CyBOK |
| Security Requirements Specification | Security-specific requirements derived from risk assessment and policy | 🔴 | CyBOK |
| Secure Design Review Report | Architecture and design review focused on security principles | 🔴 | CyBOK |
| Secure Coding Guidelines | Security-specific coding rules beyond general standards (OWASP, CERT) | 🔴 | CyBOK (CERT, OWASP) |
| SAST Report | Static Application Security Testing findings and remediation status | 🔴 | CyBOK |
| SCA Report | Software Composition Analysis of third-party and open-source components | 🔴 | CyBOK |
| DAST Report | Dynamic Application Security Testing results from runtime analysis | 🔴 | CyBOK |
| Penetration Test Report | Authorized simulated attack results with findings and risk ratings | 🔴 | CyBOK |
| SSDLC Process Documentation | Secure Software Development Lifecycle integrated into the development process | 🔴 | CyBOK (ISO/IEC 27034) |
| Access Control Policy | Role-based access control definitions, least privilege, and review cadence | 🔴 | CyBOK |
| Authentication Standard | Authentication mechanisms, MFA requirements, and session management | 🔴 | CyBOK |
| Incident Response Plan | Procedures for detecting, containing, eradicating, and recovering from security incidents | 🔴 | CyBOK (ISO/IEC 27035) |
| Business Continuity Plan (BCP) | Strategies for maintaining critical functions during and after a disruption | 🔴 | CyBOK |
| Vulnerability Management Report | Ongoing vulnerability scanning, prioritization, and remediation tracking | 🔴 | CyBOK |
| Compliance Assessment Report | Mapping of security controls to regulatory and contractual requirements | 🔴 | CyBOK |
| Network Security Architecture | Network segmentation, firewall rules, encryption in transit, and monitoring | 🔴 | CyBOK |
| DevSecOps Pipeline Configuration | Security tooling integrated into CI/CD (SAST, SCA, DAST, secrets scanning) | 🔴 | CyBOK |
| Security Metrics Dashboard | KPIs for vulnerability density, mean time to remediate, and compliance posture | 🔴 | CyBOK |
| Digital Forensics Report | Post-incident forensic analysis and evidence preservation (if incidents occur) | 🟡 | CyBOK |
| Adversary Emulation Plan | Red team scenario planning based on relevant threat intelligence | 🟡 | CyBOK |

---

## 10. Data Management

> **Owner:** Data Architect / DBA

| Document | Description | Priority | ISO/IEEE Reference |
|----------|-------------|----------|--------------------|
| Data Governance Charter | Authority, roles, policies, and decision rights for data management | 🔴 | DMBOK (ISO/IEC 38505) |
| Enterprise Data Model (EDM) | Organization-wide conceptual data model showing key entities and relationships | 🔴 | DMBOK |
| Data Architecture Blueprint | Data storage, integration, and flow architecture across the system | 🔴 | DMBOK |
| Data Flow Diagram | Visual representation of how data moves through the system | 🔴 | DMBOK |
| Data Lineage Documentation | End-to-end tracking of data from origin through transformations to consumption | 🔴 | DMBOK |
| Conceptual Data Model (CDM) | High-level business-oriented data model independent of technology | 🔴 | DMBOK |
| Logical Data Model (LDM) | Normalized data structures with entities, attributes, and relationships | 🔴 | DMBOK |
| Physical Data Model (PDM) | Technology-specific data model with storage, indexing, and partitioning | 🔴 | DMBOK |
| Data Dictionary | Standardized definitions of all data elements, formats, and business rules | 🔴 | DMBOK (ISO/IEC 11179) |
| Data Classification Schema | Classification levels (public, internal, confidential, restricted) with handling rules | 🔴 | DMBOK (ISO/IEC 27001) |
| Data Encryption Standards | Encryption algorithms, key management, and data-at-rest/in-transit policies | 🔴 | DMBOK |
| Data Quality Rules + Scorecard | Defined quality dimensions, measurement rules, and quality metrics | 🔴 | DMBOK (ISO 8000) |
| Backup & Recovery Plan | Backup frequency, retention, recovery procedures, and RTO/RPO targets | 🔴 | DMBOK |
| Data Retention & Archival Policy | Retention periods, archival procedures, and legal hold processes | 🔴 | DMBOK |
| Data Breach Response Plan | Procedures for data breach notification, containment, and regulatory reporting | 🔴 | DMBOK |
| Privacy Impact Assessment | Evaluation of data processing impact on personal data privacy | 🔴 | DMBOK (GDPR) |
| Data Integration Architecture | ETL/ELT patterns, data pipeline design, and synchronization strategy | 🟡 | DMBOK |
| Metadata Repository | Centralized catalog of data definitions, lineage, and governance metadata | 🟡 | DMBOK |

---

## 11. Deployment & Operations

> **Owner:** DevOps / SRE

| Document | Description | Priority | ISO/IEEE Reference |
|----------|-------------|----------|--------------------|
| CI/CD Pipeline Configuration | Automated build, test, and deployment pipeline with quality gates | 🔴 | SWEBOK |
| Deployment Plan | Step-by-step deployment procedures with rollback triggers and verification | 🔴 | SWEBOK |
| Rollback Plan | Procedures for reverting to the previous known-good state | 🔴 | SWEBOK |
| SLA | Service Level Agreements defining availability, performance, and support commitments | 🔴 | SWEBOK |
| Operations Manual / Runbook | Operational procedures for monitoring, troubleshooting, and incident handling | 🔴 | SWEBOK |
| Release Notes | Documented changes, known issues, and upgrade instructions per release | 🔴 | SWEBOK |
| Incident Management Process | Incident classification, escalation, resolution, and post-mortem procedures | 🔴 | SWEBOK |
| Disaster Recovery Plan | Procedures for restoring service after catastrophic failure | 🔴 | SWEBOK |
| Capacity Plan | Resource forecasting and scaling strategy for projected growth | 🟡 | SWEBOK |
| Monitoring Dashboard | Real-time system health, performance, and alert visualization | 🟡 | SWEBOK |
| Container Configurations | Container definitions, orchestration specs, and resource limits | 🟡 | SWEBOK |
| Infrastructure-as-Code | Version-controlled infrastructure definitions (Terraform, CloudFormation, etc.) | 🟡 | SWEBOK |
| SLO/SLI Definitions | Service Level Objectives and Indicators for reliability measurement | 🟡 | SWEBOK |
| Operational KPIs Report | Periodic operational metrics including uptime, MTTR, and change failure rate | 🟡 | SWEBOK |

---

## 12. Maintenance & Support

> **Owner:** Support / Maintainer

| Document | Description | Priority | ISO/IEEE Reference |
|----------|-------------|----------|--------------------|
| Maintenance Plan | Approach for corrective, adaptive, perfective, and preventive maintenance | 🔴 | SWEBOK (ISO/IEC/IEEE 14764) |
| Impact Analysis Report | Assessment of change impact on existing functionality, safety, and performance | 🔴 | SWEBOK |
| MR/PR (Modification Request) | Formal change requests with justification, scope, and priority | 🔴 | SWEBOK |
| Maintenance Log / Change History | Chronological record of all maintenance activities and changes | 🔴 | SWEBOK |
| Technical Debt Register | Identified technical debt with severity, remediation plan, and cost of delay | 🟡 | SWEBOK |
| SLA Compliance Report | Periodic assessment of SLA adherence with trend analysis | 🟡 | SWEBOK |

---

## 13. Quality Assurance

> **Owner:** QA Lead

| Document | Description | Priority | ISO/IEEE Reference |
|----------|-------------|----------|--------------------|
| SQAP (Software Quality Assurance Plan) | QA activities, responsibilities, standards, and reporting for the project | 🔴 | SWEBOK (IEEE 730) |
| V&V Plan | Verification and Validation plan with scope, methods, resources, and schedule | 🔴 | SWEBOK (IEEE 1012) |
| Review Records | Formal review records for all artifact inspections (requirements, design, code, tests) | 🔴 | SWEBOK (ISO/IEC 20246) |
| Defect Log / Metrics | Defect tracking with classification, severity, root cause, and trend metrics | 🔴 | SWEBOK (IEEE 1044) |
| QMS Documentation | Quality Management System documentation per ISO 9001 framework | 🔴 | SWEBOK (ISO 9001) |
| Audit Reports | Internal and external audit findings, nonconformances, and corrective actions | 🟡 | SWEBOK |
| Quality Metrics Dashboard | Dashboards for defect density, review effectiveness, and process compliance | 🟡 | SWEBOK |
| RCA Reports | Root Cause Analysis for significant defects, failures, and process deviations | 🟡 | SWEBOK |
| FMEA / FTA Reports | Failure Mode and Effects Analysis / Fault Tree Analysis for safety assessment | 🟡 | SEBOK (IEC 60812/61025) |

---

## 14. Configuration Management

> **Owner:** Config Manager

| Document | Description | Priority | ISO/IEEE Reference |
|----------|-------------|----------|--------------------|
| SCMP (Software Configuration Management Plan) | CM strategy, tools, procedures, and responsibilities | 🔴 | SWEBOK (IEEE 828) |
| Baseline Records | Documented baselines (functional, allocated, product) with approved contents | 🔴 | SWEBOK |
| Change Request (CR/SCR) | Formal change request lifecycle with impact analysis and approval workflow | 🔴 | SWEBOK |
| FCA Report (Functional Configuration Audit) | Verification that the deliverable meets its functional requirements | 🔴 | SWEBOK |
| PCA Report (Physical Configuration Audit) | Verification that the as-built product matches its technical documentation | 🔴 | SWEBOK |
| Version Description Document | Release identification, contents, known issues, and installation instructions | 🔴 | SWEBOK |
| Configuration Status Accounting Reports | Historical record of all configuration items, baselines, and changes | 🟡 | SWEBOK |

---

## 15. Systems Engineering Cross-Cutting

> **Owner:** Systems Engineer

| Document | Description | Priority | ISO/IEEE Reference |
|----------|-------------|----------|--------------------|
| SEMP (SE Management Plan) | Systems engineering approach, processes, and technical management activities | 🔴 | SEBOK (ISO/IEC/IEEE 15288) |
| Implementation Plan | Strategy for constructing, coding, or fabricating the system elements | 🔴 | SEBOK |
| Integration Plan + Reports | Planned and actual integration sequence, procedures, and results | 🔴 | SEBOK |
| Transition Plan | Strategy for transitioning the system from development to operations | 🔴 | SEBOK |
| Technical Review Records (SRR, PDR, CDR, TRR) | Formal review gate outcomes with action items and maturity assessments | 🔴 | SEBOK |
| Technical Performance Measures (TPMs) | Tracked key technical parameters against allocated budgets and thresholds | 🔴 | SEBOK |
| System Safety Plan / Safety Case | Safety strategy, evidence, and argument that the system is acceptably safe | 🔴 | SEBOK (IEC 61508) |
| Hazard Analysis (PHA, SHA, SSHA) | Preliminary, System, and Subsystem Hazard Analysis with risk levels and mitigations | 🔴 | SEBOK |
| Standards Compliance Matrix | Mapping of project practices and deliverables to applicable standards | 🔴 | SEBOK |
| Configuration Baselines | SE-controlled baselines for requirements, design, and product configuration | 🔴 | SEBOK |
| System Disposal / Retirement Plan | End-of-life decommissioning, data migration, and environmental considerations | 🔴 | SEBOK |
| Capability Upgrade Plan | Roadmap for future capability increments and technology refresh | 🟡 | SEBOK |

---

## 16. Domain-Specific (apply as relevant)

> **Owner:** Varies

| Document | Description | Priority | ISO/IEEE Reference |
|----------|-------------|----------|--------------------|
| Software Development Plan | Tailored SDLC plan for software-intensive systems with lifecycle stage definitions | 🔴 (SW-intensive) | SEBOK (ISO/IEC/IEEE 12207) |
| Software Assurance Plan | Assurance activities specific to safety-critical software (MC/DC coverage, independence) | 🔴 (safety-critical SW) | SEBOK (DO-178C, IEC 62304) |
| Medical Device File | Complete technical documentation per medical device regulatory requirements | 🔴 (medical) | SEBOK (ISO 13485) |
| Security Accreditation Package | Authority to Operate (ATO) documentation for government/defense systems | 🔴 (gov/defense) | SEBOK (NIST SP 800-37) |

---

## Quick-Start Checklist

> [!tip]
> Print this checklist. Check off each 🔴 document as it's produced. This is your certification readiness tracker.

| Phase                            | Document                                      | ✓   |
| -------------------------------- | --------------------------------------------- | --- |
| **1. Business Analysis**         | Business Case                                 | ☐   |
|                                  | Business Requirements                         | ☐   |
|                                  | Business Objectives                           | ☐   |
|                                  | Current State Description                     | ☐   |
|                                  | Future State Description                      | ☐   |
|                                  | Change Strategy                               | ☐   |
|                                  | Solution Scope                                | ☐   |
|                                  | Gap Analysis                                  | ☐   |
|                                  | Potential Value                               | ☐   |
|                                  | Risk Analysis Results                         | ☐   |
| **2. Concept & Mission**         | Mission Analysis Report                       | ☐   |
|                                  | Stakeholder Needs Document                    | ☐   |
|                                  | Concept of Operations (ConOps)                | ☐   |
|                                  | System Requirements Specification (SyRS)      | ☐   |
|                                  | Stakeholder Register                          | ☐   |
| **3. Requirements**              | SRS (Software Requirements Specification)     | ☐   |
|                                  | Nonfunctional Requirements Catalog            | ☐   |
|                                  | Requirements Traceability Matrix              | ☐   |
|                                  | Requirements Change Log                       | ☐   |
|                                  | Requirements Architecture                     | ☐   |
|                                  | Stakeholder Analysis                          | ☐   |
|                                  | Acceptance Criteria                           | ☐   |
| **4. Project Management**        | Project Charter                               | ☐   |
|                                  | Project Management Plan                       | ☐   |
|                                  | WBS + WBS Dictionary                          | ☐   |
|                                  | Schedule Baseline                             | ☐   |
|                                  | Cost Baseline                                 | ☐   |
|                                  | Risk Management Plan                          | ☐   |
|                                  | Risk Register                                 | ☐   |
|                                  | Change Management Plan                        | ☐   |
|                                  | Configuration Management Plan                 | ☐   |
|                                  | Stakeholder Engagement Plan                   | ☐   |
|                                  | Communications Management Plan                | ☐   |
|                                  | Quality Management Plan                       | ☐   |
|                                  | Procurement Management Plan + SOW + Contracts | ☐   |
|                                  | RACI Matrix                                   | ☐   |
|                                  | Lessons Learned Register                      | ☐   |
| **5. Systems Architecture**      | Functional Architecture                       | ☐   |
|                                  | Logical Architecture                          | ☐   |
|                                  | Physical Architecture                         | ☐   |
|                                  | System Architecture Description               | ☐   |
|                                  | Interface Control Document (ICD)              | ☐   |
|                                  | Trade Study Reports                           | ☐   |
|                                  | SAD (Software Architecture Document)          | ☐   |
|                                  | ADR (Architecture Decision Records)           | ☐   |
|                                  | Architecture Views (4+1)                      | ☐   |
|                                  | ASR Catalog                                   | ☐   |
| **6. Software Design**           | HLD (High-Level Design)                       | ☐   |
|                                  | LLD (Low-Level Design)                        | ☐   |
|                                  | API Specification                             | ☐   |
|                                  | ERD                                           | ☐   |
|                                  | Database Schema (DDL)                         | ☐   |
|                                  | Data Dictionary                               | ☐   |
|                                  | Design Review Records                         | ☐   |
|                                  | Class Diagrams                                | ☐   |
| **6b. UX/UI Design**             | User Personas                                 | ☐   |
|                                  | Journey Map                                   | ☐   |
|                                  | Information Architecture (IA)                 | ☐   |
|                                  | Wireframes (Low-fi)                           | ☐   |
|                                  | Wireframes (Mid-fi)                           | ☐   |
|                                  | Interactive Prototype                         | ☐   |
|                                  | UI Mockups                                    | ☐   |
|                                  | Component Library                             | ☐   |
|                                  | Design System                                 | ☐   |
|                                  | Responsive Specifications                     | ☐   |
|                                  | Accessibility Audit                           | ☐   |
|                                  | Usability Test Plan                           | ☐   |
|                                  | Usability Test Report                         | ☐   |
|                                  | Design Specifications                         | ☐   |
| **7. Construction**              | Source Code                                   | ☐   |
|                                  | README / Developer Guide                      | ☐   |
|                                  | Unit Test Code                                | ☐   |
|                                  | Integration Test Code                         | ☐   |
|                                  | Build Scripts                                 | ☐   |
|                                  | Dependency Manifest                           | ☐   |
|                                  | SBOM                                          | ☐   |
|                                  | API Documentation                             | ☐   |
|                                  | Coding Standards / Style Guide                | ☐   |
|                                  | Code Review Records                           | ☐   |
|                                  | Commit Messages / Changelog                   | ☐   |
|                                  | Static Analysis Reports                       | ☐   |
| **8. Testing & Verification**    | Test Plan                                     | ☐   |
|                                  | Test Strategy                                 | ☐   |
|                                  | Test Cases                                    | ☐   |
|                                  | Test Suite                                    | ☐   |
|                                  | Test Data                                     | ☐   |
|                                  | Test Scripts (automated)                      | ☐   |
|                                  | Defect Report                                 | ☐   |
|                                  | Regression Test Suite                         | ☐   |
|                                  | Traceability Matrix (Req ↔ Tests)             | ☐   |
|                                  | Test Report                                   | ☐   |
|                                  | Test Completion Report                        | ☐   |
|                                  | UAT Sign-off                                  | ☐   |
|                                  | Coverage Report                               | ☐   |
|                                  | Performance Test Report                       | ☐   |
|                                  | Security Test Report                          | ☐   |
|                                  | Verification Plan                             | ☐   |
|                                  | Verification Reports                          | ☐   |
|                                  | Validation Plan                               | ☐   |
|                                  | Validation Reports                            | ☐   |
| **9. Security**                  | ISMS Documentation                            | ☐   |
|                                  | Security Policy                               | ☐   |
|                                  | Risk Assessment Report                        | ☐   |
|                                  | Risk Treatment Plan                           | ☐   |
|                                  | Threat Model                                  | ☐   |
|                                  | Security Requirements Specification           | ☐   |
|                                  | Secure Design Review Report                   | ☐   |
|                                  | Secure Coding Guidelines                      | ☐   |
|                                  | SAST Report                                   | ☐   |
|                                  | SCA Report                                    | ☐   |
|                                  | DAST Report                                   | ☐   |
|                                  | Penetration Test Report                       | ☐   |
|                                  | SSDLC Process Documentation                   | ☐   |
|                                  | Access Control Policy                         | ☐   |
|                                  | Authentication Standard                       | ☐   |
|                                  | Incident Response Plan                        | ☐   |
|                                  | Business Continuity Plan (BCP)                | ☐   |
|                                  | Vulnerability Management Report               | ☐   |
|                                  | Compliance Assessment Report                  | ☐   |
|                                  | Network Security Architecture                 | ☐   |
|                                  | DevSecOps Pipeline Configuration              | ☐   |
|                                  | Security Metrics Dashboard                    | ☐   |
| **10. Data Management**          | Data Governance Charter                       | ☐   |
|                                  | Enterprise Data Model (EDM)                   | ☐   |
|                                  | Data Architecture Blueprint                   | ☐   |
|                                  | Data Flow Diagram                             | ☐   |
|                                  | Data Lineage Documentation                    | ☐   |
|                                  | Conceptual Data Model (CDM)                   | ☐   |
|                                  | Logical Data Model (LDM)                      | ☐   |
|                                  | Physical Data Model (PDM)                     | ☐   |
|                                  | Data Dictionary                               | ☐   |
|                                  | Data Classification Schema                    | ☐   |
|                                  | Data Encryption Standards                     | ☐   |
|                                  | Data Quality Rules + Scorecard                | ☐   |
|                                  | Backup & Recovery Plan                        | ☐   |
|                                  | Data Retention & Archival Policy              | ☐   |
|                                  | Data Breach Response Plan                     | ☐   |
|                                  | Privacy Impact Assessment                     | ☐   |
| **11. Deployment & Operations**  | CI/CD Pipeline Configuration                  | ☐   |
|                                  | Deployment Plan                               | ☐   |
|                                  | Rollback Plan                                 | ☐   |
|                                  | SLA                                           | ☐   |
|                                  | Operations Manual / Runbook                   | ☐   |
|                                  | Release Notes                                 | ☐   |
|                                  | Incident Management Process                   | ☐   |
|                                  | Disaster Recovery Plan                        | ☐   |
| **12. Maintenance & Support**    | Maintenance Plan                              | ☐   |
|                                  | Impact Analysis Report                        | ☐   |
|                                  | MR/PR (Modification Request)                  | ☐   |
|                                  | Maintenance Log / Change History              | ☐   |
| **13. Quality Assurance**        | SQAP                                          | ☐   |
|                                  | V&V Plan                                      | ☐   |
|                                  | Review Records                                | ☐   |
|                                  | Defect Log / Metrics                          | ☐   |
|                                  | QMS Documentation                             | ☐   |
| **14. Configuration Management** | SCMP                                          | ☐   |
|                                  | Baseline Records                              | ☐   |
|                                  | Change Request (CR/SCR)                       | ☐   |
|                                  | FCA Report                                    | ☐   |
|                                  | PCA Report                                    | ☐   |
|                                  | Version Description Document                  | ☐   |
| **15. Systems Engineering**      | SEMP                                          | ☐   |
|                                  | Implementation Plan                           | ☐   |
|                                  | Integration Plan + Reports                    | ☐   |
|                                  | Transition Plan                               | ☐   |
|                                  | Technical Review Records (SRR, PDR, CDR, TRR) | ☐   |
|                                  | Technical Performance Measures (TPMs)         | ☐   |
|                                  | System Safety Plan / Safety Case              | ☐   |
|                                  | Hazard Analysis (PHA, SHA, SSHA)              | ☐   |
|                                  | Standards Compliance Matrix                   | ☐   |
|                                  | Configuration Baselines                       | ☐   |
|                                  | System Disposal / Retirement Plan             | ☐   |
| **16. Domain-Specific**          | Software Development Plan                     | ☐   |
|                                  | Software Assurance Plan                       | ☐   |
|                                  | Medical Device File                           | ☐   |
|                                  | Security Accreditation Package                | ☐   |

---

## Related

- [[BABOK Essential Documents]] — Business Analysis Body of Knowledge
- [[PMBOK Essential Documents]] — Project Management Body of Knowledge
- [[SWEBOK Essential Documents]] — Software Engineering Body of Knowledge
- [[SEBOK Essential Documents]] — Systems Engineering Body of Knowledge
- [[DMBOK Essential Documents]] — Data Management Body of Knowledge
- [[CyBOK Essential Documents]] — Cyber Security Body of Knowledge
- [[Profile-Small-Startup]] — Essential documents for small/startup teams
- [[Profile-Medium-Enterprise]] — Essential documents for medium enterprise projects
- [[Essential Documents - Overview]] — Master overview of all essential document profiles
