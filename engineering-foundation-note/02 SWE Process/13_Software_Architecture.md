---
tags: [architecture, design, quality-attributes, software-engineering]
source: "SWE Theory & Practice — Ch 5: Designing the Architecture"
---

# Software Architecture

## The Design Process

Design is the creative process of figuring out **how** to implement all of the customer's requirements. Early design decisions address the system's **architecture** — how to decompose the system into units, how units relate to one another, and any externally visible properties of the units (Bass, Clements, and Kazman 2003).

### Routine vs. Innovative Design

| Aspect | Routine Design | Innovative Design |
|---|---|---|
| Approach | Reuse and adapt existing solutions | Novel solution from first principles |
| Speed | Fast — leverages proven patterns | Slower — irregular bursts of insight |
| Risk | Lower — track record exists | Higher — must be vigorously evaluated |
| Guidance | Architectural styles, patterns, conventions | Basic design principles (descriptive, not prescriptive) |

### Sources of Design Advice

- **Similar systems** — clone or adapt existing solutions
- **Reference models** — standard generic architectures for a domain (e.g., compiler reference model)
- **Architectural styles** — generic patterns for decomposing problems (pipe-and-filter, client-server, etc.)
- **Design patterns** — generic solutions for lower-level module decisions (Ch 6)
- **Design conventions / idioms** — collections of decisions promoting certain qualities (e.g., ADTs)
- **Design principles** — descriptive characteristics of good design

### Design Process Model

The architecture development process is **iterative**, converging on a design through four activities:

1. **Modeling** — experiment with possible decompositions
2. **Analysis** — assess the preliminary architecture against quality attributes
3. **Documentation** — record architectural decisions in a SAD (Software Architecture Document)
4. **Review** — check that the architecture satisfies the requirements

> The SAD communicates system-level design decisions to the development team, helps onboard new members, and guides maintenance.

### Agile Architectures

Agile methods conflict with architecture's role in documenting hard-to-change decisions. Agile architectures:
- Model only small features, often one at a time
- Discard and rebuild models as solutions become clear
- Follow "just barely good enough" modeling (Ambler 2003)
- Face challenges with continuous refactoring of large, complex systems

---

## Modeling Architectures

Six uses of architectural models (Garlan 2000):

1. **Understand** the system — what it will do and how
2. **Reuse** assessment — what can be borrowed from prior systems; what will be reusable
3. **Blueprint** for construction — identify "load-bearing" design decisions
4. **Evolution** reasoning — performance, cost, prototyping concerns
5. **Dependency analysis** — select appropriate design, implementation, and testing techniques
6. **Management decisions** — understand risks in implementation and maintenance

> Box-and-arrow diagrams are the most common modeling approach, often with a legend explaining box and arrow types.

---

## Decomposition and Views

### Design Methods

| Method | Focus | High-Level Design |
|---|---|---|
| **Functional decomposition** | Functions/requirements | System-level functions → subfunctions → modules |
| **Feature-oriented** | Features | Service + collection of features; how each augments the service |
| **Data-oriented** | Data partitioning | Conceptual data structures → distributed data objects |
| **Process-oriented** | Concurrent processes | Main tasks → runtime processes → coordination |
| **Event-oriented** | Event handling | Input events → states → state transformations |
| **Object-oriented** | Object types | Object types + relationships → attributes + operations |

### Key Terminology

- **Module** — structural unit of code (atomic like a Java class, or aggregated)
- **Component** — identifiable runtime element (e.g., the parser in a compiler)
- **Software unit** — generic term for a system's composite parts

### Modularity

A design is **modular** when:
- Each activity is performed by **exactly one** software unit
- The inputs and outputs of each unit are **well-defined** (interface accurately specifies externally visible behavior)

### Architectural Views

| View | What It Shows |
|---|---|
| **Decomposition** | Programmable units in a hierarchy |
| **Dependencies** | Which units depend on which (calls, data reliance) |
| **Generalization** | Inheritance / specialization relationships |
| **Execution** | Runtime components and connectors (box-and-arrow) |
| **Implementation** | Code units mapped to source files |
| **Deployment** | Runtime entities mapped to hardware resources |
| **Work-assignment** | Design tasks mapped to project teams |

> The full architecture is the **collection of all views**, with documented mappings among them.

---

## Architectural Styles

Architectural styles are established, large-scale patterns of system structure — loose templates offering solutions for coordinating a system's components. They constrain intercomponent interactions to help achieve specific system properties.

### Pipe-and-Filter

- **Components**: Filters (independent data-transforming functions) + Pipes (data connectors)
- **Properties**:
  - ✅ System transformation = functional composition of filters
  - ✅ Filters reusable across systems with same data format
  - ✅ Easy evolution — add/replace/remove filters independently
  - ✅ Supports throughput analysis
  - ❌ Performance penalty from repeated parsing/unparsing of fixed-format data

### Client-Server

- **Components**: Clients (request services) + Servers (provide services via request/reply)
- **Asymmetric**: Clients know servers; servers don't know clients
- **Variants**: Centralized, replicated, or distinct servers; **multi-tier** hierarchies
- **Properties**:
  - ✅ Separates client/server code → flexible deployment
  - ✅ Supports reuse of common-service servers
  - ✅ Multi-tier improves modularity

### Peer-to-Peer (P2P)

- **Components**: Each peer acts as both client and server to other peers
- **Properties**:
  - ✅ Scales well — added component = new capacity
  - ✅ Fault-tolerant — data replicated across peers
  - ❌ Insecure — encourages data sharing; accidental exposure common
  - ❌ Poor for frequently changing content, quality-critical data, or trust-sensitive scenarios

### Publish-Subscribe

- **Components**: Publishers (announce events) + Subscribers (react to events) + Event bus infrastructure
- **Implicit invocation**: Subscribers register procedures with events; infrastructure invokes them on event occurrence
- **Properties**:
  - ✅ Strong support for evolution and customization — add components without affecting others
  - ✅ Easy component reuse in other event-driven systems
  - ❌ Shared persistent data requires a repository, diminishing extensibility
  - ❌ Difficult to test — behavior depends on which subscribers are present

### Repositories

- **Components**: Central data store + data-accessing components
- **Blackboard variant**: Data accessors (knowledge sources) are **reactive** — they execute in response to the data store's current state
- **Properties**:
  - ✅ Centralized data management — localize persistence, concurrency, security, fault protection
  - ❌ Distributing/replicating data adds complexity and consistency challenges

### Layering

- **Structure**: Layers provide services to the layer above; act as clients of the layer below
- **Constraint**: Pure layering — access only same layer + immediately below (layer bridging relaxes this)
- **Example**: OSI network model (Physical → Data Link → Network → Transport → Session → Presentation → Application)
- **Properties**:
  - ✅ Each layer raises abstraction, hides implementation details
  - ✅ Easy portability if lowest layer encapsulates platform interactions
  - ✅ Layer modification affects at most two adjacent layers
  - ❌ Not always easy to structure into distinct layers
  - ❌ Potential performance cost from inter-layer calls (reduced by compilers/linkers)

### Combining Styles

Real architectures rarely use a single pure style. Combining approaches:
1. **Different styles at different decomposition levels** (e.g., client-server at top, layers inside server)
2. **Mixture of styles** for different components/interaction types
3. **Separate views** for incompatible styles, with documented correspondences

---

## Achieving Quality Attributes

**Tactics** are fine-grained design decisions that improve how well a design achieves specific quality goals.

### Modifiability

More than half of full life-cycle cost is spent after the first release — modifiability is essential.

**Minimizing directly affected units** (cluster anticipated changes):
- **Anticipate expected changes** — encapsulate each likely change in its own unit
- **Cohesion** — keep units focused; changes confined to few units
- **Generality** — general units accommodate change via input modification

**Minimizing indirectly affected units** (reduce dependencies):
- **Coupling** — lower coupling reduces ripple effects
- **Interfaces** — interact only through public interfaces; changes don't spread unless interface changes
- **Multiple interfaces** — new data/services via new interfaces without changing existing ones

### Performance

Attributes: response time, throughput, load capacity.

**Tactics**:
- **Improve resource utilization** — concurrency, data replication (with consistency overhead)
- **Manage resource allocation** — scheduling policies:
  - First-come/first-served
  - Explicit priority (risk of starvation)
  - Earliest deadline first
  - Preemption and round-robin
- **Reduce demand** — more efficient code, lower-frequency sampling of sensor data

### Security

Two key characteristics:
- **Immunity** — ability to thwart attacks (include all security features; minimize exploitable weaknesses)
- **Resilience** — ability to recover from successful attacks (segment functionality; enable rapid restoration)

> Layering inherently supports security by restricting interactions. P2P is inherently difficult to secure.

### Reliability

A system is **reliable** if it correctly performs required functions under assumed conditions. A system is **robust** if it functions correctly in the presence of invalid inputs or stressful conditions.

**Fault Prevention & Detection**:
- **Active fault detection** — periodic checks for fault symptoms; exception handling for known exceptions
- **Redundancy checks** — forward/backward pointers, checksums, n-version programming
- **Monitoring** — second system interrogates primary; diagnostic transactions

**Fault Recovery Tactics**:
| Tactic | Description |
|---|---|
| Undo transactions | Partial effects undone on fault |
| Checkpoint/rollback | Record state periodically; roll back and reapply on fault |
| Backup | Substitute faulty unit (parallel or on-demand) |
| Degraded service | Roll back and offer reduced service |
| Correct and continue | Fix symptoms rather than root cause |
| Report | Record fault and state for later fix |

### Robustness

**Defensive design** — anticipate external problems:
- **Mutual suspicion** — each unit checks inputs for correctness and preconditions
- **Health checks** — periodic pings in distributed systems
- **Redundancy** — multiple computers vote (e.g., space shuttle uses 5 duplicate computers)

### Usability

Architectural implications:
- User interface should reside in **its own software unit or layer**
- System needs processes for **generic commands** (cancel, undo, aggregate, multiple views)
- System should maintain **environmental models** for time-activated and system-initiated activities

### Business Goals

| Trade-off | Consideration |
|---|---|
| Buy vs. build | Purchased components save time but constrain design and create supplier dependency |
| Initial development vs. maintenance | Modifiability increases complexity and initial cost but reduces long-term cost |
| New vs. known technologies | New tech requires expertise acquisition; costs money and delays release |

---

## Collaborative Design

Design is typically performed by teams. Key challenges:
- Differences in experience, understanding, and preference
- Cultural differences in group behavior (e.g., Japanese developers less likely to express individual opinions in groups)
- **Outsourcing** challenges: time zones, unstable connections, mismatched processes, lack of local business knowledge

### Causes of Design Breakdown

- Lack of specialized data schemas
- Poor resource allocation to design activities
- Poor prioritization of issues
- Difficulty considering all constraints
- Difficulty performing mental simulations with many steps
- Difficulty tracking postponed subproblems
- Difficulty merging partial solutions

---

## Architecture Evaluation and Refinement

### Measuring Design Quality

- Coupling metrics (Briand, Devanbu, and Melo 1997): count interactions between classes by relationship type, interaction type, and ripple-effect locus
- High coupling to non-ancestor/descendant/non-friend attributes → more fault-prone
- Design information predicts which parts are most problematic → focus testing and fault prevention

### Safety Analysis (Fault-Tree Analysis)

Originally developed for the U.S. Minuteman missile program. Process:

1. **Identify possible failures** using guidewords (no, more, less, part of, other than, early, late, before, after)
2. **Build fault tree** — root = failure; other nodes = events/faults leading to it
   - **AND gate** — both children must occur for parent
   - **OR gate** — one child sufficient for parent
   - **n-of-m** — n faulty components out of m redundant ones
3. **Derive cut-set tree** — reveals minimal event combinations that cause the failure
   - Singletons in cut-set → single point of failure
   - Multi-element sets → compound faults

**Remediation choices**: correct the fault, add prevention components, add detection/recovery components.

### Security Analysis (Allen et al. 2008)

Six steps:
1. **Software characterization** — review requirements, use cases, SAD, test plans
2. **Threat analysis** — identify who might attack and when
3. **Vulnerability assessment** — find flaws that threats can exploit
4. **Risk likelihood determination** — motivation, ability, impact, current controls
5. **Risk impact determination** — business-ending > damaging > recoverable > nuisance
6. **Risk mitigation planning** — prioritize projects by business impact, likelihood, cost

### Trade-off Analysis

Different architectural styles can solve the same problem with different quality trade-offs. The **KWIC system** (Parnas 1972) illustrates four designs:

| Design | Style | Strengths | Weaknesses |
|---|---|---|---|
| Shared-data | Repository | Efficient (no data replication) | Hard to change (modules access data directly) |
| Data abstraction | Module-based | Reusable; data changes isolated | Functionality changes difficult (tightly coupled with data) |
| ADT + implicit invocation | Publish-subscribe | Easy to add/remove functionality | More complex infrastructure |
| Pipe-and-filter | Pipe-and-filter | Easy to add/replace filters | Performance penalty from data conversion |

> **No single design is best for all quality goals.** Trade-off analysis requires comparing alternatives against the system's specific quality requirements.

---

## Summary

| Concept | Key Takeaway |
|---|---|
| Architecture | Earliest, hardest-to-change design decisions |
| Architectural styles | Generic templates for structuring systems |
| Views | Multiple perspectives on the same architecture |
| Quality attributes | Achieved through architectural styles + fine-grained tactics |
| Evaluation | Fault-tree analysis, security analysis, coupling metrics, trade-off analysis |
| Collaboration | Team design requires managing cultural, communication, and coordination challenges |


## Related

- [[Engineering Foundation Overview]] — All engineering foundation topics
- [[02 SWE Process/12_Requirements_Engineering|12_Requirements_Engineering]] — Requirements that drive architecture
- [[02 SWE Process/14_Design_Principles_and_Patterns|14_Design_Principles_and_Patterns]] — Module-level design patterns
- [[02 SWE Process/18_Evaluation_and_Improvement|18_Evaluation_and_Improvement]] — Architecture evaluation techniques
