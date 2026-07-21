---
tags:
  - architecture
  - evaluation
  - atam
  - governance
  - software-architecture
  - testing
  - implementation
  - conformance
  - management
source: "Bass, Clements & Kazman, Software Architecture in Practice (SAiP), Chapters 19–22"
created: 2026-07-21
---

# Architecture Evaluation & Governance

## 1. Architecture and Implementation (Ch 19)

### 1.1 Code Templates

Code templates bridge the gap between architecture and implementation. They encode architectural patterns as reusable code structures that developers fill in.

- **Example**: A failover protocol template — a continuous loop that services incoming events (normal processing, state data updates, image requests, switch directives, reconstitution requests)
- **Benefits**: Simplifies adding new applications; coders need not understand fault-tolerance mechanisms; once debugged, entire classes of errors disappear

### 1.2 Keeping Code and Architecture Consistent

Code drifts from architecture in three common ways:
1. No constraints imposed on coders to follow the architecture
2. Architecture abandoned when problems are encountered
3. After fielding, changes made only to code without updating architecture

**Sync strategies**:

| Strategy | Description |
|----------|-------------|
| Sync at life-cycle milestone | Update architecture at end of phase/release/iteration |
| Sync at crisis | Undesirable; update when technical quagmire demands it |
| Sync at check-in | Automated rules vet every check-in; flag when code "breaks" architecture |

**Practical advice**: Mark outdated sections as "no longer applicable" or "to be revised" rather than treating architecture as all-or-nothing.

### 1.3 The Architecture–Implementation Ontology Gap

| Architects think in terms of | Implementers think in terms of |
|------------------------------|-------------------------------|
| Modules, components, connectors | Objects, methods, algorithms |
| Stakeholders, evaluation, analysis | Data structures, variables, debugging |
| Documentation, views, modeling | Code comments, compilers, build scripts |
| Quality attributes, business goals | Pointers, generics, operator overloading |

- **UML limitations**: No built-in concept for layers; connectors are impoverished (delegation and assembly only); architectural connectors like ESBs require workarounds
- **Frameworks and interfaces** live on the cusp of both domains — they provide hope for unification

### 1.4 Architecture and Testing

#### Levels of Testing

| Level | Description | Architecture's Role |
|-------|-------------|---------------------|
| **Unit Testing** | Tests individual pieces in isolation | Defines the units (module view elements); assigns responsibilities |
| **Integration Testing** | Tests interactions between units | Determines integration increments (uses view); defines interfaces; runtime QA testing happens here |
| **System Testing** | All elements in intended environment | End-to-end quality attribute validation |
| **Acceptance Testing** | By users in deployment setting | Guides stress testing of QA behavior (security attacks, heavy loads, resource deprivation) |
| **Regression Testing** | Re-running tests after changes | Ensures architecture hasn't regressed |

#### Black-Box, White-Box, and Gray-Box Testing

| Type | Approach | Architecture Connection |
|------|----------|------------------------|
| **Black-box** | No knowledge of internals; requirements only | Architecture describes element requirements and interfaces |
| **White-box** | Full knowledge of internals (control paths, algorithms) | Can test component interactions exploiting internal knowledge |
| **Gray-box** | Partial internal knowledge | Tests interactions between components |

#### Risk-Based Testing

Concentrate testing effort where risk is highest:
- Immature technologies
- Requirements uncertainty
- Developer experience gaps
- Architecturally significant requirements (if unmet, system is unacceptable by definition)

#### Test Activities

1. **Test planning** — Allocate resources (time, labor, technology)
2. **Test development** — Write test procedures, choose cases, create datasets
3. **Test execution** — Apply tests, capture errors
4. **Test reporting and defect analysis** — Report to developers and management; adjudicate disposition of faults
5. **Test harness creation** — Architect builds harnesses for convenient element testing

#### Architect's Role in Testing

- Design for **testability** (apply testability tactics from Ch 10)
- Give testers access to source code, design documents, change records
- Provide ability to control/reset persistent datasets (e.g., design a "persistence layer" for database independence)
- Enable installation of multiple product versions on a single machine
- Work with test team to establish testability scenarios

---

## 2. Architecture Reconstruction and Conformance (Ch 20)

> *"Architecture reconstruction is the process of reverse-engineering the as-built architecture from existing system artifacts."*

### 2.1 The Reconstruction Process (4 Phases)

```
Raw View Extraction  →  Database Construction  →  View Fusion & Manipulation  →  Architecture Analysis
      ↑                                                                                    |
      └────────────────────────────── iterative ───────────────────────────────────────────┘
```

#### Phase 1: Raw View Extraction
Extract raw information from:
- **Source code** (static analysis)
- **Execution traces** (dynamic analysis)
- **Build scripts**, configuration files, design models

**Static vs. Dynamic Analysis**:
| Static | Dynamic |
|--------|---------|
| Observing system artifacts only | Observing how the system runs |
| Lexical analyzers, parsers, AST analyzers | Profilers, code instrumentation, aspects |
| May miss dynamically bound calls (polymorphism, function pointers, plugins) | Captures actual runtime behavior but may miss rarely executed paths |

**Extraction tools**: Parsers, AST analyzers, lexical analyzers (grep, Perl), profilers, code instrumentation, aspects

#### Phase 2: Database Construction
Convert extracted information into a standardized form and populate a reconstruction database.

#### Phase 3: View Fusion and Manipulation
Combine multiple extracted views to improve accuracy:
- Fuse static and dynamic call graphs (static misses late binding; dynamic misses rarely executed paths)
- Create hypothesized architectural views (layers, vertical slices)
- Expert interpretation: aggregate elements into layers, components, etc.
- **Tools**: SonarJ, Lattix, Structure101

#### Phase 4: Architecture Analysis — Finding Violations
Test hypotheses about the architecture. Two approaches to maintaining conformance:

| Approach | Description | Limitations |
|----------|-------------|-------------|
| **Conformance by construction** | Auto-generate system from architectural specification | Limited applicability; requires specific tools/languages |
| **Conformance by analysis** | Reverse-engineer and flag nonconforming elements | Mismatch between static code structures and runtime structures |

- Static analysis finds module structure violations (e.g., illegal layer dependencies)
- Dynamic analysis finds C&C structure violations (e.g., forbidden database writes bypassing entity beans)

### 2.2 Guidelines for Reconstruction

1. **Have a goal** and set of objectives before starting
2. **Obtain a coarse representation** first (identify layers as starting point)
3. **Existing documentation may be inaccurate** — use only for high-level concepts
4. **Tools shorten the process but cannot automate entirely** — involve people familiar with the system

### 2.3 Uses of Reconstruction Results

- Basis for architecture documentation (when none exists or is outdated)
- Conformance checking against as-designed architecture
- Starting point for reengineering to a new architecture
- Identify elements for reuse or product line establishment

---

## 3. Architecture Evaluation — ATAM (Ch 21)

> *"If a system is important enough for you to explicitly design its architecture, then that architecture should be evaluated."*

### 3.1 Evaluation Factors

**Three evaluation formats**:
| Format | Description | When to Use |
|--------|-------------|-------------|
| **Evaluation by the designer within the design process** | Self-analysis during decision-making | Continuously, for every important decision |
| **Peer review** | Colleagues review the architecture | At any point where a reviewable portion exists |
| **Analysis by outsiders** | External evaluators provide objective assessment | For complete architectures; expensive but authoritative |

**Contextual factors**: Available artifacts, who sees results (public/private), evaluator skills, stakeholder participation, business goal clarity

### 3.2 ATAM — Architecture Tradeoff Analysis Method

#### Participants (Three Groups)

| Group | Composition | Role |
|-------|-------------|------|
| **Evaluation Team** | 3–5 external people; team leader, evaluation leader, scenario scribe, proceedings scribe, questioner | Conduct the evaluation; must be unbiased |
| **Project Decision Makers** | Project manager, customer, architect (mandatory) | Empowered to mandate changes |
| **Stakeholders** | Developers, testers, integrators, maintainers, users, performance engineers (~12–15 people) | Articulate quality attribute goals |

#### Outputs

1. Concise architecture presentation (1 hour)
2. Articulated business goals
3. **Prioritized quality attribute scenarios** (often the most valued output)
4. **Risks and nonrisks** — architectural decisions that may (or may not) lead to undesirable consequences
5. **Risk themes** — overarching systemic weaknesses
6. Mapping of architectural decisions to quality requirements
7. **Sensitivity points and tradeoff points** — decisions with marked effect on quality attributes

#### The Four Phases

| Phase | Activity | Participants | Duration |
|-------|----------|--------------|----------|
| **0: Partnership & Preparation** | Logistics, team formation, architecture review | Evaluation leadership + key decision makers | Several weeks (informal) |
| **1: Evaluation** | Steps 1–6 (decision makers only) | Evaluation team + project decision makers | 1–2 days |
| **2: Evaluation (continued)** | Steps 1–6 recap + Steps 7–9 (with stakeholders) | Evaluation team + decision makers + stakeholders | 2 days |
| **3: Follow-up** | Written final report | Evaluation team + evaluation client | ~1 week |

#### The Nine Steps

**Phase 1 (Steps 1–6)** — with decision makers:

| Step | Activity | Key Output |
|------|----------|------------|
| 1 | Present the ATAM method | Shared understanding of process |
| 2 | Present business drivers | System functions, constraints, business goals, stakeholders, architectural drivers |
| 3 | Present the architecture | Views (context, module/layer, C&C, deployment), patterns/tactics, scenario traces |
| 4 | Identify architectural approaches | Catalog of patterns and tactics used |
| 5 | Generate quality attribute utility tree | Prioritized, refined QA scenarios as tree leaves |
| 6 | Analyze architectural approaches | Risks, nonrisks, sensitivity points, tradeoffs for high-priority scenarios |

**Phase 2 (Steps 7–9)** — with all stakeholders:

| Step | Activity | Key Output |
|------|----------|------------|
| 7 | Brainstorm and prioritize scenarios | Stakeholder-generated scenarios; voting (30% of total scenario count) |
| 8 | Analyze architectural approaches | Same as Step 6, applied to top 5–10 stakeholder scenarios |
| 9 | Present results | Risk themes linked to business drivers; all findings summarized |

#### Key Concepts

- **Risk**: An architectural decision that may lead to undesirable consequences
- **Nonrisk**: A decision deemed safe upon analysis
- **Sensitivity point**: A decision where small changes have marked effect on a quality attribute (e.g., heartbeat frequency → fault detection time)
- **Tradeoff point**: A decision that affects multiple quality attributes in opposing directions (e.g., higher heartbeat frequency → better availability but worse performance)

### 3.3 Lightweight Architecture Evaluation

For smaller, less risky projects:
- **Duration**: 4–6 hours (single afternoon)
- **Participants**: Internal to organization; fewer people
- **Steps**: Streamlined — some steps omitted (brainstorming, separate analysis); utility tree reused if existing
- **Output**: No formal report; scribe captures results
- **Tradeoff**: Less depth and objectivity, but inexpensive and low ceremony

---

## 4. Management and Governance (Ch 22)

> *"How does a project get to be a year behind schedule? One day at a time."* — Fred Brooks

### 4.1 Planning

**Dual scheduling approach**:

```
Top-Down Schedule  →  First-Level Decomposition  →  Bottom-Up Schedule  →  Reconciliation  →  Software Development Plan
```

- **Top-down**: Initial estimate for management buy-in (inherently inaccurate)
- **Bottom-up**: Emerges from architecture design; team leads provide estimates for their pieces
- **Rules of thumb** (medium ~150 KSLOC projects): ~150 components, ~4 hrs/component paper design, 8 weeks between releases, 40% design / 20% coding / 40% testing

**Budgeting models**:
- **Top-down planning**: Budget covers entire project including schedule
- **Architecture-first**: Budget only for architecture design phase; full project budget emerges after

### 4.2 Organizing

#### Team Structure
- Architecture team members become leads for implementation teams (distributed ownership)
- Support functions (documentation, testing, QA, CM) in **matrix organization** form
- **Typical roles**: Team leader, developer, configuration manager, system test manager, product manager

#### Project Manager vs. Software Architect

| Domain | Project Manager | Software Architect |
|--------|-----------------|-------------------|
| **Integration** | Organize project, manage budget/schedule | Create design, organize team around design |
| **Scope** | Negotiate scope with marketing | Elicit/review runtime requirements; estimate cost/schedule/risk |
| **Time** | Oversee progress against schedule | Define tracking measures; recommend resource allocation |
| **Cost** | Calculate cost at various stages | Gather team costs; recommend build/buy decisions |
| **Quality** | Define productivity and quality measures | Design for quality; define code-level quality metrics |
| **Human Resources** | Map skill sets; ensure training | Define technical skill needs; mentor developers |
| **Communications** | Manage external communications | Ensure developer coordination; solicit feedback |
| **Risk** | Prioritize risks; report to management | Identify risks; adjust architecture to mitigate |
| **Procurement** | Procure resources; introduce technology | Determine technology requirements; recommend tools |

> **Core principle**: Project manager handles external/business aspects; architect handles internal/technical aspects. Both must coordinate closely.

#### Global / Distributed Development

**Drivers**: Cost arbitrage, skill set availability, local market knowledge

**Challenge**: Module dependencies across teams require coordination:
- **Co-located**: Informal contacts (coffee room, hallway)
- **Distributed**: Documentation, formal meetings, electronic communication (email, wikis, blogs)

**Architecture becomes more critical** in distributed development — it defines the interfaces and coordination points between teams.

### 4.3 Implementing

#### Tradeoffs

Between **quality, schedule, functionality, and cost** — the project manager's constituency.

- Creeping functionality: PM acts as gatekeeper; **change control board** adds bureaucracy to manage requests
- Architect is gatekeeper for **architectural changes** — any change incurs cost in code, documentation, and conformance tools
- **Documentation is especially important in distributed development** — substitutes for informal coordination

#### Incremental Development

Every 6–8 weeks a new release. Each release cycles through:
1. **Planning** — Update software development plan
2. **Development** — Code; daily builds and automated testing
3. **Test and repair** — Execute test plan; fix or carry forward defects

#### Tracking Progress

- **Personal contact**: One-on-one meetings (doesn't scale well)
- **Status meetings**: Teams report progress; issues assigned to individuals for resolution outside meeting
- **Meetings should have**: Written agendas, read-ahead prework, only essential attendees

#### Risk Management

- Risks raised by developers, reviews, architecture evaluations
- PM prioritizes risks (with architect's help)
- Develop **mitigation strategies** for serious risks (cost/benefit decision)

### 4.4 Measuring

#### Global Metrics

| Metric | Purpose |
|--------|---------|
| **Project size** | Lines of code, function points, test suite size |
| **Schedule deviation** | Actual vs. planned work time (time lost can never be recovered) |
| **Developer productivity** | Anomalies indicate training gaps or estimation errors; earned value management |
| **Defects** | Track over time; also track **technical debt** |

All metrics have both a **historical basis** (for estimation) and a **project basis** (for ongoing management).

#### Phase Metrics and Cost to Complete

- **Open issues** per phase — track and allocate resources for timely resolution
- **Unmitigated risks** from reviews — report number of high-priority risks and mitigation status
- **Cost to complete** — bottom-up measure from team ownership of schedule

### 4.5 Governance

> *"The practice and orientation by which enterprise architectures and other architectures are managed and controlled."* — The Open Group

#### Governance Board Responsibilities

1. Implement system of controls over creation and monitoring of all architectural components and activities
2. Ensure compliance with internal and external standards and regulatory obligations
3. Establish processes for effective management within agreed parameters
4. Develop practices ensuring accountability to clearly identified stakeholders

#### Governance Challenges

Each system in an enterprise has its own stakeholders, release cycles, and governance processes. Example:
- Manufacturing systems: 6-month release cycle
- Integration systems: 9-month release cycle (tied to commercial product)
- Enterprise systems: 12-month release cycle (tied to fiscal year)

**The governance problem**: Reconciling heterogeneous release schedules into a coherent end-to-end solution while respecting each system's existing stakeholders.

#### Key Insight

> Maintaining effective governance without excessive overhead is a difficult balance. The question of "who is in control" over a particular portion of the system may prevent some business goals from being realized.

---

## Summary Table: Chapters 19–22 at a Glance

| Chapter | Focus | Key Methods/Frameworks | Key Outputs |
|---------|-------|------------------------|-------------|
| **19** | Implementation & Testing | Code templates, test levels, risk-based testing, testability design | Testable architecture; alignment of code and architecture |
| **20** | Reconstruction & Conformance | 4-phase reconstruction process, static/dynamic analysis tools | As-built architecture; conformance violations |
| **21** | Evaluation | ATAM (9 steps, 4 phases), Lightweight Architecture Evaluation | Risks, nonrisks, sensitivity/tradeoff points, risk themes |
| **22** | Management & Governance | Top-down/bottom-up planning, PM-architect collaboration, metrics | Software development plan; governance framework |


## Related

- [[Software Architecture Overview]] — All architecture topics
- [[07_Design_and_Documentation]] — Architecture design process
- [[08_Architecture_in_Agile]] — Agile architecture
- [[10_Economics_and_Product_Lines]] — Economic evaluation (CBAM)
