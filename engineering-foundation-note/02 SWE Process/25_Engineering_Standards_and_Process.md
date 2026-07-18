---
tags: [standards, engineering-process, iso, ieee, engineering-foundations, swebok]
---

# Engineering Standards and Process

> *Source: SWEBOK v4 Engineering Foundations KA*

## Purpose

Standards define minimum requirements for quality, safety, and interoperability. The engineering process provides the systematic framework for applying them. This note covers the standards landscape and the generic engineering process that underpins all engineering disciplines.

---

## Part 1: Engineering Standards

### What Are Standards?

Definitional documents establishing requirements, specifications, or guidelines for acceptable quality. Standards are critical for engineers — conformance divides organizations/products into compliant and non-compliant.

> **Standards represent minimum requirements** — often legally mandated — and must be incorporated as design constraints.

### Standards Organizations

| Organization | Full Name | Scope |
|---|---|---|
| **ISO** | International Organization for Standardization | International standards across all industries |
| **IEC** | International Electrotechnical Commission | Electrical, electronic, and related technologies |
| **IEEE** | Institute of Electrical and Electronics Engineers | Electrical engineering, computer science, electronics |
| **ITU** | International Telecommunication Union | Telecommunications and ICT |
| **ANSI** | American National Standards Institute | US national standards coordination |
| **ASTM** | American Society for Testing and Materials | Materials, products, systems, services testing |
| **SAE** | Society of Automotive Engineers | Automotive, aerospace, commercial vehicles |
| **UL** | Underwriters Laboratories | Product safety testing and certification |

### Key Software Engineering Standards

| Standard | Title | Scope |
|---|---|---|
| **ISO/IEC/IEEE 12207** | Software Life Cycle Processes | Defines all software life cycle processes |
| **ISO/IEC/IEEE 15288** | System Life Cycle Processes | Defines all system life cycle processes |
| **ISO/IEC/IEEE 29148** | Requirements Engineering | Requirements specification and management |
| **ISO/IEC/IEEE 42010** | Architecture Description | Architecture documentation framework |
| **ISO/IEC/IEEE 29119** | Software Testing | Test processes, documentation, techniques |
| **ISO/IEC 25010** | SQuaRE | Software product quality model |
| **ISO/IEC 27001** | Information Security | ISMS requirements |
| **ISO 9001** | Quality Management | QMS requirements |
| **ISO 31000** | Risk Management | Risk management framework |
| **IEEE 730** | Software Quality Assurance | SQA processes |
| **IEEE 828** | Software Configuration Management | SCM plans |
| **IEEE 1012** | Software Verification and Validation | V&V processes |
| **IEEE 1044** | Software Anomaly Classification | Defect classification standard |
| **IEEE 1540** | Software Risk Management | Risk management processes |

### How Standards Are Made

1. **Proposal** — Industry need identified
2. **Committee formation** — Experts from relevant stakeholders
3. **Drafting** — Consensus-based development
4. **Review** — Public comment period
5. **Approval** — Voting by member bodies
6. **Publication** — Released as official standard
7. **Revision** — Periodic updates (typically 5-year cycle)

> **Consensus process:** Standards are not written by a single author — they represent the agreement of all participating stakeholders.

### Using Standards in Practice

**Do:**
- Identify which standards apply to your project early
- Incorporate standards as design constraints
- Stay current with revisions
- Use standards as minimum requirements, not maximum aspirations
- Document compliance evidence

**Don't:**
- Ignore applicable standards (may be legally required)
- Treat standards as checklists without understanding intent
- Assume compliance equals quality
- Apply standards that don't fit your context

---

## Part 2: The Generic Engineering Process

### The Five-Step Framework

All engineering disciplines share a common iterative process:

```
┌─────────────────────────────────────────────────────┐
│                                                     │
│  1. Understand the Real Problem                     │
│     ↓                                               │
│  2. Define Selection Criteria                       │
│     ↓                                               │
│  3. Identify All Feasible Solutions                 │
│     ↓                                               │
│  4. Evaluate Each Against Criteria                  │
│     ↓                                               │
│  5. Select Preferred Option & Monitor Performance   │
│     │                                               │
│     └──────→ Feedback → Iterate ←───────────────────│
│                                                     │
└─────────────────────────────────────────────────────┘
```

### Step 1: Understand the Real Problem

> **"The problem you're asked to solve ≠ the problem that needs solving."**

**Activities:**
- Stakeholder analysis — who cares about this problem?
- Root cause analysis — what's actually causing the symptom?
- Problem framing — how you frame the problem determines what solutions you consider
- Requirements elicitation — what does the customer actually need?

**Tools:** 5-whys, Ishikawa diagrams, stakeholder maps, problem statements

### Step 2: Define Selection Criteria

**Activities:**
- Identify constraints (budget, time, technology, regulatory)
- Define quality attributes (performance, reliability, security, usability)
- Establish priorities (what's most important?)
- Create measurable criteria (how will we know a solution is good?)

**Tools:** MoSCoW prioritization, weighted scoring, quality attribute scenarios

### Step 3: Identify All Feasible Solutions

> **"The first solution that comes to mind is rarely optimal."**

**Activities:**
- Brainstorm alternatives
- Research existing solutions (COTS, open source, reuse)
- Consider build vs buy vs reuse
- Explore different architectural approaches

**Tools:** Brainstorming, decision matrices, architecture alternatives analysis

### Step 4: Evaluate Each Against Criteria

**Activities:**
- Analyze trade-offs (cost vs quality vs time)
- Prototype risky alternatives
- Conduct risk assessment for each option
- Use quantitative analysis where possible

**Tools:** Trade-off analysis, cost-benefit analysis, risk matrices, decision trees

### Step 5: Select Preferred Option & Monitor Performance

**Activities:**
- Make the decision (document rationale)
- Implement the solution
- Monitor real-world performance
- Collect feedback for next iteration

> **All engineering depends on estimates; estimates can be wrong.** Ongoing monitoring and iteration are essential.

**Tools:** Earned value analysis, performance metrics, feedback loops, retrospective analysis

### The Process Is Iterative

Knowledge gained at any step may trigger revisiting earlier steps:

- Monitoring reveals the problem was misunderstood → back to Step 1
- Evaluation shows no solution meets criteria → revise criteria (Step 2) or find more solutions (Step 3)
- Implementation reveals new constraints → back to Step 2

### Root Cause Analysis in the Engineering Process

RCA serves four roles:

1. **Understanding the real problem** (Step 1) — What's actually wrong?
2. **Defining criteria** (Step 2) — What does "fixed" look like?
3. **Evaluating solutions** (Step 4) — Does this solution address the root cause?
4. **Monitoring** (Step 5) — Did the fix actually work?

### Engineering Process vs Software Process

| Aspect | Generic Engineering Process | Software Process |
|---|---|---|
| **Scope** | All engineering disciplines | Software-specific |
| **Focus** | Problem-solving framework | Development lifecycle |
| **Iterations** | Knowledge-driven revisits | Sprint/iteration-based |
| **Deliverables** | Physical products + documentation | Software + documentation |
| **Standards** | ISO/IEC/IEEE 15288 (systems) | ISO/IEC/IEEE 12207 (software) |

> **The generic engineering process underpins all software process models.** Waterfall, spiral, agile — they all implement these five steps differently, but the steps are always present.

## Essential Concepts

- **Standards are minimum requirements** — often legally mandated, not optional
- **Consensus-based development** — standards represent stakeholder agreement
- **The five-step engineering process is universal** — applies to bridges, software, and everything in between
- **"Understand the real problem" is step one** — the most common engineering failure is solving the wrong problem
- **Multiple feasible solutions must be considered** — the first idea is rarely optimal
- **Monitor real-world performance** — estimates can be wrong; feedback is essential
- **The process is iterative** — knowledge gained at any step may trigger revisiting earlier steps

## Related

- [[Engineering Foundation Overview]] — All engineering foundation topics
- [[02 SWE Process/20_Root_Cause_Analysis|20_Root_Cause_Analysis]] — RCA techniques for Step 1
- [[02 SWE Process/21_Measurement_Theory|21_Measurement_Theory]] — Measurement for defining criteria (Step 2) and monitoring (Step 5)
- [[02 SWE Process/10_SE_Fundamentals_and_Process|10_SE_Fundamentals_and_Process]] — Software-specific process models
- [[02 SWE Process/18_Evaluation_and_Improvement|18_Evaluation_and_Improvement]] — Process evaluation and improvement
