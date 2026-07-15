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

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 08 — Software Configuration Management
> **Purpose:** Identify, control, audit, and report on all artifacts of a software system throughout its lifecycle.

## What Is This?

Software Configuration Management (SCM) is the discipline of managing change to software artifacts — source code, documentation, test scripts, build configurations, infrastructure definitions, and anything else that constitutes the system. Without SCM, teams cannot answer basic questions: What version is running in production? What changed since the last release? Who approved this change? Can we reproduce the build from three months ago?

SCM provides the infrastructure of trust that enables all other engineering activities. Development needs version control to work in parallel. Testing needs reproducible builds and known baselines. Operations needs traceable releases and rollback capability. Management needs status accounting to track project state. Quality assurance needs configuration audits to verify that what was built matches what was specified.

The core of SCM is deceptively simple — identify what you have, control how it changes, record the state of everything, and verify completeness. In practice, these activities become complex at scale: thousands of configuration items across multiple branches, environments, and release cycles. Modern tools (Git, CI/CD platforms, artifact registries) automate much of the mechanics, but the principles of SCM remain essential engineering knowledge.

## The 7 Topic Areas

### 1. [[Identification]]
- Configuration items (CIs): source files, documents, test data, build scripts, infrastructure definitions
- Naming conventions and identification schemes
- Configuration item attributes: version, owner, status, relationships
- Decomposition: system → subsystem → component → file
- Bill of materials: what goes into each release
- **Book:** *Software Configuration Management Patterns* — Berczuk & Appleton

### 2. [[Change Control]]
- Change request process: submit, evaluate, approve/reject, implement, verify
- Change Control Board (CCB): roles, authority, decision criteria
- Change impact analysis (links to [[07_Software_Maintenance/Software Maintenance Overview|Maintenance]])
- Emergency change procedures and expedited review
- Access control: who can change what, code review requirements
- **Book:** *Continuous Delivery* — Jez Humble & David Farley

### 3. [[Status Accounting]]
- Recording and reporting the status of configuration items
- Change logs, history, and audit trails
- Metrics: change volume, change frequency, lead time for changes
- Dashboards and reporting for stakeholders
- Traceability: linking changes to requirements, defects, and releases
- **Book:** *Software Configuration Management Patterns* — Berczuk & Appleton

### 4. [[Configuration Audits]]
- **Functional Configuration Audit (FCA):** Verify that the software meets its functional requirements
- **Physical Configuration Audit (PCA):** Verify that the as-built artifacts match the as-documented configuration
- Baseline audits: confirming completeness and consistency before release
- Audit findings and corrective actions
- Relationship to quality assurance and formal reviews
- **Book:** *Continuous Delivery* — Jez Humble & David Farley

### 5. [[Version Control]]
- Branching strategies: trunk-based development, GitFlow, GitHub Flow, release branches
- Merge vs. rebase, conflict resolution, cherry-picking
- Distributed vs. centralized version control systems
- Monorepo vs. multi-repo strategies
- Git internals: commits, trees, blobs, refs
- Version control for non-code artifacts: docs, data, ML models
- **Book:** *Version Control with Git, 3rd Ed.* — Loeliger & McCullough

### 6. [[Baselines]]
- Definition: a baseline is a formally reviewed and agreed-upon configuration that can only be changed through formal change control
- Baseline types: functional, allocated, developmental, product
- Baselining at milestones: requirements baseline, design baseline, code baseline
- Baseline management in agile contexts: continuous baselining
- Rollback and baseline restoration
- **Book:** *Agile Configuration Management* — Matthew Parkinson

### 7. [[Release Management]]
- Release planning: scope, schedule, go/no-go criteria
- Release packaging: versioning schemes (semver), release notes, artifacts
- Deployment strategies: blue-green, canary, rolling, feature flags
- Release governance: approval workflows, compliance checks
- Release retrospectives and continuous improvement
- **Book:** *Continuous Delivery* — Jez Humble & David Farley

## Recommended Books (Priority Order)

| # | Book | Author(s) | Pages | Priority |
|---|------|-----------|:-----:|:--------:|
| 1 | *Continuous Delivery* (2010) | Jez Humble & David Farley | 512 | 🔴 Essential |
| 2 | *Software Configuration Management Patterns* (2003) | Steve Berczuk & Brad Appleton | 256 | 🔴 Essential |
| 3 | *Version Control with Git, 3rd Ed.* (2022) | Jon Loeliger & Matthew McCullough | 544 | 🟡 Recommended |
| 4 | *Agile Configuration Management* (2015) | Matthew Parkinson | 180 | 🟢 Supplementary |

## Relationship to Other KAs

- **[[Software Construction Overview|Software Construction]]** — Version control is a daily tool for developers. Build reproducibility and code reviews are construction activities that depend on SCM.
- **[[Software Testing Overview|Software Testing]]** — Test environments, test data, and test scripts are configuration items. Baselines provide the stable reference point for testing.
- **[[Software Engineering Operations Overview|Software Engineering Operations]]** — CI/CD pipelines, infrastructure as code, and release management are SCM activities at the operational level.
- **[[Software Maintenance Overview|Software Maintenance]]** — Change control and impact analysis are essential for safe maintenance. Every maintenance change goes through the SCM process.
- **[[Software Quality Overview|Software Quality]]** — Configuration audits (FCA/PCA) are quality assurance activities. Status accounting provides data for quality metrics.
- **[[Software Engineering Management Overview|Software Engineering Management]]** — SCM provides visibility into project status through change metrics and release tracking.

## Related
- [[SWEBOK v4 - Overview]]
- [[06_Software_Engineering_Operations/Software Engineering Operations Overview|Operations Overview]]
- [[07_Software_Maintenance/Software Maintenance Overview|Maintenance Overview]]
