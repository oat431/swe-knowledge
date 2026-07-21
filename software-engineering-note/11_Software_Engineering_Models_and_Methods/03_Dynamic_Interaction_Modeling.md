---
tags:
  - modeling
  - dynamic-modeling
  - state-machines
  - interaction-diagrams
  - software-engineering
source: "Gomaa, Software Modeling and Design, Chapters 9–11"
created: 2026-07-21
---

# Dynamic Interaction Modeling (Gomaa Ch 9–11)

## Overview

Dynamic modeling provides a view of a system in which **control and sequencing** are considered, either:

- **Between objects** — by analysis of object interactions (Ch 9, Ch 11)
- **Within an object** — by means of a finite state machine (Ch 10)

Dynamic interaction modeling is based on the realization of use cases. For each use case, we determine how the objects that participate in it dynamically interact with each other.

Two fundamental flavors:

| Type | Chapter | Key Mechanism |
|------|---------|---------------|
| **Stateless** (basic) dynamic interaction modeling | Ch 9 | Objects collaborate via messages; no state memory needed |
| **State-dependent** dynamic interaction modeling | Ch 11 | At least one state-dependent control object executes a statechart (Ch 10) that governs interactions |

---

## Chapter 9: Stateless Dynamic Interaction Modeling

### 9.1 Object Interaction Modeling

For each use case, the objects that realize it dynamically cooperate and are depicted on one of two UML diagram types:

#### Communication Diagrams
- Show objects, their links, and **numbered messages** indicating sequence
- The message sequence numbering corresponds to the interaction sequence between actor and system described in the use case
- **COMET preference**: communication diagrams over sequence diagrams, because integrating them is easier when transitioning to the software architecture (Ch 13)
- Example structure: `A1: Operator Request → A1.1: Alarm Request → A1.2: Alarm → A1.3: Display Info`

#### Sequence Diagrams
- Show object interactions arranged in **time sequence** (top to bottom)
- Clear sequential ordering but harder to see object connectivity
- Can depict loops (`loop`), alternatives (`alt`), and references (`ref`)
- Messages are often numbered to show correspondence to communication diagrams

#### Key Design Decisions (Deferred to Design Phase)
- Whether a message is synchronous or asynchronous
- Whether an object is active or passive
- Which operation is invoked by a message arrival
- During analysis: all messages are shown as **simple messages**

#### Scenarios vs Use Cases
- A **scenario** is one specific path through a use case
- An interaction diagram typically depicts a scenario (instance form)
- The **instance form** shows one possible sequence; the **generic form** shows all alternatives with loops, branches, and conditions
- Instance form is usually clearer for all but the simplest use cases

### 9.2 Message Sequence Numbering

Message label syntax (analysis phase):
```
[sequenceExpression]: MessageName (argumentList)
```

#### Sequence Number Conventions
- **Simple**: `1, 2, 3, ...` or `M1, M2, M3, ...`
- **Dewey decimal**: `A1, A1.1, A1.1.1, A1.2, ...` — external events are whole numbers, internal events are decimal extensions
- **Use case identifier**: Optional letter prefix (e.g., `V1` for View Alarms)
- **Recurrence indicators**:
  - `* [iteration-clause]` — repeated execution, e.g., `3* [i := 1,n]`
  - `[condition-clause]` — conditional branch, e.g., `4 [x < n]` or `5 [Normal]`

#### Concurrent Sequences
- Lowercase letter suffix indicates concurrency: `A3` and `A3a` are concurrent
- Subsequent messages: `A3a.1, A3a.2, ...`

#### Alternative Sequences
- Uppercase letter suffix: `1.4 [Normal]` (main) vs `1.4A [Error]` (alternative)
- Each alternative branch gets its own subsequence numbering

#### Message Sequence Description
- Supplementary **narrative documentation** accompanying each interaction diagram
- Describes what each destination object does when it receives a message
- Uses the same message sequence numbers as the diagram
- Provides detail not visible on the diagram (e.g., which attributes are referenced)

### 9.3 Dynamic Interaction Modeling Approach

An iterative strategy to determine how analysis model objects interact. For each use case:
1. Determine objects that participate (using object structuring criteria from Ch 8)
2. Analyze how these objects collaborate to execute the use case
3. This analysis may reveal the need for additional objects and/or interactions

### 9.4 Four Steps of Stateless Dynamic Interaction Modeling

1. **Develop use case model** (from Ch 6). Consider each actor-system interaction. Start with the main path scenario. The actor starts the interaction with an external input; the system responds with internal execution and typically a system output.

2. **Determine objects needed to realize the use case**:
   - **2a. Determine boundary object(s)**: A software boundary object (user interaction object, input object) receives the external input and typically sends a message to an internal object.
   - **2b. Determine internal software objects**: Using object structuring criteria — control objects, entity objects, service objects, etc.

3. **Determine message communication sequence**: Draw an interaction diagram showing objects and message sequence. Start with external input from actor → sequence of internal messages → external output to actor. Assign message sequence numbers.

4. **Determine alternative sequences**: Consider error handling and other alternatives from the use case. Determine additional objects and message sequences needed for each alternative branch.

### 9.5 Examples

#### Example 1: View Alarms (Simple)
- Two objects: `OperatorInteraction` (user interaction) + `AlarmService` (service)
- Message flow: `A1: Operator Request → A1.1: Alarm Request → A1.2: Alarm → A1.3: Display Info`

#### Example 2: Make Order Request (Service-Oriented)
- Objects: `CustomerInteraction`, `CustomerCoordinator`, `CustomerAccountService`, `CreditCardService`, `DeliveryOrderService`, `EmailService`
- Main sequence: Order Request → Account Info → Credit Card Authorization → Store Order → Confirmation + Email
- Alternative 1 (No Account): `M4A [no account]` → Create new account sequence (`M4A.1–M4A.8`)
- Alternative 2 (Credit Card Denied): `M6A [denied]` → Denial notification (`M6A.1–M6A.2`)
- Generic communication diagram combines all three scenarios using alternative message numbering

---

## Chapter 10: Finite State Machines

Finite state machines model the **control and sequencing view** of a system or object. In highly state-dependent systems (e.g., real-time systems), FSMs provide crucial insight into system complexity.

In UML, a state transition diagram is called a **state machine diagram** (statechart). A state-dependent object is always in one of the states of its FSM.

### 10.1 Events and States

#### Events
- An **event** is an occurrence at a point in time (atomic, zero duration conceptually)
- Also called: discrete event, discrete signal, stimulus
- Can be **external** (e.g., `Card Inserted` from user) or **internal** (e.g., `Valid PIN` generated by system)
- Events can depend on each other (e.g., `Card Inserted` always precedes `PIN Entered`)

#### States
- A **state** represents a recognizable situation that exists over an **interval of time**
- The arrival of an event usually causes a transition from one state to another
- Some states represent **waiting** (e.g., `Waiting for PIN` — waiting for external input)
- Others represent **processing** (e.g., `Validating PIN` — waiting for internal response)
- **Initial state**: entered when the state machine is activated (depicted by arc from black circle)

### 10.2 Statechart Examples

#### ATM Statechart
- **Main sequence**: `Idle → [Card Inserted] → Waiting for PIN → [PIN Entered] → Validating PIN → [Valid PIN] → Waiting for Customer Choice`
- **Alternative transitions from Validating PIN**: Valid PIN, Invalid PIN (re-enter PIN), Third Invalid PIN/Stolen/Expired (Confiscating)
- **Same event, different states**: `Cancel` in Waiting for PIN, Validating PIN, or Waiting for Customer Choice → all transition to Ejecting
- **Same event, different effect**: `PIN Entered` in Idle is ignored, but in Waiting for PIN causes transition
- **Complete withdrawal scenario**: Withdrawal Selected → Processing Withdrawal → [Approved] → Dispensing → [Cash Dispensed] → Printing → [Receipt Printed] → Ejecting → [Card Ejected] → Terminating → [after Elapsed Time] → Idle

#### Microwave Oven Statechart
- States: `Door Shut → Door Open → Door Open with Item → Door Shut with Item → Ready to Cook → Cooking`
- Door opening during cooking: transitions back to `Door Open with Item`

### 10.3 Events and Guard Conditions

A state transition can be made **conditional** using a **guard condition**:

```
Event [Condition]
```

- The condition is a Boolean expression (True/False) that holds for some time
- When the event arrives, the transition occurs **only if** the guard condition is True
- Conditions are optional
- An event's impact can be stored as a condition for later checking (e.g., `Zero Time` vs `Time Remaining` in microwave)

### 10.4 Actions

An **action** is a computation that executes as a result of a state transition. An event is the **cause**; an action is the **effect**. Actions execute instantaneously (conceptually zero duration).

#### Transition Actions
- Labeled on the transition: `Event / Action` or `Event [Condition] / Action`
- Example: `Card Inserted / Get PIN`, `PIN Entered / Validate PIN`, `Valid PIN / Display Menu`
- **Multiple actions** on one transition: all execute simultaneously — must have no interdependencies!
- If sequential dependency exists, introduce an intermediate state

#### Entry Actions
- Depicted as `entry / Action` inside the state box
- **When to use**: the same action must execute on **every** transition into the state
- Example: `Cooking` state has `entry / Start Cooking` because both `Start` and `Minute Pressed` transitions need it
- More concise than repeating the action on each incoming transition

#### Exit Actions
- Depicted as `exit / Action` inside the state box
- **When to use**: the same action must execute on **every** transition out of the state
- Example: `Cooking` state has `exit / Stop Cooking` because both `Timer Expired` and `Door Opened` transitions need it
- More concise than repeating the action on each outgoing transition

### 10.5 Hierarchical Statecharts

Hierarchical statecharts solve the problem of state/transition proliferation (clutter) through **composite states** (superstates) and hierarchical decomposition.

#### Key Concepts
- A **composite state** at one level is decomposed into two or more **substates** on a lower-level statechart
- Also called **sequential state decomposition** (substates execute sequentially)
- Any hierarchical statechart can be mapped to a semantically equivalent flat statechart

#### Depiction
- Composite state shown as outer rounded box with name at top left; substates as inner rounded boxes
- Alternatively, composite state as a **black box** (substates not revealed)

#### Transition Rules
- Transition **into** a composite state = transition into one (and only one) substate
- Transition **out of** a composite state = transition from one (and only one) substate
- Aggregating transitions at the composite level simplifies the diagram (e.g., `Cancel` transitions aggregated out of `Processing Customer Input` instead of each substate)

#### ATM Hierarchical Statechart Example
- **Top-level** (Fig 10.18): `Idle`, `Processing Customer Input` (composite), `Processing Transaction` (composite), `Terminating Transaction` (composite), `Closed Down`
- **Processing Customer Input** (Fig 10.19): `Waiting for PIN`, `Validating PIN`, `Waiting for Customer Choice`
- **Processing Transaction** (Fig 10.20): `Processing Withdrawal`, `Processing Transfer`, `Processing Query`
- **Terminating Transaction** (Fig 10.21): `Dispensing`, `Printing`, `Ejecting`, `Confiscating`, `Terminating`

### 10.7–10.8 Developing Statecharts from Use Cases

The process:

1. **Develop statechart for each use case** — construct statechart for the main sequence of each use case
2. **Consider alternative sequences** — add transitions for alternative/error scenarios from the use case
3. **Develop integrated statechart** — merge use case–based statecharts at integration points (e.g., `Waiting for Customer Choice` is the end state of Validate PIN and the start state of Withdraw Funds)
4. **Develop hierarchical statechart** — identify states that can be aggregated into composite states. Look for situations where aggregating transitions simplifies the statechart.

Two approaches to hierarchical decomposition:
- **Top-down**: identify major high-level states (modes of operation), then decompose
- **Bottom-up**: develop flat statechart first, then aggregate into composite states

---

## Chapter 11: State-Dependent Dynamic Interaction Modeling

State-dependent dynamic interaction modeling involves object interactions where at least one object is a **state-dependent control object** that executes a statechart (Ch 10). The statechart provides the **overall control and sequencing** of interactions with other objects.

### Key Distinction
- **Ch 9** (Stateless): Objects collaborate without state memory — same input always produces same response
- **Ch 11** (State-Dependent): Response depends on **current state** of the control object — same input may produce different responses in different states

### Object Roles in State-Dependent Interactions
1. **State-dependent control object** — executes the state machine (statechart)
2. **Boundary objects** — send events to the control object (cause state transitions)
3. **Action/activity objects** — receive output from the control object (triggered by state transitions)
4. **Other participating objects** — entity objects, service objects, etc.

### 11.1 Six Steps of State-Dependent Dynamic Interaction Modeling

1. **Determine boundary object(s)** — objects that receive inputs from external environment
2. **Determine the state-dependent control object** — at least one, which executes the statechart; more may be needed for complex interactions
3. **Determine other software objects** — objects that interact with the control object or boundary objects
4. **Determine object interactions in the main sequence scenario** — in conjunction with step 5
5. **Determine the execution of the statechart** — define states, events, conditions, and actions triggered by the control object
6. **Consider alternative sequence scenarios** — perform state-dependent dynamic analysis on alternative sequences from the use case

### 11.2 Consistency Between Interaction Diagrams and Statecharts

To ensure **consistency** between the two views:

- A **message** on the interaction diagram corresponds to an **event** on the statechart
- An **output message** from the control object corresponds to an **action** on the statechart (transition action, entry action, or exit action)
- The equivalent message and event **must have the same name**
- The **same message-numbering sequence** must be used on both diagrams

**Flow**: Source object → input event → state-dependent control object → state transition → output event(s) → destination object(s)

### 11.3 Banking System Example: Validate PIN Use Case

#### Objects Identified
- `CardReaderInterface` (external I/O boundary) — reads ATM card
- `ATMCard` (entity) — stores card data temporarily
- `CustomerInteraction` (user interaction) — keyboard/display interaction
- `ATMTransaction` (entity) — stores transaction data for PIN validation
- `ATMControl` (state-dependent control) — executes the statechart
- `BankingService` (subsystem) — validates PIN

#### Main Sequence (Valid PIN)

```
1: Card Reader Input → 1.1: Card Data → 1.2: Card Inserted → 1.3: Get PIN → 1.4: PIN Prompt
2: PIN Input → 2.1: Card Request → 2.2: Card Data → 2.3: Prepare Transaction → 2.4: PIN Validation Transaction
→ 2.5: PIN Entered → 2.6: Validate PIN → 2.7 [Valid]: Valid PIN → 2.8: Display Menu, 2.8a: Update Status → 2.9: Selection Menu
```

#### Statechart for Valid PIN Scenario
```
Idle → [1.2: Card Inserted / 1.3: Get PIN] → Waiting for PIN
→ [2.5: PIN Entered / 2.6: Validate PIN] → Validating PIN
→ [2.7 [Valid]: Valid PIN / 2.8: Display Menu, 2.8a: Update Status] → Waiting for Customer Choice
```

#### Alternative Sequences

| Scenario | Condition | Response |
|----------|-----------|----------|
| Invalid PIN (1st/2nd attempt) | `2.7A* [Invalid]` | Invalid PIN Prompt → re-enter PIN → Validate again (loop max 2 times) |
| Third Invalid PIN | `2.7B [ThirdInvalid]` | Confiscate card → Confiscating state |
| Stolen or Expired Card | `2.7C [Stolen OR expired]` | Confiscate card (same as Third Invalid) |
| Cancel | `Cancel` (from any substate) | Eject card → Ejecting state |

#### Generic Interaction Diagram
- All alternatives can be shown on a single **generic communication diagram** (or sequence diagram)
- Uses alternative message numbering: `2.7 [Valid]`, `2.7A [Invalid]`, `2.7B [ThirdInvalid]`, `2.7C [Stolen OR expired]`
- Use only if alternatives can be clearly depicted; otherwise, use separate diagrams per scenario

#### Concurrent Actions
- At the Valid PIN transition, actions `2.8: Display Menu` and `2.8a: Update Status` execute **concurrently**
- All actions at a given transition execute in an unconstrained, nondeterministic order
- Depicted with concurrent message sequences (e.g., `2.8` and `2.8a`)

---

## Key Relationships Across Chapters

```
Use Case (Ch 6)
    │
    ├──→ Stateless Dynamic Interaction (Ch 9)
    │       └── Communication/Sequence Diagrams
    │
    ├──→ Finite State Machines (Ch 10)
    │       └── Statecharts (flat → hierarchical)
    │
    └──→ State-Dependent Dynamic Interaction (Ch 11)
            └── Interaction Diagrams + Statecharts (consistent)
```

### When to Use Each Approach

| Situation | Approach |
|-----------|----------|
| Simple request-response interactions | Ch 9: Stateless dynamic interaction modeling |
| Object behavior depends on what happened before | Ch 10: Model with finite state machine (statechart) |
| Interactions governed by a state-dependent controller | Ch 11: State-dependent dynamic interaction modeling (combine interaction diagrams + statecharts) |
| Multiple control objects in complex system | Ch 11: Each state-dependent control object with its own statechart; interactions between them via messages |

### Design Phase Transition
- During analysis, all messages are **simple messages** (no sync/async decision)
- During design (Ch 12–13): message interfaces are defined, communication diagrams are synthesized into an **integrated communication diagram** — the first step in developing the software architecture


## Related

- [[Software Engineering Models and Methods Overview]] — All models and methods topics
- [[02_Use_Case_and_Static_Modeling]] — Use cases and static modeling
- [[01_Modeling_Fundamentals]] — UML notation and fundamentals
