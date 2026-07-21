---
tags:
  - requirements
  - quality-attributes
  - nonfunctional
  - prototyping
  - software-requirements
---

# Quality Attributes & Prototyping

> **Source:** Karl Wiegers, *Software Requirements* — Chapters 14 (Beyond Functionality) & 15 (Risk Reduction Through Prototyping)
> **Purpose:** Identify, specify, and validate nonfunctional requirements (quality attributes + constraints) and use prototyping to reduce requirements uncertainty and expectation gaps.

## What Is This?

There is more to software success than delivering the right functionality. Users hold unstated expectations about **how well** the product works — how easy, fast, reliable, secure, and robust it is. These characteristics, called **quality attributes** (a.k.a. quality factors, the "–ilities"), constitute a major portion of the system's **nonfunctional requirements**. Quality attributes distinguish a product that merely works from one that delights its users; they also drive significant architectural decisions that are far costlier to retrofit than to design for up front.

Nonfunctional requirements encompass three classes:
1. **Quality attributes** — how well the system performs its functions (this chapter's focus)
2. **Constraints** — restrictions on design/implementation choices
3. **External interface requirements** — covered separately (Ch 10)

When users cannot articulate needs from text or models alone — the dreaded "IKIWISI" (*I'll Know It When I See It*) — **prototyping** offers a tangible, low-risk step into the solution space. Prototypes clarify, complete, and validate requirements; explore design alternatives; and can even grow into the delivered product. Used deliberately, prototyping closes the expectation gap between user vision and developer understanding.

> **Key insight:** The categorization (functional vs. nonfunctional) matters less than *identifying* the requirement. Don't debate taxonomy — make sure nothing important is missed.

## Knowledge Areas

### Quality Attributes — External vs. Internal

Quality attributes are classified by whether they are discernible during software execution.

**External quality attributes** — observed during execution; primarily important to users.

| Attribute | Description |
|-----------|-------------|
| **Availability** | Extent to which services are available when/where needed. `Availability = MTBF / (MTBF + MTTR)` |
| **Installability** | How easy to correctly install, uninstall, reinstall the application |
| **Integrity** | Protection against data inaccuracy, loss, and corruption; no tolerance for error |
| **Interoperability** | How readily the system exchanges data/services with other systems or hardware |
| **Performance** | How quickly and predictably the system responds to inputs/events |
| **Reliability** | How long the system runs before failure (probability of failure-free execution) |
| **Robustness** | How well the system responds to unexpected operating conditions (fault tolerance, survivability, recoverability) |
| **Safety** | How well the system prevents injury to people or damage to property |
| **Security** | How well the system blocks unauthorized access to functions/data; protection from malware |
| **Usability** | How easy to learn, remember, and use the system; encompasses ease of learning, memorability, error handling, efficiency, accessibility, ergonomics |

**Internal quality attributes** — not directly observable during execution; significant to dev/maintenance staff; indirectly affect customer satisfaction.

| Attribute | Description |
|-----------|-------------|
| **Efficiency** | How well the system uses processor, disk, memory, bandwidth |
| **Modifiability** | How easily designs/code can be understood, changed, extended (covers corrective, perfective, adaptive maintenance + supportability) |
| **Portability** | Effort to migrate software between operating environments (incl. internationalization/localization) |
| **Reusability** | Effort to convert a component for use in other applications |
| **Scalability** | Ability to grow (users, data, servers, transactions) without compromising performance/correctness |
| **Verifiability** | How readily developers/testers can confirm correct implementation (a.k.a. testability) |

> **Project-type emphasis:**
> - **Embedded systems:** performance, efficiency, reliability, robustness, safety, security, usability
> - **Internet & corporate apps:** availability, integrity, interoperability, performance, scalability, security, usability
> - **Desktop & mobile:** performance, security, usability

### Exploring Quality Attributes — 5-Step Approach (Brosseau)

1. **Start with a broad taxonomy** — use a rich set (e.g., Table 14-1) to reduce the chance of overlooking an important dimension.
2. **Reduce the list** — cross-section of stakeholders assesses which attributes are in scope; record rationale for in/out decisions. If you don't specify quality goals, don't be surprised when they're missing.
3. **Prioritize the attributes** — pairwise ranking (e.g., spreadsheet: `<` = row more important, `^` = column more important). Reveals priorities for elicitation focus and guides resolution when quality requirements conflict.
4. **Elicit specific expectations** — ask guiding questions (users can't answer "What are your reliability requirements?"). Consider specifying *unacceptable* characteristics (properties that would violate expectations) — particularly valuable for safety-critical systems. Decompose stakeholder quality goals into functional + nonfunctional subgoals.
5. **Specify well-structured quality requirements** — apply **SMART** (Specific, Measurable, Attainable, Relevant, Time-sensitive). Include **fit criteria** (Robertson & Robertson) — a quantification demonstrating the standard the product must reach. Look for existing **requirement patterns** (Withall) for performance, availability, scalability, security, etc.

> **Trap:** Don't neglect maintenance programmers and technical support staff as stakeholders — their quality priorities can differ sharply from end users. Quality priorities also vary across user classes; conflicts surfaced early are cheap to resolve.

### Specifying Quality Requirements with Planguage (Gilb)

**Planguage** is a keyword-rich language for precise quality-attribute specification. Addresses the problem that vague statements like "the system shall be user-friendly" are unmeasurable and provide no developer guidance.

**Key Planguage keywords:**

| Keyword | Meaning |
|---------|---------|
| `TAG` | Unique hierarchical label for the requirement |
| `AMBITION` | Purpose/objective leading to the requirement |
| `SCALE` | Units of measurement |
| `METER` | How measurements are made |
| `GOAL` | Minimum acceptable achievement level (must be fully satisfied) |
| `FAIL` | Condition that constitutes failure (alternative to GOAL) |
| `STRETCH` | More desirable performance objective |
| `WISH` | Ideal outcome |
| `DEFINED` | Clarifies specialized terms (e.g., "base user platform") |

**Example (performance):**
```
TAG: Performance.Report.ResponseTime
AMBITION: Fast response time to generate accounting reports on the base user platform.
SCALE: Seconds between pressing Enter/clicking OK and beginning of report display.
METER: Stopwatch testing on 30 test reports representing a defined usage profile.
GOAL: No more than 8 seconds for 95% of reports. ←Field Office Manager
STRETCH: No more than 2 seconds for predefined reports, 5 seconds for all reports.
WISH: No more than 1.5 seconds for all reports.
base user platform DEFINED: Quad-core, 8GB RAM, Windows 8, QueryGen 3.3, single user,
  ≥50% RAM and ≥70% CPU free, ≥30 Mbps network.
```

**Advantages:** Multiple target levels (Goal/Stretch/Wish) yield richer statements than binary yes/no; unambiguous measurement prevents developer-user debates. **Drawback:** bulkier than simple statements, but the precision outweighs the inconvenience. Even without full formalism, thinking through the keywords yields more precise shared expectations.

### Quality Attribute Trade-offs

Attribute combinations have inescapable trade-offs (see Wiegers Fig. 14-2):
- **`+` (positive):** increasing the row attribute usually helps the column attribute (e.g., portability → interoperability, reusability, verifiability).
- **`–` (negative):** increasing the row attribute usually hurts the column (e.g., performance ↔ modifiability, portability; security → performance).
- **Blank:** little effect.

The matrix is **not symmetrical** — increasing security hurts performance, but increasing performance has little effect on security. Examples of conflicts:
- Maximizing **usability** is hard when software must run on multiple platforms (portability) with minimal modification.
- Highly **secure** systems are hard to fully test for integrity; reused generic components can compromise security.
- Highly **robust** code can suffer reduced **performance** due to data validations and error checking.

> **Overconstraining** system expectations or defining conflicting requirements makes it impossible for developers to fully satisfy the requirements. Use the prioritization step to bias conflict resolution toward higher-priority attributes.

### Implementing Quality Attributes (Translation to Technical Specs)

Quality requirements are nonfunctional but spawn derived **functional requirements, design guidelines, design constraints, or architectural decisions**. Table 14-5 maps quality attributes to likely technical information categories:

| Quality Attributes | Likely Technical Information Category |
|--------------------|--------------------------------------|
| Installability, integrity, interoperability, reliability, robustness, safety, security, usability, verifiability | **Functional requirement** |
| Availability, efficiency, modifiability, performance, reliability, scalability | **System architecture** |
| Interoperability, security, usability | **Design constraint** |
| Efficiency, modifiability, portability, reliability, reusability, scalability, verifiability, usability | **Design guideline** |
| Portability | **Implementation constraint** |

BAs without development experience may not appreciate technical implications — engage knowledgeable stakeholders early. Example: scalability requirements may lead developers to retain performance buffers (disk/CPU/network) and influence hardware/environment decisions. This is why developers should be involved early in requirements elicitation and reviews.

### Constraints

A **constraint** places restrictions on design or implementation choices available to the developer. Constraints are imposed by external stakeholders, interacting systems, life-cycle activities, agreements, management, and technical decisions.

**Sources of constraints:**
- Specific technologies, tools, languages, databases that must be used/avoided
- Operating environment / platform restrictions (browser/OS versions)
- Required development conventions or standards (design notations, coding standards)
- Backward/forward compatibility with earlier products
- Regulatory/compliance requirements
- Hardware limitations (timing, memory, processor, size, weight, materials, cost)
- Physical restrictions from operating environment or user characteristics
- Existing interface conventions when enhancing a product
- Interfaces to existing systems (data formats, communication protocols)
- Display size restrictions (tablet/phone)
- Standard data interchange formats (XML, RosettaNet)

**Detecting hidden constraints:** Users often present "requirements" that are actually solution ideas. The BA must distinguish the underlying need from the constraint the solution imposes. Asking **"why"** a few times generally leads to the real requirement. Some constraints exist to comply with an unstated quality expectation — ask why each constraint is imposed to reach the underlying quality requirement.

**Constraint examples:**
- `CON-1` — User clicks top of project list to change sort sequence. *[UI design constraint]*
- `CON-2` — Only open-source software under GNU GPL may be used. *[implementation constraint]*
- `CON-3` — Application must use Microsoft .NET framework 4.5. *[architecture constraint]*
- `CON-4` — ATMs contain only $20 bills. *[physical constraint]*
- `CON-5` — Online payments may be made only through PayPal. *[design constraint]*
- `CON-6` — All textual data stored as XML files. *[data constraint]*

### Quality Attributes on Agile Projects

- Quality characteristics are **expensive to retrofit** late in development — even agile projects must specify significant quality attributes and constraints **early** so developers can make appropriate architectural/design foundations.
- Nonfunctional requirements need priority alongside user stories; they cannot be deferred to a later iteration.
- Quality requirements can be written as stories: *"As a help desk technician, I want the knowledge base to respond to queries within five seconds so the customer doesn't get frustrated and hang up."*
- Quality requirements **span multiple stories and iterations** — they are not implemented in the same discrete way as user stories and may not be readily divisible into smaller chunks.
- As functionality accumulates across iterations, efficiency and performance can deteriorate — specify performance goals and begin performance testing with early iterations.
- Quality attributes can spawn new **product backlog items** (derived functionality, e.g., a security requirement → multiple security-related user stories).
- **Acceptance tests** can be written for quality attributes to quantify them. Multiple acceptable satisfaction levels mirror Planguage's Goal/Stretch/Wish. Use Planguage's `Scale` and `Meter` to define precisely what "return a result" means and how to test it.
- Part of accepting an iteration as complete is assessing whether pertinent nonfunctional requirements are satisfied.

### Prototyping — What and Why

A **software prototype** is a partial, possible, or preliminary implementation of a proposed new product. It makes requirements more real, brings use cases to life, and closes gaps in understanding. Prototyping introduces customer contact points that reduce the **expectation gap** between user vision and developer understanding.

**Three major purposes** (must be clear from the beginning):
1. **Clarify, complete, and validate requirements** — as a requirements tool: obtain agreement, find errors/omissions, assess accuracy. Especially helpful for poorly understood, risky, or complex parts.
2. **Explore design alternatives** — as a design tool: explore interaction techniques, optimize usability, evaluate technical approaches, demonstrate feasibility.
3. **Create a subset that grows into the ultimate product** — as a construction tool: a functional implementation of a subset, elaborated through small-scale cycles. Safe only if the prototype is designed with eventual release intended from the start.

**Primary reason to prototype:** resolve uncertainties early. You don't need to prototype the entire product — focus on high-risk areas or known uncertainties. For each prototype, know and communicate: *why* you're creating it, *what* you expect to learn, and *what you'll do with it* after evaluation.

### Prototype Attributes — Three Binary Dimensions

Each prototype has a specific combination of these attributes; some combinations don't make sense (e.g., evolutionary paper proof-of-concept).

| Dimension | Option A | Option B |
|-----------|----------|----------|
| **Scope** | **Mock-up** (horizontal prototype) — focuses on user experience, UI facades, navigation; little/no real functionality | **Proof of concept** (vertical prototype) — implements a slice through all architectural layers; works like the real system |
| **Future use** | **Throwaway** (nonreleasable) — discarded after generating feedback | **Evolutionary** — grows into the final product through iterations |
| **Form** | **Paper** (low-fidelity) — sketch on paper, whiteboard, or drawing tool | **Electronic** — working software for part of the solution |

### Mock-ups vs. Proofs of Concept

**Mock-up (horizontal prototype):**
- Usually what people mean by "software prototype"
- Displays UI screen facades with navigation between them; little/no real functionality (think Western movie set — false fronts, nothing behind)
- Functional options, look and feel, navigation structure are demonstrable; some controls do nothing; query results are faked/constant; report contents hardcoded
- Use **actual data** in sample displays/outputs to enhance validity — but make clear to evaluators that displays are simulated, not live
- Doesn't perform useful work, but looks as if it should — good enough for users to judge missing/wrong/unnecessary functionality
- For **throwaway mock-ups**: focus on broad requirements and workflow; don't worry about precise element positioning, fonts, colors, graphics yet
- For **evolutionary mock-ups**: building in refinements moves the UI toward releasable

**Proof of concept (vertical prototype):**
- Implements a slice of functionality from UI through all technical service layers
- Works like the real system because it touches all implementation levels
- Build when uncertain whether a proposed architectural approach is feasible/sound, or to optimize algorithms, evaluate database schema, confirm cloud solution soundness, or test critical timing requirements
- Construct using **production tools** in a **production-like environment** for meaningful results
- Useful for improving estimation accuracy for a user story or functionality block
- Agile projects sometimes call this a **"spike"**

### Throwaway vs. Evolutionary Prototypes

**Throwaway (nonreleasable) prototype:**
- Built to answer questions, resolve uncertainties, improve requirements quality — then discarded
- Build as **quickly and cheaply** as possible; more effort invested → more reluctance to discard → less time for the real product
- Emphasizes quick implementation/modification over robustness, reliability, performance, maintainability
- **Must not** allow low-quality code to migrate into production — users and maintainers will suffer for the product's life
- Most appropriate when facing uncertainty, ambiguity, incompleteness, or vagueness in requirements
- You don't *have* to throw it away (can keep for future reference), but it won't be incorporated into the delivered product
- **Wireframe** is a common throwaway approach for custom UI/website design — explores: (1) conceptual requirements, (2) information architecture/navigation, (3) high-resolution detailed page design. The **dialog map** (Ch 12) is excellent for iterating on page navigation.

> **Trap:** Don't make a throwaway prototype more elaborate than necessary. Resist temptation — or user pressure — to keep adding capabilities.

**Evolutionary prototype:**
- Provides a solid architectural foundation for building the product incrementally as requirements become clear over time
- **Agile development is an example** of evolutionary prototyping: build through iterations, use feedback on early iterations to adjust direction
- Must be built with **robust, production-quality code from the outset** — no room for shortcuts
- Takes longer to create than a throwaway prototype simulating the same capabilities
- Must be **designed for easy growth and frequent enhancement** — emphasize architecture and solid design principles
- First iteration = **pilot release** implementing an initial portion of requirements; lessons from acceptance testing and initial usage lead to modifications in the next iteration
- Full product = culmination of a series of evolutionary prototyping cycles
- Well suited for apps that will grow over time but are valuable without all planned functionality; for web development projects
- Agile projects often plan so they could stop at end of an iteration and still have a useful (if incomplete) product

> **Critical:** You cannot successfully turn the deliberately low quality of a throwaway prototype into the maintainable robustness a production system demands. Working prototypes that serve a handful of concurrent users likely won't scale to thousands without major architectural changes.

**Typical applications (Table 15-1):**

| | **Throwaway** | **Evolutionary** |
|---|---|---|
| **Mock-up** | • Clarify/refine user & functional requirements<br>• Identify missing functionality<br>• Explore UI approaches | • Implement core user requirements<br>• Implement additional requirements by priority<br>• Implement/refine websites<br>• Adapt to rapidly changing business needs |
| **Proof of concept** | • Demonstrate technical feasibility<br>• Evaluate performance<br>• Improve construction estimates | • Implement/grow core multi-tier functionality & communication layers<br>• Implement/optimize core algorithms<br>• Test and tune performance |

### Paper vs. Electronic Prototypes

**Paper prototype (low-fidelity):**
- Cheap, fast, low-tech — tools no more sophisticated than paper, index cards, sticky notes, whiteboards
- Tests whether users and developers share an understanding of requirements
- Low-risk step into the solution space before production code
- **Storyboard** is a similar deliverable
- Use **low-fidelity** to explore functionality and flow; use **high-fidelity** to determine precise look and feel
- Users willingly critique paper designs; they're sometimes less eager to critique a lovely computer-based prototype in which the developer has clearly invested a lot of work. Developers too may resist substantial changes to a carefully crafted electronic prototype.
- **Evaluation method:** someone plays the role of the computer while a user walks through a scenario. User says aloud what she'd do ("I'm going to select Print Preview from the File menu"); the "computer" displays the paper/index card representing that screen. If wrong, take a blank page and try again.
- **Wizard of Oz prototype:** e.g., buy a refrigerator, write COPIER on the box, have someone sit inside and respond as the copier would while a user simulates activities outside. Simple, fun, effective early feedback.
- Faster than any electronic tool; facilitates rapid iteration (a key success factor in requirements development)
- Excellent for refining requirements **prior to** detailed UI design, evolutionary prototyping, or traditional design/construction

**Electronic prototype:**
- Tools range from simple drawing tools (Visio, PowerPoint) to commercial prototyping tools, GUI builders, and dedicated website wireframe tools
- For **throwaway electronic prototypes**: tools let you easily implement/modify UI components regardless of inefficient temporary code
- For **evolutionary prototypes**: must use **production development tools** from the outset
- **Application simulation** tools let you quickly assemble screen layouts, UI controls, navigation flow, and functionality that closely resembles the product — valuable for iterating with user representatives

> **Caution with any prototyping form:** The BA must avoid getting drawn into high-precision UI design prematurely. Evaluators offer feedback like "Can this text be a little darker red?" or "I don't like that font" — unless the prototype's purpose is detailed screen/webpage design, these are distractions. Color, font, and positioning are immaterial if the application doesn't properly support users' business tasks. Focus on refining **requirements**, not visual designs, until you have a rich understanding of necessary functionality.

### Working with Prototypes — Activity Sequence

A progressive refinement sequence (Wiegers Fig. 15-2) moves from use cases to detailed UI design with a throwaway prototype:

1. **Use case descriptions** — sequence of actor actions and system responses
2. **Dialog map** — models a possible UI architecture from the use case
3. **Throwaway prototype / wireframe** — elaborates dialog elements into specific screens, menus, dialog boxes
4. **User evaluation** — feedback may change use case descriptions (alternative flows discovered) or the dialog map
5. **Detailed UI design** — each UI element optimized for usability (after requirements refined and screens sketched)

These activities need not be strictly sequential — **iterating** on the use case, dialog map, and wireframe is the fastest way to an acceptable, agreed-upon UI approach.

**When to stop prototyping:** Only perform as many steps as necessary to acceptably reduce the risk of going wrong on UI design. If the team is confident they understand the requirements, that they're sufficiently complete, and they have a good handle on the right UI, there's little point in prototyping. Focus prototyping on user requirements with **big risk of error** or **big impact if there's a problem**.

**Example — PearlsFromSand.com:**
- Identified use cases per user class (Visitor: get book/author info, read samples/blog, contact author; Customer: order/download product, request assistance; Administrator: manage product list, issue refund, manage email list)
- Drew a **dialog map** of pages and navigation pathways (each box = a page; arrows = links)
- Built a **throwaway wireframe** (PowerPoint, a few minutes) to work out visual design approach with user representatives
- Produced **final implemented page** — culmination of the iterative requirements analysis and prototyping

### Prototype Evaluation

Prototype evaluation is related to **usability testing** (Rubin & Chisnell). You learn more by **watching** users work with the prototype than by just asking what they think.

**What to watch for:**
- Where the user's fingers or mouse pointer try to go instinctively
- Places where the prototype conflicts with behavior of other applications evaluators use
- Incorrect keyboard shortcuts attempted; "mousing around" hunting for the correct menu option
- The furrowed brow of a puzzled user who can't determine what to do next, how to navigate, or how to take a side trip
- Dead ends (e.g., when a user submits a form on a website)

**Who should evaluate:**
- Include members of **multiple user classes**, both experienced and inexperienced
- When presenting, stress that the prototype addresses only a portion of the functionality; the rest will be implemented when the actual system is developed

## Key Takeaways

- **Quality attributes are nonfunctional requirements that make or break product success** — they drive architecture, spawn functional requirements, and are far costlier to retrofit than to design for up front.
- **External** attributes (availability, performance, usability, security, etc.) are user-facing; **internal** attributes (efficiency, modifiability, portability, etc.) are dev/maintainer-facing but indirectly affect customer satisfaction.
- **Use the 5-step Brosseau approach** (broad taxonomy → reduce → prioritize → elicit → specify) and **SMART + fit criteria** to craft measurable quality requirements.
- **Planguage** (Gilb) provides keyword-rich precision: TAG, AMBITION, SCALE, METER, GOAL/FAIL, STRETCH, WISH, DEFINED — multiple achievement levels beat binary yes/no.
- **Quality attributes trade off** — performance vs. modifiability/portability; security vs. performance; usability vs. portability. Prioritize and respect priorities when resolving conflicts. Don't overconstrain.
- **Constraints** restrict design/implementation choices; ask "why" to distinguish the underlying need from an imposed solution and to reach the quality requirement behind the constraint.
- **Even agile projects must specify quality attributes early** — they span stories/iterations, spawn backlog items, and need acceptance tests with multiple satisfaction levels.
- **Prototyping** reduces the expectation gap when users say "IKIWISI." Three purposes: clarify/validate requirements, explore design alternatives, grow into the product.
- **Three binary dimensions** define a prototype: scope (mock-up vs. proof of concept), future use (throwaway vs. evolutionary), form (paper vs. electronic).
- **Never let throwaway prototype code migrate to production.** Evolutionary prototypes must be built with production-quality code from the outset.
- **Paper prototyping** is fast, cheap, low-risk, and users critique it more freely than polished electronic prototypes. Use low-fidelity for functionality/flow; high-fidelity for look and feel.
- **Watch users, don't just ask them** — prototype evaluation is akin to usability testing. Include multiple user classes, experienced and inexperienced.
- **Avoid premature high-precision UI design** during prototyping — focus on requirements and workflow until functionality is well understood.

## My Notes

*(Add personal insights, project experiences, and cross-references here)*

## What's Missing

- Detailed elicitation question banks per quality attribute (see Roxanne Miller 2009; Lauesen 2002 for extensive examples)
- ISO/IEC 9126 / 25010 quality model cross-reference
- Security requirement patterns and threat-modeling integration
- Performance engineering budgets (response-time budget allocation across components)
- Availability patterns: hot standby, warm standby, cold standby, failover clusters, multi-region active-active
- Prototyping tool landscape (current commercial/open-source options)
- Integrating prototyping with ATDD/BDD acceptance criteria

## Relationship to Other Topics

- **[[Software Requirements Overview]]** — Quality attributes and constraints are core nonfunctional requirements; prototyping is a key validation technique.
- **[[03_Business_Rules]]** — Security requirements often originate from corporate security policies (business rules).
- **[[04_Documenting_Requirements]]** — Section 6 of the SRS template (Ch 10) is devoted to quality attributes; associate quality requirements specific to features/components/FRs/user stories with the appropriate item in the requirements repository.
- **[[05_Data_Requirements]]** — Integrity requirements overlap with data dictionary and data modeling; report/dashboard prototyping draws on data requirements.
- **[[06_Use_Cases_and_User_Stories]]** — Prototypes elaborate use case dialog maps into screens; quality requirements span multiple user stories on agile projects.
- **Expectation gap (Ch 2)** — Prototyping is a primary technique for closing the gap between user vision and developer understanding.
- **Requirements reuse (Ch 18)** — Reusability quality attribute; reusable artifacts include requirements, architectures, designs, code, tests, business rules, data models, user class descriptions, stakeholder profiles, glossary terms.

## Source Reference

Karl Wiegers with Joy Beatty, *Software Requirements* (3rd ed.), Microsoft Press, 2013 — Chapter 14 "Beyond Functionality" and Chapter 15 "Risk Reduction Through Prototyping."
