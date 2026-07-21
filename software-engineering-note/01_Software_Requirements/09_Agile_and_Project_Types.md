---
tags:
  - requirements
  - agile
  - project-types
  - embedded
  - software-requirements
source: "Wiegers & Beatty, Software Requirements (Ch 20-26)"
chapter: "Ch 20: Agile Projects; Ch 21: Enhancement & Replacement Projects; Ch 22: Packaged Solution Projects; Ch 23: Outsourced Projects; Ch 24: Business Process Automation Projects; Ch 25: Business Analytics Projects; Ch 26: Embedded & Other Real-Time Systems Projects"
created: 2026-07-21
aliases:
  - Agile Requirements
  - Enhancement Projects
  - Replacement Projects
  - COTS
  - Packaged Solutions
  - Outsourced Projects
  - Business Process Automation
  - BPA
  - Business Analytics
  - Embedded Systems
  - Real-Time Systems
---

# Agile & Project Types

> **From one-size-fits-all to fit-for-purpose.** Not all software projects face the same requirements challenges. A green-field web app, a legacy mainframe replacement, a COTS implementation, an offshore contract build, a process automation initiative, a big-data analytics platform, and a pacemaker's firmware all demand *the same core requirements practices* — elicitation, analysis, specification, validation, management — but adapted in timing, depth, documentation, and emphasis. Part III of Wiegers is about making those adaptations deliberately rather than by default.

---

## 1. Agile Projects (Ch 20)

### 1.1 What "Agile" Means for Requirements

- **Agile development** = a family of iterative/incremental methods (Scrum, XP, Lean, FDD, Kanban) emphasizing continuous stakeholder collaboration and frequent delivery of small, useful increments.
- Roots in the 2001 **Manifesto for Agile Software Development** (Beck et al.); iterative approaches predate it (Boehm 1988; Gilb 1988).
- All agile methods favor an **adaptive (change-driven)** approach over a **predictive (plan-driven)** one (Boehm & Turner 2004; IIBA 2009).
  - *Predictive* (waterfall): minimize risk via extensive up-front planning & documentation. Works when requirements are well-understood and stable.
  - *Adaptive* (agile): designed to accommodate inevitable change. Works for uncertain or volatile requirements.
- **Key differentiator across life cycles:** time elapsed between when a requirement is created and when software based on it reaches customers.

> [!important] Wiegers deliberately avoids the term "agile requirements." Requirements for agile projects are *not qualitatively different* — developers need the same information. The differences are in **timing, depth, and documentation extent**, not in the nature of the requirements themselves. Use the phrase "requirements *for* agile projects."

### 1.2 Limitations of the Waterfall

- In theory: catch flaws early, allocate budget precisely, measure progress, estimate completion accurately.
- In practice: rarely sequential; stakeholders **will** change requirements mid-project because:
  - They don't know precisely what they want at the start.
  - They can articulate a vision only after seeing something that *doesn't* match it.
  - Business needs change during the project.
- Large waterfall projects are often delivered late, missing features, and failing expectations — the layers of dependency built on early requirements become a liability when those requirements shift.
- **Royce (1970)** — often credited with the formal waterfall model — actually presented it as "risky and invites failure" and advocated overlapping phases and iterative feedback. He even proposed prototyping/simulation before full commitment.

### 1.3 The Agile Approach

- Development is broken into short **iterations** (or **sprints** in Scrum), 1–4 weeks long.
- Each iteration: team adds a small set of functionality (customer-prioritized), tests it, validates against customer acceptance criteria.
- Subsequent increments **modify, enrich, add, and correct** — guided by ongoing customer participation.
- **Goal:** potentially shippable software at the end of *every* iteration, even if only a sliver of the ultimate product.

### 1.4 Essential Aspects of Agile Requirements

| Aspect | Agile vs. Traditional |
|---|---|
| **Customer involvement** | Waterfall: heavy up-front + UAT, little during construction. Agile: continuous engagement; product owner available throughout to clarify, prioritize, test, and give feedback. |
| **Documentation detail** | Waterfall: detailed specs (customers absent during build). Agile: minimum documentation needed to guide developers/testers; precision developed through **conversations**. Not "no documentation" — *just-enough* documentation. Risky/high-impact stories get more detail (often as acceptance tests). |
| **Backlog & prioritization** | One product backlog (user stories, other requirements, defects). Ongoing reprioritization for each iteration. Trace backlog items to business requirements to facilitate prioritization. |
| **Timing** | Same requirement *types* needed, but detailed requirements are **not all documented up front**. High-level stories populate backlog early; details clarified within the iteration implementing them. **Exception:** learn nonfunctional requirements early so architecture can be designed for performance, usability, availability. |

### 1.5 Epics, User Stories, and Features

- **User story** — concise statement of a user need; starting point for conversation (not a spec). Sized to be fully implementable in a single iteration.
- **Epic** (Cohn 2010) — a user story too large for one iteration; must be split into smaller stories. Sometimes subdivided into multiple epics first. This breakdown is **story decomposition** (IIBA 2013).
- **Feature** — a grouping of system capabilities providing value to a user; could encompass one story, multiple stories, one epic, or multiple epics.
- **MMF (Minimum/Minimally Marketable Feature)** (Denne & Cleland-Huang 2003) — smallest set of functionality that delivers value to the customer.

> [!tip] Don't obsess over whether a thing is a "story," "epic," or "feature." Focus on developing **high-quality requirements** that guide developers to satisfy customer needs.

### 1.6 Expect Change

- The biggest BA mindset shift on agile: when a change arises, say **"Okay, let's talk about the change"** — not "that's out of scope" or "we need a formal change process."
- Encourage customer collaboration to create/change stories and prioritize each change against the existing backlog.
- Anticipate *and embrace* change, but manage it thoughtfully.
- **Look ahead** — don't design for every possible future, but a glimpse of what's coming lets developers create more expandable architectures or design hooks.
- Change also means **removing** items from scope (implementation issues, unacceptable stories discovered during testing, higher-priority items displacing planned ones).

### 1.7 Adapting Requirements Practices to Agile

- Most practices in the book adapt to agile — by altering **timing, degree, or who performs** each practice.
- Table 20-1 road map: Ch 2 (agreement), Ch 4 (BA role), Ch 5 (vision/scope), Ch 6 (user representation), Ch 8 (user stories), Ch 10 (specifying for agile), Ch 12 (modeling), Ch 14 (quality attributes up front), Ch 15 (evolutionary prototyping), Ch 16 (prioritization), Ch 17 (acceptance criteria/tests), Ch 27 (backlogs, burndown), Ch 28 (change management).

### 1.8 Transitioning to Agile — Suggestions for BAs

- Determine your role on the team (dedicated BA vs. others performing BA activities).
- Read up on the **product owner** role (e.g., Pichler 2010): user stories, acceptance tests, backlog prioritization, why the agile BA is never "finished" until release end.
- Identify agile practices that fit your organization; carry forward what already works.
- Pilot with a small project, or adopt a few agile practices on your next project.
- For hybrid models, start with low-risk practices that work in any methodology.
- If new to agile, bring in an experienced coach for 3–4 iterations to prevent reverting to comfortable old habits.
- **Don't be an agile purist for the sake of being a purist.**

> [!warning] **Cautionary tale** — One organization tried to adopt agile dogmatically across the entire org at once: story cards only, no other documentation, customers unprepared for their new role. It failed badly; the IT executive mandated a return to waterfall. The dev teams quietly adopted a *hybrid* (backlogs, 3-week iterations, just-in-time detailed requirements) and called it "standard waterfall" to avoid trouble. Most agile practices actually worked well once executed properly. Lesson: **be agile when adopting agile.**

---

## 2. Enhancement & Replacement Projects (Ch 21)

### 2.1 Definitions

- **Enhancement project** — adds new capabilities to an existing system; may also fix defects, add reports, modify functionality for revised business rules.
- **Replacement (reengineering) project** — replaces an existing application with a new custom-built system, a COTS system, or a hybrid. Typically driven by performance improvement, cost reduction (maintenance/licensing), modernization, or regulatory compliance.
- Most organizations devote substantial effort here, not to green-field development.

### 2.2 Expected Challenges

- Changes could **degrade** performance users are accustomed to.
- Little or no requirements documentation for the existing system.
- Users familiar with current behavior may **resist** changes.
- Risk of **breaking or omitting** functionality vital to some stakeholder group.
- Stakeholders may request **gold-plating** — new functionality that isn't really needed.
- Existing documentation, if present, may be **out of date** or not match reality.
- For replacements: don't blindly carry forward *all* old functionality — some shouldn't be migrated.

### 2.3 Valuable Techniques for Existing Systems (Table 21-1)

| Technique | Why it's relevant |
|---|---|
| **Feature tree** | Show features being added; identify existing features that won't be in the new system. |
| **User classes** | Assess who is affected; identify new user classes whose needs must be met. |
| **Business processes** | Understand how the current system intertwines with stakeholders' daily jobs; define new processes to align with new features/replacement. |
| **Business rules** | Record rules currently embedded in code; look for new rules; redesign to handle volatile rules that were expensive to maintain. |
| **Use cases / user stories** | Understand what users must do; how new features should work; prioritize functionality. |
| **Context diagram** | Identify/document external entities; extend interfaces for new features; identify interfaces that need changing. |
| **Ecosystem map** | Find other affected systems; identify new, modified, and obsolete interfaces. |
| **Dialog map** | See how new screens fit existing UI; show navigation changes. |
| **Data models** | Verify existing model sufficiency or extend it; confirm all entities/attributes still needed; consider data migration/conversion/archival/discard. |
| **Quality attributes** | Ensure new system fulfills quality expectations; improve over the existing system. |
| **Report tables** | Convert existing reports still needed; define new reports. |
| **Prototypes** | Engage users in redevelopment; prototype major enhancements if uncertain. |
| **Inspect SRS** | Identify broken traceability links; determine if previous requirements are obsolete/unnecessary. |

> [!tip] Enhancement projects are ideal **low-risk pilots** for trying new requirements methods in bite-sized chunks — reducing risk before applying them on a big green-field project.

### 2.4 Prioritizing with Business Objectives

- **Enhancement risk:** getting caught up in excitement and adding unnecessary capabilities (gold-plating). Trace requirements back to business objectives.
- Prioritize enhancement requests **against correction of existing defects**.
- **Replacement risk:** uncontrolled scope growth — customers imagine "while you're building a new system anyway, add lots of capabilities." Many replacement projects have collapsed under this weight.
- Better strategy: stable first release (lets users do their jobs), then add features through subsequent enhancements.
- Justify replacement with **anticipated cost savings** (e.g., reduced maintenance) **+ value of new functionality** (Devine 2008).
- Look for existing functionality that **doesn't need to be retained**. Don't replicate shortcomings; don't miss opportunities to update for new business needs.
- Use usage data: screens/functions/entities rarely accessed may not need re-implementation.

> [!warning] **Trap** — Don't let stakeholders justify requirements with "I have it today, so I need it in the new system." That's not a justification.

### 2.5 Gap Analysis

- **Gap analysis** = comparison of functionality between existing system and desired new system.
- Can be expressed as use cases, user stories, or features.
- **Enhancement:** perform gap analysis to understand *why* the current system isn't meeting business objectives.
- **Replacement:** understand existing functionality + discover desired new functionality (Figure 21-1):
  - Existing user requirements to re-implement.
  - New user requirements the existing system doesn't address.
  - Change requests never implemented in the existing system.
  - Prioritize existing + new requirements **together**.

### 2.6 Maintaining Performance Levels

- Existing systems set user expectations for performance/throughput.
- Stakeholders have **KPIs** for existing processes they'll want to maintain.
- **KPIM (Key Performance Indicator Model)** (Beatty & Chen 2012) — identifies and specifies these metrics for corresponding business processes; helps stakeholders see business outcomes will be at least as good as before.
- Unless explicitly planned, performance can be compromised as systems are enhanced (example: data sync tool that grew from 1 hour to 20 hours, breaching the implicit overnight window).
- For replacements: prioritize KPIs that matter most; trace business processes → KPIs → requirements; implement the enabling requirements first.

### 2.7 When Old Requirements Don't Exist — "Software Archaeology"

- Most older systems lack documented (let alone accurate) requirements.
- **Reverse-engineer** understanding from UIs, code, and database.
- **Record findings** as requirements and design descriptions — halts the knowledge drain and positions the team for low-risk enhancement/replacement.
- If updating requirements is overly burdensome, it falls by the wayside. But what's the cost if a future maintainer has to *rediscover* it?
- **Incremental cost of recording newly found knowledge is small** compared to the cost of someone rediscovering it later.
- Add new requirements to an existing repository; for replacements, document the new system's requirements and keep them current throughout the project.
- **Try to leave the requirements in better shape than you found them.**

### 2.8 Which Requirements to Specify?

- Rarely worth generating a complete set for an entire production system.
- Two extremes: (a) no documentation forever, (b) reconstruct perfect spec. Most situations lie between.
- Focus detailed requirements efforts on **the changes needed to meet business objectives**.
- For replacements: start with areas most important to business objectives or highest implementation risk.
- Use the "region A/B" mental model (Figure 21-2):
  - **Region A** = new functionality being added. Record in structured SRS or RM tool.
  - **Region B** = the portion of the existing system you must understand to make region A fit seamlessly.
  - Rarely need to document the entire existing system.

### 2.9 Level of Detail

- For enhancements: defining requirements for new functionality alone *might* suffice.
- Usually beneficial to also document **closely related functionality** (region B) so the change fits seamlessly.
- Example: adding a discount code to a shopping cart → also capture other shopping cart user stories; valuable next time you modify the cart.
- Decision to improve old requirements depends on **product longevity and reuse plans**:
  - Long-lived product line (10+ years), spin-offs planned → invest in improving version 1's SRS.
  - Well-worn system expected to retire within a year → not worth reconstructing comprehensive requirements.

### 2.10 Trace Data

- Traceability helps enhancement developers know which components to modify when a requirement changes.
- Ideal: full functional requirements → establish traceability between old and new systems to avoid overlooking requirements.
- Reality: poorly documented old systems lack trace info; establishing it for both is time-consuming.
- **Good practice:** create a traceability matrix linking new/changed requirements to design elements, code, and test cases as you do the work. Accumulating links incrementally is cheap; regenerating them from a completed system is expensive.
- For replacements: trace at a **high level** — list features/stories for existing system, prioritize, determine which will be implemented in the new system.

### 2.11 Discovering Requirements of an Existing System

- **Enhancement:** draw dialog maps for new screens showing navigation to/from existing elements; write use cases/stories spanning new and existing functionality.
- **Replacement:** understand all desired functionality (like any new project). Study:
  - Existing UI → candidate functionality.
  - Existing system interfaces → data exchanged today.
  - How users use the current system.
  - Code or database (if no one understands the functionality/business rules behind the UI).
  - Existing documentation (design docs, help screens, user manuals, training materials).
- You may not need functional requirements for the existing system at all — instead, create **models** to fill the information void:
  - Swimlane diagrams (how users do jobs today).
  - Context diagrams, DFDs, ERDs.
  - High-level user requirements (without all details).
  - Data dictionary entries when adding/modifying data elements.
  - **Test suite** as an alternative view of requirements.

### 2.12 Encouraging New System Adoption

- Expect resistance; people are naturally reluctant to change.
- A new feature that makes jobs easier is good *from our view*, but users are accustomed to current behavior.
- For replacements: changing the entire look/feel, menus, environment, possibly the user's whole job.
- Users may fear disruption, loss of productivity, or even job loss.
- Mitigations:
  - Understand business objectives and user requirements — miss the mark and you lose trust quickly.
  - During elicitation, focus on **benefits to users**, not just organizational value.
  - Show prototypes early to reveal adoption issues.
  - Let users know ASAP about features they might lose or quality attributes that might degrade.
  - System adoption involves as much **emotion as logic** — expectation management is critical.
- **Transition requirements** (IIBA 2009) — capabilities the *whole solution* must have to enable moving from existing to new system:
  - Data conversions, user training, organizational/business process changes, running old and new in parallel.
- **Caveat:** KPIs for certain groups may be negatively affected even if the org benefits overall.

### 2.13 Can We Iterate?

- **Enhancement projects are incremental by definition** — agile methods fit readily (prioritize enhancements via product backlog).
- **Replacement projects** don't always lend themselves to incremental delivery — users need a *critical mass* of functionality before they can use the new system for their jobs.
- Big-bang migrations are also challenging/unrealistic for mature, multi-release systems.
- **Incremental approach:** identify functionality that can be **isolated** and build just those pieces first.
  - Example: inventory management (~10% of fulfillment system) moved to a new system first — separate user group, clear boundary. Downside: new interface needed between new inventory system and truncated fulfillment system.
- Biggest challenges: lack of documentation for existing system + battle for user adoption.

---

## 3. Packaged Solution Projects (Ch 22)

### 3.1 COTS and SaaS Overview

- **Packaged solutions** (COTS — Commercial Off-The-Shelf) and **SaaS/cloud** solutions are acquired and adapted rather than built from scratch.
- Whether purchasing a package as part or all of a solution, you **still need requirements** — to evaluate candidates and then adapt the chosen package.
- COTS packages typically need to be **configured, integrated, and/or extended** (Figure 22-1). Some deploy out-of-the-box; most require customization.
- This chapter treats COTS and SaaS identically due to similar requirements activities.
- The COTS-vs-custom decision is outside scope; this chapter covers requirements for *selecting and implementing* packaged solutions.

### 3.2 Requirements for Selecting Packaged Solutions

- COTS offers **less flexibility** than custom (bespoke) development — you must know which capabilities are non-negotiable vs. adjustable.
- The only way to choose the right package is to understand the **business activities** the package must let users perform.
- Level of detail/effort depends on: expected package costs, evaluation timeline, number of candidates.
  - Buying personal finance software → name the most important use cases.
  - Buying a multimillion-dollar financial app for 5,000 people → full use cases, data models, quality requirements.
- Examples:
  - Law office: 20 tasks → 10 features → 4 candidate packages. Lightweight evaluation appropriate.
  - Semiconductor plant: 50 people, 6 months on selection alone, 3 candidates — high cost justified detailed evaluation.

### 3.3 Developing User Requirements

- Any package must let users accomplish task objectives, though different solutions do so differently.
- **Majority of COTS acquisition requirements effort should be at the user requirements level.**
- Use cases and user stories work well; process models (if they exist) are useful.
- **Little point** in specifying detailed functional requirements or designing a UI — the vendor already did that.
- List desired **features** from understanding user needs and business processes.
  - Example: "As a Research Manager, I need to review and approve new experiments..." → identifies need for an approval workflow feature.
- No package will accommodate every use case → **prioritize**. Trace to business requirements. Distinguish:
  - Must be available day one.
  - Can wait for future extensions.
  - Users can live without (perhaps forever).

### 3.4 Considering Business Rules

- Identify pertinent business rules the COTS product must conform to.
- Can you configure the package to comply with corporate policies, industry standards, regulations?
- How easily can you modify the configured package when rules change?
- Focus on the most important business rules (evaluating all is time-consuming).
- Questions about vendor-provided rules (e.g., income tax withholding):
  - Do you trust they're implemented correctly?
  - Timely updates when rules change? At what cost?
  - Will the vendor supply a list of business rules the package implements?
  - Can you disable/modify/work around intrinsic rules that don't apply to you?
  - Does the vendor accept enhancement requests? How prioritized?

### 3.5 Identifying Data Needs

- Define data structures required to satisfy user requirements and business rules, especially if integrating into an existing application ecosystem.
- Look for major disconnects between your data model and the vendor's.
- Don't be distracted by differently-named entities/attributes — focus on entities/attributes that **don't exist** in the package or have significantly different definitions.
- Specify **reports** the COTS product must generate:
  - Correct formats for mandated reports?
  - Extent of customization of standard reports?
  - Can you design new reports to integrate with vendor-supplied ones?

### 3.6 Defining Quality Requirements

- Quality attributes (Ch 14) are vital for COTS selection. Explore at least:

| Attribute | Questions |
|---|---|
| **Performance** | Maximum acceptable response times? Can it handle anticipated concurrent users/transaction throughput? |
| **Usability** | Conforms to UI conventions? Similar to existing apps? How easily learned? Vendor training included in cost? |
| **Modifiability** | How hard to modify/extend? Appropriate hooks/APIs? Do extensions survive version upgrades? |
| **Interoperability** | How easily integrates with enterprise apps? Standard data interchange formats? Forces upgrades to other tools? |
| **Integrity** | Safeguards data from loss, corruption, unauthorized access? |
| **Security** | Controls over user access/functions? Necessary privilege levels definable? **For SaaS:** evaluate SLAs very carefully. |

### 3.7 Evaluating Solutions

- Initial market research to identify viable candidates.
- Use requirements as **evaluation criteria**.
- **One evaluation approach** (Lawlis et al. 2001):
  1. **Weight** requirements on a scale of 1–10 (importance).
  2. **Rate** each candidate: 1 (full satisfaction), 0.5 (partial), 0 (none). Use product literature, vendor RFP responses, or direct examination. **Direct examination necessary for high-priority requirements.**
  3. **Calculate score** for each candidate based on weighted factors.
  4. Evaluate product cost, vendor experience/viability, vendor support, external interfaces, compliance with technology constraints. **Evaluate candidates initially without considering cost.**
- Consider requirements not met by any candidate → will require extension development (significant cost).
- **Alternative approach:** derive tests from high-priority use cases; walk through them with candidate packages. Or run the product through a suite of scenarios representing expected usage patterns (**operational profile**, Musa 1999).

> [!warning] **Trap** — If you don't have at least one person whose involvement spans all evaluations, there's no assurance comparable interpretations of features and scores were used.

- **Output:** an evaluation matrix (selection requirements in rows, candidate scores in columns). Figure 22-2 shows a sample for an RM tool.

### 3.8 Multi-Stage Evaluation (Example)

- Selecting an RM tool: 200 features, 60 vendor choices — too many for the timeline.
- **First pass:** 30 most important/distinguishing features → narrowed to 16 tools.
- **Second pass:** 16 tools against all 200 features → 5 closely ranked tools.
- **Third pass:** try each of the 5 on **real projects** (not just the tutorial) → select favorite.
- Also do an objective analysis alongside the subjective real-project evaluation.

### 3.9 Requirements for Implementing Packaged Solutions

- Four types of COTS implementations (Table 22-1, Figure 22-3), not mutually exclusive:

| Type | Description |
|---|---|
| **Out-of-the-box** | Install and use as is. |
| **Configured** | Adjust settings without writing new code. |
| **Integrated** | Connect to existing systems; usually requires some custom code. |
| **Extended** | Develop additional functionality with custom code to close needs gaps. |

- Any of these might also require infrastructure changes (OS upgrades, etc.).
- **Advantage:** package may provide useful capabilities you hadn't originally sought — discovered during implementation.

#### 3.9.1 Configuration Requirements

- Essential to most successful COTS implementations.
- Define configuration requirements **one process flow, use case, or user story at a time**.
- Walk through user manuals to learn how to execute a specific task, looking for settings to configure.
- Consider the **full set** of business rules, not just those examined during selection.
- Decision tables and decision trees can model these requirements.
- Use a **roles and permissions matrix** (Figure 9-2) to define roles and permissions using the package's predefined mechanisms.

#### 3.9.2 Integration Requirements

- Unless standalone, you'll need to integrate the package into your application environment.
- Precisely specify requirements for interchanging data and services between the package and other components.
- Custom code forms:
  - **Adapters** — modify interfaces or add missing functionality.
  - **Firewalls** — isolate the COTS software from other enterprise parts.
  - **Wrappers** — intercept inputs/outputs and modify data as necessary (NASA 2009).

#### 3.9.3 Extension Requirements

- **Goal:** minimize customizations — otherwise you should have custom-built.
- For each gap, decide:
  - **Ignore** it (remove the requirement, live with the tool).
  - **Change how you do something** outside the solution (modify the business process).
  - **Build something** to bridge the gap (extend the solution).
- If extending: fully specify requirements for new capabilities as you would for any new product development.
- If replacing an older system: also see Ch 21 practices.
- Assess whether extensions could negatively affect existing package elements/workflows.

#### 3.9.4 Data Requirements

- Begin with data requirements from the selection process.
- **Map** data entities/attributes from your existing data dictionary to the COTS entities/attributes.
- Handle data gaps (entities/attributes the solution doesn't handle) by adding attributes or repurposing existing structures.
- Without proper mapping, **data conversion will lose unmapped data**.
- Use **report tables** (Ch 13) to specify deploying existing or new reports. Many COTS packages provide standard report templates.

#### 3.9.5 Business Process Changes

- COTS is usually selected because it's expected to be cheaper than custom build.
- Organizations must be prepared to **adapt business processes** to the package's workflow capabilities and limitations — the opposite of custom development, where software accommodates processes.
- A COTS solution fully configurable to your existing processes is likely **expensive and complex** (more knobs = harder to configure).
- Strike a balance between desired functionality and out-of-the-box capability (Chung, Hooper, Huynh 2001).
- Start with user requirements from selection. Develop use cases or swimlane diagrams to understand how tasks change.
- Involve users early — they're more willing to accept changes they helped shape.
- Example (insurance company): modeled as-is processes → studied package manuals → created to-be processes → created data dictionary with COTS mapping column. Users helped develop all work products, so no surprises.

### 3.10 Common Challenges with Packaged Solutions

| Challenge | Mitigation |
|---|---|
| **Too many candidates** | Select a short list of criteria to narrow to a few top choices for refined evaluation. |
| **Too many evaluation criteria** | Use business objectives to select the most important criteria. If few candidates remain, evaluate against a long list. |
| **Vendor misrepresents capabilities** | Ask for a vendor **technical specialist** during the sales cycle, not just sales staff. Determine if you can have a healthy partner relationship. |
| **Incorrect solution expectations** | During selection, have the vendor **walk through your actual use cases** — don't rely on demos. |
| **Users reject the solution** | Engage users in selection or early implementation; manage expectations. |

> [!tip] A careful package selection and implementation process finds the optimum balance of **capability, usability, extensibility, and maintainability**. You don't want to pay for features you don't need, nor build a fragile structure of extensions/integrations that breaks with the next vendor release.

---

## 4. Outsourced Projects (Ch 23)

### 4.1 Why Outsource? And the Requirements Cost

- Organizations outsource to access skills they lack, augment internal staff, save money, or accelerate development.
- Supplier could be nearby or **offshore** (or **nearshore** if close by/shared language/culture).
- All outsourced projects involve **distributed teams** in two or more locations.
- The **BA role is even more important** — and harder — than on co-located projects.

### 4.2 Requirements-Related Challenges

- Harder to get developer input on requirements; harder to pass user feedback to developers.
- **Formal contractual definition** of requirements is necessary → contention if interpretation differences surface late.
- Bigger gap between what customers ultimately need and what they get from initial requirements (fewer course-correction opportunities).
- Longer issue resolution due to time zone differences.
- Language and cultural barriers complicate communication.
- **Limited written requirements that work in-house are insufficient** for outsourced projects — users/BAs aren't readily available to clarify.
- Remote developers lack the organizational/business knowledge that in-house developers acquire.

> [!warning] **Cost reality** — Although offshoring arguments cite lower hourly costs, many offshore projects experience a **net increase in cost** due to: more precise requirements effort, additional dev iterations to close gaps from unstated implied/assumed requirements, contractual overhead, team-norming costs, and increased project communications/oversight.

### 4.3 Appropriate Levels of Requirements Detail

- Outsourcing demands **high-quality written requirements** — direct interactions with the dev team will be minimal.
- You send the supplier: **RFP, requirements specification, product acceptance criteria** (Figure 23-1). Both parties review and reach agreement (with negotiation/adjustments) before development begins.
- **Anticipate that the supplier will build exactly what you ask for — no more, no less, sometimes with no questions asked.**
- Implicit/assumed requirements you thought too obvious to write down **won't be implemented.**
- Poorly defined/managed requirements are a common cause of outsourced project failure.
- **RFP needs:** suppliers must know exactly what you're requesting before realistic responses/estimates (Porter-Roth 2002). May need to develop more detailed requirements **earlier** than on in-house projects (Morgan 2009).
- At minimum for the RFP: rich set of **user requirements** and **nonfunctional requirements**.
- After project start: specify all requirements with **more precision** than if in-house, particularly if offshore.
- **If ever inclined to overspecify requirements, outsourced projects are the place to do so.**
- Include deliverables required for process certification/compliance in the RFP.

### 4.4 Augmenting Written Specifications

- **Visual requirements models** augment functional/nonfunctional requirements — create *more* models than for in-house teams to increase communication bandwidth.
- Especially valuable across cultures/native languages — gives developers something to check interpretations against.
- **Ensure developers can understand the models** — unfamiliar models only raise confusion potential.
- Example: the **display-action-response (DAR) model** (Ch 19) was developed specifically to meet an outsourced project's needs for a complex UI (Beatty & Chen 2012).
- **Prototypes** clarify expectations; supplier can create prototypes to demonstrate their interpretation — creating more customer-development interaction points for early course adjustments.
- Watch for **ambiguous terms** (Table 11-2, Ch 11) — e.g., "support" is dangerously vague.
- A **glossary** is valuable when people don't share the acquiring company's tacit knowledge.
- **Planguage** (Gilb 2007, Ch 14) can describe requirements very explicitly for outsourced development.

### 4.5 Acquirer-Supplier Interactions

- Arrange **formal touch points** in the absence of real-time, face-to-face communication.
- Some outsourced projects have the supplier help write functional requirements (Morgan 2009) — increases initial cost but reduces misunderstanding risk.
- Plan time for **multiple review cycles** of the requirements. Use collaboration tools for multi-location peer reviews (Wiegers 2002).
- **Cultural caution:** members of certain cultures find it difficult to offer constructive criticism; authors may take review comments personally (Van Veenendaal 1999). Reviewers may sit politely, saying nothing — courteous but doesn't contribute to early defect discovery.
- **Cautionary tale:** one failed offshore project schedule included a one-week "Hold requirements workshops" task followed immediately by implementation tasks — **forgot to include** documenting, reviewing, and revising the SRS. Slow turnaround on questions derailed the schedule; ended in litigation.
- Peer reviews, prototypes, and **incremental development** provide insight into the supplier's interpretation and permit course corrections.
- Document supplier questions and integrate answers into the requirements (Gilb 2007). Monitor resolution in an **issue-tracking tool** both teams can access (Ch 27).
- Consider delivering **domain/company training** to contractor staff before requirements review.

### 4.6 Cultural and Language Considerations

- Suppliers eager to please may agree to outcomes they can't deliver; may strive to **save face** by not fully accepting responsibility for problems.
- Some developers hesitate to ask for help/clarification; reluctant to say "no" or "I don't understand" → misinterpretations, unresolved issues, unachievable commitments.
- Mitigations: read between the lines; ask **open-ended questions**; establish **ground rules** for interaction.
- Developers whose first language differs may interpret requirements **literally**, missing nuances. They may make unexpected UI design choices.
- Vary by country: date formats, measurement systems, color symbolism, order of given/family names.
- Make intentions clear in **simple language**; avoid colloquialisms, jargon, idioms, pop-culture references.
- **Cautionary tale:** one offshore team translated each requirement into their language, coded it, moved on — the product technically met requirements but fell far short of expectations. Customer brought most work back in-house and effectively **paid twice**.

> [!warning] **Trap** — Don't assume suppliers will interpret ambiguous/incomplete requirements the same way you do. **The burden is on the acquirer** to communicate all necessary information, using frequent conversations. **But the burden is on the supplier** to proactively ask clarifying questions instead of making assumptions.

### 4.7 Change Management

- Establish a mutually acceptable **change control process** at project start, usable by all participants regardless of location.
- Use a **common set of web-based tools** for change requests and issue tracking.
- Change always has a price — using change management to control scope creep is vital in contract development.
- Identify **decision makers** for proposed changes and communication mechanisms.
- Most outsourced work has **contractual agreements** specifying what the dev team must deliver. The contract should specify:
  - Who pays for various kinds of changes (new functionality vs. corrections to original requirements).
  - The process for incorporating changes.
- When requirements and delivery misalign, ensuing arguments are **contractual** in nature — often both parties lose (McConnell 1997).

### 4.8 Acceptance Criteria

- "Begin with the end in mind" (Covey 2004) — define in advance how you'll assess whether the contracted product is acceptable.
- How will you judge whether to make the final payment?
- If acceptance criteria aren't fully satisfied, who is responsible for corrections, and who pays?
- **Include acceptance criteria in the RFP** so the supplier knows up front what to expect.
- **Validate the requirements before giving them to the outsourced team** to help ensure the delivered product is on target. (See Ch 17 for acceptance criteria approaches and review/test methods.)

> [!important] An essential starting point for successful outsourced development is a set of **high-quality, complete, and explicitly clear requirements.** If the requirements you provide are incomplete or misunderstood, failure is probably at least as much your fault as theirs.

---

## 5. Business Process Automation Projects (Ch 24)

### 5.1 Overview

- Organizations replace manual business processes with software to **lower operational costs.** Most corporate IT projects involve some BPA.
- Processes can be automated by building new software, extending existing systems, or buying a COTS package.
- **Case study:** A loan risk profile spreadsheet took 300 inputs from different sources. Analysis revealed assembling the data took the most time; calculations were nearly instantaneous. Phase 1: automatically pull most data into the spreadsheet. Phase 2: automate remaining inputs. Decision: don't replicate calculations (already fast enough).
- This illustrates the typical BPA project: identify a time-consuming, repetitive activity → analyze for bottlenecks → identify efficiencies → requirements and project plans for a partial solution that saves time, reduces costs, and reduces errors.

### 5.2 Modeling Business Processes

- Eliciting requirements to automate processes begins by **modeling those processes.**
- **As-is processes** — how the business currently works.
- **To-be processes** — the envisioned future state.

#### Business Process Acronyms

| Acronym | Meaning | Purpose |
|---|---|---|
| **BPA** | Business Process Analysis | Understanding processes as a basis for improving them (≈ process modeling, IIBA 2009). |
| **BPR** | Business Process Reengineering | Analyzing and redesigning processes for greater efficiency/effectiveness; may target specific areas or overhaul from the ground up (Hammer & Champy 2006). |
| **BPI** | Business Process Improvement | Measuring and looking for incremental improvement (Harrington 1991); uses Six Sigma and lean tools (Schonberger 2008). |
| **BPM** | Business Process Management | Encompasses understanding all enterprise processes, analyzing them, and working with orgs to change them (Harmon 2007; Sharp & McDermott 2008). May combine BPA, BPR, BPI. |
| **BPMN** | Business Process Model and Notation | Graphical notation for modeling business processes (OMG 2011); robust symbol language when basic swimlane syntax doesn't suffice. |

- These are established approaches for understanding business challenges/opportunities. After deciding software is part of the solution, the requirements engineering techniques in this book become valuable.

### 5.3 Using Current Processes to Derive Requirements

General sequence (not always the same; not all steps needed on every project):

1. **Understand business objectives** — link each objective to one or more processes.
2. **Use organization charts** to find affected organizations and potential user classes.
3. **Identify all relevant business processes** involving those user classes.
4. **Document as-is processes** using flow charts, activity diagrams, or swimlane diagrams. Users can quickly read them and point out missing/incorrect steps, roles, or decision logic (Beatty & Chen 2012). Judge how deep to drill.
5. **Analyze as-is processes** to determine biggest improvement opportunities. If not obvious, gather data on execution times. Use the **KPIM** (later in chapter). Ensure you're addressing **true bottlenecks** so accelerating them speeds up the overall process.
6. **Walk through each as-is process flow** with appropriate stakeholders to elicit software requirements supporting each step. Use Ch 7 elicitation techniques. Look for industry standards to set improvement goals.
7. **Trace requirements to process flow steps** — obvious if you're missing requirements for any steps. If steps have no requirements traced, confirm they're not being automated.
8. **Document to-be process flows** — helps business prepare for the new system and identifies gaps the new system might leave. Create use cases for more detail on user interaction. Use to-be flows and use cases for training materials and to identify transition requirements. Helps stakeholders understand what manual activities/automated systems need to be **unplugged**.

> [!example] **When software isn't the solution** — A company's internal website had wrong implementation consultant data >50% of the time, costing enormous cumulative time across 200 people. The problem: no process existed between sales and implementation teams to update the data after project start. The solution: assign a sales contact to manually gather and update implementation contact info. **New software wouldn't have helped.**

### 5.4 Designing Future Processes First

- **Chicken-and-egg problem:** people expect building a new system will drive process improvements, but the way the app is used may not enable desired changes.
- Process changes involve **culture changes and user education** that software can't deliver.
- Users won't embrace a system just because a developer says to.
- **Better approach:** devise new business processes **first**, then assess needed information system changes. Supporting a new process might involve changing multiple systems.
- Thinking about which users will use the system and how → correct user requirements → maximized user adoption.
- **Concurrent development** of new processes and new applications helps ensure they merge nicely.

### 5.5 Modeling Business Performance Metrics — KPIM

- Understand which business performance metrics are most important so development can be prioritized.
- Success metrics may come from the vision and scope document (Ch 5, §1.4). If not, the metrics developed here help complete it.
- **KPIM (Key Performance Indicator Model)** — associates business processes with their important performance metrics. Drawn as flowcharts, swimlane diagrams, or activity diagrams with **KPIs overlaid** on related steps (Figure 24-1).
- Most important processes to automate = those with the most important metrics to maintain or improve.
- Determine a **current baseline** for each metric so you can tell if automation is improving things.
- You may **degrade certain metrics to improve others** (like quality attribute trade-offs in Ch 14, but here favoring one performance metric over another).
- Tracing requirements → process flow steps → KPIs allows prioritization of requirements.
- May need to build functionality to **periodically measure KPIs** and raise a warning flag if one falls out of tolerance.

> [!tip] Business users often think "always best to automate if you can." But all development projects have costs. Business analysis determines **which processes are worth automating and which are not.**

- **Example:** Seilevel uses a COTS sales pipeline tool and a COTS resource allocation tool. A consulting manager manually transfers data weekly (~30 min). Enabling the integration feature is simple, but automating the *decision logic* the manager goes through would require custom development — **more effort than justified.**

### 5.6 Good Practices for BPA Projects (Table 24-1)

| Technique | Chapter |
|---|---|
| Identify user classes with processes that might need automation | Ch 6 |
| Create/extend data models for information handled manually | Ch 13 |
| Create roles & permissions matrix for security previously enforced manually | Ch 9 |
| Identify business rules that must be automated when affected processes are | Ch 9 |
| Create flowcharts, swimlane diagrams, activity diagrams, or use cases (as-is and to-be) | Ch 8, Ch 12 |
| Use DFDs to identify automatable processes; create new DFDs for interactions with existing parts | Ch 12 |
| Adapt business processes to permit use of a COTS solution | Ch 22 |
| Create trace matrices mapping process steps to requirements | Ch 29 |

> [!important] You will likely apply BPA concepts on **almost every information systems project** you work on. Use this chapter's framework to ensure you fully understand the goals of automation and the requirements to support it.

---

## 6. Business Analytics Projects (Ch 25)

### 6.1 Overview

- **Business analytics** (aka business intelligence or reporting) projects turn large, complex data sets into meaningful information for decision-making.
- Some systems **automate decision-making** by interpreting data and taking actions based on predefined algorithms/rules.
- Decisions can be **strategic, operational, or tactical**:
  - *Tactical:* who to promote.
  - *Operational:* which products need different marketing strategies.
  - *Strategic:* which products to target by markets.
- All software systems with an analytics component should enable users to make **decisions that improve organizational performance** in some dimension.
- Many commercial applications implement analytics solutions — the BA may need to perform requirements activities for tool selection/implementation (see Ch 22).
- This chapter is an **introduction**; Brijs (2013) provides an extensive resource.

### 6.2 Analytics Project Characteristics

- On most information systems, reports are a small portion of functionality. On analytics projects, **complex reports and the ability to manipulate their contents constitute the core functionality.**
- Output of analysis is often **embedded in applications** that automate decision-making.
- Multiple layers, all needing requirements: **understanding data required, operations performed on data, formatting/distribution for use** (Figure 25-1). No rigid sequence — users iterate.
- **Descriptive analytics** (past/present) vs. **predictive analytics** (future) — Figure 25-2 shows the spectrum.
- Standard requirements practices (process flows, use cases, user stories) can reveal that someone needs to generate analytics results, and performance requirements describe how quickly, but **none uncovers the complex knowledge required to implement the system.**
- If new to analytics, **pilot a few small projects** to demonstrate value and learn (Grochow 2012).
- Good candidates for **incremental development** — identify the most important/time-critical decisions for the next iteration.
- Stakeholders may struggle to articulate/prioritize business problems; elicitation may need to begin with **education** about new capabilities over traditional reporting (Imhoff 2005).

### 6.3 Requirements Development for Analytics Projects

- First define **business objectives** to establish/prioritize scope.
- If stakeholders request an analytics project, they've already decided on a solution — explore underlying objectives (analytics might not be the right solution).
- Questions to help stakeholders state actual business objectives:
  - Why do you think an analytics solution will help achieve desired business outcomes?
  - What do you want to accomplish by implementing analytics reporting?
  - How do you expect to use analytics to improve business outcomes?
  - How are you hoping to use improved reporting capabilities or prediction results?

#### Decision-Driven Elicitation (Taylor 2013)

1. Describe the **business decisions** that will be made using system outputs.
2. **Link those decisions** to the project's business objectives.
3. **Decompose the decisions** to discover:
   - Questions that need to be answered.
   - Hierarchy of precursor questions feeding the main questions.
   - What role analytics information plays in producing answers.
4. Determine how analytics could be applied to assist in making these decisions.

- User requirements describe how analytics information will be used and what decisions will be made.
- Understanding expected usage modes → specify how information should be distributed and what users need to see → define requirements for the data itself and for the analyses to be performed.

### 6.4 Prioritizing Work Using Decisions

- On most projects, features are prioritized by contribution to business objectives.
- On analytics projects, there aren't discrete "features" — instead, **prioritize the business decisions** the solution will enable, based on contribution to objectives.
  - Example: deciding which products to sell has greater impact on revenue than decisions about sales team vacation time → implement analytics/reports for product decisions first.
- **Decisions should be stated as unambiguously as requirements.**
  - Good example: "The VP of marketing needs to decide each quarter how much marketing budget to allocate to each region based on current and targeted sales by region."
- Understand the underlying stakeholder need, not just the presented solution. If stakeholders request certain data/reports, ask "Why do you need that information?" and "How will the recipient use that report?" Then work backward to decisions and objectives.
- **Decision management techniques** (Taylor 2012) help stakeholders identify decisions they could/should make. A **decision model** maps information (data) and knowledge (policies/regulations constraining decisions) to related decisions, organizing them for prioritization (Taylor 2013).

### 6.5 Defining How Information Will Be Used

- The BA must determine **how smart the system is** — how much decision-making is done by a human vs. automated.
- This distinction drives the type of elicitation questions.

#### Information Usage by People

Three aspects of information delivery:

| Aspect | Questions |
|---|---|
| **Delivery mechanism** | How is information physically made available? What tools can the user employ: email, portals, mobile devices? |
| **Format** | Reports, dashboards, raw data, other? |
| **Flexibility** | To what extent must the user be able to manipulate the information following delivery? |

- Spectrum of delivery:
  - Each user creates personal view (local spreadsheet copy).
  - Distribute central aggregation (emailed spreadsheets with standard dashboard views).
  - Expose data for users to manipulate (portal permitting ad hoc querying).
- Capture in **user requirements** and **report specifications**. Techniques: process flows, use cases, user stories.
- Use decisions to determine how users should receive output, how it should look, and how they can manipulate it.
- **Report tables** (Ch 13) useful; extend with **layers** for complex options (Beatty & Chen 2012).
- **Dashboard reporting** (Ch 13) — multiple charts/reports in a single display.
- **Display-action-response (DAR) model** (Ch 19) — valuable for specifying comprehensive requirements for manipulating data in reports when simple report tables don't suffice (filters, drill-down changes).
- May reveal new **processes** and **security requirements**:
  - Example: president receives weekly P&L → if correct, shares with executive team only → implies access controls.
  - Regional sales VPs see only their region's data; global VP sees all.

#### Information Usage in Systems

- Analytics may be used **directly within software systems** instead of delivered to humans.
- Examples:
  - Retail: customer purchasing history → personalized discounts.
  - Grocery: coupons based on current/prior purchases.
  - Customized ads for website visitors.
  - Call center: offers for a particular caller.
- Information delivery mechanism and format may be specified through **external interface requirements**.
- Still important to understand how information will be used so correct data is transformed and delivered in usable form.

### 6.6 Specifying Data Needs

- Data forms the core of all analytics solutions.
- BAs can define requirements for **data sources, storage, management, and extraction** — may engage data specialists early.
- Analytics projects may involve identifying **new data sources** to analyze.
- Fully understand data requirements so technical teams can design complex infrastructures.

#### Big Data

- **Big data** = large **volume** (much data), high **velocity** (flows rapidly), and/or highly **complex** (diverse) (Franks 2012).
- Think about daily personal data interactions: social media, email, videos, digital images, electronic transactions. A commercial aircraft generates **10 TB during a 30-minute flight** (Scalable Systems 2008).
- Data models from Ch 13 best suited to **relational** data stores. Big data is often **semi-structured or unstructured**.
  - **Unstructured** (voicemails, text messages) — no obvious place to begin looking for information. Example: scanning Internet traffic for "bomb" requires context to distinguish terrorist threat from WWII article or bad play review.
  - **Good news:** most data has accompanying **metadata** (data about data) — semi-structured sources (email, image files, video files) have metadata providing some structure/content info. May be able to create ERDs and data dictionaries for what you *do* know.

#### Data-Based (Not "Database") Requirements

- Similar to other IS projects in terms of questions asked, though nature may differ.
- Most big data is generated by automated systems and represents a **new data source** — more work to determine requirements.
- Derive many data requirements from **decision-management criteria.**
- **Elicitation questions** (Brijs 2013):

**Data sources:**
- What data objects/attributes do you need? From what sources?
- Do you already have each source available? If not, where is the data? Need to develop requirements to populate sources?
- What external or internal systems are providing data?
- How likely are sources to change over time?
- Need for initial migration of historical data?

**Data storage:**
- How much data today?
- Expected growth and over what period?
- What types of data to store?
- How long to store? How securely?

**Data management and governance:**
- Structural characteristics of the data?
- Expected structure/value changes over time?
- What transformations before storage or analysis?
- Transformations to standardize data from disparate systems?
- Conditions for deleting old data? Archival? Destruction?
- Integrity requirements (unauthorized access, loss, corruption)?

**Data extraction:**
- How fast do users expect queries to return results?
- Real-time or batched? If not real-time, at what frequency?

- Ensure data-related requirements **don't constrain developers with unnecessary design.**

### 6.7 Defining Analyses That Transform the Data

- **Analysis** is the computational engine — transforms data and leads to answers (Franks 2012).
- User defines problem → receives data hoped to contain answer → analyzes → decides on solution. Or system analyzes and takes action.
- **Challenge:** the decision maker may not know what he's looking for. He wants to explore, running what-if queries. "He literally doesn't know what he doesn't know."
  - This is why **start by understanding what decisions stakeholders are trying to make.** Even if he doesn't know exactly what he's looking for, he should be able to define the type of problem.
- Defining necessary data analysis involves **big-picture thinking** (Davenport, Harris, Morrison 2010). A BA with creative-thinking skills can help stakeholders explore new ideas.
- **Elicitation questions** (Davenport, Harris, Morrison 2010):
  - What time frame: past, present, or future?
  - If past: what kinds of insights about the past?
  - If present: what do you need to understand about the current situation for immediate actions?
  - If future: what kinds of predictions or decisions?
- These define **functional requirements** specifying analyses the system must perform.
- Some analysis requires **sophisticated algorithms** to process, filter, organize data (Patel & Taylor 2010). Example: retail store targeted video ads using facial recognition + customer attributes + decision logic. Represent with **decision tables or decision trees** (Ch 12).
- Be explicitly clear about **automated decision-making implications.**
  - **Cautionary tale (2013):** false social media report of U.S. president injured in explosion → automated stock-trading systems sold → other systems detected decline and sold → sharp market drop within moments. Hoax discovered quickly; human decisions reversed the drop. Perhaps systems behaved as intended, but maybe decision logic was missing that could have limited impact.
- **What-if scenario analysis** is one of the most valuable aspects — exploring future-state strategic possibilities. Models and algorithms must be specified in requirements; enlist **data experts, statisticians, mathematical modelers** for complex cases.
- Calculations may be defined by **business rules or industry standards** (e.g., gross profit margins by region — specify exactly how calculated). Use the **calculation formula requirement pattern** (Withall 2007): description of value, formula, variables, source of values, response time requirements.

### 6.8 The Evolutionary Nature of Analytics

- Back-and-forth interactions between data, analysis, and usage (Figure 25-1, Franks 2012).
- Occasionally: user receives report, makes decision, done. More commonly: user starts with a question → requests report → extracts data → applies analytics → delivers report → user thinks of **new questions** → requests new reports/analyses → cycle repeats.
- **Key:** start somewhere. Begin with what stakeholders already know they want to learn; plan for questions to evolve.
- Understand how much users expect their needs to evolve — if significant, they may require a solution that is **easily adaptable** with minimal additional development.
- Consider the forms/conditions of data at extraction, analysis, and viewing:
  - Raw data delivered for manual report generation, or software-organized predefined structured format?
  - Set of recurring weekly questions for a year, or new questions daily requiring rapid development of new analysis/presentation?
- Answers tell the dev team whether to make data sets available for user manipulation or have an analytics team generate/format new information.
- **The BA's job:** work with stakeholders to understand their decision processes. Use those decisions to elicit requirements that access necessary data, specify analyses, and define data presentation. Understand expected results, hoped-for decisions, and desired dynamic modification. Look for opportunities to help users envision solutions they hadn't imagined possible.

---

## 7. Embedded & Other Real-Time Systems Projects (Ch 26)

### 7.1 What Makes These Projects Different

- **Embedded systems** — products that use software to control hardware devices: cell phones, TV remotes, kiosks, Internet routers, robot cars, and countless others.
- In this chapter, **system** = a product containing multiple, integrated software and hardware subsystems.
- Software can be embedded in the device (dedicated computer) or reside in a host computer separate from the hardware it controls.
- Components: sensors, controllers, motors, power supplies, integrated circuits, mechanical/electrical/electronic components operating under software control.

#### Hard vs. Soft Real-Time

| Type | Characteristics | Example |
|---|---|---|
| **Hard real-time** | Rigid time constraints; operations must execute within specified deadlines or **bad things happen.** Life-critical/safety-critical. | Air traffic control — missed deadline could result in collision. |
| **Soft real-time** | Time constraints exist, but consequences of missing a deadline are less severe. | ATM — if communication doesn't complete in time, it can retry or terminate; no one dies. |

> [!important] On embedded projects, it's especially important to have a good understanding of requirements **before getting too far into development.** Software is more malleable than hardware — excessive requirements churn that dictates hardware changes is more expensive than comparable volatility on software-only projects. Constraints that both hardware and software engineers must respect: physical sizes, electrical components/connections/voltages, standard communication protocols, operation sequences, etc. **Hardware already chosen imposes constraints on hardware yet to be chosen.**

### 7.2 System Requirements, Architecture, and Allocation

#### The SyRS

- Many teams first create a **System Requirements Specification (SyRS)** (ISO/IEC/IEEE 2011).
- The SyRS describes capabilities of the system as a whole — capabilities that could be provided by **hardware components, software components, and/or humans.**
- Describes all inputs and outputs associated with the system.
- Specifies critical **performance, safety, and other quality requirements** for the product.
- Feeds into preliminary design analysis guiding architectural component choice and capability allocation.
- The SyRS can be a separate deliverable from the SRS, or the SRS can be **embedded within the SyRS** (particularly if most complexity lies within software).

#### Architecture and Allocation

- Requirements analysis of a complex system is **tightly intertwined with the system's architecture.** Requirements thinking and design thinking become more commingled than in other software projects.
- Architecture = top level of design, often depicted with simple box-and-arrow diagrams (though many other notations exist).
- **Three elements of a system's architecture:**
  1. **Components** — software object/module, physical device, or person.
  2. **Externally visible properties** of the components.
  3. **Connections (interfaces)** between components.
- Developed **top-down, iteratively** (Nelsen 1990; Hooks & Farry 2001).
- Lead role: system analyst, requirements engineer, system engineer, or system architect with strong technical background.
- The analyst **partitions** the system into software and hardware subsystems/components accommodating all inputs and producing all outputs.
- Individual system requirements may:
  - Turn directly into software requirements (if software is the correct medium).
  - Be **decomposed** into numerous derived software, hardware, and/or manual requirements (Figure 26-1).
- Deriving software requirements from system requirements can **expand the volume several-fold**, partly because derivation generates interface requirements between components.
- The analyst **allocates** individual requirements to the most appropriate components, iteratively refining partitioning and allocations.
- **Outcome:** a set of requirements for each software, hardware, and human component that will collaborate to provide system services.
- Establish **trace links** between system requirements, derived software/hardware requirements, and architectural components (Ch 29).

> [!warning] **Poor allocation decisions** can result in:
> - Software expected to perform functions easier/cheaper for hardware (or reverse).
> - A person expected to perform functions easier/cheaper for hardware/software (or reverse).
> - Inadequate performance.
> - Inability to easily upgrade or replace components.
>
> Software is easier to change than hardware, but engineers shouldn't use that flexibility as a reason to skimp on hardware design. Allocators must understand capabilities, limitations, costs, and risks of implementing functionality in each medium.

### 7.3 Modeling Real-Time Systems

- Visual modeling is a powerful analysis technique. Most relevant models:

#### State-Transition Diagrams (STDs)

- **State-transition diagrams** or more sophisticated variants (statechart diagrams, UML state machine diagrams) are particularly relevant.
- Most real-time systems can exist in **multiple states** with defined conditions and events permitting transitions.
- **State tables** and **decision tables** can supplement or replace STDs, often revealing errors in the diagrams.
- Bruce Powel Douglass (2001) gives examples of using use cases and other UML models for real-time systems.

#### Context Diagram

- Shows the environment in which the system operates and boundaries between the system and external entities.
- Example (Figure 26-2): treadmill context diagram — large square for the system, showing multiple input/output flows between the system and the Exerciser (person using the treadmill), the manufacturer's website (workout program downloads), and a pulse rate sensor.

#### State-Transition Diagram (Example: Treadmill)

- Figure 26-3: boxes = states, arrows = allowed transitions, labels = conditions/events triggering changes.
- Begins to reveal **user interface controls** needed (Speed, Incline, Start, Pause, Stop).
- "Pressing" a control could be implemented in various ways.

#### Event-Response Table

- **Event-response analysis** provides another way to think about behavior and hence functional requirements (Wiley 2000).
- Events: business events triggering use cases, signal events (sensor input), temporal events (time interval or specific point in time).
- Table 26-1 (partial, for treadmill):

| Event | Treadmill state | Response |
|---|---|---|
| Exerciser presses Incline Up button | Below maximum incline | Increase incline by 0.5 degree |
| Exerciser presses Incline Up button | At maximum incline | Generate "at limit" audio signal |
| Exerciser presses Speed Down button | Above minimum speed | Decrease speed by 0.1 mph |
| Exerciser presses Speed Down button | At minimum speed | Stop treadmill belt |
| Exerciser removes safety key | Running | Stop belt and turn power off |
| Exerciser removes safety key | Idle | Turn power off |
| Exerciser presses Pause button | Running | Stop belt; initiate timer |
| Exerciser presses Pause button | Paused or idle | Generate "error" audio signal |
| Timer for paused condition reaches timeout | Paused | Go to idle state |
| Exerciser presses Start button | Running | Generate "error" audio signal |
| Exerciser presses Start button | Paused | Start belt on current speed setting |
| Exerciser presses Start button | Idle | Start belt at lowest speed |

- Provides detailed requirements fleshing out the high-level STD view.
- **Great aid for conceiving tests.**
- Even a complete event-response table leaves design thinking to be done (e.g., degrees per minute for incline motor, acceleration rate from stopped to set speed). **Safety considerations** will influence these decisions — dangerous if belt starts, accelerates, or stops too abruptly.
- Embedded systems must manage a combination of:
  - **Event-based functions** (as in the table).
  - **Periodic control functions** — executed repeatedly while in a particular state (e.g., monitoring pulse rate every second and adjusting belt speed to maintain a preset pulse rate).

> [!tip] **Drawing models finds missing requirements.** Wiegers once reviewed an SRS with a long table of machine states, functionality, and navigation destinations. Drawing the STD revealed **two missing requirements:** no way to turn the machine off, and no provision for entering an error state while running. This illustrates the value of creating **multiple representations** and verifying them against each other.

#### Architecture Diagram

- Generally part of high-level design. Figure 26-4: identifies major subsystems and data/control interfaces at a high level.
- Richer **architecture description languages** and **UML** also work well (Rozanski & Woods 2005).
- Subsystems can be further elaborated into specific hardware (motors, sensors) and software components.
- A preliminary architecture analysis can reveal and refine functional, interface, and quality requirements not evident from other elicitation activities.

> [!note] Drawing architecture models during requirements analysis is clearly taking a step into design — but it's a **necessary step.** Iterating on architectural partitioning and allocation is how an architect devises the most appropriate solution. Further requirements elicitation is still needed.

- Functional requirements example:
  - `Incline.Angle.Range`: The Exerciser shall be able to increase and decrease the incline angle from 0° through 10°, inclusive, in 0.5-degree increments.
  - `Incline.Angle.Limits`: The treadmill shall stop changing its angle and provide audible feedback when it has reached the minimum or maximum limit of its incline range.
- **Business rules** still apply — e.g., calculating calories burned from weight + workout program (segments of duration, incline angle, belt speed). Virtually all requirements practices in this book apply to embedded/real-time systems.

### 7.4 Prototyping

- Prototyping and **simulation** are powerful techniques for eliciting and validating requirements.
- Because of hardware costs/time (and rebuild costs if errors are found), prototypes test operational concepts and explore requirements/design options.
- Simulations help understand UI displays/controls, network interactions, and hardware-software interfaces (Engblom 2007).
- **Caveat:** the simulation will differ from the real product in numerous respects.

### 7.5 Interfaces

- Interfaces are a **critical aspect** of embedded and real-time systems.
- The SRS should address four classes of external interface requirements (Ch 10): **user, software, hardware, and communications.**
- Partitioning complex systems into subsystems creates numerous **internal interfaces** between components.
- Embedded systems can be incorporated into other embedded systems as part of a larger product (e.g., cell phone integrated into a motor vehicle's communication system) — interface issues become even more complex.
- **Requirements analysis should concentrate on external interface issues**, leaving internal interface specifications for architecture design.
- For simple external interfaces: specify as in SRS template section 5 (Figure 10-2, Ch 10).
- For complex systems: create a **separate interface specification document** (Figure 26-5 template) accommodating both external and internal interfaces.

### 7.6 Timing Requirements

- Timing requirements lie at the **heart** of real-time control systems (Koopman 2010).
- Undesirable outcomes if: signals not received from sensors as scheduled, software can't send control signals when anticipated, or physical devices don't perform actions on time.

#### Three Dimensions of Timing

| Dimension | Definition | Example concern |
|---|---|---|
| **Execution time** | Elapsed time from when a task is initiated to when it completes. | Measured as duration between two specific events bounding the task's execution. |
| **Latency** | Time lag between when a trigger event occurs and when the system begins to respond. | Music recording/production: multiple prerecorded and live audio tracks must be precisely synchronized. |
| **Predictability** | Repeated, consistent timing of a recurring event. | Digitizing an audio waveform at 44,100 cycles/second — sampling frequency must be predictable to avoid constructing a distorted digital representation. |

#### Issues to Explore for Real-Time Task Timing/Scheduling

- **Periodicity** (frequency) of execution and tolerances.
- **Deadlines** and tolerances for each task.
- **Typical and worst-case execution time** for each task.
- **Consequences of missing a deadline.**
- Minimum, average, and maximum **arrival rate of data** in each relevant component state.
- Maximum time before the **first input or output** is expected after a task initiates.
- What to do if data is not received within the maximum time before expected first input (**timeout**).
- The **sequence** in which tasks must run.
- Tasks that must **begin or end** execution prior to other tasks beginning.
- **Task prioritization** — which tasks can interrupt/preempt others, and on what basis.
- Functions that depend on **system mode** (e.g., normal mode vs. firefighter service mode for an elevator).

> [!important] When specifying timing requirements, indicate **constraints and acceptable tolerances.** Understand the distinction between soft and hard real-time demands so you don't specify overly stringent requirements — those lead to over-engineering at excessive cost/effort. Broader tolerances may permit less expensive hardware. As Philip Koopman (2010) points out: *"Real-time performance is seldom about being as fast as absolutely possible. Rather, it is about being just as fast as you need to be, and minimizing overall cost."*

- Specifying timing involves understanding deadlines for time-critical functions and **scheduling both sequential and concurrent functions** within constraints of processor capacity, I/O rates, and network communication rates.
- **Creative example:** one team used a project-scheduling tool to model timing requirements at the **millisecond time scale** rather than days/weeks — unconventional but worked very well.
- Timing/scheduling algorithms may be imposed through requirements as **design constraints**, but more frequently are design choices. Kavi, Akl, & Hurson (2009) offer a valuable overview of scheduling issues.

### 7.7 Quality Attributes for Embedded Systems

- Quality attribute requirements are **especially critical** for embedded/real-time systems — vastly more complex and intertwined than for other software applications.
- Business software generally operates in offices with little environmental variance. Embedded systems may face **temperature extremes, vibration, shock, and other factors** dictating specific quality considerations.
- **Important quality categories:** performance, efficiency, reliability, robustness, safety, security, usability.

#### Physical Quality Attributes (Beyond Software)

- Embedded systems are also subject to quality attributes/constraints applying **only to physical systems**: size, shape, weight, materials, flammability, connectors, durability, cost, noise levels, strength.
- All can dramatically increase the cost and effort needed to validate requirements adequately.
- **Business/political considerations:** avoid materials whose supply might be threatened by conflict/boycott (causing price spikes); avoid materials with environmental impacts. Avoiding optimal materials could lead to trade-offs in performance, weight, cost, or other attributes.
- **Address these requirements early** during elicitation — difficult and expensive to build in desired quality characteristics after hardware design is complete.
- Because quality characteristics profoundly impact a complex product's architecture, perform **attribute prioritization and trade-off analysis before design.**
- Koopman (2010) presents a good discussion of nonfunctional requirements especially important for embedded systems.

#### Performance

- The essence of a real-time system: performance must satisfy timing needs/constraints of the operating environment.
- All **processing deadlines** for specific operations must be in the requirements.
- Performance goes beyond operational response times — includes:
  - **Startup and reset times.**
  - **Power consumption.**
  - **Battery life** and recharge time (e.g., electric automobiles).
  - **Heat dissipation.**
- **Energy management** has multiple dimensions: voltage drops momentarily? High current load during startup? External power lost and device switches to battery backup?
- Unlike software, many components **degrade over time** — requirements for how long a battery maintains a given power profile before replacement?

#### Efficiency

- **Internal quality counterpart** to externally observable performance.
- Focuses on consumption (and remaining availability) of resources: processor capacity, memory, disk space, communication channels, electrical power, network bandwidth.
- Requirements, architecture, and design become **tightly coupled.**
  - Example: if total power demand could exceed available power, can the device cut power to components that don't need it all the time?
- Specify **maximum anticipated consumption** of various resources so designers can provide sufficient **slack** for future growth and unexpected conditions.
- **Concurrent hardware/software design is vital.** If software consumes too much, developers resort to clever tricks. Choosing more capable hardware up front is **much less costly** than fine-tuning software components (Koopman 2010).

#### Reliability

- Embedded/real-time systems often have **stringent reliability and availability requirements.**
- Life-critical systems (medical devices, airplane avionics) offer little room for failure.
  - Example: an artificial cardiac pacemaker must work reliably for years; if it fails or battery dies prematurely, the patient can die.
- **Realistically assess the likelihood and impact of failure** so you don't over-engineer a product whose true reliability requirements aren't as demanding as you might think.
- Increasing reliability and availability **comes at a price** — sometimes you need to pay it; sometimes you do not.

> [!example] **An open-door policy** — A door on a light-rail train car failed to close when the train left the station. Sensors apparently failed to notify the driver. The train traveled at 55 mph with an open door — a scary safety hazard. Developers might have had a reliability/safety requirement that such an event happens no more often than once every 100 million operating hours. You can't run a railway for hundreds of millions of hours before release to test this. Instead, **design systems so the probability of safety-critical failure is sufficiently low** to meet the requirement. But things can still fail — in complex systems, it's usually the **combinations of failures you didn't think of** (corrosion on two switches, in this case) that cause rare problems.

#### Robustness

- **Robustness** = how well the system responds to **unexpected operating conditions.**
- One aspect is **survivability** — often considered military, but has everyday applications.
  - Example: aircraft "black boxes" (actually bright orange) — electronic recording devices designed to survive the horrific trauma of an airplane crash.

---

## 8. Key Takeaways

- **Adapt, don't abandon.** The core requirements practices — elicitation, analysis, specification, validation, management — apply across all project types. What changes is *timing, depth, documentation extent, and emphasis.*
- **Agile ≠ no requirements.** "Requirements for agile projects" are the same in nature; differences are in continuous customer involvement, just-enough documentation, backlog-driven prioritization, and iteration-scoped detail. Embrace change but manage it thoughtfully.
- **Existing systems are both asset and trap.** Enhancement/replacement projects let you pilot new techniques at low risk, but beware gold-plating, scope growth, degraded performance, and user resistance. **Leave the requirements in better shape than you found them.**
- **COTS demands requirements too.** Requirements drive package selection (user requirements level, prioritized, with quality attributes), then configuration, integration, extension, data mapping, and business process adaptation. Strike a balance between desired functionality and out-of-the-box capability.
- **Outsourcing amplifies requirements quality stakes.** Write high-quality, complete, explicitly clear requirements; augment with visual models, prototypes, glossaries, and Planguage. Establish change control and acceptance criteria up front. The acquirer owns communication; the supplier owns asking clarifying questions.
- **BPA is nearly universal.** Model as-is processes, analyze bottlenecks, derive requirements per process step, trace to KPIs, design to-be processes. Sometimes software isn't the solution. Design future processes first, then assess system changes.
- **Analytics projects are decision-driven.** Elicit business decisions → link to objectives → decompose into questions → determine data/analysis/presentation needs. Plan for evolutionary exploration. Distinguish human vs. automated decision-making. Beware automated decision logic gaps (the 2013 stock-drop cautionary tale).
- **Embedded/real-time demands early, rigorous requirements.** Software is malleable; hardware isn't — requirements churn that dictates hardware changes is expensive. Use SyRS, architecture-driven allocation, state-transition diagrams, event-response tables, and rigorous timing/quality specifications. Address physical quality attributes (size, weight, materials, power) early. "Real-time performance is about being just as fast as you need to be, and minimizing overall cost."

---

## 9. Next Steps (from Wiegers)

- **Agile:** If you're a BA new to agile, determine your role, read up on the product owner role (e.g., Pichler 2010), identify practices that fit your org, pilot with a small project or a few practices on your next project, and bring in an experienced coach for 3–4 iterations. Don't be an agile purist for the sake of being a purist.
- **Enhancement/Replacement:** On your next enhancement, try one new requirements technique (e.g., user stories) on a small, low-risk feature. Perform a gap analysis. Identify existing functionality that doesn't need to be retained. Trace requirements to business objectives to combat gold-plating.
- **Packaged Solutions:** For your next COTS evaluation, weight requirements 1–10, rate candidates (1/0.5/0), and calculate scores. Consider a multi-stage evaluation. Map your data dictionary to the COTS data model. Have the vendor walk through your actual use cases during selection.
- **Outsourced:** Audit your current SRS for ambiguous terms (Table 11-2). Create a glossary. Establish a mutually acceptable change control process with web-based tools accessible to both teams. Include acceptance criteria in the RFP. Plan time for multiple review cycles.
- **BPA:** For a manual process you want to automate, model the as-is process, identify the true bottleneck, create a KPIM, and trace requirements to process steps. Ask whether software is actually the solution.
- **Analytics:** For an analytics project, start by eliciting business decisions (not features). Link decisions to objectives. Decompose into questions. Determine data/analysis/presentation needs. Pilot a small project first. Plan for evolutionary exploration.
- **Embedded/Real-Time:** For an embedded system, create a SyRS. Draw a context diagram, state-transition diagram, event-response table, and architecture diagram — verify them against each other to find missing requirements. Specify timing requirements with tolerances. Address physical quality attributes early. Perform attribute prioritization and trade-off analysis before design.

---

## Related Notes

- [[Software Requirements Overview]] — SWEBOK overview; this note deepens the project-types knowledge area.
- [[01_Requirements_Fundamentals]] — Good practices for requirements engineering (Ch 3); foundational to all project types.
- [[05_Documenting_Requirements]] — SRS template (Ch 10); referenced throughout for interface specifications and requirement quality.
- [[06_Requirements_Modeling]] — Visual models (Ch 12–13); especially relevant for embedded (state-transition, event-response) and BPA (swimlane, DFD).
- [[07_Quality_and_Prototyping]] — Quality attributes and prototyping (Ch 14–15); critical for COTS selection and embedded systems.
- [[08_Prioritization_Validation_and_Reuse]] — Prioritization and acceptance criteria (Ch 16–19); feed directly into agile backlogs and outsourced acceptance criteria.
- [[10_Requirements_Management]] — Baseline, change control, traceability (Ch 27–29); essential for outsourced contracts and embedded system requirements trace links.
- Chapters 20–26 form **Part III** of Wiegers — "Requirements for specific project classes" — applying the general practices of Parts I–II to distinct project contexts.
