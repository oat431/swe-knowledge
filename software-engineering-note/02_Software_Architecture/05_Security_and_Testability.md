---
tags:
  - architecture
  - security
  - testability
  - usability
  - tactics
  - software-architecture
---

# Security, Testability & Usability

> **Source:** Bass, Clements & Kazman — *Software Architecture in Practice* (SAiP), Chapters 9–11
> **Purpose:** Cover the general scenarios, architectural tactics, and design checklists for security, testability, and usability — three quality attributes that are often in tension but all critical to system success.

---

## 1. Security (Ch 9)

Security is a measure of the system's ability to protect data and information from unauthorized access while still providing access to authorized persons and systems. Attacks can be characterized as threats against:

- **Confidentiality** — data is protected from unauthorized access
- **Integrity** — data is delivered as intended, without unauthorized modification or deletion
- **Availability** — the system is accessible to those entitled to use it

### 1.1 Security General Scenario

| Portion | Possible Values |
|---|---|
| **Source** | Human or another system — correctly identified, incorrectly identified, or unknown. Attacker may be external or internal. |
| **Stimulus** | Unauthorized attempt to display data, change/delete data, access system services, change system behavior, or reduce availability. |
| **Artifact** | System services, data within the system, a component or resource, data produced or consumed by the system. |
| **Environment** | Online or offline; connected or disconnected from network; behind firewall or open; fully/partially/not operational. |
| **Response** | Data/services are protected from unauthorized access; data not manipulated without authorization; parties identified with assurance; non-repudiation; availability for legitimate use; system tracks activities (recording access/modification/attempts, notifying appropriate entities). |
| **Response Measure** | How much of the system is compromised; time before attack detected; number of attacks resisted; time to recover; amount of data vulnerable. |

### 1.2 Security Tactics

The tactics are organized into four categories: **Detect**, **Resist**, **React**, and **Recover**.

#### Detect Attacks

| Tactic | Description |
|---|---|
| **Detect Intrusion** | Compare network traffic or service request patterns to signatures/known patterns of malicious behavior (protocol, TCP flags, payload sizes, applications, addresses, ports). |
| **Detect Service Denial** | Compare incoming traffic patterns to historic profiles of known denial-of-service attacks. |
| **Verify Message Integrity** | Use checksums or hash values to verify integrity of messages, resource files, deployment files, and configuration files. |
| **Detect Message Delay** | Check message delivery time to detect man-in-the-middle attacks where transit time is highly variable. |

#### Resist Attacks

| Tactic | Description |
|---|---|
| **Identify Actors** | Identify the source of any external input — user IDs, access codes, IP addresses, protocols, ports. |
| **Authenticate Actors** | Ensure an actor is actually who/what it purports to be. Passwords, one-time passwords, digital certificates, biometric identification. |
| **Authorize Actors** | Ensure an authenticated actor has rights to access/modify data or services. Access control by actor, actor class, groups, roles, or individual lists. |
| **Limit Access** | Control what/who may access which parts — process management, memory protection, blocking hosts, closing ports, rejecting protocols. Firewalls, DMZ. |
| **Limit Exposure** | Reduce probability of successful attack or restrict potential damage. Conceal facts ("security by obscurity"), distribute critical resources across locations. |
| **Encrypt Data** | Protect data from unauthorized access. Symmetric (same key) or asymmetric (public/private keys). VPN, SSL/TLS for communication links. |
| **Separate Entities** | Physical separation (different servers/networks), virtual machines, air gaps, separating sensitive from non-sensitive data. |
| **Change Default Settings** | Force users to change default settings to prevent attacks through publicly known defaults. |

#### React to Attacks

| Tactic | Description |
|---|---|
| **Revoke Access** | Severely limit access to sensitive resources when an attack is believed underway, even for normally legitimate users. |
| **Lock Computer** | Limit access from a particular computer after repeated failed login attempts (often time-limited). |
| **Inform Actors** | Notify operators, personnel, or cooperating systems when an attack is detected. |

#### Recover from Attacks

- **Restore services** — leverage availability tactics (Chapter 5); keep additional servers/connections in reserve.
- **Maintain audit trail** — record user and system actions and their effects to trace/identify attackers, support prosecution, and improve future defenses.

### 1.3 Security Tactics Tree

```
Security Tactics
├── Detect Attacks
│   ├── Detect Intrusion
│   ├── Detect Service Denial
│   ├── Verify Message Integrity
│   └── Detect Message Delay
├── Resist Attacks
│   ├── Identify Actors
│   ├── Authenticate Actors
│   ├── Authorize Actors
│   ├── Limit Access
│   ├── Limit Exposure
│   ├── Encrypt Data
│   ├── Separate Entities
│   └── Change Default Settings
├── React to Attacks
│   ├── Revoke Access
│   ├── Lock Computer
│   └── Inform Actors
└── Recover from Attacks
    ├── Restore (see Availability)
    └── Maintain Audit Trail
```

---

## 2. Testability (Ch 10)

Testability refers to the ease with which software can be made to demonstrate its faults through (typically execution-based) testing. Industry estimates indicate that 30–50%+ of the cost of developing well-engineered systems is taken up by testing — so any architectural improvement to testability yields a large payoff.

A system is testable when it is possible to **control** each component's inputs (and possibly internal state) and **observe** its outputs (and possibly internal state). This is frequently done through a **test harness** — specialized software designed to exercise the software under test.

### 2.1 Testability General Scenario

| Portion | Possible Values |
|---|---|
| **Source** | Unit testers, integration testers, system testers, acceptance testers, end users — manually or using automated testing tools. |
| **Stimulus** | A set of tests is executed due to completion of a coding increment (class/layer/service), completed integration, full implementation, or delivery to customer. |
| **Artifact** | A unit of code (module), a subsystem, or the whole system. |
| **Environment** | Design time, development time, compile time, integration time, deployment time, run time. May include test harness or test environments. |
| **Response** | The system can be controlled to perform desired tests and results from the test can be observed. |
| **Response Measure** | Effort to find a fault or class of faults; effort to achieve given percentage of state space coverage; probability of fault being revealed by next test; time to perform tests; length of longest dependency chain; time/effort to prepare test environment; reduction in risk exposure ($\text{size(loss)} \times \text{prob(loss)}$). |

### 2.2 Testability Tactics

Two categories: **Control and Observe System State** and **Limit Complexity**.

#### Control and Observe System State

| Tactic | Description |
|---|---|
| **Specialized Interfaces** | Provide testing interfaces to control or capture variable values — set/get methods, report methods returning full object state, reset methods, verbose output/logging toggles. Keep them separate from functional interfaces so they can be removed if needed. |
| **Record/Playback** | Capture state crossing an interface to "play the system back" and re-create faults. Refers to both capturing information and using it as input for further testing. |
| **Localize State Storage** | Store state in a single place so the system can be started in an arbitrary state for testing. Use state machines to externalize and track/report current state. |
| **Abstract Data Sources** | Abstract interfaces so you can substitute test data easily — e.g., point test system at other test databases or test data files without changing functional code. |
| **Sandbox** | Isolate an instance from the real world to enable experimentation with no permanent consequences. Virtualize resources (clock, memory, battery, network). Stubs, mocks, and dependency injection are simple forms. |
| **Executable Assertions** | Hand-coded assertions placed at desired locations to indicate when/where a program is in a faulty state. Pre- and post-conditions, class-level invariants. Effectively embed the test oracle in the code. |

Techniques for replacing components with testable versions:

- **Component replacement** — swap implementation via build scripts
- **Preprocessor macros** — expand to state-reporting code or probe statements
- **Aspects** — handle cross-cutting concern of state reporting (aspect-oriented programming)

#### Limit Complexity

| Tactic | Description |
|---|---|
| **Limit Structural Complexity** | Avoid/resolve cyclic dependencies; isolate/encapsulate dependencies on external environment; reduce dependencies between components. In OO: limit inheritance depth, polymorphism, dynamic calls. Keep "response of a class" metric low. High cohesion, loose coupling, separation of concerns (modifiability tactics) also help. Consider eventual consistency over strict consistency. Layered style enables testing lower layers first. |
| **Limit Nondeterminism** | Find and eliminate sources of nondeterminism. Unavoidable nondeterminism (e.g., multi-threaded response to unpredictable events) can be addressed with record/playback. |

### 2.3 Testability Tactics Tree

```
Testability Tactics
├── Control and Observe System State
│   ├── Specialized Interfaces
│   ├── Record/Playback
│   ├── Localize State Storage
│   ├── Abstract Data Sources
│   ├── Sandbox
│   └── Executable Assertions
└── Limit Complexity
    ├── Limit Structural Complexity
    └── Limit Nondeterminism
```

---

## 3. Usability (Ch 11)

Usability is concerned with how easy it is for the user to accomplish a desired task and the kind of user support the system provides. It encompasses:

- **Learning system features** — help features for unfamiliar users
- **Using a system efficiently** — ability to redirect the system after issuing a command, suspend/resume tasks
- **Minimizing the impact of errors** — cancel incorrectly issued commands
- **Adapting the system to user needs** — auto-fill, personalization
- **Increasing confidence and satisfaction** — feedback, progress indicators

### 3.1 Usability General Scenario

| Portion | Possible Values |
|---|---|
| **Source** | End user, possibly in a specialized role (e.g., system/network administrator). |
| **Stimulus** | End user tries to use a system efficiently, learn to use the system, minimize the impact of errors, adapt the system, or configure the system. |
| **Environment** | Runtime or configuration time. |
| **Artifact** | System or the specific portion of the system with which the user is interacting. |
| **Response** | The system should either provide the user with the features needed or anticipate the user's needs. |
| **Response Measure** | Task time, number of errors, number of tasks accomplished, user satisfaction, gain of user knowledge, ratio of successful operations to total operations, amount of time/data lost when an error occurs. |

### 3.2 Usability Tactics

Tactics are organized by who takes initiative: **user initiative**, **system initiative**, or **mixed**.

> **Key insight:** There is a strong connection between usability and modifiability. Separating the user interface from the rest of the system (via MVC, semantic coherence, encapsulation, deferred binding) facilitates rapid prototyping and iterative UI improvement. MVC has become the dominant separation pattern, now built into a wide variety of frameworks.

#### Support User Initiative

| Tactic | Description |
|---|---|
| **Cancel** | System must have a constant listener (not blocked by the action being canceled); command must be terminated; resources freed; collaborating components informed. |
| **Undo** | Maintain sufficient state information (snapshots/checkpoints or reversible operations) so an earlier state can be restored. Not all operations are easily reversible — some require elaborate records. |
| **Pause/Resume** | For long-running operations, temporarily free resources so they can be reallocated to other tasks. |
| **Aggregate** | Allow grouping of lower-level objects so operations can be applied to the group in one action, reducing repetitive drudgery and potential for mistakes. |

#### Support System Initiative

When the system takes initiative, it relies on models of the user, the task, or itself:

| Tactic | Description |
|---|---|
| **Maintain Task Model** | Determine context so the system knows what the user is attempting and can provide assistance (e.g., auto-correcting lowercase letters at sentence start). |
| **Maintain User Model** | Represent user's knowledge, behavior (expected response time), and other user/class-specific aspects. Enables pacing of interactions, controlling assistance/suggestions. Supports user interface customization. |
| **Maintain System Model** | Explicit model of the system itself — used to determine expected behavior and give appropriate feedback (e.g., progress bar predicting time to complete current activity). |

### 3.3 Usability Tactics Tree

```
Usability Tactics
├── Support User Initiative
│   ├── Cancel
│   ├── Undo
│   ├── Pause/Resume
│   └── Aggregate
└── Support System Initiative
    ├── Maintain Task Model
    ├── Maintain User Model
    └── Maintain System Model
```

---

## 4. Cross-Cutting Observations

### Relationships Between Quality Attributes

| Relationship | Details |
|---|---|
| **Security ↔ Usability** | Often in tension: security imposes procedures/processes that feel like overhead to casual users. The best approach is to make the system easy to use securely — security and usability should go hand in hand. |
| **Testability ↔ Modifiability** | Many testability tactics (high cohesion, loose coupling, separation of concerns) are also modifiability tactics. Limiting complexity benefits both. |
| **Usability ↔ Modifiability** | Strongly complementary: separating the UI (MVC, encapsulation) makes the system both more usable (faster prototyping/iteration) and more modifiable. However, business-rule-driven UI validation can create tension — duplicating rules on client and server helps performance but hurts modifiability. |
| **Testability ↔ Fault Tolerance** | In tension: a testable system "gives up its faults easily"; a fault-tolerant system "jealously hides its faults." Designing for both requires careful balance. |

---

## 5. Design Checklists Summary

### Security Checklist (Table 9.2)

- **Allocation of Responsibilities:** Identify actors, authenticate, authorize, grant/deny access, record attempts, encrypt data, recognize reduced availability, recover from attack, verify checksums/hash values.
- **Coordination Model:** Authenticate/authorize communicating systems, encrypt transmission, monitor for unexpectedly high demands, restrict/terminate connections.
- **Data Model:** Separate data of different sensitivity; different access rights; log sensitive data access; encrypt data and separate keys; enable data restoration.
- **Mapping among Architectural Elements:** Ensure each mapping has responsibilities for identification, authentication, authorization, access control, recording, encryption, and attack recognition/recovery.
- **Resource Management:** Identify/monitor resources for authentication; prevent exhaustion of critical resources; prevent contamination; prevent shared resources from leaking sensitive data.
- **Binding Time:** Qualify late-bound components; manage access by late-bound components; encrypt data and withhold keys from untrusted late-bound components.
- **Choice of Technology:** Technologies for user authentication, data access rights, resource protection, data encryption.

### Testability Checklist (Table 10.2)

- **Allocation of Responsibilities:** Execute test suite and capture results; capture (log) activity resulting in fault; control and observe relevant system state; ensure high cohesion, low coupling, separation of concerns, low structural complexity.
- **Coordination Model:** Support test suite execution and result capture; support activity capture within/between systems; support state injection and monitoring into communication channels; do not introduce needless nondeterminism.
- **Data Model:** Capture values of data abstractions; set values when injecting state; exercise and capture creation/initialization/persistence/manipulation/destruction.
- **Mapping among Architectural Elements:** Test possible mappings (process-to-processor, threads-to-processes, modules-to-components); identify race conditions; test for illegal mappings.
- **Resource Management:** Sufficient resources for test suite execution; representative test environment; ability to test resource limits, capture detailed resource usage, inject new resource limits, provide virtualized resources.
- **Binding Time:** Test late-bound components in late-bound context; capture late bindings for failure re-creation; test full range of binding possibilities.
- **Choice of Technology:** Technologies for regression testing, fault injection, recording and playback; ensure chosen technologies support the level of testing appropriate.

### Usability Checklist (Table 11.2)

- **Allocation of Responsibilities:** Assist user in learning, efficiently achieving tasks, adapting/configuring, recovering from errors.
- **Coordination Model:** Ensure timeliness/currency/completeness/correctness/consistency of coordination does not impair learning, task achievement, adaptation, error recovery, or confidence.
- **Data Model:** Design data abstractions to support undo/cancel; ensure transaction granularity is appropriate.
- **Mapping among Architectural Elements:** Determine which mappings are visible to end user (e.g., local vs. remote services); ensure visibility does not impair usability.
- **Resource Management:** Allow user configuration without negatively affecting task achievement; avoid configurations with excessively long response times.
- **Binding Time:** Give users control over binding time decisions (configuration, communication protocols, plug-ins) without impairing usability.
- **Choice of Technology:** Technologies that aid online help, training materials, user feedback collection; ensure technologies do not adversely affect usability.

---

## 6. Key Takeaways

1. **Security is a lifecycle concern.** No tactic is foolproof — systems will be compromised. The four-part framework of detect → resist → react → recover acknowledges this reality.

2. **Testability is about control and observation.** The architect's primary lever is making system state accessible and manipulable through specialized interfaces, sandboxing, and abstraction — while limiting structural and behavioral complexity.

3. **Usability depends on initiative.** Both user-initiative tactics (cancel, undo, pause/resume, aggregate) and system-initiative tactics (task/user/system models) require architectural support — not just UI polish.

4. **Modifiability is the common thread.** Separating concerns (especially UI from logic), limiting dependencies, and deferring binding benefit all three quality attributes. MVC and similar patterns are cross-cutting enablers.

5. **Trade-offs are real.** Security vs. usability, testability vs. fault tolerance — the architect's job is to make these trade-offs explicit and intentional through scenarios and checklists.


## Related

- [[Software Architecture Overview]] — All architecture topics
- [[02_Quality_Attributes_Overview]] — QA scenarios framework
- [[04_Modifiability_and_Performance]] — Cross-cutting performance tradeoffs
- [[06_Tactics_and_Patterns]] — Related architecture patterns
