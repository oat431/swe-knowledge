---
tags:
  - modeling
  - client-server
  - soa
  - component-based
  - distributed
  - software-engineering
source: "Gomaa, Software Modeling and Design, Chapters 15–17"
created: 2026-07-21
---

# 05 — Distributed and Component-Based Architectures

**Covers:** Gomaa Ch 15 (Client/Server), Ch 16 (Service-Oriented), Ch 17 (Component-Based)

---

## 15. Designing Client/Server Software Architectures

### 15.1 Core Concepts

| Term | Definition |
|------|------------|
| **Server** | A hardware/software system that provides one or more services for multiple clients |
| **Service** | An application software component that fulfills the needs of multiple clients; executes on a server |
| **Client** | A requester of services |

Key distinction: a server may host multiple services; a large service may span multiple server nodes. In client/server systems the service executes on fixed server node(s) and the client has a fixed connection to the server.

---

### 15.2 Architectural Structure Patterns

#### Multiple Client / Single Service (Client/Server Pattern)
- Simplest and most common client/server architecture
- Several clients request a service; one service fulfills client requests
- Example: Banking System — multiple ATM Clients communicating with one Banking Service
- Clients are **tightly coupled** to the service (send message, wait for response)

#### Multiple Client / Multiple Service
- A client may communicate with several services
- Services may communicate with each other
- Example: Banking Federation — ATM clients from multiple banks accessing multiple bank services over a WAN

#### Multi-tier Client/Service
- An intermediate tier acts as **both client and service**
- Example: Three-tier Banking System — ATM Client → Banking Service → Database Service
- Viewed as layered architecture: client is at a higher layer (depends on / uses the service)

---

### 15.3 Architectural Communication Patterns

#### Synchronous Message Communication with Reply (Request/Response)
- Most common client/server communication pattern
- Client sends request, waits for response
- Multiple clients → message queue builds at service
- Service processes on FIFO basis
- Can establish a **connection** for multi-message dialogs

#### Asynchronous Message Communication with Callback
- Client sends request, continues executing without waiting
- Client provides a **callback handle** (remote reference)
- Service uses the handle to send the response asynchronously
- Variation: service can delegate the callback to another component

#### Other Patterns Referenced
- Asynchronous Message Communication (Ch 12)
- Synchronous Communication without Reply (Ch 18)
- Broker patterns (Ch 16)
- Group communication patterns (Ch 17)

---

### 15.4 Middleware in Client/Server Systems

Middleware sits above heterogeneous OS to provide a uniform platform for distributed applications.

| Technology | Description |
|------------|-------------|
| **RPC** (Remote Procedure Call) | Procedures in server address space invoked by remote calls |
| **RMI** (Java Remote Method Invocation) | Client invokes method on a specific remote service object |
| **DCE** | Distributed Computing Environment (uses RPC) |
| **COM** | Component Object Model |
| **CORBA** | Common Object Request Broker Architecture |
| **J2EE** | Java 2 Platform Enterprise Edition |
| **Jini** | Service discovery for Java |

#### RMI Architecture
- **Client Proxy**: same interface as service object; hides communication details from client; marshals parameters into messages
- **Service Proxy**: unmarshals messages; calls actual service method; marshals response
- Net effect: remote method invocation appears as local method invocation to both sides

---

### 15.5 Service Subsystem Design

#### Sequential Service
- Processes client requests one at a time (completes one before starting next)
- Designed as one concurrent object with a message queue
- Uses **Layers of Abstraction** pattern:
  - **Coordinator** (façade) — provides uniform client interface
  - **Business Logic** objects — encapsulate business rules
  - **Database Wrapper** objects — encapsulate data access

#### Concurrent Service
- Shared among several concurrent objects for higher throughput
- Each transaction manager is a separate concurrent object
- Coordinator delegates to individual transaction managers
- Often paired with **Asynchronous Message Communication with Callback**

---

### 15.6 Database Wrapper Classes

Wrapper classes hide how data is accessed from a database. They provide an **object-oriented interface** to a relational database.

- Encapsulate all SQL statements
- Usually hide access to one relational table (or a view / join)
- Map analysis-model entity classes to both:
  - A **relational table** (data)
  - A **database wrapper class** (operations like create, read, update, delete, validate)

**Example (DebitCard):**
- Operations: `create()`, `validate()`, `checkDailyLimit()`, `updateDailyTotal()`, `read()`, `delete()`, plus various update operations per attribute

---

### 15.7 Static Models → Relational Database Design

#### Basic Mapping
- Entity class → Relational table (same name)
- Entity attributes → Table columns
- Object instances → Table rows

#### Primary Keys
- Every table must have a primary key (uniquely identifies a row)
- Can be a **concatenated key** (multiple attributes)
- Notation: `TableName(primaryKey, attribute1, attribute2)`

#### Foreign Keys (Associations)
- **One-to-one / Zero-or-one**: Primary key of either table as foreign key in the other; for zero-or-one, put FK in the "optional" table to avoid nulls
- **One-to-many**: FK is in the "many" table
- **Many-to-many**: Mapped to an **association table** with concatenated primary key from both participating tables

#### Association Classes → Association Tables
- Association class becomes its own table
- Primary key = concatenation of primary keys from participating tables
- Each component of the key is also a foreign key (allows navigation)

#### Whole/Part (Aggregation/Composition)
- Whole class → Table with its own primary key
- Part classes → Each gets its own table
- Whole's primary key becomes: the part table's primary key (one-to-one), part of concatenated key (one-to-many), or a foreign key in the part table

#### Generalization/Specialization — Three Mapping Strategies

| Strategy | Approach | When to Use |
|----------|----------|-------------|
| **Superclass + Subclasses** | Each class → separate table; shared PK; discriminator attribute in superclass | Clean, extensible; doubles DB accesses |
| **Subclasses Only** | Only subclass tables; superclass attributes replicated in each | Subclasses have many attributes, superclass has few; faster performance |
| **Superclass Only** | One table with all subclass attributes + discriminator; nulls for unused attributes | Superclass has many attributes, subclasses few, small number of subclasses |

---

## 16. Designing Service-Oriented Architectures (SOA)

### 16.1 Core Concepts

A **Service-Oriented Architecture (SOA)** is a distributed software architecture consisting of multiple **autonomous services** that can execute on different nodes with different providers, implemented in different languages, communicating via standard protocols.

#### Key Difference from Client/Server
- Client/Server: fixed connection to a specific service on a fixed server
- SOA: **loosely coupled** services that can be **discovered** and linked to with the assistance of **service brokers**

#### Service Design Principles

| Principle | Description |
|-----------|-------------|
| **Loose Coupling** | Services are relatively independent; minimal dependencies |
| **Service Contract** | Well-defined interface with operations, I/O parameters, QoS parameters |
| **Autonomy** | Self-contained; can operate independently |
| **Abstraction** | Implementation details hidden; only interface revealed |
| **Reusability** | Designed for reuse across applications |
| **Composability** | Can be assembled into larger composite services |
| **Statelessness** | Minimal or no information about specific client activities |
| **Discoverability** | External description enables discovery |

---

### 16.2 Broker Patterns

Brokers act as intermediaries between clients and services, providing **location transparency** and **platform transparency**.

| Pattern | Description | Use Case |
|---------|-------------|----------|
| **Service Registration** | Service registers name, description, location with broker; re-registers on relocation | Initial service joining |
| **Broker Forwarding** (White Pages) | Broker forwards every client message to service and every reply back (4 messages per exchange) | High security (each message vetted); cost: doubled traffic |
| **Broker Handle** (White Pages) | Broker returns a service handle; client communicates directly with service thereafter | Client-service dialog with multiple messages; most commercial brokers |
| **Service Discovery** (Yellow Pages) | Client queries broker for all services of a type; selects one; receives handle; then communicates directly | Client knows service type but not specific service |

**White pages** = client knows the specific service needed but not its location (analogous to telephone white pages).

**Yellow pages** = client knows the type of service needed but not the specific service (analogous to telephone yellow pages).

---

### 16.3 Technology Support

| Technology | Role |
|------------|------|
| **XML** | Enables heterogeneous system interoperability through data/text exchange |
| **SOAP** (Simple Object Access Protocol) | XML/HTTP-based protocol: envelope, encoding rules, RPC conventions |
| **WSDL** (Web Services Description Language) | XML-based language describing what a service does, where it resides, how to invoke |
| **UDDI** (Universal Description, Discovery, and Integration) | SOAP-based protocol for registering and discovering Web services |
| **Web Services** | APIs providing standard communication among software applications on the Web |

---

### 16.4 Transaction Patterns

A **transaction** is a request consisting of two or more operations that perform a single logical function — must complete entirely or not at all.

#### ACID Properties

| Property | Meaning |
|----------|---------|
| **Atomicity** | Indivisible unit; all committed or all rolled back |
| **Consistency** | System must be in a consistent state after execution |
| **Isolation** | Behavior unaffected by other concurrent transactions |
| **Durability** | Changes are permanent and survive system failures |

#### Two-Phase Commit Protocol
Used for atomic transactions in distributed systems.

- **Commit Coordinator** orchestrates the protocol
- **Phase 1 (Prepare)**:
  - Coordinator sends `prepareToCommit` to each participant
  - Each participant locks the record, performs the update, sends `readyToCommit`
  - If any participant cannot perform → sends `refuseToCommit`
- **Phase 2 (Commit/Abort)**:
  - If all ready → coordinator sends `commit`; each makes update permanent, unlocks, sends `commitCompleted`
  - If any refused → coordinator sends `abort` to all; participants roll back

#### Compound Transaction Pattern
- Breaks a large transaction into smaller **flat atomic transactions**
- Each can be performed and rolled back independently
- Example: Travel agent — airline, hotel, car rental as separate transactions

#### Long-Living Transaction Pattern
- Splits a long transaction (with human in the loop) into two or more separate transactions
- Human decision-making occurs between successive transaction pairs
- Avoids locking records for extended periods
- Example: Airline reservation — query (display seats) → reserve (but must recheck availability)

---

### 16.5 Negotiation Pattern

Client and service agents negotiate cooperatively:

| Role | Actions |
|------|---------|
| **Client Agent** | Propose a service (negotiable), Request a service (non-negotiable), Reject an offer |
| **Service Agent** | Offer a counter-proposal, Reject client request/proposal, Accept client request/proposal |

**Example workflow**: Client agent proposes trip constraints → Service agent queries airlines → Service agent offers best matches → Client agent selects/requests → Reservation confirmed or rejected → Alternative flights offered

---

### 16.6 Service Interface Design

- Messages arriving at a service form the basis for designing service operations
- Analyze message name, input parameters, output parameters
- Operations are defined on a **provided interface** (e.g., `IInventoryService`)
- Service has a **provided port** that supports the provided interface

**Example — Inventory Service Interface (`IInventoryService`):**
- `checkInventory(in itemId, in amount, out inventoryStatus)`
- `update(in itemId, in amount)`
- `reserveInventory(in itemId, in amount, out inventoryStatus)` — Phase 1 of 2PC
- `commitInventory(in itemId, in amount, out inventoryStatus)` — Phase 2 of 2PC
- `abortInventory(in itemId, in amount, out inventoryStatus)`

---

### 16.7 Service Coordination

| Coordination Type | Description |
|-------------------|-------------|
| **Orchestration** | Centrally controlled workflow coordination logic; sequences multiple participant services |
| **Choreography** | Distributed coordination among services; used for cross-organization collaboration |

**Goal**: Keep services **stateless** for reusability. State information is stored in records (e.g., database). Sequencing logic is encapsulated in **coordinator objects**.

---

### 16.8 Service Reuse

- Services should have **only provided interfaces** (no required interfaces) — unless using asynchronous communication with callback
- This makes services **self-contained** and more reusable
- New coordinator objects are created to control and sequence the desired workflow for each new application
- Calling components must follow any constraints on operation invocation order

---

## 17. Designing Component-Based Software Architectures

### 17.1 Core Concepts

A **distributed component** is a concurrent object with a well-defined interface — a logical unit of distribution and deployment.

| Concept | Description |
|---------|-------------|
| **Simple Component** | No internal part components |
| **Composite Component** | Composed of other part components |
| **Configurable Component** | Can be deployed to different nodes at deployment time (not design time) |

**Key Goal**: Design a concurrent message-based architecture that is **highly configurable** — the same architecture can be deployed to many different distributed configurations.

**Critical Rule**: All communication between components must be restricted to **message communication** (since components may reside on separate nodes).

---

### 17.2 Design Steps

1. **Design distributed software architecture** — Structure into constituent components using subsystem structuring criteria (Ch 13.8) + component structuring criteria (Section 17.5); define interfaces
2. **Design constituent components** — Internal design of each simple component using sequential OO design methods (Ch 14)
3. **Deploy the application** — Define instances, interconnect, map to physical nodes

---

### 17.3 Composite Subsystems and Components

- A **composite subsystem** is a component; objects within must reside at the same location
- Objects in different geographical locations are never in the same composite subsystem
- The composite component adds no functionality — functionality comes entirely from its **part components**
- Incoming messages are passed through to the appropriate internal component; outgoing messages are passed to the external destination
- Usually depicted with **UML active class notation** (concurrent components)

---

### 17.4 UML Component Modeling

#### Interfaces
- Specifies externally visible operations without revealing implementation
- A component can provide **multiple interfaces** for different client needs

#### Provided and Required Interfaces
- **Provided Interface**: operations a component must fulfill (what others can call on it)
- **Required Interface**: operations other components provide that this component needs

#### Ports
- A component has one or more **ports** through which it interacts with others
- **Provided Port (P*)**: supports a provided interface
- **Required Port (R*)**: supports a required interface
- **Complex Port**: supports both provided and required interfaces

#### Connectors
- Join the required port of one component to the provided port of another
- Ports must be **compatible** — required interface operations must match the provided interface operations
- **Delegation Connector**: connects an outer port of a composite component to an inner port of a part component (messages forwarded through)

#### Composite Structure Diagrams
- UML structured classes with ports, interfaces, and connectors
- Part components within composites are depicted as **instances** (multiple instances possible)

---

### 17.5 Component Structuring Criteria

| Criterion | Guideline |
|-----------|-----------|
| **Proximity to Physical Data** | Place component close to data source for fast access (esp. high data rates) |
| **Localized Autonomy** | Each instance on separate node; operates independently; survives other node failures |
| **Performance** | Time-critical functions within a node for better, predictable performance |
| **Specialized Hardware** | Component on node with special-purpose hardware, sensors, or actuators |
| **I/O Component** | Relatively autonomous; close to physical data source; may contain device interface, control, and entity objects |

I/O component types: input, output, I/O (both), network interface, system interface components.

---

### 17.6 Group Message Communication Patterns

#### Broadcast Message Communication
- Unsolicited message sent to **all** recipients
- Each recipient decides whether to process or discard
- Example: Alarm Handling Service broadcasts alarm to all Operator Interaction components

#### Subscription/Notification (Multicast)
- Components **subscribe** to a group; messages sent to all group members (multicast)
- Sender (**publisher**) does not need to know individual members
- Components can be members of multiple groups; can subscribe/unsubscribe
- Popular on the Internet
- Example: Operator Interaction components subscribe to alarm types; Alarm Handling Service multicasts alarm notifications to subscribers only

**Variation (Single Subscriber)**: Useful for peer-to-peer — consumer subscribes to producer; reverses dependency (consumer depends on producer, not vice versa).

#### Concurrent Service with Subscription/Notification
- News Archive Service example: multiple concurrent services (News Archive, News Update, Subscription, Notification)
- Three interaction types: Query (Q), Event Subscription (S), Event Notification (E)
- Access synchronization needed for concurrent data access

---

### 17.7 Application Deployment

#### Deployment Activities
1. **Define component instances** — For each multi-instance component, define each instance, its unique name, and parameterized values (e.g., sensor names, limits, alarm names)
2. **Interconnect component instances** — Wire instances according to the application architecture
3. **Map instances to physical nodes** — Assignment can vary: components can run on separate nodes or co-located on the same node; depicted on a **deployment diagram**

#### Deployment Diagram Example (Emergency Monitoring System)
| Component Instance | Deployment Strategy | Rationale |
|--------------------|---------------------|-----------|
| Monitoring Sensor | 1 node per sensor | Localized autonomy, performance |
| Remote System Proxy | 1 node per remote system | Proximity to physical data |
| Alarm Service | Separate node | Performance (responsiveness) |
| Monitoring Data Service | Separate node | Performance |
| Operator Presentation | 1 node per operator | Localized autonomy |

---

## Summary: Comparing the Three Architecture Styles

| Aspect | Client/Server (Ch 15) | SOA (Ch 16) | Component-Based (Ch 17) |
|--------|----------------------|-------------|--------------------------|
| **Coupling** | Tight (fixed connection) | Loose (discoverable) | Configurable (deployment-time) |
| **Service Discovery** | Fixed, known location | Broker-mediated (white/yellow pages) | Via ports and connectors |
| **Coordination** | Implicit (request/response) | Orchestration / Choreography | Message-based |
| **Transactions** | Not emphasized | ACID, 2PC, Compound, Long-Living | N/A |
| **Deployment** | Fixed node mapping | Services on different providers | Configurable at deployment time |
| **Communication** | Sync with Reply, Async with Callback | Sync with Reply, Async, Broker | Broadcast, Subscription/Notification, Sync/Async |
| **Key UML** | Deployment, Communication diagrams | Communication diagrams, Interfaces | Composite Structure diagrams, Ports, Connectors |


## Related

- [[Software Engineering Models and Methods Overview]] — All models and methods topics
- [[04_Architectural_Design]] — OO architecture design
- [[06_Real_Time_and_Product_Lines]] — Real-time systems
