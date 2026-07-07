---
tags: [technical-management, processes, systems-engineering, sebok]
---

> *Source: SEBoK v2, Part 3c — Technical Management Processes*

# 06 — Technical Management Processes

## Purpose

**Technical Management Processes** govern the resources and assets allocated to perform systems engineering (SE), typically in the context of a project or service. These processes are distinguished from general project management by their focus on the *technical* or engineering aspects of a project. Success in the technical domain is not possible without the managerial: management provides the planning, organizational structure, collaborative environment, and program controls to ensure stakeholder needs are met.

The Technical Management Knowledge Area covers the following processes:

| Process | Purpose |
|---------|---------|
| **Project Planning** | Develop and integrate technical plans to achieve project objectives within resource constraints and risk thresholds |
| **Project Assessment and Control** | Provide visibility into actual technical progress and risks; trigger corrective and preventive actions |
| **Decision Management** | Provide a structured, analytical framework for evaluating alternatives and selecting the most beneficial course of action |
| **Requirements Management** | Ensure alignment of requirements with other representations, analyses, and system artifacts across the life cycle |
| **Risk Management** | Reduce risks to an acceptable level before they occur, throughout the life of the product or project |
| **Configuration Management** | Manage system and element configurations over the life cycle — tracking changes, managing versions, maintaining consistency |
| **Information Management** | Generate, obtain, confirm, transform, retain, retrieve, disseminate, and dispose of information to designated stakeholders |
| **Quality Management** | Assure the deliverable meets the needs of the customer and is fit for use |
| **Measurement** | Provide objective information about products, services, and processes to support effective management |

These processes are closely coupled and interdependent. Planning uses estimates from measurement; assessment and control relies on measurement for progress tracking; risk management uses planning models and measurement data; decision management evaluates alternatives informed by all other processes.

---

## Project Planning

SE planning is performed concurrently and collaboratively with project planning. It involves developing and integrating technical plans to achieve technical project objectives, involving success-critical stakeholders to ensure tasks are defined with the right timing in the life cycle.

### SE Planning Provides

- Definition of the project from a technical perspective
- Tailoring of engineering processes, practices, methods, and enabling environments
- Definition of technical organizational, personnel, and team functions and responsibilities
- Definition of the appropriate life cycle model
- Definition and timing of technical reviews, assessments, and control mechanisms across the life cycle — including cost, schedule, and technical performance success criteria at identified milestones
- Estimation of technical cost and schedule based on the effort needed to meet requirements
- Determination of critical technologies, associated risks, and actions needed to manage them
- Identification of linkages to other project management efforts
- Documentation of and commitment to the technical planning

### Key Planning Documents

The **Systems Engineering Management Plan (SEMP)** — also called the Systems Engineering Plan (SEP) — is the highest-level technical plan, subordinate to the project plan. It often has subordinate technical plans for specific focus areas. In U.S. defense work, the SEP (written by the government customer) and the SEMP (written by the developer/contractor) are distinct documents with different intents and content.

### Scope and Integration

SE planning addresses the full scope of technical activities: system development and definition, risk management, quality management, configuration management, measurement, information management, production, verification and testing, integration, validation, and deployment. Integration of each plan with higher-level, peer, and subordinate plans is essential. Plans are updated and refined throughout the development process.

### Linkages

- **Measurement** → provides inputs for estimation models
- **Decision Management** → uses estimates and other planning products
- **Assessment and Control** → uses planning results for milestones and progress assessment
- **Risk Management** → uses planning cost models, schedule estimates, and uncertainty distributions

### Pitfalls and Good Practices

| Pitfall | Good Practice |
|---------|---------------|
| Incomplete and rushed planning | Use multiple disciplines in planning |
| Inexperienced staff assigned to planning | Resolve schedule and resource conflicts early |
| | Integrate risk management with SE planning |
| | Use historical data for estimates and adjust for project differences |
| | Use Integrated Product Development Teams (IPDTs/IPTs) |
| | Update plans as additional information becomes available |

---

## Project Assessment and Control

The purpose of **Systems Engineering Assessment and Control (SEAC)** is to provide adequate visibility into the project's actual technical progress and risks with respect to technical plans (SEMP/SEP and subordinate plans). This visibility allows the project team to take timely preventive action when disruptive trends are recognized, or corrective action when performance deviates beyond established thresholds.

### SEAC Process Steps

1. Monitor and review technical performance and resource use against plans
2. Monitor technical risk; escalate significant risks to the project risk register
3. Hold technical reviews and report outcomes at project reviews
4. Analyze issues and determine appropriate actions
5. Manage actions to closure
6. Hold a post-delivery assessment (post-project review) to capture knowledge

### Major Technical Reviews

The SEAC process employs a series of formal reviews throughout the life cycle (from DAU):

| Review | Purpose |
|--------|---------|
| **Alternative Systems Review** | Ensure requirements agree with customer needs |
| **System Requirements Review (SRR)** | Ensure requirements are defined, testable, and consistent with constraints |
| **System Functional Review (SFR)** | Establish the functional baseline |
| **Preliminary Design Review (PDR)** | Establish the allocated baseline |
| **Critical Design Review (CDR)** | Establish the initial product baseline |
| **Test Readiness Review (TRR)** | Ensure readiness for formal testing |
| **Functional Configuration Audit** | Verify as-tested performance complies with requirements |
| **Physical Configuration Audit** | Verify the actual configuration of the as-built item |
| **System Verification Review (SVR)** | Ensure readiness for production |
| **Production Readiness Review (PRR)** | Determine if design is ready for production |
| **Operational Test Readiness Review** | Ensure readiness for operational test and evaluation |

### Pitfalls and Good Practices

| Pitfall | Good Practice |
|---------|---------------|
| No measurement — assessment without measurement is ineffective | Provide independent assessment and recommendations |
| "Something in time" culture — schedule takes priority over quality | Use peer reviews before gate reviews |
| Technical review gates lack "teeth" (can be overridden) | Accept and communicate uncertainties |
| Baselining requirements or designs too early | Just-in-time baselining |
| | Leverage previous root cause analysis |
| | Hold post-delivery assessments for lessons learned |

### Linkages

SEAC is closely coupled with Measurement (indicators for comparing actuals to plans), Planning (estimates and milestones), Decision Management (results as criteria for control decisions), and Risk Management (monitoring and escalating risks).

---

## Decision Management

The purpose of the Decision Management process (per ISO/IEC/IEEE 15288) is:

> *"...to provide a structured, analytical framework for objectively identifying, characterizing and evaluating a set of alternatives for a decision at any point in the life cycle and select the most beneficial course of action."*

The method most commonly employed by systems engineers is the **trade study**. Trade studies aim to define, measure, and assess shareholder and stakeholder value to find the alternative that represents the best balance of competing objectives.

### Decision Management Process Activities

1. **Framing and Tailoring the Decision** — Describe the system baseline, boundaries, interfaces, decision context, life cycle stage, decision makers and stakeholders, and available resources. Define a decision problem statement.

2. **Developing Objectives and Measures** — Develop fundamental objectives and measures using interviews and focus groups with SMEs and stakeholders. Typical competing objectives: performance, development schedule, unit cost, support costs, and growth potential. Test objectives for completeness, non-redundancy, conciseness, specificity, and understandability.

3. **Value Functions** — Transform from measure space to value space using value functions. Define a *walk-away point* (minimum acceptable) mapped to 0 value, and a *stretch goal* (ideal) mapped to 100. Document rationale for shape of value functions.

4. **Swing Weight Matrix** — Determine priority weighting using the swing weight matrix, which considers both importance (defining/critical/enabling function) and the gap between current and desired capability.

5. **Generating Creative Alternatives** — Use an alternative generation table (morphological box) to create a comprehensive set of alternatives spanning the decision space.

6. **Assessing Alternatives via Deterministic Analysis** — Engage SMEs with operational data, test data, simulations, models, and expert knowledge. Capture scores on scoring sheets with source and rationale.

7. **Synthesizing Results** — Transform scores into value tables, use color heat maps, additive value models, value component charts, and stakeholder value scatter plots to visualize multi-dimensional trade-offs.

8. **Identifying Uncertainty and Conducting Probabilistic Analysis** — Use Monte Carlo Simulation and sensitivity analysis (tornado diagrams, waterfall diagrams) to assess uncertainty impact.

9. **Improving Alternatives** — Use value-focused thinking to modify design choices and generate improved alternatives.

10. **Communicating Trade-offs and Presenting Recommendations** — Identify key observations and present recommendations as actionable task lists with comprehensive reports.

### Cognitive Bias in Decision Making

Research by Kahneman and Thaler shows that cognitive biases (confirmation bias, rankism, complacency bias, optimism bias) can seriously distort decisions. Mitigation strategies include: **Independent Technical Authority (ITA)**, **Crew Resource Management (CRM)**, and **premortem exercises** (experts present the negative argument against a decision).

### Best Practices

- Utilize sound mathematical techniques of decision analysis (MODA)
- Develop one master decision model, refined and updated throughout the life cycle
- Use Value-Focused Thinking to create better alternatives
- Identify uncertainty and assess risks for each decision

---

## Requirements Management

Requirements Management (RM) ensures alignment of requirements with other representations, analyses, and system artifacts generated across the life cycle. RM includes managing artifacts from Concept Definition (Business or Mission Analysis, Stakeholder Needs Definition) and traceability to System Definition artifacts.

### RM Activities

RM is a cross-cutting series of activities:
- Managing the sets of needs and design input requirements
- Baselining needs and requirements
- Communicating needs and requirements
- Managing flow-down (allocation and budgeting) of requirements
- Managing internal and external interfaces
- Managing bidirectional traceability
- Managing design, verification, and validation artifacts
- Managing change

### Configuration Management and Change Control

RM is closely tied to CM. When needs and requirements are defined, assessed, and approved, they are **baselined**. The baseline allows the project to define budgets, schedules, and analyze the impact of proposed changes. Reasons for change include: changing stakeholder needs, budget/schedule changes, emerging threats and risks, re-allocation of system performance, and changing operational environments.

Changes must include rationale to support impact assessment. Traceability is a powerful enabler for assessing and understanding the impact of changes.

### Monitoring and Control

The monitoring process uses Measurement to enable situational awareness of status and quality of RM activities. Metrics include: number of needs/requirements, number with TBDs/TBRs, untraced items, verification/validation status. Attributes such as Rationale, Status, Criticality, Priority, Stability, and Owner support monitoring.

### Requirements Management Tools (RMT)

Desired RMT capabilities: definition, collaboration, change control, and traceability to other project data. RMTs should enable sharing of data with other tools as part of a larger digital engineering ecosystem to establish the **Authoritative Source of Truth (ASoT)**.

### Requirements Management Planning

A Requirements Management Plan (RMP) defines scope and processes for managing needs and requirements. For smaller projects, RM processes may be captured within the PMP or SEMP; for larger projects, a standalone RMP is recommended. The RMP addresses: responsible parties, tool selection, stakeholder interaction, needs/requirements definition activities, quality assessment, change control, traceability types, hierarchy management, attributes, metrics, and TBR/TBD management.

---

## Risk Management

The purpose of risk management is to reduce risks to an acceptable level before they occur, throughout the life of the product or project. It is a continuous, forward-looking process applied to anticipate and avert risks that may adversely impact the project. A balance must be achieved between project management and systems engineering ownership of risk management.

### Definitions of Risk

Two prominent definitions are recognized in the INCOSE Handbook:

1. **The effect of uncertainty on objectives** (ISO 15288, ISO 31000, ISO 16085) — effects may be negative or positive. Ties risks to specified objectives.
2. **The combination of the probability of occurrence of harm and the severity of that harm** (ISO Guide 51, ISO 14971) — accommodates only negative effects. Used extensively in safety engineering.

A long-established approach characterizes each individual risk as the combination of: (1) **probability** (likelihood) of failing to achieve a particular outcome, and (2) **consequences** (impact) of failing to achieve that outcome. This supports the use of the risk matrix (risk cube). ISO 31010:2019 describes 41 risk assessment techniques.

### Risk Management Process Activities

1. **Risk Management Planning** — Establish and maintain the strategy; document in a Risk Management Plan (RMP). The RMP should include: project summary, acquisition/contracting strategies, key definitions, process steps, inputs/tools/outputs, linkages to other processes, ground rules, risk categories, and roles/responsibilities.

2. **Risk Identification** — Examine project products, processes, and requirements using top-level approaches (WBS, key processes/requirements evaluation) and lower-level approaches (brainstorming, checklists, taxonomies, expert judgment, Ishikawa diagrams). Use structured risk descriptions (e.g., if-then format). Perform continuously at both individual and structured levels.

3. **Risk Analysis** — Systematically evaluate each identified risk to estimate probability and consequence of occurrence, converting results to a risk level or rating. Methods grouped into qualitative and quantitative. Prioritize risks by risk level, score, time-frame, frequency, and interrelationships.

4. **Risk Treatment** — Identify and select options and implement the desired option to reduce risk to an acceptable level, given program constraints. Options include: **assumption**, **avoidance**, **control (mitigation)**, and **transfer**. All four should be evaluated; hybrid strategies are possible. Buying information (prototypes, benchmarking, modeling) can clarify treatment decisions.

5. **Risk Monitoring** — Evaluate effectiveness of risk treatment activities against established metrics. Approaches include: earned value, program metrics, Technical Performance Measures (TPMs), schedule analysis, and variations in risk level.

### Opportunity Management

Opportunity management is the duality to risk management, with two components: probability of achieving an improved outcome, and impact of achieving that outcome. However, positive opportunity exposure does not match negative risk exposure in utility space. All candidate opportunities should be thoroughly evaluated for potential risks to prevent unintended consequences.

### Integrated Risk Management (IRM)

Per ISO 31000:2018, integrating risk management with all organizational processes improves performance while gaining efficiencies. Risk management is linked to: Measurement (provides indicators for risk analysis), Project Planning (risk identification and stakeholder planning), Project Assessment and Control (monitoring project risks), and Decision Management (evaluating alternatives for risk treatment).

### Pitfalls and Good Practices

| Pitfall | Good Practice |
|---------|---------------|
| Over-reliance on process without attention to human/organizational behavior | Both top-down and bottom-up risk management |
| Lack of continuity — risk management done only for reviews | Early planning |
| Over-reliance on tools and techniques | Understand limitations of analysis tools |
| Automatic mitigation selection (not evaluating all options) | Robust risk treatment strategy (reduce both probability and consequence) |
| "Sea of green" — plan progress tracked, but risk itself unaffected | Structured risk monitoring |
| Band-aid risk treatment — patching symptoms | Update risk database throughout the program |

---

## Configuration Management

Configuration Management (CM) helps teams keep track of changes to a system over its life cycle — ensuring that what is built matches what was planned, and that everyone stays aligned as updates occur. Per ISO/IEC/IEEE 15288: *the purpose of the CM process is to manage system and system element configurations over their life cycle*.

### Fundamental Concepts

- **Configuration**: The interrelated functional and physical characteristics outlined in configuration information
- **Configuration Items (CI)**: The system and its individual elements that fall under CM
- **Configuration Information**: Comprehensive data necessary to describe and document CIs throughout the life cycle — including requirement specifications, architecture descriptions, design descriptions, and user documentation

CM objectives: guarantee consistency between system and configuration information, ensure integrity and traceability of information and its changes, and facilitate reproducibility.

### CM vs. Information Management (IM)

CM focuses on the relevance of information related to the system and its evolution over time. IM concentrates on the management of the information itself (generation, collection, validation, transformation, dissemination, disposal). While all information can be managed, only *configuration information* is subject to CM.

### CM Process Activities

1. **CM Planning and Management** — Define scope and organization of CM tasks. Formalize in a CM plan covering: CM tasks, organization, schedule, responsibilities, and tools. Must accommodate acquisition, subcontracting, and partnership situations.

2. **Configuration Identification** — Identify which system elements and information should be under CM. Define identification rules, structure items and their relationships, define validation/release rules, and define baselines to be established at life cycle milestones.

3. **Configuration Change Management** — Encompasses analysis, justification, evaluation, coordination, and disposition of changes and variances. Managed through **Configuration Control Boards (CCBs)** which typically include: a CCB Chair, Configuration Manager, systems engineer, domain SMEs, and procurement/contracting specialists. A **Technical Review Board (TRB)** may precede the CCB for complex systems to ensure thoroughness and correctness of supporting information.

4. **Configuration Status Accounting** — All reporting tasks providing dedicated reporting about configuration information and CM activities (internal reports, external deliverables, certificates, conformity reports).

5. **Configuration Verification and Audit** — Verify that the system conforms to its configuration information through Functional Configuration Audits (FCA) and Physical Configuration Audits (PCA).

### Configuration Baselines

A configuration baseline is a formally approved snapshot of a system's attributes at a specific point in its development. Three primary baselines:

| Baseline | Description |
|----------|-------------|
| **Functional Baseline (FBL)** | Approved originating set of performance requirements (functional, interoperability, interface characteristics) and verification required |
| **Allocated Baseline (ABL)** | Allocation of requirements to major system elements (lower-level CIs); defines what is expected from each component |
| **Product Baseline (PBL)** | Approved definition and description information resulting from system definition activities (design definition) |

Each baseline has associated documentation: Functional Configuration Documentation (FCD), Allocated Configuration Documentation (ACD), and Product Configuration Documentation (PCD). Baselines serve as immutable reference points for further changes and are typically generated in order, aligned with life cycle stage events.

### CM Implementation Considerations

- **Tailoring**: CM activities must be adjusted to system type, life cycle, complexity, criticality, and organizational context
- **CCB Organization**: Small organizations may use a single CCB; large/complex systems may use TRB + CCB hierarchy
- **CM Training**: All stakeholders trained to minimum awareness; CCB members understand workflow and artifacts
- **CM Measurement**: Effectiveness (strategy deployment, maturity, stakeholder awareness, tool adequacy) and performance (CI volume, change volume/costs, impact analysis duration, change approval/implementation time, discrepancies, audit pass rates)
- **CM Tools**: Product Lifecycle Management, Application Lifecycle Management — federated environment encompassing information management, tracking, version management, release management, baseline management, and breakdown structures

---

## Information Management

Information Management (IM) helps teams access and manage information over its lifecycle, ensuring that information is relevant, complete, verified, protected, distributed, and managed. Per ISO/IEC/IEEE 15288: *the purpose of the IM process is to generate, obtain, confirm, transform, retain, retrieve, disseminate, and dispose of information to designated stakeholders*.

### Fundamental Concepts

- **Data vs. Information**: Data are raw unstructured values; information is structured data in context that has been analyzed and interpreted.
- **Information Asset**: Any piece or collection of information that must be managed independently. Can exist in tangible or intangible forms, in electronic or physical format.
- **Metadata**: "Data about data" — characterizes the information to be managed (title, author, provenance, status). Key role in organizing information governance, search, and navigation.

### IM Enables Business Objectives

| Objective | How IM Enables It |
|-----------|-------------------|
| Operational Efficiency | Organized secure processes for collection, access, and distribution |
| Analytics and Decision-Making | Controlled management of tangible and intangible assets; preservation against uncontrolled modifications, threats, and loss |
| Quality and Security | Protection against uncontrolled modifications, threats, loss; cybersecurity dispositions and disaster recovery |
| Competitiveness and Innovation | Access to crucial information for decisions; capability to extract insights from analysis |
| Compliance | Planning adequate dispositions and access controls for legal, IP, and regulatory requirements |

### IM vs. Knowledge Management (KM)

KM is managed at the organizational level and focuses on providing capability to re-apply existing knowledge. IM aids KM by providing means to manage information regarding knowledge assets (technical content, architectural/design patterns, reusable content, process information).

### IM vs. Configuration Management

IM and CM are closely interconnected. Whenever information assets correspond to identified Configuration Information, those assets should be subject to CM. CM is concerned with the significance and applicability of information to specific system elements; IM deals with the management of the information itself over its own lifecycle.

### Information Security Management

IM incorporates the fundamental need to manage information security — broader than cybersecurity. Five major steps:

1. **Define Sensitive Information** — Determine sensitivity levels relative to pre-defined criteria
2. **Define Data and Asset Classifications** — Global classification dictionary with clear rules
3. **Determine Security Controls** — Encryption, digital signatures, distribution restrictions, NDAs, user education
4. **Implement Security Controls** — Adapted to each classification and organizational context
5. **Monitor Execution** — Measures to provide insights and status about information security

### IM Process (Inspired by INCOSE SE Handbook)

- **Prepare for IM** — Define scope (relevant information, directives/standards, policies/constraints, stakeholders), define approach (information dictionary, lifecycle, transition triggers, change process, authorized formats, storage/archive policy), develop IM plan (identification rules, management requirements, control/security rules, auditing, roles/responsibilities/authorities)
- **Perform IM** — Collect, organize, analyze, distribute, protect, and manage end-of-life of information

### Information Management System (IMS)

A dedicated IMS is needed to enable and support the IM process. Capabilities include: collection and organization, storage and retrieval, analysis/transformation, reporting, protection, access management, and compliance. The IMS itself should be engineered using life cycle processes.

### Pitfalls and Good Practices

| Pitfall | Good Practice |
|---------|---------------|
| No information/data dictionary | Refer to applicable Data Management guides (DAMA DMBOK) |
| No information identification strategy | Recognize information as a strategic asset |
| No metadata | Plan for information storage capacity growth |
| No predefined information lifecycle | Ensure effective information access |
| No security rules | Invest in cross-organization data modeling (focus on semantics) |
| No clear communication rules | Be rigorous about quality of information |
| Missing content/access/repository obsolescence management | Organize lifecycle and security to prevent undesirable modification |
| Inadequate back-up policy | Design information repositories with security and compliance in mind |

---

## Quality Management

Whether a systems engineer delivers a product, a service, or an enterprise, the deliverable should meet the needs of the customer and be fit for use. Such a deliverable is said to be of high quality. The process to assure high quality is called quality management.

### Stages of the Quality Movement

1. **Acceptance Sampling** — Apply statistical tests to decide whether to accept a lot of material based on a random sample
2. **Statistical Process Control (SPC)** — Determine if production processes are stable; measure processes instead of products
3. **Design for Quality** — Design processes robust against causes of variation, reducing monitoring requirements
4. **Six Sigma** — Apply statistical thinking to improve business processes

### Key Definitions (ASQ)

- **Quality**: The characteristics of a product or service that bear on its ability to satisfy stated or implied needs; or a product/service free of deficiencies. Per Juran: "fitness for use"; per Crosby: "conformance to requirements."
- **Acceptance Sampling**: Inspection of a sample to decide whether to accept the entire lot (attributes sampling or variables sampling)
- **SPC**: Application of statistical techniques to control a process
- **Six Sigma**: Method providing tools to improve business process capability; ±6σ from the centerline indicates a well-controlled process

### Quality Attributes

Quality attributes (non-functional requirements) are used to evaluate system performance. It is important to conduct trade-off analysis to identify relationships between attributes — e.g., flexibility and maintainability may correlate positively, while flexibility and reliability may trade off. To achieve high quality, one must: identify and prioritize attributes → identify metrics → measure and monitor → validate measurements → analyze results → establish improvement processes.

#### Quality Attributes by Domain

- **Products**: Conformance to specifications — physical characteristics within tolerance ranges. Summarized as "in compliance" or "defective."
- **Services**: Customer satisfaction is the primary measure. Attributes include: affordability, availability, dependability, efficiency, predictability, reliability, responsiveness, safety, security, usability.
- **Enterprises**: Large, complex interconnected entities with many stakeholders. Quality attributes vary by stakeholder group (e.g., passengers care about safety and affordability; airlines care about adaptability and profitability).

### Measurement System Analysis (MSA)

A set of measuring instruments providing adequate capability for monitoring and controlling quality. Components: Tools (instruments, calibration), Processes (methods, specifications), Procedures (policies, methodologies), People (managers, testers, analysts), and Environment (settings simulating operational conditions).

### Quality Management Strategies

| Strategy | Description |
|----------|-------------|
| **Acceptance Sampling** | Sample from a lot; categorize each member as acceptable/unacceptable (attributes) or measure against metrics (variables). Four outcomes: no errors, consumer risk, producer risk, no errors. Standards: ANSI/ISO/ASQ. |
| **Statistical Process Control (SPC)** | Monitor and control processes using statistical techniques. Control charts (Shewhart 3-sigma charts) distinguish between common cause variation (inherent) and assignable cause variation (events not part of normal process). |
| **Design for Quality** | Design processes to be robust against variation in inputs. Uses response surface experimental design and analysis (pioneered by Taguchi). |
| **Six Sigma** | Five-stage process: **D**efine → **M**easure → **A**nalyze → **I**mprove → **C**ontrol (DMAIC for existing processes; DMADV for new). Lean Six Sigma additionally emphasizes eliminating waste. |

### Quality Standards

Primary standards maintained by ISO (ISO 9000 series) provide requirements for quality management systems without specifying how they are to be met — the key requirement is that systems must be audited. In the U.S., the Malcolm Baldrige National Quality Award Criteria have become de facto standards for assessing organizational quality performance.

---

## Measurement

Measurement and the accompanying analysis are fundamental elements of systems engineering and technical management. SE measurement provides information relating to products, services, and processes to support effective management and objectively evaluate quality. Measurement supports realistic planning, provides insight into actual performance, and facilitates assessment of suitable actions.

### Fundamental Concepts

Three key SE measurement concepts:

1. SE measurement is a consistent but flexible process **tailored to unique information needs** and characteristics of a particular project or organization, revised as needs change
2. Decision makers must **understand what is being measured** — connecting measures to what they need to know as part of a closed-loop, feedback control process
3. Measurement **must be used to be effective** — measuring quality alongside cost and schedule provides the overall effectiveness picture

### Measurement Process (PSM / ISO/IEC/IEEE 15939)

Four activities:

1. **Establish and Sustain Commitment** — Establish resources, training, and tools; ensure management commitment to use the information produced.

2. **Plan Measurement** — Define measures providing insight into information needs. Approaches include: PSM (information categories, measurable concepts, prospective measures), Goal-Question-Metric (GQM), and ISO/IEC/IEEE 15939. Good measures have attributes of relevance, completeness, timeliness, simplicity, cost effectiveness, repeatability, and accuracy. Each measure must be unambiguously defined and documented.

3. **Perform Measurement** — Collect and prepare measurement data (verification, normalization, aggregation), perform analysis (estimation, feasibility analysis, performance analysis), and present results to inform decision makers. Data quality depends on valid, accurate, and unbiased collection.

4. **Evaluate Measurement** — Periodically evaluate and improve the measurement process and specific measures. Ensure measures continue to align with business goals and information needs.

### Systems Engineering Leading Indicators

Leading indicators provide **predictive insight** pertaining to an information need — evaluating the effectiveness of how a specific activity is applied in a manner that provides information about impacts likely to affect system performance objectives. A leading indicator is composed of characteristics, a condition, and a predicted behavior, analyzed periodically to predict future states.

### Technical Measurement

Technical measurement provides information about progress in definition and development of the technical solution, ongoing assessment of associated risks, and the likelihood of meeting critical objectives. Three types of technical measures:

- **Measures of Effectiveness (MOEs)** — Operational measures of success related to mission accomplishment
- **Measures of Performance (MOPs)** — Measures characterizing physical or functional attributes of the system
- **Technical Performance Measures (TPMs)** — Measures predicting the degree to which the system will meet performance requirements

These are planned early in the life cycle and performed throughout with increasing fidelity as the technical solution develops.

### Service Measurement

For services, the terms **Critical Success Factors (CSF)** and **Key Performance Indicators (KPI)** are commonly used. CSFs are the key elements most important to achieve business objectives; KPIs are specific values measured to assess achievement of those objectives. Good service measures are outcome-based, customer-focused (availability, reliability, performance), and forward-looking.

### Digital Engineering (DE) Measurement

The PSM Digital Engineering Measurement Framework provides constructs to isolate measurements most closely linked to the primary benefits of DE. DE measures focus on the digital information products (data and models) developed and used across a product life cycle, covering: digital infrastructure, life cycle models, process models, data and model ontology, and operational/system/discipline-specific data and models.

### Continuous Iterative Development (CID) Measurement

The CID Measurement Framework details common information needs and measures effective for evaluating CID/agile approaches, addressing team, product, and enterprise perspectives.

### Linkages

- **Project Planning** — Historical data and estimation support
- **Project Assessment and Control** — Objective information for assessment and control actions
- **Risk Management** — Quantifies risks and provides data for treatment evaluation
- **Decision Management** — Objective insight for informed decision making

### Pitfalls and Good Practices

| Pitfall | Good Practice |
|---------|---------------|
| Golden measures — looking for one-size-fits-all | Regularly review each measure collected |
| Single-pass perspective — viewing measurement as one-time | Action-driven — measurement results provided to decision makers |
| Unknown information need — measuring without understanding why | Integration into project processes and business rhythm |
| Inappropriate usage — measuring individuals or without context | Timely information — early enough to take action |
| | Data availability — use best available data, complemented by qualitative insight |
| | Use historical data as the basis of plans |

---

## Related Chapters

- [[00_Introduction_to_SEBoK]] — Overview and structure of the SEBoK
- [[02_Nature_of_Systems]] — Foundational systems concepts
- [[03_Systems_Engineering_and_Management]] — SE life cycle and process knowledge
- [[04_Applications_of_Systems_Engineering]] — Domain-specific SE applications
- [[05_Enabling_Systems_Engineering]] — Organizational support for SE
- [[06_Related_Disciplines]] — SE interaction with other disciplines (Project Management, Software Engineering, Quality Attributes, etc.)
- [[07_Implementation_Examples]] — Case studies and vignettes
