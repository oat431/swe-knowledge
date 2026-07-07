---
tags: [realization, maintenance, systems-engineering, sebok]
---

> *Source: SEBoK v2, Part 3c — System Realization and System Maintenance*

## Purpose

System Realization and System Maintenance together span the operational spine of a system's lifecycle. **Realization** encompasses the activities that create and test versions of a system — implementation, integration, verification, transition, validation, and operation — as specified by system definition. **Maintenance** sustains operational capability for the full life of the system through logistics, service life management, life extension, capability updates/upgrades/modernization, and eventual disposal and retirement.

Realization processes are not sequential but are performed **concurrently, iteratively, and recursively** depending on the selected life cycle model. Every life cycle model includes realization activities, principally verification and validation. Maintenance planning begins early in the acquisition process with development of a maintenance concept and continues concurrently with operations.

---

## System Realization

### System Implementation

**Definition & Purpose:** Implementation is the process that yields the lowest-level system elements in the system breakdown structure. System elements are **made, bought, or reused**. It bridges the system definition processes and the integration process.

**Key Activities:**
- **Define the implementation strategy** — fabrication and coding procedures, tools and equipment, tolerances, auditing criteria. In mass manufacturing, the strategy is retained for repeatable, consistent production. Includes arrangements for packing, storing, and supplying the implemented element.
- **Realize the system element** — fabricate hardware, develop software, define training capabilities, draft training documentation, and train initial operators and maintainers.
- **Provide evidence of compliance** — conduct peer reviews, unit testing, inspection of manuals; acquire measured properties (weight, capacity, performance, reliability, availability).
- **Package, store, and supply** the implemented element per the implementation strategy.

**Key Considerations:**
- Implementation outcomes may generate constraints (derived requirements) applied to the architecture of the higher-level system.
- Purchasing or reusing an existing element introduces constraints that the architecture must accommodate.
- Supporting documentation (operation, maintenance, installation manuals) is part of implementation and utilized during deployment and use.
- Execution is governed by industrial/government standards and enterprise safety practices.

### System Integration

**Definition & Purpose:** System integration consists of progressively assembling implemented elements to form complete or partial system configurations. The purpose is to ensure individual elements function properly as a whole and satisfy design properties. Integration is **developmental**, not to be confused with production-line assembly.

**Key Principles:**
- Integration follows the **bottom-up branch of the Vee Model**, combining assembly with verification tasks (excluding final validation).
- Integration is based on the notion of an **aggregate** — a subset of the system on which verification and validation (V&V) actions are applied.
- Integration proceeds **level by level** according to the physical architecture defined during system definition.
- Integration can reveal anomalies requiring design modifications — but modification itself belongs to the design process, not integration.

**Integration Techniques:**

| Technique | Description | Strengths | Weaknesses |
|-----------|-------------|-----------|------------|
| Global (Big-Bang) | All elements assembled in one step | Simple, no simulators needed | Faults detected late, hard to localize |
| With the Stream | Assemble elements as they become available | Quick start | Complex due to simulators, end-to-end tests late |
| Incremental | Add one or few elements to an existing integrated increment | Fast fault localization | Many test cases, simulators needed |
| Subsets (Functional Chains) | Assemble subsets, then combine subsets | Time saving via parallel work | Subsets must be defined during design |
| Top-Down | Integrate in activation/utilization order | Early architectural fault detection, realistic tests | Many stubs/caps needed |
| Bottom-Up | Integrate in reverse activation order | Easy test definition, early fault detection | Over-testing, architectural faults detected late |
| Criterion-Driven | Most critical elements integrated first | Early testing of critical elements | Test cases hard to define |

**Key Activities:**
- Establish the integration plan (concurrent with design): optimized strategy, V&V actions, aggregate configurations, and integration/verification means.
- Obtain integration and verification means (procurement, development, reuse, sub-contracting).
- Take delivery of each implemented element: unpack, check configuration, conformance, and interfaces.
- Assemble elements into aggregates per the integration plan using assembly tools and procedures.
- Verify each aggregate: check assembly correctness, perform V&V procedures, and record results.

**Practical Considerations:**
- Avoid big-bang integration for complex systems — fast fault detection is critical.
- Coupling matrices and N-squared diagrams are basic tools for optimizing aggregate definitions and interface verification.
- Integration strategy must be flexible: elements rarely arrive in expected order and tests never proceed exactly as foreseen.

### System Verification

**Definition & Purpose:** Verification is the confirmation, through objective evidence, that **specified requirements have been fulfilled**. It identifies faults/defects introduced during any transformation of inputs into outputs. The key distinction: **verification = "built the system right"** (comparing against design references).

**Key Concepts:**
- A verification action identifies the element to verify and the reference defining the expected result, then obtains a result, compares, and deduces correctness.
- Verification relates to one element at a time (local, detail-oriented) and is generally binary (pass/fail).
- Verification is a **transverse activity** — applied to every life cycle stage and every engineering element (requirements, design, system, documents, procedures).

**Verification vs. Validation:**

| Aspect | Verification | Validation |
|--------|-------------|------------|
| Purpose | Detect faults/defects (supplier-oriented) | Acquire confidence (end-user oriented) |
| Basis | Truth (objective/unbiased) | Value judgment (more subjective) |
| Level of Concern | Detail and local | Global in context of use |
| Vision | Glass box (how it runs inside) | Black box (produces expected effect) |
| Reference | System design | System requirements, stakeholder requirements |
| Order | First | Second |

**Verification Techniques:**
- **Inspection** — visual or dimensional examination using human senses or simple measurement.
- **Analysis** — mathematical/probabilistic calculation, modeling, simulation under defined conditions.
- **Analogy/Similarity** — evidence from similar elements with equivalent or more stringent verification history.
- **Demonstration** — showing correct operation against observable characteristics without physical measurements (field testing).
- **Test** — quantitative verification under controlled real or simulated conditions using special equipment.
- **Sampling** — verification of characteristics using samples with specified tolerances.

**Key Activities:**
- Identify verification scope (list characteristics to check).
- Identify constraints (technical feasibility, cost, time, personnel, contractual).
- Define verification techniques and perform trade-off of scope against constraints.
- Optimize strategy and schedule execution in project milestones.
- Detail each verification action (expected results, techniques, means).
- Carry out procedures, capture results, analyze and compare to expected results.

**Pitfalls & Good Practices:**
- Start verification **early** in development — corrections are cheaper and easier.
- Define ending criteria (coverage rates, error counts) — don't stop when funding runs out.
- Don't use only testing; analysis and inspections early in design are cost-effective.
- Involve designers with verification teams.

### System Transition

**Definition & Purpose:** Transition bridges the gap between qualification and use — the handoff from development to logistics, operations, maintenance, and support. Per ISO/IEC/IEEE 15288, transition "installs a verified system, together with relevant enabling systems (operating system, support system, operator training system, user training system)."

**Key Activities:**
- **Develop a Deployment/Transition Strategy** — planning ideally begins early in the SE life cycle. Includes considerations for installation, checkout, integration, and testing.
- **Develop Transition Plans** — consistent with strategy and agreed to by stakeholders. Includes support strategy (organic support, contractor logistics, etc.) and levels of support.
- **Installation** — physically instate the system; connect interfaces (electrical, computer, security, software); document complexity, environmental conditions, interface specifications, and human factors.
- **Integration** — additional integration into the operational setting, especially for incremental deliveries.
- **Verification & Validation (V&V)** — physical, electrical, and mechanical checks:
  - **On-Site Acceptance Test (OSAT)** — functional tests in the operational environment.
  - **Field Acceptance Test** — performance tests in the actual operating environment (flight/sea acceptance).
  - **Operational Test & Evaluation (OT&E)** — estimates operational effectiveness.
- **Evaluate readiness** — based on transition criteria; use integration, V&V results to gauge readiness.
- **Analyze results** — identify additional actions and improvements for future transitions.

**Additional Considerations:**
- **System Run-In** — verified for a stated period (1–2 years) for adequacy of reliability and maintainability (R&M) and integrated logistics support (ILS) deliverables.
- **Phasing-In/Phasing-Out** — minimize disruption; operate new and legacy systems in parallel; establish feedback from users; disposal planning.
- **Reliability Demonstration Testing (RDT)** — field tests under actual conditions; data analysis for system reliability determination.

### System Validation

**Definition & Purpose:** Validation is the confirmation, through objective evidence, that the requirements for a **specific intended use or application have been fulfilled**. The key distinction: **validation = "built the right system"** (comparing against stakeholder needs and system requirements). It is a transverse activity performed throughout the lifecycle, not just at the end.

**Key Concepts:**
- A validation action identifies the element and the reference, obtains a result, compares, deduces compliance, and decides on **acceptability** (may require value judgment).
- Validation presupposes that verification actions have already been performed.
- Validation accumulates gradually: through V&V actions on every engineering element, through final validation in an industrial environment, and through operational validation in the context of use.

**Validation Levels:**
- Validation is performed **level by level** — every system and element is verified, validated, and possibly corrected before integration into the parent system.
- Within each level, verification and validation actions are performed during both system definition and system realization.
- The **V&V strategy** optimizes verification, validation, and integration strategies together — minimizing actions while maximizing fault detection and confidence.

**Key Activities:**
- Establish validation strategy (concurrent with system definition): identify scope (all requirements), identify constraints, define techniques, trade-off scope/constraints, optimize strategy, schedule execution.
- Perform validation actions: detail each action, acquire means, carry out procedures, capture and record results.
- Analyze results, compare to expected results, decide compliance, generate reports and change requests as needed.
- Control the process: update plan, coordinate with project manager, designers, and configuration manager.

**Key Artifacts:**
- Validation plan (strategy), validation matrix, validation procedures, validation reports, validation tools, validated elements, issue/trouble reports, change requests.

**Pitfalls & Good Practices:**
- Don't wait until the end to start validation — start as early as possible.
- Requirements should be **verifiable**: quantitative, measurable, unambiguous, testable.
- Document validation actions and results for accountability.
- Involve end users directly in validation under their own conditions.
- Use a validation traceability matrix to track coverage.

### System Operation

**Definition & Purpose:** The operation process assigns personnel to operate the system and monitors services and operator-system performance. It identifies and analyzes operational problems against agreements, stakeholder requirements, and organizational constraints. The systems engineer ensures the system maintains key mission/business functions and is operationally effective.

**Key SE Roles During Operation:**
- Active participation in reviews, change management, and integrated master schedule activities.
- Ensuring operations continue to meet evolving stakeholder needs consistent with the approved architecture through eventual decommissioning or disposal.
- Managing migration between legacy and replacement systems to prevent service breakdown.

**Key SE Activities:**
- **Training** — update and develop training for operational and support personnel based on performance evaluation.
- **Evaluation of Operational Effectiveness** — monitor measures established during planning against mission and business goals; recommend actions.
- **Failure Reporting and Corrective Actions (FRACA)** — collect and analyze operational data to identify trends requiring design or component changes; manage safety implications.

**Special Considerations:**
- **Service Life Extension Program (SLEP)** — when replacement cost is prohibitive; addresses obsolescence, diminishing manufacturing sources and material shortages (DMSMS), and changes in ConOps.
- **Data Collection** — design for data collection during operations (autonomous sensors or operator reports); systems engineer must understand how all data is collected and presented for analysis.

**Outputs of the Operation Process:**
- Operational strategy (staffing, sustainment of enabling systems and materials)
- System performance reports (statistics, usage data, operational cost data)
- System trouble/anomaly reports with recommendations
- Operational availability constraints to influence future designs

---

## System Maintenance

Maintenance planning begins early in acquisition with development of a maintenance concept. The maintenance process must be executed concurrently with the operations process. Key objectives include maximizing system availability, preserving operating potential through reliability-centered maintenance, segmenting activities for potential outsourcing, and harnessing IT for maintenance management.

**Maintenance Process Activities:**
- Scheduled servicing (daily inspections, cleaning)
- Unscheduled servicing (fault detection, isolation, replacement)
- Re-configuration for different roles/functions
- Minor modifications and damage repairs
- Major scheduled servicing (overhaul, corrosion treatment)
- Major repairs (beyond normal removal and replacement)

**Key Considerations:**
- Support resources (test equipment, technical data, personnel, spares, facilities) must be factored in during acquisition.
- Training and certification — initial and continuation training for technical personnel; certification standards in supply agreements.
- Clear thresholds for distinguishing maintenance changes from formal system engineering lifecycle projects (based on cost, schedule, risk, criticality).

### Logistics

**Definition:** Logistics is the science of planning and implementing the acquisition and use of resources necessary to sustain the operation of a system. Sustainment is determined by both the inherent supportability of the system (a function of design) and the processes used to sustain system functions and capabilities.

**Sustainment Planning — Influencing Inherent Supportability:**

- **Architecture Considerations** — openness, modularity, scalability, and upgradeability are critical for incremental acquisition. These attributes pay dividends when obsolescence and end-of-life issues are resolved through technology refreshment.
- **Reliability Considerations** — critical to system effectiveness and logistics burden. The primary objective is achieving necessary probability of operational success and minimizing failure risk within availability, cost, schedule, weight, power, and volume constraints.
- **Maintainability Considerations** — reduce maintenance burden and supply chain through:
  - **Modularity** — packaging components for remove-and-replace vs. on-board repair.
  - **Interoperability** — standard interface protocols (black box technology).
  - **Physical accessibility** — direct visibility and access to frequently maintained components.
  - **Minimum preventative maintenance** — including corrosion prevention and mitigation.
  - **Embedded training and testing** — when optimal from total ownership cost perspective.
  - **Human Systems Integration (HSI)** — minimal manpower, effective training, safe and habitable design.
- **Support Considerations** — diagnostics, prognostics, calibration, HSI, documentation, maintenance data collection, compatibility, interoperability, transportability, packing, and facility requirements.

**Sustainment Planning — Planning Sustainment Processes:**
Process efficiency reflects how well the system can be produced, operated, serviced, and maintained. Achieving process efficiency requires continuous emphasis — processes present improvement opportunities even after the design-in window has passed (lean-six sigma, supply chain optimization, continuous process improvement).

**Sustainment Analysis — Product Support Package (12 Elements):**
1. Product support management (integrated life cycle sustainment planning)
2. Design Interface (reliability, maintainability, supportability, affordability, configuration management, safety, environmental/HAZMAT, HSI, calibration, anti-tamper, habitability, disposal, legal)
3. Sustainment Engineering (FRACAS, value engineering, DMSMS)
4. Supply Support (materiel planning)
5. Maintenance Planning (RCM, maintenance concepts, levels of maintenance/LORA, condition-based maintenance, prognostics)
6. Support Equipment
7. Technical Data
8. Manpower & Personnel
9. Training & Training Support
10. Facilities & Infrastructure
11. Packaging, Handling, Storage, & Transportation
12. Computer Resources

**Sustainment Implementation:**
Once the system becomes operational, SE supports execution of the 12 integrated product support elements. Changes to fielded systems must include adjustments to IPS elements to maintain effectiveness. Minor problems may require simple adjustments (procedure change, supplier change, training, technical manual). Major problems (system redesign) require engineering change proposals, IPS element trade studies, business case analysis, and product support strategy updates.

### Service Life Management

**Definition:** Product and service life management (also called system sustainment) deals with the overall life cycle planning and support of a system. The life of a product or service spans considerably longer than the time required to design and develop it.

**Key Areas:**
1. **Service Life Extension** — principles, challenges during modifications, and issues with disposal/retirement.
2. **Modernization and Upgrades** — engineering change management with understanding of design life constraints; requires complete inventory of system interfaces and technical drawings.
3. **Disposal and Retirement** — environmental concerns, hazardous waste handling, and concurrent operation of replacement systems.

**Sustainment Activities Include:**
- Design for maintainability
- Built-in test, diagnostics, prognostics, and condition-based maintenance
- Logistics footprint reduction strategies
- Technology insertion opportunities
- Operations and support cost reduction
- Monitoring of key support metrics

**Good Practices:**
- The SE process does not stop when the product or service becomes operational.
- Life management functions in the post-delivery phase are part of the SE process.
- Modifications must comply with system requirements.
- Users must be able to continue maintenance activities after upgrades.
- Account for changing user requirements over the system life cycle.
- Adapt support concepts throughout the life cycle.
- Apply engineering change management to the total system.

### Service Life Extension

**Definition:** Service life extension (SLE) involves continued use of a product and/or service beyond its original design life. It involves assessing risks and life cycle cost (LCC) of continuing use versus replacement cost.

**When SLE is Required:**
- System no longer meets performance or reliability requirements.
- Cost of operation/maintenance exceeds the cost of SLE or available budgets.
- Parts are no longer available for repair and maintenance.
- Operation violates rules/regulations (environmental, safety).
- Parts are about to reach operational life limits.

**Key Factors for the Systems Engineer:**
- Current life cycle costs
- Design life and expected remaining useful life
- Software maintenance
- Configuration management
- Warranty policy
- Availability of parts, subsystems, and manufacturing sources
- Availability of system documentation

**Critical Considerations:**
- System design life parameters (safety limits, material life) are established early in design and are critical to defining extension possibilities.
- Systems that age through use (aircraft, bridges, nuclear plants) require periodic inspection for aging and fatigue.
- Software maintenance is critical — legacy systems may include computer resources operating for many years with essential functions that must not be disrupted.
- Diminishing manufacturing sources and material shortages (DMSMS) must be addressed early.
- Life cycle cost analysis assumptions should be revisited and challenged early in the SLE process.

**Applications:**
- **Product Systems** — LCC analysis comparing continued use vs. replacement. Military SLEP: well-developed guidance for continuous technology insertion. Aircraft examples: B-52 (since 1955) and Boeing 737 (since 1967).
- **Service Systems** — continued delivery without disruption; capital investment and phased deployment (water handling, transportation, energy, healthcare).
- **Enterprise Systems** — holistic view across the entire enterprise; balanced approach to operating older components vs. implementing improvements (NASA shuttle, oil/gas reservoirs).

### Capability Updates, Upgrades, and Modernization

**Definition:** Modernization and upgrades involve changing the product or service to include new functions and interfaces, improve system performance, and/or improve system supportability. System modernization resolves supportability problems and reduces operational costs.

**Reasons for Modernization:**
1. Reduced performance, safety, or reliability in the system or subsystems.
2. Stakeholder desires new capability.
3. Component obsolescence (including lack of spare parts).
4. New uses requiring modification to add capabilities not originally built.

**Key Factors for the Systems Engineer:**
- Type of system (space, air, ground, maritime, safety critical)
- Missions and scenarios of expected operational usage
- Policy and legal requirements
- Product/service life cycle costs
- Electromagnetic spectrum usage and RF emissions changes
- OEM and key supplier availability
- Understanding and documenting functions, interfaces, and performance requirements
- System-of-systems interoperability challenges between legacy, modified, and new systems
- Amount of regression testing required on existing software

**Key Processes:**
- Legislative policy adherence review and certification
- Safety critical review
- Engineering change management and configuration control
- Analysis of alternatives
- Warranty and product return process
- Manufacturing and supplier source availability

**Form, Fit, Function, and Interface (F3I) Principle:**
Backward compatibility is a critical principle for upgrades. Changes at the lowest architectural levels should be F3I compatible so higher-level requirements are unaffected.

**The Vee Model for Modifications:**
Modifications can enter the Vee model at three different levels: system-level (A), subsystem-level (B), or component-level (C). Changes introduced at lower levels must flow requirements upward through "parent" requirements to system-level requirements. If system documentation is unavailable, **reverse engineering** is required to capture the "as-is configuration."

**Key Practices:**
- Regression testing on unmodified portions to confirm upgrades did not impact existing functions.
- Use of requirements verification traceability matrix (RVTM) during regression testing.
- Consider changes to the system support environment (test equipment, packaging, transportation).
- **Block method** for managing concurrent modifications — group and deploy together.
- **Continuous integration** method for systems where multiple changes occur concurrently.

**COTS Considerations:**
Commercial-off-the-shelf components provide advantages (technological advancement, increased functionality, lower cost) and risks (component obsolescence within ~2 years, interface changes).

### System Disposal and Retirement

**Definition:** Disposal and retirement is the process of removing a system element from the operational environment with the intent of permanently terminating its use, and dealing with any hazardous or toxic materials or waste products in accordance with applicable guidance, policy, regulation, and statutes.

**Key Ecological Considerations:**
- Air Pollution and Control
- Water Pollution and Control
- Noise Pollution and Control
- Radiation
- Solid Waste

**Key Regulatory Context:**
- **US:** EPA and OSHA govern disposal and retirement of commercial systems. Hazardous materials under OSHA 1910.119A. EPA waste management regulations under 40 C.F.R. parts 239-282 (hazardous waste starts at part 260). RCRA establishes "cradle-to-grave" tracking.
- **EU:** REACH regulation requires registration and disclosure of substances in products. RoHS restricts hazardous substances in electrical/electronic equipment.
- **Military:** DoD Directive 4160.21-M governs disposition of excess, surplus, and foreign excess personal property. Defense Logistics Agency (DLA) leads worldwide reuse, recycling, and disposal. Demilitarization is a critical responsibility.

**Application to Product Systems:**
- Retirement includes disposal or preservation (mothballing).
- Disposal of components occurs during operational life as components fail and are replaced — planning must occur well before demand exists.
- Key considerations: transportation of failed items, handling equipment, special training, facilities, technical procedures, HAZMAT remediation, reclamation/salvage value.
- "Green engineering" — design for recycling, reuse, and responsible disposal.
- Disposability often has lower priority because: no direct revenue associated, costs are initially hidden, and disposal is typically performed by someone outside SE.

**Application to Service Systems:**
- Proper continuation of services during retirement is critical — replacement systems operate in parallel with existing systems over a significant period.
- Lack of proper documentation, configuration management, and adequate O&M budget can shorten useful life and lead to early retirement.

**Application to Enterprise Systems:**
- Phased approach with capital planning in stages.
- Parallel operation of replacement and existing systems to prevent loss of functionality.

**Practical Considerations:**
- Design products so components can be recycled without detrimental environmental effects.
- Green engineering: minimize generation of pollutants at the source and minimize risks to human health and the environment.
- Upfront planning and development of a disposal plan to manage all retirement activities.
- Nuclear decommissioning exemplifies the need for proper hazardous material control, handling, and transportation.

---

## Related Chapters
- [[01_Systems_Fundamentals]] — definitions, concepts, principles, and heuristics
- [[02_Nature_of_Systems]] — foundational perspectives across natural, social, and engineered systems
- [[05_Representing_Systems_with_Models]] — abstraction and modeling
- [[06_Systems_Approach_Applied]] — structured methods for SE practice
- [[07_Systems_Engineering_Life_Cycle_Models]] — life cycle models and process relationships
- [[09_Systems_Engineering_Management]] — SEM planning, assessment, decision management, risk, CM, and quality
- [[SEBoK_Part_4]] — Applications of Systems Engineering
