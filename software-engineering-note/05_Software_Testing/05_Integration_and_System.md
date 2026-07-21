---
tags:
  - testing
  - integration
  - system-testing
  - software-testing
source: "Jorgensen Ch 13-14"
---

# 05 Integration & System Testing

> **Source:** Jorgensen, P.C., *Software Testing: A Craftsman's Approach*, 4th Edition, CRC Press. Chapters 13–14.
> **Scope:** Integration testing strategies (decomposition-based, call graph-based, path-based) and system testing (threads, use cases, event-driven models, operational profiles, risk-based testing, nonfunctional testing).

---

## Part I: Integration Testing (Ch 13)

Integration testing is the least well-understood of the three testing levels (unit, integration, system). The classic cautionary tale: in 1999, the Mars Climate Orbiter failed because Lockheed Martin used English units (pounds) while JPL used metric (newtons) — a failure integration testing should have caught.

The goal of integration testing shifts from testing *interfaces* among separately tested units to testing *interactions* ("co-functioning") among them. Interfaces are structural; interaction is behavioral.

### Overview of Strategies

| Strategy | Basis | Tests Interfaces | Tests Co-Functionality | Fault Isolation |
|---|---|---|---|---|
| Functional decomposition | Decomposition tree | Acceptable but can be deceptive | Limited to pairs of units | Good, to faulty unit |
| Call graph | Call graph | Acceptable | Limited to pairs of units | Good, to faulty unit |
| MM-path | Execution paths | Excellent | Complete | Excellent, to faulty unit execution path |

---

### 13.1 Decomposition-Based Integration

Based on the **functional decomposition tree** derived from the final source code. All strategies assume units have been separately tested — the goal is testing interfaces among them.

#### Top-Down Integration

Begins with the main program (root of tree). Lower-level units appear as **stubs** — throwaway code that emulates a called unit by returning hard-coded correct responses.

**Process:**
1. Develop stubs for all units called by main
2. Test main program functionality
3. Replace one stub at a time with actual code (breadth-first traversal)
4. On failure → fault must be in the most recently replaced stub's interface

**Drawbacks:**
- Significant stub development effort (consider maintaining stubs under configuration management)
- The decomposition tree creates **impossible interfaces** — main program never directly calls leaf-level units like `isLeap` or `weekDay`, so those test sessions are empty

#### Bottom-Up Integration

Mirror image of top-down: stubs are replaced by **driver modules** that emulate units at the next level up.

**Process:**
1. Start with leaves of the decomposition tree
2. Use driver versions of calling units to supply test cases
3. Gradually replace drivers as units are integrated
4. Less throwaway code, but impossible interfaces still persist

#### Sandwich Integration

Combines top-down and bottom-up. A sandwich is a full path from root to leaves. Semantically coherent units are integrated together, but:
- No stubs or drivers needed
- Fault isolation is sacrificed (essentially "medium bang" on a subtree)
- Missing units in a sandwich path won't be covered (e.g., `isLeap` missing means end-of-February tests won't run)

#### Big Bang Integration

All units compiled and tested at once. When a failure occurs, few clues exist to isolate the fault. **Not recommended.**

#### Pros and Cons

| Pros | Cons |
|---|---|
| Intuitively clear — build with tested components | Structural basis presumes correct behavior follows from correct units + correct interfaces |
| Failure → suspect most recently added unit | Significant stub/driver development + retesting effort |
| Progress easily tracked against decomposition tree | Decomposition tree is artificial; serves project management more than developers |

---

### 13.2 Call Graph–Based Integration

Resolves the "impossible interfaces" problem by using the **call graph** instead of the decomposition tree. Nodes = units; edges = actual execution-time calls.

#### Pairwise Integration

Eliminates stub/driver development: use the actual code, but restrict each session to **one edge (pair) in the call graph.**

| Aspect | Detail |
|---|---|
| **Sessions** | One per edge in the call graph |
| **Fault isolation** | High — if a test fails, fault must be in one of the two units |
| **Drawback** | For units in multiple pairs, a fix in one pair may break another |

#### Neighborhood Integration

Borrows the topological notion of a **neighborhood** (radius 1): the set of all immediate predecessor and successor nodes of a given node.

**Computing neighborhoods:**
```
Interior nodes = nodes − (source nodes + sink nodes)
Neighborhoods = interior nodes + source nodes
             = nodes − sink nodes
```

| Aspect | Detail |
|---|---|
| **Sessions** | Usually fewer than pairwise |
| **Stub/driver effort** | Reduced |
| **Fault isolation** | Difficulties of "medium bang" integration |
| **Build sequences** | Neighborhoods can define builds — merge adjacent neighborhoods for composition-based growth |
| **Retesting burden** | Nodes with high indegree/outdegree appear in many neighborhoods; changing one forces retesting of all affected neighborhoods |

#### Pros and Cons

| Pros | Cons |
|---|---|
| Moves from purely structural toward behavioral basis | Fault isolation problem, especially for large neighborhoods |
| Matches well with composition-based development | Node appearing in many neighborhoods → retesting cascade |
| Neighborhoods support build/merge sequences | Structural testing doesn't guarantee correct system-level behavior |

---

### 13.3 Path-Based Integration (MM-Paths)

The goal: an integration testing analog of DD-paths that represents actual system behavior.

#### Core Definitions

| Term | Definition |
|---|---|
| **Source node** | Statement fragment where program execution begins or resumes |
| **Sink node** | Statement fragment where execution terminates (end of unit, or transfer of control to another unit) |
| **Module execution path** | Sequence of statements from a source node to a sink node, with no intervening sink nodes |
| **Message** | Programming language mechanism by which one unit transfers control to another and acquires a response |
| **MM-path** | Interleaved sequence of **m**odule execution paths and **m**essages (or method–message in OO) |

MM-paths always represent feasible execution paths crossing unit boundaries. In procedural software, they begin and end in the main program.

**Message quiescence** — when a unit that sends no further messages is reached — provides natural endpoints for MM-paths.

#### MM-Path Graph

Given a set of units, their **MM-path graph** is a directed graph where:
- Nodes = module execution paths
- Edges = messages and returns between units

This directly supports composition-based integration testing.

#### Relationship to DD-Paths

- A program path is a sequence of DD-paths
- An MM-path is a sequence of module execution paths
- No simple relationship exists between DD-paths and module execution paths — they may partially overlap
- The intersection of an MM-path with a unit is analogous to a *slice* with respect to the MM-path function

#### Sufficiency Criterion

The set of MM-paths should **cover all source-to-sink paths** in the set of units. Loops are handled via condensation graphs (producing DAGs), resolving the infinite-path problem.

#### Pros and Cons

| Pros | Cons |
|---|---|
| Hybrid of functional + structural testing — all functional techniques are applicable | More effort needed to identify MM-paths |
| Avoids the pitfall of purely structural testing | Offset by elimination of stub/driver development |
| Seamless junction with system testing (threads) | |
| Works equally well for waterfall and composition-based lifecycles | |
| Applies directly to object-oriented software (method–message) | |

---

### 13.4 Recommendations

| Priority | Strategy | When to Use |
|---|---|---|
| **1st choice** | MM-path–based | When thread-level behavior coverage is the goal |
| **Fallback** | Call graph–based (pairwise or neighborhood) | When MM-path identification effort is prohibitive |
| **Avoid** | Decomposition-based | Only if call graph is not available |

---

## Part II: System Testing (Ch 14)

System testing evaluates a product against *expectations* — behavior, not code. The craftsman views system testing in terms of **threads of system-level behavior.**

### 14.1 Threads and Atomic System Functions

Threads unify the three levels of testing:
- **Unit thread:** execution-time path of source instructions (sequence of DD-paths)
- **Integration thread:** MM-path (alternating module execution paths + messages)
- **System thread:** sequence of **Atomic System Functions (ASFs)**

#### Definitions

| Term | Definition |
|---|---|
| **Atomic System Function (ASF)** | An action observable at the system level in terms of port input and output events |
| **Event quiescence** | System is (nearly) idle, waiting for a port input event — natural endpoint for ASFs |
| **ASF graph** | Directed graph where nodes = ASFs, edges = sequential flow |
| **Source ASF** | ASF appearing as a source node in the ASF graph |
| **Sink ASF** | ASF appearing as a sink node in the ASF graph |
| **System thread** | Path from a source ASF to a sink ASF in the ASF graph |

ASFs represent the **seam between integration and system testing** — the largest item for integration testing, the smallest for system testing.

---

### 14.2 Requirements Specification Basis Concepts

Every system can be modeled in terms of five fundamental concepts:

| Concept | Description | Thread Identification |
|---|---|---|
| **Data** | Variables, structures, records, files, data stores. E/R models are most common. | Relationships (1:1, 1:N, M:N) imply thread patterns; read-only data indicates source ASFs |
| **Actions** | Transforms, processes, activities, tasks, methods. Have inputs/outputs (data or port events). | I/O view = basis of specification-based testing; decomposition = basis of code-based testing |
| **Devices** | Port devices — sources/destinations of system I/O. Physical-to-logical translation points. | Define input/output spaces for testing; help identify all screens/states |
| **Events** | System-level inputs/outputs on port devices. Discrete or continuous. Can be context-sensitive. | Context-sensitive events must be tested in each context |
| **Threads** | Sequences of actions. Least frequently used in specifications — testers must find them. | Direct source of system test cases |

The five concepts are in **many-to-many relationships** — testers must use events and threads to ensure all relationships are correct.

---

### 14.3 Model-Based Thread Identification

#### Finite State Machine Approach

FSMs are the best starting point for finding system testing threads. Use a **hierarchy of state machines:**

1. **Upper level:** Macro-states representing stages of processing; transitions triggered by abstract logical events
2. **Mid level:** Decomposed states with port output events; inputs still abstract
3. **Lower level:** True port input events drive transitions; port output events as actions

**Generating test cases:** follow a path of transitions, noting port inputs and outputs along the way. For FSMs with loops, replace loops with two paths (as in program graph testing).

**⚠️ Hard lesson from industry:** A 200+ state FSM generated 3000+ test cases — but one was logically impossible due to a subtle dependency between two states. FSMs require *independent* states; detecting all such dependencies is equivalent to the Halting Problem.

#### Event-Driven Petri Nets (EDPNs)

EDPNs extend ordinary Petri nets with port events (triangles), data places (circles), and transitions (rectangles). Advantages over FSMs:

| Feature | EDPN Advantage |
|---|---|
| **Composition** | Easily composed with other EDPNs |
| **Context-sensitive events** | Multiple input events can fire depending on data place state |
| **Intension/Extension** | One unmarked EDPN → many possible marking sequences (executions) |
| **Analysis** | Wealth of Petri net analytical capabilities (conflict detection, invariants, reachability) |

#### Converting Between Models

- **Use cases → EDPN:** Partially automatable — port events and interleaved order preserved; pre/postconditions → data places. Needs well-formed use cases.
- **FSM → EDPN:** Guaranteed (FSMs are a special case of Petri nets where every transition has one input and one output place).

**Which view is best?**
- **Use cases:** Best for customer/developer communication; limited analysis
- **FSMs:** Common, tools available; state explosion problem
- **EDPNs:** Preferred for system testing — compose well, rich analysis, clean intension/extension mapping

---

### 14.4 Use Case–Based Threads

Use cases capture the *does* view (behavior), not the *is* view (structure).

#### Larman's Hierarchy of Use Cases

| Level | Content | Example (Correct PIN on 1st Try) |
|---|---|---|
| **High level** | Name + brief description | "Customer enters PIN correctly on first attempt" |
| **Essential** | + Event sequence (input/output events) | Sequence of digit entry events and screen responses |
| **Expanded essential** | + Preconditions and postconditions | "Expected PIN is known," "Select Transaction screen is active" |
| **Real** | + Actual data values | "Expected PIN is '2468'" |

#### Well-Formed Use Case Requirements

1. Event sequence cannot begin with an output event (treat as precondition)
2. Event sequence cannot end with an input event (treat as postcondition)
3. Preconditions must be both necessary and sufficient
4. At least one precondition and one postcondition must exist

---

### 14.5 Short vs Long Use Cases

**Long use cases** = full end-to-end transactions (e.g., card entry → PIN → withdraw → close session). Problem: the SATM FSM has **1909 possible paths** — too many.

**Short use cases** = begin with a port input event, end with a port output event. Must be at Expanded Essential level (pre/postconditions known).

Short use cases are linked by **pre/postcondition matching:** use case B can follow A if A's postconditions are consistent with B's preconditions.

**Compression power:** 1909 long use cases covered by **25 short use cases** (12 for successful transactions + 13 for PIN entry failure modes).

---

### 14.6 How Many Use Cases?

Four strategies using **incidence matrices** to determine sufficiency:

| Strategy | Matrix | Sufficiency Criterion |
|---|---|---|
| **Incidence with input events** | Use cases × port input events | When existing input events suffice for any new use case |
| **Incidence with output events** | Use cases × port output events | When all screens/outputs are covered |
| **Incidence with all port events** | Combined input + output | Reorderable into cohesive subgroups (e.g., PIN-related events) |
| **Incidence with classes** | Use cases × classes | When a sufficient set of classes has been identified |

---

### 14.7 Coverage Metrics for System Testing

#### Model-Based Coverage

| Model | Coverage Levels |
|---|---|
| **Decision tables** | Every condition → every action → every rule |
| **Finite state machines** | Every state → every transition → every path |
| **Petri nets** | Every place → every transition → every sequence of transitions |

Model-based metrics serve as a **cross-check** on use case–based threads (pseudostructural testing).

#### Specification-Based Coverage — Port Input Events

| Metric | Description |
|---|---|
| **Port Input 1** | Each port input event occurs — bare minimum |
| **Port Input 2** | Common sequences of port input events occur — most common, corresponds to "normal use" |
| **Port Input 3** | Each port input event occurs in every "relevant" data context — addresses context-sensitive events |
| **Port Input 4** | For a given context, all "inappropriate" input events occur — testing proscribed behavior |
| **Port Input 5** | For a given context, all possible input events occur — exhaustive per context |

#### Specification-Based Coverage — Port Output Events

| Metric | Description |
|---|---|
| **Port Output 1** | Each port output event occurs — acceptable minimum |
| **Port Output 2** | Each port output event occurs for each cause — good goal, hard to quantify |

#### Port-Based Coverage

For each port, exercise events that can occur at that port. Complements event-based testing: event-based covers 1:N (events → ports), port-based covers 1:N (ports → events).

---

### 14.8 Operational Profiles

**Zipf's law:** 80% of thread executions traverse only 20% of the threads. Operational profiles determine execution frequencies to prioritize high-traffic threads when test time is limited.

#### Process

1. Estimate **transition probabilities** on the FSM
2. Compute **path probabilities** (product of transition probabilities along the path)
3. Sort paths from most to least probable
4. Test in descending probability order

**Estimating probabilities:** historical data from similar systems, customer-supplied estimates, or Delphi method (group of experts, eliminate extremes).

#### Sensitivity Analysis

Overall ordering of probabilities is not particularly sensitive to small variations in individual transition estimates.

#### Secondary Uses

Operational profiles also support early performance estimation and system transaction capacity analysis via simulators.

---

### 14.9 Risk-Based Testing

Refinement of operational profiles: some obscure threads may have catastrophic consequences.

**Basic definition:**
```
Risk = Cost × Probability of Occurrence
```

#### Schaefer's Risk Categories

| Category | Cost Weight (logarithmic) | SATM Examples |
|---|---|---|
| **Catastrophic** | 10 | Deposits, invalid withdrawals |
| **Damaging** | 3 | Normal withdrawals |
| **Hindering** | 3 | Invalid ATM card, PIN entry failure |
| **Annoying** | 1 | Balance inquiries |

**Why logarithmic weighting?** Linear scales (1–5) don't create enough distinction in subjective assessments.

#### Refinement

Assign multiple weighted attributes to each use case (e.g., customer convenience, bank security, identity theft) for a composite risk score.

---

### 14.10 Nonfunctional System Testing

Nonfunctional testing addresses *how well* a system performs its functional requirements — the "-abilities": reliability, maintainability, scalability, usability, compatibility.

#### Stress Testing Strategies

| Strategy | Description | Example |
|---|---|---|
| **Compression** | Reduce traffic/device counts proportionally to manageable test sizes | 50,000 BHCAs → 500 call attempts with 50 DTMF receivers (vs. 5000) |
| **Replication** | Reproduce extreme conditions indirectly when direct testing would destroy the system | Parachute drop → 10-foot forklift drop; bird strike → chicken cannon |

#### Mathematical Approaches

| Approach | Application |
|---|---|
| **Queuing theory** | Models servers, queues, arrival rates, service times. Single queue + multiple servers = most efficient. |
| **Reliability models** | Computes MTTF, MTBF, MTTR from component failure rates. Good for hardware; software doesn't deteriorate. |
| **Monte Carlo testing** | Randomly generate large numbers of threads/transactions. Last resort when other approaches fail. |

**Key insight:** Software, once well tested, does not deteriorate like physical components. Testing based on operational profiles and risk-based testing is a good start, but no amount of testing can guarantee the absence of software faults.

---

## Key Takeaways

1. **Integration testing** should be based on the call graph, not the decomposition tree — decomposition trees create impossible interfaces. MM-paths are the gold standard but require more effort.

2. **System testing** is about threads — sequences of Atomic System Functions from source to sink. ASFs are the seam between integration and system testing.

3. **Use cases** are the best communication tool, but EDPNs are the best analysis tool for system testing — they handle composition, context-sensitive events, and conflict detection.

4. **Short use cases** (linked by pre/postconditions) dramatically compress the testing space — 1909 paths covered by just 25 short use cases.

5. **Operational profiles** prioritize high-traffic threads; **risk-based testing** adds cost-of-failure weighting. Use both when test time is constrained.

6. **Nonfunctional testing** requires domain-specific strategies — compression, replication, or mathematical modeling. There's no one-size-fits-all approach.

---

## Sources

- Jorgensen, P.C., *Software Testing: A Craftsman's Approach*, 4th Edition, CRC Press, 2014. Chapters 13 (Integration Testing) and 14 (System Testing).
- Schaefer, H., Risk-Based Testing, 2005.
- Larman, C., *Applying UML and Patterns*, 2001.


## Related

- [[Software Testing Overview]] — All testing topics
- [[06_Model_Based_and_Lifecycle]] — Model-based testing
- [[07_OO_and_Complexity]] — OO testing and complexity
