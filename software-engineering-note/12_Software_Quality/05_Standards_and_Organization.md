---
title: SQA Standards & Organization
aliases:
  - SQA Standards
  - Quality Management Standards
  - SQA Organizational Framework
tags:
  - quality
  - iso-9000
  - cmmi
  - ieee-standards
  - sqa-unit
  - software-quality
source: "Galin SQA Ch 23–26"
created: 2026-07-21
---

# SQA Standards & Organization

Comprehensive reference covering **quality management standards** (ISO 9001/9000-3, CMM/CMMI, Bootstrap, SPICE/ISO 15504), **IEEE project process standards** (12207, 1012, 1028), **management's role in SQA**, and the **SQA organizational framework** (unit, trustees, committees, forums).

---

## I. Standards: Classification and Benefits

### 1.1 Two Classes of SQA Standards

| Characteristic | **Quality Management Standards** | **Project Process Standards** |
|---|---|---|
| **Target** | Management of software development and SQA units | Software development/maintenance project team |
| **Focus** | Organization of SQA systems, infrastructure, requirements | Methodologies for carrying out projects |
| **Objective** | "**What**" to achieve | "**How**" to perform |
| **Goal** | Assure supplier's software quality and assess process capability | Assure quality of a specific software project |
| **Examples** | ISO 9000-3, SEI CMM, ISO/IEC 15504 | ISO/IEC 12207, IEEE Std 1012, IEEE Std 1028 |

### 1.2 Standards-Developing Organizations

- **IEEE** (Institute of Electrical and Electronics Engineers) Computer Society
- **ISO** (International Organization for Standardization)
- **DOD** (US Department of Defense) — replaced MIL-STD-498 with IEEE/EIA 12207
- **ANSI** (American National Standards Institute)
- **IEC** (International Electrotechnical Commission)
- **EIA** (Electronic Industries Association)

**Trends:** Joint standards (IEEE/ANSI, ISO/IEC, IEEE/ISO); adoption of international standards as national standards; convergence toward three universal standards: ISO/IEC 9000-3, ISO/IEC 15504, ISO/IEC/IEEE 12207.

### 1.3 Three Ways Organizations Contribute to SQA

1. **Provision of standards** — documented methodologies for professionals and managers (ISO for management standards, IEEE for professional/engineering standards)
2. **SQA certification** — independent professional quality audits (ISO 9000 Certification Service)
3. **Professional support / self-assessment** — tools for self-evaluation of SQA systems (CMM by SEI, ISO/IEC 15504)

---

## II. Quality Management Standards (Ch 23)

### 2.1 ISO 9001 and ISO 9000-3

ISO 9000-3 adapts the general ISO 9000 quality management methodology to software development and maintenance. From the 1997 edition onward, ISO 9000-3 is a **stand-alone "all-inclusive" standard** for the software industry.

#### Eight Guiding Principles (ISO 9000:2000)

| # | Principle | Description |
|---|---|---|
| 1 | **Customer focus** | Understand current and future customer needs |
| 2 | **Leadership** | Create and maintain an internal environment for achieving objectives |
| 3 | **Involvement of people** | Full involvement at all levels for the organization's benefit |
| 4 | **Process approach** | Manage activities and resources as a process |
| 5 | **System approach to management** | Manage interrelated processes as a system |
| 6 | **Continual improvement** | Ongoing improvement of overall performance |
| 7 | **Factual approach to decision making** | Decisions based on data and information analysis |
| 8 | **Mutually supportive supplier relationships** | Interdependence enhances value creation |

#### ISO 9000-3 Requirements (New Edition — 5 Classes)

| Class | Key Subjects |
|---|---|
| **4. Quality Management System** | General requirements, Documentation requirements |
| **5. Management Responsibilities** | Management commitment, Customer focus, Quality policy, Planning, Responsibility/authority/communication, Management review |
| **6. Resource Management** | Provision of resources, Human resources, Infrastructure, Work environment |
| **7. Product Realization** | Planning, Customer-related processes, Design & development, Purchasing, Production & service provision, Control of monitoring & measuring devices |
| **8. Measurement, Analysis & Improvement** | Monitoring & measurement, Control of non-conforming product, Data analysis, Improvement |

#### TickIT Initiative

- Launched in late 1980s by the UK software industry + UK Department for Trade and Industry
- Developed methodology for adapting ISO 9001 to software industry
- Managed by BSI DISC; accredited for IT certification in UK and Sweden
- ~1,252 client organizations in 42 countries (as of 2002)
- Publishes the **TickIT Guide** (Edition 5.0); auditors registered through IRCA

### 2.2 ISO 9000-3 Certification Process

Five stages to obtain and maintain certification:

```
Decision → Planning → Development → Implementation → Certification Audits
                                                         ↓
                                              Periodic Re-certification (1–2×/year)
```

| Stage | Key Activities |
|---|---|
| **1. Planning** | Internal survey of current SQA system gaps (procedures, staff knowledge, documentation, configuration management, SQA unit capabilities); construct action plan with timetable and resource estimates |
| **2. Development of SQA System** | Quality manual & SQA procedures; staff training/certification programs; CAB committee; configuration management; documentation/quality record controls; project progress control system |
| **3. Implementation** | Staff instruction programs; support services for SQA tools; internal quality audits to verify implementation success |
| **4. Certification Audits** | (a) Review of quality manual and SQA procedures for completeness/accuracy; (b) Verification audits — staff knowledge, procedure implementation, documentation compliance; based on random project/team selection |
| **5. Retention** | Periodic re-certification audits (1–2×/year); must demonstrate continuing development, quality/productivity improvements, procedure updates |

### 2.3 CMM and CMMI Assessment Methodology

Developed by SEI (Carnegie Mellon University), first released 1992 (feedback), public version 1993.

#### CMM Principles

- More elaborate management methods (quantitative) → increased quality control and productivity
- **Five-level maturity model** enables evaluation and direction for improvement
- **Process areas are generic** — define "what" not "how" (any life cycle, methodology, tool, language, documentation standard)

#### CMM Five Levels and Key Process Areas (KPAs)

| Level | Name | Key Process Areas |
|---|---|---|
| **1** | Initial | No KPAs required |
| **2** | Repeatable | Requirements management, Project planning, Project tracking/oversight, Subcontract management, SQA, Configuration management |
| **3** | Defined | Organization process focus/definition, Training program, Integrated software management, Software product engineering, Inter-group coordination, Peer reviews |
| **4** | Managed | Quantitative process management, Software quality management |
| **5** | Optimizing | Defect prevention, Technology change management, Process change management |

#### CMM Evolution → CMMI

Specialized CMM variants developed: SE-CMM (systems engineering), T-CMM (trusted/classified), SSE-CMM (security), P-CMM (people), SA-CMM (acquisition), IPD-CMM (integrated product development).

**CMMI (Capability Maturity Model Integration)** — late 1990s — solved coordination problems of multiple CMM variants. Three integrated models:
- CMMI-SE/SW (systems + software)
- CMMI-SE/SW/IPPD/SS (+ integrated product/process + supplier sourcing)
- CMMI-SE/SW/IPPD

**18 KPAs → 25 Process Areas (PAs)**, with CMMI Level 4 renamed to "Quantitatively Managed."

#### CMMI Process Areas by Level

| Level | Process Areas |
|---|---|
| **2: Managed** | RM, PP, PMC, SAM, MA, PPQA, CM |
| **3: Defined** | RD, TS, PI, VER, VAL, OPF, OPD, OT, IPM, IT, RM (Risk Mgmt), DAR, OEI |
| **4: Quantitatively Managed** | OPP, QPM |
| **5: Optimizing** | OID, CAR |

#### CMM Implementation Results

| Organization | Key Results |
|---|---|
| **Boeing Space Transportation** | Defect detection shifted from 89% late (testing) → 83% early (reviews); 31% rework decrease; ~100% pre-release defect elimination; 140% productivity increase |
| **Tata Consultancy Services** | Rework: 12% → 4%; Schedule slippage: >3% → <2.5%; Review effectiveness: 40% → 80% |
| **Telcordia Technologies** | 94% reduction in field fault density; 98% on-schedule major releases; Customer satisfaction: 60% → >95% |
| **Raytheon** (via Gartner) | Original work grew from 34% (Level 1) → 76% (Level 4); Rework dropped from 41% → 7% |

#### CMM Level Transition Times (Gartner data)

| Transition | Mean Time | Organizations |
|---|---|---|
| Level 1 → 2 | 24 months | 125 |
| Level 2 → 3 | 21.5 months | 124 |
| Level 3 → 4 | 33 months | 18 |
| Level 4 → 5 | 18 months | 19 |

### 2.4 Bootstrap Methodology

European initiative (ESPRIT + ESI), operated by the Bootstrap Institute.

- **31 quality attributes** grouped into 3 classes: **Process, Organization, Technology**
- **5-grade scale** applied per attribute
- Supports: CMM evaluation, ISO 15504 (SPICE) evaluation, ISO 9000-3 gap assessment
- **Three-tier assessor accreditation**: Trained Assessor → Assessor → Lead Assessor
- Maintains an **anonymous Bootstrap database** of assessment results for member benchmarking

### 2.5 SPICE Project and ISO/IEC 15504

Joint ISO/IEC initiative (1993) to harmonize assessment methodologies. TR version released 1998; working toward full international standard.

#### Guiding Principles

- **Harmonize** existing assessment methodologies (framework model: "what" not "how")
- **Universal** applicability to all software suppliers, customers, and categories
- **Highly professional**
- **International acceptance** as a world standard

#### Six Capability Levels

| Level | Name | Process Attributes |
|---|---|---|
| **0** | Incomplete | No requirements — little or no implementation |
| **1** | Performed | Process performance (identify processes, inputs, outputs) |
| **2** | Managed | (a) Performance management; (b) Work products management |
| **3** | Established | (a) Process definition; (b) Process resources |
| **4** | Predictable | (a) Measurement; (b) Process control |
| **5** | Optimizing | (a) Process change; (b) Continuous improvement |

#### Achievement Grade Scale

| Grade | Rating | Achievements |
|---|---|---|
| **F** (Fully) | 86–100% | Systematic and complete performance |
| **L** (Largely) | 51–85% | Significant achievement; some low performance areas |
| **P** (Partially) | 16–50% | Some achievement; other aspects uncontrolled |
| **N** (Not) | 0–15% | Little or no achievement |

#### Accumulative Requirements (for a given level)

- Process attributes of **target level** must be ≥ **L** (Largely)
- Process attributes of **all lower levels** must be **F** (Fully)

#### Structure (9 Parts)

Parts 1–9: Concepts & guide, Reference model, Performing assessment, Guide to performing, Assessment model & indicator guide, Competency of assessors, Process improvement, Supplier process capability, Vocabulary.

#### 29 Processes in 5 Subject Areas

| Subject Area | # Processes | Focus |
|---|---|---|
| **CUS** (Customer–Supplier) | 5 | Acquisition, needs, supply, operation, customer service |
| **ENG** (Engineering) | 7 | System/software requirements, design, implementation, integration & test, maintenance |
| **SUP** (Support) | 8 | Documentation, configuration, QA, verification, validation, joint reviews, audits, problem resolution |
| **MAN** (Management) | 4 | Project, quality, risk, subcontractor management |
| **ORG** (Organization) | 5 | Business engineering, process definition/improvement, human resources, engineering infrastructure |

#### Trials

Three-phase trial (1995–2000), >200 full-scale assessments across continents and software specializations. Validated conformity, usability, and accumulated experience for transforming the TR into an international standard.

---

## III. IEEE Project Process Standards (Ch 24)

### 3.1 IEEE Standards Classification

| Class | Description | Examples |
|---|---|---|
| **Conceptual** | Guiding principles and overall approach | IEEE 610.12 (Glossary), 1061 (Quality Metrics Methodology), 12207.0 (Life Cycle Processes) |
| **Prescriptive** | Conformance requirements | IEEE 828 (SCM Plans), 829 (Test Documentation), 1012 (V&V), 1028 (Reviews) |
| **Guidance** | Implementation guides | IEEE 1233 (System Requirements), 12207.1 (Life Cycle Data), 12207.2 (Implementation) |

### 3.2 IEEE/EIA Std 12207 — Software Life Cycle Processes

**Framework standard** covering the entire spectrum of software life cycle processes. Product of joint efforts by DOD, ANSI, IEEE, EIA, ISO, IEC.

#### Architecture (4-Level Tree)

```
Process Classes → Processes → Activities → Tasks
```

#### Three Process Classes

| Class | Processes |
|---|---|
| **Primary** | Acquisition, Supply, Development, Operation, Maintenance |
| **Supporting** | Documentation, Configuration Management, Quality Assurance, Verification, Validation, Joint Review, Audit, Problem Resolution |
| **Organizational** | Management, Infrastructure, Improvement, Training |

#### Key Concepts

**General:**
- **Tailoring** — adapt to project size, complexity; not for COTS
- **All participants** — acquirers, suppliers, developers, operators, maintainers
- **"How to do" not "exactly how to do"** — flexibility for life cycle model, tools, metrics, milestones, documentation
- **Software-system links** at each life cycle phase
- **TQM consistency** — quality integral to every process
- **No certification** requirement — supports worldwide acceptance
- **Baselining** — progressively improved configuration versions

**Task-related:**
- Responsibility assigned to specific unit/individual
- Modular life cycle components
- Four conformance levels: **will**, **shall**, **should**, **may**

### 3.3 IEEE Std 1012 — Verification and Validation (V&V)

Defines processes for determining whether a software product conforms to specifications (**verification**) and satisfies intended use objectives (**validation**).

#### Four Software Integrity Levels

| Level | Criticality |
|---|---|
| **High** | Affects critical system performance |
| **Major** | Affects important system performance |
| **Moderate** | Affects performance but alternative operation method exists |
| **Low** | Only inconveniences the user |

Higher integrity → more V&V tasks required.

#### Key Concepts

- **Broad V&V definition** — reviews, testing, evaluation, hazard identification, risk analysis
- **Integrity-level-graded** V&V requirements
- **Prescriptive** — lists tasks with methodology, inputs, outputs per activity
- **IV&V Independence**: Managerial (separate from dev management), Technical (separate team with own tools), Financial (independent budget)
- **V&V Metrics**: (a) Evaluation of development processes/products; (b) Quality/coverage evaluation of V&V activities
- **Quantitative criteria**: correctness, consistency, completeness, accuracy, readability, testability
- **Reusable software V&V** — special considerations for COTS and library software
- Compliance with IEEE/EIA 12207 and ISO/IEC 12207

#### SVVP (Software V&V Plan) Outline

1. Purpose
2. Referenced Documents
3. Definitions
4. V&V Overview (Organization, Master Schedule, Integrity Level Scheme, Resources, Responsibilities, Tools/Techniques/Methods)
5. V&V Processes (Management, Acquisition, Supply, Development, Operation, Maintenance)
6. V&V Reporting Requirements
7. V&V Administrative Requirements (Anomaly Resolution, Task Iteration Policy, Deviation Policy, Control Procedures, Standards/Practices/Conventions)
8. V&V Documentation Requirements

### 3.4 IEEE Std 1028 — Software Reviews

Defines **how to perform a systematic review** — a review performed by a team according to a documented procedure producing documented results.

#### Five Types of Systematic Reviews

1. **Management reviews**
2. **Technical reviews** (formal design reviews)
3. **Inspections**
4. **Walkthroughs**
5. **Audits**

#### Key Concepts

- **High formality** — authorization and documentation requirements
- **Follow-up** — mandatory for all review types; corrections must be approved
- **Compliance** with IEEE 12207, ISO/IEC 9000-3, IEEE 1012

#### Nine-Component Review Structure

| Component | Content |
|---|---|
| **1. Introduction** | Purpose, typical examples of software products |
| **2. Responsibilities** | Mandatory and optional participants (e.g., technical review: decision-maker, review leader, recorder, technical staff) |
| **3. Input** | Mandatory/optional data items (objectives, software product, standards) |
| **4. Entry Criteria** | Authorization preconditions (objectives stated, input data available) |
| **5. Procedure** | Management preparations, planning, team preparation, examination, follow-up |
| **6. Exit Criteria** | Procedures completed, corrections approved, documentation done |
| **7. Output** | Mandatory output items per review type |
| **8. Data Collection** | Anomaly data (classified/ranked) for inspections and walkthroughs |
| **9. Improvements** | Analyze accumulated data → improved procedures, updated checklists, better processes |

---

## IV. SQA Organizational Framework

### 4.1 Three Management Levels

| Level | Roles |
|---|---|
| **Top Management** | General manager, executives (CEOs), including the executive in charge of software quality |
| **Department Management** | Managers of software development, maintenance, and testing departments |
| **Project Management** | Project managers, team leaders |

### 4.2 SQA Framework Actors

| Category | Actors |
|---|---|
| **Managers** | Top executives (especially SQA executive), department managers, testing department managers, project managers, team leaders, testing team leaders |
| **Testers** | Software testing team members |
| **SQA Professionals & Practitioners** | SQA unit members, SQA trustees, SQA committee members, SQA forum members |

Only **SQA unit members** and **software testing department staff** are full-time on SQA. Others contribute part-time or volunteer.

---

## V. Management's Role in SQA (Ch 25)

### 5.1 Top Management Responsibilities

**Three main tools:**

1. **Software Quality Policy** — communicates:
   - Conformity to organization's purpose/goals
   - Commitment to SQA concepts and adopted quality standards
   - Commitment to adequate resources
   - Commitment to continuous improvement

2. **Executive in Charge of Software Quality** — responsibilities:
   - Prepare annual SQA activities program and budget
   - Prepare SQA system development plans
   - Overall control of SQA program and development project implementation
   - Present and advocate SQA issues to executive management

3. **Management Reviews** — periodic meetings (1–2×/year) to assess:
   - Compliance with quality policy
   - Achievement of quality objectives
   - Need for updates and improvements
   - Major deficiencies and solutions
   - Additional resource allocation

   **Management review report** (prepared by SQA unit) includes: progress on previous recommendations, quality metrics, customer satisfaction feedback, audit findings, risks, improvement recommendations.

### 5.2 Department Management Responsibilities

**Quality system-related:**
- Prepare department's annual SQA program and budget
- Prepare department's SQA development plans
- Control performance of SQA activities and projects
- Present department's SQA issues to top management

**Project-related:**
- Control compliance to QA procedures (CAB, SCM, SCCA)
- Contract review and proposal approval follow-up
- Review activities and project phase completion approval
- Software test follow-up and product approval
- Schedule/budget deviation tracking
- Support project managers in resolving difficulties
- Maintenance service quality follow-up
- Risk follow-up
- Customer satisfaction monitoring
- Approval of large change orders and specification deviations

### 5.3 Project Management Responsibilities

**Professional hands-on:**
- Prepare/update project and quality plans
- Participate in joint customer–supplier committees
- Follow up project team staffing (recruitment, training, instruction)

**Management follow-up:**
- Review activities and corrections
- Development, integration, and system testing
- Acceptance tests
- Installation and running-in at customer sites
- SQA training/instruction of team members
- Schedule and resource allocation
- Customer requests and satisfaction
- Project risks and their solutions

---

## VI. The SQA Unit and Other Actors (Ch 26)

### 6.1 SQA Unit Organizational Structure

```
                     Head of SQA Unit
                           │
         ┌─────────────────┼─────────────────┐
         │                 │                 │
   SQA Operations    SQA Development
         │            & Maintenance
   ┌─────┼─────┐    ┌──────┼──────┐
   │     │     │    │      │      │
Project  Infra  Audits  Standards  Engineering  Information
Life     -struc & Cert  & Proce-   Development  Systems
Cycle    ture           dures      & Maint.
         Ops
```

#### 6.1.1 Head of SQA Unit — Tasks

| Category | Tasks |
|---|---|
| **Planning** | Annual activity program & budget; quality management system planning; recommended department SQA programs and development plans |
| **Management** | Team activity management; SQA program monitoring; nomination of team members, committee members, trustees; periodic reports |
| **External Contacts** | Customer address for quality issues; representation before external bodies; management review reports; SQA issues for top management |
| **Professional** | Participation in joint committees, formal design reviews; deviation approval; consultation to project managers; SQA committees and forums |

#### 6.1.2 SQA Sub-Units

| Sub-Unit | Key Tasks |
|---|---|
| **Project Life Cycle** | Control: compliance follow-up, product approval, maintenance monitoring, customer satisfaction; Participation: contract reviews, quality plans, design reviews, testing, installation |
| **Infrastructure Operations** | Publication of procedures/templates/checklists; training/instruction; trustee instruction; monitoring new procedure implementation; staff certification follow-up; CAB participation; configuration management; documentation compliance |
| **Audits & Certification** | Internal audits (program, performance, follow-up, summary reports); subcontractor/supplier audits; coordination for external certification audits; customer audit support |
| **Support** | Consulting on: project/quality plans, review team staffing, development methodologies/tools, risk solutions, schedule/budget remedies, SQA metrics, SQA information systems |
| **Standards & Procedures** | Annual program for new/updated procedures; procedure development; follow-up of standards changes; initiation of adaptations |
| **Engineering Development** | Testing new tools/versions; evaluating new methods; solving tool/method difficulties; developing quality/productivity measurement methods; technological support to CAB |
| **Information Systems** | SQA IS for data collection/processing/reporting; quality metrics and cost estimation; IS updates; organization's SQA intranet/internet site |

### 6.2 SQA Trustees

Staff members who **volunteer part of their time** to promoting quality, instructed by the SQA unit.

**Unit-related tasks:**
- Support colleagues in SQA procedure implementation
- Help unit manager with SQA tasks
- Promote compliance and monitor SQA procedures
- Report substantial non-compliance and severe quality failures to SQA unit

**Organization-related tasks:**
- Initiate changes/updates to organization-wide SQA procedures
- Initiate improvements and CAB applications for recurrent failures
- Identify training needs and propose programs

### 6.3 SQA Committees

| Type | Scope | Examples |
|---|---|---|
| **Permanent** | Integral part of SQA framework, defined in procedures | SCC (software change control), CA (corrective actions), procedures, methods, development tools, quality metrics |
| **Ad hoc** | Short-term per-problem basis; members nominated by relevant body | Specific procedure updates, software failure analysis, targeted metrics, quality cost updates |

### 6.4 SQA Forums

**Informal, volunteer-based** communities — not subject to standard requirements or procedures.

**Typical focus areas:**
- SQA procedure improvements and implementation
- Quality metrics
- Corrective actions (failure/success case analysis)
- Quality system issues (new tool development)
- Quality line management problems

**Characteristics:**
- Scope and operation mode defined by members
- May be **closed** (e.g., quality line managers only) or **open** to all
- May publish newsletters, periodic reviews, task force reports with recommendations
- Exemplified by the **"Template Forum"** — 4 team leaders created a set of templates for 11 teams, issued ~20 templates over 3 years

---

## VII. Summary: Key Relationships

```
                    SQA Standards
                         │
         ┌───────────────┴───────────────┐
         │                               │
  Quality Management              Project Process
  ("What")                       ("How")
         │                               │
    ┌────┼────┐                   ┌──────┼──────┐
    │    │    │                   │      │      │
  ISO   CMM  SPICE             IEEE    IEEE    IEEE
 9000-3 CMMI 15504            12207   1012    1028
 (Cert) (Assess) (Assess)    (Life    (V&V)  (Reviews)
                              Cycle)

                    SQA Organization
                         │
         ┌───────────────┼───────────────┐
         │               │               │
   Top Mgmt        Dept Mgmt       Project Mgmt
   (Policy,        (Program        (Plans,
   Exec,           control,        reviews,
   Reviews)        approvals)      follow-up)
                         │
              SQA Professionals & Practitioners
         ┌───────────────┼───────────────┐
         │               │               │
    SQA Unit        Trustees        Committees
   (7 sub-units)   (Volunteers)    (Perm + Ad hoc)
                         │
                      Forums
                    (Informal)
```

---

## References

- Galin, D. (2004). *Software Quality Assurance: From Theory to Implementation*. Pearson Education. Chapters 23–26.
- ISO 9000-3:1997 — Guidelines for the Application of ISO 9001 to Software
- ISO/IEC 15504 (SPICE) — Software Process Assessment
- IEEE Std 12207 — Software Life Cycle Processes
- IEEE Std 1012-1998 — Software Verification and Validation
- IEEE Std 1028-1997 — Software Reviews
- SEI CMMI v1.1 (2002)
