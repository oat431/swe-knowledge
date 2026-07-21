---
modeling: []
use-cases: []
static-modeling: []
uml: []
software-engineering: []
source: "Gomaa, Software Modeling and Design, Chapters 6–8"
created: 2026-07-21
---

# 02 — Use Case and Static Modeling

> **Source:** Hassan Gomaa, *Software Modeling and Design: UML, Use Cases, Patterns, and Software Architectures*, Cambridge University Press.  
> **Chapters covered:** Ch 6 (Use Case Modeling), Ch 7 (Static Modeling), Ch 8 (Object and Class Structuring).

---

## 1. Use Case Modeling (Chapter 6)

Use case modeling describes the **functional requirements** of a system from the user's perspective. The system is treated as a **black box** — only external behaviour matters, not internal structure.

### 1.1 Requirements Modeling

Two phases:

| Phase | Activity |
|-------|----------|
| **Requirements Analysis** | Interview users, analyse existing manual/automated systems, identify stakeholder needs. |
| **Requirements Specification** | Produce the SRS (Software Requirements Specification) — the contract between customer and developer. |

A **functional requirement** describes what the system must do (inputs, outputs, stored data).  
A **nonfunctional requirement** (quality attribute) addresses performance, availability, security, etc.

**Quality attributes of a good SRS:** correct, complete, unambiguous, consistent, verifiable, understandable, modifiable, traceable.

### 1.2 Actors

An **actor** is an external entity (outside the system) that interacts with the system. Actors represent **roles**, not individual users.

| Actor Type | Description | Example |
|------------|-------------|---------|
| **Human actor** | End-user interacting via standard I/O devices. | ATM Customer |
| **External system actor** | Another system that interfaces to the system. | Remote System, Supervisory System |
| **Input/Output device actor** | Hardware device with no human involvement. | Monitoring Sensor |
| **Timer actor** | Periodically sends timer events to initiate actions. | Report Timer |

**Primary actor** — initiates the use case.  
**Secondary actor** — participates but does not initiate; gains value from the use case.

Actor **generalization** can model shared roles: e.g., `RemoteSensor` generalises both `MonitoringSensor` and `RemoteSystem`.

### 1.3 Use Cases

A **use case** defines a sequence of interactions between one or more actors and the system. It always starts with input from an actor.

- **Main sequence** — the most common (success) path.
- **Alternative sequences** — branches for error cases, less frequent scenarios.
- **Scenario** — one complete path through the use case (main or alternative).

### 1.4 Documenting a Use Case

Each use case is documented with:

| Section | Content |
|---------|---------|
| **Use case name** | Unique identifier. |
| **Summary** | One- or two-sentence description. |
| **Dependency** | Whether it includes or extends other use cases (optional). |
| **Actors** | Primary actor (always) + secondary actors. |
| **Precondition** | What must be true before the use case starts. |
| **Main sequence** | Narrative of the normal path (actor input → system response). |
| **Alternative sequences** | Branches off the main sequence with conditions. |
| **Nonfunctional requirements** | Performance, security, etc., for this use case. |
| **Postcondition** | What is true after the main sequence completes. |
| **Outstanding questions** | Issues to discuss with users. |

### 1.5 The Include Relationship

When the **same sequence of interactions** appears in several use cases, extract it into an **inclusion use case** (abstract). The original use cases become **base use cases** that `«include»` the inclusion use case.

> **Analogy:** An inclusion use case is like a library routine; a base use case is like a program that calls it.

**Guideline:** Do NOT create inclusion use cases for single functions (e.g., "Dispense Cash" alone) — that leads to functional decomposition and a fragmented model.

**Example:** `Validate PIN` is included by `Withdraw Funds`, `Query Account`, and `Transfer Funds`.

### 1.6 The Extend Relationship

Used when a use case has **too many alternative/optional/exceptional paths**. Split off a conditional branch into an **extension use case**.

- The **base use case** does NOT depend on the extension.
- The **extension use case** depends on the base and executes only when its condition is true.
- **Extension points** name the exact locations in the base use case where extensions can be inserted.

**Example:** `Checkout Customer` has an extension point `payment`. Three extension use cases — `Pay by Cash`, `Pay by Credit Card`, `Pay by Debit Card` — each with a mutually exclusive selection condition (`[cash payment]`, `[credit card payment]`, `[debit card payment]`).

### 1.7 Use Case Packages

For large systems, group related use cases into **use case packages**. Packages often correspond to major actors or functional areas. Nonfunctional requirements that apply to a group can be assigned to the package.

### 1.8 Activity Diagrams for Use Cases

An **activity diagram** provides a more precise description of a use case by showing:

- Activity nodes (one or more use case steps)
- Decision nodes (branching points for alternative sequences)
- Arcs (sequencing)
- Loops (rejoining the main sequence)

Aggregate activity nodes can represent inclusion/extension use cases that are decomposed on a separate diagram.

---

## 2. Static Modeling (Chapter 7)

The **static model** captures the structural view — classes, attributes, relationships — that does not vary with time. Depicted with **UML class diagrams**.

### 2.1 Associations

An **association** is a structural relationship between classes. A **link** is an instance of an association between objects.

**Multiplicity** — how many instances of one class relate to one instance of another:

| Type | Notation | Example |
|------|----------|---------|
| One-to-one | `1` — `1` | Company `Is led by` CEO |
| One-to-many | `1` — `1..*` | Bank `Administers` Account |
| Many-to-many | `*` — `1..*` | Student `Enrolls in` Course |
| Optional (zero-or-one) | `0..1` | Customer `Owns` DebitCard |
| Optional (zero-or-many) | `0..*` | Customer `Owns` CreditCard |
| Numerically specified | `2, 4` | Car `Is entered through` Door |

**Association classes** model associations that have their own attributes (e.g., `Hours` with `hoursWorked` between `Employee` and `Project`).

**Ternary associations** link three classes (e.g., Buyer–Seller–Agent).

**Unary (self) associations** link objects of the same class (e.g., `Person Is child of Person`).

### 2.2 Composition and Aggregation (Whole/Part)

Both model "Is part of" relationships:

| Hierarchy | Strength | Characteristic |
|-----------|----------|----------------|
| **Composition** | Strong | Part objects are created, live, and die with the whole. A part belongs to exactly one whole. Often physical. |
| **Aggregation** | Weaker | Parts can be added/removed independently. A part can belong to multiple aggregates. Often conceptual. |

**Composition example:** ATM is composed of CardReader, CashDispenser, ReceiptPrinter, KeypadDisplay (each part is physically part of one ATM).

**Aggregation example:** College consists of AdminOffice, Departments, ResearchCenters (departments can be created/removed/merged).

> Composition > Aggregation > Association (in strength).

### 2.3 Generalization/Specialization (Inheritance)

An **"Is a"** relationship. Common attributes/operations are abstracted into a **superclass**; subclasses inherit them and add or specialise.

**Discriminator** — an attribute that indicates which property is being abstracted (e.g., `accountType` discriminates `CheckingAccount` from `SavingsAccount`).

**Example:** `Account` (superclass) with `accountNumber`, `balance`. Subclasses: `CheckingAccount` adds `lastDepositAmount`, `SavingsAccount` adds `cumulativeInterest`.

### 2.4 Constraints

A **constraint** specifies a condition or restriction that must be true:

- **Attribute constraint:** `{balance >= 0}` on Account.
- **Association constraint:** `{ordered by time}` on Account → ATMTransaction.

Constraints can be expressed in natural language or OCL (Object Constraint Language).

### 2.5 Static Modeling of the Problem Domain

The COMET approach emphasises modelling:

1. **Physical classes** — tangible real-world entities (devices, users, external systems, timers). Particularly important in embedded/real-time systems.
2. **Entity classes** — conceptual, data-intensive classes (often persistent). Prevalent in information systems.

### 2.6 System Context Modeling

| Context Level | What Is Inside | What Is Outside |
|---------------|---------------|-----------------|
| **Total system** (hardware + software) | I/O devices, software, hardware | Human actors, external systems |
| **Software system** | Software only | Hardware I/O devices, human actors, external systems |

- **System context class diagram** — shows the total system as a black box with external entities.
- **Software system context class diagram** — shows the software system with external classes (including hardware devices).

### 2.7 Categorization of Classes (UML Stereotypes)

Stereotypes distinguish among kinds of classes using guillemets: `«stereotype»`.

**External classes** (from the software system perspective):

| Stereotype | Description |
|------------|-------------|
| `«external user»` | Human user via standard I/O devices. |
| `«external system»` | Another system that communicates with this system. |
| `«external input device»` | Hardware device providing input only (e.g., sensor). |
| `«external output device»` | Hardware device receiving output only (e.g., actuator). |
| `«external I/O device»` | Hardware device for both input and output (e.g., card reader). |
| `«external timer»` | Provides timer events to the system. |

**Standard associations** on context diagrams:
- `Inputs to` — external input device → software system
- `Outputs to` — software system → external output device
- `Interacts with` — external user ↔ software system
- `Communicates with` — external system ↔ software system
- `Signals` — external timer → software system

### 2.8 Entity Classes

Entity classes are **data-intensive**, often **persistent** classes. Their main purpose is to **store data** and provide access to it.

- Entity class modelling is analogous to logical database schema design.
- An entity class should have several attributes; a class with only one attribute is probably not an entity class — it should be an attribute of another class.
- Operations are deferred to design (Chapter 14).

**Example:** Online shopping entity model — `Customer`, `CustomerAccount`, `DeliveryOrder`, `Item`, `Catalog`, `Inventory`, `Supplier`.

---

## 3. Object and Class Structuring (Chapter 8)

After use cases and the static model of the problem domain are defined, the next step is to determine the **software objects** that realise the use cases.

### 3.1 Object Structuring Criteria

Objects are categorised by the **role they play** in the application using stereotypes. There is no single "correct" decomposition — it depends on the problem.

**Four main categories:**

| Category | Role | When Needed |
|----------|------|-------------|
| **Entity** | Stores data; provides access to stored information. | Information-intensive systems. |
| **Boundary** | Interfaces to and communicates with the external environment. | All systems that interact externally. |
| **Control** | Provides overall coordination for a collection of objects. | Complex use cases. |
| **Application logic** | Encapsulates business rules, algorithms, or services. | When logic changes independently of data. |

> It is more important to identify all objects than to agonise over categorising borderline cases.

### 3.2 Boundary Classes and Objects

Boundary objects are the **software interface** to the external environment. Each external class maps to one boundary class inside the system.

#### 3.2.1 User Interaction Objects (`«user interaction»`)
- Communicate with human users via standard I/O (keyboard, display, mouse).
- Can be composite (e.g., multiple windows/widgets).
- Receive user input and display output.

#### 3.2.2 Proxy Objects (`«proxy»`)
- Interface to an **external system** or subsystem.
- Local representative of the external system; hides communication details.
- Example: `Pick & Place Robot Proxy` communicates with the real external robot.

#### 3.2.3 Device I/O Boundary Objects (`«input»`, `«output»`, `«input/output»`)
- Software interface to **hardware I/O devices** (sensors and actuators).
- Only needed for **nonstandard, application-specific** devices (standard devices are handled by the OS).
- For every relevant real-world physical object, there should be a corresponding software object.

| Subcategory | Stereotype | Direction |
|-------------|-----------|-----------|
| Input object | `«input»` | Receives from external input device. |
| Output object | `«output»` | Sends to external output device. |
| I/O object | `«input/output»` | Both receives and sends. |

**Example:** Banking System boundary classes — `CardReader Interface` (I/O), `CashDispenser Interface` (output), `ReceiptPrinter Interface` (output), `Customer Interaction` (user interaction), `Operator Interaction` (user interaction).

### 3.3 Entity Classes and Objects (`«entity»`)

- Software objects that **store information**.
- Often **persistent** (stored in files/databases; survive system shutdown).
- Attributes and relationships determined during static modelling (Chapter 7).
- Provide limited access to data via operations.
- May access other entity objects to update encapsulated data.

**Example:** `Account` entity with `accountNumber`, `balance`. Accessed by multiple use cases (withdraw, query, transfer, open, close, monthly statements).

### 3.4 Control Classes and Objects (`«control»`)

Control objects provide **overall coordination** — the "conductor of the orchestra."

#### 3.4.1 Coordinator Objects (`«coordinator»`)
- **Stateless** decision-making: action depends only on the incoming message, not on history.
- Determines overall sequencing for a use case.
- **Example:** `BankCoordinator` receives ATM transactions and routes to the appropriate transaction manager (`Withdrawal`, `Query`, `Transfer`, `PIN Validation`).

#### 3.4.2 State-Dependent Control Objects (`«state dependent control»`)
- Behaviour **varies by state**; defined by a finite state machine (statechart).
- Output depends on both the input event and the current state.
- Multiple instances of the same class can each be in different states.
- **Example:** `ATMControl` (one instance per ATM) controls cash dispenser and receipt printer.

#### 3.4.3 Timer Objects (`«timer»`)
- Activated by an **external timer** (real-time clock or OS clock).
- Either performs an action itself or activates another object.
- **Example:** `ReportTimer` awakened by `DigitalClock` to trigger `WeeklyReport` preparation.

### 3.5 Application Logic Classes and Objects

Encapsulate application-specific logic separately from data, so logic can change independently.

#### 3.5.1 Business Logic Objects (`«business logic»`)
- Encapsulate **business rules** for processing a client request.
- **Guideline:** Use a separate business logic object when the rule requires accessing **two or more entity objects**. If only one entity is needed, the operation can reside on that entity.
- **Example:** `WithdrawalTransactionManager` checks minimum-balance and daily-withdrawal-limit rules by accessing `Account` and `DebitCard` entities.

#### 3.5.2 Algorithm Objects (`«algorithm»`)
- Encapsulate a **substantial algorithm**, prevalent in real-time/scientific/engineering domains.
- Algorithms are refined independently of the data they manipulate.
- May encapsulate initialisation data, intermediate results, or threshold values.
- **Example:** `Cruiser` in a Train Control System calculates speed adjustments by comparing current speed to desired cruising speed.

#### 3.5.3 Service Objects (`«service»`)
- Provide **services** for client objects, typically in service-oriented architectures.
- Never initiate requests; respond to service requests (may seek help from other services).
- May encapsulate data or access entity objects.
- **Example:** `CatalogService` provides catalog browsing and item selection for the `CustomerCoordinator`.

### 3.6 Relationship Between External and Boundary Classes

| External Class | Communicates With | Boundary Class (Internal) |
|---------------|-------------------|---------------------------|
| `«external user»` | `«user interaction»` | User interaction object |
| `«external system»` | `«proxy»` | Proxy object |
| `«external input device»` | `«input»` | Input object |
| `«external output device»` | `«output»` | Output object |
| `«external I/O device»` | `«input/output»` | I/O object |
| `«external timer»` | `«timer»` | Timer object |

There is usually a **one-to-one association** between an external class and its corresponding internal boundary class.

---

## 4. Summary of the COMET Analysis Workflow (Ch 6–8)

```
Chapter 6           Chapter 7               Chapter 8
─────────          ──────────             ──────────
Use Case       →   Static Modeling    →   Object & Class
Modeling           of Problem Domain       Structuring
(actors, use       (entity classes,        (boundary, entity,
 cases, include/    associations,          control, application
 extend, packages,  context diagrams,      logic objects)
 activity diag.)    external classes)
```

1. **Identify actors and use cases** (Ch 6) — functional requirements.
2. **Develop static model** (Ch 7) — entity classes, associations, system context, external classes.
3. **Structure software objects** (Ch 8) — categorise boundary, entity, control, and application logic objects.
4. **Dynamic interaction modeling** (Ch 9–11) — determine object interactions for each use case.
5. **Class design** (Ch 14) — define operations for each class.


## Related

- [[Software Engineering Models and Methods Overview]] — All models and methods topics
- [[01_Modeling_Fundamentals]] — COMET method and UML overview
- [[03_Dynamic_Interaction_Modeling]] — Dynamic modeling and state machines
