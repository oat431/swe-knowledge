---
tags: [waterfall, v-model, traditional, methodology, software-methodology]
---

# Waterfall and V-Model

> *Source: Winston Royce (1970), German Federal Government V-Model (1980s), ISO/IEC/IEEE 15288*

## Purpose

Waterfall and V-Model are traditional, sequential software development methodologies. While Agile and Lean have become dominant, these approaches remain essential for regulated industries, safety-critical systems, and projects with fixed requirements. Understanding when and why to use them is as important as knowing Agile.

## Waterfall

### Origin
Winston Royce (1970) described a sequential process, though he actually criticized the pure waterfall model in his paper. The industry adopted his diagram as the standard anyway.

### Phases
1. **Requirements** — Gather and document all requirements upfront
2. **Design** — Create system architecture and detailed design
3. **Implementation** — Write the code
4. **Testing** — Verify against requirements
5. **Deployment** — Release to production
6. **Maintenance** — Fix bugs and enhance

### Characteristics
- **Sequential** — Each phase must complete before the next begins
- **Document-heavy** — Each phase produces formal deliverables (SRS, SDD, STD)
- **Fixed scope** — Requirements are baselined early
- **Change control** — Changes require formal change requests
- **Milestone-driven** — Progress measured by phase completion

### Strengths
- Simple and easy to understand
- Predictable timeline and budget
- Well-suited for fixed-price contracts
- Comprehensive documentation
- Clear milestones and accountability

### Weaknesses
- Late feedback (working software comes late)
- High cost of change (requirements are frozen)
- Risk of building the wrong thing
- Long time-to-market
- Assumes requirements can be fully known upfront

### When to Use
- Fixed-scope contracts (government, defense)
- Well-understood requirements
- Small, simple projects
- Teams with limited Agile experience
- Regulatory requirements mandate sequential phases

## V-Model

### Origin
Developed by the German federal government (1980s) as an extension of Waterfall with explicit verification at each level.

### Key Insight
Every development phase has a corresponding verification phase:

```
Requirements      ↔  Acceptance Testing
System Design     ↔  System Testing
Architecture      ↔  Integration Testing
Detailed Design   ↔  Unit Testing
Implementation    ↔  (Code Reviews)
```

### Strengths
- **Early verification planning** — Test plans created during design
- **Traceability** — Every requirement traced to test cases
- **Formal documentation** — Comprehensive, auditable
- **Quality built-in** — Verification at every level
- **Regulatory compliance** — Meets standards like DO-178C, IEC 61508

### Weaknesses
- Still sequential (high change cost)
- Heavy documentation overhead
- Long time-to-market
- Assumes requirements are stable

### When to Use
- Safety-critical systems (avionics, medical, automotive)
- Regulated industries (FDA, FAA, DO-178C)
- Government contracts requiring formal verification
- Systems where failure has severe consequences

## Waterfall vs. V-Model vs. Agile

| Aspect | Waterfall | V-Model | Agile |
|---|---|---|---|
| **Approach** | Sequential | Sequential with verification | Iterative |
| **Requirements** | Fixed upfront | Fixed upfront | Evolving |
| **Testing** | After implementation | Paired with each phase | Continuous (TDD) |
| **Documentation** | Heavy | Very heavy | Lightweight |
| **Change cost** | High | High | Low |
| **Time-to-market** | Long | Long | Short (incremental) |
| **Risk discovery** | Late | Earlier (verification) | Continuous |
| **Best for** | Fixed scope, simple | Safety-critical, regulated | Evolving requirements |
| **Team structure** | Specialized roles | Specialized + verification | Cross-functional |
| **Customer involvement** | Start and end | Start and end | Continuous |

## Documentation by Phase

### Waterfall Documents
| Phase | Document | Purpose |
|---|---|---|
| Requirements | SRS (Software Requirements Specification) | What the system must do |
| Design | SDD (Software Design Document) | How the system will work |
| Implementation | Source Code + Code Comments | The actual software |
| Testing | STD (Software Test Description) | How to verify it works |
| Deployment | Release Notes, Deployment Guide | How to deploy |
| Maintenance | Change Requests, Bug Reports | How to maintain |

### V-Model Documents
| Development Phase | Document | Verification Phase | Document |
|---|---|---|---|
| Requirements | SRS | Acceptance Testing | Acceptance Test Plan |
| System Design | System Design Doc | System Testing | System Test Plan |
| Architecture | Architecture Design Doc | Integration Testing | Integration Test Plan |
| Detailed Design | Detailed Design Doc | Unit Testing | Unit Test Plan |
| Implementation | Source Code | Code Reviews | Review Reports |

## Hybrid Approaches

Many organizations blend Waterfall/V-Model with Agile:

### Water-Scrum-Fall
- **Waterfall** for requirements and deployment
- **Scrum** for implementation and testing
- Common in enterprises transitioning to Agile

### V-Model with Iterations
- V-Model structure for verification
- Iterative development within each phase
- Common in automotive and aerospace

### SAFe (Scaled Agile Framework)
- Portfolio level: Waterfall-like planning
- Team level: Agile sprints
- Common in large enterprises

## Anti-Patterns

| Anti-Pattern | What It Looks Like | Fix |
|---|---|---|
| **Waterfall in sprints** | Long requirements phase, then "sprints" | Actually iterate; deliver working software each sprint |
| **Documentation for documentation's sake** | Writing docs nobody reads | Focus on documentation that adds value (API docs, ADRs) |
| **Big upfront design** | Months of architecture before coding | Design enough to start; iterate on architecture |
| **Requirements freeze** | "No changes allowed after requirements sign-off" | Embrace change; use change control boards |
| **Testing at the end** | All testing happens in the final phase | Shift left; test continuously |

## When Waterfall/V-Model Still Makes Sense

Despite the Agile revolution, these approaches remain valid for:

1. **Regulated industries** — FDA, FAA, DO-178C require formal verification
2. **Safety-critical systems** — Medical devices, avionics, nuclear systems
3. **Fixed-price contracts** — Government contracts with fixed scope
4. **Simple, well-understood projects** — Small scope, clear requirements
5. **Teams new to Agile** — Waterfall is simpler to understand initially
6. **Hardware-software integration** — When hardware timelines are fixed

## Related

- [[00_Agile_Methodology]] — Agile as an alternative approach
- [[01_Lean_Methodology]] — Lean principles that can improve any methodology
- [[02_Methodologies_Overview]] — Comparison across all methodologies
- [[03_Kanban_and_Flow]] — Kanban as a transition from Waterfall
