---
tags:
  - quality
  - contract-review
  - quality-planning
  - vv
  - software-quality
source: "Galin, SQA — Chapters 5–6"
created: 2026-07-21
---

# 02 — Pre-Project and Planning: Contract Review & Development/Quality Plans

## Overview

Before a single line of code is written, SQA begins with preventive activities during the pre-project phase. Galin dedicates two chapters to this critical window:

| Chapter | Focus | Key Output |
|---------|-------|------------|
| **Ch 5** | Contract Review | Validated proposal, error-free contract |
| **Ch 6** | Development & Quality Plans | Detailed project roadmap, SQA activity schedule |

The core principle: **A bad contract yields low-quality software.** SQA starts before development — at the proposal and contract stage.

---

## Chapter 5 — Contract Review

### 5.1 The Problem: The CFV Case

A 10-month project celebrated as "on schedule" actually lost **$90,000** because no one read the RFP clause requiring on-site training at **19 branches (6 overseas)**. This illustrates the catastrophic cost of sloppy proposal/contract work.

> *"Unrealistic professional commitments lead to failure to achieve the required software quality."*

### 5.2 The Two-Stage Process

```
┌─────────────────────────────────────────────────────────────────┐
│                    CONTRACT REVIEW PROCESS                       │
│                                                                  │
│  STAGE 1: Proposal Draft Review                                 │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │ Reviews: final proposal draft + its foundations:          │   │
│  │ • Customer requirement documents (RFP)                    │   │
│  │ • Cost & resource estimates                               │   │
│  │ • Partner/subcontractor agreements                        │   │
│  └──────────────────────────────────────────────────────────┘   │
│                          ↓                                       │
│  STAGE 2: Contract Draft Review                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │ Reviews: contract draft against the proposal + all         │   │
│  │ understandings reached during negotiations                │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

### 5.3 Contract Review Objectives

#### Proposal Draft Review — 9 Objectives

| # | Objective | Key Concern |
|---|-----------|-------------|
| 1 | **Customer requirements clarified & documented** | Vague RFPs need additional detail approved by both parties |
| 2 | **Alternative approaches examined** | Software reuse, partnerships, subcontracting options |
| 3 | **Formal relationship aspects specified** | Communication channels, deliverables, acceptance criteria, phase approval, change request procedure |
| 4 | **Development risks identified** | Knowledge gaps, tool unfamiliarity, technical risks |
| 5 | **Resources & timetable adequately estimated** | Staff, budget, subcontractor fees, schedule accounting for all parties |
| 6 | **Company's capacity examined** | Professional competence + availability of staff and facilities |
| 7 | **Customer's capacity examined** | Financial and organizational readiness (staffing, hardware, communications) |
| 8 | **Partner & subcontractor participation defined** | QA issues, payment schedules, profit distribution, team cooperation |
| 9 | **Proprietary rights defined & protected** | Reused software rights, future reuse rights, data file ownership, security |

#### Contract Draft Review — 3 Objectives

| # | Objective |
|---|-----------|
| 1 | No unclarified issues remain in the contract draft |
| 2 | All post-proposal understandings are fully and correctly documented |
| 3 | No unintended changes, additions, or omissions entered the contract draft |

### 5.4 Implementation of Contract Review

#### Factors Affecting Review Extent
- **Project magnitude** (man-months)
- **Technical complexity**
- **Staff experience** with project area (higher → reduced review)
- **Organizational complexity** (number of partners, subcontractors, customers)

#### Who Performs the Review (ascending complexity)

| Level | Reviewer |
|-------|----------|
| Simple | Leader/member of proposal team |
| Moderate | Members of proposal team |
| Substantial | Outside professional or non-team staff member |
| Major | **Team of outside experts** |

#### Difficulties in Major Contract Reviews

1. **Time pressure** — reviews must complete in days, not weeks
2. **Substantial professional effort required** — deep expertise needed
3. **Reviewers are busy** — senior staff already committed to other tasks

#### Recommended Avenues for Major Reviews

- Include review in the proposal preparation **schedule**
- Use a **team** to distribute workload
- Appoint a **team leader** responsible for: recruitment, task distribution, coordination (internal + with proposal team), follow-up, summary delivery

### 5.5 Contract Review Subjects

Checklists (Appendices 5A & 5B in Galin) organize review subjects by objective:

**Proposal Draft Review Checklist covers:**
- Functional requirements, operating environment, interfaces, performance, reliability, usability
- Training/installation efforts (locations, numbers)
- Warranty period, maintenance beyond warranty
- Alternative approaches: reuse, partners, subcontractors
- Formal relationships: committees, documentation, approvals, change requests, completion criteria, bonuses/penalties, cancellation terms
- Development risks: knowledge gaps, hardware/software availability
- Resource estimates: man-days per phase (including corrections), documentation effort, warranty resources
- Firm capacity: knowledge pool, staff availability, computer resources, special tools/standards
- Customer capacity: financial, facilities, staffing, task completion
- Partner/subcontractor: responsibility allocation, payment schedules, QA participation
- Proprietary rights: purchased software, data files, future reuse, development-period protection

**Contract Draft Review Checklist covers:**
- Supplier obligations as defined
- Customer obligations as defined
- All post-proposal understandings: functional requirements, financial issues, customer obligations, partner/subcontractor obligations
- Completeness check: no missing sections, no unauthorized changes/omissions/additions

### 5.6 Contract Reviews for Internal Projects

**Problem:** Internal projects often use "loose relationships" (goodwill-based) instead of formal customer-supplier relationships, leading to:

| Problem | Impact on Customer | Impact on Developer |
|---------|-------------------|-------------------|
| Inadequate requirements definition | Implementation deviates from needs; low satisfaction | High change requirements; wasted resources |
| Poor resource estimates | Unrealistic expectations; inter-unit friction | Budget overruns |
| Poor timetable | Missed dates; time-pressured development | Low quality; delayed staff release |
| Inadequate risk awareness | Unprepared for consequences | Delayed mitigation efforts |

**Recommendation:** Apply the same formal contract review procedures to internal projects.

---

## Chapter 6 — Development and Quality Plans

### 6.1 Why New Plans Are Needed (Even After Contract Review)

The opening anecdote illustrates that even with a thorough contract review:
- **Time has passed** (7 months between proposal and contract signing in the example)
- Staff may no longer be available
- Subcontractors may have gone bankrupt
- Technology may have shifted

> *"The project needs plans that are more comprehensive than the approved proposal."*

Relevant standards: **ISO 9000-3**, **IEEE 730**, **CMM**.

### 6.2 The Five Planning Objectives

| # | Objective |
|---|-----------|
| 1 | **Schedule development activities** for timely completion + estimate manpower/budget |
| 2 | **Recruit team members & allocate resources** according to activity schedules |
| 3 | **Resolve development risks** |
| 4 | **Implement required SQA activities** |
| 5 | **Provide management with project control data** |

### 6.3 Elements of the Development Plan (11 elements)

```
DEVELOPMENT PLAN
├── 1. Project Products
│   ├── Design documents (with completion dates, deliverables marked)
│   ├── Software products (completion date + installation site)
│   └── Training tasks (dates, participants, sites)
├── 2. Project Interfaces
│   ├── Software interfaces (existing packages)
│   ├── Team interfaces (cooperation/coordination with other teams)
│   └── Hardware interfaces
├── 3. Project Methodology & Development Tools
│   └── Tools and methods for each phase
├── 4. Software Development Standards & Procedures
│   └── List of standards to apply
├── 5. Mapping of the Development Process
│   ├── Phase definitions (inputs, outputs, activities)
│   ├── Activity duration estimates
│   ├── Logical sequencing (dependencies)
│   ├── Professional resources per activity
│   └── Tools: GANTT, CPM, PERT (Microsoft Project™ etc.)
├── 6. Project Milestones
│   └── Completion times + products for each milestone
├── 7. Project Staff Organization
│   ├── Organizational structure (teams and tasks)
│   ├── Professional requirements (certification, language/tool experience)
│   ├── Headcount per period
│   └── Names of team leaders and members
├── 8. Required Development Facilities
│   └── Hardware, software, office space (with timetable)
├── 9. Development Risks & Risk Management Actions
│   └── Identification → Evaluation → Planning → Implementation → Monitoring
├── 10. Control Methods
│   └── Progress reports, coordination meetings (see Ch 19)
└── 11. Project Cost Estimates
    └── Based on proposal costs, updated for current realities
```

#### Development Risk Management (Appendix 6A)

**Definition:** A development risk is *"a state or property of a development task or environment, which, if ignored, will increase the likelihood of project failure."* (Ropponen & Lyytinen, 2000)

**Six Risk Classes:**
1. Scheduling and timing risks
2. System functionality risks
3. Subcontracting risks
4. Requirement management risks
5. Resource usage and performance risks
6. Personnel management risks

**Top 10 Software Risk Items** (Boehm & Ross, 1989):

| # | Risk Item | Class |
|---|-----------|-------|
| 1 | Personnel shortfalls | Personnel |
| 2 | Unrealistic schedules and budgets | Scheduling |
| 3 | Developing wrong software functions | Functionality |
| 4 | Developing wrong user interface | Functionality |
| 5 | Gold plating | Requirements |
| 6 | Continuing stream of requirement changes | Requirements |
| 7 | Shortfalls in externally furnished components | Subcontracting |
| 8 | Shortfalls in externally performed tasks | Subcontracting |
| 9 | Real-time performance shortfalls | Resources/Performance |
| 10 | Straining computer science capabilities | Resources/Performance |

**Risk Management Actions (RMAs)** serve three purposes:
- **Prevention** — stop the risk from materializing
- **Early identification** — detect risk before it causes damage
- **Resolution** — fix the risk once it appears

RMAs are grouped into:
- **Internal RMAs** — within the organization (training, prototyping, simulation, early scheduling, etc.)
- **Subcontracting RMAs** — comprehensive contracts, participation in subcontractor QA, professional loans
- **Customer RMAs** — thorough customer contracts, renegotiation of requirements/schedules

**Risk Management Process:**
```
Risk Identification → Risk Evaluation → RMA Planning → RMA Implementation → RMA Monitoring
```

Risk priority can be calculated as: `Priority = Prob(materialization) × Est(damage)`

### 6.4 Elements of the Quality Plan (5 elements)

```
QUALITY PLAN
├── 1. Quality Goals
│   ├── Derived from customer acceptance criteria (RFP)
│   ├── Prefer quantitative over qualitative measures
│   └── Examples: uptime %, response time, training time, error recovery time
├── 2. Planned Review Activities
│   ├── Design reviews (DRs), design inspections, code inspections
│   ├── For each: scope, type, schedule, procedures, responsible person
├── 3. Planned Software Tests
│   ├── Unit, integration, system tests
│   ├── For each: unit/system tested, test type, schedule, procedures, responsible person
├── 4. Planned Acceptance Tests for Externally Developed Software
│   ├── Purchased software, subcontractor-developed, customer-supplied
│   └── Parallel to internally developed software testing
└── 5. Configuration Management
    └── Tools, procedures, change-control mechanisms
```

**Quality Plan Format:** May be part of the development plan, an independent document, or split into specialized documents (DR plan, testing plan, acceptance test plan).

### 6.5 Small Projects: Adapted Plans

**Key decision framework:**

| Situation | Action |
|-----------|--------|
| Very small (e.g., <15 man-days) | No plans required |
| Small (<50 man-days, no significant risks) | Decision left to project leader |
| Small but complex/penalty-laden | Plans required |
| Above threshold | Plans obligatory |

**Recommended minimum elements for small projects:**

| Development Plan | Quality Plan |
|-----------------|--------------|
| Project products (deliverables) | Quality goals |
| Project benchmarks | |
| Development risks | |
| Project cost estimates | |

**Benefits of planning even for small projects:**
1. More comprehensive understanding of the task
2. Greater responsibility/accountability for meeting obligations
3. Easier management/customer control and early delay detection
4. Better developer-customer understanding of requirements and timetable

### 6.6 Internal Projects: Full Plans Recommended

The "Super-Monster 2000" case: A computer game project skipped planning, missed the Christmas market window, cost overrun from $240K → $385K, and the company abandoned all future internal game development.

**Benefits of full plans for internal projects:**

| Stakeholder | Benefits |
|-------------|----------|
| **Development Dept** | Avoid budget overruns; prevent damage to other projects from delayed staff release; protect firm reputation |
| **Internal Customer** | Smaller deviations from planned dates/budget; better process control; fewer internal delay damages |
| **Organization** | Reduced market-loss risk; reduced lawsuit/penalty risk; preserved reputation; reduced budget supplement risk |

---

## Chapter 7 — Integrating Quality Activities in the Project Life Cycle

> **Note:** The source file focuses primarily on Chapters 5–6 content. Chapter 7 topics — integrating quality activities in the project life cycle, V&V (Verification & Validation), qualification, defect removal effectiveness models — are referenced in Galin's framework but were not included in the extracted source material. Key concepts to supplement:

### Key Concepts from Galin Ch 7 (Framework)

- **Quality activities integrated throughout the SDLC** — not bolted on at the end
- **V&V (Verification & Validation):**
  - **Verification:** "Are we building the product right?" (reviews, inspections, testing)
  - **Validation:** "Are we building the right product?" (acceptance testing, user validation)
- **Qualification:** Formal demonstration that a product meets specified requirements
- **Defect Removal Effectiveness (DRE):** `DRE = Defects removed during phase / (Defects removed + Defects escaped)` — measures how effective each quality activity is at catching defects before they escape downstream

### Quality Activities Across the Life Cycle

| Phase | Quality Activities |
|-------|-------------------|
| **Pre-Project** | Contract review, quality planning |
| **Requirements** | Requirements reviews, risk assessment |
| **Design** | Design reviews, design inspections |
| **Coding** | Code inspections, unit testing |
| **Integration** | Integration testing, interface reviews |
| **System Testing** | System tests, acceptance tests, performance tests |
| **Deployment** | Installation testing, user training validation |
| **Maintenance** | Regression testing, change reviews |

---

## Summary: Pre-Project SQA Flow

```
RFP / Tender
     │
     ▼
┌─────────────────────┐
│ PROPOSAL PREPARATION │
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐     ┌──────────────────────────────┐
│ STAGE 1: Proposal    │────▶│ 9 Objectives Checklist        │
│ Draft Review         │     │ (Appendix 5A)                 │
└─────────┬───────────┘     └──────────────────────────────┘
          │ (corrections)
          ▼
     Proposal Submitted
          │
          ▼
     Negotiations
          │
          ▼
┌─────────────────────┐     ┌──────────────────────────────┐
│ STAGE 2: Contract    │────▶│ 3 Objectives Checklist         │
│ Draft Review         │     │ (Appendix 5B)                 │
└─────────┬───────────┘     └──────────────────────────────┘
          │ (corrections)
          ▼
     Contract Signed
          │
          ▼
┌─────────────────────────────────────────────────────────┐
│ DEVELOPMENT PLAN (11 elements) + QUALITY PLAN (5 elements) │
│                                                           │
│ • Updated from proposal (time has passed!)                │
│ • More comprehensive than proposal                       │
│ • Includes risk management actions                        │
│ • Adapted for small/internal projects if needed           │
└─────────────────────────────────────────────────────────┘
          │
          ▼
     Project Execution (with integrated quality activities)
```

---

## Key Takeaways

1. **SQA begins before coding** — at the contract review stage
2. **Two reviews, different goals:** Proposal review ensures a winnable, doable bid; Contract review ensures no surprises after signing
3. **Plans ≠ Proposal:** Development and quality plans must be freshly prepared, more comprehensive, and updated for current realities
4. **Risk management is systematic:** Identify → Evaluate → Plan → Implement → Monitor
5. **Small projects still need plans** — scaled-down versions provide disproportionate benefits
6. **Internal projects are NOT exempt** — loose relationships breed failure; formal processes protect both sides
7. **Quality is integrated, not added** — quality activities must be planned into the schedule from day one

---

*Source: Galin, D. — Software Quality Assurance: From Theory to Implementation (Chapters 5–6, with Chapter 7 framework concepts)*
