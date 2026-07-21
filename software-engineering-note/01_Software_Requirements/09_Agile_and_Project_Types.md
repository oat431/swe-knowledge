---
tags:
  - requirements
  - agile
  - project-types
  - embedded
  - software-requirements
---

# Agile & Project Types — Software Requirements Ch 20-26

> **Source:** Wiegers & Beatty, *Software Requirements* 3rd ed., Part III — Chapters 20–26
> **Theme:** How requirements practices adapt across agile, enhancement/replacement, packaged, outsourced, business process automation, business analytics, and embedded/real-time projects.

---

## Ch 20 — Agile Projects

### Adaptive vs. predictive approaches
- **Agile** = a family of iterative/incremental methods (Scrum, XP, Lean, FDD, Kanban) anchored in the 2001 Agile Manifesto.
- **Predictive (plan-driven)** approaches (e.g., waterfall) minimize risk by heavy up-front planning and documentation; work well when requirements are well understood and stable.
- **Adaptive (change-driven)** approaches embrace change; suit projects with uncertain or volatile requirements.
- Key differentiator across the spectrum: **elapsed time between requirement creation and customer-visible delivery**.

### Waterfall limitations
- Linear "fully specify → design → code → test" works in theory; rarely so in practice.
- Royce (1970) actually described the pure waterfall as "risky and invites failure" — he recommended overlapping phases and prototyping.
- Long waterfall projects deliver late, miss features, and fail expectations because stakeholders *will* change requirements mid-project (don't know what they want, learn by seeing, business shifts).

### "Agile requirements" is a misnomer
- The information developers need is **the same** regardless of lifecycle.
- Agile and traditional projects differ in **timing, depth, and documentation level** of requirements activities — not in the requirements themselves.
- Prefer the phrase **"requirements for agile projects."**

### Essential aspects of agile requirements

**Customer involvement**
- Waterfall: customers engaged up front (elicitation) and at UAT; minimal contact during construction.
- Agile: product owner / customers engaged **continuously** — prioritize backlog, clarify during iteration, test completed features.
- Inexpertly written user stories need review by someone with solid BA skills.

**Documentation detail**
- Waterfall demands detailed written specs because developers lack customer access during construction.
- Agile: write the **minimum documentation** needed to accurately guide developers and testers; precision developed through conversation.
- Riskiest / highest-impact functionality specified in more detail (typically as acceptance tests).

**Backlog and prioritization**
- Product backlog = list of work items (user stories, other requirements, defects).
- Maintain **one backlog** per project; rewrite defects as user stories if needed.
- Priorities are **stable only for the next iteration**; trace backlog items to business requirements to aid prioritization.

**Timing**
- Same requirement activities (elicit, analyze, document, validate) — but **not all up front**.
- High-level requirements (user stories) populate backlog early; details clarified per iteration.
- **Nonfunctional requirements must be learned early** so architecture supports performance, usability, availability goals.

### Epics, user stories, features
- **User story**: concise statement of a user need; sized to be fully implementable in one iteration.
- **Epic**: a story too large for one iteration; split into smaller epics, then into stories (**story decomposition**).
- **Feature**: grouping of system capabilities that provides value; could span stories or epics.
- **Minimum Marketable Feature (MMF)**: smallest set of functionality delivering customer value.
- Don't obsess over story/epic/feature labels — focus on **high-quality requirements**.

### Expect change
- Agile BA's reflex to a change request: "Okay, let's talk about it" — not "out of scope."
- Add/change stories, reprioritize against backlog.
- Still manage change thoughtfully; look ahead to design extensible architecture.
- Change also includes **removing** scope items (implementation issues, unacceptable stories, higher-priority replacements).

### Transitioning to agile
- Most existing BA practices still apply.
- Determine your role (dedicated BA or other title performing BA activities).
- Read up on the product owner role (e.g., Pichler 2010).
- Start with a pilot project or a few low-risk practices.
- Consider a hybrid model; bring in an experienced coach for 3–4 iterations.
- **Don't be an agile purist for its own sake.**

---

## Ch 21 — Enhancement & Replacement Projects

### Definitions
- **Enhancement project**: adds capabilities to an existing system; may also fix defects, add reports, modify for new business rules.
- **Replacement (reengineering) project**: replaces an existing app with new custom-built, COTS, or hybrid system.
- Common drivers: improve performance, cut costs, adopt modern tech, meet regulations.

### Expected challenges
- Changes may degrade accustomed performance.
- Little or no requirements documentation for the existing system.
- Users may resist changes.
- Risk of breaking/omitting vital functionality.
- Stakeholders may gold-plate with unneeded new functionality.

### Techniques when an existing system exists

| Technique | Why it's relevant |
|---|---|
| Feature tree | Show features added; identify existing features being dropped |
| User classes | Assess who is affected; identify new user classes |
| Business processes | Understand current system intertwining with jobs; define new processes |
| Business rules | Record rules embedded in code; redesign for volatile rules |
| Use cases / user stories | Understand required capabilities; prioritize |
| Context diagram | Document external entities; extend/change interfaces |
| Ecosystem map | Find affected systems; new/modified/obsolete interfaces |
| Dialog map | Fit new screens into existing UI; show navigation changes |
| Data models | Verify existing model sufficiency; consider migration/conversion |
| Quality attributes | Ensure new system meets quality expectations |
| Report tables | Convert existing reports; define new ones |
| Prototypes | Engage users; resolve uncertainties |
| Inspect SRS | Find broken trace links; identify obsolete requirements |

### Prioritizing with business objectives
- Enhancement projects risk gold-plating — trace requirements to business objectives.
- Prioritize enhancement requests against defect correction.
- Replacement projects: focus on migrating functionality; beware uncontrolled scope growth.
- **Don't accept "I have it today, so I need it tomorrow" as justification** — validate against current business objectives.

### Gap analysis
- Comparison of existing vs. desired system functionality.
- For replacements: identify existing requirements to re-implement, new requirements, never-implemented change requests.
- Prioritize closing gaps using business objectives.

### Maintaining performance levels
- Existing systems set user expectations; stakeholders have KPIs they want maintained.
- **Key Performance Indicator Model (KPIM)**: identifies and specifies metrics for business processes.
- Prioritize KPIs to maintain; implement requirements tracing to most important KPIs first.

### When old requirements don't exist
- Reverse-engineer understanding from UI, code, database — **"software archaeology."**
- Record findings as requirements and design descriptions.
- Incremental cost of recording new knowledge is small compared to rediscovery cost.
- **Leave requirements in better shape than you found them.**

### Which requirements to specify?
- Rarely worth documenting the entire existing system.
- Focus on changes needed for business objectives.
- For replacements: start with highest-priority or highest-risk areas.
- Level of detail: for enhancements, may suffice to define new functionality; usually beneficial to document related functionality too.

### Trace data
- Trace helps enhancement developers identify affected components.
- For replacements: create traceability matrix linking new/changed requirements to design, code, tests.
- Perform high-level tracing: list features/stories, prioritize for new system.

### Discovering existing system requirements
- Study existing UI, system interfaces, user workflows.
- Look at code/database if no one understands functionality.
- Analyze existing docs: design docs, help screens, user manuals, training materials.
- Use swimlane diagrams, context diagrams, DFDs, ERDs.
- Test suite can be an alternative view of requirements.

### Encouraging adoption
- Anticipate resistance; users fear disruption, job changes.
- Understand business objectives and user requirements — missing the mark loses trust.
- Focus on benefits to users during elicitation.
- Show prototypes early.
- **Transition requirements**: data conversions, training, process changes, parallel running.

### Can we iterate?
- Enhancement projects are incremental by definition → agile fits well.
- Replacement projects: need critical mass before users can switch; big-bang is risky.
- Strategy: isolate functionality that can stand alone (e.g., inventory management split from fulfillment system).

---

## Ch 22 — Packaged Solution Projects

### Overview
- **COTS** (commercial off-the-shelf) / **SaaS** solutions: acquire and adapt rather than build.
- Still need requirements to evaluate candidates and adapt the package.
- COTS packages typically need **configuration, integration, and/or extension**.

### Requirements for selection
- COTS offers less flexibility than custom (bespoke) development.
- Must know which capabilities are negotiable vs. non-negotiable.
- Level of detail depends on package cost, evaluation timeline, number of candidates.
- Focus at **user requirements level** — use cases, user stories, process models.
- Little point in detailed functional requirements or UI design (vendor did that).

### Developing user requirements
- Identify desired features from user needs and business processes.
- Prioritize; trace to business requirements.
- Distinguish day-one must-haves from future extensions and nice-to-haves.

### Business rules
- Identify rules the COTS must conform to (policies, standards, regulations).
- Evaluate configurability and modifiability when rules change.
- Watch for intrinsic rules that don't apply — can you disable them?

### Data needs
- Define data structures, especially for integration scenarios.
- Look for disconnects between your data model and vendor's.
- Specify required reports and customization extent.

### Quality requirements
- **Performance**: response times, concurrent user load, throughput.
- **Usability**: UI conventions, learnability, training availability.
- **Modifiability**: extension hooks, APIs, upgrade survival of extensions.
- **Interoperability**: integration ease, standard data formats, backward compatibility.
- **Integrity**: data protection from loss/corruption/unauthorized access.
- **Security**: access control, user privilege levels, SLA evaluation (especially SaaS).

### Evaluation approach (Lawlis et al. 2001)
1. Weight requirements 1–10 by importance.
2. Rate each candidate: 1 (full), 0.5 (partial), 0 (none).
3. Calculate weighted scores.
4. Evaluate cost, vendor viability, support, interfaces, compliance — initially without cost.
- Alternative: derive tests from high-priority use cases; run operational profile scenarios.
- **Trap**: ensure at least one person spans all evaluations for consistent interpretation.
- Output: evaluation matrix (requirements × solutions).

### Multi-stage evaluation example
- 200 features, 60 vendors → first pass with 30 key features → 16 tools → full 200-feature evaluation → 5 tools → real-project trial.

### Implementation spectrum

| Type | Description |
|---|---|
| Out-of-the-box | Install and use as-is |
| Configured | Adjust settings without new code |
| Integrated | Connect to existing systems; usually requires custom code |
| Extended | Custom code to add functionality and close gaps |

### Configuration requirements
- Define per process flow / use case / user story.
- Walk through user manuals; identify settings.
- Consider full set of business rules (not just selection-time).
- Use decision tables/trees; roles and permissions matrix.

### Integration requirements
- Specify external interfaces; precisely define data/service interchange.
- Custom code forms: **adapters**, **firewalls**, **wrappers**.

### Extension requirements
- Minimize customizations — otherwise just custom build.
- For each gap: ignore it, change the business process, or build a bridge.
- Fully specify extension requirements as for new product development.

### Data requirements
- Map existing data dictionary to COTS entities/attributes.
- Handle data gaps (add attributes, repurpose structures).
- Use report tables; leverage vendor-supplied templates.

### Business process changes
- COTS selected for cost efficiency; be prepared to **adapt processes to the package**.
- Fully configurable COTS is expensive and complex.
- Balance desired functionality against out-of-the-box capability.
- Model as-is → study manuals → create to-be processes; involve users early.

### Common challenges
- Too many candidates → short-list with key criteria.
- Too many evaluation criteria → use business objectives to focus.
- Vendor misrepresents capabilities → involve technical specialist during sales cycle.
- Incorrect solution expectations → have vendor walk through your actual use cases.
- Users reject solution → engage users in selection/implementation; manage expectations.

---

## Ch 23 — Outsourced Projects

### Context
- Outsourcing: contract development to another company (onshore, nearshore, or offshore).
- All outsourced projects involve **distributed teams**.
- BA role is more important and harder than on co-located projects.

### Requirements-related challenges
- Harder to get developer input on requirements and pass user feedback.
- Formal contractual definition of requirements necessary → contention if interpretation differs late.
- Bigger gap between customer need and delivered product (fewer course-correction opportunities).
- Slower issue resolution across time zones.
- Language and cultural barriers.
- Limited written requirements insufficient — users/BAs not available to clarify.
- Remote developers lack organizational/business knowledge.
- Net cost often **increases** (precise requirements effort, extra iterations, contractual overhead, communication, oversight).

### Appropriate levels of requirements detail
- Demands **high-quality written requirements** — minimal day-to-day clarifications.
- Send supplier: RFP, requirements specification, acceptance criteria.
- Supplier builds **exactly what you ask** — no more, no less; implicit/assumed requirements won't be implemented.
- Develop more detailed requirements **earlier** than for in-house projects.
- Err on the side of **overspecifying**.
- Visual models augment written specs; prototypes clarify expectations.
- Avoid ambiguous terms (e.g., "support"); provide a glossary; consider Planguage for explicit specification.

### Acquirer-supplier interactions
- Arrange formal touch points; plan multiple review cycles.
- Allow sufficient time for documentation, review, revision.
- Cultural note: some cultures avoid constructive criticism in peer reviews.
- Supplier may help write functional requirements (increases initial cost, reduces misunderstanding risk).
- Incremental development permits course corrections.
- Document supplier questions; integrate answers into requirements; track in shared issue tool.
- Provide domain/company training to contractor staff.
- Watch for cultural behaviors: eagerness to please → overcommitment; reluctance to say "no" or "I don't understand."
- Use simple language; avoid colloquialisms, jargon, idioms, pop-culture references.
- **Trap**: acquirer must communicate all necessary info; supplier must proactively ask clarifying questions.

### Change management
- Establish mutually acceptable change control process up front.
- Use common web-based tools for change requests and issue tracking.
- Contract should specify who pays for various change types and the incorporation process.

### Acceptance criteria
- Define in advance how to assess product acceptability.
- Include acceptance criteria in the RFP.
- Validate requirements before handing to outsourced team.
- If requirements are incomplete or misunderstood, failure is at least as much the acquirer's fault as the supplier's.

---

## Ch 24 — Business Process Automation Projects

### Overview
- Replace manual processes with software to lower operational costs.
- Most corporate IT projects involve some business process automation.
- Solution may be new build, existing system extension, or COTS.

### Business process acronyms

| Term | Meaning |
|---|---|
| **BPA** | Business Process Analysis — understand processes to improve them |
| **BPR** | Business Process Reengineering — redesign for greater efficiency/effectiveness |
| **BPI** | Business Process Improvement — incremental improvement (Six Sigma, lean) |
| **BPM** | Business Process Management — enterprise-wide process analysis and change |
| **BPMN** | Business Process Model and Notation — graphical modeling language |

### Using current processes to derive requirements
1. Understand business objectives; link each to processes.
2. Use org charts to find affected organizations and user classes.
3. Identify relevant business processes.
4. Document **as-is** processes (flowcharts, activity diagrams, swimlane diagrams).
5. Analyze as-is to find biggest improvement opportunities; use KPIM to gather data.
6. Walk through each as-is flow with stakeholders to elicit software requirements per step.
7. Trace requirements to process steps; confirm unautomated steps.
8. Document **to-be** process flows; create use cases; develop training materials; identify transition requirements.

### When software isn't the solution
- Sometimes a **process fix** (e.g., assigning a contact owner) beats automation.
- Analyze where time is actually spent before assuming software is the answer.

### Designing future processes first
- Chicken-and-egg: new system vs. new process.
- Better to devise new business processes **first**, then assess system architecture changes.
- Concurrent development of new processes and applications ensures they merge well.

### Modeling business performance metrics (KPIM)
- **KPIM** = flowchart/swimlane/activity diagram with KPIs overlaid on related steps.
- Prioritize automating processes with the most important metrics to maintain/improve.
- Establish baseline values; build in KPI measurement to verify improvement.
- Trade-offs: may degrade some metrics to improve others.
- Trace requirements → process steps → KPIs for prioritization.

### Good practices road map

| Technique | Chapter |
|---|---|
| Identify user classes with automatable processes | Ch 6 |
| Create/extend data models for manual information | Ch 13 |
| Roles and permissions matrix for security | Ch 9 |
| Identify business rules to automate | Ch 9 |
| Flowcharts/swimlanes/activity diagrams/use cases (as-is and to-be) | Ch 8, 12 |
| Data flow diagrams to identify automatable processes | Ch 12 |
| Adapt processes to COTS | Ch 22 |
| Trace matrices mapping process steps to requirements | Ch 29 |

---

## Ch 25 — Business Analytics Projects

### Overview
- **Business analytics** (a.k.a. business intelligence / reporting): turn large, complex data sets into meaningful information for decisions.
- Decisions can be **strategic, operational, or tactical**.
- Complex reports and data manipulation = core functionality (unlike most systems where reports are minor).
- Analytics spectrum: **descriptive** (what happened/is happening) → **predictive** (what might happen).

### Analytics framework layers
1. **Data** — what data is required.
2. **Analysis** — operations performed on the data.
3. **Formatting & distribution** — delivery for use.
- No rigid sequence; users iterate between layers.

### Requirements development
- Still produces business, user, functional, and nonfunctional requirements.
- Standard practices (process flows, use cases, user stories) reveal *that* someone needs analytics, but not the complex implementation knowledge.
- Pilot small projects if new to analytics; incremental development suits these projects.
- Stakeholders may struggle to articulate/prioritize problems; begin with education on analytics capabilities.

### Elicitation via decisions
- Drive requirements from **decisions stakeholders need to make**:
  1. Describe business decisions using system outputs.
  2. Link decisions to business objectives.
  3. Decompose decisions into questions and precursor questions.
  4. Determine how analytics can assist.
- Prioritize decisions by contribution to objectives (e.g., "which products to sell" > "vacation time decisions").
- State decisions unambiguously, like requirements.
- Ask "Why do you need that?" and "How will you use that report?" to surface underlying decisions.

### Defining information usage
- Determine how "smart" the system is: human decision vs. automated action.
- **Delivery mechanism**: email, portal, mobile, etc.
- **Format**: reports, dashboards, raw data.
- **Flexibility**: extent of user manipulation.
- Spectrum: personal local view → distributed central aggregation → ad-hoc query portal.
- Capture as user requirements and report specifications; use report tables, dashboard specs, display-action-response models.
- May reveal new processes and security requirements (e.g., regional access controls).

### Information usage in systems
- Analytics may be embedded in applications (e.g., personalized discounts, targeted ads, call center offers).
- Specify via external interface requirements; still understand usage to ensure correct data transformation.

### Specifying data needs
- Data forms the core; engage data specialists early.
- Explore data types, total quantity, growth over time.
- May need to identify **new data sources**.
- **Big data**: large volume, high velocity, high complexity.
  - Often semi-structured or unstructured; traditional ERDs/data dictionaries may not suffice.
  - Leverage metadata for semi-structured data.
- Data-based requirements questions cover: **sources, storage, management/governance, extraction**.
  - Sources: objects/attributes needed, availability, external/internal systems, change likelihood, historical migration.
  - Storage: current volume, growth, types, retention, security.
  - Management: structural characteristics, transformations, standardization, archival/destruction, integrity.
  - Extraction: query speed, real-time vs. batched, frequency.

### Defining analyses
- Analysis transforms data into answers.
- Challenge: decision maker may not know what he's looking for → enable exploration with what-if queries.
- Elicitation questions:
  - **Past**: what insights are you looking for?
  - **Present**: what do you need to understand for immediate action?
  - **Future**: what predictions or decisions do you want to make?
- Some analyses require sophisticated algorithms (e.g., facial recognition + logic for targeted ads) — use decision tables/trees.
- Be explicit about automated decision logic (cautionary tale: 2013 fake news tweet triggered automated stock selling).
- Specify calculation formulas (Withall 2007 pattern): description, formula, variables, value sources, response time.
- Enlist data experts, statisticians, mathematical modelers for complex analyses.

### Evolutionary nature
- Analytics is iterative: user gets a report → thinks of new questions → requests new analyses.
- **Start somewhere**; plan for questions to evolve.
- Understand how much users expect needs to evolve — drives adaptability requirements.
- Determine: raw data for manual manipulation vs. structured app-delivered reports; fixed recurring questions vs. daily new questions.

---

## Ch 26 — Embedded & Other Real-Time Systems Projects

### Overview
- **Embedded systems**: software controls hardware devices (cell phones, routers, kiosks, robot cars).
- **System** here = product with integrated software + hardware subsystems.
- Software may be embedded in device or reside in a host computer.
- Components: sensors, controllers, motors, power supplies, ICs, mechanical/electrical parts.

### Hard vs. soft real-time
- **Hard real-time**: rigid deadlines; missing them causes bad things (life/safety-critical, e.g., air traffic control).
- **Soft real-time**: time constraints exist but consequences of missing are less severe (e.g., ATM retry).

### Why requirements matter more here
- Software is more malleable than hardware — requirements churn dictating hardware changes is expensive.
- Must know constraints both hardware and software engineers must respect: physical sizes, electrical components, protocols, operation sequences.
- Already-selected hardware components impose constraints on remaining choices.

### System requirements, architecture, and allocation
- **System Requirements Specification (SyRS)**: describes system-as-a-whole capabilities (hardware, software, human), all I/O, critical performance/safety/quality requirements.
- SyRS may be separate from SRS or contain it.
- **Architecture** = top-level design: components + externally visible properties + connections (interfaces).
- Developed top-down, iteratively; lead by system analyst/engineer/architect.
- System requirements decomposed into **derived software, hardware, and manual requirements**, allocated to components.
- Derivation can expand requirements several-fold (generates interface requirements).
- Establish trace links between system requirements, derived requirements, and architectural components.

### Poor allocation consequences
- Software doing what hardware would do cheaper (or reverse).
- Person doing what hardware/software would do cheaper (or reverse).
- Inadequate performance.
- Inability to upgrade/replace components.

### Modeling real-time systems
- **State-transition diagrams (STDs)** / statecharts / UML state machines — particularly relevant.
- **State tables** and **decision tables** supplement/reveal diagram errors.
- **Context diagram** — shows environment and system boundaries.
- **Architecture diagram** — partitions system into subsystems with interfaces.

#### Treadmill example models
- **Context diagram**: system + external entities (Exerciser, manufacturer website, pulse sensor) + I/O flows.
- **STD**: states (Idle, Running, Paused) + transitions triggered by events (button presses, safety key removal, timer timeout).
- **Event-response table**: detailed event/state/response triples (e.g., "Exerciser presses Incline Up / Below max → increase incline 0.5°").
  - Covers event-based functions and **periodic functions** (e.g., pulse monitoring every second).
- **Architecture diagram**: major subsystems + data/control interfaces.

### Prototyping
- Prototyping and simulation are powerful — hardware is costly to build/rebuild.
- Test operational concepts; explore requirements and design options.
- Simulations help understand UI, network interactions, hardware-software interfaces.
- Simulation differs from real product in many respects.

### Interfaces
- Four SRS interface classes: **user, software, hardware, communications**.
- Partitioning creates numerous **internal interfaces**.
- Embedded systems can nest within larger products → complex interface issues.
- Concentrate requirements analysis on **external interfaces**; leave internal specs for architecture design.
- Complex projects may create a separate **interface specification** document.

### Timing requirements
- Heart of real-time control systems.
- Dimensions:
  - **Execution time**: elapsed time from task initiation to completion.
  - **Latency**: time lag between trigger event and system response start.
  - **Predictability**: consistent recurring timing (e.g., 44,100 Hz audio sampling).
- Issues to explore: periodicity/frequency, deadlines/tolerances, typical/worst-case execution times, consequences of missed deadlines, data arrival rates, timeouts, task sequencing, prioritization/preemption, mode-dependent functions.
- Distinguish soft vs. hard real-time demands — don't over-specify → over-engineering at excessive cost.
- "Real-time performance is seldom about being as fast as absolutely possible. Rather, it is about being just as fast as you need to be, and minimizing overall cost." — Koopman 2010
- One team used a project-scheduling tool at millisecond scale to model timing requirements.

### Quality attributes for embedded systems
- More complex and intertwined than for business software.
- Operating environment may involve temperature extremes, vibration, shock.
- Critical categories: **performance, efficiency, reliability, robustness, safety, security, usability**.
- Physical-system quality attributes: size, shape, weight, materials, flammability, connectors, durability, cost, noise, strength.
- Material choice trade-offs: conflict minerals, environmental impact vs. performance/weight/cost.
- Address quality requirements **early** — difficult/expensive to build in after hardware design complete.
- Perform attribute prioritization and trade-off analysis **before** design.

#### Performance
- Beyond response times: startup/reset times, power consumption, battery life, recharge time, heat dissipation.
- Energy management: voltage drops, high current load, power loss → battery backup.
- Component degradation over time (battery replacement cycles).

#### Efficiency
- Internal counterpart to performance: consumption of processor, memory, disk, communication channels, power, network bandwidth.
- Specify maximum anticipated resource consumption; provide slack for growth.
- Concurrent hardware/software design is vital — more capable hardware up front is cheaper than software fine-tuning.

#### Reliability
- Stringent reliability/availability requirements (medical devices, avionics, pacemakers).
- Realistically assess likelihood and impact of failure — don't over-engineer.
- Reliability comes at a price; sometimes worth it, sometimes not.
- Complex systems fail from **unanticipated combinations** of failures (e.g., corrosion on two switches → open train door).

#### Robustness
- How well system responds to unexpected operating conditions.
- **Survivability**: e.g., aircraft "black boxes" designed to survive crash trauma.
- Military applications plus everyday ones.

---

## Cross-Cutting Themes

- **Requirements activities are universal** — every project type needs elicitation, analysis, specification, validation, and management; what changes is timing, depth, documentation level, and who performs them.
- **Existing systems** (enhancement, replacement, COTS implementation) all benefit from gap analysis, traceability, and **leaving requirements in better shape than you found them**.
- **Distributed/contractual projects** (outsourced) demand higher precision and more formal communication — written requirements carry the burden that casual conversation would in co-located teams.
- **Process-centric projects** (BPA, analytics) require understanding the business *before* specifying software; model as-is, design to-be, trace to KPIs/decisions.
- **Embedded/real-time** projects intertwine requirements and architecture; hardware-software allocation decisions, timing constraints, and physical quality attributes dominate.
- **Adoption matters everywhere**: users resist change; involve them early, manage expectations, prototype, and plan transition requirements.

---

## Key Terms

- **Adaptive vs. predictive** — change-driven vs. plan-driven lifecycle.
- **Product backlog** — single prioritized list of work items on an agile project.
- **Epic / User story / Feature / MMF** — progressive decomposition of agile requirements.
- **Gap analysis** — comparison of existing vs. desired system functionality.
- **KPIM** — Key Performance Indicator Model; overlays KPIs on process flow steps.
- **COTS / SaaS** — commercial off-the-shelf / software-as-a-service packaged solutions.
- **SyRS** — System Requirements Specification (system-as-a-whole, including hardware).
- **Hard vs. soft real-time** — rigid vs. flexible timing deadlines.
- **STD** — State-Transition Diagram.
- **Transition requirements** — capabilities needed to move from old system to new (data conversion, training, parallel running).

---

## References (from source)

- Beck et al. 2001 — Agile Manifesto
- Boehm & Turner 2004 — adaptive vs. predictive
- Cohn 2004, 2010 — acceptance tests, epics, backlog
- Denne & Cleland-Huang 2003 — MMF
- Lawlis et al. 2001 — COTS evaluation approach
- Koopman 2010 — real-time / embedded nonfunctional requirements
- Brijs 2013 — business analytics business analysis
- Taylor 2012, 2013 — decision management
- Franks 2012 — big data, analytics data
- ISO/IEC/IEEE 2011 — SyRS standard
- Royce 1970 — waterfall model (actually "risky and invites failure")
