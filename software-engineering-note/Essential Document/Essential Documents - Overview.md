---
tags: [overview, essential-documents, software-engineering, project-management, systems-engineering, business-analysis]
---

# Essential Documents — Overview

> **Purpose:** A practical, phase-organized checklist of documents you need to produce across four disciplines — Business Analysis, Software Engineering, Project Management, and Systems Engineering.
> Each checklist is extracted from its respective Body of Knowledge and links back to the full reference for deeper study.

## What Is This?

This folder contains **document-only extracts** from four major Bodies of Knowledge:

| Discipline | Source | File | Focus |
|---|---|---|---|
| 📊 **Business Analysis** | BABOK® Guide v3 (IIBA) | [[BABOK Essential Documents]] | BA workflow: Planning → Strategy → Elicitation → Analysis & Design → Life Cycle Mgmt → Solution Evaluation |
| 💻 **Software Engineering** | SWEBOK v4 (IEEE) | [[SWEBOK Essential Documents]] | SDLC phases: Requirements → Architecture → Design → Construction → Testing → DevOps → Maintenance |
| 📋 **Project Management** | PMBOK® Guide v8 (PMI) | [[PMBOK Essential Documents]] | Five Focus Areas: Initiating → Planning → Executing → M&C → Closing |
| ⚙️ **Systems Engineering** | SEBoK v2 (BKCASE) | [[SEBOK Essential Documents]] | SE Life Cycle: Concept → Architecture → Realization → Operations → Maintenance |

> 📊 **BABOK was added to fill the gap between business strategy and technical execution.** It covers what happens *before* development starts — understanding the business need, defining the future state, and specifying what value the solution must deliver.

Each checklist follows the same format:
- 🔴🟡🟢 **Priority legend** — Must Have / Nice to Have / Optional
- **Phase-organized tables** with document name, description, priority, and ISO/IEEE reference
- **Key Standards Referenced** table at the bottom
- ⚠️ **Link-back banner** pointing to the full Body of Knowledge

## Why This Folder Exists

The full BOK vaults are deep — 200-370 KB each with principles, processes, tools, techniques, case studies, and theory. When you're working on a project and need a quick answer to *"what documents should I be producing right now?"*, this folder gives you the checklist without the theory.

These are designed for:
- **Project kickoff** — What do I need to produce in the first week?
- **Phase planning** — What documents does this phase require?
- **Audit prep** — Are we meeting standards for documentation?
- **Team onboarding** — Here's what we produce and why

## Quick Comparison

| Stage      | BABOK (Business)                                                                             | SWEBOK (Software)                                    | PMBOK (Project)                                               | SEBOK (Systems)                                              |
| ---------- | -------------------------------------------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------ |
| **Start**  | Current State, Business Requirements, Business Objectives, Future State, Change Strategy     | BRD, SRS, User Stories, Acceptance Criteria          | Business Case, Project Charter, Stakeholder Register          | Mission Analysis, ConOps, Stakeholder Needs, SyRS            |
| **Design** | Requirements (specified), Requirements Architecture, Design Options, Solution Recommendation | SAD, ADR, Architecture Views, API Specs, HLD/LLD     | Project Mgmt Plan, WBS, Schedule, Budget, Risk Register       | Functional/Logical/Physical Architecture, ICD, Trade Studies |
| **Build**  | Requirements (verified/validated/approved)                                                   | Source Code, Unit Tests, Build Scripts, Code Reviews | Team Assignments, Issue Log, Change Requests, Lessons Learned | Implementation Plan, Integration Plan, Verification Plan     |
| **Test**   | Solution Performance Measures, Performance Analysis                                          | Test Plan, Test Cases, Defect Reports, UAT Sign-off  | Work Performance Reports, Variance Analysis, EVM              | Verification Reports, Validation Reports, Audit Reports      |
| **Ship**   | Solution/Enterprise Limitations, Recommended Actions                                         | Deployment Plan, CI/CD, SLA, Runbook, Release Notes  | Final Report, Project Closure, Verified Deliverables          | Transition Plan, Operations Manual, Maintenance Plan         |
| **Cross**  | BA Approach, Governance, Stakeholder Engagement, Info Mgmt                                   | QMS, SQAP, Threat Model, SCMP, Baseline Records      | Change Mgmt Plan, Config Mgmt Plan, Procurement Docs          | SEMP, Safety Case, Security Plan, Standards Matrix           |

## How BABOK Connects to SWEBOK

```
BABOK (Business Analysis)     ──feeds──→     SWEBOK (Software Engineering)
─────────────────────────                    ────────────────────────────
Strategy Analysis                             Requirements
  → Current State, Business Req,               → SRS, BRD, User Stories
    Future State, Change Strategy

Requirements Analysis & Design                Architecture + Design
  → Specified Requirements,                    → SAD, HLD, LLD, API Specs
    Requirements Architecture

Requirements Life Cycle Mgmt                  Construction + Testing
  → Traced, Maintained,                        → Source Code, Test Cases
    Prioritized, Approved

Solution Evaluation                           Operations + Maintenance
  → Performance Measures,                      → Deployment Plan, SLA,
    Limitations, Actions                         Incident Mgmt, MR/PR
```

## Which Checklist Do I Need?

- **Analyzing business needs and defining value?** → Start with [[BABOK Essential Documents]]
- **Building software?** → Start with [[SWEBOK Essential Documents]]
- **Managing a project?** → Start with [[PMBOK Essential Documents]]
- **Engineering a system (hardware + software + people)?** → Start with [[SEBOK Essential Documents]]
- **All four?** → They're designed to be complementary. A software project typically needs BABOK + SWEBOK + PMBOK. A large system integration needs all four.

## Related Vaults

For the full body of knowledge behind these checklists:

- [[BABOK v3 - Overview]] — Business Analysis Body of Knowledge (IIBA, 2015)
- [[SWEBOK v4 - Overview]] — Software Engineering Body of Knowledge (IEEE, 2024)
- [[PMBOK v8 - Overview]] — Project Management Body of Knowledge (PMI, 2025)
- [[SEBoK v2 - Overview]] — Systems Engineering Body of Knowledge (BKCASE, 2025)
