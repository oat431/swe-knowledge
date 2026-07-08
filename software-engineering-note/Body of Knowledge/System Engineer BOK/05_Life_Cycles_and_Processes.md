---
tags:
- life-cycle
- processes
- systems-engineering
- sebok
---

# Life Cycles and Processes

> *Source: SEBoK v2, Part 3a-3b — Life Cycle Concepts and Process Concepts*

> **Purpose:** This knowledge area covers the fundamental concepts of system life cycles, development approaches, agile methods, process description and application, and the selection and tailoring of both life cycle models and processes. Together they form the backbone of disciplined systems engineering practice — differentiating it from ad-hoc approaches — by providing a structured framework of processes and activities organized into stages, combined with the flexibility to select, adapt, and tailor those frameworks to the specific needs of each project and system.

## Life Cycle Terms and Concepts

### Core Definitions

A **life cycle** is the evolution of a system, product, service, project or other human-made entity from conception through retirement (ISO/IEC/IEEE 24748-1:2024). A **stage** is a period within the life cycle that relates to the state of its description or realization. Stages relate to major progress, achievement milestones, or decision points. A **life cycle model** is a framework of processes and activities concerned with the life cycle, organized into stages and acting as a common reference for communication and understanding.

### Typical Life Cycle Stages (ISO/IEC/IEEE 15288)

| Stage | Primary Goals |
|---|---|
| **Concept** | Identify stakeholders' needs. Explore concepts. Propose viable solutions. |
| **Development** | Refine system requirements. Create solution description. Build system. Perform verification and validation. |
| **Production** | Produce systems. Inspect and test. |
| **Utilization** | Operate system to satisfy users' needs. |
| **Support** | Provide sustained system capability. |
| **Retirement** | Store, archive, or dispose of the system. |

Four common principles associated with life cycle models:
1. A system progresses through specific stages during its life.
2. Enabling systems should be available for each stage to achieve the outcomes of that stage.
3. Quality characteristics (producibility, usability, supportability, disposability) should be specified and designed into the system at appropriate stages.
4. Stages begin and end based on criteria or external events.

### Decision Gates and Entry/Exit Criteria

Movement between stages represents a **decision gate** — a milestone with specific entry and exit criteria. Decision options at each gate include: begin another stage, continue this stage, restart another stage, pause activity, or terminate. Criteria can include an assessment of **technical debt** and its implications for other stages. Not all parts of a system-of-interest reach the same maturity at the same time, so interim maturity reviews are essential.

### Technical Reviews and Audits

A **technical review** is a series of SE activities conducted at logical transition points, assessing project progress against technical requirements using pre-established criteria. An **audit** is an independent examination of work products to assess compliance with specifications, standards, or contractual agreements.

Technical reviews and audits support:
- **Project assessment and control** — visibility into technical progress and risks, supporting decision gates
- **Configuration management** — establishing, revising, and verifying configuration baselines
- **Validation** — through involvement of appropriate stakeholders including end users

Reviews can be **schedule-based** (explicit dates), **event-based** (end of a phase/delivery of artifacts), or **evidence-based** (achieving a specific technical risk level). The review process follows: Prepare → Conduct → Complete.

### Life Cycle Spectrum

All life cycle approaches fall on a spectrum from fully plan-driven (high certainty, stable requirements) to fully agile/emergent (high uncertainty, evolving requirements). Neither extreme is common in practice. The DevOps/DevSecOps infinity loop model represents one end of this spectrum, where there is no distinction between development and support — the work is never "done" — and retirement risks are considered negligible.

## Development Approaches

Development approaches apply primarily to the concept and development stages. The classification is based on: how well requirements are known, whether there are increments, and what those increments are used for. Development is always concurrent and iterative to some degree.

### Sequential Development Approach

**Best for:** Systems with well-defined, stable requirements unlikely to change. Common in aerospace, defense, healthcare, and civil engineering where precision, predictability, and traceability are critical.

The sequential approach follows a structured, phase-based process. The original **Waterfall model** (Royce, 1970) outlines: requirements analysis → system design → implementation → integration and testing → deployment → maintenance. Although Royce advocated for iteration (especially early), this aspect was often overlooked in practice. In modern practice, sequential approaches incorporate planned iterations in early phases while maintaining an overall linear flow.

**Practical considerations:** Late modifications are costly and time-consuming. Early engagement in requirement analysis and design is critical. Verification and validation typically occur after integration, demanding thorough interface definitions and systems thinking during earlier phases.

**Vee Model:** An extension of the Waterfall model that integrates verification and validation explicitly. Visualized as a "V" shape:
- **Left side (decomposition):** System definition — requirements flow down from system to subsystem to component
- **Bottom:** The "build" point
- **Right side (integration):** System realization — verification and validation flow upward from component to subsystem to system

The Vee model highlights continuous validation with stakeholders, the need to define verification plans during requirements development, and the importance of continuous risk assessment. It depicts relationships between decomposition and integration levels, encouraging engineers to think ahead about how each part will be verified to reduce late failures. Note: the Vee Model covers only the development stage, not the full life cycle.

### Incremental Development Approach

**Best for:** Complex, high-uncertainty projects involving new technologies, evolving requirements, or significant stakeholder interaction. Delivers value in chunks with early feedback.

System development proceeds through repeated cycles of planning, designing, implementing, and evaluating. Each cycle may result in a tangible increment (deliverable/prototype) or serve as a learning opportunity. Early increments provide stakeholders with partial versions enabling verification, validation, and feedback before the final system is complete.

**Practical considerations:** Traceability and configuration management take on different character than sequential — requirements, design elements, and interfaces may evolve over iterations. Robust version control and clear documentation of assumptions are essential. Validation and verification activities are distributed across the timeline rather than concentrated at the end, demanding flexible test planning.

**Incremental Commitment Spiral Model (ICSM):** A modern incremental approach extending Boehm's Spiral Model. Divides development into a series of cycles, each leading to a commitment review. Stakeholders and engineers make incremental commitments only as far as evidence and confidence gathered in each cycle justify. Each cycle involves stakeholder engagement, feasibility assessments, risk analysis, and prototype development. Provides clear decision checkpoints useful in defense, transportation, and critical infrastructure.

### Evolutionary Development Approach

**Best for:** Innovative or dynamic markets where requirements are likely to change and speed of delivery is important. Reduces risk by allowing early validation through real-world use.

Systems are developed, built, and delivered in a series of working iterations, each released to users. Feedback from users, performance data, and market response shape subsequent versions. Each iteration adds new capabilities or improves existing ones.

**Practical considerations:** Architecture must be modular and scalable to accommodate extension without major rework. Interfaces should be well-defined and loosely coupled. Each iteration must meet quality standards independently. Traceability and configuration management are even more important — maintaining clear documentation of what changed, why, and its impact is essential. Close ongoing collaboration with stakeholders is vital.

**DevOps as example:** A set of principles blending development, operations, and continuous feedback loops. Promotes CI/CD (continuous integration / continuous delivery), enabling rapid iteration and frequent releases. Can also be understood as a life cycle model — an infinite repetition of development and operations.

### Agile Development Approach

**Best for:** Dynamic environments where customer needs evolve rapidly. Emphasizes delivering value early and often.

Agile development is based on iterative development, frequent inspection and adaptation, and incremental deliveries where requirements and solutions evolve through collaboration in cross-functional teams and through continual stakeholder feedback. Work is broken into small, manageable units called **iterations** or **sprints** (1–4 weeks), each delivering a potentially shippable product increment.

**The Agile Manifesto (2001)** outlines four core values for software development:
1. Individuals and interactions over processes and tools
2. Working systems over comprehensive documentation
3. Customer collaboration over contract negotiation
4. Responding to change over following a plan

These principles were crafted for software development and may not transfer seamlessly to the broader domain of systems engineering, which involves hardware, human elements, and processes with different considerations, timelines, and integration points.

**Practical considerations:** Teams use system-level user stories ("As a <role>, I want <activity> so that <business value>"). Frequent stand-ups, sprint planning, reviews, and retrospectives foster continuous learning. Decision points avoid terms like "milestones" and "decision gates" — stakeholder interaction is more frequent, smaller in scope, and less formal.

**Frameworks:** Scrum (most widely used — time-boxed sprints, defined roles), SAFe (Scaled Agile Framework — scales Agile across multiple teams with synchronized delivery cycles), and LeSS (Large-Scale Scrum — a lighter-weight alternative extending Scrum principles to multiple teams).

### Lean Engineering

**Lean Systems Engineering (LSE)** is the application of lean thinking to SE and related enterprise and project management. The goal is to deliver the best life-cycle value for technically complex systems with minimal waste.

**Six Lean Principles (Oppenheim, 2011):**
1. **Capture value defined by the customer** — with precision, clarity, and completeness before resource expenditures ramp up.
2. **Map the value stream and eliminate waste** — map end-to-end linked tasks and eliminate non-value-added activities.
3. **Flow the work** — through planned, streamlined value-adding steps without stopping, unplanned rework, or backflow.
4. **Let customers pull value** — every task must be justified by a specific need and completed when the customer needs the output.
5. **Pursue perfection** — continuously improve processes (distinguish process improvement from work output refinement, which is bounded by value proposition).
6. **Respect for people** — people are the most important resource; they identify problems honestly and plan solutions by consensus.

**Lean Enablers for Systems Engineering (LEfSE):** A collection of "dos" and "don'ts" of SE based on lean thinking, covering a spectrum of SE and enterprise management practices. Not intended to be mandatory but to serve as a checklist of good practices.

## Agile Approaches (Agile SE, Industrial DevOps)

### Agile Systems Engineering

Agile SE is a principle-based approach for designing, building, sustaining, and evolving systems when knowledge is uncertain and environments are dynamic. The key distinction: **Agile SE is about *being* agile, not *doing* agile.** It is a *what*, not a *how*.

**Eight Strategic Aspects of SE Agility** (ISO/IEC/IEEE 24748-10):

| Aspect | Need | Behavior |
|---|---|---|
| **Adaptable Modular Architecture** | Facilitate product and process experimentation, modification, evolution | Composable and reconfigurable designs from reusable assets |
| **Iterative Incremental Development** | Minimize rework, maximize quality, facilitate innovation | Incremental loops of building, evaluating, correcting, improving |
| **Attentive Situational Awareness** | Timely knowledge of emergent risks and opportunities | Active monitoring of internal and external environmental factors |
| **Attentive Decision Making** | Timely corrective and improvement actions | Systemic linkage of situational awareness to decisive action |
| **Common-Mission Teaming** | Coherent collective pursuit of a common mission | Engaged collaboration, cooperation, and teaming among all stakeholders |
| **Shared-Knowledge Management** | Accelerated mutual learning, single source of truth | Facilitated communication, collaboration, and knowledge curation |
| **Continual Integration & Test** | Early revelation of system integration issues | Integrated test and demonstration of work-in-process |
| **Being Agile** | Attentive operational response to evolving knowledge | Sensing, responding, evolving |

The degree of agility is a product of how many of these aspects are operational and how effectively each contributes. Big-bang concurrent implementation of all aspects is not necessary to gain benefits.

### Industrial DevOps

Industrial DevOps (IDO) extends Lean, Agile, and DevOps principles to **cyber-physical systems** (CPS) throughout their life cycles. While DevOps originates from software, IDO applies these principles to systems that blend hardware and software.

**Nine Principles of Industrial DevOps:**

| Principle | Benefit |
|---|---|
| 1. Organize Around the Flow of Value | Reduces handoffs; improves delivery speed |
| 2. Apply Multiple Horizons of Planning | Increases delivery predictability; reduces risk |
| 3. Implement Data-Driven Decisions | Increases transparency into scope, schedule, cost, quality |
| 4. Architect for Change and Speed | Rapidly adapts to change; reduces lead time and impact |
| 5. Manage Queues and Create Flow | Reduces bottlenecks; increases delivery speed |
| 6. Establish Cadence / Synchronization | Enables shorter integration loops; reduces risk |
| 7. Integrate Early and Often | Faster feedback cycles; reduced risk |
| 8. Shift Left | Reduced rework; built-in quality; increased delivery speed |
| 9. Adopt a Growth Mindset | Increased quality and innovation |

**Enabling Practices:** Value Stream Management, Integrated Digital Engineering (IDE) with digital twins, Agile Program Execution, Test Automation and Virtualization, CI/CD, Infrastructure as Code (IaC), AI and Digital Twins, Lean Governance and Compliance Automation.

## Life Cycle Model Selection and Adaptation

### Selecting the Life Cycle Model

A life cycle model is a framework of processes and activities organized into stages, acting as a common reference. Organizations create and maintain life cycle models to develop systems that meet their strategic plans, policies, goals, and objectives.

Life cycle models should always be **organization-specific**. Organizations may develop several alternative life cycle models with instructions on which to use in which cases and what adaptation may be needed. Inputs include: legislative and regulatory requirements, industry standards and training, academic research, and future concepts.

**Value Proposition of company-wide life cycle models:**
- Repeatable/predictable performance across projects
- Leverage proven successful practices across projects
- Enable process improvement across the organization
- Improve ability to transfer staff across projects
- Enable leveraging lessons learned
- Improve startup of new projects (less reinventing the wheel)
- Shared resources (tool support, document templates, knowledge management)

Life cycle models must be **continuously evaluated and improved** to keep pace with changing organization and customer requirements. Key performance indicators should be developed to measure efficiency and effectiveness.

### Adapting the Life Cycle Model

Adaptation tailors the model to specific project needs — adding, deleting, combining, or modifying stages and their entry/exit criteria. Adaptation guidelines specify the extent to which models may be adapted; exceeding these limits requires organizational approval. All adaptations must be documented.

**Adaptation for multi-party scenarios:** An organization might buy a concept, develop the system, outsource production, sell to a customer who operates it, and handle disposal — requiring careful management of life cycle stage boundaries and transitions.

**Adaptation for domains:** A life cycle model appropriate for software may differ from one for hardware. A system with both may use a combination of life cycle models, synchronized at certain points.

**Adaptation for all stages:** Approaches need to be selected and adapted not just for development but for production, utilization, support, and retirement.

### Selecting the Development Approach

The development approach for the system-of-interest should be selected after the life cycle model is chosen and adapted. Different system elements and enabling systems may use different approaches.

**Candidate approaches:** Sequential, Incremental, Evolutionary, Agile.

**Selection considerations:**
- How well are requirements understood?
- How large is the system?
- How quickly is technology changing?
- How constrained are staff and budget?
- Does the user have preferences on capabilities in first delivery?
- Does the user need to phase out an old system at once?

Risk analysis can compare development approaches. PMI's Situation Context Framework (SCF) defines seven dimensions (team size, geographic distribution, organizational distribution, skill availability, compliance, domain complexity, solution complexity). Agreement between acquirer and supplier on approach is essential.

### Adapting the Development Approach

Since every project is unique, the development approach must be adapted. Reasons include: previously unknown technologies, diverse system elements with different domain characteristics, and different make-or-buy strategies.

**Adaptation for domains:** For large complex systems using different approaches for different elements, development results must be regularly aligned. Intervals should not be too short (hardware doesn't change as fast as software) or too long (design decisions may be made without cross-domain consideration).

**Adaptation for quality characteristics:** Certain quality characteristics dominate — stealth in military submarines demands tight cross-element coordination; safety and security in healthcare demand predictability, stability, repeatability, and high assurance (characteristics of sequential approaches).

## Process Concepts

### Process Description (ISO/IEC/IEEE 24774)

A **life cycle process** is defined by a uniform structure standardized by ISO/IEC/IEEE 24774. At minimum, every process description includes three required elements:

| Element | Description |
|---|---|
| **Process Name** (Required) | A concise noun phrase identifying the main focus. Avoid verbs. |
| **Process Purpose** (Required) | High-level goals defining why the process is performed. Ideally one sentence: "The purpose of the xxx process is..." Avoid summarizing activities or outcomes; avoid "and" to prevent listing unrelated goals. |
| **Process Outcomes** (Required) | Measurable, observable results achieved by executing the process. Written as present-tense declarative sentences. Typically 3–7 outcomes, each directly supporting the purpose. |

**Optional elements:** Activities (sets of cohesive tasks), Tasks (required/recommended/permissible actions), Inputs (items transformed into outputs), Outputs (artifacts or information items), and Notes/Controls/Constraints.

Consistent process descriptions ensure they are understandable, comparable, and manageable. This provides a common language, enables alignment with standards and regulations, and supports systematic analysis and improvement.

### Process Concurrency, Iteration, and Recursion

System life cycle processes are not linear and sequential. Three concepts ensure dynamic, non-linear management:

**Concurrency:** Performing two or more processes in parallel when they do not strongly depend on each other's outputs. Risk management and measurement processes are typically continuous and concurrent. Processes like requirements definition, architecture definition, and verification may occur simultaneously.

**Iteration:** Repeated application and interaction between processes to accommodate stakeholder decisions, incorporate learning and feedback, and handle evolving understanding, architectural constraints, and trade-offs. Frequently occurs between system requirements definition and system architecture definition — evolving requirements drive architectural changes, which prompt updates to requirements, design, or trade-offs.

**Recursion:** Repeated application of life cycle processes throughout the system structure (decomposition layers). Technical Management and Technical Processes are applied recursively at each level until decisions on make, buy, or reuse are made. Outputs from one element level serve as inputs for another, ensuring consistency and integrity across the system hierarchy.

The process activity intensity varies across the life cycle — often visualized as a "hump diagram" where each process peaks during its primary stage but has through-life presence. Solution synthesis typically employs a combination of top-down (problem to solution) and bottom-up (evolution of existing solutions) approaches, often referred to as "middle-out."

## Process Selection and Tailoring

### Guidelines for Selection

Selecting appropriate life cycle processes is a strategic challenge. The goal is a flexible, adaptable framework ensuring compliance, quality, and efficiency while allowing innovation and agility.

**Core principles:**
- **Risk and context:** Process rigor should match system complexity, criticality, and regulatory environment.
- **Tailoring upfront:** Consider it from the start — it will happen. A process framework with tailoring guidelines is more effective than a single uniform process.
- **Traceability:** End-to-end traceability between requirements, design, implementation, verification, and other artifacts.
- **Tool and data integration:** Processes connect with PLM/ALM/CM tools for automation, audit trails, and reporting.
- **Governance and accountability:** Clear process ownership, change control boards, and role definitions.

**Good practices:** Build around core processes (lightweight frameworks), define decision criteria for mandatory vs. optional activities, scale test rigor to risk, train and coach, measure and collect feedback, run pilot projects before full rollout, and ensure processes are effective *before* making them efficient.

### Using ISO/IEC/IEEE 24748-2

ISO/IEC/IEEE 24748-2 provides guidelines for applying ISO/IEC/IEEE 15288. It addresses:
- **Application strategy** — plan, adapt, pilot, formalize, institutionalize
- **System concepts** — defining SoI, system structure, interfacing/enabling/interoperating systems
- **Life cycle concepts** — generic stages, decision gates, application approaches
- **Organizational concepts** — internal application or through agreements
- **Project concepts** — how the four process groups establish common understanding
- **Process concepts** — detailed guidelines for each 15288 process
- **Conformance and adaptation concepts** — guidelines for tailoring

ISO/IEC/IEEE 15288 serves as a foundation for domain-specific standards (ASPICE, ISO 26262, DO-178C, DO-254, IEC 62304, ISO 13485).

### Tailoring at the Project Level

Tailoring adapts ISO/IEC/IEEE 15288 processes to the specific project environment, stakeholders, and risks. It balances two extremes: **too little rigor** (raising risk of technical issues, schedule slips, cost overruns) vs. **too much rigor** (unnecessary overhead, reduced flexibility, inflated cost).

**Project context factors (PMI scaling factors):** Team size, geographic distribution, organizational distribution, skill availability, compliance, domain complexity, solution complexity. Additional factors: stakeholder landscape, budget/schedule, risk tolerance, criticality.

**Key practices:**
- **Fact-based tailoring:** Decisions based on evidence and risk assessment, not intuition. Use structured Decision Management to document alternatives, trade-offs, and rationales.
- **Document assumptions and rationale:** Every tailoring choice should be explicitly justified and recorded.
- **Collaborate with stakeholders:** Involve acquirers, suppliers, and regulators in defining and approving the tailoring approach.
- **Integrate with project planning:** Tailoring should be conducted in parallel with planning — scope, schedule, budget, milestones are linked to chosen processes.
- **Dynamic assessment and control:** Tailoring is not static. Effectiveness and efficiency should be continuously assessed and adjusted as risks and circumstances evolve.
- **Purpose-driven tool selection:** Tools should support tailored processes; process comes before tool.

**Common tailoring traps:**
- Reusing baselines blindly without revisiting assumptions
- Over-engineering — applying all processes "just to be safe"
- Assuming uniformity across all projects
- Failing to engage stakeholders, leading to lack of buy-in

**Practical application of life cycle processes** (ISO/IEC/IEEE 24748-2):
1. Establish agreements to acquire or supply products/services (Agreement Processes)
2. Establish organizational capability to initiate, support, and control projects (Organizational Project-Enabling Processes)
3. Establish and evolve project plans, assess progress, and control execution (Technical Management Processes)
4. Satisfy technical objectives for one or more life cycle stages (Technical Processes)

## Essential Concepts

- **Life cycle ≠ development approach.** The adjectives "waterfall," "incremental," and "evolutionary" apply to the development stage only — not to the life cycle as a whole. There is no such thing as "waterfall production" or "evolutionary retirement."

- **Sequential approaches still iterate.** Fully sequential approaches are purely theoretical. In practice, sequential development incorporates planned iterations, especially in early phases, while maintaining an overall linear flow with well-defined stage boundaries.

- **Agile SE is about *being* agile, not *doing* agile.** It's a strategic capability — sense changes, respond effectively, evolve solutions — not a fixed method. The eight core aspects work in concert; the degree of agility depends on how many are operational and how effectively each contributes.

- **DevOps can be both a development approach and a life cycle model.** As a development approach, it's evolutionary — continuous iteration, deployment, and feedback. As a life cycle model (infinity loop), it removes the distinction between development and support, with work never truly "done."

- **The Vee model is not a life cycle model.** It covers only the development stage. It visualizes the relationship between system definition (decomposition) and system realization (integration), emphasizing that verification planning should begin during requirements development.

- **Process descriptions require only Name, Purpose, and Outcomes** (ISO/IEC/IEEE 24774). Activities, tasks, inputs, and outputs are optional elaborations. The core elements focus on the intended results — the "what" — without prescribing the "how."

- **Processes are not executed in serial, sequential fashion.** Concurrency (parallel execution), iteration (repeated application for refinement), and recursion (application at each system hierarchy level) are fundamental to effective SE practice.

- **One size does not fit all for entry/exit criteria.** Not all parts of a system-of-interest reach the same maturity at the same time. Tailoring of criteria is essential, and interim maturity reviews maintain timely awareness of progress across system elements.

- **Life cycle models should be organization-specific and continuously improved.** Models that work excellently in one organization cannot easily be transferred. They must be reviewed and adapted as the organization, technology, and market change.

- **Tailoring balances risk and process overhead.** The goal is not minimizing process effort or maximizing bureaucracy — it's applying the right processes, at the right level of rigor, at the right time.

- **Tailoring is dynamic, not one-time.** As risks and circumstances evolve throughout the project, tailoring should be revisited and adjusted. It requires continuous assessment and control.

- **ISO/IEC/IEEE 15288 does not prescribe a specific life cycle model.** Its processes, activities, and tasks can be used with agile and other techniques with or without tailoring. This is a common myth about the standard.

- **Technical debt accumulates in any life cycle stage** and negatively impacts design suitability. Decision criteria for stage transitions should include an assessment of technical debt.

- **Different system elements may need different development approaches.** A system with both hardware and software elements could use a combination of development approaches, synchronized at certain integration points.

- **Processes must be effective before they can be made efficient.** Getting the wrong answer faster or cheaper doesn't count. Efficiency improvements only matter once the right outcomes are being achieved.

## Key Standards Referenced

| Standard | Title |
|---|---|
| ISO/IEC/IEEE 15288:2023 | Systems and software engineering — System life cycle processes |
| ISO/IEC/IEEE 24748-1:2024 | Life cycle management — Part 1: Guidelines for life cycle management |
| ISO/IEC/IEEE 24748-2:2024 | Life cycle management — Part 2: Guidelines for the application of ISO/IEC/IEEE 15288 |
| ISO/IEC/IEEE 24748-8:2019 | Life cycle management — Part 8: Technical reviews and audits on defense programs |
| ISO/IEC/IEEE 24748-10:2026 | Life cycle management — Part 10: Guidelines for systems engineering agility |
| ISO/IEC/IEEE 24774:2021 | Life cycle management — Specification for process description |
| ISO/IEC/IEEE 32675:2022 | Information technology — DevOps — Building reliable and secure systems |
| ISO/IEC/IEEE 26515:2018 | Developing information for users in an agile environment |
| INCOSE SEH v5 (2023) | Systems Engineering Handbook, 5th Edition |

## Related Chapters

- [[00_Introduction_to_SEBoK]]: Foundational SE principles and the systems approach
- [[03_Systems_Science_and_Thinking]]: The intellectual foundation — concurrency, iteration, recursion in problem-solving
- [[07_System_Definition_and_Architecture]]: Concept definition and requirements definition processes
- [[08_System_Realization_and_Maintenance]]: Implementation, integration, verification, and validation processes
- [[06_Technical_Management_Processes]]: Project planning, assessment, control, risk, and decision management
- [[08_System_Realization_and_Maintenance]]: Deployment, operation, maintenance, and disposal processes
- [[08_System_Realization_and_Maintenance]]: Life cycle sustainment and service management
- [[14_Enabling_Businesses_and_Enterprises]]: Enterprise and organizational enablers that support life cycle execution
