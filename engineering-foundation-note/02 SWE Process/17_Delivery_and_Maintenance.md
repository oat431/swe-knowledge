---
tags: [delivery, maintenance, documentation, software-engineering, training, legacy-systems, impact-analysis, rejuvenation]
source: "SWE Theory & Practice — Ch 10 (Delivering the System) & Ch 11 (Maintaining the System)"
---

# Delivery and Maintenance

System delivery is not a formality — it is the critical transfer from developer to user that determines whether a well-built system is actually used effectively. Maintenance is the longest and most expensive phase of a system's life.

---

## Part I — Delivering the System (Ch 10)

### Users vs. Operators

| Role | Focus | Analogy |
|---|---|---|
| **User** | Exercises primary system functions (data manipulation, analysis, communication) | Chauffeur — drives the car |
| **Operator** | Performs support functions (backups, access control, device installation) | Mechanic — services the car |

> The same person may fill both roles, but training for each is distinct.

---

### Training

#### User Training
- Focuses on **what** the system does — major functions, navigation, data access
- Must address **task interference**: subtle differences between old and new systems can impede learning
- Users need not understand internal implementation (sorting algorithms, storage locations)

#### Operator Training
- Focuses on **how** the system works — configuration, access control, performance monitoring, recovery
- Two levels: (1) bringing up / running the system, (2) supporting users

#### Special Training Needs
- **Replacement users** — need training on demand, not just at delivery
- **Infrequent users** — only basic functions; knowledge fades without regular use
- **Refresher training** — review modules for functions not exercised regularly
- **Specialized courses** — limited-function training for users who only need a subset

#### Training Aids

| Aid | Description |
|---|---|
| **Documents / Manuals** | Formal reference; only 10–15% of users read them before use |
| **Icons & Online Help** | Interface-driven learning; hypermedia for deep dives |
| **Demonstrations & Classes** | Show-and-try; multimedia reinforcement; verbal + visual + hands-on |
| **Expert Users** | Peers trained in advance; act as classroom helpers and post-training consultants; bridge user–analyst communication |

#### Guidelines for Training
- **Individualize** — accommodate varying backgrounds, experience, keyboard skills
- **Divide into small units** — short sessions beat marathon trainings
- **Location matters** — Web-based / CBT for distributed installations
- **Feedback** — encouragement and correction during training improve retention

---

### Documentation

#### Audience-Specific Documents

| Document | Audience | Content |
|---|---|---|
| **User's Manual** | End users | System purpose → capabilities → one-by-one functional descriptions (screens, inputs, outputs, features). Layered: overview → detail |
| **Operator's Manual** | System operators | Hardware/software configuration, access control, peripheral management, backup/recovery procedures |
| **General System Guide** | Customer / management | Nontechnical overview; cross-references to user and operator manuals |
| **Programmer's Guide** | Maintenance team | Technical counterpart to user's manual; component descriptions, diagnostic tools, debugging support, cross-references to user manual |
| **Tutorials / Automated Overviews** | All users | Step-by-step guided exercises; document + interactive program |

#### User Help & Troubleshooting

| Type | Purpose |
|---|---|
| **Failure Message Reference Guide** | Lists all system failure messages with full explanations; cross-referenced by message number |
| **Online Help** | Context-sensitive; menu item or function key; can link to deeper documentation |
| **Quick Reference Guide** | 1–2 page summary of primary functions; kept at workstation; function keys, common commands |

#### Key Documentation Principles
- **Plan from the start** — documentation is a system feature, developed alongside the product
- **Write user manuals from requirements** at the same time testers write test scripts
- **Keep documentation current** — distribute updates (known faults, workarounds, new features) via central location or web
- **Ariane-5 lesson**: undocumented assumptions in reused code led to disaster — design decisions must be captured explicitly

---

## Part II — Maintaining the System (Ch 11)

### Types of Systems (Lehman's Classification)

| Type | Description | Change Likelihood |
|---|---|---|
| **S-system** | Formally specified, derivable from a complete specification (e.g., matrix operations) | Low — if world changes, it's a new problem |
| **P-system** | Practical abstraction of the problem; approximate solution (e.g., chess program) | Medium — incremental changes as discrepancies are found |
| **E-system** | Embedded in the real world; changes as the world changes (e.g., economic forecasting) | High — nearly constant change |

> The more a system depends on the real world, the more maintenance it will need.

---

### Types of Maintenance

| Type | Purpose | Effort Share (Lientz & Swanson) |
|---|---|---|
| **Corrective** | Fix faults / failures in day-to-day operation | ~21% |
| **Adaptive** | Secondary changes to accommodate environment / platform / tool updates | ~25% |
| **Perfective** | Improve aspects not driven by faults (readability, test coverage, design clarity) | ~50% |
| **Preventive** | Fix latent faults before they cause failures (type checking, fault handling) | ~4% |

> More than **80%** of total programming effort goes to maintenance (the "80–20 rule").

---

### Lehman's Five Laws of Software Evolution

1. **Continuing Change** — a program that is used undergoes continual change or becomes progressively less useful
2. **Increasing Complexity** — as a system evolves, its structure deteriorates and complexity increases unless actively reduced
3. **Fundamental Law** — program evolution is self-regulating with statistically determinable trends and invariances
4. **Conservation of Organizational Stability** — the global activity rate in a project is statistically invariant
5. **Conservation of Familiarity** — the release content (changes, additions, deletions) of successive releases is statistically invariant

---

### Maintenance Problems

#### Staff Problems
- **Limited understanding** — 47% of maintenance effort is spent understanding the software; a one-line change can require hundreds of tests across interfaces
- **User misunderstanding** — users provide incomplete/misleading data when reporting problems
- **Management priorities** — business urgency overrides engineering elegance
- **Low morale** — maintenance seen as second-class; programmers pulled across multiple projects

#### Technical Problems
- **Inflexible designs** — tape-only I/O, fixed field sizes, Y2K-style shortcuts
- **OO maintenance challenges** — deep inheritance, dynamic binding, delocalized program plans
- **Testing difficulties** — 24/7 systems can't go offline; test data may not exist for new scenarios
- **Concurrent changes** — two maintainers modifying the same component can introduce new faults

#### Maintenance Cost
- **1970s**: development dominated budgets
- **1980s**: ratio reversed
- **2000s**: maintenance ≈ **80%** of lifetime cost
- **Life-cycle cost** = all costs from creation through retirement

#### Factors Affecting Maintenance Effort
- Application type (real-time is harder)
- System novelty (new domains require more learning)
- Staff turnover and availability
- System life span (long-lived systems accumulate complexity)
- Dependence on changing environment (E-systems cost most)
- Hardware reliability and vendor support
- Design, code, and documentation quality
- Test suite quality

#### Belady–Lehman Maintenance Effort Model

$$M = p + K \cdot c^{d}$$

| Symbol | Meaning |
|---|---|
| M | Total maintenance effort |
| p | Productive effort (analysis, design, coding, testing) |
| K | Empirical constant |
| c | System complexity (grows with patches) |
| d | Degree to which the team lacks familiarity with the system |

> As complexity grows and team familiarity drops, effort increases exponentially.

---

### Maintenance Activities & Roles

1. Understanding the system
2. Locating information in documentation
3. Keeping documentation up to date
4. Extending / adding functions
5. Finding failure sources and correcting faults
6. Answering system questions
7. Restructuring / rewriting design and code
8. Deleting obsolete components
9. Managing changes

> Software engineering principles (modularity, cohesion, low coupling, cross-referencing) serve maintenance just as well as development.

---

### System Evolution vs. Decline

Questions to decide whether to **maintain, rebuild, or replace**:

- Is maintenance cost too high?
- Is reliability unacceptable?
- Can the system no longer adapt within reasonable time?
- Is performance beyond constraints?
- Are functions of limited usefulness?
- Can other systems do the job better / faster / cheaper?
- Is hardware maintenance cost justified?

> Compare **life-cycle costs** of old, revised, and new systems to decide.

---

### Software Rejuvenation

When a legacy system is no longer maintainable, **rejuvenation** strategies include:

- **Re-engineering** — restructuring code and design without changing functionality
- **Wrapping** — encapsulating legacy components with modern interfaces
- **Migration** — porting to new platforms/languages incrementally
- **Replacement** — building a new system to supersede the old one

---

## Key Takeaways

1. **Delivery is not a ceremony** — it requires planned training and documentation designed alongside the system
2. **Training must be ongoing** — not just at delivery; support replacement users, infrequent users, and refresher needs
3. **Documentation must be audience-specific** — user, operator, programmer, and management each need different views
4. **Software does not wear out** — it becomes inadequate as the world changes around it
5. **Maintenance is the dominant cost** — plan for it from day one with good design, modularity, and documentation
6. **Lehman's laws predict system behavior** — complexity grows, change is constant, team output stabilizes
7. **The biggest maintenance expense is understanding** — invest in clear, cross-referenced, up-to-date documentation


## Related

- [[Engineering Foundation Overview]] — All engineering foundation topics
- [[02 SWE Process/16_Testing_Strategies|16_Testing_Strategies]] — Testing before delivery
- [[02 SWE Process/18_Evaluation_and_Improvement|18_Evaluation_and_Improvement]] — Maintenance measurement and improvement
- [[02 SWE Process/10_SE_Fundamentals_and_Process|10_SE_Fundamentals_and_Process]] — Life cycle models and maintenance phase
