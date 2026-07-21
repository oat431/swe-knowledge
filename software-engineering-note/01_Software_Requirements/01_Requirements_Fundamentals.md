---
title: "Software Requirements Fundamentals (Ch 1-4)"
tags:
  - requirements
  - fundamentals
  - business-analyst
  - software-requirements
source: "Wiegers & Beatty, *Software Requirements*, 3rd Ed., Ch 1-4 (Microsoft Press, 2013)"
created: 2026-07-21
updated: 2026-07-21
aliases:
  - Requirements Fundamentals
  - SW Req Ch1-4
---

# Software Requirements Fundamentals

> *"The hardest single part of building a software system is deciding precisely what to build."*
> — Frederick Brooks, "No Silver Bullet" (1987)

This note distills Chapters 1-4 of Wiegers & Beatty, covering the **essential software requirement**, the **customer perspective**, **good practices** for requirements engineering, and the **business analyst** role.

---

## 1 | The Essential Software Requirement

### 1.1 Why Requirements Matter

- Requirements are the **foundation** for both software development and project management.
- Errors introduced during requirements activities account for **40–50%** of all defects found in a software product (Davis 2005).
- Requirements errors can account for **70–85%** of rework cost (Leffingwell 1997).
- Rework often consumes **30–50%** of total development cost (Shull et al. 2002).
- **Cost amplification:** A requirement defect costing $1 to fix during requirements costs ~$100+ to fix in production (Boehm 1981; Grady 1999). One consulting client measured a **21× amplification** ($200 to fix via inspection vs. $4,200 to fix after user report).

> **Trap** — Don't assume all project stakeholders share a common notion of what requirements are. Establish definitions up front.

### 1.2 Defining "Requirement"

Several definitions exist:

| Source | Definition |
|--------|-----------|
| Brian Lawrence (1997) | "Anything that drives design choices." |
| Generic | "A property that a product must have to provide value to a stakeholder." |
| **Sommerville & Sawyer (1997)** — *preferred* | "A specification of what should be implemented. They are descriptions of how the system should behave, or of a system property or attribute. They may be a constraint on the development process of the system." |

**Key insight:** Requirements include a **time dimension** — present tense (current system), near-term (high priority), mid-term (medium priority), or hypothetical (low priority). Even deferred or discarded items are still requirements.

### 1.3 Levels & Types of Requirements

Software requirements exist at **three levels**, plus an assortment of nonfunctional requirements:

```
┌─────────────────────────────────────────────────────────┐
│  BUSINESS REQUIREMENTS  (Why?)                          │
│  Business objectives, vision & scope                    │
│  Source: Sponsor, customer, marketing, product visionary│
│  Stored in: Vision & Scope Document                     │
└───────────────────────┬─────────────────────────────────┘
                        │ aligns with
┌───────────────────────▼─────────────────────────────────┐
│  USER REQUIREMENTS  (What for?)                          │
│  Goals/tasks users must accomplish; product attributes   │
│  Source: Actual user representatives                     │
│  Represented as: Use cases, user stories, event-response │
│  Stored in: User Requirements Document                   │
└───────────────────────┬─────────────────────────────────┘
                        │ derives
┌───────────────────────▼─────────────────────────────────┐
│  FUNCTIONAL REQUIREMENTS  (What does it do?)             │
│  Behaviors the system exhibits under specific conditions │
│  Written as: "shall" statements                          │
│  Stored in: Software Requirements Specification (SRS)    │
└─────────────────────────────────────────────────────────┘
        +
┌─────────────────────────────────────────────────────────┐
│  NONFUNCTIONAL REQUIREMENTS                              │
│  Quality attributes, constraints, external interfaces,  │
│  compliance/regulatory, localization                     │
└─────────────────────────────────────────────────────────┘
```

#### Types of Requirements Information (Table 1-1)

| Term | Definition |
|------|-----------|
| **Business requirement** | A high-level business objective of the organization that builds a product or of a customer who procures it. |
| **Business rule** | A policy, guideline, standard, or regulation that defines or constrains some aspect of the business. *Not* a software requirement itself, but the origin of several types. |
| **Constraint** | A restriction imposed on the choices available to the developer for design and construction. |
| **External interface requirement** | A description of a connection between a software system and a user, another software system, or a hardware device. |
| **Feature** | One or more logically related system capabilities that provide value to a user, described by a set of functional requirements. |
| **Functional requirement** | A description of a behavior that a system will exhibit under specific conditions. |
| **Nonfunctional requirement** | A description of a property or characteristic that a system must exhibit, or a constraint it must respect. |
| **Quality attribute** | A kind of nonfunctional requirement describing a service or performance characteristic (performance, safety, availability, portability, etc.). |
| **System requirement** | A top-level requirement for a product containing multiple subsystems (all software, or software + hardware). |
| **User requirement** | A goal or task that specific classes of users must be able to perform with a system, or a desired product attribute. |

> **Note on "nonfunctional":** The term is imperfect (says what they are *not*), but retained for lack of a better inclusive alternative. Nonfunctional ≠ just quality attributes — it also includes constraints, external interfaces, compliance, and localization.

### 1.4 The Three Levels Illustrated

**Example — Text editor with multilanguage spell checker:**

| Level | Example |
|-------|---------|
| Business requirement | "Increase non-US sales by 25% within 6 months." |
| Feature | Multilanguage spelling checker |
| User requirements | "Select language for spelling checker," "Find spelling errors," "Add a word to a dictionary" |
| Functional requirements | Highlight misspelled words, autocorrect, display suggested replacements, globally replace |
| Nonfunctional (usability) | How the software is localized for specific languages and character sets |

**Stakeholder participation by level:**
- **Business requirements** ← Managers, marketing, funding sponsor, product visionary
- **User requirements** ← User representatives (collaborating with BA)
- **Functional requirements** ← BA derives from user requirements; developers implement; testers verify

### 1.5 Product vs. Project Requirements

- **Product requirements** — Describe properties of the software system to be built. Housed in the SRS. Should *not* include design/implementation details (except known constraints), project plans, or test plans.
- **Project requirements** — Non-software deliverables necessary for project success:

**Examples of project requirements:**
- Physical resources (workstations, testing labs, videoconferencing)
- Staff training needs
- User documentation (training materials, tutorials, reference manuals, release notes)
- Support documentation (help desk, field maintenance)
- Infrastructure changes
- Release, installation, configuration, and testing procedures
- Transition requirements (data migration, conversion, security setup, production cutover)
- Product certification and compliance
- Revised policies, processes, organizational structures
- Sourcing, acquisition, licensing of third-party components
- Beta testing, manufacturing, packaging, marketing, distribution
- Customer service-level agreements
- IP legal protection (patents, trademarks, copyrights)

> **Solution scope** = product requirements (BA's responsibility) + project requirements (PM's responsibility).

### 1.6 Requirements Development & Management

The discipline of **requirements engineering** splits into two halves:

```
┌──────────────────────────────────────────────┐
│          REQUIREMENTS ENGINEERING            │
├──────────────────┬───────────────────────────┤
│   DEVELOPMENT    │       MANAGEMENT          │
│ (Part II)        │   (Part IV)               │
├──────────────────┼───────────────────────────┤
│ • Elicitation    │ • Define baselines        │
│ • Analysis       │ • Change impact analysis  │
│ • Specification  │ • Incorporate approved    │
│ • Validation     │   changes controlled      │
│                  │ • Keep plans current      │
│                  │ • Negotiate new commitments│
│                  │ • Define relationships &  │
│                  │   dependencies            │
│                  │ • Trace reqs → design →   │
│                  │   code → tests            │
│                  │ • Track status & changes  │
└──────────────────┴───────────────────────────┘
```

#### Requirements Development — 4 Subdisciplines

| Subdiscipline | Key Actions |
|---------------|-------------|
| **Elicitation** | Discover requirements via interviews, workshops, document analysis, prototyping. Identify user classes & stakeholders; understand user tasks/goals; learn the operating environment; work with user reps to understand functionality needs and quality expectations. |
| **Analysis** | Distinguish task goals from functional requirements, quality expectations, business rules, suggested solutions. Decompose high-level reqs; derive functional reqs; understand quality attribute importance; allocate to components; negotiate priorities; identify gaps. |
| **Specification** | Translate collected user needs into written requirements and diagrams suitable for comprehension, review, and use by intended audiences. |
| **Validation** | Review documented requirements to correct problems before development accepts them. Develop acceptance tests and criteria. |

> **Key principle:** Iteration is essential. Plan for multiple cycles of exploring, refining, and confirming. The goal is *not perfect requirements* but a **shared understanding good enough to proceed at an acceptable level of risk**.

**Two elicitation strategies:**
- **Usage-centric** — Emphasize understanding user goals to derive functionality (*recommended*)
- **Product-centric** — Focus on features expected to lead to market/business success (risk: building unused features)

### 1.7 Common Requirements Problems

| Problem | Description |
|---------|-------------|
| **Insufficient user involvement** | Late-breaking requirements → rework and delay. User surrogates don't always understand real needs. |
| **Inaccurate planning** | Vague requirements → overly optimistic estimates → overruns. Top causes of poor estimation: frequent changes, missing requirements, insufficient user communication, poor specification, insufficient analysis. |
| **Creeping user requirements** | Scope creep. Mitigate with clear business objectives, vision, scope, success criteria; evaluate changes against reference; build schedule buffers; agile adjusts scope per iteration. |
| **Ambiguous requirements** | Multiple readers interpret differently. Ferret out via multi-perspective inspection, collaborative workshops, writing tests, prototypes. |
| **Gold plating** | Developer adds unrequested features ("users will love it"). Mitigate: present creative ideas for stakeholder consideration; trace each feature to origin and business justification. |
| **Overlooked stakeholders** | Missed user classes → unmet needs. Cast a wide net early; include maintenance, support, data conversion, regulatory stakeholders. |

### 1.8 Benefits of a High-Quality Requirements Process

- Fewer defects in requirements and delivered product
- Reduced development rework
- Faster development and delivery
- Fewer unnecessary and unused features
- Lower enhancement costs
- Fewer miscommunications
- Reduced scope creep
- Reduced project chaos
- Higher customer and team member satisfaction
- Products that do what they're supposed to do

> Investing in good requirements virtually always returns more than it costs.

---

## 2 | Requirements from the Customer's Perspective

### 2.1 The Expectation Gap

Without adequate customer involvement, an **expectation gap** opens between what customers need and what developers deliver. Frequent **contact points** (interviews, reviews, prototype evaluations, agile feedback) progressively shrink this gap.

```
  Customer Need ───────────────────────────────
        ╲    ╲    ╲    ╲    ╲    ← frequent contact points
         ╲    ╲    ╲    ╲    ╲     shrink the gap
  Developer Delivers ─────────────────────────
              ↑ smaller gap with frequent contact
```

### 2.2 Stakeholders, Customers, and Users

- **Stakeholder** — A person, group, or organization actively involved in a project, affected by its process or outcome, or able to influence it. Internal or external.
- **Customer** — A subset of stakeholders; an individual or organization that derives direct or indirect benefit from a product. Customers *request, pay for, select, specify, use, or receive output* from the product.
- **User** (end user) — A subset of customers who actually use the product, either **directly** (hands-on) or **indirectly** (receive outputs without touching it).

> **Trap** — Customers who provide business requirements sometimes purport to speak for users. They are often too far removed to provide accurate user requirements. Business requirements ← accountable sponsor; User requirements ← people who press the keys / touch the screen / receive outputs.

### 2.3 Requirements Bill of Rights for Software Customers

| # | Right |
|---|-------|
| 1 | Expect BAs to speak your language. |
| 2 | Expect BAs to learn about your business and your objectives. |
| 3 | Expect BAs to record requirements in an appropriate form. |
| 4 | Receive explanations of requirements practices and deliverables. |
| 5 | Change your requirements. |
| 6 | Expect an environment of mutual respect. |
| 7 | Hear ideas and alternatives for your requirements and their solution. |
| 8 | Describe characteristics that will make the product easy to use. |
| 9 | Hear about ways to adjust requirements to accelerate development through reuse. |
| 10 | Receive a system that meets your functional needs and quality expectations. |

### 2.4 Requirements Bill of Responsibilities for Software Customers

| # | Responsibility |
|---|----------------|
| 1 | Educate BAs and developers about your business. |
| 2 | Dedicate the time to provide and clarify requirements. |
| 3 | Be specific and precise when providing input. |
| 4 | Make timely decisions about requirements when asked. |
| 5 | Respect a developer's assessment of cost and feasibility. |
| 6 | Set realistic requirement priorities in collaboration with developers. |
| 7 | Review requirements and evaluate prototypes. |
| 8 | Establish acceptance criteria. |
| 9 | Promptly communicate changes to the requirements. |
| 10 | Respect the requirements development process. |

> **Trap** — Don't assume participants instinctively know how to collaborate on requirements. Take time to discuss the working approach and write it down.

### 2.5 Reaching Agreement — Sign-off & Baselines

**Sign-off pitfalls:**
- Customer who views it as meaningless ritual ("I signed because otherwise they wouldn't start coding")
- Developer who weaponizes it to freeze requirements ("You signed off, so that's what we're building")

**Better approach — the requirements baseline:**

A **requirements baseline** is a reviewed and agreed-upon set of requirements serving as the basis for further development. The implicit agreement:

> *"I agree that this set of requirements represents our best understanding for the next portion of this project and that the solution described will meet our needs as we understand them today. I agree to make future changes through the project's defined change process. I realize that changes might require renegotiating cost, resource, and schedule commitments."*

**Agile adaptation:** No formal sign-off; agreement reached per-iteration in planning sessions. Stories for an iteration are frozen; new requests go to the backlog. The ultimate "sign-off" is acceptance of working, tested software from the iteration. Sign-off can serve as a lightweight "We are Here" ceremony — a reference point for change.

### 2.6 Decision Makers & Decision Rules

Identify requirements decision makers **early**. A small group representing management, customers, BA, development, and marketing works best. Select a **decision rule** (Gottesdiener 2001):

- Decision leader decides (with/without discussion)
- Majority vote
- Unanimous vote
- Consensus (everyone can live with it and commits to supporting it)
- Delegation to an individual
- Group decides, but one person has veto authority

> No single rule works in every situation — establish guidelines before the first significant decision.

---

## 3 | Good Practices for Requirements Engineering

### 3.1 Process Framework

Requirements development is **iterative and interwoven**, not linear:

```
Elicitation ⇄ Analysis ⇄ Specification ⇄ Validation
     ↑__________________________________|
              (iterate throughout)
```

**Life cycle impact on requirements timing:**

| Life Cycle | Requirements Effort Distribution |
|------------|----------------------------------|
| **Waterfall** | Heavy front-loading; one major requirements phase |
| **Iterative (RUP)** | Requirements every iteration; heavier in first |
| **Agile/Incremental** | Frequent but small efforts; just-in-time detail per sprint |

### 3.2 Good Practices by Category (Table 3-1)

> 50+ practices grouped into 7 categories. Select, apply, and adapt thoughtfully — no practice suits every situation.

#### Elicitation
- Define vision and scope
- Identify user classes
- Select product champions
- Conduct focus groups
- Identify user requirements
- Identify system events and responses
- Hold elicitation interviews
- Hold facilitated elicitation workshops (JAD)
- Observe users performing their jobs
- Distribute questionnaires
- Perform document analysis
- Examine problem reports
- Reuse existing requirements

#### Analysis
- Model the application environment (context diagram, ecosystem map)
- Create prototypes (UI and technical)
- Analyze feasibility
- Prioritize requirements
- Create a data dictionary
- Model the requirements (DFD, ERD, state-transition, dialog maps, decision trees)
- Analyze interfaces
- Allocate requirements to subsystems

#### Specification
- Adopt requirement document templates
- Identify requirement origins (traceability)
- Uniquely label each requirement
- Record business rules (enterprise-level asset, separate from project)
- Specify nonfunctional requirements

#### Validation
- Review the requirements (peer review / inspection)
- Test the requirements
- Define acceptance criteria
- Simulate the requirements

#### Requirements Management
- Establish a change control process (CCB)
- Perform change impact analysis
- Establish baselines and control versions
- Maintain change history
- Track requirements status
- Track requirements issues
- Maintain a requirements traceability matrix
- Use a requirements management tool

#### Knowledge
- Train business analysts
- Educate stakeholders about requirements
- Educate developers about the application domain
- Define a requirements engineering process
- Create a glossary

#### Project Management
- Select an appropriate life cycle
- Plan requirements approach
- Estimate requirements effort
- Base plans on requirements
- Identify requirements decision makers
- Renegotiate commitments when requirements change
- Manage requirements risks
- Track requirements effort
- Review past lessons learned

### 3.3 Getting Started — Value vs. Difficulty (Table 3-2)

| | High Value | Medium Value | Low Value |
|---|-----------|-------------|-----------|
| **High Ease** | Define RE process; Base plans on reqs; Identify user classes; Hold interviews; Model environment; Identify origins; Define vision & scope; Establish baselines; Change control process; Review reqs; Allocate to subsystems; Record business rules | Train BAs; Plan reqs approach; Identify user reqs; Identify decision makers; Establish versions; Change impact analysis; Select life cycle; Manage risks; Review lessons; Track effort | Distribute questionnaires |
| **Medium Ease** | Maintain traceability matrix; Facilitated workshops; Estimate effort; Acceptance criteria; Model reqs; Analyze interfaces | Educate stakeholders; Focus groups; Prototypes; Analyze feasibility; Prioritize reqs; Use RM tool; Track status; Track issues; Identify system events | Examine problem reports |
| **Low Ease** | | Educate developers about domain; Adopt templates; Specify nonfunctional reqs | Create data dictionary; Observe users; Test reqs; Document analysis; Reuse reqs; Uniquely label; Create glossary; Maintain change history; Simulate reqs |

> Don't try all at once. Begin with high-impact, easy-to-implement practices. Treat the rest as items for your requirements tool kit.

---

## 4 | The Business Analyst

### 4.1 The BA Role

> A **business analyst** enables change in an organizational context by defining needs and recommending solutions that deliver value to stakeholders.

- The BA is the **principal interpreter** through which requirements flow between the customer community and the development team.
- **BA is a project role, not necessarily a job title.** Synonyms: requirements analyst, systems analyst, requirements engineer, requirements manager, application analyst, business systems analyst, IT business analyst, analyst.
- May be performed by dedicated specialists or by team members wearing multiple hats (PM, product manager, product owner, SME, developer, even user).
- When someone with another role also serves as BA, they are doing **two distinct jobs** requiring different skill sets.

> **Trap** — Don't assume any talented developer or knowledgeable user can automatically be an effective BA without training, resources, and coaching.

**Impact of BA experience:** Experienced analysts produce specs with fewer defects (inspected 2× faster than novice work). In COCOMO II, analyst capability can reduce project effort by **one-third** compared to inexperienced analysts (Boehm et al. 2000).

### 4.2 BA Tasks

| Task | Description |
|------|-------------|
| **Define business requirements** | Help sponsor/product manager express vision and scope clearly; suggest templates. |
| **Plan the requirements approach** | Develop plans to elicit, analyze, document, validate, and manage requirements; align with project plans. |
| **Identify stakeholders & user classes** | Select representatives; enlist participation; negotiate responsibilities. |
| **Elicit requirements** | Use varied techniques to help users articulate needed capabilities. |
| **Analyze requirements** | Find derived and implicit requirements; use models to find gaps/conflicts; determine detail level. |
| **Document requirements** | Write well-organized specs using standard templates. |
| **Communicate requirements** | Determine when visual models, tables, equations, or prototypes add value beyond text. Ongoing collaboration, not "toss over the wall." |
| **Lead requirements validation** | Ensure requirement quality; central participant in reviews; review derived designs and tests. |
| **Facilitate prioritization** | Broker collaboration/negotiation among stakeholders and developers. |
| **Manage requirements** | Help create/execute requirements management plan; track status; manage baseline changes; collect traceability. |

### 4.3 Essential BA Skills

| Skill | Why It Matters |
|-------|----------------|
| **Listening** | Active listening: eliminate distractions, maintain eye contact, restate key points, read between the lines. |
| **Interviewing & questioning** | Surface essential info; probe for exceptions and error conditions, not just normal flow. |
| **Thinking on your feet** | Spot contradictions, vagueness, assumptions in real time; ask the unscripted follow-up. |
| **Analytical** | Move between high and low abstraction; generalize from specific needs; separate "wants" from true needs; distinguish solutions from requirements. |
| **Systems thinking** | See the big picture; check requirements against the whole enterprise for inconsistencies and impacts. |
| **Learning** | Learn new domains quickly; translate knowledge into practice; be honest about what you don't know. |
| **Facilitation** | Lead groups toward success; essential for collaborative definition, prioritization, conflict resolution. |
| **Leadership** | Influence stakeholders toward common goals; negotiate; foster trust. |
| **Observational** | Detect significant passing comments; spot subtleties users don't mention. |
| **Communication** | Write for multiple audiences (customers validating, developers implementing); speak clearly; adapt to terminology and dialect. |
| **Organizational** | Structure vast jumbled information into a coherent whole; set up information architecture. |
| **Modeling** | Flowcharts, structured analysis (DFD, ERD), UML; know when to select each model and educate others. |
| **Interpersonal** | Get people with competing interests to work together; comfortable across job functions and levels. |
| **Creativity** | Not merely a scribe — invent potential requirements; conceive innovative capabilities; find ways to satisfy needs users didn't know they had. Avoid gold-plating. |

### 4.4 Essential BA Knowledge

- Contemporary requirements engineering practices and how to apply across life cycles
- Project management, development life cycles, risk management, quality engineering
- Product management concepts (commercial setting)
- Architecture and operating environment basics (for technical conversations)
- **Business, industry, and organizational knowledge** — minimizes miscommunication; detects unstated assumptions; suggests process improvements and valuable functionality

### 4.5 The Making of a Business Analyst

Great BAs are **grown from diverse backgrounds**. All should identify their gaps and actively fill them. Mentoring/coaching (apprenticeship) is valuable for new BAs.

| Background | Strengths | Risks/Gaps |
|-----------|-----------|------------|
| **Former user** | Knows business & environment; trusted by colleagues; speaks user's language | Little software engineering knowledge; may not solicit current user input; stuck in "as-is" thinking; UI-centric |
| **Former developer/tester** | Technical understanding; can work with code | May lack patience with users; technical jargon; needs business domain knowledge and soft skills training. Testers have useful analytical/exception-thinking mindset. |
| **Former/concurrent PM** | Strong communication, negotiation, facilitation, organizational, writing skills | Needs to learn RE practices; must shift focus from timelines/budget to understanding business needs and analysis/modeling skills |
| **Subject matter expert (SME)** | Can judge requirement reasonableness; knows system impacts | May specify to own preferences; blinders on creativity; difficulty imagining "to-be" system. Better paired with a BA from dev team. |

> **Key lesson:** The BA helps stakeholders find the **difference between what they say they want and what they really need.** A skilled BA digs below presented solutions to understand the user's true objectives — there are almost always multiple ways to solve the problem.

---

## Key Takeaways

1. **Requirements are the foundation** — 40-50% of defects originate here; fixing late costs 100×+ more.
2. **Three levels** — Business (why) → User (what for) → Functional (what does it do), plus nonfunctional requirements.
3. **Product ≠ Project requirements** — SRS holds product requirements; project requirements live in the PM plan.
4. **Requirements engineering** = Development (elicit, analyze, specify, validate) + Management (baseline, change control, trace, track).
5. **Iterate** — Perfect requirements are impossible; aim for "good enough to proceed at acceptable risk."
6. **Customer partnership is essential** — Frequent contact points shrink the expectation gap. Use the Bill of Rights & Responsibilities to set expectations.
7. **Baseline, don't freeze** — Sign-off establishes a reference point, not a weapon. Changes flow through a defined process.
8. **50+ good practices** exist across 7 categories — start with high-value, easy-to-implement ones; build a tool kit.
9. **The BA is central** — Not just a scribe but an analyst, facilitator, leader, and creative problem-solver. Experience matters enormously.
10. **BAs come from diverse backgrounds** — each with strengths and gaps; mentoring and training are essential.

---

## Related Notes

- [[02_Business_Requirements_Vision_Scope]] — Ch 5: Establishing business requirements
- [[03_User_Requirements_Use_Cases]] — Ch 6-8: Finding the voice of the user, elicitation, understanding user requirements
- [[04_Documenting_Requirements_SRS]] — Ch 9-10: Business rules, documenting requirements
- [[05_Writing_Excellent_Requirements]] — Ch 11: Quality characteristics of requirement statements
- [[06_Requirements_Modeling]] — Ch 12-13: Analysis models, data requirements
- [[07_Nonfunctional_Requirements]] — Ch 14: Beyond functionality — quality attributes
- [[08_Requirements_Validation]] — Ch 15-17: Prototyping, prioritization, validation
- [[09_Requirements_Management]] — Ch 27-29: Management practices, change control, traceability

---

*Source: Karl Wiegers & Joy Beatty, "Software Requirements," 3rd Edition (Microsoft Press, 2013), Chapters 1-4.*
