---
tags:
  - architecture
  - design
  - add
  - documentation
  - views
  - software-architecture
---

# Design & Documentation — SAiP Ch 16–18

> **Source:** Bass, Clements & Kazman, *Software Architecture in Practice* (3rd Edition), Chapters 16–18
> **Purpose:** Cover the full pipeline of architectural design — from capturing architecturally significant requirements (ASRs) and business goals, through the Attribute-Driven Design (ADD) method, to documenting architectures using the Views and Beyond approach.

---

## Chapter 16: Architecture and Requirements

Architectures are driven by **Architecturally Significant Requirements (ASRs)** — requirements that have a profound impact on the architecture. An ASR must exhibit: (a) high architectural impact — including it will likely result in a different architecture; (b) high business or mission value.

### 16.2 Gathering ASRs by Interviewing Stakeholders

Architects are often called upon to help set quality attribute requirements. Stakeholders frequently have no idea what QAs they want — architects can provide feedback on what's reasonable, what's problematic, and where they can deliver better than expected.

#### The Quality Attribute Workshop (QAW)

A facilitated, stakeholder-focused method to generate, prioritize, and refine quality attribute scenarios before the architecture is completed. The QAW involves **8 steps**:

| Step | Name | Description |
|------|------|-------------|
| 1 | QAW Presentation & Introductions | Facilitators describe motivation, explain steps; stakeholders introduce themselves |
| 2 | Business/Mission Presentation | Management stakeholder presents business context, broad requirements, constraints, known QAs (~1 hour) |
| 3 | Architectural Plan Presentation | Architect presents current architectural thinking and plans, to the extent they exist |
| 4 | Identification of Architectural Drivers | Facilitators share distilled list of drivers; stakeholders clarify, add, delete, correct — reach consensus |
| 5 | Scenario Brainstorming | Each stakeholder expresses scenarios; facilitators ensure explicit stimulus + response; at least one scenario per driver from Step 4 |
| 6 | Scenario Consolidation | Similar scenarios are merged (with proposers' agreement) to prevent vote dilution |
| 7 | Scenario Prioritization | Each stakeholder gets votes = 30% of total scenario count; allocate freely across scenarios |
| 8 | Scenario Refinement | Top scenarios refined into six-part form (source-stimulus-artifact-environment-response-response measure); issues around satisfaction are recorded |

**Results** of stakeholder interviews include: a list of architectural drivers and prioritized QA scenarios. These are used to:
- Refine system requirements
- Understand/clarify architectural drivers
- Provide rationale for design decisions
- Guide prototypes and simulations
- Influence architecture development order

### 16.3 Gathering ASRs by Understanding Business Goals

Business goals are the *raison d'être* for building a system. Three possible relationships between business goals and architecture:

1. **Business goals → QA requirements** — every QA requirement should originate from a higher purpose describable in terms of added value
2. **Business goals → direct architectural impact** — without precipitating a QA requirement (e.g., "database team needs work")
3. **No influence** — some business goals (e.g., "reduce cost") may be realized through non-architectural means

#### 11 Standard Business Goal Categories

Use these as conversation starters for elicitation:

| # | Category |
|---|----------|
| 1 | Contributing to the growth and continuity of the organization |
| 2 | Meeting financial objectives |
| 3 | Meeting personal objectives |
| 4 | Meeting responsibility to employees |
| 5 | Meeting responsibility to society |
| 6 | Meeting responsibility to state |
| 7 | Meeting responsibility to shareholders |
| 8 | Managing market position |
| 9 | Improving business processes |
| 10 | Managing the quality and reputation of products |
| 11 | Managing change in environmental factors |

#### Business Goal Scenario (7 Parts)

1. **Goal-source** — people or artifacts providing the goal
2. **Goal-subject** — stakeholders who own the goal and wish it to be true
3. **Goal-object** — entities to which the goal applies (individual → system → portfolio → organization → nation → society)
4. **Environment** — context: social, legal, competitive, customer, technological
5. **Goal** — the articulated business goal
6. **Goal-measure** — testable measurement, usually with a time component
7. **Pedigree and value** — confidence level, volatility, relative importance

**Template sentence:** *"For the system being developed, `<goal-subject>` desires that `<goal-object>` achieve `<goal>` in the context of `<environment>` and will be satisfied if `<goal-measure>`."*

#### PALM (Pedigreed Attribute eLicitation Method)

A 7-step workshop method (~1.5 days):

| Step | Activity |
|------|----------|
| 1 | PALM overview presentation |
| 2 | Business drivers presentation by project management |
| 3 | Architecture drivers presentation by architect |
| 4 | Business goals elicitation — using standard categories, expressed as scenarios, consolidated, prioritized |
| 5 | Identification of potential quality attributes from business goals |
| 6 | Assignment of pedigree to existing QA drivers — which business goal does each ASR support? |
| 7 | Exercise conclusion — review, next steps, feedback |

**Uses of PALM:**
- Sniff out missing requirements early in the life cycle
- Discover and carry additional context for existing requirements (e.g., real requirement is "beat the competitor," not "half-second turnaround")
- Examine difficult QA requirements to see if they can be relaxed (many expensive requirements have no basis beyond someone's best guess)

### 16.4 Capturing ASRs in a Utility Tree

A **utility tree** begins with "utility" as root (expressing overall "goodness" of the system), then branches into quality attributes → refinements → ASRs (QA scenarios).

Each ASR is evaluated against two criteria:
- **Business value:** H (must-have), M (important but not critical), L (nice-to-have)
- **Architectural impact:** H (profound effect), M (somewhat affects), L (little effect)

**Uses of the utility tree:**
- Identify gaps: QA refinements without ASRs indicate attention needed
- Prioritize: (H,H) ASRs deserve most attention; too many may signal infeasibility
- Stakeholder review: ensure all concerns are addressed
- Alternative: organize by stakeholder roles instead of quality attributes

### 16.5 Tying the Methods Together

- If requirements process already gathers/prioritizes ASRs → use that
- If business goals un-captured → PALM
- If stakeholders overlooked → QAW
- If prioritization missing → utility tree
- Combined: PALM as subroutine from QAW; utility tree as repository for QAW output
- **Pick the approach that fills the biggest gap:** stakeholder representation, business goal manifestation, or ASR prioritization

---

## Chapter 17: Designing an Architecture (ADD)

### 17.1 Design Strategy

Three key ideas:

#### 1. Decomposition
Quality attributes refer to the system *as a whole*. To design for QA requirements, begin with the whole system and decompose. As design is decomposed, QA requirements are also decomposed and assigned to elements. Decomposition accommodates constraints (preexisting components, legacy systems).

Types of decompositions: module decomposition, C&C pattern decomposition (e.g., MVC → model, views, controllers).

#### 2. Designing to ASRs
- **Non-ASR requirements:** Three outcomes — (a) still met, (b) met with slight adjustment, (c) cannot be met. For (c): relax the requirement, reprioritize, or report inability.
- **One at a time vs. all at once:** Novices focus on one ASR at a time; experienced architects develop intuition and employ patterns to address multiple ASRs simultaneously.

#### 3. Generate and Test
Design as hypothesis generation and testing:

```
Generate → Test → (fail?) → Generate (improved) → Test → ... → Done
```

**Four key questions:**

| Question | Answer |
|----------|--------|
| Where does the initial hypothesis come from? | Existing systems, frameworks, patterns/tactics, domain decomposition, design checklists |
| What are the tests? | Analysis techniques (Ch 14), design checklists (Ch 5–11), ASRs |
| How is the next hypothesis generated? | Apply tactics to fix quality attribute problems identified by tests |
| When are you done? | ASRs satisfied OR design budget exhausted |

**Sources of initial design hypothesis (collateral):**
- **Existing systems** — most powerful; similar business context and requirements
- **Frameworks** — partial design with code; constrains architectural assumptions
- **Patterns and tactics** — cataloged solutions to common problems
- **Domain decomposition** — actors and entities; good for modifiability but not other QAs
- **Design checklists** — ensure completeness across QA concerns

**Termination:** When ASRs are satisfied or budget exhausted. Options if budget runs out: proceed with best hypothesis (relax unmet ASRs), argue for more budget, or suggest project termination if all ASRs are critical and unmet.

### 17.2 The Attribute-Driven Design (ADD) Method

ADD is an iterative method packaging the generate-and-test strategy. At each iteration it helps the architect:
- Choose a part of the system to design
- Marshal all ASRs for that part
- Create and test a design for that part

**Output:** Sketches of architectural views — not complete in every detail, but a "workable" architecture with main design approaches selected and vetted.

**Inputs to ADD:**
- ASRs (quality, functional, constraints)
- Context description: system boundaries (what's inside vs. outside), external systems/devices/users/environmental conditions

### 17.3 The Five Steps of ADD

#### Step 1: Choose an Element to Design
- **Green-field:** start with entire system
- **Existing/constrained:** pick an element not yet designed
- **Refinement strategies:**
  - **Breadth-first:** all second-level elements designed before third-level. Preferred all else equal — apportions most work to most teams soonest.
  - **Depth-first:** one downward chain completed before beginning another. Useful when important team has closing availability window, or to prototype risky/unfamiliar technology.
  - **Mixed:** defer some concerns (e.g., medium-priority availability) to later iterations; may require backtracking.

#### Step 2: Identify the ASRs for the Chosen Element
- Use utility tree: (H,H) entries are the ASRs; also pay attention to (H,M) and (M,H).
- If chosen element is not the whole system, construct a focused utility tree.

#### Step 3: Generate a Design Solution
The heart of ADD — application of generate-and-test. For each ASR, develop a solution:
- **Initial design candidate** inspired by patterns, augmented by tactics
- **Refine** using design checklists (Ch 5–11) to instantiate the pattern
- Patterns often address multiple ASRs simultaneously
- Design decisions become constraints on all future steps

#### Step 4: Verify, Refine Requirements, Generate Input for Next Iteration
A test step. Four possible dispositions for each requirement:

| Disposition | Meaning |
|-------------|---------|
| **Satisfied** | Requirement met; removed from further consideration |
| **Delegated** | Assigned to a child element for future design |
| **Distributed** | Divided among children (e.g., latency budget: 0.8s + 0.9s + 0.3s = 2.0s) |
| **Cannot be satisfied** | Backtrack (revisit design) or push back on the requirement |

**Actions for unmet ASRs:**

| Problem Type | Recommended Action |
|--------------|-------------------|
| QA requirement not met | Apply (more) tactics. Ask: Will this improve sufficiently? Use with another tactic? Tradeoffs? |
| Functional responsibility not met | Add to existing modules or create new ones. Assign to modules with similar responsibilities, QA characteristics, or timing behavior |
| Constraint not met | Modify design to accommodate, or relax the constraint. If impossible, report to project manager with justification |

#### Step 5: Repeat Steps 1–4 Until Done
**Termination conditions:**
- All requirements clearly satisfied
- Architecture sketched to sufficient depth — trust implementation team to flesh out rest (2 levels of breadth-first may suffice)
- Contractual arrangement requires legally enforceable specification → continue until that level
- Design budget exhausted

**Release early:** Don't wait until architecture is "finished." Early broad-and-shallow views are enormously helpful — managers can budget, experts can scout commercial products, support staff can build infrastructure, and early release invites early feedback.

---

## Chapter 18: Documenting Software Architectures

> *"If it is not written down, it does not exist." — Philippe Kruchten*

Creating an architecture isn't enough — it must be communicated so stakeholders can use it properly. The best architects produce good documentation because they see it as essential to producing a high-quality product predictably.

### 18.1 Uses and Audiences

Architecture documentation is both **prescriptive** (constraining future decisions) and **descriptive** (recounting decisions already made). Three fundamental uses:

1. **Education** — introducing new team members, external analysts, customers, or a new architect
2. **Communication among stakeholders** — especially the *future architect* (who may be the same person). Documentation serves as a repository of design decisions too numerous to reproduce from memory.
3. **Basis for system analysis and construction** — tells implementers what to implement; provides fodder for evaluation; incorporates models for code-generation tools

### 18.2 Notations for Architecture Documentation

| Level | Characteristics | Examples | Analysis |
|-------|----------------|----------|----------|
| **Informal** | General-purpose tools, visual conventions chosen per system; semantics in natural language | PowerPoint, whiteboard photos | Cannot be formally analyzed |
| **Semiformal** | Standardized graphical elements and construction rules; no complete semantic treatment | UML | Rudimentary syntactic analysis |
| **Formal** | Precise (usually mathematical) semantics | ADLs (Architecture Description Languages) | Full syntax + semantic analysis; automation support |

Tradeoff: more formal = more time/effort to create and understand, but less ambiguity and more analysis opportunities.

### 18.3 Views

> **Fundamental principle:** Documenting an architecture is a matter of documenting the relevant views and then adding documentation that applies to more than one view. → **Views and Beyond**

A **view** is a representation of a set of system elements and relations of a particular type. Views divide the multidimensional entity of an architecture into manageable representations.

#### Module Views

| Aspect | Description |
|--------|-------------|
| **Elements** | Modules — implementation units providing coherent responsibilities (classes, collections of classes, layers, aspects) |
| **Relations** | *Is part of* (part/whole), *depends on* (dependency), *is a* (generalization/specialization) |
| **Constraints** | Topological constraints (e.g., visibility limitations between modules) |
| **Usage** | Blueprint for code construction; change-impact analysis; planning incremental development; requirements traceability; communicating functionality; work assignments, schedules, budgets |

**Module properties to document:**
- **Name** — may reflect position in hierarchy (A.B.C)
- **Responsibilities** — establishes identity beyond name
- **Visibility of interfaces** — public vs. private submodule interfaces
- **Implementation information** — mapping to source code units, test information, management information, implementation constraints, revision history

Module views are static partitions — not typically used for runtime quality analysis (performance, reliability). For those, use C&C and allocation views.

#### Component-and-Connector (C&C) Views

| Aspect | Description |
|--------|-------------|
| **Elements** | **Components** (runtime entities: processes, objects, clients, servers, data stores) with **ports** (points of interaction) and **Connectors** (pathways of interaction: links, protocols, info flows) with **roles** (interfaces) |
| **Relations** | **Attachments** — component ports associated with connector roles to form a graph. **Interface delegation** — ports associated with internal subarchitecture ports. |
| **Constraints** | Components attach only to connectors, not directly to components; attachments only between compatible ports and roles; connectors cannot appear in isolation |
| **Usage** | Show how the system works; guide development of runtime elements; reason about runtime QAs (performance, availability) |

**C&C element properties** (depending on analysis needs):
- Reliability (failure likelihood), Performance (response time, bandwidth, latency), Resource requirements, Functionality, Security, Concurrency, Modifiability, Tier

**Connector characteristics:**
- Can be n-ary (not just binary) — e.g., publish-subscribe with multiple publishers/subscribers
- Complex connectors can be decomposed into sub-architectures of components and connectors
- Connectors embody a **protocol of interaction** — conventions about order, locus of control, error handling, timeouts

**UML for C&C Views:**
- UML components map well to C&C components (interfaces, properties, behavioral descriptions)
- UML ports map well to C&C ports; can be decorated with multiplicity
- **Simple connectors** → UML connector line (adequate for function calls, data reads)
- **Rich connectors** → UML component or annotated line connector

#### Allocation Views

| Aspect | Description |
|--------|-------------|
| **Elements** | **Software elements** (from module or C&C views) with *required* properties; **Environmental elements** (processor, disk farm, file system, development team) with *provided* properties |
| **Relations** | *Allocated to* — mapping from software element to environmental element |
| **Usage** | Reasoning about performance, availability, security, safety; distributed development and work allocation; concurrent version access; system installation |

Allocation views compare required vs. provided properties to determine if the allocation will succeed. They can be **static** (fixed allocation) or **dynamic** (allocation changes based on loading conditions).

#### Quality Views

Structural views may spread solutions for a particular QA across multiple structures. **Quality views** extract relevant pieces of structural views and package them together:

- **Security view** — components with security roles, communication channels, security data repositories, threat responses
- **Communications view** — component-to-component channels, network channels, QoS parameters, concurrency areas
- **Exception/error-handling view** — error detection, reporting, and resolution mechanisms
- **Reliability view** — replication, switchover, timing issues, transaction integrity
- **Performance view** — network traffic models, maximum latencies, etc.

### 18.4 Choosing the Views

**Minimum:** At least one module view, at least one C&C view, and for larger systems, at least one allocation view.

**Three-step method:**

| Step | Action |
|------|--------|
| **1. Build stakeholder/view table** | Stakeholders as rows, candidate views as columns. Fill each cell: none / overview / moderate detail / high detail. Candidate views = those where some stakeholder has interest. |
| **2. Combine views** | Winnow list: merge marginal views (overview-only or few stakeholders) with stronger-constituency views. |
| **3. Prioritize and stage** | Decomposition view is particularly helpful to release early (staffing, budgets, outsourcing). 80% of information often sufficient. Breadth-first approach is best. |

### 18.5 Combining Views

Because all views describe the same system, many have strong associations. **Combined views** contain elements and relations from two or more views — useful when coupling is tight.

**Views that combine naturally:**
- Various C&C views (different parts/refinements of runtime structure)
- Deployment view + SOA/communicating-processes views
- Decomposition view + work assignment, implementation, uses, or layered views

**Overlay technique:** Create an overlay combining information that would be in two separate views. Elements and relations keep types from their constituent views.

### 18.6 Building the Documentation Package

#### View Template

| Section | Content |
|---------|---------|
| **1. Primary Presentation** | Graphical (usually) or textual depiction of elements and relations. Must include a **key** explaining the notation. |
| **2. Element Catalog** | Details all elements from the primary presentation: (A) Elements and their properties, (B) Relations and their properties, (C) Element interfaces, (D) Element behavior |
| **3. Context Diagram** | How the system/portion relates to its environment — scope of the view |
| **4. Variability Guide** | How to exercise variation points in the architecture |
| **5. Rationale** | Why the design is as it is; justification of pattern choices |

#### Documentation Beyond Views

| Section | Content |
|---------|---------|
| **Document control** | Version, date, status, change history, submission procedure |
| **1. Documentation Roadmap** | Scope/summary; how documentation is organized; view overview (name, pattern, element/relation/property types, notation); how stakeholders can use it |
| **2. How a View Is Documented** | Standard view template explanation |
| **3. System Overview** | Short prose: function, users, background, constraints |
| **4. Mapping Between Views** | Tables showing element associations across views (many-to-many) |
| **5. Rationale** | Architectural decisions applying to more than one view; system-wide pattern choices |
| **6. Directory** | Index, glossary, acronym list |

### 18.7 Documenting Behavior

Behavior documentation complements structural views by describing how architecture elements interact.

**Two kinds of notations:**

#### Trace-Oriented Languages
Describe sequences of activities/interactions for specific stimuli:

| Notation | Description |
|----------|-------------|
| **Use cases** | How actors accomplish goals; textual description + UML overview diagram |
| **Sequence diagrams** | Time-ordered interactions among instances; synchronous/asynchronous messages, lifelines, execution occurrences |
| **Communication diagrams** | Graph of interacting elements with numbered order; useful for verifying functional requirements |
| **Activity diagrams** | Flow-chart-like; conditional branching, concurrency (fork/join), event sending/receiving |

#### Comprehensive Languages
Show *complete* behavior — all possible paths from initial to final state:

| Notation | Description |
|----------|-------------|
| **UML state machine diagrams** | States (boxes), transitions (arrows), events, guard conditions, actions/effects; entry/exit actions |
| **AADL** | Architecture Analysis and Design Language — runtime behavior reasoning |
| **SDL** | Specification and Description Language — telecom systems |

### 18.8 Architecture Documentation and Quality Attributes

Five major ways QAs show up in documentation:

1. **Design approach rationale** — patterns have QA properties (client-server → scalability, layering → portability, information hiding → modifiability)
2. **Element interface QA bounds** — service-level agreements, performance/security/reliability properties
3. **QA "language"** — security → audit trails, firewalls; performance → buffer capacities, deadlines; availability → MTBF, failover, redundancy
4. **Requirements mapping** — trace QA requirements to where they are satisfied in the architecture
5. **Documentation roadmap** — tells each stakeholder where to find what they care about

### 18.9 Documenting Architectures That Change Faster Than You Can Document Them

For highly dynamic systems (runtime reconfiguration, daily redeployment):

- **Document invariants** — what is true about all versions. May be more of a constraint/guideline description than a traditional architecture document.
- **Document how the architecture is allowed to change** — captured in the **variability guide** (Section 4 of the view template).

### 18.10 Documenting Architecture in an Agile Development Project

Key principles aligning Views and Beyond with Agile:

- **"Write for the reader"** — if no audience, don't document
- Produce a view **only if** it addresses explicitly identified stakeholder concerns
- Produce documentation in **prioritized stages** — satisfy stakeholders who need it now
- **Adopt a template** to capture design decisions
- Fill sections **when information becomes available**, in any order — only if it makes downstream work easier/cheaper
- Don't separate "architectural design document" from "detailed design document" — produce just enough to move on to code
- Don't feel obliged to fill all sections; "N/A" is acceptable
- Primary presentation may be a **digital photo of a whiteboard**; verbal communication of catalog, rationale, etc. is fine initially; template provides place to record later
- Use lightweight, easily changeable formats — **wikis** are ideal

---

## Key Concepts Summary

| Concept | Chapter | Core Idea |
|---------|---------|-----------|
| **ASR** | 16 | Architecturally Significant Requirement — must have high architectural impact AND high business value |
| **QAW** | 16 | 8-step workshop to generate, prioritize, and refine QA scenarios |
| **PALM** | 16 | 7-step method to elicit business goals and link them to ASRs |
| **Utility Tree** | 16 | Hierarchical organization of QA requirements: Utility → QA → Refinement → Scenario (with H/M/L ratings) |
| **ADD** | 17 | 5-step iterative method: Choose element → Identify ASRs → Generate design → Verify/refine → Repeat |
| **Generate and Test** | 17 | Design as hypothesis: generate from collateral, test against ASRs, improve with tactics |
| **Module View** | 18 | Implementation units, static structure; blueprint for construction |
| **C&C View** | 18 | Runtime components and connectors; reasoning about dynamic QAs |
| **Allocation View** | 18 | Mapping software to environment (hardware, file systems, teams) |
| **Quality View** | 18 | Cross-cutting extraction of structural views for specific QA concerns |
| **View Template** | 18 | Primary Presentation → Element Catalog → Context Diagram → Variability Guide → Rationale |
| **Documentation Beyond Views** | 18 | Roadmap, system overview, view mappings, cross-view rationale, directory |

---

## Related Notes
- [[Software Architecture Overview]] — SWEBOK v4 overview
- Earlier SAiP chapters: Quality Attributes (Ch 4–11), Patterns & Tactics (Ch 13), Analysis (Ch 14)
