---
title: "01 – Data Governance"
tags: [data-governance, data-management, dmbok]
source: "DMBoK v2 (Chapter 2/3)"
created: 2026-07-09
---

# 01 – Data Governance

## Overview

**Data Governance (DG)** is the exercise of authority and control — planning, monitoring, and enforcement — over the management of data assets. It guides all other [[00_Introduction_to_DMBOK|data management functions]] to ensure data is managed properly, according to policies and best practices.

> *"The purpose of Data Governance is to ensure that data is managed properly, according to policies and best practices."* — DMBoK v2

While [[00_Introduction_to_DMBOK|data management]] overall drives toward getting value from data, DG focuses on *how decisions are made* about data and *how people and processes are expected to behave* in relation to data.

---

## Goals

1. Enable an organization to manage its data as an **asset**
2. Define, approve, communicate, and implement **principles, policies, procedures, metrics, tools, and responsibilities** for data management
3. Monitor and guide **policy compliance, data usage, and management activities**

---

## Business Drivers

### Reducing Risk
- **General risk management** — oversight of risks data poses to finances or reputation (including legal/regulatory)
- **[[05_Data_Security|Data security]]** — protection of data assets through controls for availability, usability, integrity, consistency, auditability
- **Privacy** — control of private/confidential/PII through policy and compliance monitoring

### Improving Processes
- **Regulatory compliance** — efficient, consistent response to regulatory requirements
- **[[11_Data_Quality|Data quality improvement]]** — making data more reliable to improve business performance
- **[[10_Metadata_Management|Metadata Management]]** — establishing a business glossary; managing the full range of metadata
- **SDLC efficiency** — addressing data management issues across the development lifecycle, including data-specific technical debt
- **Vendor management** — control of contracts dealing with data (cloud storage, data purchase, data as a product, outsourcing)

---

## Core Principles

Successful DG programs are:

| Principle | Description |
|-----------|-------------|
| **Sustainable** | DG is an ongoing process, not a project. Requires organizational commitment and business leadership/sponsorship. |
| **Embedded** | DG activities must be incorporated into SDLC, analytics, MDM, and risk management — not an add-on. |
| **Measured** | DG done well has positive financial impact; requires understanding baseline and planning for measurable improvement. |

### Guiding Principles for DG Foundation
- **Leadership and strategy** — visionary, committed leadership; data strategy driven by business strategy
- **Business-driven** — DG governs IT decisions about data as much as business interaction with data
- **Shared responsibility** — between business data stewards and technical data management professionals across all knowledge areas
- **Multi-layered** — occurs at enterprise, local, and intermediate levels
- **Framework-based** — an operating framework defining accountabilities and interactions
- **Principle-based** — guiding principles are the foundation of DG activity and especially DG policy

---

## Data Governance vs. IT Governance

| Data Governance | IT Governance |
|----------------|---------------|
| Focuses exclusively on management of **data assets** | Makes decisions about IT investments, application portfolio, technical architecture |
| Governs data as an asset | Aligns IT strategies with enterprise goals (e.g., COBIT) |
| Business program with IT participation | Technology program |

---

## Essential Concepts

### The Data-Centric Organization

A data-centric organization values data as an asset through all lifecycle phases. Key characteristics:
- Data is managed as a corporate asset
- Data management best practices are incented across the organization
- Enterprise data strategy is directly aligned with overall business strategy
- Data management processes are continuously improved

### Data Governance Organization Structure

DG can be understood through a political governance lens:
- **Legislative functions** — defining policies, standards, enterprise data architecture
- **Judicial functions** — issue management and escalation
- **Executive functions** — protecting, serving, and administrative responsibilities

Most organizations adopt a *representative* form of governance so all stakeholders are heard.

#### Typical DG Bodies

| Body | Role |
|------|------|
| **Data Governance Steering Committee** | Primary and highest authority; cross-functional senior executives; oversight, support, funding |
| **Data Governance Council (DGC)** | Manages DG initiatives, issues, escalations; consists of executives per the operating model |
| **Data Governance Office (DGO)** | Ongoing focus on enterprise-level data definitions, standards across all knowledge areas; consists of coordinating stewards/custodians, data owners |
| **Data Stewardship Teams** | Communities of interest focused on specific subject areas or projects; business and technical stewards and analysts |
| **Local Data Governance Committee** | Division/department-level councils working under enterprise DGC (in large organizations) |

### DG Operating Model Types

| Model | Description |
|-------|-------------|
| **Centralized** | One DG organization oversees all activities in all subject areas |
| **Replicated** | Same DG operating model and standards adopted by each business unit |
| **Federated** | One DG organization coordinates with multiple business units to maintain consistent definitions and standards |

---

## Data Stewardship

**Data Stewardship** is the most common label for accountability and responsibility for data and processes ensuring effective control and use of data assets.

> A steward manages the property of another person. Data Stewards manage data assets on behalf of others and in the best interests of the organization.

### Stewardship Activities
1. **Creating and managing core Metadata** — definition and management of business terminology, valid data values, and the Business Glossary (system of record for business terms)
2. **Documenting rules and standards** — business rules, data standards, data quality rules; surfacing consensus on expectations
3. **Managing data quality issues** — identification and resolution of data-related issues
4. **Executing operational DG activities** — day-to-day, project-by-project adherence to DG policies and initiatives

### Types of Data Stewards

| Role | Description |
|------|-------------|
| **Chief Data Stewards** | Chair DG bodies in lieu of CDO; may act as CDO in virtual/distributed organizations |
| **Executive Data Stewards** | Senior managers serving on a Data Governance Council |
| **Enterprise Data Stewards** | Oversight of a data domain across business functions |
| **Business Data Stewards** | Business professionals / SMEs accountable for a subset of data; work with stakeholders to define and control data |
| **Data Owner** | A business Data Steward with approval authority for decisions about data in their domain |
| **Technical Data Stewards** | IT professionals in integration, DBA, BI, data quality, or metadata administration |
| **Coordinating Data Stewards** | Lead and represent teams of business and technical stewards across teams and with executives (especially important in large organizations) |

---

## Data Policies

**Data policies** are directives that codify principles and management intent into fundamental rules governing the creation, acquisition, integrity, security, quality, and use of data and information.

- Data policies are **global** — they support data standards and expected behaviors
- Policies describe **"what"** to do (and what not to do)
- Standards and procedures describe **"how"** to do it
- There should be relatively few policies, stated briefly and directly
- Examples:
  - *The DGO will certify data for use by the organization*
  - *Business owners will designate Data Stewards from their business capability areas*
  - *Certified Users will be granted access to Certified Data for ad hoc / non-standard reporting*
  - *All certified data will be evaluated on a regular basis for accuracy, completeness, consistency, accessibility, uniqueness, compliance, and efficiency*

Policies must be effectively communicated, monitored, enforced, and periodically re-evaluated.

---

## Data Asset Valuation

Data asset valuation is the process of understanding and calculating the economic value of data.

### Valuation Approaches
- **Replacement cost** — cost of data lost in a disaster or breach (transactions, domains, catalogs, documents, metrics)
- **Market value** — value as a business asset at merger or acquisition
- **Identified opportunities** — income gained from opportunities in data (BI, transactions, selling data)
- **Selling data** — packaging data as a product or selling insights
- **Risk cost** — valuation based on potential penalties, remediation costs, and litigation expenses

### Generally Accepted Information Principles

| Principle | Description |
|-----------|-------------|
| **Accountability** | Identify individuals ultimately accountable for data and content |
| **Asset** | Data and content are assets and should be managed, secured, and accounted for as material/financial assets |
| **Audit** | Accuracy of data is subject to periodic independent audit |
| **Due Diligence** | Known risks must be reported; possible risks must be confirmed |
| **Going Concern** | Data and content are critical to ongoing business operations (not temporary by-products) |
| **Level of Valuation** | Value data at a level that makes sense or is easiest to measure |
| **Liability** | Financial liability connected to data based on regulatory and ethical misuse |
| **Quality** | Meaning, accuracy, and lifecycle of data affect financial status |
| **Risk** | Risk associated with data must be formally recognized as liability or managed costs |
| **Value** | Value in data based on usage to meet objectives, intrinsic marketability, and/or goodwill contribution |

---

## DG Activities

### 1. Define Data Governance for the Organization
Align DG efforts with business strategy and goals. DG enables shared responsibility for data-related decisions across organizational and system boundaries.

### 2. Perform Readiness Assessment
Assessments describing the current state of information management capabilities:
- **Data management maturity** — what the organization does with data; current capabilities and capacity
- **Capacity to change** — measure the organization's ability to change behaviors; identify resistance points
- **Collaborative readiness** — ability to collaborate in managing and using data (stewardship crosses functional areas)
- **Business alignment** — how well the organization aligns data use with business strategy

### 3. Perform Discovery and Business Alignment
Identify and assess effectiveness of existing policies and guidelines. Data Quality analysis provides insight into existing issues, obstacles, and risks. Derive DG requirements from discovery and alignment activities.

### 4. Develop Organizational Touch Points
The CDO works with:
- **Procurement & Contracts** — standard contract language for data management
- **Budget & Funding** — preventing duplicate efforts, optimizing acquired data assets
- **Regulatory Compliance** — understanding and working within local, national, international regulatory environments
- **SDLC / Development Framework** — control points for enterprise policies, processes, and standards in development lifecycles

### 5. Develop Data Governance Strategy
Deliverables include:
- **Charter** — business drivers, vision, mission, principles, readiness assessment, success criteria
- **Operating framework and accountabilities** — structure and responsibility for DG activities
- **Implementation roadmap** — timeframes for policies, business glossary, architecture, asset valuation, standards
- **Plan for operational success** — target state of sustainable DG activities

### 6. Define the DG Operating Framework
Consider:
- Value of data to the organization (sells data vs. uses as operational lubricant)
- Business model (centralized vs. decentralized, local vs. international)
- Cultural factors (acceptance of discipline, adaptability to change)
- Impact of regulation (highly regulated industries need different mindset)

### 7. Develop Goals, Principles, and Policies
Drafted by data management professionals or business policy staff under DG auspices. Reviewed and refined by Data Stewards and management. Final review/adoption by the Data Governance Council.

### 8. Underwrite Data Management Projects
DGC defines business cases, oversees project status on data management improvements. Coordinates with PMO and large enterprise-wide programs (ERP, CRM, MDM). Every project with a significant data component should capture data management requirements early in the SDLC.

### 9. Engage Change Management
**Organizational Change Management (OCM)** is critical for behavioral changes required to sustain DG. The DG change management team is responsible for:
- Planning (stakeholder analysis, sponsorship, communications)
- Training (DG program training plans)
- Influencing systems development (adding DG steps to SDLC)
- Policy implementation (communicating policies and commitment)
- Communications (awareness of steward roles, DG objectives)
- Implementing new metrics and KPIs (realigned incentives for cross-functional collaboration)

**ADKAR Model focus areas:**
- Awareness of the need to change
- Desire to participate and support the change
- Knowledge about how to change
- Ability to implement new skills and behaviors
- Reinforcement to keep the change in place

### 10. Engage in Issue Management
Process for identifying, quantifying, prioritizing, and resolving DG-related issues:
- **Authority** — decision rights and procedures
- **Change management escalations** — issues from the change process
- **Compliance** — meeting compliance requirements
- **Conflicts** — conflicting policies, procedures, business rules, names, definitions, standards, architecture, data ownership
- **Conformance** — conformance to policies, standards, architecture, and procedures
- **Contracts** — data sharing agreements, buying/selling data, cloud storage
- **Data security and identity** — privacy/confidentiality issues, breach investigations
- **Data quality** — detection and resolution of data quality issues

**Escalation Path:** 80-85% resolved at Data Stewardship Teams → <20% to DGC → <5% to Steering Committee

### 11. Assess Regulatory Compliance Requirements
Every enterprise is affected by governmental and industry regulations. Key regulations:
- **BCBS 239 / Basel II** — banking risk data aggregation and reporting
- **CPG 235** — Australian banking/insurance data risk management
- **PCI-DSS** — payment card industry data security
- **Solvency II** — EU insurance industry regulations
- **GDPR and other privacy laws** — local, sovereign, and international
- **GASB / FASB** — accounting standards (US)

Key questions: relevance, what constitutes compliance, when required, industry standards, demonstration, penalty/risk, how non-compliance is managed.

### 12. Implement Data Governance
Requires an implementation roadmap with timeframes and relationships between activities. Prioritized early activities:
- Defining DG procedures for high-priority goals
- Establishing a business glossary
- Coordinating with Enterprise/Data Architecture
- Assigning financial value to data assets

### 13. Sponsor Data Standards and Procedures
**Standards** — codified decisions that promote consistent results; should be mandatory, communicated, monitored, and periodically reviewed. **Procedures** — documented methods, techniques, and steps to accomplish specific activities.

Standardization applies across all knowledge areas: data architecture, modeling, storage, security, integration, documents/content, reference/master data, warehousing/BI, metadata, data quality, and Big Data/Data Science.

### 14. Develop a Business Glossary
Data Stewards are responsible for business glossary content. Objectives:
- Enable common understanding of core business concepts and terminology
- Reduce risk of data misuse due to inconsistent understanding
- Improve alignment between technology assets and business organization
- Maximize search capability and access to documented institutional knowledge

A business glossary is *not merely a list of terms and definitions* — each term is associated with synonyms, metrics, lineage, business rules, and the steward responsible.

### 15. Coordinate with Architecture Groups
DGC sponsors and approves data architecture artifacts (e.g., enterprise data model). May appoint or interact with an Enterprise Data Architecture Steering Committee or Architecture Review Board (ARB). The enterprise data model is developed jointly by data architects and Data Stewards.

### 16. Sponsor Data Asset Valuation
DGC organizes the effort and sets standards for placing monetary value on data. Starting point: estimate the value of business losses due to inadequate information. Information gaps = business liabilities; cost of closing gaps can estimate the value of missing data.

### 17. Embed Data Governance
Sustainability means processes and funding are in place for continued DG performance. The organization accepts governance of data; results are monitored and measured. Create a **Data Governance Community of Interest** especially in early years.

---

## Tools and Techniques

| Tool | Purpose |
|------|---------|
| **Online Presence / Website** | Core documents, policies, standards, steward roles, news, forums, training, issue procedures, contact info |
| **Business Glossary** | Core DG tool housing agreed-upon business term definitions related to data |
| **Workflow Tools** | Managing processes like new policy implementation, policy administration, issue resolution |
| **Document Management Tools** | Managing policies and procedures |
| **Data Governance Scorecards** | Automated reporting of DG metrics and compliance to DGC and Steering Committees |

---

## Implementation Guidelines

### Organization and Culture
- DG requires a **cultural shift** in organizational thinking and behavior about data
- An ongoing program of change management must support new thinking, behaviors, policies, and processes
- **Sustainability** is the target — measuring how easy it is for the process to continue adding value
- Ignoring culture diminishes chances for success regardless of strategy quality

### Adjustment and Communication
- DG programs are implemented incrementally within wider business and data management strategy
- Key tools: Business/DG strategy map, DG roadmap, ongoing business case, DG metrics

---

## Metrics

### Value
- Contributions to business objectives
- Reduction of risk
- Improved efficiency in operations

### Effectiveness
- Achievement of goals and objectives
- Extent stewards are using relevant tools
- Effectiveness of communication
- Effectiveness of education/training
- Speed of change adoption

### Sustainability
- Performance of policies and processes
- Conformance to standards and procedures (are staff following guidance and changing behavior?)

---

## Relationship to Data Handling Ethics

Data Governance has a particular oversight requirement to review plans and decisions proposed by BI, analytics, and Data Science studies. Oversight for appropriate handling of data falls under both DG and legal counsel — together they must:

- Keep up-to-date on legal changes
- Reduce the risk of ethical impropriety by ensuring employees are aware of their obligations
- Set standards and policies for data handling practices
- Ensure fair handling of employees, protection from reporting possible breaches, and non-interference in personal lives

See [[00_Introduction_to_DMBOK|Data Handling Ethics]] for ethical principles including Respect for Persons, Beneficence, and Justice.

---

## Key References
- Ladley, John. *Data Governance: How to Design, Deploy and Sustain an Effective Data Governance Program*. Morgan Kaufmann, 2012.
- Seiner, Robert S. *Non-Invasive Data Governance*. Technics Publications, 2014.
- Plotkin, David. *Data Stewardship: An Actionable Guide to Effective Data Management and Data Governance*. Morgan Kaufmann, 2013.
- McGilvray, Danette. *Executing Data Quality Projects*. Morgan Kaufmann, 2008.
- Hiatt, Jeff and Creasey, Timothy. *Change Management: The People Side of Change*. Prosci, 2012.
- The Data Governance Institute — [datagovernance.com](http://datagovernance.com)

