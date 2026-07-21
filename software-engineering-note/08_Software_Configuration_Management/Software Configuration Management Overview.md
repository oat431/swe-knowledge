---
tags:
  - overview
  - swebok
  - software-configuration-management
  - version-control
  - release-management
  - change-control
---

# Software Configuration Management — Overview

> **Source:** SWEBOK v4 Chapter 08
> **Purpose:** Identify, control, audit, and track the evolution of software artifacts throughout the entire lifecycle, ensuring integrity and traceability.

## What Is This?

Software Configuration Management (SCM) is the discipline of managing change to software artifacts — source code, documentation, test scripts, build configurations, infrastructure definitions, and anything else that constitutes the system. Without SCM, teams cannot answer basic questions: What version is running in production? What changed since the last release? Who approved this change? Can we reproduce last month's build?

SCM provides the backbone for collaborative development and quality assurance. Development needs version control to work in parallel. Testing needs reproducible builds and known baselines. Operations needs traceable releases and rollback capability. Quality assurance needs configuration audits to verify that what was built matches what was specified. The discipline covers version control, baseline establishment, formal change control, status accounting, auditing, and release management.

The core principles are deceptively simple — identify what you have, control how it changes, record the state of everything, and verify completeness. In practice, these activities become complex at scale: thousands of configuration items across multiple branches, environments, and release cycles. Modern tools (Git, CI/CD platforms, artifact registries) automate much of the mechanics, but the engineering principles remain essential knowledge.

## Knowledge Areas

### Management of the SCM Process
- Planning, organizing, and monitoring SCM activities including tool selection and vendor/subcontractor control
- The SCM Plan (SCMP) as a living document covering organization, responsibilities, schedules, and resources
- Interface control and constraints from the organizational and project context

### Software Configuration Identification
- Determining which artifacts are Configuration Items (CIs) with unique identifiers and attributes
- Establishing baselines — formally approved, fixed versions that can only change through formal change control
- Tracking CI relationships: dependencies, derivation, succession, and variants

### Software Configuration Change Control
- Formal processes for requesting, evaluating, approving, and implementing changes via Change Requests (CRs)
- Configuration Control Board (CCB) as the authority body for evaluating and deciding on changes
- Handling deviations (pre-approved departures) and waivers (post-discovery nonconformance acceptance)

### Software Configuration Status Accounting
- Recording and reporting CI status, baselines, and relationships throughout the lifecycle
- Providing change traffic metrics, implementation status, and evidence for governance/compliance
- Supporting audits and process measurement through verifiable status records

### Software Configuration Auditing
- Functional Configuration Audit (FCA): verifying software is consistent with its governing specifications
- Physical Configuration Audit (PCA): verifying documentation is consistent with the as-built product
- In-process audits conducted throughout development, not just at release milestones

### Software Release Management and Delivery
- Building correct versions, packaging artifacts, and producing version description documents (VDD)
- CI/CD pipelines for automated build-test-deploy triggered by commits
- Software Bill of Materials (SBOM) for security, compliance, and vulnerability tracking

### Software Configuration Management Tools
- Version control systems (Git, SVN) supporting branching, merging, and parallel/distributed development
- Build automation tools, change control/issue tracking tools, and integrated CM workbenches
- Configuration Management Databases (CMDBs) for persistent storage of configuration data and relationships

## My Notes

### SCM Patterns (Berczuk & Appleton)
- [[01_SCM_Fundamentals]] — Key concepts, workspace, architecture, SCM in agile, pattern overview
- [[02_Codeline_and_Branching]] — Mainline, Active Development Line, Private Workspace, Repository
- [[03_Workspace_and_Build]] — Private System Build, Integration Build, Third Party Codeline
- [[04_Commit_and_Testing]] — Task Level Commit, Codeline Policy, Smoke/Unit/Regression Tests
- [[05_Release_and_Version_Management]] — Release Line, Release-Prep, Task Branch, Private Versions

### Version Control Practice
- [[Version Control/]]

## Relationship to Other KAs

- **[[Software Construction Overview|Software Construction]]** — Version control and build automation are tightly integrated with daily coding workflows and code reviews.
- **[[Software Testing Overview|Software Testing]]** — Test environments, test data, and test scripts are configuration items. Baselines provide stable reference points for testing.
- **[[Software Engineering Operations Overview|Software Engineering Operations]]** — CI/CD pipelines, infrastructure as code, and release management are SCM at the operational level.
- **[[Software Maintenance Overview|Software Maintenance]]** — Change control and impact analysis are essential for safe maintenance. Every maintenance change goes through SCM.
- **[[Software Quality Overview|Software Quality]]** — Configuration audits (FCA/PCA) are quality assurance activities. SQA depends on SCM for controlled baselines.
- **[[Software Engineering Management Overview|Software Engineering Management]]** — SCM provides visibility into project status through change metrics and release tracking.

---

## SWEBOK v4 Coverage Map

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 08 | **Last analyzed:** 2026-07-21 | **Coverage:** ~92% (updated)

| # | SWEBOK Topic | Status | Vault File(s) | Notes |
|---|---|---|---|---|
| 1 | Management of SCM Process | ✅ | `01`, `11_Vendor_and_Interface_Control.md` (32 KB) | Vendor SCM, ICD, distributed teams, microservices SCM |
| 2 | Software Configuration Identification | ✅ | `01`, `02_Codeline_and_Branching` | CIs, codelines, versions, labels, branches well covered |
| 3 | Software Configuration Change Control | ✅ | `02`, `04`, `09_Change_Control_and_Compliance.md` (26 KB) | CCB, CR lifecycle, SBOM, VDD, CMDB, cryptographic hashing |
| 4 | Software Configuration Status Accounting | ✅ | `07_SCSA_and_Status_Accounting.md` (14 KB) | CI status, change traffic metrics, governance/compliance evidence |
| 5 | Software Configuration Auditing | ✅ | `08_Configuration_Auditing.md` (19 KB) | FCA, PCA, in-process audits, deviations/waivers, SCMP |
| 6 | Release Management & Delivery | ✅ | `05_Release_and_Version_Management` | Release Lines, Release-Prep, Task Branch, Daily Build |
| 7 | SCM Tools | ✅ | `Version Control/01-03` | Git extensively covered; CMDB, SBOM tools missing |

### Gaps to Fill

| Priority | Gap | SWEBOK Topic | What's Missing |
|----------|-----|-------------|----------------|
| 🔴 High | SCSA | Status Accounting | Recording/reporting CI status, baselines, change traffic metrics |
| 🔴 High | FCA/PCA Audits | Configuration Auditing | Functional and Physical Configuration Audits |
| 🟡 Medium | CCB/CR Workflows | Change Control | Configuration Control Board, formal change request lifecycle |
| 🟡 Medium | SBOM | Release Management | Software Bill of Materials for security/compliance |
| 🟡 Medium | CMDB | SCM Tools | Configuration Management Database concepts |
| 🟢 Low | Deviations vs Waivers | Change Control | Pre-approved departures vs post-discovery nonconformance acceptance |
| 🟢 Low | SCMP | Management of SCM | SCM Plan structure, content, lifecycle |
| 🟢 Low | VDD | Release Management | Version Description Document structure and purpose |
