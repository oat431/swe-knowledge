---
tags:
  - architecture
  - patterns
  - tactics
  - reference
  - software-architecture
source: Bass, Clements & Kazman — Software Architecture in Practice, 3rd Edition, Chapter 13
aliases:
  - SAiP Ch13
  - Architectural Tactics and Patterns
  - Architecture Patterns Catalog
---

# 06 — Tactics and Patterns

> *"Tactics are atoms and patterns are molecules."*

## Overview

**Architectural tactics** are simpler than patterns: they typically use a single structure or computational mechanism to address a single architectural force. **Architectural patterns** package multiple design decisions (tactics) together. Tactics give more precise control; patterns give more comprehensive solutions. Most patterns consist of several different tactics.

---

## 13.1 Architectural Patterns

An architectural pattern establishes a relationship among three things:

| Element | Description |
|---------|-------------|
| **Context** | A recurring, common situation in the world that gives rise to a problem |
| **Problem** | The problem (generalized), its variants, complementary and opposing forces. Often includes quality attributes that must be met |
| **Solution** | The architectural resolution — element types, connectors/interaction mechanisms, topological layout, and semantic constraints |

The solution is determined by:
- A set of **element types** (e.g., data repositories, processes, objects)
- A set of **interaction mechanisms or connectors** (e.g., method calls, events, message bus)
- A **topological layout** of the components
- A set of **semantic constraints** covering topology, element behavior, and interaction mechanisms

> Complex systems exhibit **multiple patterns at once**. A web-based system might use three-tier client-server, but within it also use replication, proxies, caches, firewalls, MVC, etc. — each of which may employ further patterns and tactics.

---

## 13.2 Patterns Catalog

Patterns are categorized by the **dominant type of elements** they show:

- **Module patterns** — show modules
- **Component-and-Connector (C&C) patterns** — show components and connectors at runtime
- **Allocation patterns** — combine software elements with non-software elements

---

### Module Patterns

#### Layered Pattern

| Aspect | Detail |
|--------|--------|
| **Context** | Complex systems need to develop and evolve portions independently. Developers need clear separation of concerns |
| **Problem** | Segment software so that modules can be developed and evolved separately with little interaction, supporting portability, modifiability, and reuse |
| **Solution** | Divide software into **layers** — groupings of modules that offer cohesive sets of services. The allowed-to-use relation is **unidirectional** (upper layers use lower layers; never vice versa). Layers completely partition the software; each partition is exposed through a public interface |

**Key Constraints:**
- Every piece of software is allocated to exactly one layer
- At least two layers (usually three or more)
- Allowed-to-use relations must not be circular
- Layer bridging (using non-adjacent lower layers) may be allowed but erodes modifiability
- **Upward calls** are allowed if the caller doesn't depend on the answer (e.g., callbacks for error handling)

**Weaknesses:** Up-front cost and complexity; performance penalty (context switching through layers)

> **Layers ≠ Tiers.** Layering is a module pattern (implementation units); tiers apply only to runtime entities.

##### Finer Points of Layers (PCC sidebar)

1. A stack-of-boxes diagram **must include a key** specifying whether layer bridging is allowed — otherwise the diagram is ambiguous
2. A set of boxes with arrows showing everything-is-allowed-to-use-everything is **not** a layered architecture
3. "Sidecar" layers (common utilities beside the main stack) are valid but must have explicit usage rules
4. Segmented layers need explicit inter-segment usage rules

---

### Component-and-Connector Patterns

#### Broker Pattern

| Aspect | Detail |
|--------|--------|
| **Context** | Systems constructed from services distributed across multiple servers; need for interoperation, dynamic binding |
| **Problem** | Structure distributed software so service users don't need to know the nature and location of service providers |
| **Solution** | Insert an intermediary **broker** between clients and servers. Client queries broker → broker forwards request to server → server processes → result returns via broker |

**Elements:**
- **Client** — requester of services
- **Server** — provider of services
- **Broker** — intermediary that locates servers, forwards requests, returns results
- **Client-side proxy** — manages communication (marshaling/sending/unmarshaling)
- **Server-side proxy** — manages communication (marshaling/sending/unmarshaling)

**Benefits:** Modifiability (use-an-intermediary tactic), availability (easy replacement of failed servers), performance (load distribution)

**Weaknesses:** Added complexity, latency (indirection), single point of failure, communication bottleneck, security attack surface, difficult to test

> First widely used implementation: CORBA. Also found in EJB, .NET platform, and any SOA ESB.

---

#### Model-View-Controller (MVC) Pattern

| Aspect | Detail |
|--------|--------|
| **Context** | User interface software is the most frequently modified portion of interactive applications; users need multiple views of same data |
| **Problem** | Keep UI functionality separate from application functionality; allow multiple coordinated views |
| **Solution** | Split into three component types: **Model** (application data), **View** (displays data, interacts with user), **Controller** (mediates, translates user actions, manages notifications) |

**Elements:**
- **Model** — encapsulates application state, responds to queries, exposes functionality, notifies views of changes
- **View** — renders models, requests updates, sends user gestures to controller
- **Controller** — defines application behavior, maps user actions to model updates, selects view for response

**Relations:** Connected via **notifications** (events or callbacks) — either push or pull. Model and view/controller are loosely coupled.

**Constraints:**
- At least one instance each of model, view, and controller
- Model should not interact directly with the controller
- Multiple views and controllers may be associated with one model

**Weaknesses:** Complexity not justified for simple UIs; conceptual mismatch with some UI toolkits (view and controller split input/output, but widgets often combine both)

> Widely used in: Java Swing, ASP.NET, Adobe Flex, Nokia Qt. A single application often contains many MVC instances (one per UI object).

---

#### Pipe-and-Filter Pattern

| Aspect | Detail |
|--------|--------|
| **Context** | Systems that transform streams of discrete data items from input to output; transformations occur repeatedly |
| **Problem** | Divide the system into reusable, loosely coupled components with simple generic interaction, supporting flexible combination and parallel execution |
| **Solution** | Successive transformations of data streams. Data arrives at a **filter's** input port, is transformed, and passes through a **pipe** to the next filter |

**Elements:**
- **Filter** — component that transforms data on input port(s) to data on output port(s). Can execute concurrently, can incrementally transform
- **Pipe** — connector that conveys data between filters. Single source, single target. Preserves sequence, does not alter data. Has buffer size, protocol, transmission speed

**Constraints:**
- Pipes connect filter output ports to filter input ports
- Connected filters must agree on data type
- Specializations may restrict to acyclic graph or linear sequence (pipeline)
- Named ports (stdin, stdout, stderr in UNIX)

**Key Properties:**
- Filters don't know identity of upstream/downstream filters
- Overall computation = functional composition of filter computations (easier end-to-end reasoning)
- Pipes buffer data → filters execute asynchronously and concurrently

**Weaknesses:** Not good for interactive systems (disallows cycles); large numbers of independent filters add computational overhead; not appropriate for long-running computations without checkpoint/restore

> Examples: UNIX pipes, Apache web server request processing, map-reduce, Yahoo! Pipes, workflow engines, scientific computation

---

#### Client-Server Pattern

| Aspect | Detail |
|--------|--------|
| **Context** | Shared resources and services that many distributed clients wish to access; control over access or quality of service is needed |
| **Problem** | Promote modifiability and reuse by factoring out common services; improve scalability and availability by centralizing control while distributing resources across multiple physical servers |
| **Solution** | **Clients** initiate interactions by requesting services of **servers**. Connector: request/reply protocol. Components may act as both client and server |

**Elements:**
- **Client** — invokes services; has ports describing required services
- **Server** — provides services; has ports describing provided services
- **Request/reply connector** — data connector with request/reply protocol (local or remote, possibly encrypted)

**Constraints:**
- Clients connected to servers through request/reply connectors
- Server components can be clients to other servers
- May be arranged in **tiers** (logical groupings of related functionality)

**Computational flow:** Asymmetric — clients initiate; servers don't know clients in advance. Usually synchronous (client blocks) but variants may be asynchronous.

**Weaknesses:** Server can be performance bottleneck; server can be single point of failure; location decisions (client vs. server) complex and costly to change

> The World Wide Web (HTTP) is the best-known example. HTTP is a stateless request/reply protocol.

---

#### Peer-to-Peer (P2P) Pattern

| Aspect | Detail |
|--------|--------|
| **Context** | Distributed entities of equal importance, each providing its own resources, need to cooperate to provide a service to a distributed community |
| **Problem** | Connect "equal" distributed entities via a common protocol so they can organize and share services with high availability and scalability |
| **Solution** | Components directly interact as **peers**. All peers are "equal" — no peer or group is critical for system health. Any component can interact with any other. Each peer is both client and server |

**Key Characteristics:**
- Bidirectional interactions (two-way communication)
- Peers discover each other on the network, then cooperate
- Search may propagate peer-to-peer with limited hops
- **Supernodes** — specialized peers with indexing/routing capabilities
- Peers can be added/removed with no significant impact → great scalability
- Multiple peers have overlapping capabilities → improved availability
- Load distributed across peers → performance advantages

**Weaknesses:** Decentralization makes security, data consistency, availability, backup, and recovery more complex. Small P2P systems may not consistently achieve quality goals (guarantees become probabilistic).

> Examples: BitTorrent, eDonkey (file sharing), Skype (VoIP/IM), Gnutella

---

#### Service-Oriented Architecture (SOA) Pattern

| Aspect | Detail |
|--------|--------|
| **Context** | Services are offered by providers and consumed by consumers; consumers need to use services without knowledge of implementation |
| **Problem** | Support interoperability of distributed components running on different platforms, written in different languages, provided by different organizations |
| **Solution** | Collection of distributed components that provide and/or consume services. Services are largely standalone, independently deployed. Quality attributes can be specified via **Service-Level Agreements (SLAs)** |

**Elements:**
- **Service providers** — provide services through published interfaces (with SLAs)
- **Service consumers** — invoke services directly or through intermediaries
- **Enterprise Service Bus (ESB)** — intermediary that routes/transforms messages, converts protocols, performs security checks, manages transactions
- **Service registry** — allows runtime service registration and discovery
- **Orchestration server** — coordinates interactions based on business process/workflow languages

**Connector Types:**
| Connector | Description |
|-----------|-------------|
| **SOAP** | Synchronous XML request/reply over HTTP |
| **REST** | Non-blocking HTTP requests using POST/GET/PUT/DELETE |
| **Asynchronous messaging** | Fire-and-forget; point-to-point or publish-subscribe |

**Benefits:** Interoperability is the main driver. Integrates legacy systems and external services. Dynamic reconfiguration via registry/ESB.

**Weaknesses:** Complex to build; don't control evolution of independent services; middleware performance overhead; services typically don't provide performance guarantees

> Shares many concepts with the broker pattern but adds registries, orchestration, SLAs, and standardized connectors.

---

#### Publish-Subscribe Pattern

| Aspect | Detail |
|--------|--------|
| **Context** | Independent producers and consumers of data that must interact; number and nature are not predetermined |
| **Problem** | Transmit messages among producers and consumers such that they are unaware of each other's identity or existence |
| **Solution** | Components interact via **announced messages (events)**. Components subscribe to events; the runtime infrastructure delivers each published event to all subscribers via an **event bus** |

**Constraints:**
- All components connected to an event distributor (bus or component)
- Publish ports → announce roles; subscribe ports → listen roles
- A component may be both publisher and subscriber
- May restrict which components can listen to which events

**Variants:**

| Variant | Description |
|---------|-------------|
| **List-based** | Each publisher maintains a subscription list. Less decoupled, more efficient, no single point of failure |
| **Broadcast-based** | Publishers broadcast events; subscribers (or their proxies) filter. Potentially inefficient with many messages |
| **Content-based** | Events delivered based on attribute matching against subscriber-defined patterns (more general than topic-based) |

**Weaknesses:** Added latency (indirection); less control over message ordering; delivery not guaranteed (sender can't know if receiver is listening); event bus may be single point of failure

> Examples: GUI event handling, MVC notifications, ERP systems, extensible programming environments (Eclipse), mailing lists, social network notifications

---

#### Shared-Data Pattern

| Aspect | Detail |
|--------|--------|
| **Context** | Multiple computational components need to share and manipulate large amounts of persistent data not belonging solely to any one component |
| **Problem** | Store and manipulate persistent data accessed by multiple independent components |
| **Solution** | Interaction dominated by exchange of persistent data between multiple **data accessors** and at least one **shared-data store**. Connector: data reading and writing |

**Data Store Responsibilities:** Shared access, data persistence, concurrent access (transaction management), fault tolerance, access control, distribution and caching

**Specializations:** Relational, object structures, layered, hierarchical structures

**Weaknesses:** Data store may be performance bottleneck; single point of failure; producers and consumers may be tightly coupled through knowledge of data structure

**When Multiple Stores Exist:** Key concern is mapping of data and computation to stores (natural/historical partitioning, or replication for performance/availability)

> Consolidating data in shared stores decouples producers from consumers → supports modifiability; facilitates performance tuning.

---

### Allocation Patterns

#### Map-Reduce Pattern

| Aspect | Detail |
|--------|--------|
| **Context** | Need to quickly analyze enormous volumes of data (petabyte scale) — logs, document repositories, web link pairs |
| **Problem** | Efficiently perform a distributed and parallel sort of a large data set and provide a simple means for the programmer to specify the analysis |
| **Solution** | Three parts: (1) specialized infrastructure for allocating software to hardware nodes and sorting data, (2) programmer-coded **map** function, (3) programmer-coded **reduce** function |

**Map Function:**
- Input: key1 + data set
- Purpose: filter and sort the data
- Output: `<key2, value>` pairs (key2 used for sorting)
- Multiple map instances process different portions in parallel

**Reduce Function:**
- Input: all `<key2, value>` pairs from all map instances in sorted order
- Purpose: programmer-specified analysis
- Output: smaller result set (hence "reduce")
- May have multiple reduce stages

**Infrastructure Responsibilities:** Assignment to hardware nodes, recovery/reassignment on hardware failure, sorting of massive intermediate lists

**Constraints:**
- Data must exist as a set of files
- Map functions are stateless and don't communicate with each other
- Only communication between map and reduce: `<key, value>` pairs

**Weaknesses:** Overhead not justified for small data sets; requires similarly sized data subsets for parallelism; multiple reduces are complex to orchestrate

> Cornerstone of Google, Facebook, eBay, Yahoo! data processing. Foundation of the NoSQL movement.

---

#### Multi-tier Pattern

| Aspect | Detail |
|--------|--------|
| **Context** | Distributed deployment — need to distribute infrastructure into distinct subsets for operational or business reasons |
| **Problem** | Split the system into computationally independent execution structures (groups of software + hardware) connected by communications media |
| **Solution** | Organize execution structures as logical groupings of components called **tiers**. Grouping criteria: component type, shared execution environment, same runtime purpose |

**Key Properties:**
- Applied to any collection of runtime components (most often client-server)
- **Topological constraints:** connectors only between same-tier or adjacent-tier components
- May constrain communication types across tiers (e.g., call-return one direction, event-based notification the other)
- Enhances security, performance optimization, availability, and modifiability

**Weaknesses:** Substantial up-front cost and complexity; not justified for simple systems

> **Tiers ≠ Layers!** Tiers apply to runtime entities; layers apply to modules (implementation units).

**Other Allocation Patterns:** Tiered Distribution (Microsoft), WebSphere topologies (IBM — 11 deployment patterns), work assignment patterns (Platform, Competence Center, Open Source for distributed Agile teams)

---

## 13.3 Relationships Between Tactics and Patterns

### Patterns Comprise Tactics

**Tactics are the building blocks** from which architectural patterns are created. Most patterns consist of several different tactics, often chosen to promote different quality attributes (e.g., a tactic that makes an availability pattern more secure).

**Example — Layered Pattern as Amalgam of Tactics:**

| Tactic | Role in Layered Pattern |
|--------|------------------------|
| **Increase semantic coherence** | Ensure a layer's responsibilities work together without excessive reliance on other layers |
| **Abstract common services** | Each layer provides a cohesive set of services through a public interface |
| **Encapsulate** | Layers hide internal implementation; only public interface is exposed |
| **Restrict communication paths** | Unidirectional allowed-to-use; reduces possible communication paths to (#layers − 1) |
| **Use an intermediary** | Each layer acts as intermediary between the layer above and the layer below |

Without any one of these tactics, the pattern might be ineffective — e.g., removing "restrict dependencies" destroys low coupling; removing "increase semantic coherence" destroys separation of concerns.

**Table: Patterns and Their Modifiability Tactics** (from Bachmann 2007):

| Pattern | Semantic Coherence | Abstract Services | Encapsulate | Wrapper | Restrict Paths | Intermediary | Raise Abstraction | Runtime Registration | Startup Binding | Runtime Binding |
|---------|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Layered | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | | | | |
| Pipes & Filters | ✓ | ✓ | ✓ | | ✓ | ✓ | | | | |
| Blackboard | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | | | |
| Broker | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | | | |
| MVC | ✓ | ✓ | | | ✓ | ✓ | | | | |
| Presentation-Abstraction-Control | ✓ | ✓ | ✓ | | ✓ | | | | | |
| Microkernel | ✓ | ✓ | ✓ | ✓ | ✓ | | | | | |
| Reflection | ✓ | ✓ | | | | | | | | |

### Using Tactics to Augment Patterns

A documented pattern is **underspecified** for a specific situation. To make a pattern work in a given architectural context, examine:

1. **Inherent quality attribute tradeoffs** the pattern makes — compare what it promotes/diminishes with your needs
2. **Other quality attributes** the pattern affects indirectly but that are important in your application

**Example — Augmenting the Broker Pattern:**

| Weakness | Tactic to Address It |
|----------|---------------------|
| **Availability** — broker is single point of failure | Increase available resources (multiple brokers); maintain multiple copies (shared state); heartbeat/ping/echo (fault detection) |
| **Performance** — indirection adds latency | Increase available resources; scheduling resources (load balancing) |
| **Testability** — highly dynamic, asynchronous | (Not addressed by broker; requires additional testing infrastructure) |
| **Security** — no authentication/authorization | Authentication/authorization tactics; secure communication channels |

> Each tactic brings tradeoffs: load balancing adds indirection (latency); the load balancer becomes a new single point of failure; replicated brokers increase design cost and complexity.

---

## 13.4 Using Tactics Together

Tactics have **main effects** and **side effects (tradeoffs)**. The architect's skill is gauging these and navigating the design space.

### Tactic Mixology Walkthrough

Starting with **ping/echo** (fault detection tactic):

```
Ping/Echo → Side effect: Performance overhead
  → Apply: Increase Available Resources → Side effect: Cost, Resource utilization
    → Apply: Scheduling Policy → Side effect: Modifiability (adding/changing policy)
      → Apply: Use an Intermediary → Side effect: Ensure all communication passes through
        → Apply: Restrict Dependencies → Side effect: Performance overhead of intermediary
          → (Recursive — evaluate if overhead is acceptable)
```

> Applying successive tactics is like **chess**: good players see immediate consequences; very good players look several moves ahead. Design becomes an exercise of "generate and test."

---

## 13.5 Summary

| Concept | Definition |
|---------|-----------|
| **Architectural Pattern** | A package of design decisions found repeatedly in practice, with known properties, describing a class of architectures. Patterns are **discovered**, not invented |
| **Architectural Tactic** | A simpler design primitive using a single structure or mechanism to address a single architectural force. Tactics are the **building blocks** of patterns |
| **Pattern-Tactic Relationship** | Tactics are atoms; patterns are molecules. Patterns package tactics |

**Pattern Template:** {Context, Problem, Solution}

**Pattern Categories by Element Type:**

| Category | Element Type | Examples |
|----------|-------------|---------|
| **Module** | Modules | Layered |
| **C&C** | Components + Connectors | Broker, MVC, Pipe-and-Filter, Client-Server, P2P, SOA, Publish-Subscribe, Shared-Data |
| **Allocation** | Software + Non-software | Map-Reduce, Multi-tier |

**Key Takeaways:**
- Patterns provide known solutions with documented properties — they enable reuse of architectural knowledge
- Patterns are underspecified — augment them with tactics for specific contexts
- Every tactic has side effects; combining tactics requires navigating tradeoffs like a chess game
- Complex systems exhibit multiple patterns simultaneously
- Patterns can be violated in small ways when there's a good design tradeoff

---

## Glossary

| Term | Definition |
|------|-----------|
| **Layer bridging** | When a module in a higher layer directly uses a module in a non-adjacent lower layer |
| **ESB** (Enterprise Service Bus) | Middleware that routes/transforms messages between service providers and consumers |
| **SLA** (Service-Level Agreement) | Contract specifying quality attributes (performance, availability, etc.) for a service |
| **Supernode** | Specialized peer in P2P networks with indexing/routing capabilities |
| **Orchestration server** | Coordinates service interactions based on business process/workflow definitions |
| **Callback** | An upward call from a lower layer to a higher layer that does not depend on the answer — maintains layering discipline |

---

*Source: Bass, Clements & Kazman — Software Architecture in Practice, 3rd Edition, Chapter 13: Architectural Tactics and Patterns*


## Related

- [[Software Architecture Overview]] — All architecture topics
- [[02_Quality_Attributes_Overview]] — QA scenarios driving tactics
- [[07_Design_and_Documentation]] — ADD method uses tactics
- [[Microservice/Microservice Overview]] — Microservice patterns
