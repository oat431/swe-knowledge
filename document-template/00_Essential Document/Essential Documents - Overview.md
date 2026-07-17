---
tags: [overview, essential-documents, software-engineering, project-management, systems-engineering, business-analysis, cyber-security, data-management, profile]
---

# Essential Documents — Overview

> **Purpose:** A practical, phase-organized checklist of documents you need to produce across seven disciplines — Business Analysis, Software Engineering, Project Management, Systems Engineering, Cyber Security, Data Management, and UX/UI Design.
> Each checklist is extracted from its respective Body of Knowledge and links back to the full reference for deeper study.

## What Is This?

This folder contains **document-only extracts** from six major Bodies of Knowledge:

| Discipline | Source | File | Focus |
|---|---|---|---|
| 📊 **Business Analysis** | BABOK® Guide v3 (IIBA) | [[BABOK Essential Documents]] | BA workflow: Planning → Strategy → Elicitation → Analysis & Design → Life Cycle Mgmt → Solution Evaluation |
| 💻 **Software Engineering** | SWEBOK v4 (IEEE) | [[SWEBOK Essential Documents]] | SDLC phases: Requirements → Architecture → Design → Construction → Testing → DevOps → Maintenance |
| 📋 **Project Management** | PMBOK® Guide v8 (PMI) | [[PMBOK Essential Documents]] | Five Focus Areas: Initiating → Planning → Executing → M&C → Closing |
| ⚙️ **Systems Engineering** | SEBoK v2 (BKCASE) | [[SEBOK Essential Documents]] | SE Life Cycle: Concept → Architecture → Realization → Operations → Maintenance |
| 🔒 **Cyber Security** | CyBOK v1.1 (NCSC) | [[CyBOK Essential Documents]] | Security domains: Governance → Threat Intel → SecOps → AppSec → Infrastructure → Advanced |
| 🗄️ **Data Management** | DMBoK v2 (DAMA) | [[DMBOK Essential Documents]] | Data domains: Governance → Architecture → Modeling → Storage → Integration → Quality |
| 🎨 **UX/UI Design** | HCI / Industry Standards | [[UX UI Essential Documents]] | Design phases: Research → UX Design → UI Design → Testing → Handoff |

> 🔒🗄️ **CyBOK and DMBOK fill the cross-cutting concerns** that SWEBOK touches on but doesn't fully cover. CyBOK provides the full security program documentation (from risk governance to incident response), while DMBOK covers the complete data management lifecycle (from governance to quality).

Each checklist follows the same format:
- 🔴🟡🟢 **Priority legend** — Must Have / Nice to Have / Optional
- **Section-organized tables** with document name, description, priority, and ISO/IEEE reference
- **Owner blockquote** under each section header indicating primary responsibility
- ⚠️ **Link-back banner** pointing to the full Body of Knowledge

## Quick Comparison

| Stage | BABOK (Business) | SWEBOK (Software) | PMBOK (Project) | SEBOK (Systems) | CyBOK (Security) | DMBOK (Data) |
|---|---|---|---|---|---|---|
| **Start** | Current State, Business Req, Objectives, Future State, Change Strategy | BRD, SRS, User Stories, Acceptance Criteria | Business Case, Project Charter, Stakeholder Register | Mission Analysis, ConOps, Stakeholder Needs, SyRS | Risk Assessment, Security Policy, ISMS, Threat Model | DG Charter, Data Strategy, Enterprise Data Model, Business Glossary |
| **Design** | Requirements Architecture, Design Options, Solution Recommendation | SAD, ADR, Architecture Views, API Specs, HLD/LLD | PM Plan, WBS, Schedule, Budget, Risk Register | Functional/Logical/Physical Architecture, ICD, Trade Studies | Secure Design Review, Secure Coding Guidelines, Access Control Policy, Network Security Architecture | CDM, LDM, PDM, Dimensional Model, Data Dictionary, Integration Architecture |
| **Build** | Requirements (verified/validated/approved) | Source Code, Unit Tests, Build Scripts, Code Reviews | Team Assignments, Issue Log, Change Requests, Lessons Learned | Implementation Plan, Integration Plan, Verification Plan | SAST/SCA Reports, DevSecOps Pipeline, SIEM Rules, IDPS Signatures | ETL/ELT Specs, Golden Record Rules, DQ Rules, Metadata Repository |
| **Test** | Solution Performance Measures, Performance Analysis | Test Plan, Test Cases, Defect Reports, UAT Sign-off | Work Performance Reports, Variance Analysis, EVM | Verification Reports, Validation Reports, Audit Reports | DAST Report, Penetration Test, Adversary Emulation, Vulnerability Assessment | Data Profiling Report, DQ Scorecard, Data Cleansing Spec |
| **Ship** | Solution/Enterprise Limitations, Recommended Actions | Deployment Plan, CI/CD, SLA, Runbook, Release Notes | Final Report, Project Closure, Verified Deliverables | Transition Plan, Operations Manual, Maintenance Plan | Incident Response Plan, SOC Runbook, BCP, SOAR Playbooks | Backup/Recovery Plan, Capacity Plan, Data Breach Response Plan |
| **Cross** | BA Approach, Governance, Stakeholder Engagement, Info Mgmt | QMS, SQAP, SCMP, Baseline Records, Threat Model | Change Mgmt Plan, Config Mgmt Plan, Procurement Docs | SEMP, Safety Case, Security Plan, Standards Matrix | Compliance Register, Security Metrics, CTI Reports, Forensic Reports | DG Scorecard, Data Lineage, Data Retention Policy, DQ Issue Log |

## Which Checklist Do I Need?

- **Analyzing business needs and defining value?** → Start with [[BABOK Essential Documents]]
- **Building software?** → Start with [[SWEBOK Essential Documents]]
- **Managing a project?** → Start with [[PMBOK Essential Documents]]
- **Engineering a system (hardware + software + people)?** → Start with [[SEBOK Essential Documents]]
- **Securing an organization or system?** → Start with [[CyBOK Essential Documents]]
- **Managing data as an asset?** → Start with [[DMBOK Essential Documents]]
- **Choosing a development methodology?** → See [[Software Methodology - Overview|Software Methodology Overview]]
- **Designing user interfaces?** → See [[Human Computer Interaction Overview|Human Computer Interaction]]
- **All six?** → Each addresses a different dimension. A comprehensive software project typically needs BABOK + SWEBOK + PMBOK. Add CyBOK for security programs and DMBOK for data-intensive systems.

## Project-Type Profiles

Not sure how much documentation your project actually needs? These profiles pre-select and prioritize documents from all six BOKs based on project size and complexity:

| Profile | Team | Timeline | Methodology | Documents | Focus |
|---|---|---|---|---|---|
| 🚀 [[Profile-Small-Startup]] | 1–5 devs | Weeks–months | Agile / Lean | ~30 | Ship fast — just enough docs to not get burned |
| 🏢 [[Profile-Medium-Enterprise]] | 10–50 | 6–18 months | Hybrid | ~110 | Governance + delivery balance |
| 🏗️ [[Profile-Large-Safety-Critical]] | 50+ | 18+ months | V-Model / Waterfall | ~150+ | Full formal — if it's not documented, it didn't happen |

Each profile includes:
- Phase-by-phase document tables with priority ratings adjusted for that project size
- A **Quick-Start Checklist** at the end (🔴 Must Have items only — printable)
- Links back to all six BOK essential documents for deeper reference

## Related Vaults

For the full body of knowledge behind these checklists:

- [[BABOK v3 - Overview]] — Business Analysis Body of Knowledge (IIBA, 2015)
- [[SWEBOK v4 - Overview]] — Software Engineering Body of Knowledge (IEEE, 2024)
- [[PMBOK v8 - Overview]] — Project Management Body of Knowledge (PMI, 2025)
- [[SEBoK v2 - Overview]] — Systems Engineering Body of Knowledge (BKCASE, 2025)
- [[CyBOK v1 - Overview]] — Cyber Security Body of Knowledge (NCSC, 2021)
- [[DMBoK v2 - Overview]] — Data Management Body of Knowledge (DAMA, 2017)
