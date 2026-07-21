---
tags:
  - quality
  - sqa
  - quality-factors
  - mccall
  - iso-9126
  - software-quality
source: "Galin, D. (2004) *Software Quality Assurance: From Theory to Implementation*, Pearson/Addison-Wesley, Ch 1–4"
created: 2026-07-21
---

# 01 — Quality Fundamentals (Galin Ch 1–4)

> **Source:** Daniel Galin, *Software Quality Assurance: From Theory to Implementation*, Chapters 1–4.
> Covers: the software quality challenge, the unique nature of SQA, software errors/faults/failures, nine causes of errors, definitions of quality and SQA, McCall's quality factor model (plus alternatives), and an overview of the SQA system architecture.

---

## 1. The Software Quality Challenge (Ch 1)

### 1.1 Why SQA Is Unique

Software differs from other industrial products in three fundamental ways that justify a **separate SQA methodology**:

| Characteristic | Software Products | Other Industrial Products |
|---|---|---|
| **Complexity** | Very high — millions of operational possibilities | Much lower — at most a few thousand operational options |
| **Visibility** | Invisible. Defects cannot be detected by sight. Missing parts are invisible (imagine a missing door on a new car — impossible in software) | Visible — defects and missing parts are apparent |
| **Development & Production Process** | Defect detection opportunities exist in **only one phase** (product development). No production-planning phase; manufacturing is mere copying. | Defects can be detected in all three phases: development, production planning, and manufacturing |

**Frame 1.1 — The uniqueness of the software development process:**
- High complexity compared to other industrial products
- Invisibility of the product
- Defect-detection opportunities are limited to the product development phase

> **Note on firmware:** Embedded software (firmware) shares the same characteristics. Throughout the text, "software" includes firmware.

The need for special SQA tools is reflected in targeted standards like **ISO 9000-3** ("Guidelines for the application of ISO 9001 to the development, supply and maintenance of software") — one of the only industry-specific ISO quality guidelines (the other being for services, ISO 9004-2).

### 1.2 The SQA Environment — Seven Characteristics

Professional software development and maintenance operate under conditions that demand both intensive **professional** and **managerial** efforts:

1. **Contractual conditions** — Defined functional requirements, budget, and timetable bind the project.
2. **Customer–supplier relationship** — Continuous oversight by the customer; requests for changes, criticisms, approvals.
3. **Teamwork** — Required due to timetable pressures, need for diverse specializations, and benefits of mutual support/review.
4. **Cooperation and coordination with other teams** — Internal software/hardware teams, other suppliers, customer teams.
5. **Interfaces with other software systems** — Input interfaces, output interfaces, and control-board interfaces.
6. **Team member turnover** — The project must continue despite departures; "the show must go on."
7. **Extended maintenance period** — Software must be maintained for 5–10 years after delivery.

> Even in-house development without formal contracts often exhibits these characteristics. The closer the relationship to a formal customer–supplier pattern, the greater the prospect of project success.

---

## 2. What Is Software Quality? (Ch 2)

### 2.1 Software — The IEEE Definition

**Software** (IEEE, 1991) comprises **four components**, all of which must be addressed by SQA:

1. **Computer programs** (the "code")
2. **Procedures** — define order, schedule, methods, and responsible persons
3. **Documentation** — for developers (requirements, design reports), users (manuals), and maintenance personnel (programmer's manual)
4. **Data** — parameters, codes, name lists, and standard test data for the specific installation

> SQA always includes, in addition to code quality, the quality of procedures, documentation, and data.

### 2.2 Software Errors, Faults, and Failures

These three terms form a **causal chain**:

```
Software Error → Software Fault → Software Failure
   (cause)         (intermediate)     (observed symptom)
```

- **Software Error:** A grammatical or logical mistake made by a programmer, analyst, tester, or other team member.
- **Software Fault:** An error that causes incorrect functioning of the software in a specific application. **Not all errors become faults** — some are neutralized by subsequent code.
- **Software Failure:** A fault that is **activated** when a user applies the specific faulty section. **Not all faults become failures** — many remain dormant because the user never invokes the faulty function, or the necessary conditions never occur.

**Key insight:** Developers care about errors and faults (elimination and prevention). Users care about failures. A software package can serve hundreds of clients for years without a failure, then "suddenly" fail when a dormant fault is finally activated by a new user scenario.

> Example: Pharm-Plus had a "super customer" identification fault that lay dormant for years until a new pharmacy decided to implement the feature. Example: Meteoro-X firmware had a 160°C limit instead of 60°C — never activated in coastal climates.

### 2.3 Nine Causes of Software Errors

All causes are ultimately **human**. They are classified by the development stage in which they occur:

| # | Cause | Description |
|---|---|---|
| 1 | **Faulty requirements definition** | Erroneous, absent, incomplete, or unnecessary requirements |
| 2 | **Client–developer communication failures** | Misunderstandings of written/oral instructions, changes, and responses |
| 3 | **Deliberate deviations from requirements** | Reusing modules without analysis, omitting functions under pressure, unapproved "improvements" |
| 4 | **Logical design errors** | Erroneous algorithms, sequencing errors, boundary-condition errors, omitted states, undefined reactions to illegal operations |
| 5 | **Coding errors** | Linguistic errors, tool-application errors, data-selection errors |
| 6 | **Non-compliance with documentation and coding instructions** | Harms team coordination, replacement, design reviews, testing, and maintenance |
| 7 | **Shortcomings of the testing process** | Incomplete test plans, undocumented faults, incomplete corrections |
| 8 | **Procedure errors** | Incorrect user-direction procedures (especially in multi-step complex systems) |
| 9 | **Documentation errors** | Omitted functions, incorrect instructions, listed-but-nonexistent functions |

### 2.4 Definitions of Software Quality

**IEEE definition (1991):**
> 1. The degree to which a system, component, or process meets **specified requirements**.
> 2. The degree to which a system, component, or process meets **customer or user needs or expectations**.

**Crosby (1979):** "Quality means conformance to requirements." → Focuses on the written spec; errors in the spec are not the developer's problem.

**Juran (1988):**
> 1. Quality consists of those product features which meet the needs of customers and thereby provide product satisfaction.
> 2. Quality consists of freedom from deficiencies.

→ Focuses on real user needs; demands that the developer validate the spec itself.

**Pressman (2000):**
> Conformance to explicitly stated functional and performance requirements, explicitly documented development standards, and implicit characteristics that are expected of all professionally developed software.

→ Adds two layers beyond functional requirements: (a) specified quality standards, and (b) **Good Software Engineering Practices (GSEP)** — state-of-the-art practices expected even when not explicitly contracted.

### 2.5 SQA — Definitions and Objectives

**IEEE definition (1991):**
> 1. A planned and systematic pattern of all actions necessary to provide adequate confidence that an item or product conforms to established technical requirements.
> 2. A set of activities designed to evaluate the process by which the products are developed or manufactured. Contrast with quality control.

**Galin's expanded definition (adopted in the book):**
> A systematic, planned set of actions necessary to provide adequate confidence that the software development process or the maintenance process of a software system product conforms to established functional technical requirements as well as with the managerial requirements of keeping the schedule and operating within the budgetary confines.

**Key expansions over IEEE:**
- Includes **maintenance**, not just development
- Includes **schedule and budget** management requirements
- Aligns with ISO 9000-3 and SEI-CMM

#### SQA vs. Quality Control (QC)

| Quality Control | Quality Assurance |
|---|---|
| Evaluates the quality of the **finished** product | Activities **throughout** the development/manufacturing process |
| Main objective: withhold non-qualifying products from shipment | Main objective: **prevent** causes of errors, detect and correct them early |
| QC is a **subset** of QA | QA substantially reduces the rate of non-qualifying products |

#### Objectives of SQA Activities

**Software development (process-oriented):**
1. Assure conformance to functional technical requirements
2. Assure conformance to managerial scheduling and budgetary requirements
3. Initiate and manage activities for improvement and greater efficiency

**Software maintenance (product-oriented):**
1. Assure maintenance activities conform to functional technical requirements
2. Assure conformance to managerial scheduling and budgetary requirements
3. Initiate and manage improvement activities for maintenance and SQA

### 2.6 SQA and Software Engineering

**Software Engineering** (IEEE): "The application of a systematic, disciplined, quantifiable approach to the development, operation, and maintenance of software."

The systematic and disciplined nature of software engineering makes it a **strong infrastructure** for achieving SQA objectives. Cooperation between software engineers and the SQA team is the accepted route to efficient, economic development and maintenance that simultaneously assures quality.

---

## 3. Software Quality Factors (Ch 3)

### 3.1 The Need for Comprehensive Requirements

Software projects often satisfy **basic correctness requirements** yet suffer from poor performance in areas like **maintenance, reliability, reusability, or training** — because requirements for these aspects were never defined. Quality factor models provide a framework to ensure that **all relevant aspects** of software use are covered.

### 3.2 McCall's Classic Factor Model (1977)

McCall classified all software requirements into **11 factors** grouped into **3 categories**:

```
                          ┌──────────────────┐
                          │   SOFTWARE       │
                          │   QUALITY        │
                          └────────┬─────────┘
                                   │
          ┌────────────────────────┼────────────────────────┐
          │                        │                        │
  ┌───────┴───────┐        ┌───────┴───────┐        ┌───────┴───────┐
  │   PRODUCT     │        │   PRODUCT     │        │   PRODUCT     │
  │   OPERATION   │        │   REVISION    │        │   TRANSITION  │
  └───────┬───────┘        └───────┬───────┘        └───────┬───────┘
          │                        │                        │
  · Correctness              · Maintainability         · Portability
  · Reliability              · Flexibility             · Reusability
  · Efficiency               · Testability             · Interoperability
  · Integrity
  · Usability
```

#### Product Operation Factors

| Factor | Description | Key Dimensions |
|---|---|---|
| **Correctness** | Required outputs and their attributes | Output mission, accuracy, completeness, up-to-dateness, availability (response time), coding/documentation standards |
| **Reliability** | Maximum allowed failure rate | System reliability, application reliability, computational/hardware failure recovery |
| **Efficiency** | Hardware resources needed to perform all functions | Processing (MIPS/MHz), storage (MB/GB/TB), communication (KBPS/MBPS/GBPS), power usage (portable units) |
| **Integrity** | Security — access control | Access control (read/write permits), access audit |
| **Usability** | Staff resources for training and operation | Operability (calls/day), training time |

#### Product Revision Factors

| Factor | Description | Key Dimensions |
|---|---|---|
| **Maintainability** | Efforts needed to locate and correct failures | Modular structure, internal documentation, programmer's manual, simplicity, self-descriptiveness |
| **Flexibility** | Efforts to adapt software to new circumstances/customers (adaptive + perfective maintenance) | Modularity, generality, simplicity, self-descriptiveness |
| **Testability** | Ease of testing and built-in diagnostics | User testability, failure maintenance testability, traceability, standard test data, auto-diagnostics |

#### Product Transition Factors

| Factor | Description | Key Dimensions |
|---|---|---|
| **Portability** | Adaptation to different hardware/OS environments | Software system independence, modularity, self-descriptiveness |
| **Reusability** | Use of modules in new projects | Modularity, document accessibility, system/application independence, generality, simplicity |
| **Interoperability** | Interfaces with other software/firmware systems | Commonality, system compatibility, software system independence, modularity |

### 3.3 Alternative Factor Models

Two models from the late 1980s expand on McCall:

| McCall (1977) — 11 factors, 3 categories | Evans & Marciniak (1987) — 12 factors, 3 categories | Deutsch & Willis (1988) — 15 factors, 4 categories |
|---|---|---|

**Five additional factors** were introduced by the alternative models:

| New Factor | Description | Models |
|---|---|---|
| **Verifiability** | Design/programming features enabling efficient verification (modularity, simplicity, adherence to guidelines) | Evans & Marciniak, Deutsch & Willis |
| **Expandability** | Future efforts to serve larger populations, improve service, add applications (≈ McCall's Flexibility) | Evans & Marciniak, Deutsch & Willis |
| **Safety** | Eliminate hazardous conditions due to process-control software errors | Deutsch & Willis |
| **Manageability** | Administrative tools for software modification (configuration management, change procedures) | Deutsch & Willis |
| **Survivability** | Continuity of service — minimum time between failures, recovery time (≈ McCall's Reliability) | Deutsch & Willis |

**Content analysis:** Expandability ≈ Flexibility, Survivability ≈ Reliability, Testability ⊆ Maintainability. Therefore the **truly new** factors from the alternatives are only: **Verifiability**, **Safety**, and **Manageability**.

#### Categories in Alternative Models

| McCall | Evans & Marciniak | Deutsch & Willis |
|---|---|---|
| Product Operation | Performance | Performance |
| Product Revision | Design | Change Management |
| Product Transition | Adaptation | Functional |

### 3.4 Sub-Factors and Compliance

Each factor is decomposed into **sub-factors** that enable measurement. Examples:

- **Correctness** → Accuracy, Completeness, Up-to-dateness, Availability, Coding/documentation consistency
- **Reliability** → System reliability, Application reliability, Computational failure recovery, Hardware failure recovery
- **Maintainability** → Simplicity, Modularity, Self-descriptiveness, Coding/documentation consistency, Document accessibility

Several sub-factors (e.g., **simplicity**, **modularity**) contribute to multiple factors.

Software quality **metrics** (Ch 21) quantify compliance against these sub-factors.

### 3.5 Who Defines Quality Requirements?

| Stakeholder | Typical Interests |
|---|---|
| **Client/Customer** | Correctness, Reliability, Efficiency, Integrity, Usability |
| **Developer** | Reusability, Portability, Verifiability, Maintainability |

Projects often operate with **two requirements documents**:
1. The **client's requirements document**
2. The **developer's additional requirements document**

---

## 4. SQA System Architecture — Overview (Ch 4)

### 4.1 Six Classes of SQA Components

The SQA system is organized into **six classes**, forming the "SQA Architecture":

```
┌──────────────────────────────────────────────────────┐
│                   SQA SYSTEM                          │
├───────────────────┬──────────────────────────────────┤
│ 1. PRE-PROJECT    │ Contract review                  │
│    COMPONENTS     │ Development & quality plans      │
├───────────────────┼──────────────────────────────────┤
│ 2. PROJECT LIFE   │ Reviews (formal DRs, peer)       │
│    CYCLE          │ Expert opinions                  │
│    COMPONENTS     │ Software testing                 │
│                   │ Software maintenance             │
│                   │ External participants' quality   │
├───────────────────┼──────────────────────────────────┤
│ 3. INFRASTRUCTURE │ Procedures & work instructions   │
│    (Error         │ Templates & checklists           │
│    Prevention &   │ Staff training & certification   │
│    Improvement)   │ Preventive & corrective actions   │
│                   │ Configuration management         │
│                   │ Documentation control            │
├───────────────────┼──────────────────────────────────┤
│ 4. MANAGEMENT     │ Project progress control         │
│    COMPONENTS     │ Software quality metrics         │
│                   │ Software quality costs           │
├───────────────────┼──────────────────────────────────┤
│ 5. STANDARDS,     │ Quality management standards     │
│    CERTIFICATION, │   (SEI CMM, ISO 9001/9000-3)    │
│    & ASSESSMENT   │ Project process standards        │
│                   │   (IEEE 1012, ISO/IEC 12207)     │
├───────────────────┼──────────────────────────────────┤
│ 6. HUMAN          │ Management's role                │
│    COMPONENTS     │ SQA unit                         │
│    (Organizing    │ SQA trustees, committees, forums │
│     for SQA)      │ Software testers                 │
└───────────────────┴──────────────────────────────────┘
```

### 4.2 Pre-Project Components

Initiated before project work begins:

- **Contract review:** Examination of proposal draft and contract drafts — clarifies requirements, reviews schedule/resources, evaluates staff capability, customer obligations, and development risks. Applies equally to maintenance contracts.
- **Development and quality plans:** Prepared after contract signing. The **development plan** covers schedules, resources, risks, team organization, methodology, and reuse plans. The **quality plan** covers quality goals (measurable), stage entry/exit criteria, and lists of reviews, tests, and verification/validation activities.

### 4.3 Project Life Cycle Components

#### Reviews
- **Formal Design Reviews (DRs):** Conducted by ad hoc committees of senior professionals. The DR report includes "action items." Outcomes: immediate approval, conditional approval (after action items), or requirement for an additional DR.
- **Peer Reviews (Inspections & Walkthroughs):** Review short documents or code modules. Peers (not superiors) detect faults. Output: list of detected faults; inspections also produce defect summaries and statistics for process improvement. Often operate on a reciprocity basis.

#### Expert Opinions
External experts are engaged when:
- In-house capabilities are insufficient
- The organization is too small for internal DR teams
- Work pressures prevent internal inspection
- In-house professionals are temporarily unavailable
- Major disagreements among senior staff need resolution

#### Software Testing
- Formal SQA evaluation of actual running software using prepared test cases
- Tests modules, integration, and entire systems
- **Regression tests** re-run after corrections
- Should be performed by an **independent testing unit** (not the project team) to avoid conflicts of interest

#### Software Maintenance Components
Maintenance categories:
- **Corrective:** Bug fixes, user support
- **Adaptive:** Adaptation to new hardware/circumstances
- **Functionality improvement:** Limited enhancements and improvements

SQA components for maintenance mirror development: pre-maintenance (contract review, plan), development life cycle components (for functionality improvements), infrastructure components, and managerial controls.

#### External Participants' Quality
Controls over subcontractors, COTS suppliers, and customer-supplied parts are defined in contracts. Special SQA efforts are needed when external participants use lower quality-assurance standards than the supplier.

### 4.4 Infrastructure Components (Error Prevention & Improvement)

These organization-wide components aim to lower fault rates and improve productivity:

| Component | Purpose |
|---|---|
| **Procedures & work instructions** | Detailed definitions for specific development activities, based on accumulated experience |
| **Templates & checklists** | Save time, contribute to completeness, improve communication between teams |
| **Staff training, instruction & certification** | New employees, retraining, continuous professional updates, knowledge certification |
| **Preventive & corrective actions** | Systematic study of failure/success data from DR reports, test reports, and customer complaints to prevent recurrence |
| **Configuration management** | Control the change process — approval, recording, version/release management, preventing unauthorized changes to issued versions |
| **Documentation control** | Ensure long-term availability of controlled documents (requirements, contracts, design reports, plans, standards) and quality records |

### 4.5 Management SQA Components

| Component | Focus |
|---|---|
| **Project progress control** | Detects deviations from plans early. Monitors resource usage, schedules, risk management, and budget. |
| **Software quality metrics** | Measures of functional quality, productivity, and organizational aspects. Covers fault density, schedule deviations, team productivity. |
| **Software quality costs** | Extended model: **control costs** (prevention, appraisal, managerial) + **failure costs** (internal, external, managerial). Used to direct SQA investment to high-return activities. |

> Up to a certain point, expanding control-cost investment yields **larger savings** in failure costs, reducing total quality costs.

### 4.6 Standards, Certification, and Assessment

**Objectives:** utilize international knowledge, improve inter-organizational coordination, and enable objective assessment.

| Sub-class | Examples | Nature |
|---|---|---|
| **Quality management standards** | SEI CMM, ISO 9001, ISO 9000-3 | "What" is required; leave "how" to the organization. Enable certification. |
| **Project process standards** | IEEE 1012, ISO/IEC 12207 | "How" — methodological guidelines for the development team. |

### 4.7 Organizing for SQA — The Human Components

The **SQA organizational base** includes:

| Role | Responsibilities |
|---|---|
| **Management** | Define quality policy, allocate resources, follow-up compliance, resolve schedule/budget/customer issues |
| **SQA Unit** | Full-time SQA staff. Prepares annual quality programs, consults, conducts internal audits, leads committees, develops and maintains SQA infrastructure |
| **SQA Trustees** | Team members with special interest in quality. Solve local problems, detect deviations, initiate improvements, report to SQA unit |
| **SQA Committees & Forums** | Appointed members from various units. Solve quality problems, analyze failure records, initiate corrective/preventive actions, develop new procedures and SQA components |

---

## Summary of Key Takeaways

1. **Software is fundamentally different** from other industrial products — higher complexity, invisibility, and a one-phase-only defect-detection window. This demands a specialized SQA methodology.

2. **Professional SQA operates in a demanding environment** characterized by contracts, customer relationships, teamwork, multi-team coordination, interfaces, turnover, and long maintenance lifecycles — requiring both professional and managerial excellence.

3. **Software quality encompasses more than code** — procedures, documentation, and data are equally part of "software" and equally subject to quality assurance.

4. **Errors → Faults → Failures** is a filtering chain: not all errors become faults, and not all faults become failures. Dormant faults can activate years later when conditions change.

5. **Nine causes of software errors** span the entire development lifecycle; all are ultimately human-caused.

6. **Software quality** is conformance to functional requirements, explicit standards, and implicit GSEP (Pressman). **SQA** (Galin's expanded definition) is a systematic, planned set of actions covering both development and maintenance, addressing both functional and managerial (schedule/budget) requirements.

7. **McCall's 11-factor model** (Correctness, Reliability, Efficiency, Integrity, Usability, Maintainability, Flexibility, Testability, Portability, Reusability, Interoperability) remains a practical framework. Alternative models add Verifiability, Safety, and Manageability.

8. **The SQA system** is built from six classes of components: pre-project, project life cycle, infrastructure, management, standards/certification, and human/organizational. These work together to prevent errors, detect them early, and continuously improve the process.
