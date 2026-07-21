---
tags:
  - requirements
  - modeling
  - dfd
  - uml
  - data-dictionary
  - software-requirements
source: Wiegers Ch 12-13
chapter: "12-13"
title: Requirements Modeling (DFD, Swimlane, STD, Dialog Maps, Decision Tables, ERD, Data Dictionary, Reports)
created: 2026-07-21
---

# Requirements Modeling

> **Core idea.** No single representation—textual or visual—provides a complete understanding of requirements. A combination of natural-language functional requirements and multiple visual analysis models, at different levels of abstraction, is needed to paint a full picture of the intended system. *Source: Wiegers, Ch 12–13.*

---

## 1. Why Model Requirements?

- **Bridges language/vocabulary barriers** among PMs, BAs, developers, testers, customers.
- **Reveals errors text hides.** By the time you reach the 15th textual requirement, you have forgotten the first few; visual models expose missing, extraneous, and inconsistent requirements.
- **Multiple thought processes surface different defects.** Different notations and authors expose inconsistencies, ambiguities, assumptions, and omissions invisible from any single view.
- **Models augment, not replace, the SRS.** Experience shows analysis models should *augment*—not replace—a requirements specification written in natural language. Detail and precision of written requirements remain essential.
- **Analysis vs. design intent.** Same notation, different intent. *Analysis models* represent the problem domain / conceptual system; *design models* show the implementation (database, classes, modules). Always label which you are drawing.

> **Trap — "Our system is too complex / we have no time to model."** A model is *simpler* than the system you are modeling. If you cannot handle the complexity of the model, you cannot handle the complexity of the system. Creating most models doesn't take significantly more time than writing/analyzing textual requirements; errors caught pre-build pay back the investment many times over.

---

## 2. Catalog of Requirements Models

| Model | Best For | Level of Abstraction |
|---|---|---|
| **Data Flow Diagram (DFD)** | Data movement, transformations, transaction flows | High → Low (layered) |
| **Swimlane (cross-functional) diagram** | Business processes, role/system interactions | Mid |
| **State-Transition Diagram (STD) / State Table** | Object/system life cycles, status changes | High |
| **Dialog Map** | UI navigation architecture | High |
| **Decision Table / Decision Tree** | Complex branching logic, combinations of conditions | Mid |
| **Event-Response Table** | Real-time & event-driven behavior, scoping | Mid |
| **Entity-Relationship Diagram (ERD)** | Data relationships (logical) | High |
| **Class Diagram (UML)** | Object classes, attributes, multiplicity | Mid |
| **Data Dictionary** | Detailed data element definitions | Low |
| **CRUD Matrix** | Cross-checking data vs. use cases/events | Mid |
| **Feature Tree** (Ch 5) | Feature hierarchy / scope | High |
| **Use Case Diagram** (Ch 8) | Actor ↔ use case relationships | High |
| **Activity Diagram (UML)** (Ch 8) | Use case flow, swimlane-style | Mid |

> **Selectivity rule.** Rarely does a team need *every* model for *every* part of the system. Focus modeling effort on the **most complex, riskiest, most safety/security/mission-critical** portions. Use models *together*—they complement each other and cross-check completeness.

---

## 3. From Voice of the Customer → Model Elements

| Customer's word type | Examples | Maps to model component |
|---|---|---|
| **Noun** | people, orgs, systems, data | External entities / data stores / data flows (DFD); actors (use case); entities & attributes (ERD); lanes (swimlane); objects-with-states (STD) |
| **Verb** | actions, events | Processes (DFD); process steps (swimlane); use cases; relationships (ERD); transitions (STD); activities (activity diagram); events (event-response) |
| **Conditional** | if/then statements | Decisions (decision tree/table/activity diagram); branching (swimlane/activity) |

---

## 4. Data Flow Diagram (DFD)

The **basic tool of structured analysis** (DeMarco 1979). A DFD identifies:

- **Processes** (transformations) → circles ("bubbles")
- **Data stores** → pairs of parallel horizontal lines
- **External entities** (terminators) → rectangles
- **Data/material flows** → arrows

### Layering
- **Context diagram** = highest level; entire system as a single bubble, with external entities and flows. (See Fig 5-6, Ch 5.)
- **Level 0 DFD** partitions the system into major processes (see Fig 12-1).
- Each level-0 process can be expanded into a **child DFD**; continue until processes are *primitive* (clearly expressible as narrative text / pseudocode / swimlane / activity diagram).
- Each child DFD must be **balanced** with its parent: all input/output flows match. Complex data structures may be split into constituent elements on lower levels (per data dictionary).

### Drawing conventions (Yourdon-DeMarco)
- Processes communicate **through data stores**, not via direct process-to-process flows.
- Data cannot flow directly store↔store or entity↔store—it must pass through a process bubble.
- **Do not imply processing sequence** in a DFD.
- Name each process as **verb + object** ("generate reports"); names meaningful to customers.
- Number processes uniquely & hierarchically: level 0 → 1, 2, 3; child of 3 → 3.1, 3.2 …
- Cap at **8–10 processes per diagram**; group into a higher-level bubble if more.
- Bubbles with only incoming or only outgoing flows are suspect—normal processing requires both input and output.

### Strengths
- Big-picture view of how data moves through a system—use cases/swimlanes can't show full data life cycle.
- Multiple data pieces pulled together and transformed by a process (e.g., cart + shipping + billing → order object) are hard to show elsewhere.
- Great for **whiteboard elicitation** with customers; easy to scribble while discussing business operations.
- Useful for **identifying missing data requirements**—flows/stores should also appear in ERD and data dictionary.

### Limitations
- Not a sole modeling technique. *How* data is transformed is better shown in use cases / swimlanes.
- Doesn't show processing sequence.

---

## 5. Swimlane Diagram (Cross-Functional)

A **flowchart variant** subdivided into *lanes* (roles, departments, or systems). Most commonly used for business processes, workflows, and system/user interactions. Similar to UML activity diagrams. Easiest model for non-technical stakeholders to understand.

### Elements
- **Process steps** → rectangles
- **Transitions** → arrows between rectangles
- **Decisions** → diamonds with labeled branches
- **Swimlanes** → horizontal or vertical dividers showing *who/what* executes each step

### Use
- Shows what happens inside DFD process bubbles; ties functional requirements to user tasks.
- Excellent **starting point for elicitation** conversations.
- Can reveal requirements by walking each box: *"What functionality must the system have to support this step? What data is required?"*

### Cross-model check
A complete business process may extend outside the software scope. Example: the *Receiving* department appears in the CTS swimlane but not in the context diagram or DFD (Receiving never interacts with CTS directly). Reviewing the ecosystem map surfaced this. Review data inputs/outputs to each DFD bubble to ensure both models consume/produce the same data.

---

## 6. State-Transition Diagram (STD) & State Table

Software systems combine functional behavior, data manipulation, and **state changes**. Real-time systems and process-control apps exist in one of a limited number of states; many information systems deal with business objects (orders, invoices, inventory items) that have life cycles of statuses.

### Elements of an STD
- **States** → rectangles (some notations use circles; pick one and be consistent)
- **Transitions** → arrows connecting states
- **Events/conditions** → text labels on transition arrows (may also identify the system response)

A **termination state** has incoming arrows but no outgoing arrows—the final status values an object can take.

### Why use it
- Describing complex state changes in natural language has a **high probability of overlooking a permitted transition or including a disallowed one**.
- State-driven requirements are often sprinkled throughout an SRS, making overall behavior hard to grasp.
- The STD spans multiple use cases/stories, each of which may perform a transition.
- Reviews of the STD surface errors invisible in the textual requirements (the CTS review found one unnecessary state, one missing state, and two incorrect transitions—none spotted from the functional requirements).
- **Facilitates early testing**—testers derive tests covering all allowed transition paths.

### State Table
A **matrix** representation: states down the first column and across the first row; each cell indicates whether the transition is valid and names the triggering event. The matrix format *guarantees* every possible transition is examined. Two diagrams show exactly the same information; you may not need both, but if you have one the other is easy to create. Rows with all "no" entries are termination states.

> **Tip.** STDs and state tables ensure all required states and transitions are correctly and completely described in the functional requirements. They are *complementary views*, not redundant.

---

## 7. Dialog Map

A **high-level UI design model** showing dialog elements and navigation links—but *not* detailed screen designs. Treats the UI as a series of state changes: only one dialog element (menu, workspace, dialog box, prompt, touch display) is active for input at a time; the user navigates to others based on actions taken. A dialog map is essentially **a UI modeled as a state-transition diagram** (Wasserman 1985).

### Notation
- Each dialog element → rectangle (state)
- Each allowed navigation → arrow (transition)
- Trigger condition → text label on arrow. Trigger types:
  - **User action** (function key, hyperlink click, gesture)
  - **Data value** (invalid input triggers error message)
  - **System condition** (printer out of paper)
  - **Combination** of the above

### Use
- Explore hypothetical UI concepts based on requirements; reach common vision of user–system interaction.
- Useful for modeling **website visual architecture**.
- Trace through to find missing/incorrect/unnecessary navigation (hence missing requirements).
- Conceptual analysis view **guides** detailed UI design later.
- Related techniques: *navigation map* (Constantine & Lockwood 1999); *user interface flow* (swimlane-style, Beatty & Chen 2012); *system storyboards* (Leffingwell & Widrig 2000).

### Simplification tips
- Omit **global functions** like F1-help from every dialog element—clutters the map. Specify in the SRS instead.
- Omit standard website navigation links and Back-button reversals.
- Branching decisions (user choices) are hidden behind the display screens; conditions appear on the transitions.
- Don't pin down all UI design details at the requirements stage—use the map to reach common understanding of *functionality*.

### Entry/exit notation
- Entry point: transition line beginning with a **solid black circle**.
- Exit point: transition line ending with a **solid black circle inside another circle**.

---

## 8. Decision Tables & Decision Trees

Software systems are often governed by **complex logic** with various combinations of conditions leading to different behaviors. Textual specifications easily **overlook a condition**, producing a missing requirement; such gaps are hard to spot by review.

### Decision Table
- Lists all factors (conditions) that influence behavior.
- Each factor can be true/false, yes/no, or multi-valued.
- Each row/column combination indicates the expected system action for that combination of conditions.
- *Irrelevant* conditions are shown as dashes (don't-care).
- For *n* binary factors there are up to 2ⁿ combinations; many collapse to the same response. Example: 4 factors → 16 combinations → **5 distinct functional requirements** (see Fig 12-6).
- Each outcome maps to a distinct functional requirement.

### Decision Tree
- Same logic as a decision table, drawn as a tree.
- Branches = conditions; leaves = outcomes.
- Easier for some stakeholders to read than a table.

### When to use
- Whenever complex branching logic governs behavior.
- To ensure **no combination of conditions is overlooked**.
- Even a complex table/tree is easier to read than a mass of repetitious textual requirements.
- Can be used to **model an event-response table** as a decision table to ensure all combinations of event + system state are analyzed.

---

## 9. Event-Response Table

Use cases/stories aren't always sufficient—especially for **real-time systems**. A traffic-light intersection has very few "use cases" but many event-driven behaviors.

### Three classes of events
| Class | Trigger | Example |
|---|---|---|
| **Business event** | Human user initiates a dialog (use case trigger) | User initiates "Request a Chemical" |
| **Signal event** | External hardware/software sends a control signal, data reading, or interrupt | Switch closes; voltage changes; another app requests a service; finger swipe |
| **Temporal event** | Time-triggered: clock reaches a specified time, or preset duration elapses since previous event | Midnight data export; log temperature every 10 seconds |

### Table contents
- Event ID
- Event description (essence, not implementation)
- System state at time of event
- Expected system response (may depend on state)
- Optional: event frequency, data elements needed, resulting system state

### Why use it
- Excellent for **real-time / control systems** and for **scoping** (listing events crossing the system boundary).
- Can serve as part of the functional requirements for that portion of the system.
- A response could alter internal state or produce an externally visible result.
- Write requirements at the **essential level** (describe the event, not the implementation) to avoid imposing unnecessary design constraints—but record known design constraints separately.

> **Reminder.** The event-response table doesn't supply all functional/nonfunctional detail. How many wiper cycles per minute on slow? Is intermittent continuously variable or discrete? Min/max delay between wipes? If omitted, the developer must track it down or decide himself.

---

## 10. A Few Words About UML

The **Unified Modeling Language** is the standard object-oriented modeling language. It is primarily a *design* notation, but several UML models are useful at the abstraction level appropriate for requirements analysis:

| UML Diagram | Requirements Use |
|---|---|
| **Class diagram** | Object classes in the application domain; attributes, behavior, properties; relations among classes. Can also serve as a data model (limited application—doesn't fully exploit semantics). |
| **Use case diagram** | Actors external to the system and their use cases (Ch 8). |
| **Activity diagram** | Flows within a use case; roles performing actions (swimlane-style); business process flow (Ch 8). |
| **State (state machine) diagram** | States of a system/object and allowed transitions—richer notation than a basic STD. |

> **Note.** OO products don't demand unique requirements approaches—requirements focus on *what users need*, not *how it's constructed*. Users don't care about objects or classes. But if you *know* you'll build with OO, identifying classes/attributes/behaviors during analysis eases the analysis→design transition.

---

## 11. Modeling on Agile Projects

- **All projects should exploit requirements models**, regardless of development approach.
- The *choice* of models is largely the same; the **difference is timing and detail**.
  - Draft a level-0 DFD early; draw more detailed DFDs *within an iteration* covering only that iteration's scope.
  - Models may be less persistent / less perfected (whiteboard sketch + photo, not stored in a formal tool).
  - As user stories are implemented, update models (e.g., color-code to indicate completeness) to reveal additional stories needed.
- **Key principle:** Create only the models you need, only when you need them, only to the level of detail needed for stakeholders to adequately understand the requirements. User stories alone won't always be sufficient (Leffingwell 2011). Don't rule out any model *just because* you're agile.

---

## 12. Specifying Data Requirements (Ch 13)

Data requirements pervade all three levels of the requirements model—wherever there are functions, there is data. The BA should begin collecting data definitions **as they pop up during elicitation**, starting with input/output flows on the context diagram. Nouns users mention often indicate data entities: *chemical request, requester, chemical, status, usage report*.

---

## 13. Entity-Relationship Diagram (ERD)

A **data model** depicting the system's data relationships. Provides the high-level view; the data dictionary provides the detailed view. A commonly used notation is the ERD (Robertson & Robertson 1994).

### Analysis vs. design ERD
- **Analysis ERD**: represents logical groups of information from the problem domain and their interconnections. Used as a requirements analysis tool; communicates data components of the business/system **without implying the product will include a database**.
- **Design ERD**: defines the logical or physical (implementation) structure of the system's database.

### Elements
- **Entities** → rectangles; named as singular nouns. Represent physical items (incl. people) or aggregations of data important to the business. During relational DB design, entities normally become **tables**.
- **Attributes** → describe each entity; individual instances have different attribute values. Precise definitions live in the data dictionary.
- **Relationships** → diamonds (Peter Chen notation) connecting pairs of entities; named to describe the connection (e.g., "placing" between Requester and Chemical Request). Read actively ("Requester places Chemical Request") or passively ("Chemical Request is placed by Requester"). Prefer a single relationship name that reads correctly in either direction.
- **Cardinality (multiplicity)** → number/letter on the connecting lines. Common patterns:
  - **One-to-many** (1:M): Requester → Chemical Request
  - **One-to-one** (1:1): Chemical Container → Container History
  - **Many-to-many** (M:N): Vendor Catalog ↔ Chemical
  - If a precise cardinality is known, show it (e.g., "2" for biological parents) instead of generic M.

### Alternative notations
- **Peter Chen**: entities (rectangles), relationships (diamonds), cardinality as 1/M on lines.
- **James Martin (crow's foot)**: entities (rectangles), relationship label on the connecting line, cardinality via symbols—vertical bar = 1, crow's foot = many, circle = zero (e.g., "zero or more").
- **UML class diagram**: classes in rectangles with three sections (name / attributes / operations). For *data modeling*, the operations section is left empty. Multiplicity notation: `1..*` (one or more), etc.

> **Notation doesn't matter—consistency does.** Everyone on the project (ideally the organization) should follow the same conventions, and all reviewers should know how to interpret them.

### Cross-model insights
- Entities in the ERD often correspond to **data stores** in the DFD.
- Relationships reveal functionality: the "tracking" relationship between Chemical Container and Container History implies a need for a use case/story/flow giving users access to container history.
- Data analysis may reveal **unneeded data** that came up in discussion but isn't used anywhere.

---

## 14. The Data Dictionary

A **shared repository** defining the meaning, composition, data type, length, format, and allowed values for data elements used in the application. Complements (and should be kept separate from) the **project glossary**, which defines domain/business terms, abbreviations, and acronyms.

### Why have one
- Avoids problems when developers use different variable names, lengths, and validation criteria for the same data item (the cautionary tale that opens Ch 13).
- Identifies data validation criteria; helps developers write correct programs; minimizes integration problems.
- Reusable across applications, especially within a product line; using consistent definitions across the enterprise reduces integration/interface errors.
- When possible, **refer to existing enterprise standard data definitions**, using a smaller project-specific set to close gaps.
- A separate dictionary (vs. sprinkling definitions throughout docs) makes information easy to find and avoids redundancies/inconsistencies.

> **Maintenance warning.** If you keep the data dictionary current, it remains valuable throughout the system's operational life. If you don't, it may falsely suggest out-of-date information, and team members will no longer trust it.

### Entry types
| Type | Description | Example (CTS) |
|---|---|---|
| **Primitive** | Cannot be further decomposed. Described by data type, length, numerical range, allowed values, etc. | Number of Containers, Quantity, Quantity Units, Request ID, Requester Name |
| **Structure (record)** | Composed of multiple data elements, separated by `+` signs. Can incorporate other structures. | Chemical Request, Delivery Location, Requested Chemical, Requester |

### Notation conventions
- **`+`** separates elements within a structure.
- **`( )`** encloses an *optional* element (value need not be supplied).
- **`{ }`** encloses a *repeating group*; prefix with `minimum:maximum` (e.g., `1:10{Requested Chemical}` = at least 1, at most 10). Use `n` for unlimited maximum (e.g., `3:n{something}`).
- **Hyperlinks** in the "Composition or Data Type" column let readers jump to definitions of referenced elements—very helpful in extensive dictionaries.
- Organize entries **alphabetically**.

### Defining primitives precisely
Seemingly simple types hide complexity. For "alphabetic characters" (e.g., Requester Name), specify:
- Case sensitivity? Convert to upper/lower? Retain entered case? Reject mismatched case?
- Which alphabets—English only, or diacritical marks (tilde, umlaut, accent, grave, cedilla)?
- Allowed non-letter characters (blanks, hyphens, periods, apostrophes)?
- Display formats (timestamps, dates vary by country).

See Stephen Withall (2007) for many considerations when specifying data types.

---

## 15. Data Analysis Techniques

### Mapping representations against each other
- Entities in the ERD should be defined in the data dictionary.
- Data flows/stores in the DFD should appear in the ERD and data dictionary.
- Display fields in a report specification should appear in the data dictionary.
- Compare these complementary views to identify errors and refine data requirements.

### CRUD Matrix
A rigorous technique for detecting **missing requirements**. CRUD = **Create, Read, Update, Delete**. Correlates system actions with data entities to show where and how each significant entity is C/R/U/D.

Possible correlations:
- Data entities ↔ system events (Ferdinandi 2002; Robertson & Robertson 2013)
- Data entities ↔ user tasks / use cases (Lauesen 2002)
- Object classes ↔ use cases (Armour & Miller 2001)

#### How to use
1. Build the matrix: rows = use cases (or events); columns = data entities.
2. In each cell, record C/R/U/D (or leave blank) to indicate how the use case uses the entity.
3. **Scan each column** for missing letters:
   - Entity updated but never *created* → where does it come from?
   - Entity never *deleted* → three possibilities:
     1. Deletion is not an expected function (correct).
     2. A use case that deletes it is **missing**.
     3. An existing use case is **incomplete**—it's supposed to permit deletion but doesn't.
   - You won't know which interpretation is correct without investigation—but the CRUD matrix is a **powerful way to detect missing requirements**.

> Optional extensions: add **L** (List selection), **M** (Move), or a second **C** (Copy). Stick with plain CRUD for simplicity unless the project demands more granularity.

---

## 16. Specifying Reports

Many applications generate reports from databases, files, or other sources—tabular, charts, graphs, or combinations. Report specification **straddles requirements** (what information, how organized) **and design** (what it looks like).

### Eliciting reporting requirements
**Context questions:**
- What reports do you currently use? Which need to be modified? Which are generated but unused?
- Are there departmental/organizational/government **standards** reports must conform to (consistent look-and-feel, regulatory compliance)? Obtain copies and examples.

**Per-report questions:**
- Name, purpose/business intent, how recipients use the information, what decisions are made and by whom.
- Manual generation? By which user classes, how frequently?
- Automatic generation? Triggering conditions/events, frequency?
- Typical and maximum sizes?
- Need for a **dashboard** displaying several reports/graphs? Drill-down / roll-up required?
- Disposition: displayed on screen, sent to recipient, exported to spreadsheet, printed, stored/archived?
- Security/privacy/management restrictions on access or included data? Relevant business rules?

**Content questions:**
- Data sources and selection criteria for pulling data.
- User-selectable parameters.
- Required calculations or data transformations.
- Sorting, page-break, and total criteria.
- System response if no data is returned.
- Should underlying data be made available for **ad hoc reporting**?
- Can this report serve as a **template** for a set of similar reports?

### Report specification considerations
- **Consider variations.** Different sequencing (order-by on additional elements); summarize vs. drill-down; provide user tools to specify column sequence.
- **Find the data.** Ensure data necessary to populate the report is available to the system—may reveal previously unknown requirements to access or generate data. Identify business rules applied to compute output.
- **Anticipate growth.** An initial layout that works with small data volumes may break at scale (e.g., too many divisions force awkward page breaks or horizontal scrolling). Consider portrait→landscape, or transpose columnar→rows.
- **Look for similarities.** Multiple users (or the same user) may request similar but not identical reports. Merge variations into a single parameterized report to avoid redundant development and maintenance.

---

## 17. Pitfalls & Guardrails

- **Analysis paralysis.** All sample "after" requirements can be improved further, but you can't spend forever perfecting them. Goal: requirements good enough to proceed with design and construction at an acceptable level of risk.
- **Dogma vs. communication.** Not everyone adheres to the same DFD conventions (e.g., some BAs show external entities only on the context diagram). Using the models to *enhance communication* is more important than dogmatic conformance.
- **Single-view trap.** No one view is sufficient; models overlap, so you won't need every kind of diagram. If you create an ERD and a data dictionary, you probably won't need a class diagram.
- **Label analysis vs. design.** Same notation, different intent—always identify each model as analysis (concepts) or design (what you intend to build).
- **Keep the data dictionary current.** A stale dictionary is worse than none—team members stop trusting it.
- **Customers can learn to read models.** Don't assume they can't; don't conclude they can. Include a key, explain purpose/notation, walk through a sample model.

---

## 18. Next Steps (Practice)

- **Practice modeling** by documenting an existing system—e.g., draw a dialog map for an ATM or a website you use.
- On your current/next project, **select one modeling technique** that complements the textual requirements. Sketch on paper/whiteboard first, then use a modeling tool. Try at least one model you haven't used before.
- **Create a visual model collaboratively** with stakeholders—use whiteboards or sticky notes to encourage participation.
- **List external events** that could stimulate your system; create an event-response table showing system state and expected response for each.
- **Hold a requirements review** with 3–6 stakeholders; inspect the SRS for the desirable characteristics; look for conflicts, missing requirements, and missing sections. Ensure defects are corrected in the SRS and downstream work products.
- **Examine a page of functional requirements** from your project; check each statement for the characteristics of excellent requirements; rewrite any that don't measure up.

---

## 19. Key Takeaways

1. **No single view suffices.** Combine textual requirements with multiple visual models at different abstraction levels.
2. **Models augment, not replace, the SRS.** Written requirements retain their value for detail and precision.
3. **Models reveal what text hides**—missing, extraneous, and inconsistent requirements.
4. **Focus modeling on the riskiest, most complex, most critical portions** of the system.
5. **Use models together** to cross-check completeness (DFD ↔ ERD ↔ data dictionary ↔ CRUD matrix).
6. **Label analysis vs. design** models clearly—same notation, different intent.
7. **Data dictionary is a serious quality investment**—keep it current or don't have one.
8. **CRUD matrix is a powerful missing-requirement detector.**
9. **Agile projects still benefit from modeling**—create only what you need, when you need it, to the detail you need.
10. **Reports straddle requirements and design**—elicit content, context, usage, and variations; anticipate growth.

---

## References

- Beatty, J., & Chen, A. (2012). *The Quest for Software Requirements.*
- Booch, G., Rumbaugh, J., & Jacobson, I. (1999). *The Unified Modeling Language User Guide.*
- Constantine, L., & Lockwood, L. (1999). *Software for Use.*
- DeMarco, T. (1979). *Structured Analysis and System Specification.*
- Ferdinandi, P. L. (2002). *A Requirements Pattern.*
- Fowler, M. (2003). *UML Distilled.*
- Gilb, T. (2005). *Competitive Engineering.*
- Gottesdiener, E. (2005). *The Software Requirements Memory Jogger.*
- Hatley, D., Hruschka, P., & Pirbhai, I. (2000). *Process for System Architecture and Requirements Engineering.*
- Leffingwell, D. (2011). *Agile Software Requirements.*
- Leffingwell, D., & Widrig, D. (2000). *Managing Software Requirements.*
- Podeswa, H. (2010). *UML for the IT Business Analyst.*
- Robertson, S., & Robertson, J. (1994, 2013). *Complete Systems Analysis.*
- Wasserman, A. (1985). *Toward a Unified Software Engineering Environment.*
- Wiegers, K. (2002). *Peer Reviews in Software.*
- Wiegers, K. (2006). *More About Software Requirements.*
- Withall, S. (2007). *Software Requirement Patterns.*


## Related

- [[Software Requirements Overview]] — All requirements topics
- [[04_Use_Cases_and_Business_Rules]] — Use cases as modeling input
- [[05_Documenting_Requirements]] — SRS incorporates models
- [[07_Quality_and_Prototyping]] — Prototyping validates models
