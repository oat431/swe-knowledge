---
tags:
  - requirements
  - business-requirements
  - user-personas
  - vision-scope
  - software-requirements
source: "Wiegers & Beatty, Software Requirements (Ch 5-6)"
chapter: "Ch 5: Establishing the Business Requirements; Ch 6: Finding the Voice of the User"
created: 2026-07-21
aliases:
  - Business Requirements
  - Vision and Scope
  - User Classes
  - Product Champions
---

# Business and User Requirements

> **Top of the requirements chain.** Business requirements define the *vision* of the solution and the *scope* of the project that will implement it. User requirements and functional requirements must align with the context and objectives the business requirements establish. Requirements that don't help the project achieve its business objectives shouldn't be implemented.

---

## 1. Defining Business Requirements (Ch 5)

Business requirements = a set of information that, in aggregate, describes a need that leads to one or more projects to deliver a solution and the desired ultimate business outcomes.

**Four components:**
1. **Business opportunities** — problem being solved / market opportunity
2. **Business objectives** — measurable targets
3. **Success metrics** — indicators to track progress
4. **Vision statement** — long-term purpose of the product

> [!important] Business requirements issues must be resolved *before* functional and nonfunctional requirements can be fully specified. They provide a reference for making decisions about proposed requirement changes and enhancements.

**Tip:** Display business objectives, vision, and scope highlights in *every* requirements elicitation session so the team can quickly judge whether a proposed requirement is in or out of scope.

### 1.1 Identifying Desired Business Benefits

- Organizations should not initiate any project without a clear understanding of the value it will add.
- Set **measurable targets** with business objectives, then define **success metrics** to measure progress.
- Sources: funding sponsors, corporate executives, marketing managers, product visionaries.
- The BA ensures the right stakeholders set the business requirements and facilitates elicitation, prioritization, and conflict resolution.

> [!warning] Simply merging two systems into one is **not** a reasonable business objective. Customers care about increasing revenue and decreasing costs — not about system count. Merging might be *part of the solution*, but it is rarely the true business objective.

---

## 2. Product Vision and Project Scope

Two core elements of the business requirements:

| Element | Description | Temporal |
|---------|-------------|----------|
| **Product Vision** | Succinctly describes the ultimate product that will achieve the business objectives. Provides context for decisions throughout the product's life. Aligns all stakeholders in a common direction. | Changes **slowly** as strategic positioning/business objectives evolve. |
| **Project Scope** | Identifies what portion of the ultimate product vision the *current* project or iteration will address. Draws the boundary between what's in and what's out. | More **dynamic** — adjusted per release within schedule/budget/resource/quality constraints. |

> [!important] The **product vision** ensures we all know where we are hoping to go *eventually*. The **project scope** ensures we are all talking about the same thing for the *immediate* project or iteration.

### 2.1 Make Sure the Vision Solves the Problem

- Teams may set clear objectives and then specify, develop, and test the system *without checking against the objectives along the way*.
- A "shiny" new feature gets added because it seems reasonable and interesting. Months later, the delivered system doesn't solve the original problem despite all its cool features.
- **Scope of future releases** is fuzzier the farther out you look. Manage scope of a specific project as a defined subset of the strategic vision.

### 2.2 Conflicting Business Requirements

Business requirements collected from multiple sources might conflict.

**Example — Kiosk stakeholders:**
- **Developer** wants wide product variety, low cost, rapid development.
- **Retailer** wants customers to linger and spend more money.
- **Customer** wants to spend *less* time purchasing, wide variety, low cost.

> [!note] Some objectives align (variety), some conflict (time spent in store). The project's decision makers — *not* the software team — must resolve these conflicts before the analyst can detail requirements. Focus on delivering maximum business value to primary stakeholders.

- Uncontrolled **scope creep** — stakeholders overstuffing the new system to satisfy every interest — can topple a project under its own weight.
- The BA surfaces potential conflicts, flags misaligned objectives, and facilitates resolution (a political/power struggle outside the book's scope).

---

## 3. Vision and Scope Document

Collects business requirements into a single deliverable that sets the stage for subsequent development work. Owner = **executive sponsor / funding authority**.

| Alternative documents | Purpose |
|-----------------------|---------|
| **Project charter** | Similar purpose (Wiegers 2007) |
| **Business case** | Similar purpose |
| **Market Requirements Document (MRD)** | Commercial software; more detail on target market segments and commercial success factors |

> [!tip] The vision and scope document only defines scope at a **high level**. Scope details are represented by each release baseline. Major new projects should have **both** a complete vision and scope document *and* an SRS.

### 3.1 Template Structure (Figure 5-3)

#### 1. Business Requirements
Projects are launched in the belief that creating or changing a product will provide worthwhile benefits. Business requirements describe the primary benefits to sponsors, buyers, and users.

- **1.1 Background** — Rationale and context; history leading to the decision.
- **1.2 Business opportunity** — Problem being solved (corporate IS) or market opportunity (commercial product). Comparative evaluation of existing products. Needs of typical customers / target market. Known critical interface or quality requirements (omit design specifics).
- **1.3 Business objectives** — Important benefits in a **quantitative and measurable** way. Platitudes and vague improvements are neither helpful nor verifiable.
- **1.4 Success metrics** — Indicators stakeholders use to define and measure success. Measure what is **important**, not just what is easy to measure.
- **1.5 Vision statement** — Concise summary of long-term purpose and intent. Balanced view for diverse stakeholders.
- **1.6 Business risks** — Marketplace competition, timing, user acceptance, implementation issues, negative impacts. Estimate potential loss, likelihood, mitigation. *(Not the same as project risks.)*
- **1.7 Business assumptions and dependencies** — Assumptions believed true without proof. Major dependencies on external factors (pending standards, regulations, deliverables from other projects, suppliers). Note impact if assumption is wrong / dependency breaks.

##### Business Objectives — Examples (Table 5-1)

| Financial | Nonfinancial |
|-----------|--------------|
| Capture X% market share within Y months | Achieve customer satisfaction ≥ X within Y months of release |
| Increase market share in country W from X% to Y% within Z months | Increase transaction-processing productivity by X%; reduce data error rate to ≤ Y% |
| Reach sales volume of X units or revenue $Y within Z months | Develop an extensible platform for a family of related products |
| Achieve X% ROI within Y months | Develop specific core technology competencies |
| Achieve positive cash flow within Y months | Be rated top product for reliability in published reviews by a specified date |
| Save $X/year on a high-maintenance legacy system | Comply with specific federal and state regulations |
| Reduce monthly support costs from $X to $Y within Z months | Receive ≤ X service calls/unit and Y warranty calls/unit within Z months |
| Increase gross margin from X% to Y% within 1 year | Reduce turnaround time to X hours on Y% of support calls |

##### Business Objectives Model (Beatty & Chen 2012)

A hierarchy of related business **problems** (what's keeping the business from meeting goals) and measurable **objectives** (ways to measure achievement). Problems and objectives are intertwined:

- Given objectives → ask *"What is keeping us from achieving the goal?"* → identify a more detailed problem.
- Given a problem → ask *"How will we assess whether the problem is solved?"* → identify a measurable objective.
- Iterate until a list of **features** emerges that would help solve the problems and meet the objectives.

##### Vision Statement — Keyword Template (Moore 2002)

```
For [target customer]
Who [statement of the need or opportunity]
The [product name]
Is [product category]
That [major capabilities, key benefit, compelling reason to buy or use]
Unlike [primary competitive alternative, current system, current business process]
Our product [statement of primary differentiation and advantages of new product]
```

**Sample — Chemical Tracking System:**
> *For* scientists *who* need to request containers of chemicals, *the* Chemical Tracking System *is* an information system *that* will provide a single point of access to the chemical stockroom and to vendors. [...] *Unlike* the current manual ordering processes, *our product* will generate all reports required to comply with federal and state government regulations.

> [!tip] Have several key stakeholders write vision statements **separately**. Comparing them is a good way to spot different understandings of project objectives. It's never too late to write one.

#### 2. Scope and Limitations

- **2.1 Major features** — List with unique persistent labels for tracing. Consider a feature tree diagram.
- **2.2 Scope of initial release** — Capabilities planned for release 1. Define in terms of features, user stories, use cases, use case flows, or external events. Include quality characteristics. **Avoid** bloatware and slipped schedules — focus on highest value at acceptable cost to broadest community in earliest timeframe. Don't neglect nonfunctional requirements (especially those affecting architecture).
- **2.3 Scope of subsequent releases** — Release roadmap. Fuzzier the farther out you look. Short release cycles provide frequent opportunities for learning from customer feedback.
- **2.4 Limitations and exclusions** — Capabilities a stakeholder might expect but that are *not* planned. List items cut from scope so the decision isn't forgotten. E.g., *"The new system will not provide mobile platform support."*

#### 3. Business Context

- **3.1 Stakeholder profiles** — People/groups/organizations actively involved, affected by, or able to influence the outcome. Each profile includes: major value/benefit, likely attitudes, features of interest, known constraints. Value can be: improved productivity, reduced rework, cost savings, streamlined processes, automation, new capabilities, compliance, improved usability.
- **3.2 Project priorities** — Five dimensions: **features, quality, schedule, cost, staff**. Each is one of:
  - **Constraint** — limiting factor within which the PM must operate
  - **Driver** — significant success objective with limited flexibility
  - **Degree of freedom** — factor the PM can adjust to balance against others
  
  > [!important] Not all five can be constraints, and not all can be drivers. The PM needs some degrees of freedom to respond when requirements or realities change.
- **3.3 Deployment considerations** — Access needs (time zones, locations), infrastructure changes (capacity, network, storage, data migration), training/process-modification info.

### 3.2 Template Tactics (Sidebar)

- Populate sections **as information accumulates**; don't fill top-to-bottom.
- Empty sections highlight **gaps** in current knowledge — trigger richer exploration.
- **"Shrink to fit"**: start with a rich template, condense to what each situation needs.
- If a section doesn't apply, put an explicit message: *"No business risks have been identified."*
- Consider embedding common elicitation questions as hidden text for reuse.
- Create a small set of templates for different project types (large new dev, small websites, enhancement projects).

---

## 4. Scope Representation Techniques

The purpose of these models is to foster clear, accurate communication among stakeholders. **Clarity > dogmatic correctness.** Adopt standard notations as team standards.

### 4.1 Context Diagram

- Visually illustrates the boundary between the system and everything else in the universe.
- System = single **circle**; external entities (terminators) = **rectangles** (user classes, organizations, other systems, hardware devices).
- Arrows = flow of data, control, or material between terminators and the system.
- Deliberately provides **no visibility** into the system's internal objects, processes, or data.
- Top level of a data flow diagram (structured analysis) — but useful for *all* projects.

### 4.2 Ecosystem Map (Beatty & Chen 2012)

- Shows all systems related to the system of interest that interact with one another and the nature of those interactions.
- Differs from context diagram: shows systems with a relationship — *including those without direct interfaces*.
- Identify affected systems by determining which ones **consume data** from your system.
- When your project no longer affects any additional data → you've identified the scope boundary.

### 4.3 Feature Tree (Beatty & Chen 2012)

- Visual depiction of the product's features organized in logical groups, hierarchically subdivided.
- Up to **three levels**: L1 (major features) → L2 (subfeatures) → L3 (sub-subfeatures).
- Concise view — ideal for showing executives a quick glance at project scope.
- **Release scoping**: select a specific set of L1/L2/L3 features to implement. Can implement a feature in its entirety or only a portion. Mark up the diagram with colors/font variations, or create a **feature roadmap table**.

### 4.4 Event List

- Identifies **external events** that could trigger behavior in the system:
  - Business events (triggered by users)
  - Temporal events (time-triggered)
  - Signal events (from external components/hardware)
- Only names the events; functional responses are detailed in the SRS via event-response tables.
- **Complements** the context diagram and ecosystem map:
  - Context diagram + ecosystem map describe *who* is involved.
  - Event list identifies *what* those actors might do to trigger behavior.
- Cross-check for correctness and completeness (e.g., *"If a chemical container can be received from a vendor, does Vendor appear in the context diagram and/or ecosystem map?"*).

### 4.5 Other Techniques

- **Affected business processes** — identifying processes touched by the solution helps define the scope boundary.
- **Use case diagrams** — depict the scope boundary between use cases and actors (Ch 8).

---

## 5. Keeping the Scope in Focus

A scope definition is a **structure, not a straitjacket**. Scope change isn't bad if it helps steer the project toward satisfying evolving customer needs.

### 5.1 The Three Responses to a New Requirement

When someone requests a new requirement, the analyst asks: *"Is this in scope?"*

1. **Clearly out of scope** — interesting, but address in a future release or another project.
2. **Clearly within scope** — incorporate if high priority relative to committed requirements. May require deferring/canceling other planned requirements (unless extending project duration).
3. **Out of scope, but a good idea** — broaden the scope, with corresponding changes in budget/schedule/staff. This is a **feedback loop** between user requirements and business requirements. Update the baselined vision and scope document (under change control). Keep a record of *why* requirements were rejected — they have a way of reappearing.

### 5.2 Using Business Objectives for Scoping Decisions

- Business objectives are the **most important factor** in scope decisions.
- Schedule features that add the most value toward business objectives for early releases.
- When a stakeholder wants to add functionality, consider how it contributes to achieving the objectives.
- **Quantify** the contribution (roughly $1,000? $100,000? $1,000,000?) so decisions are based on facts, not emotions.

### 5.3 Assessing Impact of Scope Changes

- Scope increase → PM renegotiates budget, resources, schedule, staff.
- Thoughtfully included **contingency buffers** can absorb some change (Wiegers 2007).
- Common consequence: completed activities must be **reworked**.
- Quality often suffers if resources/time aren't increased when functionality is added.
- Documented business requirements help justify saying *"no"* — or at least *"not yet."*

### 5.4 Vision and Scope on Agile Projects

- Scope of each iteration = user stories selected from a dynamic product backlog, based on relative priority and estimated team capacity per timebox.
- Instead of fighting scope creep, the team **prioritizes** new requirements against existing backlog items and allocates them to future iterations.
- Some agile projects fix overall project duration and modify scope; others fix scope and let duration vary.
- A high-level roadmap of iterations is defined at the beginning; user story allocation per iteration is performed at the start of each iteration.
- Many agile projects conduct an upfront **iteration zero** to define the overarching product vision and business requirements.
- Business objectives help prioritize the backlog to deliver the most business value in the earliest iterations.
- Success metrics should be defined so that as iterative releases go live, success can be measured and the backlog adjusted.

### 5.5 Using Business Objectives to Determine Completion

- A BA familiar with the business objectives can help determine when desired value has been delivered — implying the work is done.
- The project is complete when success metrics indicate a good chance of meeting the business objectives.
- **Vague business objectives** guarantee an open-ended project with no way to know when you're done. Sponsors can't budget/schedule/plan; customers may receive an on-time, on-budget solution that doesn't provide needed value.

> [!quote] Focus on defining clear business requirements for all of your projects. Otherwise, you are just wandering about aimlessly hoping to accomplish something useful without any way to know if you're reaching your destination.

---

## 6. Finding the Voice of the User (Ch 6)

> Success in software requirements depends on getting the **voice of the user** close to the **ear of the developer**.

**Three steps:**
1. Identify the different **classes of users** for your product.
2. Select and work with individuals who **represent** each user class and other stakeholder groups.
3. Agree on who the **requirements decision makers** are for your project.

> [!warning] The "wants" users present don't necessarily equate to the functionality they *need* to perform their tasks. The BA must collect wide-ranging user input, analyze and clarify it, and specify what needs to be built. This is an **iterative process that takes time**. Skipping it guarantees rework, missed deadlines, cost overruns, and customer dissatisfaction.

---

## 7. User Classes

A user class is a subset of the product's users, which is a subset of the product's customers, which is a subset of its stakeholders.

```
Stakeholders ⊃ Customers ⊃ Users ⊃ User Classes
```

An individual can belong to **multiple user classes** (e.g., an administrator who also uses the system as an ordinary user).

### 7.1 Classifying Users

Users may differ in:

- Access privilege / security levels (ordinary user, guest, administrator)
- Tasks performed during business operations
- Features they use
- Frequency of use
- Application domain experience and computer systems expertise
- Platforms (desktop, laptop, tablet, smartphone, specialized devices)
- Native language
- Direct vs. indirect interaction with the system

> [!tip] Group users by **tasks** they perform, not by geography or company type. A banking system's logical user classes are *teller, loan officer, business banker, branch manager* — not *large bank, small bank, credit union* (those are **market segments**).

### 7.2 Favored, Disfavored, and Ignored User Classes

| Class | Description | Treatment |
|-------|-------------|-----------|
| **Favored** | Satisfaction most closely aligned with achieving project business objectives | Preferential treatment in conflict resolution and priority decisions. Not necessarily the paying customer or most politically powerful. |
| **Disfavored** | Groups who aren't supposed to use the product for legal, security, or safety reasons | Build features to **deliberately make it hard** for them: access security, privilege levels, antimalware, usage logging, account lockout after failed logins, CAPTCHA. |
| **Ignored** | Will use the product, but you don't specifically build it to suit them | — |
| **Equal** | All other classes | Standard treatment |

> [!trap] Don't overlook **indirect user classes**. They won't use your application themselves — instead accessing its data or services through other applications or reports. Your *customer once removed* is still your customer.

### 7.3 Non-Human User Classes

User classes need not be human beings. They could be **software agents** (bots):
- Scan networks for information about goods and services
- Assemble custom news feeds
- Process incoming email
- Monitor physical systems and networks
- Perform data mining

**Disfavored non-human classes:** internet agents probing websites for vulnerabilities or generating spam. Specify requirements not to meet their needs but to **thwart** them (e.g., CAPTCHA).

### 7.4 Identifying Your User Classes

**"Expand then contract" collaboration pattern (Gottesdiener 2002):**
1. Ask the project sponsor who they expect to use the system.
2. **Brainstorm** as many user classes as possible (don't worry if there are dozens).
3. Look for groups with similar needs to **combine**, or treat as a major class with subclasses.
4. **Pare down** to ~15 or fewer distinct user classes.

**Analysis models that help:**
- **Context diagram** — external entities are candidates for user classes (Ch 5).
- **Corporate organization chart** — look for:
  - Departments that *participate in* the business process
  - Departments *affected by* the business process
  - Role names where direct/indirect users might be found
  - User classes spanning multiple departments
  - Departments interfacing to external stakeholders

> [!tip] Consider building a **catalog of user classes** that recur across multiple applications. Defining user classes at the enterprise level lets you reuse descriptions in future projects.

### 7.5 Example — Chemical Tracking System User Classes (Table 6-1)

| Name | Number | Description |
|------|--------|-------------|
| **Chemists** (favored) | ~1,000 across 6 buildings | Request chemicals from vendors and stockroom. Use system several times/day — mainly requesting chemicals and tracking containers in/out of lab. Search vendor catalogs for specific chemical structures. |
| **Buyers** | 5 | Purchasing dept. Process chemical requests, place/track orders with vendors. Little chemistry knowledge — need simple query facilities. Won't use container-tracking. ~25 uses/day. |
| **Chemical stockroom staff** | 6 technicians + 1 supervisor | Manage inventory of >500,000 containers. Supply from 3 stockrooms, request new chemicals, track all container movement. Only users of inventory-reporting feature. High transaction volume → features must be automated and efficient. |
| **Health and Safety Dept staff** (favored) | 1 manager | Use system only to generate predefined quarterly reports for federal/state chemical usage and disposal regulations. Manager requests report changes as regulations change — **highest priority, time critical**. |

---

## 8. User Personas

A **persona** is a description of a hypothetical, generic person who serves as a stand-in for a group of users having similar characteristics and needs (Cooper 2004; Leffingwell 2011).

**Purposes:**
- Help bring user classes to life.
- Understand requirements and design the user experience for specific user communities.
- Serve as a **placeholder** when the BA doesn't have an actual user representative at hand — envision the persona performing a task to draft a requirements starting point.

**Persona details (commercial customer):**
- Social and demographic characteristics
- Behaviors, preferences, annoyances

> [!important] Make sure personas truly are **representative** of their user class, based on market, demographic, and ethnographic research.

### 8.1 Example Persona — Fred the Chemist

> *Fred, 41, has been a chemist at Contoso Pharmaceuticals since he received his Ph.D. 14 years ago. He doesn't have much patience with computers. Fred usually works on two projects at a time in related chemical areas. His lab contains approximately 300 bottles of chemicals and gas cylinders. On an average day, he'll need four new chemicals from the stockroom. Two of these will be commercial chemicals in stock, one will need to be ordered, and one will come from the supply of proprietary Contoso chemical samples. On occasion, Fred will need a hazardous chemical that requires special training for safe handling. When he buys a chemical for the first time, Fred wants the material safety data sheet emailed to him automatically. Each year, Fred will synthesize about 20 new proprietary chemicals to go into the stockroom. Fred wants a report of his chemical usage for the previous month to be generated automatically and sent to him by email so that he can monitor his chemical exposure.*

**Using personas:**
- As the BA explores requirements, think: *"What would Fred need to do?"*
- Working with a persona makes the requirements thought process more **tangible** than contemplating a faceless group.
- Some teams attach a random human face of appropriate gender to make the persona seem even more real.
- **Leffingwell (2011):** design the system to make it easy for the individual described in your persona to use the application. Focus on meeting that one (imaginary) person's needs — provided the persona accurately represents the user class, this helps satisfy the whole class.

---

## 9. Connecting with User Representatives

Every kind of project — corporate IS, commercial applications, embedded systems, websites, contracted software — needs suitable representatives to provide the voice of the user.

> [!important] Users should be involved **throughout the development life cycle**, not just in an isolated requirements phase at the beginning. Each user class needs someone to speak for it.

### 9.1 Communication Pathways

The most direct communication occurs when developers talk to appropriate users themselves (developer also performing the BA role). This works on very small projects but **doesn't scale** to large projects with thousands of users and dozens of developers.

> [!warning] As in the children's game "Telephone," intervening layers between user and developer increase the chance of **miscommunication** and delay transmission. Some layers add value (skilled BA collecting/evaluating/refining input), but recognize the risks of using marketing staff, product managers, or SMEs as **surrogates** for the actual voice of the user.

### 9.2 Access Strategies

| Project type | Strategy |
|-------------|----------|
| Corporate IS (own company) | Easiest to gain access to actual users |
| Commercial software | Engage beta-testing / early-release sites; set up **focus groups** of current or competitors' users; ask them instead of guessing |

> [!tip] Focus groups should include **both expert and less experienced** customers. If your focus group represents only early adopters or blue-sky thinkers, you'll end up with sophisticated, technically difficult requirements that few customers find useful.

---

## 10. The Product Champion

> **Product champion** = a key member of the user community who serves as the primary interface between members of a single user class and the project's business analyst (Wiegers 1996). Provides an effective way to structure the customer-development collaborative partnership.

### 10.1 Characteristics of Great Product Champions

- **Actual users**, not surrogates (not funding sponsors, marketing staff, user managers, or developers imagining themselves to be users).
- Have a **clear vision** of the new system.
- **Enthusiastic** — they see how it will benefit them and their peers.
- **Effective communicators** respected by their colleagues.
- Thorough understanding of the application domain and operating environment.

> [!note] Requirements development is a **shared responsibility** of the BA and selected users, although the BA should actually *write* the requirements documents. It's not realistic to expect users who have never written requirements to do a good job.

### 10.2 Benefits Beyond Requirements

- Champions can **speak out on the software team's behalf** with their colleagues, breaking down customer/development tension.
- Champions can **lead adoption** of the application by the user community — a success metric managers appreciate.
- Offer public **reward and recognition** for their contributions.

### 10.3 Empowerment

> [!important] The product champion approach works best if each champion is **fully empowered** to make binding decisions on behalf of the user class they represent. If a champion's decisions are routinely overruled, their time and goodwill are being wasted.

However, champions must remember they are **not the sole customers**. Problems arise when the liaison doesn't adequately communicate with peers and presents only their own wishes.

### 10.4 External Product Champions (Commercial Software)

When developing commercial software, it can be difficult to find champions from outside your company.

**Strategies:**
- Rely on internal SMEs or outside consultants as surrogates (watch for disconnects with real users).
- Engage major corporate customers who might welcome participation in requirements elicitation.
- Offer **economic incentives** — discounts on the product, payment for time spent.
- **Hire** a suitable product champion with the right background (e.g., a company hired three store managers; a medical software company hired a doctor).
- Send BAs **to customer sites** to work with the customers' staff.

> [!warning] Anytime the product champion is a former or simulated user, watch out for **disconnects** between the champion's perceptions and the current needs of real users. Some domains change rapidly; others are stable. The essential question: can the champion accurately represent the needs of **today's** real users?

### 10.5 Product Champion Activities (Table 6-2)

| Category | Activities |
|----------|-----------|
| **Planning** | Refine scope and limitations; identify interacting systems; evaluate impact on business operations; define transition path; identify relevant standards/certifications |
| **Requirements** | Collect input from other users; develop usage scenarios/use cases/user stories; resolve conflicts within user class; define implementation priorities; provide performance/quality input; evaluate prototypes; resolve cross-stakeholder conflicts; provide specialized algorithms |
| **Validation & verification** | Review requirements specs; define acceptance criteria; develop user acceptance tests; provide test data sets; perform beta/UAT testing |
| **User aids** | Write user documentation/help text; contribute to training materials; demonstrate system to peers |
| **Change management** | Evaluate/prioritize defect corrections and enhancements; dynamically adjust scope of future releases; evaluate change impact on users/processes; participate in change decisions |

### 10.6 Multiple Product Champions

One person can rarely describe the needs for all users of an application.

**Chemical Tracking System example:**
- 4 major user classes → 4 product champions
- 3 BAs worked with the 4 champions (one BA handled two small classes: Buyer + Health and Safety)
- Champions were **not full-time** — several hours per week
- One BA assembled all input into a unified SRS
- The Chemist champion assembled a **backup team** of chemists to represent the diverse needs of ~1,000 chemists across 6 buildings

---

## Key Takeaways

1. **Business requirements sit at the top of the requirements chain** — they define vision and scope, and all other requirements must align with them.
2. **Business objectives must be measurable.** Platitudes are neither helpful nor verifiable. Use them to make scoping decisions and determine when the project is done.
3. **Vision ≠ scope.** Vision is long-term and slow-changing; scope is per-project/iteration and dynamic.
4. **The vision and scope document** is the primary deliverable for business requirements — but adapt the template to your needs ("shrink to fit").
5. **Scope representation techniques** (context diagram, ecosystem map, feature tree, event list) foster clear communication. Use the ones that provide the most insight per project.
6. **Scope change isn't inherently bad** — manage it consciously, by the right people, for the right business reasons, with accepted tradeoffs.
7. **Identify user classes early** using "expand then contract." Group by tasks, not by market segment.
8. **Favored, disfavored, and ignored** user classes drive different design decisions.
9. **Personas** bring user classes to life and make requirements thinking more tangible.
10. **Product champions** are the critical bridge between user classes and the BA — they must be actual users, empowered, and representative of their class.

---

## Related Notes

- [[01_Software_Requirements_Fundamentals]] — Ch 1-2: Essential software requirement, customer perspective
- [[03_Requirements_Elicitation]] — Ch 7: Elicitation techniques
- [[04_Understanding_User_Requirements]] — Ch 8: Use cases and user stories
- [[05_Documenting_Requirements]] — Ch 10: SRS template and structure

---

*Source: Karl Wiegers & Joy Beatty, _Software Requirements_ (2nd/3rd ed.), Chapters 5-6.*
