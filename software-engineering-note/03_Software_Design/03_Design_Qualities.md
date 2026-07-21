---
tags: [design-qualities, concurrency, persistence, distribution, fault-tolerance, software-design, swebok]
---

# Design Qualities — Cross-Cutting Concerns

> *Source: SWEBOK v4 Chapter 03 — Software Design*

## Purpose

Design qualities are properties that span the entire system — not owned by any single component, but affecting every component's behavior. Getting them right requires intentional design; getting them wrong produces fragile, unscalable systems.

## Concurrency

**Definition:** Multiple computations executing simultaneously, competing for shared resources.

**Design issues:**
- **Race conditions** — Two operations interleave unexpectedly, producing wrong results
- **Deadlock** — Two or more threads wait forever for each other's resources
- **Livelock** — Threads keep responding to each other without making progress
- **Starvation** — A thread never gets access to needed resources

**Design approaches:**

| Approach | Description | When to Use |
|---|---|---|
| **Locking (mutex, semaphore)** | Serialize access to shared data | Simple shared state |
| **Message passing** | Communicate via queues/channels | Distributed systems, actors |
| **Immutable data** | Never modify — create new copies | Functional design, event sourcing |
| **STM (Software Transactional Memory)** | Atomic transactions on memory | Complex shared state |
| **Lock-free structures** | CAS-based concurrent data structures | High-performance requirements |

## Control and Event Handling

**Definition:** How the system responds to external and internal events.

**Design issues:**
- What events does the system handle?
- What's the priority of each event?
- How are events queued, prioritized, and dispatched?
- What happens when events arrive faster than processing?

**Design patterns:**
- **Observer/Pub-Sub** — Components subscribe to events they care about
- **Command pattern** — Encapsulate events as objects for queuing and undo
- **Saga pattern** — Distributed transactions via event chains with compensating actions
- **Event sourcing** — Store events, not state; rebuild state from event history
- **CQRS** — Separate read and write models for different event processing needs

## Data Persistence

**Definition:** How data survives system restarts — what's stored, where, and how.

**Design issues:**
- What data must persist vs. what's ephemeral?
- What's the access pattern (read-heavy, write-heavy, balanced)?
- What consistency guarantees are needed?
- How is data versioned and migrated?

**Design choices:**

| Choice | Trade-off |
|---|---|
| **Relational (SQL)** | Strong consistency, flexible queries, harder to scale horizontally |
| **Document (MongoDB)** | Flexible schema, good for read-heavy, eventual consistency |
| **Key-Value (Redis)** | Extremely fast, limited query capability |
| **Event Store** | Full audit trail, complex to query current state |
| **File/Blob Storage** | Cheap, slow, good for media/documents |

## Distribution of Components

**Definition:** How components are partitioned across processes, machines, and networks.

**Design issues:**
- **Latency** — Network calls are orders of magnitude slower than local calls
- **Partial failure** — Some components may be unreachable
- **Consistency** — Distributed data may be inconsistent across nodes
- **Discovery** — How do components find each other?
- **Serialization** — How are objects converted for network transport?

**Design approaches:**
- **Synchronous (REST, gRPC)** — Request-response, simple but tight coupling
- **Asynchronous (messaging, events)** — Loose coupling, eventual consistency
- **Replication** — Multiple copies for availability and read scaling
- **Sharding** — Partition data for write scaling
- **Circuit breaker** — Stop calling failing services to prevent cascading failures

## Error and Exception Handling

**Definition:** How the system detects, reports, recovers from, and prevents errors.

**Design issues:**
- What can fail? (Identify failure modes for each component)
- What's the failure strategy? (Fail fast, fail safe, graceful degradation)
- How are errors communicated? (Exceptions, error codes, Result types)
- How is the system monitored? (Logging, metrics, alerting)

**Design principles:**
- **Don't catch what you can't handle** — Let errors propagate to the right level
- **Use exceptions for exceptional conditions** — Not for normal control flow
- **Fail fast** — Detect problems early, don't let bad state propagate
- **Provide meaningful error messages** — What went wrong, where, and what to do
- **Design for recovery** — Retry, fallback, circuit breaker, bulkhead

## Essential Concepts

- **Concurrency requires intentional design** — it won't "just work."
- **Event-driven design is the default for distributed systems** — synchronous coupling doesn't scale.
- **Persistence choices have architectural consequences** — they're not just implementation details.
- **Distribution changes everything** — latency, partial failure, and consistency are the new design constraints.
- **Error handling is a design concern, not an afterthought** — plan failure modes during design.

## Related

- [[Software Design Note Overview]] — All design topics
- [[01_Design_Fundamentals_and_Principles]] — Core design principles
- [[../02_Software_Architecture/02_Quality_Attributes_Overview]] — Quality attribute framework
- [[../02_Software_Architecture/Microservice/04 Resilience/]] — Resilience patterns in practice
