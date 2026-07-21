---
quality: review
reviews: inspection
inspections: walkthrough
procedures: templates
training: capa
software-quality: configuration-management
source: "Galin SQA Ch 8, 14–19"
created: 2026-07-21
tags:
  - software-quality
  - reviews
  - inspections
  - procedures
  - training
  - CAPA
  - configuration-management
  - documentation-control
  - SQA-infrastructure
---

# 03 — Reviews & SQA Infrastructure

> **Source:** Daniel Galin, *Software Quality Assurance: From Theory to Implementation*  
> **Chapters:** 8 (Reviews), 14 (Procedures & Work Instructions), 15–19 (Infrastructure Components)  
> **IEEE Reference:** IEEE Std 1028-1997 (Software Reviews), ISO 9000-3

---

## 1. Review Objectives (Ch 8.1)

Reviews are **structured examinations of work products** by people not directly involved in creating them. They provide **early detection** of analysis and design errors, preventing defects from propagating downstream where correction is far more costly.

> **IEEE Definition (1990):** *"A process or meeting during which a work product is presented to project personnel, managers, users, customers, or other interested parties for comment or approval."*

### Direct Objectives

| # | Objective | Description |
|---|-----------|-------------|
| 1 | **Detect errors** | Find analysis/design errors and identify subjects needing corrections, changes, or completions |
| 2 | **Identify new risks** | Discover risks likely to affect project completion |
| 3 | **Locate deviations** | Find departures from templates, style procedures, and conventions — restoring uniformity improves communication and coordination |
| 4 | **Approve the product** | Grant formal approval allowing the team to continue to the next development phase |

### Indirect Objectives

| # | Objective | Description |
|---|-----------|-------------|
| 1 | **Knowledge exchange** | Provide an informal meeting place for sharing professional knowledge about development methods, tools, and techniques |
| 2 | **Corrective action data** | Record analysis/design errors that serve as a basis for future corrective actions, improving development methods (→ Ch 17) |

> **Key insight:** For better "filtering out" of errors, a **double or triple "net"** of review methods should be applied. Each method emphasizes different objectives to varying degrees.

---

## 2. Formal Design Reviews — DRs (Ch 8.2)

**Formal Design Reviews** (also called "design reviews", "DRs", or "formal technical reviews — FTR") are the **only reviews with the authority to approve** a design product. Without approval, the development team cannot proceed to the next phase.

### 2.1 Common Formal Design Reviews

| Acronym | Review Type |
|---------|-------------|
| DPR | Development Plan Review |
| SRSR | Software Requirement Specification Review |
| PDR | Preliminary Design Review |
| DDR | Detailed Design Review |
| DBDR | Database Design Review |
| TPR | Test Plan Review |
| STPR | Software Test Procedure Review |
| VDR | Version Description Review |
| OMR | Operator Manual Review |
| SMR | Support Manual Review |
| TRR | Test Readiness Review |
| PRR | Product Release Review |
| IPR | Installation Plan Review |

### 2.2 Participants

#### Review Leader
Must possess:
- Knowledge and experience in development of similar projects (prior acquaintance with the current project is **not** necessary)
- Seniority at a level similar to or higher than the project leader
- Good relationship with the project leader and team
- A position **external to the project team**

Suitable candidates: department manager, chief software engineer, leader of another project, head of SQA unit, or (in special cases) the customer's chief software engineer.

> ⚠️ **Anti-pattern:** Appointing the project leader as review leader. This tends to limit the scope of review, avoid incisive criticism, and undermine the review's purpose.

#### Review Team
- Selected from senior project team members, senior professionals from other projects/departments, customer–user representatives, and consultants
- **Non-project staff should form the majority**
- Optimal size: **3–5 members** (larger teams create coordination problems and reduce preparation quality)

### 2.3 Preparations

| Participant | Responsibilities |
|-------------|-----------------|
| **Review Leader** | Appoint team members; schedule sessions; distribute design document |
| **Review Team** | Review the document; list comments prior to session; use **checklists** (→ Ch 15) |
| **Development Team** | Prepare a **short presentation** focusing on main professional issues, not general project description |

> Sessions should be scheduled **shortly after** document distribution to prevent schedule delays.

### 2.4 The DR Session

**Agenda:**
1. Short presentation of the design document
2. Comments by review team members
3. Verification and validation — discussing each comment to determine required actions
4. **Decision** about the design product

**Three possible decisions:**
- **Full approval** — immediate continuation to next phase (possibly with minor corrections)
- **Partial approval** — continuation for some parts; major action items required for remaining parts
- **Denial of approval** — repeat the DR (applied for multiple major/critical defects)

> ⚠️ **Anti-pattern:** The "comprehensive presentation" — an excessively long presentation that exhausts the team and leaves no time for discussion.

### 2.5 Post-Review Activities

#### DR Report
Issued immediately after the session, containing:
- Summary of review discussions
- Decision about project continuation
- Full list of required actions (corrections, changes, additions) with completion dates and responsible persons
- Name(s) of review team member(s) assigned to follow up

#### Follow-Up
The appointed person verifies that each action item has been satisfactorily accomplished. Follow-up must be **fully documented**.

> ⚠️ **Red flags in DR reports:** extremely short reports with no defects listed; reports listing action items but no follow-up schedule; reports with no documented follow-up activities.

### 2.6 Pressman's 13 Golden Guidelines

**Infrastructure:**
- Develop checklists for each type of design document
- Train senior professionals in technical and review process issues
- Periodically analyze past DR effectiveness
- Schedule DRs as part of the project activity plan

**Team:**
- Limit review teams to 3–5 members

**Session:**
- Discuss professional issues constructively; avoid personalizing
- Keep to the agenda
- Focus on **defect detection**, not solution design
- When disagreement occurs, note the issue and move it to another forum
- Properly document discussions
- Session duration: **≤ 2 hours**

---

## 3. Peer Reviews — Inspections & Walkthroughs (Ch 8.3)

Peer reviews differ from DRs in **participants** (peers, not superiors) and **authority** (cannot approve design documents — focus on error detection and standards compliance).

Despite the rise of CASE tools, empirical research consistently shows peer reviews are **highly efficient and effective**.

### 3.1 Inspection vs. Walkthrough

| Aspect | Inspection | Walkthrough |
|--------|-----------|-------------|
| **Formality** | More formal | Less formal |
| **Emphasis** | Corrective action; improving development methods | Comments on the reviewed document |
| **Infrastructure** | Checklists, defect frequency tables, periodic effectiveness analysis, scheduled in project plan | Minimal |
| **Training** | Formal moderator training required | Not formally required |
| **Post-review** | Follow-up of corrections; reports to CAB (Corrective Action Board) | No follow-up of corrections |

> Gilb & Graham (1993): Walkthroughs display "far fewer defects found but at the same cost" compared to inspections.

### 3.2 Participants

#### Review Leader (Moderator/Coordinator)
Must be:
1. Well-versed in similar projects and technologies
2. On good terms with the author and team
3. External to the project team
4. Experienced in leading professional meetings
5. For inspections: formally trained as a moderator

#### Specialized Professionals

**Inspection team:**
- **Designer** — the systems analyst responsible for the design
- **Coder/Implementer** — detects defects leading to coding errors
- **Tester** — identifies design errors usually found during testing

**Walkthrough team:**
- **Standards enforcer** — locates deviations from standards and procedures
- **Maintenance expert** — focuses on maintainability, flexibility, testability, and documentation completeness
- **User representative** — examines from the user/consumer perspective

#### Team Assignments
- **Presenter:** In inspections, usually NOT the author (often the coder). In walkthroughs, typically the author.
- **Scribe:** Records defects with location, description, type (Wrong/Missing/Extra), and severity.

### 3.3 Session Process

1. **Preparation:** Inspection team members prepare thoroughly (read document, list comments); walkthrough members do a brief overview reading.
2. **Overview meeting** (inspections only): Author provides background on project, logic, processes, inputs/outputs, interfaces.
3. **Session:** Presenter reads sections; participants deliver comments; discussion confined to **error identification** (not solutions).
4. **Duration:** ≤ 2 hours per session; ≤ 2 sessions per day.

### 3.4 Error Severity Classification (MIL-STD-498)

| Severity | Description |
|----------|-------------|
| **5 — Critical** | Prevents essential capabilities; jeopardizes safety/security |
| **4** | Adversely affects essential capabilities; no work-around known |
| **3** | Adversely affects essential capabilities; work-around known |
| **2** | User/operator inconvenience; does not affect essential capabilities |
| **1 — Minor** | Any other effect |

### 3.5 Session Documentation

**Inspection** (two documents):
1. **Inspection Session Findings Report** — immediate documentation of identified errors for correction and follow-up
2. **Inspection Session Summary Report** — compiled after session series; summarizes findings, resources invested, quality/efficiency metrics; serves as input for CAB analysis

**Walkthrough:**
- Walkthrough Session Findings Report only

### 3.6 Post-Inspection Activities

1. Prompt, effective correction and reworking of all errors by the designer/author
2. Follow-up by inspection leader to verify corrections
3. Transmission of reports to the **Corrective Action Board (CAB)** for analysis (→ Ch 17)

### 3.7 Efficiency Metrics

| Metric | Formula |
|--------|---------|
| Detection efficiency | Average work-hours per defect detected |
| Defect detection density | Average defects per page of design document |
| Internal effectiveness | % of defects detected by peer review out of total defects detected |

**Empirical findings (Litton Project — Madachy):**
- Design inspections: 2.25 defects/page (requirements), 1.45 defects/page (high-level design)
- Code inspections: 1.42 defects/KLOC
- Internal effectiveness improved dramatically at Fujitsu (1977–1982): defects per 1000 lines of maintained code dropped from 0.19 to 0.02 as inspection share increased from 15% to 30%

### 3.8 Peer Review Coverage

Coverage of only **5–15%** of document pages can still significantly contribute to quality if the **right sections** are chosen:

| Include | Omit |
|---------|------|
| Sections of complicated logic | Straightforward sections |
| Critical sections (defects severely damage essential capability) | Sections of a type already reviewed several times |
| Sections dealing with new environments | Sections not expected to affect functionality |
| Sections by new/inexperienced team members | Reused design and code |
| | Repeated parts of design/code |

---

## 4. Comparison of Team Review Methods (Ch 8.4)

| Property | Formal Design Reviews | Inspections | Walkthroughs |
|----------|----------------------|-------------|-------------|
| **Main direct objectives** | Detect errors; identify new risks; approve document | Detect errors; identify deviations from standards | Detect errors |
| **Main indirect objectives** | Knowledge exchange | Knowledge exchange; support corrective actions | Knowledge exchange |
| **Review leader** | Chief software engineer or senior staff | Trained moderator (peer) | Coordinator (peer) |
| **Participants** | Top-level staff + customer representatives | Peers | Peers |
| **Project leader participation** | Yes | Yes | Yes (usually as initiator) |
| **Specialized professionals** | — | Designer, coder/implementer, tester | Standards enforcer, maintenance expert, user representative |
| **Overview meeting** | No | Yes | Yes |
| **Prep thoroughness** | Yes — thorough | Yes — thorough | Yes — brief |
| **Follow-up of corrections** | Yes | Yes | No |
| **Formal training** | No | Yes | No |
| **Use of checklists** | No (informally) | Yes | No |
| **Error data collection** | Not formally required | Formally required | Not formally required |

---

## 5. Expert Opinions (Ch 8.5)

Outside experts reinforce internal quality assurance by either:
- Preparing an expert's judgment about a document or code section
- Participating as a member of an internal review team

### When to Use External Experts

- **Insufficient in-house capabilities** in a specialized area
- **Temporary lack** of in-house professionals due to workload pressures
- **Indecisiveness** caused by major disagreements among senior professionals
- **Small organizations** with insufficient suitable candidates for review teams

---

## 6. Procedures and Work Instructions (Ch 14)

### 6.1 Conceptual Hierarchy

```
International/National SQA Standard
            ↓
  Organization's SQA Policy
            ↓
  Organization's SQA Procedures
            ↓
       SQA Work Instructions
```

### 6.2 Why Procedures and Work Instructions?

They apply the organization's **accumulated know-how, experience, and expertise** to:

- **Effectiveness & efficiency:** Perform tasks in the most effective and efficient way without deviating from quality requirements
- **Communication:** Uniformity reduces misunderstandings that lead to software errors
- **Coordination:** Simplified coordination between teams means fewer errors

### 6.3 Procedures

Procedures supply all details needed to carry out a task according to the **prescribed method**. They resolve the **Five W's**:

| Question | Issue |
|----------|-------|
| **What** | Activities to be performed |
| **How** | How each activity should be performed |
| **When** | Timing of the activity |
| **Where** | Location of the activity |
| **Who** | Responsibility for the activity |

**Standardized structure (fixed table of contents):**
1. Introduction
2. Purpose
3. Terms and abbreviations
4. Applicable documents
5. Method
6. Quality records and documentation
7. Reporting and follow-up
8. Responsibility for implementation
9. List of appendices
10. Appendices

> **Tip:** Use appendices for documentation forms, reporting forms, and tables/lists that change frequently — this allows updates without modifying the procedure itself.

### 6.4 The SQA Procedures Manual

The collection of all SQA procedures. Contents vary according to:
- Types of software development/maintenance activities
- Range of activities belonging to each type
- Range of customers and suppliers
- Conceptions governing the organization's SQA approach

The manual's structure often mirrors the **ISO 9000-3** table of contents (or equivalent standard) as a skeleton.

### 6.5 Work Instructions

Work instructions **adapt procedures** to the requirements of a specific project team, customer, or unit. Key properties:
- Provide precise details for specific contexts
- **Cannot contradict** their parent procedure
- Several work instructions can be associated with one procedure
- Can be added, changed, or cancelled **without altering** the parent procedure

**Examples:**
- *Departmental:* Audit process for new subcontractors; C++ programming instructions; design documentation templates
- *Project:* Coordination with customer; weekly progress reporting; beta site reporting follow-up

### 6.6 Preparation, Implementation, and Updating

**Preparation:**
- Start with the "procedure of procedures" — defines the conceptual and organizational framework
- Form an ad hoc committee of professionals from involved units, SQA members, and topic experts
- Alternative: hire an external consultant (adds expertise, reduces burden; risk: reduced applicability due to organization-specific characteristics)

**Implementation:**
- Distribution and instruction alone are often **insufficient** for full conformity
- Follow-up and individual instruction are mandatory for integration into daily routines

**Updating:**
- Ongoing activity by SQA team members together with relevant teams/units
- Ensures procedures adapt to changes in technology, clientele, and competition

---

## 7. Supporting Quality Devices — Templates & Checklists (Ch 15)

> *Note: Chapter 15 is not directly present in the source file; this section is compiled from cross-references in Chapters 8–14.*

### 7.1 Purpose

Supporting quality devices are standardized tools that:
- Promote **uniformity** in documentation and processes
- **Remind** reviewers and developers of all primary and secondary issues requiring attention
- Reduce the risk of overlooking important quality aspects

### 7.2 Checklists

Checklists are the **primary quality device** supporting review completeness:

| Context | Application |
|---------|-------------|
| **Design Reviews** | General DR checklist + specialized checklists for each type of design document (SRS, DDR, etc.) |
| **Inspections** | Specialized checklists for each document type, coding language, and tool — **periodically updated** |
| **Corrective Maintenance** | Checklists for locating causes of software failure; checklists for preparing mini-testing procedure documents |

> Checklists are the hallmark distinguishing **inspections** from walkthroughs (inspections require formal checklist usage).

### 7.3 Templates

Templates standardize documentation structure. Common examples:
- Software Test Plan (STP) template
- Software Test Description (STD) template
- Software Test Report (STR) template
- Templates for reporting how software failures were solved
- DR report form templates
- Inspection session findings report templates

### 7.4 Defect Frequency Tables

Developed from past inspection findings, these tables direct inspectors to potential **"defect concentration areas"** — sections of documents or code that historically contain the most defects.

---

## 8. Staff Training and Certification (Ch 16)

> *Note: Chapter 16 is not directly present in the source file; this section is compiled from cross-references in Chapters 8–14.*

### 8.1 Training Objectives

| Program | Purpose |
|---------|---------|
| **DR training** | Train senior professionals in technical and review process issues; create a reservoir of qualified DR team members |
| **Inspection moderator training** | Formal training required for inspection moderators — ensures competent session leadership |
| **Inspection team training** | Train professionals in inspection process issues so they can serve as inspection leaders or team members |
| **Maintenance team training** | Ensure continuous service delivery; prepare for peak-load periods; train replacement personnel |

### 8.2 Certification

**Why certification matters:**

- **Corrective maintenance professionals** often work alone, under heavy time pressure, and at customer sites with limited professional support
- Certification ensures minimum competency for independent task performance
- Required in subcontractor contracts — records documenting professional certification must be available for contractor review

### 8.3 Organizational Impact

- Trained professionals serve as a **reservoir** of qualified reviewers available for future projects
- Regular training reduces reliance on "on-the-spot improvised solutions" which can be harmful
- Training and certification programs support both development and maintenance quality

---

## 9. Corrective and Preventive Actions — CAPA (Ch 17)

> *Note: Chapter 17 is not directly present in the source file; this section is compiled from cross-references in Chapters 8–14.*

### 9.1 Concept

**Corrective actions** address the **root causes** of detected defects — they go beyond fixing the specific error to improving the development/maintenance processes that produced it.

**Preventive actions** proactively identify potential problems before they occur, based on analysis of past defect patterns.

### 9.2 The Corrective Action Board (CAB)

An internal committee found in major software development organizations that:
- Screens collected defect and failure information
- Reviews and analyzes findings
- Devises recommendations for improving development and maintenance processes

### 9.3 Data Sources for CAPA

| Source | Data Type |
|--------|-----------|
| **Design reviews** | Recorded analysis/design errors (→ Ch 8.1, indirect objective #2) |
| **Inspections** | Inspection session summary reports sent to CAB |
| **Maintenance records** | Software failure records, user support requests, correction failure rates |
| **Quality metrics** | Trends in maintenance efficiency, effectiveness, customer satisfaction |

### 9.4 Issues Forwarded to CAB

- Changes in content and frequency of customer requests for user support
- Increased average time invested in user support requests
- Increased average time invested in repairing software failures
- Increased percentage of software correction failures

### 9.5 Integration with Reviews

- Inspections explicitly emphasize corrective action as an objective (unlike walkthroughs)
- Inspection findings are incorporated into efforts to **improve development methods per se**, not just fix the current document
- DR and inspection reports feed into the CAB for systemic improvement

---

## 10. Configuration Management (Ch 18)

> *Note: Chapter 18 is not directly present in the source file; this section is compiled from cross-references in Chapters 8–14.*

### 10.1 Purpose

Configuration management ensures:
- Accurate tracking of software versions, installations, and changes
- Reliable support for maintenance activities over the software's lifetime
- Control over the **baseline** and all subsequent modifications

### 10.2 Maintenance Applications

**Failure Repair:**
- Information about the software version installed at each customer site
- Access to the correct copy of code and documentation
- **Quality contribution:** Fewer errors in failure correction trials; reduced correction resources

**Group Replacement (upgrading all customers with the same version):**
- Decision-making about advisability of group replacement
- Planning replacement, allocating resources, determining timetable
- **Quality contribution:** Replacement of current version with improved, less failure-prone version

### 10.3 Broader Role

Configuration management serves not only maintenance but also:
- Development — tracking versions during iterative development
- Subcontractor management — knowing which version each supplier is working with
- Customer support — accurate records of each customer's installed configuration

---

## 11. Documentation and Quality Records Control (Ch 19)

> *Note: Chapter 19 is not directly present in the source file; this section is compiled from cross-references in Chapters 8–14.*

### 11.1 Purpose

Documentation and quality records control ensures:
- Proper creation, review, approval, distribution, and archival of all SQA-related documents
- Traceability of decisions and actions
- Availability of evidence for audits, customer claims, and process improvement

### 11.2 Key Documentation Types in SQA

| Document | Purpose |
|----------|---------|
| **DR Reports** | Record design review discussions, decisions, action items, and follow-up |
| **Inspection Findings Reports** | Document identified errors with location, description, type, and severity |
| **Inspection Summary Reports** | Summarize findings, resources, and metrics for CAB analysis |
| **Walkthrough Findings Reports** | Document errors identified during walkthrough sessions |
| **Maintenance Quality Records** | Software failure records, repair records, user support request logs |
| **Customer Satisfaction Surveys** | Evidence for service quality assessment |

### 11.3 Quality Records for Maintenance

Documentation and quality records prepared during maintenance serve to:
1. **Supply data** for preventive and corrective actions (→ Ch 17)
2. **Support handling** of future customer failure reports and user support requests
3. **Provide evidence** in response to future customer claims and/or complaints

### 11.4 Control Mechanisms

- Documentation requirements are specified in the relevant procedures (→ Ch 14)
- Follow-up documentation must be complete to enable future clarification of corrections
- Records must be accessible for periodic review and audits

---

## 12. Summary: The SQA Infrastructure Framework

Galin's infrastructure components (Ch 14–19) form the **foundation** upon which all other SQA activities depend:

```
┌─────────────────────────────────────────────────────┐
│                 SQA INFRASTRUCTURE                   │
├─────────────────────────────────────────────────────┤
│  Ch 14: Procedures & Work Instructions               │
│  Ch 15: Supporting Quality Devices (Templates,       │
│          Checklists, Defect Frequency Tables)         │
│  Ch 16: Staff Training & Certification               │
│  Ch 17: Corrective & Preventive Actions (CAPA)       │
│  Ch 18: Configuration Management                     │
│  Ch 19: Documentation & Quality Records Control      │
├─────────────────────────────────────────────────────┤
│  Ch 8:  Reviews (DRs, Inspections, Walkthroughs,     │
│          Expert Opinions)                             │
└─────────────────────────────────────────────────────┘
```

These components are applied **throughout the entire software life cycle** — from development through maintenance — and their consistent application is what distinguishes a mature SQA organization from an ad hoc one.

---

## Related Notes

- [[01_Software_Quality_Overview|Software Quality Overview]] — SQA fundamentals and factor models
- [[02_Pre_Project_and_Planning|Pre-Project & Planning]] — Contract reviews, development plans, quality plans
- [[04_Standards_and_Organization|Standards & Organization]] — SQA standards (ISO 9000-3, IEEE) and organizational structures
- [[05_Metrics_and_Costs|Metrics & Costs]] — Quality metrics and cost of software quality
