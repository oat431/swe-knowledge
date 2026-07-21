---
tags:
  - architecture
  - quality-attributes
  - scenarios
  - tactics
  - software-architecture
---

# Quality Attributes Overview

> **Source:** Bass, Clements & Kazman, *Software Architecture in Practice* (SAiP), Chapters 4 & 12
> **Purpose:** Understand how to specify quality attribute requirements, achieve them through architectural tactics, and guide quality-driven design decisions.

## 1. Architecture and Requirements (Ch 4.1)

Requirements come in three categories:

| Category | Description | Architecture's Response |
|---|---|---|
| **Functional** | What the system must do; behavior/runtime response to stimuli | Assign responsibilities to architectural elements |
| **Quality Attribute (QA)** | Qualifications of functional requirements (how fast, how resilient) or of the overall product (time to deploy, operational cost) | Satisfied by structures, behaviors, and interactions of architectural elements |
| **Constraints** | Design decisions with zero degrees of freedom (mandated language, existing module, management fiat) | Accept and reconcile with other design decisions |

## 2. Functionality and Architecture (Ch 4.2)

Functionality has a paradoxical relationship with architecture:

- **Functionality does not determine architecture.** Given a set of functions, infinitely many architectures could satisfy them. If functionality were all that mattered, a monolithic blob would suffice.
- **We structure systems** (layers, services, modules, threads, tiers, databases) to make them understandable and to support *other* quality attributes — not just functionality.
- **Functionality constrains allocation.** When QAs (modifiability, performance, security) are important, architecture constrains how responsibilities are allocated to elements.
- **"Responsibility" is a better term** than "functional requirement." Questions like "What are the timing constraints on that responsibility?" or "What modifications are anticipated for that responsibility?" are precise and actionable.

## 3. Quality Attribute Considerations (Ch 4.3)

Three problems with traditional QA discussions:

1. **Untestable definitions.** "The system will be modifiable" is meaningless — every system is modifiable with respect to *some* changes and not others. QAs must be defined in context.
2. **Overlapping concerns.** Is a denial-of-service attack an availability, performance, security, or usability problem? All four communities claim it — this doesn't help architects create solutions.
3. **Different vocabularies.** Performance has "events," security has "attacks," availability has "failures," usability has "user input" — all may refer to the same occurrence.

**Two categories of QAs:**
- **Runtime qualities:** Properties observable during execution (availability, performance, security, usability)
- **Development-time qualities:** Properties of the development process (modifiability, testability, deployability)

**Tradeoffs are inevitable.** Almost every QA negatively affects performance. Portability requires isolation (process/procedure boundaries) → overhead → hurts performance. Architecture design is about making the right tradeoffs.

## 4. Specifying Quality Attribute Requirements (Ch 4.4)

A QA requirement must be **unambiguous and testable**. SAiP uses a common **six-part scenario** form:

| Part | Description |
|---|---|
| **1. Source of Stimulus** | Entity (human, system, actuator) that generates the stimulus |
| **2. Stimulus** | Condition requiring a response when it arrives |
| **3. Environment** | Circumstances under which the stimulus occurs (normal, overloaded, startup, degraded mode) |
| **4. Artifact** | Portion of the system stimulated (whole system, specific component, data store) |
| **5. Response** | Activity undertaken upon stimulus arrival |
| **6. Response Measure** | Quantifiable metric to test whether the requirement is satisfied |

**General vs. Concrete Scenarios:**
- **General scenarios:** System-independent; characterize a QA generically (e.g., a fault arrives during normal operation; system detects and recovers within X time)
- **Concrete scenarios:** System-specific instances of general scenarios (e.g., "The heartbeat monitor detects server unresponsiveness during normal operation; the system informs the operator and continues with no downtime")

This form unifies QA specification across all attributes — performance, security, availability, modifiability, usability, and testability all use the same six-part template.

## 5. Achieving Quality Attributes through Tactics (Ch 4.5)

**Architectural tactics** are design decisions that influence a single QA response. They are the "atomic" design techniques architects use to achieve quality goals.

Key distinctions:

| Concept | Scope |
|---|---|
| **Tactic** | Focuses on a single QA response; no built-in tradeoff consideration |
| **Architectural Pattern** | Bundle of design decisions; tradeoffs are built-in |

**Why catalog tactics?**
1. Patterns are often too complex to apply as-is; understanding tactics helps architects modify and adapt patterns.
2. When no pattern fits, tactics let architects construct design fragments from "first principles."
3. Cataloging tactics makes design more systematic.

**Tactics need refinement.** E.g., "schedule resources" (performance tactic) refines into shortest-job-first, round-robin, etc. "Use an intermediary" (modifiability tactic) refines into layers, brokers, proxies.

## 6. Guiding Quality Design Decisions — Seven Categories (Ch 4.6)

Architecture can be viewed as applying a collection of design decisions across seven categories:

### 6.1 Allocation of Responsibilities
- Identify important responsibilities: system functions, architectural infrastructure, QA satisfaction
- Determine allocation to non-runtime and runtime elements (modules, components, connectors)
- Strategies: functional decomposition, real-world modeling, grouping by system modes, grouping by similar QA requirements (frame rate, security level, expected changes)

### 6.2 Coordination Model
- Identify elements that must coordinate (or are *prohibited* from coordinating)
- Determine coordination properties: timeliness, currency, completeness, correctness, consistency
- Choose communication mechanisms with appropriate properties: stateful vs. stateless, synchronous vs. asynchronous, guaranteed vs. non-guaranteed delivery, throughput/latency

### 6.3 Data Model
- Choose major data abstractions, their operations and properties (creation, initialization, access, persistence, manipulation, translation, destruction)
- Compile metadata for consistent data interpretation
- Organize data: relational DB, object collection, or both (with mapping)

### 6.4 Management of Resources
- **Hard resources:** CPU, memory, battery, hardware buffers, system clock, I/O ports
- **Soft resources:** system locks, software buffers, thread pools, non-thread-safe code
- Determine which elements manage each resource, sharing/arbitration strategies, and saturation impact

### 6.5 Mapping among Architectural Elements
- Map between different structure types (modules → threads/processes)
- Map software elements to environment elements (processes → CPUs)
- Map data model items to data stores, modules/runtime elements to units of delivery

### 6.6 Binding Time Decisions
Establish allowable ranges of variation — when and by whom they are bound:
- Build-time: parameterized makefiles (allocation of responsibilities)
- Runtime: protocol negotiation (coordination model), peripheral plug-and-play (resource management)
- App-store: automatic version selection per device (choice of technology)

**Tradeoff:** Later binding costs more to implement but makes future modifications cheaper.

### 6.7 Choice of Technology
- Determine available technologies for decisions in other categories
- Assess tool support (IDEs, simulators, testing tools)
- Evaluate internal familiarity and external support (courses, tutorials, contractors)
- Identify side effects (required coordination model, constrained resource management)
- Check compatibility with existing technology stack (runtime, communication, monitoring/management)

## 7. Quality Attribute in Detail: Availability (Ch 5)

Availability serves as the book's detailed walkthrough of one QA — illustrating scenarios, tactics, and design checklists.

### 7.1 Definition

**Availability:** The ability of a system to mask or repair faults such that the cumulative service outage period does not exceed a required value over a specified time interval.

Key concepts:
- **Fault** → cause of failure (internal or external)
- **Error** → intermediate state between fault occurrence and failure
- **Failure** → externally visible deviation from specification
- **MTBF** (Mean Time Between Failures) + **MTTR** (Mean Time to Repair) determine steady-state availability: `MTBF / (MTBF + MTTR)`

| Availability | Downtime / 90 days | Downtime / year |
|---|---|---|
| 99.0% | 21h 36m | 3d 15.6h |
| 99.9% | 2h 10m | 8h 46s |
| 99.99% | 12m 58s | 52m 34s |
| 99.999% ("5 nines") | 1m 18s | 5m 15s |
| 99.9999% | 8s | 32s |

### 7.2 Availability General Scenario

| Part | Possible Values |
|---|---|
| **Source** | Internal/external: people, hardware, software, physical infrastructure, physical environment |
| **Stimulus** | Fault: omission, crash, incorrect timing, incorrect response |
| **Artifact** | Processors, communication channels, persistent storage, processes |
| **Environment** | Normal operation, startup, shutdown, repair mode, degraded operation, overloaded operation |
| **Response** | Prevent fault → failure; Detect (log, notify); Recover (disable source, be unavailable, fix/mask, degraded mode) |
| **Response Measure** | Availability %, time to detect/repair, time in degraded mode, proportion of faults handled |

### 7.3 Availability Tactics

Three categories:

#### Detect Faults
| Tactic | Description |
|---|---|
| **Ping/Echo** | Async request/response between nodes to determine reachability and round-trip delay |
| **Monitor** | Component that watches health of processors, processes, I/O, memory |
| **Heartbeat** | Periodic message exchange between monitor and monitored process; watchdog timer variant |
| **Timestamp** | Detect incorrect event sequences via local clock or sequence numbers |
| **Sanity Checking** | Validate reasonableness of operations/outputs at interfaces |
| **Condition Monitoring** | Check conditions/assumptions; e.g., checksums |
| **Voting** | Triple Modular Redundancy (TMR): three components, identical inputs, voter detects inconsistency |
| **Exception Detection** | System exceptions (divide by zero), parameter fence, parameter typing, timeout |
| **Self-Test** | Components run procedures to test correct operation |

#### Recover from Faults
| Subcategory | Tactic | Description |
|---|---|---|
| **Preparation & Repair** | Active Redundancy (hot spare) | All nodes receive/process identical inputs in parallel; synchronous state; millisecond takeover |
| | Passive Redundancy (warm spare) | Active nodes provide periodic state updates to spares; balance of availability vs. cost |
| | Spare (cold spare) | Redundant spares remain out of service until fail-over; power-on-reset on activation |
| | Exception Handling | Mask fault by correcting cause and retrying; error codes or exception classes |
| | Rollback | Revert to previous known-good state (checkpoint); promote standby component |
| | Software Upgrade | In-service upgrades: function patch, class patch, hitless ISSU |
| | Retry | Assume transient fault; retry with limit before declaring permanent failure |
| | Ignore Faulty Behavior | Filter spurious messages (e.g., ACL for DoS attacks) |
| | Degradation | Maintain critical functions, drop less critical ones on component failure |
| | Reconfiguration | Reassign responsibilities to remaining functional resources |
| **Reintroduction** | Shadow | Operate repaired component in "shadow mode" before restoring active role |
| | State Resynchronization | Compare/reconcile states between active and standby (checksums, hash digests) |
| | Escalating Restart | Vary restart granularity to minimize service impact (child threads → unprotected memory → all memory → full reload) |
| | Non-Stop Forwarding (NSF) | Split control plane / data plane; data plane continues forwarding during control plane restart |

#### Prevent Faults
| Tactic | Description |
|---|---|
| **Removal from Service** | Temporarily take component out of service to scrub latent faults (memory leaks, fragmentation); a.k.a. *software rejuvenation* |
| **Transactions** | ACID semantics (atomic, consistent, isolated, durable); two-phase commit prevents race conditions |
| **Predictive Model** | Monitor health metrics to predict and preempt faults (session rates, queue lengths, threshold crossing) |
| **Exception Prevention** | Smart pointers, wrappers, abstract data types that prevent dangling pointers and resource leaks |
| **Increase Competence Set** | Design components to handle more edge cases as normal operation (e.g., wait for resource instead of throwing exception) |

### 7.4 Design Checklist for Availability

| Design Decision Category | Key Questions |
|---|---|
| **Allocation of Responsibilities** | Which responsibilities must be highly available? Are responsibilities allocated for fault detection (log, notify, disable, fix/mask, degrade)? |
| **Coordination Model** | Can coordination detect faults under degraded communication? Does it support artifact replacement? Does it work at startup/shutdown/overload? |
| **Data Model** | Which data abstractions could cause faults? Can they be disabled, made unavailable, or fixed/masked? Are write requests cached for unavailable servers? |
| **Mapping among Elements** | Can processes be reassigned at runtime? Which processors/stores/channels can be reactivated? How quickly can the system be reinstalled? Are redundant module mappings correct? |
| **Resource Management** | What critical resources are needed during faults? Are there sufficient resources to log/notify/disable/fix? Are input queues large enough to buffer during server failure? |
| **Binding Time** | Do late binding mechanisms cover faults from all bound sources? Is recovery fast enough for fault detection thresholds? Can the binding mechanism itself fail? |
| **Choice of Technology** | What technologies detect/recover/reintroduce? What faults can the chosen technologies themselves recover from or introduce? |

## 8. Other Quality Attributes (Ch 12 Overview)

Chapter 12 surveys additional QAs beyond the detailed chapters (5–11):

- **Conceptual Integrity:** Coherence of design — the architecture "hangs together" as a unified whole; consistency of design decisions across the system.
- **Buildability:** Ease with which the system can be built from its architecture; module structure supports parallel development and incremental integration.
- **Marketability (Time to Market):** How architecture enables rapid delivery; decisions about what to build vs. buy, reuse, or defer.
- **Interoperability:** Ability of systems to exchange data (syntactic) and interpret exchanged data (semantic). Maturity frameworks define five levels from no data sharing to full semantic interoperability.
- **Other QAs** surveyed: configurability, scalability (horizontal/vertical), safety, security (detailed in Ch 9), and deployability.

## 9. Key Takeaways

1. **QA requirements use a unified six-part scenario form** (source, stimulus, environment, artifact, response, response measure) — making them testable and comparable across attributes.
2. **Tactics are atomic design decisions** targeting single QA responses; patterns are bundles of tactics with built-in tradeoffs.
3. **Seven categories of design decisions** (allocation of responsibilities, coordination model, data model, resource management, mapping, binding time, choice of technology) provide a systematic framework for QA-driven design.
4. **Tradeoffs are the architect's central challenge** — QAs interact (almost everything hurts performance) and the right balance depends on business goals.
5. **Availability exemplifies the full QA workflow**: general scenario → concrete scenario → tactics (detect, recover, prevent) → design checklist across all seven categories.

## Related Notes

- [[Software Architecture Overview|Software Architecture Overview]]
- [[../01_Software_Requirements/Software Requirements Overview|Software Requirements Overview]]
- [[../03_Software_Design/Software Design Overview|Software Design Overview]]
- [[../08_Software_Quality/Software Quality Overview|Software Quality Overview]]
