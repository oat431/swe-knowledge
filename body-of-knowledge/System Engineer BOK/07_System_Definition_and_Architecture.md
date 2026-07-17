---
tags: [architecture, design, requirements, systems-engineering, sebok]
---

> *Source: SEBoK v2, Part 3c — System Concept Definition & Architecture Design*

## Purpose

This chapter covers what a system must do and how it will do it — the two intertwined knowledge areas of **System Concept Definition** and **System Architecture Design Definition**. Concept Definition examines the problem space (why the system is needed, what stakeholders need), while Architecture Design Definition translates those needs into functional, logical, and physical solution descriptions. Together they form the bridge from stakeholder intent to implementable design.

---

## System Concept Definition

Concept Definition is the set of SE activities in which the problem space and stakeholder needs are closely examined — **before** any formal definition of the system-of-interest (SoI) is developed. It answers *why* the solution is needed and *what* it must accomplish, deferring *how* to architecture and design. The two primary activities occur concurrently with System Architecture Design Definition processes.

### Business or Mission Analysis

> **Purpose:** Understand a mission or market problem, threat, or opportunity; establish the goals, objectives, and measures of success for a potential solution class (ISO/IEC/IEEE 15288).

This is the **first process** in Concept Definition. It defines the strategic rationale for the project before a project team is even formed.

**Key Outputs:**
- Identification of major stakeholders (captured in a stakeholder register)
- Definition of the problem, threat, or opportunity statement
- Elaboration of the **Mission, Goals, Objectives (MGOs)** and measures of success
- Preliminary life cycle concepts (acquisition, development, operations, support, retirement)
- Candidate solution classes (new system, modified existing system, operational change, service)
- Confirmation of organizational support (go/no-go decision)

**Mission–Goal–Objective Hierarchy:**
- **Mission:** *Why* does the project exist? What value does the SoI bring? (e.g., "Increase revenue by providing a home-based, one-stop, multi-function hot beverage maker.")
- **Goals:** What must be achieved for mission success? (e.g., "Increase revenue. Increase market share.")
- **Objectives:** What specifically must the team/SoI do to achieve the goals? (e.g., "Increase market share of Company X sales regions.")
- **Measures:** Quantitative metrics — Measures of Effectiveness (MOEs) used to validate the SoI against objectives (e.g., "Improve market share by 20%.")

**Process Approach:**
1. **Identify major stakeholders** — enterprise-level decision makers and key affected parties
2. **Define the problem, threat, or opportunity** — four steps: identify impacted stakeholders, collaborate to understand impact, clearly define the statement, obtain stakeholder concurrence
3. **Define MGOs and measures** — characterise "as-is" vs. "to-be" states; distinguish business-perspective MGOs from user-perspective MGOs
4. **Capture preliminary life cycle concepts** — operational scenarios, use cases, misuse cases, loss scenarios; enables feasibility analysis
5. **Identify uncertainties and risks** — feed into Risk Management
6. **Identify preliminary solution classes** — assess against MGOs and measures using Decision Management
7. **Assess continuation** — organization evaluates alignment with enterprise strategy; if "go," outputs pass to the project team

**Push vs. Pull Paradigms:**
- **Pull:** Solution responds to an identified problem or gap (e.g., missing defense capability)
- **Push:** Solution creates or anticipates an opportunity (e.g., emerging market product)

**Green-field vs. Brown-field:**
- **Green-field:** New system from a "blank piece of paper"
- **Brown-field:** Evolution or transformation of an existing system, leveraging established data

### Stakeholder Needs Definition

> **Purpose:** Explore what capabilities are needed by stakeholders for the SoI to accomplish its mission. The outcome is an **integrated set of needs** — the basis for System Validation and input to System Requirements Definition.

This is the **second process** in Concept Definition. It goes far beyond simple elicitation: it is a series of analysis activities ensuring all parameters — risks, drivers, constraints, life cycle concepts — are captured and matured.

**Integrated Set of Needs — Inputs From:**
- Stakeholder needs, requirements, and expectations (elicited directly)
- Higher-level requirements (from business/mission analysis)
- Life cycle concepts (matured from preliminary versions)
- Drivers and constraints (design standards, regulations, operating environments, technology maturity)
- Risk analysis (threats, hazards, misuse scenarios)
- Measures of success (MOEs)

**Process Activities:**
1. **Identify all stakeholders across the life cycle** — from concept through disposal; capture in a stakeholder register with roles and involvement. Includes both internal and external stakeholders, and crucially, the *approving authorities* (regulators, certifiers).
2. **Elicit stakeholder needs** — using structured brainstorming, interviews, workshops, focus groups, documentation review, and feedback from verification/validation. Topics include expected/off-nominal use cases, desired capabilities, external system interactions, quality attributes ("ilities"), risks/hazards, rationale, criticality, and priorities.
3. **Identify drivers and constraints** — factors outside project control that constrain the solution space (regulations, HMI, operating environment, technology maturity, cost, schedule).
4. **Identify, assess, and handle risks** — anything that could prevent delivery of a successful SoI or impact intended use.
5. **Mature life cycle concepts** — develop architectural and analytical/behavioral models (often MBSE); transform preliminary concepts into a mature, consistent, complete, and feasible set.
6. **Define and baseline the integrated set of needs** — needs are written in structured natural language from the stakeholder's perspective (without "shall"). They define what stakeholders need the system to do, how well, under what operating conditions, and with what compliance.

**Needs vs. Requirements Distinction:**
- *Needs* are from the stakeholder's external, black-box perspective: "The user needs the coffee maker to stop heating water once the user-selected temperature has been achieved."
- *Requirements* are from the developer's internal, white-box perspective: "The Coffee Maker shall stop the heating function once the selected Brew Temperature has been achieved."

### System Requirements Definition

> **Purpose:** Transform the stakeholder view of desired capabilities into a technical, developer view of how the system can achieve those capabilities.

System requirements describe what the SoI **must** fulfill to satisfy stakeholder needs. They:
- Form the basis of system architecture and design activities
- Form the basis of system integration and verification activities
- Provide a means of communication across the project team

**Transforming Needs into Requirements:**
The process is more than rewording — it is an engineering analysis of *what the system must do*. Techniques include functional analysis, interface analysis, data flow diagrams, performance analysis, and needs-to-requirements transformation matrices. MBSE tools (activity diagrams, sequence diagrams, state machine diagrams) support this analysis.

**Requirement Categories (Fit/Form/Function/Quality/Compliance):**
| Category | Description |
|----------|-------------|
| **Function/Performance** | Primary functions the SoI must perform and associated performance (how well, how many, how fast) |
| **Fit/Operational** | Secondary/enabling capabilities — interfaces with external systems, operating environment compatibility, human-system interaction |
| **Form** | Physical characteristics (shape, size, mass); for software: language, lines of code, memory requirements |
| **Quality** | "-ilities": reliability, testability, operability, availability, maintainability, supportability, interoperability |
| **Compliance** | Conformance with design/construction standards and regulations |

**Key Practices:**
- **Attributes:** Associate rationale, trace to source, verification success criteria, owner, category, status, criticality, and priority with each requirement
- **Allocation & Budgeting:** Requirements at one level of the physical architecture are assigned to entities at the next lower level. *Budgeting* decomposes a system-level parameter value (mass, power, bandwidth, time) across lower-level elements — a change in one element's budgeted value affects others.
- **Interface Requirements:** Three-step process: (1) identify interface boundaries and interactions, (2) define interaction characteristics, (3) write the interface requirements (often referencing interface control documents)
- **TBX Management:** Use TBD (To Be Determined) for unknown values and TBR (To Be Resolved) for low-confidence values. Systematic TBX closure is a valuable maturity metric.
- **Traceability:** Bi-directional links between requirements and their sources (needs, parent requirements), enabling impact analysis of changes. A traceability relationship model should be defined at project start.
- **Early Verification Planning:** Define verification success criteria, strategy, and method when requirements are generated — ensures requirements are well-formed and verifiable.

**Requirement Verification vs. Validation (of the requirements themselves):**
- **Verification:** Are the requirements well-formed? (Do they follow the organization's criteria for good requirements?)
- **Validation:** Do we have the *right* requirements? (Do they communicate the intent of the needs? Are they correct and achievable?)

**Artifacts Produced:**
- Well-formed requirements (in models, databases, or documents)
- Requirement tree (hierarchy)
- Traceability matrices
- Defined interactions across boundaries
- Performance budgets
- Initial system verification plans

---

## System Architecture Design Definition

The system architecture design defines system **behavior and structure** characteristics in accordance with derived requirements. It ensures system elements operate together in the applicable operating environment to satisfy stakeholder needs. The modern approach is **model-based** (MBSE with SysML), enabling a digital Authoritative Source of Truth (ASoT) — a repository of requirement, functional, logical, and physical design representations at each abstraction level.

The architecture definition follows a progression:
```
Stakeholder Needs → System Requirements → Functional Architecture → Logical Architecture → Physical Architecture → Detailed Design
```

**Model-Based Approach Highlights:**
- SysML provides industry-standard graphical and textual notations with formal semantics
- The system design model provides an ASoT digital repository of the project technical baseline
- Integrated simulation capabilities enable verification of design decisions in digital environments (digital threads)
- The model with simulations provides a system "digital twin" for design optimization and alternative exploration
- Viewpoints and views are tailored to address each stakeholder's concerns

### Functional Architecture

> The inter-related set of transformative processes and purposeful input-output tasks (i.e., **functions**) that a system performs to produce outputs supporting mission objectives.

Functional architecture defines **what actions** a system can perform and how those actions relate to each other. It is an architectural *view* — not a standalone sub-unit — and is typically developed early in the system life cycle.

**What Functional Architecture Must Define (Minimum):**
1. The functional capabilities the system uses to fulfill mission objectives and meet stakeholder needs
2. Required inputs and outputs for each function
3. Steps through which the system transforms inputs into desired outputs
4. Grouping of steps into independently executable sub-functions (with their inputs/outputs)
5. Variations in inputs, outputs, and execution for each system mode
6. Internal and external interfaces enabling input/output flow for each function

**Relation to Behavioral Architecture:**
- **Functional architecture** = input-output transformations (the *what* of processing)
- **Behavioral architecture** = sequencing and execution of actions, interaction with checkpoint conditions and task initiation/completion (the *when* and *how* of execution)
- Both are often considered jointly in SysML models using Use Case Diagrams, Block Definition Diagrams, Activity Diagrams, and State Machine Diagrams

**Modeling Approaches:**
- Text-based descriptions — suitable for simple systems with few functions
- Visual diagrams (UML/SysML activity diagrams, eFFBDs) — preferred for complex, conditional, or highly interconnected processes
- Digital MBSE models with automated error checking

### Logical Architecture

> A model composed of related technical concepts and principles that support the logical operation of the system. It is a contraction of "Logical View of the System Architecture."

The logical architecture elaborates **what the system must be able to do** to satisfy the stated need — independent of implementation technology (Platform Independent Model / PIM). It includes three primary views:

**1. Functional Architecture View:**
- Set of functions and sub-functions defining transformations to complete the mission
- **Function** = an action transforming inputs into outputs (data, materials, energy): *y = f(x, t)*
- Two kinds of functions: those directly deduced from requirements, and those derived from physical architecture solution alternatives
- **Functional hierarchy:** Decomposition from a single "black box" mission function (F0) into sub-functions and leaf-functions

**2. Behavioral Architecture View:**
- Arrangement of functions, interfaces, execution sequencing, conditions for control/data-flow, and performance levels
- **Control (Trigger):** An element (signal, event) that activates a function as a condition of execution
- **Scenario:** A chain of functions performed as a sequence, synchronized by control flows — expressed via eFFBDs or SysML Activity/Sequence diagrams
- **Operational Mode:** A view focusing on active/non-active states and mode transitions triggered by control flows
- **Behavioral Patterns:** Reusable constructs — sequence, iteration, selection, concurrence, FDIR (failure detection, identification, recovery) patterns

**3. Temporal Architecture View:**
- Classification of functions by execution frequency
- **Synchronous functions:** Executed cyclically
- **Asynchronous functions:** Executed upon event/trigger occurrence
- **Temporal/Decisional Hierarchy:** High-frequency disturbances handled at lower levels; low-frequency changes (operator actions, maintenance) handled at upper levels

**Process Activities:**
1. Identify and analyze functional and behavioral elements from system requirements
2. Assign system requirements (performance, effectiveness, constraints) to functional/behavioral elements; establish traceability
3. Define candidate logical architecture models — model operational modes, scenarios, integrate into behavioral architecture, decompose as needed, assign temporal constraints, perform FMEA
4. Synthesize the selected independent logical architecture model — assess candidates against criteria; the selected model should be as implementation-independent as possible
5. Feedback logical architecture to system requirements and physical architecture development

**Key Pitfalls:**
- Decomposing functions too deeply, too early
- Considering only actions while forgetting inputs/outputs
- Relying only on static function decomposition without behavioral scenarios
- Mixing governance, management, and basic operations

### Physical Architecture

> An arrangement of physical elements (system elements and physical interfaces) that provides the solution for a product, service, or enterprise — implementable through technological system elements. It is a contraction of "Physical View of the System Architecture."

The physical architecture answers: **what concrete elements will realize the logical architecture?** It is a Platform Specific Model (PSM).

**System Elements and Physical Interfaces:**
- A **system element** is a discrete, implementable part: hardware, software, data, humans, processes, procedures, facilities, materials, or any combination
- A **physical interface** binds two system elements together (protocols, connectors, documents, procedures)
- Complex systems are structured in layers: a guideline is **5 ± 2 elements per level**

**Design Properties:**
Properties obtained during system architecture through assignment of non-functional requirements, estimates, analyses, calculations, or simulations. They correspond to stakeholder concerns: reliability, availability, maintainability, modularity, robustness, operability, environmental resistance, dimensional limits, etc.

**Allocation and Partitioning:**
- **Allocation:** Assigning functions from the logical architecture to physical system elements
- **Partitioning:** Decomposing, gathering, or separating functions using affinity criteria (similar technology, efficiency levels, exchange types, control centralization, frequency levels, dependability conditions, enterprise constraints) to identify feasible system elements

**Process Activities:**
1. **Partition and allocate functional elements** — search for system elements/technologies able to perform functions; assess using criteria from design properties; check technology and interface compatibility
2. **Constitute candidate physical architecture models** — group system elements into sub-systems; represent with physical block diagrams; enhance with design properties; optionally use executable prototypes (HW-SW-in-the-loop)
3. **Assess and select** the most suitable candidate — using system analysis and decision management processes
4. **Synthesize the selected model** — formalize physical elements and properties; identify derived elements and requirements; establish traceability

**Legacy and System-of-Systems Considerations:**
- When interactions with external systems are small, use static external interface requirements (standards) as constraints
- When interactions are intense, recognize the SoS context and apply enterprise SE approaches
- For re-use across a family of systems, apply top-down or middle-out design to balance SoI needs with family constraints
- Systems intended for SoS configurations need open/configurable interfaces and clearly defined, packageable functions

### System Detailed Design Definition

> The process of developing, expressing, documenting, and communicating the realization of the architecture through a complete set of design characteristics in a form suitable for implementation.

System design is the link between architecture and implementation. It addresses the **"implement-to"** level: dimensions, shapes, materials, data structures, algorithms — the domain-specific details needed by discipline engineers (mechanical, electrical, software).

**Architecture vs. Design:**
- **Architecture** = structural, functional, behavioral, temporal, and physical aspects; what elements exist and how they relate
- **Design** = characteristics and enablers for implementation; the specific values, dimensions, patterns, and algorithms that make each element realizable

**Design Characteristics and Enablers:**
- *Design characteristics:* dimensions, shapes, materials, data processing structures
- *Design enablers:* formal expressions/equations, drawings, diagrams, tables of metrics with values and margins, patterns, algorithms, heuristics
- Every technological domain (mechanics, electronics, software, chemistry) has its own laws and enablers

**Process Activities:**
1. **Initialize design definition** — plan technology management; identify technologies composing system elements; plan for obsolescence; document the design definition strategy
2. **Establish design characteristics** — allocate remaining requirements; define and check implementable design characteristics using models and heuristics; refine internal/external interfaces; record rationale for major implementation options
3. **Assess alternatives** — identify existing COTS/NDI/reusable elements; assess design options using selection criteria; decide make vs. buy
4. **Manage the design** — capture rationale for all decisions; assess and control design evolution; maintain traceability to architecture and requirements; baseline for configuration management

### System Analysis

> A rigorous approach to technical decision-making used throughout system definition — performing trade-off studies, modeling and simulation, cost analysis, technical risk analysis, and effectiveness analysis.

System analysis is the **evaluation engine** of systems engineering. It is used whenever technical choices or decisions are made to determine compliance with system requirements.

**Three Pillars of Analysis:**

1. **Effectiveness Analysis** — evaluates candidate solutions against essential characteristics: performance, usability, dependability, manufacturability, maintainability, environmental compatibility. The challenge is selecting the *right* set of effectiveness aspects for the specific system.

2. **Cost Analysis** — considers full life cycle costs (LCC / Total Ownership Cost): development, product manufacturing, sales/after-sales, customer utilization, supply chain, maintenance, and disposal.

3. **Technical Risk Analysis** — identifies potential threats/undesired events, their probability, consequences, and mitigation. Technical risks address the system itself (not project process risks): incorrect technology assessment, over-estimation of maturity, part failures, obsolescence, supplier weakness, human factors.

**Trade-Off Studies:**
- Compare characteristics of each candidate system element/architecture against weighted assessment criteria
- Criteria are derived from non-functional requirements and design properties
- Handle both objective and subjective criteria with sensitivity analysis
- Multi-criteria decision models are recommended when criteria exceed ~10

**Where System Analysis Is Applied:**
- **Requirements definition:** Resolving conflicts among requirements (cost vs. performance vs. risk)
- **Logical architecture:** Assessing characteristics of candidate logical architectures
- **Physical architecture:** Evaluating design properties and selecting the most efficient physical solution

**Modeling Techniques:**
- **Physical models:** Scale models, mock-ups, test benches, prototypes, wind tunnels
- **Representation models:** eFFBDs, statecharts, SysML state machine/activity diagrams
- **Analytical models:** Deterministic (statistics, analogy, learning curves), probabilistic (stochastic), and multi-criteria decision models

**Key Principle:** System-level optimization is **not** the sum of individually optimized elements. Coherence across decomposition levels must be maintained.

---

## Related Chapters

- [[00_Introduction_to_SEBoK]] — Overview of the SEBoK structure and knowledge areas
- [[02_Nature_of_Systems]] — Foundational system concepts: Fit–Form–Function triad, engineered vs. natural systems
- [[01_Software_Requirements]] — Software-specific requirements engineering: elicitation, analysis, specification, validation, management
- [[02_Software_Architecture]] — Software architecture design, architectural views, patterns, and quality attributes
- [[03_Software_Design]] — Detailed software design principles, patterns, and implementation considerations
- [[05_Software_Testing]] — Verification and validation; testing aligned with requirements and architecture
