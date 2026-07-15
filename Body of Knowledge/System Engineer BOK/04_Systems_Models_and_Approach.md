---
tags:
- modeling
- systems-approach
- systems-engineering
- sebok
---

# 04 — Systems Models and Approach

> *Source: SEBoK v2, Part 2 — Representing Systems with Models & Systems Approach Applied to Engineered Systems*

## Purpose

This chapter summarizes two foundational Knowledge Areas from Part 2 (Foundations of Systems Engineering) of the Guide to the Systems Engineering Body of Knowledge (SEBoK v2):

1. **Representing Systems with Models** — the role of abstraction, modeling languages, model types, and standards in understanding, designing, and communicating about systems.
2. **Systems Approach Applied to Engineered Systems** — a structured, holistic problem-solving paradigm that spans the entire life of an engineered system, from identifying problems through deploying and sustaining solutions.

Together, these KAs equip the systems engineer with the conceptual tools (models) and the methodical framework (the systems approach) needed to tackle complex engineered system problems.

---

## Representing Systems with Models

A **model** is a simplified, selective representation of a system at some point in time or space, intended to promote understanding. As an abstraction, it offers insight into one or more aspects: function, structure, properties, performance, behavior, or cost.

Modeling serves to make concepts concrete and formal, enhance quality and productivity, and reduce the cost and risk of systems development. It occurs at many levels — component, subsystem, system, and system-of-systems — and throughout the entire life cycle.

### What is a Model?

A model can take physical, mathematical, or logical form:

- **Physical model**: a concrete mockup (e.g., a model airplane).
- **Mathematical model**: represents relationships such as flight trajectories via acceleration, speed, and position equations.
- **Logical model**: captures relationships such as fault trees showing how engine failure leads to loss of power.

A model is expressed in a **modeling language** that has:

- **Abstract syntax** — the constructs and rules for building well-formed models.
- **Concrete syntax** — the symbols or text used to express those constructs.
- **Semantics** — the meaning assigned to each construct.

Models are built using **modeling tools** (typically software) that provide tool palettes, syntax checking, and conformance enforcement.

**Model-Based Systems Engineering (MBSE)** is "the formalized application of modeling to support system requirements, design, analysis, verification, and validation activities beginning in the conceptual design phase and continuing throughout development and later life cycle phases" (INCOSE 2007). In MBSE, models are the primary artifacts — managed, controlled, and integrated — in contrast to the traditional document-centric approach.

### Why Model?

System models serve many purposes throughout the life cycle:

| Purpose | Description |
|---|---|
| **Characterizing an existing system** | Capture poorly documented legacy designs to facilitate maintenance or upgrade |
| **Mission and concept formulation** | Explore trade-spaces early; validate that requirements meet stakeholder needs |
| **Design synthesis and requirements flowdown** | Support architecting; flow requirements to components across functional, interface, performance, and physical domains |
| **Integration and verification support** | Integrate hardware/software design models with system-level models; enable hardware/software-in-the-loop testing |
| **Training support** | Simulate system aspects to train operators and maintainers |
| **Knowledge capture and evolution** | Retain organizational knowledge; support product family evolution |

**Indicators of an effective model:**

- **Scope** — appropriate breadth (requirements coverage), depth (decomposition level), and fidelity (level of detail).
- **Quality** — adherence to modeling guidelines (naming conventions, proper use of constructs, reuse considerations). Model quality is distinct from design quality.
- **Metrics** — models provide data for progress assessment, effort/cost estimation, technical quality/risk assessment, and model quality assessment.

### Types of Models

**Formal vs. Informal:** An informal model (e.g., a sketch or free-text description) may lack precision. MBSE focuses on formal models with well-defined syntax and semantics.

**Physical vs. Abstract:**

| Type | Sub-type | Description |
|---|---|---|
| **Physical** | — | Concrete, tangible representation |
| **Abstract — Descriptive** | — | Describes logical relationships (whole-part trees, interconnections, functions, test cases) |
| **Abstract — Analytical** | Dynamic | Time-varying state (aircraft position, velocity, fuel consumption over time) |
| **Abstract — Analytical** | Static | Non-time-varying computations (mass properties, reliability predictions) |

Hybrid models combine descriptive and analytical aspects.

**Domain-specific models** target properties (performance, reliability, thermal, power, structural), design disciplines (electrical, mechanical, software), subsystems (communications, power distribution), or application domains (aerospace, automotive, medical). A single system model often spans multiple domains.

**Simulation** is the method for implementing a model over time. Classifications include: stochastic/deterministic, steady-state/dynamic, continuous/discrete, local/distributed. Simulations may be live, virtual, or constructive, and can integrate real hardware/software in the loop.

**Model integration** across domains requires semantic interoperability — ensuring the same construct means the same thing across models. This is achieved through model transformations, standard data exchange formats, and shared repositories.

### System Modeling Concepts

**Abstraction** is the most fundamental concept: hiding unimportant details to focus on essential characteristics. Two key abstraction techniques:

- **View and Viewpoint** (IEEE 1471): A *view* represents the whole system from the perspective of a related set of concerns. A *viewpoint* is the specification/conventions for constructing a view. Standard views include requirements, functional, structural, parametric, and discipline-specific views (reliability, safety, security).

- **Black-Box and White-Box Models**: A black-box model exposes only externally visible features and behavior. A white-box model reveals internal structure and behavior. This abstraction is recursively applied at each level of design decomposition.

**Conceptual Model**: The set of concepts within a system and their relationships. The Systems Engineering Concept Model defines concepts for requirements, behavior, structure, properties, and stakeholders. It serves as a meta-model that can specify the abstract syntax of a modeling language.

**Integrating Supporting Aspects**: MBSE extends the core system model to incorporate:

- Project and Engineering Management (project-product integration via WBS, DSM)
- Requirements Engineering and Management (SysML Requirements Diagram, OPM-based authoring)
- Risk Modeling, Analysis, and Management (MBRA, CORAS, STAMP, ROSE)
- Quality Assurance, Testing, Verification and Validation
- Analysis of "-ilities" (RAMSS: Reliability, Availability, Maintainability, Safety, Security; plus Manufacturability, Extensibility, Robustness, Resilience, Flexibility, Evolvability)

### Modeling Standards

Standards enable cross-discipline, cross-project, and cross-organization communication and model integration.

**Descriptive modeling languages:**

| Standard | Domain |
|---|---|
| Functional Flow Block Diagram (FFBD) | Functional modeling |
| IDEF0 | Functional modeling |
| Object-Process Methodology (OPM) — ISO/PAS 19450 | Holistic system modeling |
| SysML (OMG) | General-purpose systems modeling |
| UPDM (DoDAF/MODAF profile) | Architecture frameworks |
| OWL (W3C) | Ontology / knowledge representation |

**Analytical models & simulation:** DIS (IEEE 1278), HLA (IEEE 1516), Modelica, FUML

**Data exchange:** AP-233 (ISO 10303-233), ReqIF (OMG), XMI (OMG), RDF (W3C)

**Model transformations:** QVT (OMG), SysML-Modelica transformation, OPM-to-SysML transformation

**General frameworks:** MDA® (OMG), IEEE 1471 / ISO/IEC 42010 (Architecture Description)

**Domain-specific:** AADL (software architecture), MARTE (real-time/embedded), UML (software design), VHDL (hardware), BPMN (business processes)

---

## Systems Approach Applied to Engineered Systems

The **systems approach** is a comprehensive problem identification and resolution paradigm based on the principles, concepts, and tools of systems thinking and systems science, combined with engineering problem-solving. It incorporates a holistic view covering the larger context — engineering and operational environments, stakeholders, and the entire life cycle.

### Overview of the Systems Approach

The systems approach for engineered systems examines the **"whole system, whole lifecycle, and whole stakeholder community"** to ensure the system's purpose is achieved sustainably without unintended negative consequences. This prevents "transferring the burden" to other parts of the environment and deters sub-optimization.

**Four key themes:**

1. **Whole System** — Multiple system types must be understood (Lawson's System Coupling Diagram: situation system, respondent system, system assets; Martin's "seven samurai": context, intervention, realization, deployed, collaborating, sustainment, and competing systems).

2. **Whole Lifecycle** — Ring's Ellipse Graphic represents the continuous cycle of value-seeking system management, applicable to long-life and rapidly refreshed systems alike.

3. **Whole Problem** — Problems range from "tame" to "wicked" (Rittel & Webber). Frameworks like PESTLE and STEEPLED capture the full scope. Sillitto emphasizes that some problem parts are "solved" while others must be "managed."

4. **Multi-Disciplinary** — Systems thinking methods must span product, service, enterprise, and system-of-systems contexts. Traditional SE standards focus on system solution development; a full systems approach requires broader problem exploration and purpose-driven life cycle thinking.

**Hitchins' seven SE principles:**

| Principle | Description |
|---|---|
| A: The Systems Approach | SE is applied to a SoI in a wider systems context |
| B: Synthesis | Bring together parts to create whole system solutions |
| C: Holism | Consider consequences on the wider system |
| D: Organismic Analogy | Consider systems as having dynamic "living" behavior |
| E: Adaptive Optimizing | Solve problems progressively over time |
| F: Progressive Entropy Reduction | Continue to make systems work through maintenance, sustainment, and upgrades |
| G: Adaptive Satisfying | A system succeeds only if it makes winners of its success-critical stakeholders |

**Synthesis for SEBoK** — the systems approach activities:

1. Identify and understand relationships between problems and opportunities
2. Gain thorough understanding of the problem in its wider context
3. Synthesize viable system solutions
4. Analyze and choose between alternatives
5. Provide evidence of correct implementation and integration
6. Deploy, sustain, and apply the solution

All activities are considered within a life cycle framework that may require concurrent, recursive, and iterative application.

### Engineered System Context

The single most important principle: the systems approach is applied to an **engineered system context**, not just a single system. Four types of engineered system contexts are recognized:

| Context | System-of-Interest | Characteristics |
|---|---|---|
| **Product System** | The product itself | Clear usage intent; formal acceptance by acquirer; fits into a wider product hierarchy or service |
| **Service System** | The service system (technology, infrastructure, people, resources) | Value co-created at time of use; dynamic; may use products owned outside the enterprise |
| **Enterprise System** | The enterprise system | Constantly evolving; ill-defined context; goals of shareholder value and customer satisfaction |
| **System of Systems (SoS)** | Cooperating set of systems | Loose coupling; systems may join/leave; late binding; each system has own objectives and ownership |

**System context boundaries:**

- **Narrower System-of-Interest (NSoI)** — system of direct concern; scope of authority or control.
- **Wider System-of-Interest (WSoI)** — logical boundary containing all elements needed to fully understand system behavior.
- **Environment** — engineered, natural, and/or social systems with which the WSoI directly interacts.
- **Wider Environment** — systems with no direct interaction but which influence life cycle decisions.
- **Meta-system (MS)** — exists outside the WSoI and exercises direct control over it.

### Identifying and Understanding Problems

The first step in the systems approach is "the recognition and formulation of the problem" (Jenkins 1969). "Problem or opportunity" recognizes that the starting point may be a negative situation or a positive improvement opportunity.

**Problem exploration** draws from both hard and soft systems thinking:

- **Soft systems** (e.g., Checkland's SSM): considers a *problematic situation* rather than a single defined problem. Uses rich pictures, root definitions, and conceptual models to help stakeholders understand each other's viewpoints.
- **Hard systems**: assumes a problem can be objectively stated and an engineered system solution developed. Still requires exploring the problem with stakeholders before solution synthesis.

**Problem classification** (Edson):

- **Tame problems** — solution may be well-defined and obvious.
- **Regular problems** — encountered regularly; solutions not obvious but tractable.
- **Wicked problems** — cannot be fully solved or even fully defined; full effects of interventions are unknowable.

**Key questions for problem understanding:** How difficult/well understood is the problem? Who or what is impacted? What are the various stakeholder viewpoints?

**Problem context** should include:
- Aim and objectives of the SoI and wider SoI
- Stakeholder outcomes and benefits
- Economic, informational, and other boundary conditions
- Cost, time to deployment, time in use, and operational effectiveness constraints

### Synthesizing Possible Solutions

System synthesis produces one or more solution options based on the problem context, for the life cycle of the system:

- Define options for a SoI with required properties and behavior.
- Provide solution options that are potentially realizable within time, cost, and risk constraints.
- Assess each candidate in its wider system context.

**Essential concept: Holism** — the system must be considered as a whole; behavior derives from parts plus interactions between parts plus interactions with other systems. Emergent properties (unpredictable from elements alone) must be evaluated.

**Synthesis activities:**

1. **Identification of the boundary** — what is inside vs. outside the system.
2. **Identification of functions** — what the system must do in its larger context.
3. **Identification of elements** — physical (hardware, software, humans), conceptual (ideas, plans, hypotheses), or processes.
4. **Division of elements** — decomposition into hierarchical layers.
5. **Grouping of elements** — forming subsystems; the largest group is the SoI.
6. **Identification of interactions** — interfaces between elements (both technical and managerial aspects).

**Top-down vs. bottom-up:** A top-down (behavioral) approach starts with system boundary and functions then decomposes. A bottom-up (structural) approach starts with major elements and builds out. Both are driven toward realizable solutions — elements that are already available, can be created from existing elements, or are described as system contexts needing future synthesis.

### Analysis and Selection

System analysis evaluates solution artifacts against assessment criteria derived from the problem context:

- **Effectiveness analysis** — performance against primary and enabling functions; includes generic non-functional qualities (safety, security, reliability, maintainability for products; agility, resilience, flexibility for services/enterprises).
- **Trade-off studies** — comparing characteristics of each candidate to determine the best global balance. Variables: cost, technical risk, effectiveness.

**Trade-off study components:**

| Component | Description |
|---|---|
| Assessment criteria | Absolute or relative measures for comparison |
| Boundaries | Limits on which characteristics are considered |
| Scales | Quantification of characteristics (linear, logarithmic) |
| Assessment score | Quantified rating per candidate per criterion |
| Optimization | Improving scores of interesting solutions |
| Sensitivity analysis | Testing robustness of selection against small variations in criteria weights |

**Systems principles of analysis:**
- Iterative activity of trade studies between solution options.
- Criteria based on an ideal system description from the problem context.
- Must consider complete solution behavior in all possible wider system contexts and environments.
- Equal consideration to primary and enabling systems.
- At minimum, criteria must include cost and time scale constraints.
- Must deal with both objective and subjective criteria.

### Implementing and Proving

**Verification** — determination that each system element meets documented specification requirements. Performed at each level of the system hierarchy.

**Validation** — determination that the entire system meets stakeholder needs. Occurs only at the top level of the system hierarchy.

In the systems approach, proving is the abstract level of providing evidence. In SE practice, it involves quantitative evidence from tests, demonstrations, and other methods.

### Deploying, Using, and Sustaining

A systems approach considers the total system and total life cycle. Creation of the system is rarely what solves the problem — it is the *use* of the system solution.

**Deployment (Transition):** Transferring custody from development to operations. Two parts: (1) ensuring interoperability with surrounding systems, (2) ensuring safety and critical operational properties. Transition of service systems is often two-stage: infrastructure acceptance then per-realization acceptance.

**Use (Operation):** System effectiveness is considered throughout operational life. Emergent behavior must be addressed in three ways: planned-for during realization, mechanisms for handling unexpected emergence during use, and procedures for wider-system consequences.

**Sustainment (Maintenance):** Handles entropy — maintaining the SoI in a viable state through resource management (acquisition, storage, distribution, conversion, disposal) and viability management (homeostasis, resilience, adaptability).

**Disposal:** Removal of system elements from the operational environment with permanent termination of use, including handling of hazardous/toxic materials and waste products. Requires its own enabling systems and must be considered from early in the life cycle.

### Applying the Systems Approach

The systems approach is applied dynamically through three key principles:

**Concurrency** — Activities of problem identification, solution synthesis, analysis/selection, implementation/proving, and deployment/sustainment/use are applied concurrently, reflecting their interrelationships. The "hump diagram" shows activity intensities varying across life cycle stages.

Six questions cycle around the value cycle:
1. What values do stakeholders want/need?
2. What system outcomes could improve this value?
3. What system can provide these outcomes?
4. How do we create such a system?
5. How do we deploy and use the system to achieve the outcomes?
6. Do these outcomes provide the expected improvement in value?

**Iteration** — The approach is applied iteratively to move toward an acceptable solution. Three iteration types:

| Type | Description |
|---|---|
| Sequential | Single application with iteration between stages to resolve detailed issues |
| Incremental | Successive versions; each increment adds functionality/effectiveness |
| Evolutionary | Series of applications for alternative solutions; each cycle provides learning for the next |

**Recursion** — The systems approach for one SoI is nested inside another. Examples: trades at one level require trades at a lower level; analysis of a system requires analysis of a system element; synthesis of a solution requires sub-system elements. At some level the approach completes and "rolls up" to allow higher levels to progress.

**Stakeholder responsibilities in different contexts:**

| Role | Product Context | Service Context | Enterprise Context |
|---|---|---|---|
| Acquirer | Procures product from supplier | Procures service system | May be internal or external |
| Supplier | Delivers product; may offer support services | Delivers service system; may use products not owned by enterprise | — |
| Operator | Uses knowledge/skills/procedures to operate the system | — | — |
| User/Customer | Benefits from operation | Co-creates value at time of service request | — |

---

## Related Chapters

- [[SEBoK v2 - Overview]]: Structure and scope of the SEBoK
- [[01_Systems_Engineering_Fundamentals]]: Systems thinking, systems science, and the nature of systems that underpin both modeling and the systems approach
- [[05_Life_Cycles_and_Processes]]: Part 3 of the SEBoK — how the systems approach activities map to formal SE technical and management processes
- [[SWEBOK v4 - Overview]]: Software Engineering Body of Knowledge; related modeling and engineering process concepts

---

*References: INCOSE Systems Engineering Handbook; Friedenthal, Moore & Steiner, A Practical Guide to SysML; Dori, Object-Process Methodology; Wymore, Model-Based Systems Engineering; Checkland, Systems Thinking, Systems Practice; Hitchins, Systems Engineering: A 21st Century Systems Methodology; Ring, "A Value Seeking Approach to the Engineering of Systems"; Lawson, A Journey Through the Systems Landscape.*
