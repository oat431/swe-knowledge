---
tags:
  - architecture
  - availability
  - interoperability
  - tactics
  - software-architecture
  - quality-attributes
---

# 03 — Availability & Interoperability

> **Source:** Bass, Clements & Kazman, *Software Architecture in Practice* (3rd ed.), Chapters 5–6
> **Purpose:** Cover the quality attributes of availability and interoperability — their general scenarios, tactics, design checklists, and how architects plan for failure and enable systems to exchange information meaningfully.

## What Is This?

These two chapters address quality attributes that sit at the heart of distributed and service-oriented systems. **Availability** is about the system being ready for use when needed — surviving faults so that service remains compliant with specification. **Interoperability** is about systems exchanging information *usefully* — beyond syntactic compatibility, it demands semantic understanding and managed interfaces.

The thread connecting both: systems don't exist in isolation. They fail, and they talk to each other. The architect's job is to design for both realities from the start, not patch them in later.

---

## Chapter 5 — Availability

Availability is the property that the system is operational and accessible when required. A **failure** occurs when the system no longer delivers a service consistent with its specification — this is observable by the system's actors. A **fault** (or combination of faults) has the *potential* to cause a failure. Availability tactics keep faults from becoming failures, or at least bound their effects and make repair possible.

### Planning for Failure

Failure is not an option — it's inevitable. The right design philosophy is to plan for failure and handle it with aplomb. Three established techniques for understanding what can go wrong:

#### Hazard Analysis
Catalogs hazards that can occur during operation and categorizes each by severity. Uses domain-specific severity scales (e.g., DO-178B for aeronautics: Catastrophic → Hazardous → Major → Minor → No Effect). Hazards whose probability × cost exceeds a threshold become subjects of mitigation.

#### Fault Tree Analysis (FTA)
A top-down analytical technique: specifies an undesired state (the "top event"), then analyzes the system's context to find all ways that state could occur. Uses Boolean logic gate symbols:
- **AND** — output fault occurs if all input faults occur
- **OR** — output fault occurs if at least one input fault occurs
- **n COMBINATION** — output fault occurs if n of the input faults occur
- **EXCLUSIVE OR** — exactly one input fault
- **PRIORITY AND** — all inputs occur in a specific sequence
- **INHIBIT** — input fault occurs in the presence of an enabling condition

Key concept: **minimal cut set** — the smallest combination of bottom-level events that can cause the top event. A singleton minimal cut set reveals a **single point of failure**, which demands scrutiny. FTA can be used statically (probabilistic analysis) or dynamically (Markov analysis for ordered failures).

#### Failure Mode, Effects, and Criticality Analysis (FMECA)
Catalogs failure modes for components of a given type based on historical data of similar systems. Each failure mode is classified as critical or noncritical with its probability. Summing critical probabilities yields overall failure probability. The NASA handbook warns: don't let form (filling tables) take priority over substance (finding what else can go wrong).

### 5.1 Availability General Scenario

| Portion | Possible Values |
|---|---|
| **Source** | Internal/external: people, hardware, software, physical infrastructure, physical environment |
| **Stimulus** | Fault: omission, crash, incorrect timing, incorrect response |
| **Artifact** | Processors, communication channels, persistent storage, processes |
| **Environment** | Normal operation, startup, shutdown, repair mode, degraded operation, overloaded operation |
| **Response** | **Prevent** the fault from becoming a failure; **Detect** the fault (log, notify); **Recover** (disable source, temporary unavailability, fix/mask/contain, degraded mode) |
| **Response Measure** | Availability percentage (e.g., 99.999%), time to detect, time to repair, time in degraded mode, proportion/rate of faults prevented or handled |

**Concrete scenario example:** The heartbeat monitor determines that the server is nonresponsive during normal operations. The system informs the operator and continues to operate with no downtime.

### 5.2 Tactics for Availability

Three categories: **Detect Faults**, **Recover from Faults**, **Prevent Faults**.

```
Availability Tactics
├── Detect Faults
│   ├── Ping/Echo
│   ├── Monitor (watchdog)
│   ├── Heartbeat
│   ├── Timestamp
│   ├── Sanity Checking
│   ├── Condition Monitoring
│   ├── Voting (TMR, replication, functional redundancy, analytic redundancy)
│   ├── Exception Detection (system exceptions, parameter fence, parameter typing, timeout)
│   └── Self-Test
├── Recover from Faults
│   ├── Preparation & Repair
│   │   ├── Active Redundancy (hot spare, 1+1)
│   │   ├── Passive Redundancy (warm spare)
│   │   ├── Spare (cold spare)
│   │   ├── Exception Handling
│   │   ├── Rollback (checkpoints, rollback line)
│   │   ├── Software Upgrade (function patch, class patch, hitless ISSU)
│   │   ├── Retry
│   │   ├── Ignore Faulty Behavior
│   │   ├── Degradation
│   │   └── Reconfiguration
│   └── Reintroduction
│       ├── Shadow
│       ├── State Resynchronization
│       ├── Escalating Restart
│       └── Non-Stop Forwarding (NSF)
└── Prevent Faults
    ├── Removal from Service (software rejuvenation)
    ├── Transactions (ACID, two-phase commit)
    ├── Predictive Model
    ├── Exception Prevention (smart pointers, wrappers)
    └── Increase Competence Set
```

#### Detect Faults — Key Tactics

| Tactic | Description |
|---|---|
| **Ping/Echo** | Async request/response pair between nodes; determines reachability, round-trip delay, and liveness. Requires a timeout threshold. |
| **Monitor** | Component that monitors health of processors, processes, I/O, memory. Can detect failure, congestion, or DoS attacks. Watchdog = monitor using a counter/timer periodically reset. |
| **Heartbeat** | Periodic message exchange between system monitor and monitored process. Difference from ping/echo: *who initiates* — here the monitor checks the component (or the component resets a watchdog timer). |
| **Timestamp** | Detects incorrect sequences of events in distributed message-passing systems. Simple sequence numbers can substitute when time info isn't needed. |
| **Sanity Checking** | Checks validity/reasonableness of operations or outputs, typically at interfaces. Based on knowledge of internal design or system state. |
| **Condition Monitoring** | Checks conditions in a process or validates design assumptions (e.g., checksums). The monitor must itself be simple and ideally provable. |
| **Voting** | Triple Modular Redundancy (TMR): three identical components, identical inputs, outputs compared by voting logic. Variants: **Replication** (exact clones — protects against random hardware failures, not design errors), **Functional Redundancy** (diversely designed/implemented — addresses common-mode failures), **Analytic Redundancy** (diverse inputs/outputs too — tolerates specification errors, common in avionics). |
| **Exception Detection** | System exceptions (divide-by-zero, bus faults), **parameter fence** (magic pattern `0xDEADBEEF` after variable-length params), **parameter typing** (strongly-typed TLV message building — trades evolution ease for availability), **timeout** (raises exception when timing constraints violated). |
| **Self-Test** | Components/subsystems run test procedures for correct operation. May use condition monitoring techniques like checksums. |

#### Recover from Faults — Key Tactics

| Tactic | Description |
|---|---|
| **Active Redundancy (Hot Spare)** | All nodes in protection group receive and process identical inputs in parallel. Redundant spare maintains synchronous state — can take over in milliseconds. Simple case: 1+1. |
| **Passive Redundancy (Warm Spare)** | Only active members process traffic; they send periodic state updates to spares. Loosely coupled state — balances availability, compute cost, and complexity. |
| **Spare (Cold Spare)** | Redundant spares stay out of service until failover; then power-on-reset before activation. Poor recovery performance — suited for high-reliability (MTBF) requirements, not high-availability. |
| **Exception Handling** | After detection, the system handles the exception (not crash). Ranges from error codes to rich exception classes carrying name, origin, and cause for fault correlation and masking. |
| **Rollback** | Revert to a previous known good state ("rollback line"). Checkpoints are stored and updated at regular intervals or at significant processing points. Often combined with redundancy. |
| **Software Upgrade** | In-service upgrades without service disruption: **function patch** (incremental linker/loader), **class patch** (OOP back-door for member data/functions), **hitless ISSU** (leverages active/passive redundancy for new features). |
| **Retry** | Assumes transient fault; retries the operation. Must have a limit on attempts before declaring permanent failure. Common in networks and server farms. |
| **Ignore Faulty Behavior** | Filter out spurious messages from a particular source (e.g., ACL filters for DoS attacks). |
| **Degradation** | Maintain most critical functions while dropping less critical ones when components fail. Graceful degradation rather than total failure. |
| **Reconfiguration** | Reassign responsibilities to remaining functioning resources while maintaining as much functionality as possible. |

#### Reintroduction — Key Tactics

| Tactic | Description |
|---|---|
| **Shadow** | Operate a previously failed or upgraded component in "shadow mode" before returning to active. Monitor behavior and repopulate state incrementally. |
| **State Resynchronization** | Active redundancy: states compared periodically via CRC or message digest (hash). Passive redundancy: based on periodic checkpointing from active to standby. Special case: stateless services where any resource can handle requests from a failed one. |
| **Escalating Restart** | Varying granularity of restart to minimize affected services. Level 0: kill/recreate child threads (least impact). Level 1: free/reinitialize unprotected memory. Level 2: free/reinitialize all memory. Level 3: complete reload of executable + data. Supports graceful degradation. |
| **Non-Stop Forwarding (NSF)** | From router design: split into control plane (routing) and data plane (forwarding). If control plane fails, data plane continues forwarding along known routes while routing info is recovered. |

#### Prevent Faults — Key Tactics

| Tactic | Description |
|---|---|
| **Removal from Service** | Temporarily take a component out of service to scrub latent faults (memory leaks, fragmentation, soft errors) — also called **software rejuvenation**. |
| **Transactions** | ACID semantics (Atomic, Consistent, Isolated, Durable) for async messages between distributed components. Two-phase commit (2PC) prevents race conditions on shared data. |
| **Predictive Model** | Monitor operational metrics (session rates, queue lengths, watermark crossings) to predict onset of faults and take corrective action before failure. |
| **Exception Prevention** | Smart pointers (bounds checking, auto-deallocation), wrappers to prevent dangling pointers and semaphore violations — preventing exceptions rather than handling them. |
| **Increase Competence Set** | A component's competence set = the set of states in which it can operate. When it raises an exception, it's outside that set. Increasing competence means designing it to handle more cases as normal operation (e.g., waiting for a blocked resource instead of throwing). |

### 5.3 Design Checklist for Availability

- **Allocation of Responsibilities:** Assign responsibilities for fault detection (omission, crash, timing, response) and response (log, notify, disable, mask, degrade).
- **Coordination Model:** Ensure coordination mechanisms detect faults, enable logging/notification/repair, support artifact replacement, and work under degraded conditions.
- **Data Model:** Identify data abstractions whose faults affect availability; ensure they can be disabled, made temporarily unavailable, or fixed/masked.
- **Mapping among Architectural Elements:** Ensure flexible mapping so processes, processors, channels, and storage can be reassigned at runtime. Consider redundancy mapping.
- **Resource Management:** Ensure sufficient resources remain to handle faults; size input queues to buffer messages during server failure.
- **Binding Time:** If late binding switches between fault-prone artifacts, ensure detection/recovery works for all bindings. Consider the availability of the binding mechanism itself.
- **Choice of Technology:** Evaluate technologies for fault detection, recovery, and reintroduction. Consider what faults the technologies themselves might introduce.

---

## Chapter 6 — Interoperability

Interoperability is about two or more systems exchanging information via interfaces — and the receiving party *understanding* that information both syntactically and semantically.

### Two Critical Concepts

1. **"Exchange information" is broad.** It includes direct program-to-program calls, indirect information flow through intermediaries, and even design-time assumptions about another system's behavior. The 1991 Patriot missile failure (28 fatalities) occurred because one component assumed periodic restarts for recalibration — an interface expectation never stated in any API but critical to correct operation.

2. **"Interface" means more than syntax.** An API is necessary but not sufficient. An interface is the full set of assumptions you can safely make about an entity — including behavioral expectations, timing, resource usage, and semantic meaning. Syntax compiles; semantics interoperates.

### Why Interoperate?

- Your system provides a service to unknown consumers (e.g., Google Maps)
- You construct capabilities from existing systems (sensing → processing → interpretation → distribution)

Two key aspects emerge: **discovery** (locating the service, possibly at runtime) and **handling of the response** (report back, forward to another system, or broadcast).

### Systems of Systems (SoS)

An SoS is an arrangement of independent and useful systems integrated into a larger system delivering unique capabilities.

| Type | Characteristics |
|---|---|
| **Directed** | Centralized management, funding, authority. Systems are subordinated to the SoS. |
| **Acknowledged** | Centralized objectives and management, but constituent systems retain their own management, funding, and authority in parallel. |
| **Collaborative** | No overall SoS-level authority. Systems voluntarily work together for shared interests. (e.g., Google Maps + individual applications) |
| **Virtual** | Like collaborative, but systems don't even know about each other. Highly ad hoc. (e.g., U.S. electric grid — 3,000+ companies, no single management authority) |

### 6.1 Interoperability General Scenario

| Portion | Possible Values |
|---|---|
| **Source** | A system initiates a request to interoperate with another system |
| **Stimulus** | A request to exchange information among system(s) |
| **Artifact** | The systems that wish to interoperate |
| **Environment** | Systems discovered at runtime OR known prior to runtime |
| **Response** | Request appropriately rejected + entities notified; Request accepted + information exchanged successfully; Request logged |
| **Response Measure** | Percentage of information exchanges correctly processed; Percentage correctly rejected |

**Concrete scenario example:** Our vehicle information system sends current location to the traffic monitoring system. The traffic monitoring system combines our location with other information, overlays it on a Google Map, and broadcasts it. Our location information is correctly included with a probability of 99.9%.

### SOAP vs. REST — Two Technology Options

| Aspect | SOAP / WS* | REST |
|---|---|---|
| **Model** | Protocol spec for XML-based info exchange; RPC-rooted but supports other models | Client-server architectural style; CRUD operations (POST, GET, PUT, DELETE) |
| **Addressing** | No mandated addressing model or procedural conventions | Single URI-based addressing scheme |
| **Type System** | Simple type system comparable to major languages | No type system — no type checking |
| **Standards** | Complete middleware stack: BPEL (composition), WS-AT/WS-BA (transactions), UDDI (discovery), WS-Reliability | Minimal standards; relies on HTTP |
| **Interoperability** | Buys syntactic interoperability only — applications must agree on payload interpretation | Buys syntactic interoperability via HTTP — semantic agreement still required at org level |
| **Message Size** | Larger (XML envelope overhead) | Smaller (fewer characters per exchange) |
| **Best For** | QoS-rich systems (security, transactions, reliability); structured enterprise integration | Read-only functionality, mashups, minimal QoS requirements |
| **Key Tradeoff** | Completeness and standardization | Simplicity and performance |

> The truth: you don't have to choose once for all time. Each is easy to use for simple applications. The decision hinges on tradeoffs in your specific context.

### 6.2 Tactics for Interoperability

Two categories: **Locate** and **Manage Interfaces**.

```
Interoperability Tactics
├── Locate
│   └── Discover Service
└── Manage Interfaces
    ├── Orchestrate
    └── Tailor Interface
```

#### Locate

| Tactic | Description |
|---|---|
| **Discover Service** | Locate a service through searching a known directory service. Multiple levels of indirection possible. Can search by type, name, location, or other attributes. Used when interoperating systems are discovered at runtime. |

#### Manage Interfaces

| Tactic | Description |
|---|---|
| **Orchestrate** | Uses a control mechanism to coordinate, manage, and sequence the invocation of particular services (which may be ignorant of each other). "Scripts" complex interactions. Examples: workflow engines, BPEL, the Mediator design pattern for simple orchestration. |
| **Tailor Interface** | Adds or removes capabilities to an interface. Add: translation, buffering, smoothing data. Remove: hide functions from untrusted users. The Decorator pattern is an example. |

The **Enterprise Service Bus (ESB)** combines both orchestrate and tailor interface tactics.

### Why Standards Are Not Enough

A system sending a price as `decimal(2)` over SOAP/HTTP achieves syntactic interoperability — but if one system assumes price *includes* tax and the other assumes it *excludes* tax, the systems fail to interoperate. Standards guarantee syntax, not semantics.

Challenges with standards:
1. Vendor customizations and extensions break identical implementations
2. Standards are deliberately open-ended with proprietary extension points
3. Standards evolve in compatible and noncompatible ways — timing adoption is risky
4. Bad standards exist: underspecified, overspecified, inconsistently specified, unstable, irrelevant
5. Competing standards bodies create conflicting standards
6. Premature standardization can hinder flexibility in rapidly emerging domains

> **Key insight:** We cannot let standards drive architectures. Architect systems first, then decide which standards support desired requirements and qualities. This allows standards to change without affecting the overall architecture.

### 6.3 Design Checklist for Interoperability

- **Allocation of Responsibilities:** Assign responsibilities for detecting, accepting, exchanging, rejecting, notifying, and logging (nonrepudiation is essential in untrusted environments) interoperability requests.
- **Coordination Model:** Consider traffic volume, timeliness, currency, and jitter of messages. Ensure all systems under your control make consistent assumptions about protocols and networks.
- **Data Model:** Determine syntax and semantics of exchanged data abstractions. If your data model is confidential, apply transformations to/from external data abstractions.
- **Mapping among Architectural Elements:** Critical mapping is components to processors — ensure components that communicate externally are hosted on network-reachable processors meeting security, availability, and performance requirements.
- **Resource Management:** Ensure interoperation (accepting/rejecting requests) can never exhaust critical resources (e.g., flood of requests causing denial of service). Ensure communication load and shared resource arbitration are acceptable.
- **Binding Time:** Determine when interoperating systems become known. Have policies for both known and unknown external systems. Support late binding with discovery mechanisms.
- **Choice of Technology:** Are chosen technologies "visible" at the interface boundary? Do they support, undercut, or have no effect on interoperability scenarios? Consider web services and other interoperability-supporting technologies.

---

## My Notes

- Availability tactics are often provided by middleware — the architect's job is **choosing and assessing** the right combination, not implementing from scratch.
- All availability tactics involve the coordination model because it must be aware of faults to generate appropriate responses.
- The heartbeat vs. ping/echo distinction (who initiates the health check) is subtle but important for system design.
- Active redundancy (hot spare) is the gold standard but expensive; passive (warm spare) balances cost and availability; cold spare is for reliability, not availability.
- Escalating restart pairs naturally with graceful degradation — restart only what's needed, preserve what's working.
- Interoperability is not just about technology choice (SOAP vs. REST) — it's about semantic agreement between systems.
- Discovery (locating services) and response handling (report/forward/broadcast) are the two pillars of interoperability design.
- The "system of systems" taxonomy (directed → virtual) maps loosely to organizational control — the less control, the more you rely on standards and discovery.

## What's Missing

- Concrete worked examples of fault trees with probabilities
- Detailed walkthrough of ATAM applied to availability scenarios
- Comparison of availability tactics with modern patterns (circuit breakers, bulkheads, retry with backoff)
- gRPC, GraphQL, and async messaging as modern interoperability alternatives to SOAP/REST
- Semantic interoperability standards (RDF, OWL, JSON-LD, schema registries)
- API versioning and compatibility strategies as an interoperability concern
- Real-world availability numbers from cloud providers and how they're measured


## Related

- [[Software Architecture Overview]] — All architecture topics
- [[02_Quality_Attributes_Overview]] — QA scenarios framework
- [[06_Tactics_and_Patterns]] — Related architecture patterns
