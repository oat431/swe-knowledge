---
tags: [life-cycle, project-management, pmbok]
---

> *Source: PMBOK® Guide — Eighth Edition by PMI, The Standard for Project Management §4 (pp. 57–74)*

## Purpose

The **project life cycle** is the series of activities and/or phases that a project passes through from its start to its closure. It provides the foundational framework for engaging with a project, regardless of the specific project work involved or the adopted development approach. This section examines the project life cycle through five factors: project phases, development approaches, considerations for selecting an approach, delivery cadence, and the five Project Management Focus Areas.

## Project Phases

A **project phase** is a collection of logically related project activities that culminates in the completion of one or more deliverables and/or outcomes. Phases may be **sequential**, **transitional**, or **overlapping** — and a combination may be used within a single project.

### Phase Attributes

Each phase can be described by measurable, phase-specific attributes:

| Attribute | Description |
|---|---|
| Name | Label identifying the phase (e.g., Design, Build, Test) |
| Number | Ordinal position in the life cycle |
| Duration | Expected length of the phase |
| Resource requirements | Staffing, budget, and material needs |
| Entrance criteria | Conditions required to enter the phase |
| Exit criteria | Conditions required to complete the phase |

### Phase Gates

A **phase gate** (also called *stage gate*, *gate review*, *iteration review*, or *decision point review*) is a review checkpoint that verifies desired outcomes or exit criteria have been achieved before proceeding. Phase gates may be placed:

- At the **end** of a phase (traditional) — review progress and make go/no-go decisions
- At the **start** of a phase (*pre-phase gates*) — ensure alignment, resources, and risk assessment before work begins

Possible decisions at a phase gate:
- Proceed to the next phase
- Proceed with modifications
- End the project or phase
- Remain in the phase
- Repeat the phase or elements of it
- Park the project temporarily

### Generic Phase Characteristics

> **Cost and Staffing:** In predictive scenarios, levels are low at start, increase as work is carried out, and drop rapidly as the project or phase ends.

> **Risk and Uncertainty:** Greatest at the start of the project or phase; decrease over the life cycle as decisions are reached and deliverables are accepted (see Figure 4-1).

> **Stakeholder Influence on Change:** Highest at the start; decreases as the project progresses. In adaptive contexts, stakeholders participate actively throughout, enabling early risk detection through consistent feedback.

### Life Cycle Flexibility

The project life cycle should be flexible enough to meet or exceed target objectives while protecting the value proposition. Flexibility includes:

- Selecting the development approach or mix of approaches
- Identifying the types of processes and activities needed
- Adjusting attributes of activities, phases, or processes (name, duration, criteria)

Project life cycles are **independent of product life cycles**, though a product may be an outcome of a project.

## Development Approaches

A **development approach** is the means used to create and evolve a product, service, or result during the project life cycle. It is distinct from the "development phase of the project" (the stage where actual creation and testing occur). The three approaches form a **spectrum**:

```
Predictive ◀──────────────────── Hybrid ────────────────────▶ Adaptive
```

### Predictive Approach

Also referred to as *waterfall*, *plan-driven*, or *traditional*. Optimal when project scope can be stabilized early.

**Characteristics:**
- Scope, schedule, cost, resource needs, quality requirements, and risks are well defined in early phases
- Much of the planning is performed **up front**
- Relies on robust change control mechanisms and planned reevaluation between phases
- The integrated baseline (scope + schedule + cost) is established early

**Best suited for:**
- Large projects with significant investment or regulatory oversight
- Projects where the **cost of iterating far exceeds its value** (e.g., the build phase of construction)
- Heavily regulated environments (e.g., healthcare, drug development) requiring formal phase gates
- Projects with well-understood, stable requirements

**Incremental Delivery Within Predictive:**

Even in predictive approaches, delivery can be **incremental** — the overall scope is planned up front, but the project is divided into phases/increments, each delivering a usable part of the product. This provides earlier stakeholder value while maintaining structured planning and controlled change management.

**Sample Predictive Life Cycle:**
```
Feasibility ⟶ Design ⟶ Build ⟶ Check ⟶ Transfer ⟶ Close
```

### Adaptive Approach

Also referred to as *change-driven* or *agile*. Useful when requirements and technical solutions are subject to high uncertainty and volatility.

**Characteristics:**
- A clear **vision** is established at the start, and initial requirements are **progressively elaborated**, changed, or replaced based on feedback
- Both **iterative** (refining through repeated cycles) and **incremental** (delivering in small, usable segments)
- Iterations are typically **1–4 weeks**, with a demonstration of accomplishments at the end of each
- The team determines scope from a prioritized **product backlog** and works collaboratively

**Flow-Based Scheduling:**
- Optimizes the flow of work through a system
- Maximizes throughput of deliverables based on resource capacity
- Minimizes time/resource waste and maximizes process efficiency
- Examples: **Kanban method** (visual WIP limits, continuous delivery) and **Theory of Constraints** (identifying and addressing the limiting factor)

**Inverted Triangle Concept:**

When constraints must be traded off, adaptive approaches offer flexibility:

| Fixed Constraint | Variable Constraint |
|---|---|
| Scope | Budget and Schedule |
| Budget and Schedule | Scope |

**Agile Methods — Breadth vs. Depth:**

Agile methods vary by breadth (how much of the project/product life cycle they cover) and depth (how detailed/prescriptive the guidance is). Examples plotted on this spectrum include Scrum, Kanban, Crystal, LeSS, Scrum of Scrums, SAFe, and Lean. **PMI Disciplined Agile® (DA®)** is a **tool kit** (a collection of flexible tools and techniques) rather than a prescriptive method.

**Best suited for:**
- **Design projects** — requirements not fully defined up front; partial deliveries and changes expected
- **New product development** — final features refined based on market needs (e.g., mobile apps, web services)
- **Digital/software projects** — evolving scope, high uncertainty, need for early or incremental delivery

### Hybrid Approach

A combination of adaptive and predictive approaches. Most projects today benefit from a hybrid approach, which provides flexibility to adapt to changing conditions while retaining control over predictable aspects.

**Four Common Patterns:**

| Pattern | Description |
|---|---|
| **1. Adaptive → Predictive rollout** | Early processes use adaptive development; followed by a predictive rollout phase |
| **2. Simultaneous** | Adaptive and predictive approaches used concurrently throughout the life cycle |
| **3. Predictive-dominant with adaptive component** | Largely predictive project with a small adaptive sub-element |
| **4. Adaptive-dominant with predictive component** | Largely adaptive approach with a predictive sub-component |

**DA Hybrid Levels:**
- **Level 1:** Predictive is dominant; adaptive elements reduce specific pain points
- **Level 2:** Both approaches contribute significantly to project success
- **Level 3:** Adaptive is dominant; predictive elements satisfy business constraints

**Example Scenarios:**
- **Construction with IT integration** — building a smart building (predictive construction + adaptive IT systems)
- **Healthcare solutions** — electronic medical records implementation (predictive infrastructure + adaptive UI development)
- **Modular deliverables** — one deliverable developed adaptively (software), another predictively (data center)

## Considerations for Approach Selection

Selection factors fall into three categories. Multiple development approaches may be used within the same project for different deliverables.

### Deliverable Factors

| Factor | Favors Predictive | Favors Adaptive |
|---|---|---|
| **Degree of innovation** | Low innovation | New/complex technological or business model innovation |
| **Requirements certainty** | Well known, straightforward | Unclear, evolving |
| **Scope stability** | Stable throughout execution | Unstable, likely to change |
| **Ease of change** | Difficult or costly to change | Easy to incorporate changes |
| **Delivery options** | Single, final delivery | Component-based or progressive elaboration |
| **Risk** | Manageable through up-front planning | Adaptive approaches help manage uncertainty |
| **Safety requirements** | Rigorous safety needs → significant up-front planning | — |
| **Feedback value** | — | High value from frequent end-user/stakeholder feedback |
| **Regulations** | Significant regulatory oversight → documented processes | — |

### Project Factors

| Factor | Implication |
|---|---|
| **Stakeholders** | Adaptive: requires significant stakeholder involvement throughout. Predictive: stakeholders engaged more at phase gates. |
| **Schedule constraints** | Need early value → iterative/adaptive. Fixed end date → predictive with focused planning. |
| **Financing uncertainty** | Adaptive/iterative may generate more value in uncertain funding environments. |
| **Project size and complexity** | Larger, complex projects with evolving requirements → more flexible (adaptive/hybrid). |
| **Team experience and skills** | Adaptive/agile benefits from strong communication, collaboration, and problem-solving skills. |
| **Interdependencies** | Reliance on other projects/departments may favor an iterative approach for coordination. |

### Organizational Factors

| Factor | Implication |
|---|---|
| **Organizational structure** | Fixed/functional hierarchy → predictive. Network-oriented → adaptive/hybrid. |
| **Culture** | Manage-and-direct, plan-against-baseline → predictive. Embraces uncertainty, self-managed teams → adaptive. |
| **Organizational capability** | Established PM processes → predictive reliability. Policies, reporting, and attitudes must align with the chosen approach. |
| **Project team size and location** | Adaptive: often 3–9 team members, favors colocation. Predictive/hybrid: size depends on requirements. Remote work can enhance efficiency in any approach. |

## Delivery Cadence

Based on the selected development approach, projects can adopt one of four delivery cadences:

| Cadence | Description | Typical Context |
|---|---|---|
| **Single delivery** | All outcomes delivered at the end of the project | Process-reengineering, traditional waterfall |
| **Multiple deliveries** | Components delivered at different times; may be sequential or parallel | Drug development (preclinical → Phase 1 → Phase 2 → Phase 3 → approval → launch); building security upgrades with parallel components |
| **Periodic deliveries** | Deliveries on a regular, fixed schedule (e.g., monthly, biweekly) | Adaptive/agile projects; internal deliveries every 2 weeks with periodic market releases |
| **Continuous delivery** | Incrementally delivering value to production on an ongoing basis; tested and validated increments are always production-ready | DevOps, flow-based approaches; leverages automation for frequent, low-risk deployments |

## Project Management Focus Areas

The five **Project Management Focus Areas** (formerly known as Process Groups) describe fundamental actions that occur in every project, regardless of development approach. They are **not project phases** — they overlap and interact dynamically throughout the project life cycle.

### The Five Focus Areas

#### 1. Initiating

Defines a new project or new phase, including formal authorization to start.

**Key actions:**
- Define initial scope and commit initial financial resources
- Identify stakeholders and assign project management roles
- Align stakeholders' expectations with the project's purpose
- Capture information in artifacts such as a **project charter** and **stakeholder register**

**Key benefit:** Projects are only authorized when aligned with organizational strategic objectives and when the business case, benefits, and stakeholders are considered from the start.

**Business documents** (business case, benefits management plan) originate outside the project but serve as inputs. The sponsor provides minimum information needed for the project manager to begin.

#### 2. Planning

Establishes the intended scope, defines and refines objectives, and develops the course of action.

**Key actions:**
- Develop artifacts such as a product backlog or **project management plan**
- Use repeated feedback loops for additional analysis
- Apply **progressive elaboration** — ongoing refinement as more information becomes available

**Predictive vs. Adaptive Planning:**
- **Predictive:** Front-loads planning; updates and refinements occur but less frequently
- **Adaptive:** Brief, high-level planning up front ("roadmapping"), followed by consistent, frequent planning and replanning throughout

**Key benefit:** Defines the course of action for successfully completing the project or phase.

#### 3. Executing

Completes the work in a manner consistent with the currently agreed-upon course of action.

**Key actions:**
- Coordinate resources
- Manage stakeholder engagement
- Perform project activities and tasks

**Key benefit:** Drives focused execution to achieve the value proposition represented by the integrated baseline. This Focus Area is where the choice of development approach (adaptive, predictive, or hybrid) is often **most evident**.

The project management plan can and should be changed whenever such a change would enhance the project's value proposition.

#### 4. Monitoring and Controlling

Tracks, measures, reviews, and regulates the progress and performance of the project.

**Key actions:**
- **Monitoring:** Compare performance data, produce performance measures, compare actual vs. planned, report performance information
- **Controlling:** Analyze variances, assess trends for process improvements, evaluate alternatives, recommend corrective actions

**Key benefit:** Ensures the project is on track to meet expectations. Performed at regular intervals, at appropriate events, or when exceptional conditions occur. Runs **in parallel** with the other Focus Areas — it is not a separate, stand-alone area.

> For projects with a stable plan, M&C effort is fairly consistent. For projects with frequent changes (common in adaptive approaches), M&C efforts are likely less consistent but more continuous.

#### 5. Closing

Formally completes or closes a project, phase, or contract — or terminates a project before completion.

**Key actions:**
- Verify that specific actions are completed for other Focus Areas before declaring closure
- Verify achievement of project outputs, outcomes, and benefits
- Transition to operations
- Address suspension or early closure when canceling maximizes return on investment (or minimizes negative return)

**Key benefit:** Phases, projects, and contracts are closed out appropriately, transitioning to operations in a manner that helps meet or exceed target business objectives.

### Focus Area Interactions

The five Focus Areas are **independent of application area** (marketing, IT, accounting) or industry focus (construction, aerospace, telecommunications). They are also **independent of approach** — all development approaches honor these five in some manner.

**Predictive Projects (Figure 4-13):**
- Focus Areas show varying levels of effort across distinct project phases (Engineering, Procurement, Construction, Commissioning, Handover)
- Planning effort is concentrated early; Executing peaks mid-project; M&C provides steady oversight
- The workflow is more sequential, though overlap still occurs

**Adaptive Projects (Figure 4-14):**
- Focus Areas are revisited within **short iterations** in a flexible, overlapping manner
- Planning occurs in bursts before each iteration; Initiating, Executing, M&C, and Closing activities interweave
- Each Focus Area is revisited regularly, enabling continuous refinement based on real-time data, feedback, and evolving needs
- This iterative approach enables greater responsiveness to change

## Related Chapters

- [[01_Value_Delivery_System|Chapter 2: A System for Value Delivery]] — how projects fit within the broader organizational value delivery system
- [[04_Governance_Performance_Domain|Chapter 4: Project Performance Domains]] — the eight domains that intersect with Focus Areas
- [[11_Tailoring|Chapter 5: Tailoring]] — adapting approaches, life cycles, and processes to individual projects
- [[13_Tools_and_Techniques|Chapter 6: Models, Methods, and Artifacts]] — commonly used tools and techniques across project management
- [[02_Project_Management_Principles|Project Management Principles]] — the 12 principles that underpin the Focus Areas and development approaches
