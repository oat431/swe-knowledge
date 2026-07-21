---
tags: [architecture, fundamentals, structures, views, software-architecture]
source: "Bass, Clements & Kazman, Software Architecture in Practice, Chapters 1–3"
created: 2026-07-21
---

# Architecture Fundamentals

> *"The software architecture of a system is the set of structures needed to reason about the system, which comprise software elements, relations among them, and properties of both."*
> — Bass, Clements & Kazman, SAiP

---

## 1. What Is Software Architecture?

### 1.1 The Definition and Its Implications

The SAiP definition deliberately avoids phrases like "early design decisions" or "major decisions." Many architectural decisions are made early — but not all (especially in Agile projects). Many early decisions are *not* architectural. Structures, by contrast, are easy to identify and form a powerful tool for system design.

Key implications:

| Implication | Meaning |
|---|---|
| **Architecture is a set of structures** | A structure is a set of elements held together by a relation. No single structure is *the* architecture. |
| **Architecture is an abstraction** | Omits information that has no ramifications outside a single element. Concerned with the public side of interfaces — not internal implementation details. |
| **Every system has an architecture** | Even a trivial single-element system has one. The architecture may exist without being known or documented. |
| **Architecture includes behavior** | Box-and-line drawings are NOT architectures. Element behavior that influences other elements or system acceptability is architectural. |
| **Not all architectures are good** | The definition is indifferent to quality. An architecture may permit or preclude achieving requirements. |

### 1.2 System Architecture vs Enterprise Architecture vs Software Architecture

| Discipline | Scope |
|---|---|
| **System Architecture** | Total system: hardware + software + humans. Maps functionality onto hardware/software components, concerned with power, weight, physical footprint. |
| **Enterprise Architecture** | Organization's processes, information flow, personnel, subunits — aligned with core goals. Specifies data models, inter-system interaction rules, standards. |
| **Software Architecture** | Focuses on the software elements, their structures, and how those structures deliver quality attributes. Must live within system and enterprise constraints. |

All three share commonalities: they can be designed, evaluated, documented; answer to requirements; satisfy stakeholders; consist of structures of elements and relationships; have patterns and styles at their disposal.

---

## 2. Architectural Structures and Views

### 2.1 Structure vs View

> **Structure** = the set of elements itself, as they exist in software or hardware.
> **View** = a representation of a structure, documented according to a template, for specific stakeholders.

Architects **design structures**. They **document views** of those structures.

### 2.2 Three Categories of Structures

```
┌─────────────────────────────────────────────────────────────────┐
│                    ARCHITECTURAL STRUCTURES                      │
├───────────────────┬───────────────────┬─────────────────────────┤
│     MODULE        │   C&C (Runtime)   │      ALLOCATION         │
│   (Static)        │    (Dynamic)      │   (Mapping to env)      │
├───────────────────┼───────────────────┼─────────────────────────┤
│ • Decomposition   │ • Service         │ • Deployment            │
│ • Uses            │ • Concurrency     │ • Implementation        │
│ • Layer           │                   │ • Work Assignment       │
│ • Class           │                   │                         │
│ • Data Model      │                   │                         │
└───────────────────┴───────────────────┴─────────────────────────┘
```

#### Module Structures (Static — code/data units)

| Structure | Elements | Relation | Used For |
|---|---|---|---|
| **Decomposition** | Modules | is-a-submodule-of | Resource allocation, project structuring, modifiability, information hiding |
| **Uses** | Modules/classes | uses (requires correct presence of) | Engineering subsets/extensions, incremental development |
| **Layer** | Layers | requires services of, provides abstraction to | Portability, incremental development on "virtual machines" |
| **Class (Generalization)** | Classes, objects | inherits-from, is-instance-of | Reuse, incremental addition of functionality |
| **Data Model** | Data entities | {one,many}-to-{one,many}, generalizes, specializes | Global data structure consistency, performance |

#### Component-and-Connector Structures (Runtime — dynamic behavior)

| Structure | Elements | Relation | Used For |
|---|---|---|---|
| **Service** | Services, ESB, registry | Runs concurrently with, may precede/exclude | Interoperability, scheduling analysis |
| **Concurrency** | Processes, threads | Can run in parallel, forks, joins | Identifying resource contention, parallelism opportunities |

**Key distinction:** In C&C structures, a *component* is always a runtime entity. Modules (from module structures) are compiled into components. The mapping is many-to-many: one module may manifest as multiple runtime components, and one component may draw from multiple modules.

#### Allocation Structures (Mapping software to environment)

| Structure | Elements | Relation | Used For |
|---|---|---|---|
| **Deployment** | Components, hardware elements | allocated-to, migrates-to | Performance, security, availability |
| **Implementation** | Modules, file structure | stored-in | Configuration control, integration, build |
| **Work Assignment** | Modules, organizational units | assigned-to | Project management, expertise allocation, commonality management |

### 2.3 Relating Structures to Each Other

Structures are not independent. A module in a decomposition structure may manifest as one, part of one, or several components in a C&C structure. Mappings between structures are many-to-many.

**Example:** A client-server system may have only 2 modules (Client, Server) but 11 runtime components (10 client instances + 1 server) connected by 10 connectors. The decomposition view is used for development organization; the C&C view is used for performance analysis.

### 2.4 How Many Structures? Which Ones?

- **Larger systems** → more dramatic differences between structures → more structures warranted.
- **Smaller systems** → fewer structures needed (single process → no concurrency structure; single processor → trivial deployment structure).
- **Rule:** Design and document a structure only if doing so brings positive ROI — usually in decreased development or maintenance costs.
- **Selection heuristic:** Choose structures that provide insight and leverage into the system's most important quality attributes.

---

## 3. Architectural Patterns

Architectural patterns are packaged strategies — compositions of elements found useful over time across many domains.

### Common Module-Type Patterns
- **Layered Pattern:** Strictly unidirectional uses relation. Each layer is a coherent set of related functionality. In strictly layered systems, a layer can only use the layer immediately below. Engenders portability by hiding implementation specifics as virtual machines.

### Common C&C-Type Patterns
- **Shared-Data (Repository) Pattern:** Components and connectors that create, store, and access persistent data. Repository = typically a database; connectors = protocols like SQL.
- **Client-Server Pattern:** Components = clients and servers; connectors = protocols and messages.

### Common Allocation Patterns
- **Multi-tier Pattern:** Distributes components across distinct subsets of hardware/software, connected by communication media.
- **Competence Center:** Work allocated to sites based on technical/domain expertise at each site.
- **Platform:** One site develops reusable core assets; other sites build applications on top.

---

## 4. What Makes a "Good" Architecture?

No architecture is *inherently* good or bad — it is fit or unfit for a specific purpose. Evaluation is only meaningful in the context of stated goals.

### Process Recommendations

1. **Single architect or small team with a technical leader** — ensures conceptual integrity and technical consistency (applies to Agile and open source too).
2. **Base the architecture on a prioritized list of quality attribute requirements** — these inform tradeoffs. Functionality matters less.
3. **Document using views** — address concerns of the most important stakeholders. Minimal documentation at first, elaborated later.
4. **Evaluate early and repeatedly** — for ability to deliver quality attributes.
5. **Lend itself to incremental implementation** — build a skeletal system first, grow incrementally, refactor as needed.

### Structural Recommendations

1. **Well-defined modules** — functional responsibilities assigned via information hiding and separation of concerns. Encapsulate things likely to change. Each module should have a well-defined interface enabling independent team development.
2. **Use well-known patterns and tactics** — unless requirements are truly unprecedented.
3. **Never depend on a particular version of a commercial product** — structure to make version/platform changes inexpensive.
4. **Separate data producers from data consumers** — increases modifiability; changes are often confined to one side.
5. **Don't expect 1:1 module-to-component mapping** — concurrency means multiple component instances from one module; multiple threads may use services from several components.
6. **Processes should be easily reassignable to processors** — perhaps at runtime.
7. **Small number of interaction mechanisms** — do the same things the same way throughout. Aids understandability, reliability, modifiability.
8. **Specific, small set of resource contention areas** — with clearly specified and maintained resolution (e.g., network traffic guidelines, time budgets for threads).

---

## 5. Why Software Architecture Matters: The List of Thirteen

### 5.1 Inhibiting or Enabling Quality Attributes

Whether a system exhibits its desired quality attributes is substantially determined by its architecture:

| Quality Attribute | Architectural Concern |
|---|---|
| **Performance** | Time-based behavior of elements, shared resource usage, frequency/volume of inter-element communication |
| **Modifiability** | Assign responsibilities so most changes affect few elements (ideally one) |
| **Security** | Manage/protect inter-element communication; control which elements access which information; introduce authorization mechanisms |
| **Scalability** | Localize resource usage; avoid hard-coding resource assumptions or limits |
| **Availability** | How components take over for each other on failure; system fault response |
| **Usability** | Isolate UI and UX elements from the rest of the system for independent evolution |
| **Testability** | Make element state observable and controllable; understand emergent behavior |
| **Safety** | Behavioral envelope of elements and emergent concert behavior |
| **Interoperability** | Identify elements responsible for external interactions |
| **Incremental Subsets** | Carefully manage inter-component usage |
| **Reusability** | Restrict inter-element coupling so extraction doesn't pull too many dependencies |

> *The architecture giveth and the implementation taketh away.* A good architecture is necessary but not sufficient. Quality must be considered throughout design, implementation, and deployment.

### 5.2 Reasoning About and Managing Change

~80% of a typical software system's total cost occurs after initial deployment. Architecture partitions changes into three categories:

| Change Type | Scope | Example |
|---|---|---|
| **Local** | Modifying a single element | Adding a new business rule to pricing logic |
| **Nonlocal** | Multiple elements, but architectural approach intact | Adding rule + DB fields + UI changes |
| **Architectural** | Fundamental changes to element interaction — changes everywhere | Client-server → peer-to-peer |

An effective architecture makes the *most common* changes *local*.

### 5.3 Predicting System Qualities

It *is* possible to make quality predictions based solely on architecture evaluation. If certain architectural decisions lead to certain quality attributes, making those decisions lets us expect those qualities. The earlier a problem is found, the cheaper it is to fix.

### 5.4 Enhancing Communication Among Stakeholders

Architecture provides a **common language** at an intellectually manageable level. Different stakeholders have different concerns:

- **User:** Fast, reliable, available when needed
- **Customer:** Implementable on schedule and budget
- **Manager:** Teams can work independently, interacting in disciplined ways
- **Architect:** Strategies to achieve all those goals

Architecture — even a box-and-line diagram — serves as a vehicle for surfacing requirements questions that might otherwise go unasked.

### 5.5 Carrying Early Design Decisions

Architecture embodies the earliest, most consequential design decisions. Examples:

- Single processor or distributed?
- Layered? How many layers? What does each do?
- Synchronous or asynchronous communication?
- OS/hardware dependencies?
- Encryption?
- Which OS? Which communication protocol?

Changing these early decisions causes a ripple effect — sometimes a tsunami. Seven categories of early design decisions are elaborated in Chapter 4.

### 5.6 Defining Constraints on Implementation

Implementers must be fluent in their element's specifications but need not know all architectural tradeoffs. The architect constrains them to meet the tradeoffs. Example: assigning performance budgets to individual software units.

### 5.7 Influencing Organizational Structure

The architecture's broadest decomposition becomes the **work-breakdown structure**, which dictates: planning, scheduling, budgeting, inter-team communication, configuration control, file-system organization, integration/test plans.

> **Conway's Law:** *"Organizations which design systems are constrained to produce designs which are copies of the communication structures of these organizations."*

Once architecture is agreed upon, it becomes very costly — for managerial and business reasons — to significantly modify it.

### 5.8 Enabling Evolutionary Prototyping

A **skeletal system** builds the infrastructure (initialization, communication, data sharing, resource access, error reporting, logging) before full functionality. This makes the system executable early, allowing performance problems to be identified early in the life cycle.

### 5.9 Improving Cost and Schedule Estimates

Bottom-up estimates based on architectural decomposition are typically more accurate than top-down estimates. Best estimates emerge from consensus between top-down (architect + PM) and bottom-up (developers).

### 5.10 Supplying a Transferable, Reusable Model

Reuse of architectures provides greater leverage than code reuse. A **software product line** uses a common architecture as its core asset, defining what is fixed and what is variable across family members. Order-of-magnitude payoffs in time-to-market, cost, productivity, and quality.

### 5.11 Allowing Incorporation of Independently Developed Components

Architecture-based development focuses on **composition** of elements developed separately. Like Eli Whitney's interchangeable musket parts, the architecture defines interfaces for fit and function. Payoffs: decreased time to market, increased reliability, lower cost, flexibility.

### 5.12 Restricting the Vocabulary of Design Alternatives

By restricting ourselves to a relatively small number of proven element/interaction choices, we minimize design complexity. Engineering requires discipline — patterns guide the architect by restricting alternatives to proven solutions.

### 5.13 Providing a Basis for Training

Architecture serves as the first introduction to the system for new team members. Module views show project structure and team assignments. C&C views explain how the system works.

---

## 6. The Many Contexts of Software Architecture

Architectures exist in four interconnected contexts:

```
                  ┌──────────────┐
                  │  Technical   │
                  │   Context    │
                  └──────┬───────┘
                         │
         ┌───────────────┼───────────────┐
         │               │               │
   ┌─────▼─────┐  ┌──────▼──────┐  ┌─────▼─────┐
   │  Project  │  │  Business   │  │Professional│
   │ Life-Cycle│  │   Context   │  │  Context   │
   └───────────┘  └─────────────┘  └───────────┘
```

### 6.1 Technical Context

**Quality Attributes:** Architecture inhibits or enables achievement of quality attributes. Nothing else influences an architecture more than the quality attribute requirements it must satisfy.

> *Why is functionality missing?* Architecture mainly provides containers into which functionality is placed. Functionality is a consequence of architecture, not a primary driver.

**Technical Environment:** The current technical environment influences architecture (e.g., today: web-based, cloud-based, service-oriented, mobile-aware). What's current now won't be in ten years.

**The Vasa Ship Story (1628):** A cautionary tale — the Vasa was the world's most formidable warship on paper, but its architect (Henrik Hybertsson) couldn't balance conflicting constraints given the technical environment of his day. On its maiden voyage, it sailed ~1,300 meters, fired a salute, and sank. The inquiry concluded: "well built but badly proportioned" — its architecture was flawed.

### 6.2 Project Life-Cycle Context

Four dominant development processes:

| Process | Characteristics |
|---|---|
| **Waterfall** | Sequential: requirements → design → implementation → integration → testing → installation → maintenance. Feedback paths allow revision of earlier artifacts. |
| **Iterative** (e.g., Unified Process) | Short cycles through all steps. Each iteration delivers something working. Phases: inception, elaboration, construction, transition. Addresses greatest risks first. |
| **Agile** (Scrum, XP, Crystal) | Early/frequent delivery, close customer collaboration, self-organizing teams, adaptation to change. Eschews substantial up-front work. Architecture and Agile *can* coexist (see Chapter 15). |
| **Model-Driven Development** | Humans create platform-independent models (PIM); combine with platform-definition model (PDM) to auto-generate code. |

Regardless of process, these architecture activities apply:

1. **Making a business case** — Justifying organizational investment. Architects must contribute to cost, market, interface, and limitation questions.
2. **Understanding architecturally significant requirements** — Elicitation via use cases, scenarios, quality attribute scenarios, prototypes, prior system analysis.
3. **Creating or selecting the architecture** — Conceptual integrity requires a small number of minds (Brooks, *The Mythical Man-Month*).
4. **Documenting and communicating** — Must be informative, unambiguous, readable by varied stakeholders. Minimal but sufficient.
5. **Analyzing or evaluating** — Choosing among competing designs rationally. Scenario-based techniques; ATAM (Architecture Tradeoff Analysis Method).
6. **Implementing and testing based on the architecture** — Keeping developers faithful to the prescribed structures and interaction protocols.
7. **Ensuring implementation conformance** — Vigilance in maintenance; recovering architecture from existing systems.

### 6.3 Business Context

**Business Goals → Quality Attributes → Architecture.** Every quality attribute requirement should originate from some higher business purpose (e.g., fast response time → differentiate from competition → capture market share).

Not all business goals become quality attribute requirements:
- Some directly dictate architectural decisions (e.g., "include a database" because the DB department is overstaffed).
- Some have no architectural effect (e.g., reducing office paper usage).

**Development Organization Influence:**
- Staff expertise shapes architectural choices (abundance of peer-to-peer experts → peer-to-peer architecture).
- Organizational investment in existing assets and architectures drives reuse assumptions.
- Long-term strategic goals (e.g., reputation for cloud-based solutions) drive infrastructural investments.
- Organizational structure shapes architecture — and vice versa.

### 6.4 Professional Context

The architect's triad:

```
         ┌──────────┐
         │  Duties  │
         └────┬─────┘
              │
    ┌─────────┼─────────┐
    │         │         │
┌───▼───┐ ┌───▼───┐ ┌───▼───┐
│Skills │ │Knowledge│ │  ...  │
└───────┘ └────────┘ └───────┘
```

Architects need more than technical skills:
- **Diplomatic, negotiation, and communication skills** — to explain tradeoff priorities to stakeholders whose expectations aren't fully met.
- **Leadership** — in the eyes of developers and management.
- **Business knowledge** — understanding the organizational context.
- **Context-switching ability** — managing a diverse workload.

**Background and experience** shape architectural choices. Good experiences with an approach → reuse it. Bad experiences → avoid it. Architects must critically examine whether proposed solutions are simply the path of least resistance. Proactive management: training, exploratory projects, critical examination.

### 6.5 Stakeholders

A **stakeholder** is anyone with a stake in the system's success. Each has different concerns — some contradictory.

| Stakeholder | Primary Interest in Architecture |
|---|---|
| **Analyst** | Satisfaction of quality attribute requirements |
| **Architect** | Negotiating tradeoffs; recording design decisions; providing evidence |
| **Business Manager** | Ability to meet business goals |
| **Conformance Checker** | Faithfulness of implementation to architectural prescriptions |
| **Customer** | Required functionality and quality; progress gauging; cost estimation |
| **Database Administrator** | Data creation, usage, updates; data properties for quality goals |
| **Deployer** | Elements to be installed; their responsibility toward system function |
| **Designer** | Resource contention resolution; how their part communicates with others |
| **Evaluator** | Architecture's ability to deliver required behavior and quality |
| **Implementer** | Inviolable constraints and exploitable freedoms |
| **Integrator** | Integration plans; source of integration failures |
| **Maintainer** | Ramifications of changes |
| **Network Administrator** | Network loads; usage patterns |
| **Product-Line Manager** | Whether a new member is in/out of scope |
| **Project Manager** | Budget/schedule setting; progress gauging; resource contention |
| **External Systems Rep** | Inter-system agreements |
| **System Engineer** | Sufficiency of environment provided for software |
| **Tester** | Tests based on behavior and interaction of elements |
| **User** | Whether desired functionality is delivered; system understanding for field maintenance |

> **Key advice:** *Know your stakeholders. Talk to them, engage them, listen to them, and put yourself in their shoes.*

---

## 7. The Architecture Influence Cycle (AIC)

Architectures are not created in a vacuum, nor do they exist in one.

```
    Architect's Influences
    ┌──────────────────────────────────────┐
    │                                      │
    ▼                                      │
┌─────────┐   ┌──────────────┐   ┌─────────┐
│Business │──▶│ Architecture │──▶│ System  │
│Technical│   │              │   │         │
│Project  │   │              │   │         │
│Profess. │   │              │   │         │
└─────────┘   └──────────────┘   └─────────┘
    ▲                                      │
    │          FEEDBACK                    │
    └──────────────────────────────────────┘
```

**Forward influences** (contexts → architecture):
- Business goals shape quality requirements
- Technical environment constrains viable approaches
- Project life-cycle process determines when/how architecture activities occur
- Architect's professional background colors design choices

**Feedback influences** (architecture → contexts):
- **Technical:** Customers may relax requirements to gain economies of an existing architecture; product lines influence future requirements.
- **Project:** Architecture prescribes team structure → teams become embedded in the organization → teams influence future decompositions.
- **Business:** Successful architecture enables market foothold (iPhone/Android platforms); organization adjusts goals to exploit newfound expertise.
- **Professional:** Successful approaches reinforce themselves; failed approaches are avoided.

---

## 8. Key Lessons from the "Ask Cal" Story

At an architecture evaluation, every tough question was answered with *"Ask Cal. Cal knows that."* Cal was the lead architect — and the sole repository of architectural knowledge.

**Lesson:** An architecture that is not documented, and not communicated, may still be a good architecture — but the risks surrounding it are enormous. If Cal gets hit by a bus or leaves the company, the architectural knowledge is gone.

The evaluation's unexpected success: it forced the team to produce respectable architecture documentation for the first time.

---

## Summary

| Concept | Core Idea |
|---|---|
| **Architecture** | Set of structures (elements + relations + properties) needed to reason about the system |
| **Structure** | Elements and relations, as they exist in software/hardware |
| **View** | Representation of a structure for specific stakeholders |
| **3 Structure Categories** | Module (static, code units), C&C (runtime, dynamic), Allocation (mapping to environment) |
| **13 Reasons Architecture Matters** | From quality attributes through training — architecture is the central artifact of system development |
| **4 Contexts** | Technical, Project Life-Cycle, Business, Professional — all influence and are influenced by architecture |
| **AIC** | Architecture Influence Cycle — feedback loops between contexts, architecture, and the resulting system |
| **Stakeholders** | ~18 distinct roles, each with specific architectural interests — know them all |
| **Good Architecture** | Fit for purpose; evaluated against specific goals; follows process and structural rules of thumb |


## Related

- [[Software Architecture Overview]] — All architecture topics
- [[02_Quality_Attributes_Overview]] — Quality attribute framework
- [[06_Tactics_and_Patterns]] — Tactics and pattern catalog
- [[07_Design_and_Documentation]] — Designing and documenting architectures
