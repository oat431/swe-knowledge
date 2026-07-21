---
tags:
  - architecture
  - modifiability
  - performance
  - tactics
  - software-architecture
---

# Modifiability & Performance

> **Source:** Bass, Clements & Kazman — *Software Architecture in Practice* (SAiP), Chapters 7–8
> **Purpose:** Cover the general scenarios, tactics, and design checklists for modifiability and performance — two quality attributes that are often in tension with each other.

## Chapter 7: Modifiability

Modifiability is about **change** and the **cost** (time, money, side effects) of making a change. The question isn't just *can* the system be changed, but *at what cost* and *with what ripple effects*.

### Modifiability General Scenario

| Portion | Possible Values |
|---|---|
| **Source** | End user, developer, system administrator |
| **Stimulus** | Directive to add/delete/modify functionality, or change a quality attribute, capacity, or technology |
| **Artifacts** | Code, data, interfaces, components, resources, configurations, … |
| **Environment** | Runtime, compile time, build time, initiation time, design time |
| **Response** | Make modification, test modification, deploy modification |
| **Response Measure** | Number/size/complexity of affected artifacts; effort; calendar time; money; extent of side effects; new defects introduced |

**Example concrete scenario:** A developer wishes to change the UI by modifying code at design time. Modifications are made with no side effects, within three hours.

### Tactics for Modifiability

The goal is to control the **complexity, time, and cost** of making changes. Tactics operate on four parameters:

1. **Size of a module** — smaller modules cost less to change
2. **Coupling** — lower coupling reduces propagation of changes
3. **Cohesion** — higher cohesion means changes affect fewer responsibilities
4. **Binding time** — deferring binding to later lifecycle phases reduces change cost

```
Modifiability Tactics
├── Reduce Size of a Module
│   └── Split Module
├── Increase Cohesion
│   └── Increase Semantic Coherence
├── Reduce Coupling
│   ├── Encapsulate
│   ├── Use an Intermediary
│   ├── Restrict Dependencies
│   ├── Refactor
│   └── Abstract Common Services
└── Defer Binding
    ├── Compile-time / Build-time
    │   ├── Component replacement (makefile)
    │   ├── Compile-time parameterization
    │   └── Aspects
    ├── Deployment-time
    │   └── Configuration-time binding
    ├── Startup / Init-time
    │   └── Resource files
    └── Runtime
        ├── Runtime registration
        ├── Dynamic lookup (service discovery)
        ├── Interpret parameters
        ├── Name servers
        ├── Plug-ins
        ├── Publish-subscribe
        ├── Shared repositories
        └── Polymorphism
```

#### Reduce the Size of a Module

- **Split module.** Refine a large module into several smaller modules to reduce the average cost of future changes. Splits should reflect the types of changes most likely to occur.

#### Increase Cohesion

- **Increase semantic coherence.** If two responsibilities A and B in a module don't serve the same purpose, place them in different modules. Hypothesize likely changes; if some responsibilities are unaffected by those changes, move them out.

#### Reduce Coupling

- **Encapsulate.** Introduce an explicit interface (API) to a module. Reduces the probability that a change to the module propagates to others. The interface limits how external responsibilities can interact with the module (perhaps through a wrapper). Interfaces should be abstract with respect to details likely to change.

- **Use an intermediary.** Break a dependency between responsibility A and B by introducing an intermediary. The type depends on the dependency: publish-subscribe for data producer/consumer, shared data repository for readers/writers, directory service for service discovery in SOA.

- **Restrict dependencies.** Restrict which modules a given module can interact with or depend on. Achieved by limiting visibility (developers can't use what they can't see) and authorization (restrict access). Seen in layered architectures and wrappers.

- **Refactor.** When two modules are affected by the same change because they're partial duplicates, factor common responsibilities into a shared home. Co-locating common responsibilities reduces coupling.

- **Abstract common services.** When two modules provide similar-but-not-identical services, implement once in a more general (abstract) form. Parameterize the description and implementation. Parameters can range from simple variable values to statements in a specialized interpreted language.

#### Defer Binding

Let computers handle change as much as possible — exercising built-in flexibility is cheaper than hand-coding. The later in the lifecycle we bind values, the better, *provided the mechanism that enables late binding is cost-effective*.

This separates **building the mechanism** (developer) from **using the mechanism** (installer, user) — called **externalizing the change**.

### Design Checklist for Modifiability

| Category | Key Considerations |
|---|---|
| **Allocation of Responsibilities** | Identify likely changes; place responsibilities changed together in the same module; separate those changed at different times |
| **Coordination Model** | Use coordination models that reduce coupling (pub-sub), defer bindings (ESB), or restrict dependencies (broadcast) |
| **Data Model** | Minimize number and severity of modifications to data abstractions; items likely to change together should be allocated together |
| **Mapping among Architectural Elements** | Use deferred binding for mapping decisions (execution dependencies, DB assignment, process/thread assignment) |
| **Resource Management** | Encapsulate all resource managers and their policies; defer bindings to the extent possible |
| **Binding Time** | Choose latest feasible binding time; evaluate cost of introducing mechanism vs. cost of making changes; don't introduce so many binding choices that dependencies become complex and unknown |
| **Choice of Technology** | Will technology choices help make/test/deploy modifications? How easy is it to modify technology choices? Choose to support most likely modifications |

---

## Chapter 8: Performance

Performance is about **time** — the system's ability to meet timing requirements. When events occur (interrupts, messages, requests, clock events), the system must respond within acceptable time constraints.

Performance is often linked to **scalability** — increasing capacity for work while still performing well. Technically, scalability is a special kind of modifiability (changing the system to handle more load).

### Performance General Scenario

| Portion | Possible Values |
|---|---|
| **Source** | Internal or external to the system |
| **Stimulus** | Arrival of a periodic, stochastic, or sporadic event |
| **Artifact** | System or one or more components |
| **Environment** | Operational mode: normal, emergency, peak load, overload |
| **Response** | Process events, change level of service |
| **Response Measure** | Latency, deadline, throughput, jitter, miss rate |

**Event arrival patterns:**
- **Periodic:** Predictable, regular intervals (e.g., every 10 ms — real-time systems)
- **Stochastic:** Probabilistic distribution
- **Sporadic:** Neither periodic nor stochastic, but can be characterized (e.g., max 600 events/min, min 200 ms between events)

**Response measures:**
- **Latency:** Time between stimulus arrival and system response
- **Deadlines:** Processing must complete by a specific time (e.g., fuel ignition at cylinder position)
- **Throughput:** Number of transactions per unit of time
- **Jitter:** Allowable variation in latency
- **Miss rate:** Number of events not processed because the system was too busy

**Example concrete scenario:** Users initiate transactions under normal operations. The system processes the transactions with an average latency of two seconds.

### Response Time Contributors

At any instant after an event arrives but before the response is complete, the system is either:

1. **Processing** (consuming resources → takes time)
2. **Blocked** (unable to respond due to contention, unavailability, or dependency)

#### Processing Time

Processing consumes resources. Different resources degrade differently as they approach saturation:
- **CPU:** Performance degrades fairly steadily as load increases
- **Memory:** When page swapping becomes overwhelming, performance crashes suddenly

#### Blocked Time

- **Contention for resources.** Resources used by a single client at a time force others to wait. Multiple streams vying for the same resource increase latency.
- **Availability of resources.** Computation can't proceed if a resource is offline, failed, or otherwise unavailable.
- **Dependency on other computation.** Waiting for synchronization with another computation's results, or for a response from a remote component (network latency).

### Tactics for Performance

Two broad categories: reduce demand or manage resources better.

```
Performance Tactics
├── Control Resource Demand
│   ├── Manage Sampling Rate
│   ├── Limit Event Response
│   ├── Prioritize Events
│   ├── Reduce Overhead
│   ├── Bound Execution Times
│   └── Increase Resource Efficiency
└── Manage Resources
    ├── Increase Resources
    ├── Introduce Concurrency
    ├── Maintain Multiple Copies of Computations
    ├── Maintain Multiple Copies of Data
    ├── Bound Queue Sizes
    └── Schedule Resources
```

#### Control Resource Demand

- **Manage sampling rate.** Reduce the frequency at which environmental data is captured. Common in signal processing (different codecs, different sampling rates). Tradeoff: lower fidelity for consistent latency.

- **Limit event response.** Process events only up to a set maximum rate when they arrive too rapidly. Requires a policy for handling overflow: log dropped events? notify? If no events can be lost, queues must handle worst case.

- **Prioritize events.** Rank events by importance. Low-priority events may be ignored when resources are scarce, improving performance for critical events. Example: fire alarm > "room too cold" in building management.

- **Reduce overhead.** Remove intermediaries (classic modifiability/performance tradeoff). Co-locate cooperating components on the same processor, or in the same runtime component. Execute single-threaded servers for simplicity, splitting workload across them. Periodically clean up resources (hash tables, virtual memory maps).

- **Bound execution times.** Limit how much execution time is used per event. For iterative algorithms, limit iterations. Cost: less accurate computation. Must assess if result is "good enough."

- **Increase resource efficiency.** Improve algorithms used in critical areas to decrease latency.

#### Manage Resources

- **Increase resources.** Faster processors, more processors, more memory, faster networks. Often the cheapest way to get immediate improvement, though cost is a factor.

- **Introduce concurrency.** Process different event streams on different threads, or create additional threads for different activities. Once concurrency exists, apply scheduling policies (fairness, throughput, etc.). **Critical:** concurrency requires careful management of shared state to prevent race conditions.

- **Maintain multiple copies of computations.** Replicate servers (client-server pattern); use a load balancer to assign work (round-robin, least-busy). Reduces contention.

- **Maintain multiple copies of data.** **Caching** — keep copies on storage with different access speeds (memory vs. disk, local vs. remote). **Data replication** — separate copies to reduce contention. Responsibility: keep copies consistent and synchronized. Can predict future requests and prefetch.

- **Bound queue sizes.** Control maximum queued arrivals and resources used. Requires a policy for queue overflow and whether losing events is acceptable. Frequently paired with *limit event response*.

- **Schedule resources.** When there is contention, resources must be scheduled (processors, buffers, networks). Choose a scheduling strategy compatible with each resource's characteristics.

### Scheduling Policies

A scheduling policy has two parts: **priority assignment** and **dispatching**. Competing criteria: optimal resource usage, request importance, minimizing resources/latency, maximizing throughput, preventing starvation.

| Policy | Description |
|---|---|
| **FIFO** | All requests equal; satisfied in order. Problem: low-priority request stuck behind long-running one |
| **Fixed-priority: Semantic importance** | Static priority based on domain characteristic of the task |
| **Fixed-priority: Deadline monotonic** | Higher priority to streams with shorter deadlines (real-time systems) |
| **Fixed-priority: Rate monotonic** | Higher priority to streams with shorter periods (special case of deadline monotonic) |
| **Dynamic: Round-robin** | Orders requests, assigns resource to next in order at each opportunity |
| **Dynamic: Earliest-deadline-first** | Highest priority to pending requests with earliest deadline |
| **Dynamic: Least-slack-first** | Highest priority to job with least slack time (time remaining minus time to deadline) |
| **Static: Cyclic executive** | Preemption points and assignment sequence determined offline; zero runtime scheduler overhead |

For a single processor with preemptible processes, both **earliest-deadline-first** and **least-slack-first** are optimal — if any schedule can meet all deadlines, these strategies will.

### Design Checklist for Performance

| Category | Key Considerations |
|---|---|
| **Allocation of Responsibilities** | Identify heavy-load, time-critical, or heavily-used responsibilities; identify bottlenecks; plan thread management and scheduling responsibilities |
| **Coordination Model** | Choose mechanisms that support concurrency (thread-safe), event prioritization, and scheduling; match communication properties (stateful/stateless, sync/async, guaranteed delivery) |
| **Data Model** | Consider multiple copies of key data; partition data; reduce processing requirements for CRUD operations; add resources to reduce bottlenecks |
| **Mapping among Architectural Elements** | Co-locate components under heavy network load; assign heavy computation to highest-capacity processors; introduce concurrency where feasible and beneficial |
| **Resource Management** | Monitor and manage critical resources under normal and overload; process/thread models; prioritization; scheduling and locking; deploy additional resources on demand |
| **Binding Time** | Evaluate time to complete late binding and overhead introduced; ensure no unacceptable performance penalties |
| **Choice of Technology** | Can you set hard real-time deadlines? Know characteristics under load and limits? Can you set scheduling policy, priorities, demand-reduction policies? Does it introduce excessive overhead? |

---

## Key Tradeoffs

- **Modifiability vs. Performance:** Intermediaries (pub-sub, service directories, wrappers) improve modifiability but add overhead. Separation of concerns increases components in an event chain. Co-location improves performance but reduces modifiability.

- **Late binding vs. Performance:** Deferring binding (runtime service discovery, interpreted parameters) increases flexibility but adds runtime overhead.

- **Fidelity vs. Performance:** Managing sampling rate, bounding execution times, and limiting event response all trade accuracy/fidelity for predictable performance.

## Summary

| Quality Attribute | Core Concern | Key Tactic Families | Response Measure |
|---|---|---|---|
| **Modifiability** | Cost of change | Reduce size, increase cohesion, reduce coupling, defer binding | Effort, time, money, side effects |
| **Performance** | Time to respond | Control resource demand, manage resources | Latency, throughput, deadlines, jitter, miss rate |

## My Notes

- [[Software Architecture Overview|Software Architecture Overview]]
