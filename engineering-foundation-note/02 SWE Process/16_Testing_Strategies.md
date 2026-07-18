---
tags: [testing, verification, validation, software-engineering]
source: "SWE Theory & Practice — Roger Pressman & Bruce Maxim, Ch 8–9"
---

# Testing Strategies

> **Source:** *Software Engineering: A Practitioner's Approach* by Roger S. Pressman & Bruce R. Maxim — Ch 8 (Testing the Programs) and Ch 9 (Testing the System)

---

## Core Philosophy of Testing

> **Testing aims to find faults, not to prove correctness.** The absence of detected faults does not guarantee correctness.

Key distinctions:
- **Fault** — a defect in requirements, design, code, documentation, or test cases
- **Failure** — a problem in the functioning of the system at runtime
- A fault causes a failure only when the right conditions are met; latent faults may never manifest during testing

### Fault-Discovery Efficiency

Olsen (1993) tracked faults in an 184K-LOC system and found that **no single technique catches all problems**:

| Activity | Faults Found |
|---|---|
| System design inspection | 17.3% |
| Component design inspection | 19.1% |
| Code inspection | 15.1% |
| Integration testing | 29.4% |
| System & regression testing | 16.6% |
| Post-deployment (field) | 0.1% |

> **Lesson:** Use a diverse portfolio of techniques — inspections, reviews, prototyping, and dynamic testing — to catch different fault types.

---

## Unit Testing (Component-Level)

### Structural (White-Box) Testing

Tests based on the internal structure of the code:

- **Statement testing** — execute every statement at least once
- **Branch testing** — exercise every decision outcome (true and false)
- **Path testing** — traverse all possible paths through the program

| Coverage Type | What It Guarantees |
|---|---|
| Statement coverage | Every line of code executed |
| Branch coverage | Every decision point taken both ways |
| Path coverage | Every combination of decision outcomes |

> For N decision points each with 2 branches → up to 2^N paths. Program structure (loops, sequential decisions) can reduce this number significantly.

### Functional (Black-Box) Testing

Tests based on the requirements specification without knowledge of internal structure:
- Derive test cases from input/output specifications
- Exercise valid and invalid inputs
- Check boundary conditions and equivalence classes

### Combining Approaches

Neither structural nor functional testing alone is sufficient. Structural testing may miss missing functionality; functional testing may miss unreachable code or structural faults.

---

## Integration Testing

When individual components pass unit testing, they are combined into a working system. The **order** and **strategy** of combining components directly impacts fault isolation.

### Bottom-Up Integration

1. Test lowest-level components individually using **component drivers** (test harnesses that call the component)
2. Combine tested components with the next level up
3. Continue until all components are integrated

**Best for:**
- General-purpose utility routines
- Object-oriented systems
- Large numbers of reusable components

**Drawback:** Top-level (controlling) components are tested last — major design faults discovered late.

### Top-Down Integration

1. Test the top-level controlling component alone, using **stubs** (simulated responses for called components)
2. Replace stubs with actual tested components, retest
3. Continue downward until all components integrated

**Advantages:**
- Major design faults found early
- No drivers needed
- Test cases defined in terms of system functions

**Drawbacks:**
- Writing stubs can be complex (must handle all possible conditions)
- Many stubs may be needed when bottom level has many utility routines

### Modified Top-Down

Components at each level are tested individually before merging — combines isolation benefits of bottom-up with early design validation of top-down. **Requires both stubs and drivers.**

### Big-Bang Integration

All components tested in isolation, then thrown together at once. **Not recommended** — faults are extremely difficult to isolate.

### Sandwich Integration

System viewed as three layers:
- **Top layer** → tested top-down
- **Bottom layer** → tested bottom-up
- **Target layer** → where the two strategies converge

Combines advantages of both approaches. Modified sandwich testing also tests upper-level components individually before merging.

### Comparison of Integration Strategies

| Criterion | Bottom-Up | Top-Down | Modified Top-Down | Big-Bang | Sandwich |
|---|---|---|---|---|---|
| Integration time | Early | Early | Early | Late | Early |
| Basic working program | Late | Early | Early | Late | Early |
| Drivers needed | Yes | No | Yes | Yes | Yes |
| Stubs needed | No | Yes | Yes | Yes | Yes |
| Path testing ease | Easy | Hard | Easy | Easy | Medium |

### Microsoft's Synch-and-Stabilize

Microsoft's market-driven approach:
- Small parallel teams (3–8 developers) implementing features
- Daily builds with feature integration
- Milestones defined by feature priority (critical → desirable → least critical)
- Buffer time for unexpected complications; least important features cut if schedule compressed

---

## Testing Object-Oriented Systems

### What Makes OO Testing Different

**Easier aspects:**
- Modularity → smaller, focused components
- Encapsulation → clearer interfaces
- Small methods → simpler unit tests

**Harder aspects:**
- **Inheritance** → inherited methods must be retested when subclass is modified
- **Polymorphism / dynamic binding** → the actual method executed depends on runtime type
- **Complex interfaces** → integration complexity shifts toward interfaces
- More integration testing needed overall

### Key OO Testing Concerns

1. **State tracking** — develop tests that track an object's state transitions
2. **Concurrency & synchronization** — beware of race conditions in concurrent objects
3. **Inherited method retesting** — required when:
   - Method is redefined in subclass
   - Method has specific behavior in derived class
   - Other functions in the class must be consistent with inherited behavior
4. **Reuse doesn't eliminate testing** — "a program adequately tested in isolation may not be adequately tested in combination" (Perry & Kaiser 1990)
5. **Coverage metrics differ** — traditional cyclomatic complexity is less useful; interaction-based coverage is more important

### OO Testing Challenges

- Few tools support requirements validation expressed as objects and methods
- Test case generators don't handle OO models well
- Source code metrics defined for procedural code are of limited value
- Code coverage of individual methods is less important than **interaction coverage**

---

## Test Planning

### Six Steps of Test Planning

1. **Establish test objectives** — what kinds of faults to look for
2. **Design test cases** — representative, thorough exercise of functions
3. **Write test cases** — implement as executable scripts
4. **Test test cases** — verify correctness and coverage before execution
5. **Execute tests** — run and capture results
6. **Evaluate test results** — compare actual vs. expected outcomes

### Test Plan Contents

- Test objectives for each level (unit → functional → acceptance → installation)
- Test strategy and methods
- Integration/build plan
- Criteria for determining test completeness
- Detailed test cases with input data and expected outcomes
- Automated tool requirements
- Schedule, personnel, and resource needs

> The test plan is developed **as the system is developed** — not after. The testing perspective often encourages questioning the nature of the problem and the appropriateness of the design.

---

## When to Stop Testing

### Fault Seeding (Mills 1972)

Estimate remaining faults by intentionally inserting a known number of seeded faults:

```
N = S × n / s
```
Where:
- S = number of seeded faults
- N = estimated indigenous (nonseeded) faults
- s = seeded faults found
- n = indigenous faults found

**Limitation:** Assumes seeded faults are representative of actual faults — difficult to guarantee.

### Two-Group Method

Use two independent test groups. Let:
- x = faults found by Group 1
- y = faults found by Group 2
- q = faults found by both groups

Estimated total faults: `n = q / (E₁ × E₂)` where E₁ = q/y, E₂ = q/x

### Confidence Level

Confidence that the software is fault-free:

```
Cₑ = S / (S - N + 1)    if n ≤ N
Cₑ = 1                   if n > N
```

> Example: To claim fault-free at 98% confidence → seed 49 faults and find all 49 without uncovering any indigenous fault.

### Coverage-Based Stopping

Track statement, branch, and path coverage using automated tools. Stop when target coverage is achieved (e.g., 100% statement coverage was required for the London air traffic control system).

### Identifying Fault-Prone Code

**Classification tree analysis** (Porter & Selby 1990) — statistical technique that identifies which measurements best predict fault-prone components:
- Size (LOC), number of decisions, nesting depth, coupling, cohesion, number of modifications
- Example: Components with 100–300 LOC and ≥15 decisions, or >300 LOC with no design review and ≥5 changes, are fault-prone

---

## Automated Testing Tools

### Static Analysis (program not running)

| Tool Type | Function |
|---|---|
| Code analyzer | Syntax checking, fault-prone constructions |
| Structure checker | Logic flow graph, structural flaw detection |
| Data analyzer | Interface checking, conflicting data definitions |
| Sequence checker | Event ordering validation |

### Dynamic Analysis (program running)

- **Program monitors** — capture execution state, statement/branch/path coverage
- **Breakpoints** — stop execution when conditions met
- **Trace tools** — forward/backward control flow and data change tracking

### Test Execution Tools

- **Capture-and-replay** — record keystrokes and responses, compare expected vs. actual
- **Automated stubs and drivers** — generate test harnesses automatically
- **Test environments** — integrate databases, measurement tools, code analysis, and simulation

### Test Case Generators

- **Structural generators** — based on source code structure (paths, branches)
- **Functional generators** — based on function/state coverage
- **Combinatorial generators** (e.g., AETG) — cover all pairwise or n-way parameter combinations

---

## System Testing

### Objective Shift

| Level | Focus |
|---|---|
| Unit testing | Code implements design correctly |
| Integration testing | Components work together |
| System testing | System does what the **customer** wants |

### Sources of System Faults

Faults can be introduced at **any** point:
- Requirements — ambiguous, incorrect, or missing
- System design — misinterpretation of requirements
- Program design — misinterpretation of higher-level design
- Implementation — syntax/semantic errors
- Maintenance — new faults introduced when fixing old ones
- Documentation — unclear or incorrect user guidance

### System Testing Steps

```
Integrated modules → Function test → Performance test → Acceptance test → Installation test → SYSTEM IN USE
```

1. **Function testing** — verifies integrated system performs functions as specified in requirements
2. **Performance testing** — compares system against nonfunctional requirements (speed, security, reliability)
3. **Acceptance testing** — customer-led evaluation confirming the system meets their needs
4. **Installation testing** — verifies system works correctly at the actual deployment site

---

## Function Testing

Also called **thread testing** — focuses on functionality, ignoring system structure (closed-box approach).

### Cause-and-Effect Graphs (IBM)

Converts natural-language requirements into formal test cases:

1. Separate requirements into individual functions
2. Identify **causes** (inputs) and **effects** (outputs/transformations)
3. Draw Boolean relationships (AND, OR, NOT, NAND, NOR)
4. Convert graph to a **decision table** — each column = one test case
5. Eliminate redundant and low-priority combinations

**Benefits:**
- Non-redundant test cases
- Discovers incomplete/ambiguous requirements
- Predicts outcomes and unintended side effects

**Limitation:** Not practical for systems with time delays, iterations, or feedback loops.

---

## Performance Testing

Tests the **nonfunctional** requirements:

| Test Type | What It Evaluates |
|---|---|
| **Stress tests** | System behavior at maximum capacity (peak load) |
| **Volume tests** | Handling of large data sets, max field/file sizes |
| **Configuration tests** | All software/hardware configurations meet requirements |
| **Compatibility tests** | Interface functions with other systems |
| **Regression tests** | New system ≥ old system performance |
| **Security tests** | Availability, integrity, confidentiality |
| **Timing tests** | Response time and function execution time |
| **Environmental tests** | Heat, humidity, motion, power disruption tolerance |
| **Quality tests** | Reliability, maintainability, availability metrics |
| **Recovery tests** | Response to fault, data loss, or resource loss |
| **Maintenance tests** | Diagnostic tools and procedures function correctly |
| **Documentation tests** | Required documents exist, are accurate and readable |
| **Human factors tests** | Usability — displays, messages, ease of use |

---

## Reliability, Availability, and Maintainability

### Definitions

| Attribute | Definition | Scale |
|---|---|---|
| **Reliability** | Probability system operates without failure under given conditions for a time interval | 0–1 |
| **Availability** | Probability system is operating correctly at a given instant | 0–1 |
| **Maintainability** | Probability maintenance can be completed within stated time using stated resources | 0–1 |

### Key Measures

- **MTTF** — Mean Time to Failure (average interfailure time)
- **MTTR** — Mean Time to Repair
- **MTBF** = MTTF + MTTR

Derived measures:
```
R = MTTF / (1 + MTTF)        Reliability
A = MTBF / (1 + MTBF)        Availability
M = 1 / (1 + MTTR)           Maintainability
```

### Failure Severity Levels (MIL-STD-1629A)

1. **Catastrophic** — death or system loss
2. **Critical** — severe injury or mission loss
3. **Marginal** — minor injury or mission degradation
4. **Minor** — unscheduled maintenance only

### Hardware vs. Software Reliability

| Aspect | Hardware | Software |
|---|---|---|
| Failure cause | Physical (corrosion, wear) | Latent fault + triggering conditions |
| Repair effect | Returns to previous reliability | May increase or decrease reliability |
| Goal | **Stability** | **Reliability growth** |

### Reliability Prediction Models

**Jelinski–Moranda Model** (earliest, best-known):
- Assumes perfect corrections (no new faults introduced)
- Each fault contributes equally to reliability improvement
- Uses exponential distribution for fault discovery times

**Littlewood Model** (more realistic):
- Each fault's contribution is an independent random variable (gamma distribution)
- Doubly stochastic — handles both types of uncertainty
- Uses Pareto distribution for fault discovery

**Motorola Zero-Failure Testing:**
- Determines how many test hours needed after last failure to meet reliability goal
- Formula: `[ln(failures/(0.5 + failures))] × hours-to-last-failure / ln[(0.5 + failures)/(test-failures + failures)]`

### Operational Profile & Statistical Testing

- **Operational profile** — probability distribution of likely user inputs over time
- **Statistical testing** — test data reflects the operational profile
- Benefits: concentrates on most-used paths; reliability predictions match user experience

---

## Acceptance Testing

Customer-led testing to verify the system meets their needs and expectations.

### Types of Acceptance Tests

| Type | Description |
|---|---|
| **Benchmark test** | Customer prepares representative test cases; system evaluated against criteria. Used to compare competing implementations. |
| **Pilot test** | System installed experimentally; users exercise it in everyday work. Less formal than benchmark. |
| **Alpha test** | In-house pilot test before customer release |
| **Beta test** | Customer's pilot test with selected sites |
| **Parallel test** | New system runs alongside old system; users compare results gradually |

> Acceptance testing often reveals requirements that were incomplete, ambiguous, or that changed during development. It is the customer's chance to verify that what was wanted is what was built.

---

## Installation Testing

Final testing at the actual user site:
- Configure system to user environment (devices, communications, files, access)
- Regression tests verify proper installation
- Focus on completeness and site-specific functional/nonfunctional characteristics
- Example: naval system must be tested aboard actual ships, not just at the developer's site

---

## Configuration Management During Testing

### Versions and Releases

- **Version** — a configuration for a particular platform or situation
- **Release** — an improved system replacing a previous one
- Version tracking is essential — working with wrong code listing wastes time and may introduce new faults

### Change Control

- Configuration management team approves all changes during testing
- Changes must be reflected in requirements, design, code, documentation, and test plans
- **Check-out/check-in** prevents concurrent developers from overwriting each other's fixes

### Regression Testing

Applied to new versions/releases to verify that:
1. New code changes work correctly
2. Previously working functions are not broken
3. Essential functions from prior version still work

> **California billing disaster:** 167,000 customers billed $667,000 for unwarranted calls because a software upgrade to a telephone switch used the wrong area code — caught by regression testing would have prevented this.

### Version Control Approaches

| Approach | Mechanism | Pros | Cons |
|---|---|---|---|
| **Separate files** | Each version stored independently | Simple | Storage, synchronization |
| **Deltas** | Store differences from baseline | Compact, single-point changes | Complex maintenance, main file corruption risk |
| **Conditional compilation** | Single file with compiler-directed branches | One correction applies to all | Readability degrades with complexity |

---

## Testing Safety-Critical Systems

Safety-critical systems require additional rigor:

- **Fault tolerance** — system must continue operating (possibly in degraded mode) despite faults
- **Redundancy** — multiple independent paths to critical functions
- **Formal verification** — mathematical proof of correctness for critical components
- **Extensive coverage requirements** — e.g., 100% statement coverage mandated by contract
- **Independent test teams** — not involved in design or implementation
- **Failure mode analysis** — exhaustively catalog failure modes and their consequences

### Ariane-5 Lessons

The Ariane-5 failure demonstrated that:
- **No test verified SRI behavior under Ariane-5 trajectory** — the specification didn't include Ariane-5 trajectory data
- Reviews were conducted at all levels but **failed to notice inadequate test coverage**
- A nonexpert reviewer might have questioned assumptions others took for granted
- Root cause was in **requirements**, not code — it could have been caught much earlier

> **Key principle:** For safety-critical systems, mandatory declaration of limitations should be part of every requirements document. Test coverage must be assessed for completeness, not assumed.

---

## Test Documentation

### Document Hierarchy

| Document | Purpose |
|---|---|
| **Test Plan** | Overall strategy, objectives, schedule, resources |
| **Test Specification & Evaluation** | Requirements traced to specific tests; evaluation criteria |
| **Test Description** | Test data and procedures for individual tests |
| **Test Analysis Report** | Results of each test execution |

### Test Plan Components

1. **Objectives** — guide management and technical effort
2. **Document references** — traceability to requirements, design, and code
3. **System summary** — context for readers not involved in earlier phases
4. **Major tests** — function, performance, acceptance, installation
5. **Schedule** — milestones, locations, pre-test requirements
6. **Materials needed** — deliverables, site-supplied resources, training

---

## Key Takeaways

- Testing is **risk management** — you can never test everything, so prioritize based on fault-proneness, criticality, and usage patterns
- **Diverse techniques** catch diverse faults — inspections + structural testing + functional testing + system testing
- **Integration strategy** affects fault isolation capability — choose based on system structure and project constraints
- **OO systems** require more extensive integration testing despite easier unit testing
- **Regression testing** is not optional — it prevents new faults from hiding behind fixed ones
- **Reliability** is probabilistic and measured over execution time; models predict but never guarantee
- **Acceptance testing** is the customer's reality check — requirements are often incomplete until users work with the system
- **Automated tools** are essential for handling the volume and complexity of modern testing
- **Test early, test often** — faults found late are exponentially more expensive to fix

---

## See Also

- [[02 SWE Process/10_SE_Fundamentals_and_Process|10_SE_Fundamentals_and_Process]] — Software process and life cycle models
- [[01 Physics and Math/08_Engineering_Materials|08_Engineering_Materials]] — Quality attributes and engineering trade-offs
