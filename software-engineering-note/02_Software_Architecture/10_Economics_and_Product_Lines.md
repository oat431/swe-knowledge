---
tags:
  - architecture
  - economics
  - cbam
  - product-lines
  - competence
  - software-architecture
source: "Bass, Clements & Kazman, Software Architecture in Practice (SAiP), Chapters 23–25"
created: 2026-07-21
---

# Economics & Product Lines

## 1. Economic Analysis of Architectures — CBAM (Ch 23)

### 1.1 Utility-Response Curves

Stakeholders express needs using concrete **response measures** (e.g., "99.999% available"). The **utility-response curve** shows how utility varies as the quality attribute response level varies — enabling tradeoffs involving cost and competing quality attributes.

**Curve shapes** (Figure 23.2):
- **(a)** Modest change in response → small change in utility (diminishing returns)
- **(b)** Small improvement in response → significant increase in utility
- **(c)** Steep rise in utility over a narrow change in response level
- **(d, e)** Step-function or nonlinear relationships

**Key insight**: Spending more money past the point where the curve flattens yields no significant utility increase (diminishing VFC).

### 1.2 Foundations of Economic Analysis

- **Weighting scenarios**: Different scenarios have different importance to stakeholders. Priorities are elicited via voting schemes (public, secret, or consensus-based). For N scenarios, weights can be assigned as: top = 1.0, next = (N−1)/N, etc.
- **Side effects**: Every architectural strategy affects not only its target QA but others as well (often negatively). Benefits must be summed across all affected quality attributes.
- **Benefit calculation**:

  $$\displaystyle B_i = \sum_{j} (b_{i,j} \times W_j)$$

  where $b_{i,j} = U_{expected} - U_{current}$ (utility change for scenario $j$ under strategy $i$)

- **Value for Cost (VFC)**:

  $$\displaystyle VFC_i = B_i / C_i$$

  Strategies are rank-ordered by VFC to select the best ROI.

### 1.3 CBAM — Cost Benefit Analysis Method

CBAM is a stakeholder-based method for choosing among competing architectural strategies using economic criteria. It assumes a pre-existing collection of quality attribute scenarios (e.g., from an ATAM exercise).

**Nine steps of CBAM**:

| Step | Description | Scenarios |
|------|-------------|-----------|
| 1 | **Collate scenarios** — Stakeholders contribute new scenarios; prioritize using "high/medium/low"; keep top one-third | N → N/3 |
| 2 | **Refine scenarios** — Elicit worst-case, current, desired, and best-case QA response levels for each scenario | N/3 |
| 3 | **Prioritize scenarios** — 100 votes per stakeholder distributed among scenarios; keep top 50%; assign relative weights | N/3 → N/6 |
| 4 | **Assign utility** — Determine utility (0–100) for each response level (worst, current, desired, best) | N/6 |
| 5 | **Map strategies to scenarios** — Determine expected QA response levels each strategy achieves | — |
| 6 | **Determine utility by interpolation** — Using formula $y = y_a + (y_b - y_a)\frac{(x - x_a)}{(x_b - x_a)}$ | — |
| 7 | **Calculate total benefit** — Sum benefit × weight across all scenarios | — |
| 8 | **Choose strategies by VFC** — Rank by benefit/cost ratio; select until budget/schedule exhausted | — |
| 9 | **Confirm with intuition** — Validate alignment with business goals; iterate if needed | — |

**Practical considerations**:
- **Utility curve determination**: Best-case = 100 utility (no further value above this), worst-case = 0 utility (minimum threshold). Current and desired levels are elicited relative to these anchors.
- **Cost estimation**: Absolute numbers aren't necessary — relative comparisons ("strategy B costs 2× strategy A") or coarse categories ("a lot", "not much", "middle") often suffice.
- **CBAM's value**: Though inputs are subjective and imprecise, CBAM provides structure to otherwise unstructured discussions, forces clarity on scenarios and utility, and results in justified choices.

### 1.4 NASA ECS Case Study

The Earth Observing System Data Information System (EOSDIS) Core System (ECS) collected and processed satellite data. With a limited annual budget and a large set of desired changes (only 10–20% fundable), CBAM was used to rank architectural strategies.

**Result**: 10 architectural strategies were developed and ranked by VFC. Strategy 1 (Order Persistence on Submission) had highest VFC (0.79); Strategy 9 (Granule-Level Order Tracking) had lowest (0.22). The ranking roughly validated stakeholders' intuition but provided quantitative justification.

### 1.5 Computing Benefit for Variation Points (McGregor's Formula)

In product-line contexts, the question is: **When will adding a variation point pay off?**

The SIMPLE cost-modeling language provides a formula for marginal value:

$$v_i(t,T) = \max\left(0,\; -E\sum_{\tau=t}^{T} c_i(\tau)e^{-r(\tau-t)} + \rho_{i,T}\, E\!\left[\sum_{k} \max\!\left(0, \sum_{\tau=T}^{T^*} X_{i,k}(\tau)e^{-r(\tau-t)}\right)\right]\right)$$

**Components**:
- **First term (cost)**: Expected cost of building variation point $i$ from now ($t$) to time $T$, adjusted for net present value via interest rate $r$
- **Second term (benefit)**: $X_{i,k}(\tau) = VMP_{i,k}(\tau) - MC_{i,k}(\tau)$ — marginal value of variation point $i$ in product $k$ at time $\tau$, minus marginal cost of tailoring it for that product; summed over all products $k$ and time intervals from $T$ to $T^*$; multiplied by probability $\rho_{i,T}$ that the variation point will be ready by time $T$

This can be implemented in a spreadsheet to quantitatively guide variation point selection.

---

## 2. Architecture Competence (Ch 24)

> *Architecture competence of an organization*: the ability to grow, use, and sustain the skills and knowledge necessary to effectively carry out architecture-centric practices at individual, team, and organizational levels to produce architectures with acceptable cost that lead to systems aligned with business goals.

### 2.1 Individual Competence: Duties, Skills, Knowledge

The triad model: Skills and Knowledge support the execution of Duties.

#### Technical Duties (Table 24.1)

| Duty Area | Specific Duties |
|-----------|----------------|
| **Architecting** | Create/design/select architecture; make design decisions; partition system; define component interactions; identify patterns and tactics |
| **Evaluating & analyzing** | Evaluate architecture satisfaction of QA scenarios; create prototypes; participate in design reviews; perform tradeoff analysis; model alternatives |
| **Documenting** | Prepare architectural documents and presentations; document interfaces; produce documentation standards; document variability |
| **Working with existing systems** | Maintain and evolve architecture; redesign for migration to new technology/platforms |
| **Other architecting duties** | Sell/keep the vision alive; provide architectural guidelines; lead improvement activities; oversee architecture definition process |

#### Nontechnical Duties (Table 24.2)

| Duty Area | Specific Duties |
|-----------|----------------|
| **Project management** | Budgeting, planning, resource management, sizing/estimation, risk assessment, scheduling |
| **People management** | Build trusted advisor relationships; coordinate; motivate; advocate; delegate; supervise |
| **Supporting management** | Advise PM on design/requirements tradeoffs; serve as bridge between technical team and PM |
| **Organization & business** | Grow evaluation capability; hiring; product marketing; translate business strategy into technical vision; align architecture with business goals |
| **Leadership & team building** | Mentor architects; produce technology roadmaps; build and align architecture team; maintain morale |

#### Nontechnical Skills (Table 24.3)

| Skill Area | Specific Skills |
|------------|----------------|
| **Communication** | Oral/written presentations; explain technical info to diverse audiences; persuade; listen; interview; consult; negotiate |
| **Interpersonal** | Team player; work with superiors/colleagues/customers; inspire collaboration; build consensus; handle conflict; be diplomatic |
| **Work skills** | Leadership; decision-making; initiative; independent judgment; work under pressure; prioritize; think strategically; handle abstraction; tolerate ambiguity; be adaptable and resilient |

#### Knowledge Areas (Table 24.4)

| Knowledge Area | Examples |
|---------------|----------|
| **Computer science — architecture** | Frameworks, patterns, tactics, viewpoints, ADLs, quality attributes, evaluation methods |
| **Software engineering** | SDLC, process management, requirements analysis, modeling techniques, component-based development, SPL techniques |
| **Design** | OOAD, UML, design of complex multi-product systems |
| **Programming** | Language models; specialized techniques for security, real-time, etc. |
| **Technologies & platforms** | Hardware/software interfaces, web technologies, RDBMS, cloud platforms, SOA |
| **Domain knowledge** | Domain-specific technologies and practices |
| **Industry & enterprise** | Best practices, standards, business practices, competitive landscape, strategic planning, financial models, budgeting |
| **Leadership & management** | Coaching, mentoring, project management, project engineering |

**Improving individual competence**:
1. Gain experience via apprenticeship (education alone only enhances knowledge)
2. Improve nontechnical skills through professional development courses
3. Master and stay current with the body of knowledge (courses, certification, reading, conferences, professional societies)

### 2.2 Organizational Competence

Organizations can help or hinder architects. Competent organizations perform specific duties:

- Hire talented architects; establish a career track
- Make the architect position highly regarded (visibility, reward, prestige)
- Establish clear responsibilities and authority
- Create mentoring, training, and certification programs
- Establish an architecture review board
- Maintain a repository of reusable architectures and artifacts
- Measure architect performance and architecture quality
- Include architecture milestones in project plans
- Give architects influence throughout the entire project life cycle

#### Assessment Goals

Four reasons to assess organizational competence:

1. **Self-improvement**: Track progress against industry norms and organizational goals
2. **Acquisition**: Evaluate contractors (like CMMI level assessments)
3. **Service organizations**: Advertise competence to attract/retain customers
4. **Product builders**: Advertise product quality; improve internal productivity

#### Competence Assessment Framework (Table 24.5)

Organized into three practice areas:

| Software Engineering | Technical Management | Organizational Management |
|---------------------|---------------------|---------------------------|
| QA elicitation practices | Business/mission goals practices | Hire talented architects |
| Tools & technology selection | Product/system definition | Career track for architects |
| Modeling & prototyping | Allocating resources | Professional development |
| Architecture design practices | Project management | Organizational planning |
| Architecture description | Process discipline | Technology planning/forecasting |
| Architecture evaluation | Collaboration with managers | |
| System implementation | | |
| Software verification | | |
| Architecture reconstruction | | |

**Four underlying models** used to populate the framework with questions:

1. **Duties, Skills, and Knowledge (DSK) model** — how the organization ensures duties are carried out competently
2. **Organizational Coordination model** — how teams at multiple sites coordinate; architecture induces coordination requirements that must match organizational bandwidth
3. **Human Performance Technology model** — value/cost ratio of architecture outputs
4. **Organizational Learning model** — how experience transforms into organizational knowledge (supra-individual)

**Assessment execution**: 3–4 trained assessors interview groups (architects, upstream managers, downstream developers/testers/maintainers). For each practice area, assign green/yellow/red light (doing well / could improve / high risk). Results are a vector (not scalar), mirroring CMMI continuous representation.

---

## 3. Architecture and Software Product Lines (Ch 25)

> A **software product line** is a set of software-intensive systems sharing a common, managed set of features that satisfy the specific needs of a particular market segment or mission and that are developed from a common set of core assets in a prescribed way.

### 3.1 Core Concepts

**Core assets** include:
- Architecture (the central asset)
- Software elements and their designs
- Documentation, user manuals
- Project management artifacts (budgets, schedules)
- Test plans, test cases, test data
- Performance models, schedulability analyses

**Variation points** are places in core assets where they can be quickly tailored in preplanned ways. System building becomes: access assets → exercise variation points → assemble system.

**Industry results**:
| Company | Result |
|---------|--------|
| Nokia | 12+ phone models/year (vs. 3 before) |
| Cummins | Engine software: 1 year → 1 week |
| Hewlett-Packard | 1/4 staff, 1/3 time, 1/25 defects |
| Deutsche Bank | $4M/year savings |
| Philips (TV) | Integration faults reduced; software off the critical path |
| NRO (satellite systems) | 10% expected developers, 1/10 expected defects |
| Philips (medical) | Defects and time-to-market cut by >50% |

**Minimum viable product line**: data shows ~3 products are needed for the investment to pay off.

### 3.2 The Problem with Clone-and-Own

Clone-and-own (copy module, change code, own new version) is expedient but doesn't scale:
- 21 products × 100 modules = potentially 2,100 separately maintained versions
- Systematic portfolio-wide changes become prohibitively expensive (labor grows as square of products)
- Organizations hit a wall of complexity

**Solution**: Introduce **variation mechanisms** so a single version of each asset handles all variations through preplanned adaptation points.

### 3.3 What Makes Product Lines Work

The commonalities shared by products are exploited through **strategic (planned) reuse**, not opportunistic reuse:

| Reuse Area | What Gets Reused |
|-----------|-----------------|
| Requirements | Common requirement set; product-specific "delta" documents |
| Architectural design | Large investment amortized across all products; QA decisions validated once |
| Software elements | Designs, interfaces, documentation, test plans |
| Modeling & analysis | Performance models, schedulability, deadlock proofs, fault tolerance schemes |
| Testing | Test plans, cases, data, harnesses, communication paths |
| Project planning | Budgets, schedules, WBS, team composition |
| People | Transferable expertise across products |
| Exemplar systems | Deployed products as high-quality demonstration prototypes |
| Defect elimination | Each new product inherits defect fixes from forebears |

**Why traditional reuse libraries failed**: elements were too small/large, pedigree unclear, QA mismatch, different architectural models, wrong interaction protocols. Product lines fix this via **strict context** — nothing enters the core asset base that wasn't built for reuse in that product line.

### 3.4 Product Line Scope

Scope defines what systems are "in" and what are "out" — like a doughnut in the space of all possible systems:
- **Center (in scope)**: Systems easily built from core assets
- **Doughnut edge (borderline)**: Possible with effort; case-by-case disposition
- **Outside (out of scope)**: Core assets not equipped to handle

**Scoping risks**:
- **Too narrow**: Insufficient products to justify investment
- **Too broad**: Effort to derive individual products exceeds savings

Input comes from strategic planners, marketing, domain analysts, and technology experts. Market segmentation and customer interaction models also influence scope (e.g., Philips has separate product lines for mass-market home video vs. professional digital video).

### 3.5 Variability as a Quality Attribute

Variability is a special form of modifiability — the ability of a core asset to adapt to different product contexts within scope.

**General Variability Scenario** (Table 25.1):

| Portion | Possible Values |
|---------|----------------|
| Source | Actor requesting variability |
| Stimulus | Requests to support variations in hardware, feature sets, technologies, UI, QAs |
| Environment | Runtime, build time, or development time |
| Artifact | Requirements, architecture, component, test suite, project plan |
| Response | Requested variants can be created |
| Response measure | Specified cost/time to create core assets and variants |

### 3.6 Role of Product Line Architecture

**Tactical role**: The architecture discriminates between what's constant and what varies across family members. All architectures are abstractions that admit multiple instances — product line architectures make this explicit with built-in variation points.

**Strategic role**: The core asset base (crowned by the architecture) serves as a springboard into new markets. Example: Cummins used its automotive diesel engine product line to rapidly dominate the neighboring industrial diesel engine market.

**Three unique product line architect concerns**:
1. Identifying variation points (from scope definition and requirements)
2. Supporting variation points (via variation mechanisms)
3. Evaluating the architecture for product line suitability

### 3.7 Variation Mechanisms

**Primary architectural variation mechanisms**:
1. **Inclusion/omission of elements** — via build procedures or conditional compilation
2. **Different number of replicated elements** — e.g., add more servers for high-capacity variants
3. **Selection of different element versions** — same interface, different behavior/QA characteristics; via static libraries, DLLs, or plug-ins at compile/build/runtime

**Element-level variation mechanisms**:

| Mechanism | Core Asset Cost | Who Exercises It | Exercise Cost |
|-----------|----------------|------------------|---------------|
| Inheritance / specialization | Medium | Product developers | Medium |
| Component substitution | Medium | Developer, sysadmin | Low |
| Add-ons / plug-ins | High | End user | Low |
| Templates | Medium | Developer, sysadmin | Medium |
| Parameters / preprocessors | Medium | Developer, sysadmin, end user | Low |
| Generator | High | Sysadmin, end user | Low |
| Aspects | Medium | Product developers | Medium |
| Runtime conditionals | Medium | None (automatic) | No dev cost; some perf cost |
| Configurator | Medium | Product developers | Low–medium |

**Choosing variation mechanisms affects**:
- Required skill sets
- One-time tool-building/acquisition costs
- Recurring exercise costs
- Target user group and product quality (performance, memory, maintainability)

**Documentation**: The **variability guide** (part of architecture documentation) describes each mechanism, how/when to exercise it, allowed variations, the instantiation process, and valid/invalid variation combinations.

### 3.8 Evaluating a Product Line Architecture

**What/how to evaluate**:
- Variation points: appropriate, sufficient flexibility, quick product building, acceptable runtime costs
- Scenario-based: elicit scenarios about instantiating the architecture for different products
- Different products may have different QA requirements — evaluate all required combinations
- For unknown hardware: establish performance bounds given hardware assumptions; identify potential contention

**When to evaluate**:
- On each instance/variation used to build products
- If product requirements match the product line envelope → abbreviated evaluation (many issues already resolved)
- Evaluation artifacts (scenarios, checklists) have reuse potential
- When a new product falls outside scope → reevaluate to determine if scope can be expanded or architecture modified
- Evaluations can also inform economic decisions (CBAM integration)

### 3.9 Key Product Line Issues

#### Adoption Strategies

| Dimension | Options |
|-----------|---------|
| **Direction** | Top-down (management mandate) vs. bottom-up (grassroots realization of duplication) |
| **Model** | Proactive (comprehensive scope definition up-front, strategic direction) vs. reactive (build next product from earlier ones, extend architecture incrementally) |
| **Pace** | Big bang (populate all core assets at once) vs. incremental (populate as resources permit) |

Both top-down and bottom-up benefit from a strong champion with authority. Proactive requires heavier initial investment but less rework; reactive relies on rework with little initial investment.

#### Evolution and Management

**External evolution drivers**:
- New vendor versions of existing elements
- New externally created elements (technology shifts)
- New features to stay competitive

**Internal evolution drivers**:
- New functions: in scope (build from assets) vs. out of scope (spin-off or expand asset base)
- Keeping old products compatible with latest asset base (time investment now vs. upgrade difficulty later)

**Organizational structures**:
1. **Shared responsibility**: No separate core asset group; product teams divide core asset maintenance among themselves. Works for small orgs; risks communication overhead and team-biased assets.
2. **Separate core asset unit** (domain engineering): Dedicated team for core asset development/maintenance. Product teams treat them like an external supplier. Better for larger organizations.

#### Configuration Management

More complex than single-system CM because each product is the result of binding many variations. Must be able to reproduce any version of any product — including all core asset versions, tailorings, and special-purpose additions.

---

## Summary

- **CBAM** provides a structured economic method for choosing architectural strategies using utility-response curves, scenario weighting, and VFC ranking. Though inputs are subjective, the process itself brings clarity and justification to architectural decisions.
- **Architecture competence** spans individual (duties, skills, knowledge) and organizational dimensions. A competent organization provides career paths, review boards, training, and knowledge management — assessed via a multi-model framework (DSK, Coordination, Human Performance, Organizational Learning).
- **Software product lines** achieve order-of-magnitude improvements in cost, quality, and time-to-market through strategic reuse of core assets with built-in variation points. Success depends on proper scoping, choosing appropriate variation mechanisms, evaluating architectures for product line suitability, and managing organizational evolution.


## Related

- [[Software Architecture Overview]] — All architecture topics
- [[09_Evaluation_and_Governance]] — Architecture evaluation
- [[06_Tactics_and_Patterns]] — Architecture patterns
