---
tags: [modeling, architecture, subsystems, oo-design, software-engineering]
source: Gomaa, Software Modeling and Design, Chapters 12-14
created: 2026-07-21
---

# 04 — Architectural Design

## Overview

**Part III of Gomaa's *Software Modeling and Design* covers architectural design** — the transition from analysis (what the system does) to design (how the system is built). The software architecture describes the high-level structure of the system in terms of subsystems, their interfaces, and their interconnections, while hiding internal details of individual subsystems.

---

## Chapter 12: Overview of Software Architecture

### 12.1 Software Architecture and Component-Based Architecture

**Software architecture** (Bass, Clements, Kazman 2003): *The structure or structures of the system, which comprise software elements, the externally visible properties of those elements, and the relationships among them.*

A **component-based software architecture** consists of multiple components where each component:
- Is self-contained and encapsulates certain information
- Is either a composite object or a simple object
- Provides an **interface** through which it communicates with other components
- Is a **black box** — implementation is hidden from other components
- Communicates using predefined communication patterns

**Sequential vs. Concurrent designs:**
- **Sequential design**: Components are passive classes (no thread of control). Communication is call/return only.
- **Concurrent/distributed design**: Components are active (concurrent), deployable to different nodes. Communication includes synchronous, asynchronous, brokered, and group communication.

**Architecture Stereotypes in UML 2**: A modeling element can have two stereotypes:
1. **Role stereotype** (analysis): e.g., `«entity»`, `«boundary»`, `«control»`
2. **Architectural structuring stereotype** (design): e.g., `«subsystem»`, `«component»`, `«service»`, `«concurrent task»`

These are orthogonal — independent of each other.

### 12.2 Multiple Views of a Software Architecture

Three key views:

#### 12.2.1 Structural View (Static)
- Depicted on **class diagrams**
- Shows subsystems as composite/aggregate classes
- Shows static relationships and multiplicities between subsystems
- Example: Banking System — 1 Banking Service to many ATM Clients (one-to-many association)

#### 12.2.2 Dynamic View (Behavioral)
- Depicted on **communication diagrams**
- Shows subsystems as concurrent components with message communication
- Uses **generic** (not instance) communication diagrams — depicts all possible interactions
- Sequence numbers omitted because all scenarios are covered
- Example: ATM Client sends `ATMTransaction` (synchronous) to Banking Service, receives `bankResponse` reply

#### 12.2.3 Deployment View (Physical)
- Depicted on **deployment diagrams**
- Shows how subsystems are allocated to physical nodes
- Shows network connections between nodes
- Example: Each ATM Client on its own node, Banking Service on a central server node, connected via wide area network

### 12.3 Software Architectural Patterns

Patterns categorized into two groups:

| Category | Description | Examples |
|----------|------------|----------|
| **Architectural Structure Patterns** | Address the static structure | Layers of Abstraction, Client/Server patterns |
| **Architectural Communication Patterns** | Address dynamic communication | Call/Return, Async Message, Sync with Reply |

#### 12.3.1 Layers of Abstraction Pattern
- Also called Hierarchical Layers / Levels of Abstraction
- Each layer uses services in the layer immediately below it
- Enables **extension** (add upper layers) and **contraction** (remove upper layers) — Parnas (1979)
- **Strict hierarchy**: Layer 3 invokes only layer 2. **Flexible hierarchy**: Layer 3 may directly invoke layer 1.

**TCP/IP example** (5-layer model):
1. **Physical layer** — hardware, transmission medium
2. **Network interface layer** — frames, transmission
3. **Internet layer (IP)** — packet format, routing
4. **Transport layer (TCP)** — message assembly, virtual connection, end-to-end protocol
5. **Application layer** — FTP, email, WWW

Key characteristic: Upper layers can be replaced while lower layers remain unchanged. Routers use only layers 1-3; end nodes use all 5.

#### 12.3.2 Call/Return Pattern
- Only communication pattern in **sequential designs**
- Calling operation in calling object invokes called operation in called object
- Control passes from caller to callee; returns on completion
- UML notation: black arrowhead (synchronous)
- Example: `WithdrawalTransactionManager` invokes `debit()` on `CheckingAccount` or `SavingsAccount`

#### 12.3.3 Asynchronous Message Communication Pattern
- Also called Loosely Coupled Message Communication
- Producer sends message, **does not wait** for reply
- Messages queued FIFO at consumer if consumer is busy
- Consumer suspended if no message available
- UML notation: stick arrowhead
- Used when sender does not need a response
- **Bidirectional asynchronous**: Both components send async messages to each other

#### 12.3.4 Synchronous Message Communication with Reply Pattern
- Also called Tightly Coupled Message Communication
- Client sends message, **waits for reply**
- Service accepts, processes, generates reply, sends response
- Typically involves multiple clients and one service
- UML notation: black arrowhead for request, dashed stick arrow for reply
- Fundamental to client/server architectures

### 12.4 Documenting Software Architectural Patterns

Standard template (Buschmann et al. 1996):
- **Pattern name**, **Aliases**, **Context**, **Problem**, **Summary of solution**
- **Strengths**, **Weaknesses**, **Applicability**, **Related patterns**, **Reference**

Example — **Layers of Abstraction**:
- Context: Software architectural design
- Problem: Need architecture supporting extension and contraction
- Solution: Lower layers provide services for higher layers
- Strengths: Promotes extension/contraction
- Weaknesses: Inefficiency if too many layers traversed
- Applicability: OS, communication protocols, product lines

### 12.5 Interface Design

**Separation of interface from implementation** is central to OO design and component-based architecture.

- An **interface** specifies externally visible operations without revealing internal structure
- Acts as a **contract** between provider and user of the interface
- Class attributes are **private**; public operations constitute the interface
- **Provided interface**: depicted as a circle (ball) — class provides the interface
- **Required interface**: depicted as a semicircle (socket) — class uses the interface
- The ball-and-socket notation shows a component with a required interface connected to a component with a provided interface
- Interface naming convention: starts with "I" (e.g., `IBasicAlarmService`)

Operations of `IBasicAlarmService`:
- `alarmRequest (in request, out alarmData)`
- `post (in alarm)`

### 12.6 Designing Software Architectures — Chapter Roadmap

| Chapter | Architecture Type | Key Focus |
|---------|------------------|-----------|
| 14 | Object-Oriented | Information hiding, classes, inheritance |
| 15 | Client/Server | One service, multiple clients |
| 16 | Service-Oriented | Multiple distributed autonomous services |
| 17 | Component-Based | Component ports, provided/required interfaces, connectors |
| 18 | Concurrent/Real-Time | Concurrent tasks, state-dependent control |
| 19 | Software Product Lines | Commonality and variability in product families |
| 20 | Quality Attributes | Nonfunctional requirements (maintainability, performance, security, etc.) |

### 12.7 Summary
- Multiple views (static, dynamic, deployment) for complete architectural description
- Architectural structure patterns (Layers of Abstraction) structure the system into subsystems
- Architectural communication patterns (Call/Return, Async, Sync with Reply) define how subsystems communicate
- Each subsystem has an explicit interface of provided and used operations

---

## Chapter 13: Software Subsystem Architectural Design

### 13.1 Issues in Software Architectural Design

**From analysis to design:**
- Analysis: decompose problem use-case by use-case (what)
- Design: synthesize solution by designing a software architecture (how)

Core design decisions:
1. Integrate use case-based interaction models into initial architecture
2. Determine subsystems using **separation of concerns** and **subsystem structuring criteria**
3. Determine the type of **message communication** between subsystems

**Key principle**: Each subsystem performs a major function that is relatively independent of other subsystems. Once interfaces are defined, subsystem design can proceed independently.

Geographical distribution often simplifies subsystem identification (e.g., clients and services in different locations).

### 13.2 Integrated Communication Diagrams

An **integrated communication diagram** synthesizes all use case-based communication diagrams:
- Merge communication diagrams in the order use cases execute
- Superimpose subsequent diagrams on top of previous ones
- Add new objects and new message interactions; duplicates shown only once
- Shows **all** message communication including alternative sequences
- Represented as a **generic** UML communication diagram (all possible interactions)
- Sequence numbers omitted to reduce clutter

**Aggregate messages** group messages to reduce diagram clutter. Example: `DisplayPrompts` aggregates `GetPIN`, `InvalidPINPrompt`, `DisplayMenu`, `DisplayCancel`, `DisplayConfiscate`, `DisplayEject`.

A **message dictionary** defines the contents of each aggregate message.

### 13.3 Separation of Concerns in Subsystem Design

Six design considerations for structuring subsystems:

#### 13.3.1 Composite Object
- Objects part of the same composite object → same subsystem
- **Composition** is stronger than aggregation: parts are created, live, and die together with the whole
- Example: ATM composite class contains CardReader, CashDispenser, ReceiptPrinter, KeypadDisplay
- **Aggregate subsystems** can contain composite subsystems; useful for layered architectures

#### 13.3.2 Geographical Location
- Objects in potentially different physical locations → different subsystems
- In distributed environments, subsystems communicate only via messages
- Example: Emergency Monitoring System — MonitoringSensor, RemoteSystemProxy, OperatorPresentation, AlarmService, MonitoringDataService on separate nodes

#### 13.3.3 Clients and Services
- Clients and services → separate subsystems (special case of geographical location)
- Example: Many ATM Client subsystems at physical ATM locations; Bank Service at centralized data center

#### 13.3.4 User Interaction
- User interaction objects → separate subsystems
- Users typically use their own PCs in distributed configurations
- GUI objects may be composite (e.g., `OperatorPresentation` containing `OperatorInteraction`, `AlarmWindow`, `EventMonitoringWindow`)

#### 13.3.5 Interface to External Objects
- An external real-world object should interface to **only one** subsystem
- Example: ATMClient interfaces to CardReader, CashDispenser, ReceiptPrinter — each external device interfaces only to ATMClient

#### 13.3.6 Scope of Control
- A control object and all entity/I/O objects it directly controls → **same** subsystem
- Example: `ATMControl` within `ATMClient` subsystem controls CardReaderInterface, CashDispenserInterface, CustomerInteraction, ATMTransaction

### 13.4 Subsystem Structuring Criteria

Six subsystem types (a subsystem can satisfy multiple criteria):

#### 13.4.1 Client Subsystem
- Requester of one or more services
- May include user interaction, control, I/O subsystems
- May combine multiple roles (e.g., ATM Client has both user interaction and control)

#### 13.4.2 User Interaction Subsystem
- Provides user interface; performs client role
- One per type of user
- Usually composite: composed of simpler user interaction objects; may include entity objects (local storage/caching) and control objects
- Simple: single thread of control (command line or single-window GUI)
- Complex: multiple windows, multiple threads, shared data

#### 13.4.3 Service Subsystem
- Provides a service for client subsystems
- Responds to requests; does **not** initiate requests
- Usually composite: entity objects, coordinator objects, business logic objects
- Often associated with a data repository or I/O device
- May be allocated its own node
- Examples: `AlarmService`, `MonitoringDataService`

#### 13.4.4 Control Subsystem
- Controls a given part of the system
- Receives inputs from external environment, generates outputs (often without human intervention)
- Often **state-dependent** (includes state-dependent control object)
- May receive high-level commands from a coordinator subsystem
- Examples: `ATMClient` (control + user interaction), `AutomatedGuidedVehicleSystem` (Vehicle Control state machine)

#### 13.4.5 Coordinator Subsystem
- Coordinates execution of other subsystems (control or service subsystems)
- **Control coordination**: When multiple control subsystems need oversight → use a hierarchical coordinator (e.g., `SupervisorySystem` assigns jobs to individual `AutomatedGuidedVehicleSystem` subsystems)
- **Service/workflow coordination**: Determines execution sequence of multiple service subsystems
- Example: `CustomerCoordinator` in online shopping coordinates Catalog Service, Customer Account Service, Credit Card Service, and Email Service

#### 13.4.6 Input/Output Subsystem
- Performs I/O operations on behalf of other subsystems
- Can be relatively autonomous ("smart" devices with localized control)
- Typically contains device interface objects, control objects, and entity objects for local data
- Example: `MonitoringSensorComponent` receives sensor inputs, posts alarms and events to services

### 13.5 Decisions About Message Communication Between Subsystems

**Analysis model**: Messages depicted as simple messages (stick arrowhead) — no decision about concurrency or communication type.

**Design model** decisions:
1. **Concurrency**: Are the communicating objects concurrent?
2. **Message type**: Synchronous or asynchronous?
3. **Precise specification**: Message name and parameters

**Unidirectional** (producer → consumer): typically mapped to asynchronous communication
**Bidirectional** (client ↔ service): typically mapped to synchronous communication with reply

The design decision is formalized into architectural communication patterns:
- Asynchronous Message Communication pattern → unidirectional
- Synchronous Message Communication with Reply pattern → bidirectional

### 13.6 Summary
- Subsystems categorized by role (client, service, control, coordinator, I/O, user interaction)
- Separation of concerns: composite objects, geographical location, client/service, user interaction, external interfaces, scope of control
- Message communication decisions: synchronous vs. asynchronous
- Each subsystem has explicit interface: operations provided and operations used

---

## Chapter 14: Designing Object-Oriented Software Architectures

### 14.1 Concepts, Architectures, and Patterns

**Object-oriented design** uses three fundamental concepts:
1. **Information hiding**: Class encapsulates information (data, state machine, algorithm) hidden from rest of system
2. **Classes**: Objects instantiated from classes, accessed through operations/methods
3. **Inheritance**: Code sharing and adaptation via class hierarchies

**Interface-implementation separation**: The interface forms a contract between provider and user.

This chapter focuses on **sequential object-oriented architectures** (one thread of control, Call/Return pattern only). OO concepts are extended in later chapters for distributed, component-based, concurrent, service-oriented, and product-line architectures.

### 14.2 Designing Information Hiding Classes

Classes from the analysis model are categorized by stereotype and refined:

| Analysis Category | Design Refinement | Description |
|------------------|-------------------|-------------|
| **Entity** (`«entity»`) | Data abstraction class | Encapsulates data structures |
| | Wrapper class | Hides interface to existing/legacy system or database |
| **Boundary** (`«boundary»`) | Device I/O class | Often active (concurrent) — Ch 18 |
| | Proxy class | Often active — Ch 18 |
| | GUI class | Passive — this chapter |
| **Control** (`«control»`) | State-machine class | Passive — encapsulates finite state machine |
| | Coordinator class | Active (concurrent) — Ch 18 |
| | Timer class | Active — Ch 18 |
| **Application Logic** | Business logic class | Encapsulates business rules — this chapter |
| | Service class | Ch 16 |
| | Algorithm class | Often active — Ch 18 |

### 14.3 Designing Class Interface and Operations

**Operations determined from the dynamic model** (communication/sequence diagrams):
- Message direction → which object invokes which operation
- Message name → operation name
- Message parameters → operation parameters
- Response messages → return parameters of the operation

**From the static model**: Standard CRUD operations (create, read, update, delete) can be tailored to specific class needs.

**Example — ATM Card class**:
- Analysis: `CardReaderInterface` sends `CardId, StartDate, ExpirationDate` to `ATMCard`; `CustomerInteraction` sends `CardRequest`
- Design: `write(in cardId, in startDate, in expirationDate)` and `read(out cardId, out startDate, out expirationDate)`
- Attributes (`atmCardId`, `atmStartDate`, `atmExpirationDate`) are private
- Interface: public `read` and `write` operations

### 14.4 Data Abstraction Classes

Entity classes that encapsulate data are designed as **data abstraction classes**:
- Hide internal data structure representation
- Provide access procedures/functions whose internals are also hidden
- Operations determined by analyzing how other objects access the data

**Example — ATM Cash**:
- Encapsulates: `cashAvailable`, `fives`, `tens`, `twenties`
- Operations:
  - `addCash(in fivesAdded, in tensAdded, in twentiesAdded)`
  - `withdrawCash(in cashAmount, out fivesToDispense, out tensToDispense, out twentiesToDispense)`
- **Invariant**: `cashAvailable = 5 × fives + 10 × tens + 20 × twenties`

### 14.5 State-Machine Classes

**Encapsulates** the information on a statechart:
- Hides the state transition table (mapped from the statechart)
- Maintains the current state
- Provides operations to process incoming events and change state

**Reusable state-machine class design**:
- `processEvent(in event, out action)` — looks up state transition table, determines new state and actions, updates state, returns actions
- `currentState(): State` — returns current state (optional)
- The contents of the table are application-dependent; defined at instantiation/initialization time
- Example: `ATMStateMachine` class encapsulates the ATM state transition table

### 14.6 Graphical User Interaction (GUI) Classes

GUI classes **hide the details of the user interface** from other classes:
- Low-level GUI classes (windows, menus, buttons, dialogs) stored in UI component libraries
- Higher-level composite user interaction classes contain lower-level GUI classes
- Designed for each individual screen/window

**Example — Banking GUI classes**: `MenuWindow`, `PINWindow`, `WithdrawalWindow`, `TransferWindow`, `QueryWindow`, `PromptWindow`

Each GUI class has:
- A `clear()` operation to blank the screen
- Display operations that output prompts and accept user input (returned as output parameters)
  - `displayMenu(out selection)` — shows menu, returns user's choice
  - `displayWithdrawalWindow(out accountNumber, out amount)` — prompts for and returns account and amount
  - `displayPrompt(in promptText)` — displays informational message (no user input expected)

### 14.7 Business Logic Classes

**Encapsulate business rules** that could change independently:
- Define decision-making, business-specific application logic
- Access various entity objects during execution

**Example — `WithdrawalTransactionManager`**:
- Operations: `initialize()`, `withdraw()`, `confirm()`, `abort()`
- `withdraw(in accountNumber, in amount, out response)` — processes withdrawal
- `confirm(in accountNumber, amount)` — confirms successful completion
- `abort(in accountNumber, amount)` — handles failure (e.g., cash not dispensed)

### 14.8 Inheritance in Design

Inheritance enables **code sharing and adaptation**. Two approaches:
- **Top-down**: Design superclass capturing overall characteristics, specialize into variant subclasses
- **Bottom-up**: Recognize common properties in existing classes, generalize into a superclass

**Important**: Inheritance breaks encapsulation (white box reuse). Child class implementation is bound to parent class implementation → ripple-effect problems with deep hierarchies. **Limit hierarchy depth.**

#### 14.8.2 Abstract Classes
- Class with **no instances**
- Template for creating **subclasses**, not objects
- Must have at least one **abstract operation** (declared but not implemented)
- Defines interface; subclasses provide implementation
- Different subclasses can implement the same abstract operation differently
- Some operations may be implemented in the abstract class (common implementation/default behavior)
- Subclasses can **override** parent operations for special cases

#### 14.8.3 Example — Account Hierarchy

**Account (abstract superclass)**:
- Attributes: `accountNumber` (Integer), `balance` (Real)
- Operations: `open()`, `close()`, `readBalance(): Real`, `credit(amount)` {abstract}, `debit(amount)` {abstract}

**CheckingAccount**:
- Additional attribute: `lastDepositAmount`
- Implements `credit`: add to balance, set `lastDepositAmount`
- Implements `debit`: deduct from balance
- Additional operation: `readLastDepositAmount(): Real`

**SavingsAccount**:
- Additional attributes: `cumulativeInterest`, `debitCount`, static `maxFreeDebits = 3`, static `bankCharge = $2.50`
- Implements `credit`: add to balance
- Implements `debit`: deduct from balance, increment `debitCount`, deduct `bankCharge` if `maxFreeDebits > debitCount`
- Additional operations: `addInterest(interestRate)`, `readCumulativeInterest(): Real`, `clearDebitCount()`

### 14.9 Class Interface Specifications

A class interface specification defines:

| Element | Description |
|---------|-------------|
| **Information hidden** | e.g., data structures encapsulated |
| **Class structuring criterion** | e.g., Data abstraction class |
| **Assumptions** | e.g., one operation must be called before another |
| **Anticipated changes** | Design for change |
| **Superclass** (if any) | Inheritance relationship |
| **Inherited operations** (if any) | From superclass |
| **Operations** | For each: function, preconditions, postconditions, invariants, parameters, operations used |

**Example — CheckingAccount specification**:
- Information hidden: Checking account attributes and their current values
- Criterion: Data abstraction class
- Assumptions: Checking accounts do not have interest
- Anticipated changes: May be allowed to earn interest
- Superclass: Account
- Inherited: `open`, `credit`, `debit`, `readBalance`, `close`
- `credit(in amount)`: Adds to balance, stores as last deposited. Precondition: Account exists. Postcondition: Account credited.
- `debit(in amount)`: Deducts from balance. Precondition: Account exists. Postcondition: Account debited.

### 14.10 Detailed Design of Information Hiding Classes

**Pseudocode** (Structured English) documents algorithmic design in a language-independent way:
- Structured constructs: `If-Then-Else`, loops, case statements
- English for sequential statements
- Readily mappable to implementation language

**Example — Account superclass pseudocode**:
- `open(in accountNumber)`: create new account; assign accountNumber; set balance to zero
- `close()`: close the account
- `readBalance(): Real`: return value of balance
- `credit(in amount)`: deferred to subclass
- `debit(in amount)`: deferred to subclass

**Example — CheckingAccount debit**:
```
debit(in amount):
  Deduct amount from balance
```

**Example — SavingsAccount debit**:
```
debit(in amount):
  Deduct amount from balance
  Increment debitCount
  if maxFreeDebits > debitCount then deduct bankCharge from balance
```

### 14.11 Polymorphism and Dynamic Binding

**Polymorphism** ("many forms"): Different classes may have the same operation name with identical specification but different implementations. Allows objects with identical interfaces to be substituted at run-time.

**Dynamic binding**: Run-time association of a request to an object and its operation (vs. compile-time binding in procedural languages). A variable may reference objects of different classes at different times.

**Example**: 
```
anAccount: Account  // can reference CheckingAccount or SavingsAccount
anAccount.debit(amount)  // invokes CheckingAccount.debit or SavingsAccount.debit at run-time
```

- `CheckingAccount.debit` simply deducts amount
- `SavingsAccount.debit` deducts amount + applies bank charge if free debits exceeded

**Substitutability**: A `CheckingAccount` or `SavingsAccount` can be assigned to an `Account` variable, but NOT vice versa (not every account is a checking account).

### 14.12 Implementation of Classes in Java

Example — ATMCash class (excerpt):
```java
public class ATMCash {
    private int cashAvailable = 0;
    int fives = 0;
    int tens = 0;
    int twenties = 0;

    public void addCash(int fivesAdded, int tensAdded, int twentiesAdded) {
        fives = fives + fivesAdded;
        tens = tens + tensAdded;
        twenties = twenties + twentiesAdded;
        cashAvailable = 5 * fives + 10 * tens + 20 * twenties;
    }

    public int withdrawCash(int cashAmount, int[] bills) {
        // given the cash amount to withdraw, return bills of each denomination
    }
}
```

### 14.13 Summary
- OO design uses information hiding, classes, and inheritance
- Information hiding classes: data abstraction, state-machine, GUI, business logic
- Class operations determined from dynamic interaction models
- Inheritance enables code sharing; abstract classes define common interfaces with deferred implementation
- Polymorphism + dynamic binding → substitutability at run-time
- Interface specifications (preconditions, postconditions, invariants) form contracts
- These concepts form the foundation for more advanced architectures (client/server, SOA, component-based, real-time, product lines)

---

## Key Takeaways

1. **Software architecture** separates overall structure (subsystems + interfaces) from internal details
2. **Three views** are essential: structural (class diagrams), dynamic (communication diagrams), deployment (deployment diagrams)
3. **Architectural patterns** provide proven templates — Layers of Abstraction for structure, Call/Return, Async, and Sync with Reply for communication
4. **Subsystem structuring** follows separation of concerns: composite objects, geography, client/service, user interaction, external interfaces, scope of control
5. **Six subsystem roles**: client, user interaction, service, control, coordinator, I/O
6. **Message communication decisions** (sync vs. async) are formalized into architectural communication patterns during design
7. **Information hiding classes** come in four types: data abstraction, state-machine, GUI, business logic
8. **Inheritance** enables code sharing but breaks encapsulation — limit hierarchy depth
9. **Abstract classes** define interfaces; subclasses provide implementations
10. **Polymorphism + dynamic binding** enable run-time substitutability of objects with identical interfaces


## Related

- [[Software Engineering Models and Methods Overview]] — All models and methods topics
- [[01_Modeling_Fundamentals]] — Architecture concepts
- [[05_Distributed_and_Component]] — Distributed architectures
- [[06_Real_Time_and_Product_Lines]] — Real-time and quality attributes
