---
tags:
  - requirements
  - use-cases
  - user-stories
  - business-rules
  - software-requirements
source: Wiegers, "Software Requirements" Ch 8-9
---

# Use Cases & Business Rules

> **Source:** Karl Wiegers, *Software Requirements*, 2nd ed., Chapters 8–9.
> **Theme:** Usage-centric requirements (use cases, user stories) + the business rules that govern them.

---

## 1. From Features to Users

Two complementary techniques dominate user-requirements exploration:

| | **Use Case** | **User Story** |
|---|---|---|
| Form | "Verb + object" name, detailed template | `As a <user>, I want <goal> so that <reason>.` |
| Direction | Elicits detailed dialog → derives functional requirements & tests | Placeholder for just-in-time conversations → acceptance tests |
| Structure | Rich template (preconditions, flows, exceptions) | Concise, intentionally lightweight |
| Origin | Jacobson/Cockburn (UML tradition) | Agile (Cohn, Beck) |
| Best when | Remote team, high-risk, novel domain, detailed tests needed | Co-located team, engaged customer, evolving requirements |

Both shift from *product-centric* ("what should the system do?") to *usage-centric* ("what does the user need to accomplish?"). Usage-centric elicitation reveals essential functionality, avoids orphan features, and drives prioritization.

### Where they fall short

- Batch processes, data warehousing, analytics, computationally intensive systems — complexity is in computations/data, not interactions.
- Embedded & real-time systems — better served by **event-response lists** (see Ch 12).
- Don't force every requirement into a use case. Some functionality is already known; creating a use case just to hold it adds no value.

---

## 2. The Use Case Approach

### Core definitions

- **Actor** — a role (not necessarily a person) that interacts with the system. Users wear "hats" labeled with actor names; the system recognizes the actor, not the person.
  - **Primary actor** — initiates the use case and derives main value.
  - **Secondary actor** — participates in successful execution (often another system).
- **Use case** — a sequence of interactions between system and external actor producing value. Name = verb + object ("Request a Chemical").
- **Scenario** — a single instance of usage. A use case is a collection of related scenarios.

### Use case diagram (UML)

- System boundary = box frame.
- Actors = stick figures outside the box.
- Use cases = ovals inside.
- Arrow actor → use case = primary actor.
- Arrow use case → actor = secondary actor.
- Arrows show *participation*, not data flow (unlike a context diagram).

### Identifying use cases

1. Identify actors first → lay out business processes → define use cases where actors and systems interact.
2. Create specific scenarios per business process → generalize into use cases.
3. From a business process description, ask "What tasks must the system perform?"
4. Identify external events → relate to actors and use cases.
5. **CRUD analysis** — every data entity needs Create/Read/Update/Delete use cases.
6. Examine the context diagram — "What does each external entity want to achieve?"

Bottom-up (brainstorm tasks) and top-down (from business processes) approaches complement each other; comparing both reduces omissions.

---

## 3. Use Case Template (Fully Dressed)

| Element | Description |
|---|---|
| **ID + Name** | Unique identifier (e.g., `UC-4 Request a Chemical`) + verb-object goal name |
| **Description** | Brief purpose statement |
| **Actor(s)** | Primary + secondary actors involved |
| **Trigger** | Event that initiates execution |
| **Preconditions** | Prerequisites the system can test before beginning (system state, not user intent) |
| **Postconditions** | System state after successful completion — user-visible outcomes, physical results, internal state changes |
| **Normal flow** | Numbered actor↔system dialog steps (the "happy path") |
| **Alternative flows** | Other success paths; branch from normal flow at decision points; may rejoin |
| **Exceptions** | Anticipated error conditions + handling; may recover or terminate |
| **Other information** | Performance, quality, constraints, external interfaces not captured elsewhere |

### Labeling convention

- Use case: `UC-4 Request a Chemical`
- Normal flow: `4.0`
- Alternative flows: `4.1`, `4.2`, …
- Exceptions: `4.0.E1` (first exception on normal flow), `4.1.E2` (second exception on alt flow 4.1)

### Preconditions & postconditions

- **Preconditions** are system-testable prerequisites, *not* the trigger and *not* user intent.
  - ATM example: "ATM contains money" is a precondition; "I need cash" is not.
  - Checking preconditions up front prevents errors and improves robustness.
- **Postconditions** describe the end state:
  - User-observable ("system displayed balance")
  - Physical ("ATM dispensed cash + printed receipt")
  - Internal ("account debited by withdrawal amount + fees")
- Users won't volunteer internal postconditions — BA must discover these with SMEs.

### Normal vs. alternative vs. exception flows

- **Normal flow** (a.k.a. main flow, basic flow, happy path, sunny-day scenario) — one success path through the use case.
- **Alternative flows** (secondary scenarios) — deliver same business outcome with variations; branch from and may rejoin the normal flow.
- **Exceptions** — conditions that can prevent success; describe anticipated errors and handling.
  - If not specified during elicitation → developers guess inconsistently, or the system crashes.
  - Cross-cutting errors (network loss, DB failure, paper jam) → treat as functional requirements rather than repeating as exceptions per use case.
- A user story may cover an entire use case, a single scenario, or one alternative flow.
- Complex flows → represent with **flowchart** or **UML activity diagram**.

### Casual vs. fully dressed

- **Casual** — textual narrative, perhaps just the Description section.
- **Fully dressed** — complete template.
- Fully dressed is valuable when:
  - Users aren't closely engaged throughout the project.
  - High complexity / high-risk failures.
  - Novel requirements unfamiliar to developers.
  - Use cases are the most detailed requirements developers receive.
  - Comprehensive test cases will be derived from use cases.
  - Remote collaborating teams need a shared group memory.

---

## 4. Extend & Include Relationships

- **Extend** — a standalone use case extends the normal flow into an alternative flow (e.g., "Search Vendor Catalogs" extends "Request a Chemical").
- **Include** — several use cases share common steps; extract into a subordinate use case they all include (e.g., "Pay a Bill" and "Reconcile Credit Card" both `<<include>>` "Write a Check"). Analogous to calling a subroutine.
- Don't over-debate when/how to use these — they are tools, not dogma.

### Chaining use cases

- Users can chain use cases into a "macro" use case (e.g., "Buy Product" = "Search Catalog" → "Add Item to Cart" → "Pay for Items").
- For chaining to work: **postconditions of one use case must satisfy preconditions of the next**.

---

## 5. Use Cases ↔ Business Rules ↔ Functional Requirements

### Use cases and business rules

- Business rules constrain which roles can perform use cases or parts thereof.
- Rules can impose preconditions the system must test before proceeding.
- Rules influence normal-flow steps (valid input values, computation methods).
- When specifying a use case, record IDs of relevant business rules and indicate which parts they affect.
- Elicitation often *discovers* business rules ("Fred shouldn't see my orders").

### Use cases and functional requirements

- Developers implement **functional requirements**, not use cases directly.
- Use cases describe the *user's perspective* (externally visible behavior); they don't contain all implementation detail.
- Passing use cases straight to developers → gaps and questions. Have a BA explicitly derive functional requirements.
- Four documentation strategies:

| Strategy | Description |
|---|---|
| **Use cases only** | Include FRs within each use case specification; cross-reference duplicates; still need NFRs and non-use-case functionality. |
| **Use cases + FRs** | Simple use cases + derived FRs in SRS/repository; establish traceability between them. |
| **FRs only** | Organize FRs by use case or feature; concise use cases as context; no separate user-requirements doc. |
| **Use cases + tests** | Detailed use cases + acceptance tests for behavior/alternatives/exceptions; avoid duplicating FRs. |

### Validating use cases

- Derive FRs from use cases → review with participants.
- Draw analysis models (e.g., state-transition diagrams spanning multiple use cases).
- Generate conceptual tests early — independent of implementation/UI specifics.
- Compare multiple representations (FRs, tests, models, prototypes) to catch errors, gaps, and different interpretations. A single representation must simply be trusted; multiple representations enable cross-checking.
- Allow ≥1 day between successive workshops — freshness blinds reviewers to errors.

---

## 6. Use Case Traps to Avoid

- **Too many use cases** — writing at too low a level; don't create one per scenario. Rule of thumb: more FRs than use cases, more use cases than business requirements.
- **Highly complex use cases** — 4+ pages of dense logic. Pick one normal flow; use alternative flows for other success branches; exceptions for failures. Each alternative should be short. If a flow exceeds 10–15 steps, question whether it's really one scenario. Don't split arbitrarily just for length.
- **Including design in use cases** — focus on *what* users accomplish, not *how* screens look. Say "System presents choices" not "System displays drop-down list."
- **Including data definitions in use cases** — store in a project-wide data dictionary / data model, not per use case. Prevents duplication and drift.
- **Use cases users don't understand** — write from the user's perspective, not the system's; have users review them.
- **Over-detailing distant use cases** — don't fully specify use cases won't be implemented for months/years; they'll likely change.

---

## 7. Business Rules (Chapter 9)

### What they are

> Policies, laws, standards, and controlling principles that govern how an organization operates.

- Business rules are a **property of the business**, not software requirements themselves.
- They are a **rich source of requirements** because they dictate properties the system must possess to conform.
- Same rule can apply to multiple manual or automated processes → treat as a separate, reusable asset.

### Business rules vs. business requirements vs. business processes

| Concept | Definition |
|---|---|
| **Business rule** | Controlling principle (policy, law, standard) governing business conduct. Originates outside any specific software. |
| **Business requirement** | Desirable outcome / high-level objective justifying a project. |
| **Business process** | Series of activities transforming inputs → outputs to achieve a result. Business rules *influence* processes. |

### How business rules influence requirements

| Requirement type | Influence | Example |
|---|---|---|
| **Business** | Government regulations → business objectives | "CTS must enable compliance with all federal/state chemical reporting regulations within 5 months." |
| **User** | Privacy policies → who can perform tasks | "Only lab managers may generate chemical exposure reports for others." |
| **Functional** | Company policy → system behavior | "If invoice from unregistered vendor, email supplier intake + W-9 forms." |
| **Quality attribute** | OSHA/EPA → safety requirements | "System must maintain training records and check them before hazardous chemical requests." |

### Why manage business rules centrally

- Undocumented rules live only in experts' heads → knowledge vacuum when they leave.
- Conflicting understandings → inconsistent enforcement across applications.
- A master repository lets all affected projects implement rules consistently.
- Tracing each rule into implementing code makes updates (e.g., password-change frequency) tractable and enables reuse.

---

## 8. Business Rules Taxonomy

The Business Rules Group (2012) defines a business rule as:

> *"A statement that defines or constrains some aspect of the business. It is intended to assert business structure or to control or influence the behavior of the business."*

A simple five-category taxonomy (plus an optional sixth: **terms** — defined words/phrases/abbreviations; group with facts or store in a glossary) covers most situations:

### 8.1 Facts

Statements that are true about the business at a point in time; describe associations between important terms. Often appear in data models.

- Every chemical container has a unique bar code identifier.
- Every order has a shipping charge.
- Sales tax is not computed on shipping charges.
- Nonrefundable airline tickets incur a fee when the itinerary changes.
- Books taller than 16 inches are shelved in Oversize.

**Focus on in-scope facts** connected to context-diagram I/O, system events, data objects, or user requirements. Collecting irrelevant facts bogs down analysis.

### 8.2 Constraints

Restrict actions the system or users may perform. Signal phrases: "must," "must not," "may not," "only certain people/roles."

Origins include:

- **Organizational policies** — "Loan applicant under 18 must have a cosigner." / "Library patron: max 10 items on hold." / "Insurance correspondence: ≤4 digits of SSN."
- **Government regulations** — "Apps must comply with accessibility regulations." / "Pilots: ≥8 continuous hours rest per 24-hour period." / "Tax returns postmarked by first business day after April 14."
- **Industry standards** — "Mortgage applicants must satisfy FHA qualification." / "No deprecated HTML5 tags/attributes."

**Representation:** Many constraints deal with who-can-do-what → use a **roles and permissions matrix** (roles × functions, X marks permissions). Group roles (e.g., employees vs. non-employees) and functions (system ops, records, items).

**Implications:** Even constraints that don't translate directly into functionality carry software implications. Example: "Only supervisors/managers may issue cash refunds >$50" → implies user privilege levels and privilege checks before actions like opening the register drawer.

### 8.3 Action Enablers

Trigger an activity when specific conditions are true. Pattern: `If <condition>, then <action>.`

- If stockroom has containers of a requested chemical, offer existing containers to requester.
- On the last day of a calendar quarter, generate OSHA/EPA reports.
- If a chemical container's expiration date has passed, notify the current possessor.

Commercial examples (impulse purchase stimulation):
- If customer ordered a book by a multi-book author, offer the author's other books before completing the order.
- After a book is placed in the cart, display "customers also bought" titles.

**Complex conditions** → represent with a **decision table** (see Ch 12).

> ⚠ **Caution:** Poorly thought-out rule implementations hurt customers. Example: an airline's fraud-prevention rule (different last names → in-person ticket pickup) surfaced as a scary error message instead of clear guidance — wasting the customer's and agent's time.

### 8.4 Inferences

Also called *inferred knowledge* or *derived facts*. Creates a new fact from other facts. Uses the `if/then` pattern like action enablers, but the "then" clause provides **knowledge**, not an action.

- If payment not received within 30 calendar days after due, account is delinquent.
- If vendor cannot ship within 5 days of receiving the order, item is back-ordered.
- Chemicals with LD₅₀ toxicity < 5 mg/kg in mice are hazardous.

### 8.5 Computations

Transform existing data into new data via specific formulas or algorithms. Many follow external rules (e.g., tax withholding).

- Domestic ground shipping for orders > 2 lbs: `$4.75 + $0.12/ounce or fraction thereof`.
- Total order price = `Σ(item prices) − volume discounts + state/county sales tax + shipping + optional insurance`.
- Unit price discount: 10% for 6–10 units, 20% for 11–20 units, 30% for >20 units.

**Representation:** Natural language can be wordy/confusing. Prefer symbolic form (math expression) or a **rules table**:

| ID | Units purchased | % discount |
|---|---|---|
| DISC-1 | 1 through 5 | 0 |
| DISC-2 | 6 through 10 | 10 |
| DISC-3 | 11 through 20 | 20 |
| DISC-4 | More than 20 | 30 |

> ⚠ **Boundary trap:** Avoid range overlaps like 1–5, 5–10, 10–20 — ambiguous for boundary values (5, 10). Use non-overlapping ranges (1–5, 6–10, 11–20).

---

## 9. Atomic Business Rules

Composite rules combine multiple details into one statement — hard to understand, maintain, and verify for completeness.

> **Librarian example:** "You can check out a DVD or Blu-ray for one week, renew up to two times for three days each, but only if no other patron has placed a hold."

This bundles *duration*, *renewal count*, *renewal duration*, and *hold condition* into one sentence. Break it into **atomic** rules that cannot be decomposed further:

| Rule ID | Atomic business rule |
|---|---|
| BR-1 | A patron may check out a DVD or Blu-ray Disc for one week. |
| BR-2 | A patron may renew a DVD or Blu-ray Disc checkout. |
| BR-3 | A patron may renew a checkout up to two times. |
| BR-4 | Each renewal extends the checkout by three days. |
| BR-5 | A patron may not renew a checkout if another patron has placed a hold on the item. |

### Atomicity heuristics (von Halle 2002)

- **No "or" on the left-hand side** of an `if/then`.
- **No "and" on the right-hand side** of an `if/then`.
- Benefits: short, simple, reusable, modifiable, combinable. Functional requirements then depend on *combinations* of atomic rules.

---

## 10. Benefits of Usage-Centric Requirements

- **Clearer user expectations** — users know what the system will let them do.
- **Reveals ambiguity early** — walking through actor-system dialogs surfaces vagueness before construction.
- **Prevents orphan functionality** — every feature traces to a user goal.
- **Aids prioritization** — top-priority FRs originate in top-priority user requirements. Priority drivers:
  - Core business process enablement.
  - High frequency of use by many users.
  - Requested by a favored user class.
  - Required for regulatory compliance.
  - Other functions depend on it.
- **Technical benefits** — use cases reveal domain objects and responsibilities → feed OO design (class diagrams, sequence diagrams). Tracing FRs/design/code/tests back to user requirements makes future changes cascade more easily.
- **Multiple representations = quality** — comparing use cases, FRs, tests, and models catches errors no single view reveals.

---

## 11. Key Takeaways

- **Use cases and user stories** both start from user goals but diverge: use cases elaborate into structured specifications + derived FRs; user stories elaborate via conversation into acceptance tests.
- **Use case structure matters** — preconditions, postconditions, normal/alternative/exception flows, labeling conventions, and extend/include relationships provide rigor that user stories lack.
- **Business rules are enterprise assets**, not software requirements. Document them centrally, trace them into requirements/code, and keep them atomic.
- **The five-category taxonomy** (facts, constraints, action enablers, inferences, computations) helps ensure coverage and suggests how each rule type should be implemented.
- **Atomic rules** are short, reusable, and maintainable — avoid composite "and/or" rule statements.
- **Multiple representations** (use cases + FRs + tests + models) cross-validate requirements and catch gaps invisible in any single view.

---

## Related Notes

- [[Software Requirements Overview]] — KA-level context.
- [[01_Requirements_Fundamentals]] — functional vs. nonfunctional, requirement levels (business → user → functional).
- [[03_Elicitation_Techniques]] — interviews, workshops, prototyping, observation.
- [[05_Documenting_Requirements]] — SRS template, natural language vs. structured specifications.
- [[12_Analysis_Models]] — state-transition diagrams, activity diagrams, decision tables, data models referenced throughout Ch 8–9.
