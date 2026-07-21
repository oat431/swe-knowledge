---
tags:
  - requirements
  - srs
  - documentation
  - specification
  - software-requirements
---

# 05 — Documenting Requirements

> **Source:** Karl Wiegers, *Software Requirements* (2nd ed.), Chapters 10–11.
> **Theme:** How to organize, label, and write the Software Requirements Specification (SRS) — and the characteristics that make requirements *excellent*, not merely present.

---

## Ch. 10 — Documenting the Requirements

### Why Document at All?

Clear communication is the **core principle** of requirements development — from people with needs, to people who design solutions, to people who build and verify them. The result of requirements work is a **documented agreement** among stakeholders.

- The **vision and scope document** holds business requirements.
- **Use cases / user stories** capture user requirements.
- The **SRS** captures functional + nonfunctional requirements delivered to designers, developers, and testers.

> **Trap** — Do not rely on telepathy and clairvoyance as substitutes for specification. They don't work.

Even on exploratory or volatile projects where you're unsure what solution you'll end up with, the cost of recording knowledge is **small** compared to the cost of re-acquiring it later. The acts of specification and modeling force people to think through and precisely state things that verbal discussion leaves ambiguous.

### Limitations of the Document Form

The SRS doesn't have to be a traditional word-processing document. Documents pose problems:

- Hard to store descriptive attributes alongside requirements
- Clumsy change management
- Difficult historical versioning
- Hard to subset requirements by iteration/release
- Hard to trace requirements to other artifacts
- Duplication causes maintenance issues

**Alternatives:** spreadsheet, Wiki, database, or a **requirements management (RM) tool**. No matter the container, the *same kinds of information* are needed. The SRS template below is a reminder of what to collect and how to organize it.

### Representation Approaches

1. **Natural language** — well-structured and carefully written (most practical for most projects)
2. **Visual models** — process flows, state diagrams, data relationships, logic flows
3. **Formal specifications** — mathematically precise (high-risk systems like nuclear plant controls)

Structured natural language, augmented with tables, mock-ups, photographs, and mathematical expressions, remains the most practical approach for most software projects.

### Progressive Refinement

A key principle: you don't need to pin down every detail early. Think in **layers**.

1. Learn just enough to prioritize and allocate requirements to releases/iterations.
2. Detail groups of requirements **just-in-time** to give developers enough to avoid excessive rework.
3. Keep communication lines open among BA, dev team, customer reps, and stakeholders.

### The Software Requirements Specification (SRS)

Also called: Business Requirements Document (BRD), functional spec, product spec, system spec. "Software Requirements Specification" is the **industry-standard term** (ISO/IEC/IEEE 2011).

**The SRS states:**
- Functions and capabilities the system must provide
- System characteristics
- Constraints it must respect
- Behaviors under various conditions
- Desired qualities (performance, security, usability)

**It is the basis for:** project planning, design, coding, system testing, and user documentation. It should **not** contain design, construction, testing, or project management details (other than known design/implementation constraints).

> **Important** — A single requirements deliverable often cannot meet the needs of all audiences. Some need only business objectives, others the high-level picture, others the user's perspective, yet others need all details. This is why we advocate separate vision-and-scope, user requirements, and SRS deliverables.

**Audiences relying on the SRS:**
- Customers, marketing, sales — what product to expect
- Project managers — schedule, effort, resource estimates
- Development teams — what to build
- Testers — requirements-based tests, test plans
- Maintenance/support staff — what each part should do
- Documentation writers — user manuals, help screens
- Training personnel — educational materials
- Legal staff — regulatory compliance
- Subcontractors — work basis (legally binding)

> If a desired capability or quality doesn't appear in the requirements agreement, no one should expect it in the product.

### How Many Specifications?

- **Most projects:** one SRS.
- **Large systems:** system requirements spec → separate software and hardware requirements specs. Example: one project had 800 high-level system requirements divided across 20 subprojects, each with ~800–900 derived requirements.
- **Too few (overload):** "The Spec" — one all-inclusive document for everything. Change management nightmare; wrong detail level for each audience.
- **Too many:** subdividing a 40–60 page SRS into 12 documents. Hard to keep synchronized.
- **Agile extreme:** sticky notes that lost adhesion and fluttered to the ground.

**Better alternative:** store requirements in an RM tool. The SRS for any portion/iteration becomes just a report generated from the database.

**Baselining** = transitioning an SRS under development into one reviewed and approved. Every project should baseline an agreement for each set of requirements **before** the team implements them.

### Readability Suggestions

- Use an appropriate **template** to organize information.
- Label sections, subsections, and individual requirements **consistently**.
- Use visual emphasis (bold, underline, italics, color, fonts) consistently and judiciously — color may not be visible to color-blind users or in grayscale print.
- Create a **table of contents**.
- Number all figures and tables, with captions, and refer by number.
- Use the word processor's **cross-reference** facility (not hard-coded page/section numbers).
- Define **hyperlinks** to related sections in the SRS or other files.
- In an RM tool, use **links** to navigate related information.
- Include **visual representations** when possible.
- Enlist a **skilled editor** for coherent vocabulary and layout.

### Labeling Requirements

Every requirement needs a **unique and persistent identifier** — for change requests, modification history, cross-references, traceability matrices, and reuse. Simple numbered or bulleted lists aren't adequate.

> **Trap (Number 8, with a bullet)** — One BA carried an SRS with up to **eight levels of bullet hierarchy**, using different symbols (❍, ■, ◆, q, ➭) but no meaningful labels. It's impossible to refer to or trace a bulleted item.

#### Method 1 — Sequence Number
- e.g., `UC-9`, `FR-26`.
- Prefix indicates type (FR = functional requirement).
- Numbers never reused (deleted requirement's number retired).
- **Pros:** simple; survives reordering.
- **Cons:** no logical/hierarchical grouping; no clue about content.

#### Method 2 — Hierarchical Numbering
- If functional requirements are in section 3.2, labels begin with `3.2.x.y`.
- e.g., `3.2.4.3` is a child of `3.2.4`.
- **Pros:** simple, compact, familiar; auto-generated by word processors.
- **Cons:** labels grow long; no content clue; typically **not persistent** — insert/delete/move shifts all following numbers, breaking references.

> **Trap** — A BA once said, "We don't let people insert requirements — it messes up the numbering." Don't let ineffective practices hamper sensible work.

**Improvement:** Number major sections hierarchically, then identify individual functional requirements with a short text code + sequence number. E.g., Section 3.5 "Editor Functions" → `ED-1`, `ED-2`, etc.

#### Method 3 — Hierarchical Textual Tags (Gilb 1988)
- e.g., `Print.ConfirmCopies` — part of the print function, relates to number of copies to print.
- **Pros:** structured, meaningful, unaffected by adding/deleting/moving requirements.
- Full unique ID = each line's label appended to parent labels above it. E.g., `Product.Cart`, `Product.Discount.Error`.
- Parent written as a **title/heading/feature name**, not as a functional requirement; children in aggregate deliver the parent's capability.
- **Cons:** tags longer; must invent meaningful names; maintaining uniqueness is challenging with multiple authors.
- **Simplification:** combine hierarchical naming with a sequence-number suffix for small sets: `Product.Cart.01`, `Product.Cart.02`.

### Dealing with Incompleteness

Use **TBD (to be determined)** to flag known knowledge gaps. Plan to resolve all TBDs before implementing a set of requirements — unresolved uncertainties increase the risk of developer/tester errors and rework.

> **Trap** — TBDs won't resolve themselves. Number them, record who is responsible for resolving each and by when, review status at regular checkpoints, and track them to closure.

If you must proceed with TBDs, either **defer** implementing unresolved requirements or **design those portions to be easily modifiable** when issues are resolved.

### User Interfaces and the SRS

**Benefits of including UI designs:**
- Paper prototypes, mock-ups, wireframes, simulation tools make requirements tangible to users and developers.
- If users have expectations about look-and-feel that would disappoint if unmet, those expectations belong in requirements.

**Drawbacks:**
- Screen images and UI architectures describe **solutions**, not requirements.
- Makes the document larger (big specs frighten some people).
- Delaying SRS baselining until UI design is complete slows development.
- Visual design can drive requirements → functional gaps.
- Once stakeholders see a UI, they won't "unsee" it — resistance to improvement.

**Balanced approach:** Include conceptual **sketches** (no matter how nicely drawn) of selected displays, without demanding implementation precisely follow them. A sketch communicates intent; a visual designer might still turn a complex dialog into a tabbed dialog for usability.

> Screen layouts don't replace written user and functional requirements. Don't expect developers to deduce underlying functionality and data relationships from screenshots.

If you want specific UI controls/screen layouts, include them as **design constraints** — but don't impose constraints unnecessarily, prematurely, or for the wrong reasons.

For many-screen projects, document UI specifics in a separate **user interface specification** or via UI/prototyping tools. Use **display-action-response models** to describe screen element names, properties, and behavior.

---

## SRS Template (Figure 10-2)

Every software development organization should adopt one or more **standard SRS templates**. Available templates: ISO/IEC/IEEE 2011; Robertson and Robertson 2013. Adopt a template for each major project class. Pick one section for each kind of information and use it consistently — avoid duplicating information across sections. Use cross-references and hyperlinks.

Use **version control** and include a **revision history** (who changed what, when, and why).

> **Important** — Incorporate material **by reference** to other project documents instead of duplicating it. Hyperlinks and traceability links work; be aware hyperlinks can break if folder hierarchies change.

### 1. Introduction

**1.1 Purpose** — Identify the product/application (with revision/release number). If the SRS covers only part of a system, identify that portion. Describe intended reader types (developers, PMs, marketing, users, testers, writers).

**1.2 Document conventions** — Standards/typographical conventions used: meaning of text styles, highlighting, notations. Specify the labeling format for requirements.

**1.3 Project scope** — Short description of the software and its purpose. Relate to user/corporate goals and business objectives. If a vision-and-scope document exists, refer to it rather than duplicating. For incremental releases, contain a scope statement as a subset of the long-term product vision. High-level summary of major features/significant functions.

**1.4 References** — List documents/resources the SRS refers to (with hyperlinks if persistent): UI style guides, contracts, standards, system requirements specs, interface specs, related product SRS. Include title, author, version, date, source, storage location, URL.

### 2. Overall Description

High-level overview of the product, environment, anticipated users, known constraints, assumptions, and dependencies.

**2.1 Product perspective** — Context and origin: next member of a product line? next version of a mature system? replacement? entirely new? If a component of a larger system, state how it relates to the overall system and identify major interfaces. Include visual models like a **context diagram** or **ecosystem map**.

**2.2 User classes and characteristics** — Identify user classes and describe pertinent characteristics. Some requirements pertain only to certain user classes. Identify **favored user classes**. If a master user class catalog exists, incorporate by reference rather than duplicating.

**2.3 Operating environment** — Hardware platform; OS and versions; geographical locations of users, servers, databases; hosting organizations. Other software with which the system must coexist. If extensive infrastructure work is needed, consider a separate **infrastructure requirements specification**.

**2.4 Design and implementation constraints** — Factors restricting developer options and the rationale for each. Watch for requirements written as solution ideas rather than needs — those impose design constraints unnecessarily.

**2.5 Assumptions and dependencies** —
- **Assumption:** a statement believed true in the absence of proof. Problems arise if assumptions are incorrect, obsolete, unshared, or change. Include only system-functionality-related assumptions here (business assumptions go in vision-and-scope).
- **Dependencies:** factors/components outside the project's control that the system depends on (e.g., .NET Framework 4.5+ must be installed).

### 3. System Features

Functional requirements **organized by system feature** (one possible arrangement; alternatives: functional area, process flow, use case, mode of operation, user class, stimulus/response, or hierarchical combinations).

**3.x System Feature X** — Name in a few words (e.g., "3.1 Spell Check"). Repeat 3.x with subsections for each feature.

**3.x.1 Description** — Short description + priority (high/medium/low). Priorities are dynamic. In an RM tool, define a requirement attribute for priority.

**3.x.2 Functional requirements** — Itemize specific functional requirements associated with the feature. Describe responses to anticipated error conditions and invalid inputs/actions. Uniquely label each. In an RM tool, create multiple attributes (rationale, origin, status).

### 4. Data Requirements

**4.1 Logical data model** — Visual representation of data objects/collections and relationships. Notations: entity-relationship diagrams, UML class diagrams. Business operations data model or logical representation of data the system will manipulate — **not** an implementation data model.

**4.2 Data dictionary** — Defines composition of data structures; meaning, data type, length, format, allowed values for data elements. Often better stored as a **separate artifact** (not embedded in the SRS) to increase reusability.

**4.3 Reports** — Identify and describe report characteristics. If a report must conform to a predefined layout, specify as a constraint (with example). Otherwise, focus on logical content descriptions, sort sequence, totaling levels — defer detailed layout to design.

**4.4 Data acquisition, integrity, retention, and disposal** — How data is acquired/maintained (e.g., initial dump + subsequent change-only feeds). Data integrity requirements: backups, checkpointing, mirroring, accuracy verification. Retention/disposal policies: temporary data, metadata, residual data (deleted records), cached data, local copies, archives, interim backups.

### 5. External Interface Requirements

> **Interface wars** — Two teams building a flagship product communicated through an API. One team periodically modified the API unilaterally; the complete system wouldn't build; diagnosis took hours. **A change in an interface demands communication with the person, group, or system on the other side of that interface.**

**5.1 User interfaces** — Logical characteristics of each UI. (Specific characteristics may also appear in 6.1 Usability.) Items to address:
- References to UI standards / product line style guides
- Standards for fonts, icons, button labels, images, color schemes, tabbing sequences, controls, branding, copyright/privacy notices
- Screen size, layout, resolution constraints
- Standard buttons/functions/navigation on every screen (e.g., help button)
- Shortcut keys
- Message display and phrasing conventions
- Data validation guidelines (input restrictions, when to validate)
- Layout standards for software localization
- Accommodations for visually impaired, color blind, or other limitations

**5.2 Software interfaces** — Connections between this product and other software components (name and version): applications, databases, OSes, tools, libraries, websites, integrated commercial components. State purpose, formats, contents of messages/data/control values exchanged. Specify mappings, translations, services needed/provided, inter-component communication nature. Identify exchanged/shared data. Specify nonfunctional requirements (response time service levels, frequencies, security controls).

**5.3 Hardware interfaces** — Characteristics of each software-hardware interface. Supported device types, data/control interactions, communication protocols. List inputs/outputs, formats, valid values/ranges, timing issues. If extensive, create a separate interface specification.

**5.4 Communications interfaces** — Requirements for communication functions: email, web browser, network protocols, electronic forms. Message formatting, communication security/encryption, data transfer rates, handshaking, synchronization. Constraints (e.g., acceptable email attachment types).

### 6. Quality Attributes

Nonfunctional requirements other than constraints (2.4) and external interface requirements (5). Should be **specific, quantitative, and verifiable**. Indicate relative priorities (e.g., ease of use over ease of learning; security over performance). Rich notations like **Planguage** clarify needed levels better than simple descriptive statements.

**6.1 Usability** — Ease of learning, ease of use, error avoidance and recovery, interaction efficiency, accessibility.

**6.2 Performance** — Specific performance requirements for system operations. If different features have different performance requirements, specify those goals with the corresponding functional requirements rather than collecting them here.

**6.3 Security** — Security/privacy requirements restricting access or use: physical, data, software. Often originate in business rules — refer to security/privacy policies/regulations (or to the business rules repository).

**6.4 Safety** — Requirements concerning possible loss, damage, or harm from product use. Safeguards/actions to take, dangerous actions to prevent. Safety certifications, policies, regulations.

**6.x Others** — Separate section for each additional quality attribute: availability, efficiency, installability, integrity, interoperability, modifiability, portability, reliability, reusability, robustness, scalability, verifiability.

### 7. Internationalization and Localization Requirements

Ensure suitability for use in nations, cultures, and geographic locations other than those where the product was created. Address: currency; formatting of dates, numbers, addresses, phone numbers; language (including national spelling conventions, e.g., American vs. British English); symbols; character sets; name order; time zones; international regulations/laws; cultural and political issues; paper sizes; weights and measures; electrical voltages and plug shapes. **Often reusable across projects.**

### 8. Other Requirements

Requirements not covered elsewhere: legal, regulatory, financial compliance, standards; product installation, configuration, startup, shutdown; logging, monitoring, audit trail. Add new sections pertinent to your project rather than lumping under "Other." Omit if all requirements are accommodated elsewhere.

**Transition requirements** (for migrating from a previous system) can go here if they involve software being written (e.g., data conversion programs) or in the project management plan if they don't (e.g., training).

### Appendix A: Glossary

Define specialized terms, acronyms, abbreviations. Spell out each acronym. Consider a **reusable enterprise-level glossary** spanning projects; each SRS defines only project-specific terms. (Data definitions belong in the data dictionary, not the glossary.)

### Appendix B: Analysis Models

Optional. Pertinent analysis models: data flow diagrams, feature trees, state-transition diagrams, entity-relationship diagrams. Often more helpful to **incorporate models into relevant sections** rather than collecting at the end.

---

## Requirements Specification on Agile Projects

Agile projects use **user stories** during elicitation. Each story is a statement of a user need/functionality valuable to the user or purchaser (Cohn 2004, 2010).

**Progressive refinement on agile:**
1. Write just enough for each story so stakeholders have general understanding and can prioritize.
2. Aggregate related stories into **minimally marketable features** that must be fully implemented prior to a release to deliver expected customer value.
3. Stories accumulated/prioritized into a **dynamic product backlog** evolving throughout the project.
4. Large stories subdivided into smaller stories allocated to multiple iterations.
5. Stories can be on index cards or in a story management tool (or not retained after implementation).

As the team enters each iteration, **conversations** among product owner, BA, developers, testers, and users flesh out details. Those details generally correspond to functional requirements in an SRS, but agile often represents them as **user acceptance tests** describing how the system will behave if the story is properly implemented.

- Tests conducted during the implementation iteration + future iterations for regression testing.
- Cover exception conditions as well as expected behavior.
- Automate tests for rapid, complete regression.
- If original stories are discarded, the only persistent requirements documentation may be the acceptance tests (if stored in a tool).

Nonfunctional requirements can be written on cards **as constraints** (Cohn 2004), or as acceptance criteria/tests associated with specific stories (e.g., security tests demonstrating permitted/blocked access). Agile teams can still use analysis models, data dictionaries, etc.

> **Overarching goal:** accumulate a shared understanding of requirements good enough to allow construction of the next portion of the product at an acceptable level of risk.

**Factors determining appropriate formality/detail:**
- Extent to which just-in-time informal verbal/visual communication can supply necessary details
- Extent to which informal communication can keep the team synchronized across time and space
- Extent to which it's valuable/necessary to retain requirements knowledge for future enhancement, maintenance, reengineering, verification, statutory/audit mandates, certification, or contracts
- Extent to which acceptance tests can serve as effective replacements for capability/behavior descriptions
- Extent to which human memories can replace written representations

> When you don't specify high-quality requirements, the resulting software is like a box of chocolates: you never know what you're going to get.

---

## Ch. 11 — Writing Excellent Requirements

> **Opening vignette:** Ruth asked Gautam about the song preview feature. The user story said only: "As a Customer, I want to listen to previews of the available songs so I can decide which ones to buy." Notes added: 30-second samples, built-in MP3 player. But Ruth wanted pause/stop, samples starting mid-song for long intros, fade in/out. Gautam: "I wish you had told me all of this when we spoke earlier. You didn't give me much to go on so I just had to make my best guess."

The best requirements repository in the world is useless if it doesn't contain **high-quality** information.

### Characteristics of Individual Requirement Statements

In an ideal world, every individual business, user, functional, and nonfunctional requirement would exhibit these qualities. The best way to tell whether your requirements possess them is to have **several stakeholders review them** — different stakeholders spot different problems.

#### Complete
Each requirement contains all information necessary for the reader to understand it. For functional requirements, this means providing what the developer needs to implement it correctly. Use **TBD** flags or issue-tracking to highlight gaps. Resolve all TBDs before developers proceed.

#### Correct
Each requirement accurately describes a capability that will meet some stakeholder's need and clearly describes the functionality to be built. Check with the source (user, higher-level system requirement, use case, business rule). A low-level requirement that conflicts with its parent is not correct. User representatives should review user requirements for correctness.

#### Feasible
It must be possible to implement each requirement within known capabilities/limitations of the system and its operating environment, and within project constraints (time, budget, staff). A developer participating during elicitation provides a reality check. Incremental development and proof-of-concept prototypes evaluate feasibility. If a requirement must be cut, understand the impact on project vision and scope.

#### Necessary
Each requirement describes a capability that provides anticipated business value, differentiates the product in the marketplace, or is required for conformance to external standards/policies/regulations. Every requirement should originate from a source with authority. Trace functional/nonfunctional requirements back to specific voice-of-the-user input (use case, user story). You should be able to relate each requirement to a business objective. If someone asks why a requirement is included, there should be a good answer.

#### Prioritized
Prioritize business requirements by importance to achieving desired value. Assign implementation priority to each functional requirement, user requirement, use case flow, or feature. If all requirements are equally important, the PM can't respond to schedule overruns, personnel losses, or new requirements. Prioritization is **collaborative** involving multiple stakeholder perspectives.

#### Unambiguous
Natural language is prone to two types of ambiguity:
1. **Self-detectable** — you can think of more than one interpretation.
2. **Harder to catch** — different people read the same requirement and come up with different interpretations, each making sense to that reader.

**Inspections** (formal peer reviews) are the best way to spot ambiguities — they let each participant compare their understanding to someone else's. "Comprehensible" is related: readers must understand what each requirement says.

> You'll never remove all ambiguity from requirements — that's the nature of human language. But reviews clean up a lot of the worst issues.

#### Verifiable
Can a tester devise tests or other verification approaches to determine whether each requirement is properly implemented? If not, deciding whether it was correctly implemented becomes a matter of opinion, not objective analysis. Requirements that are incomplete, inconsistent, infeasible, or ambiguous are also unverifiable. **Include testers in requirements peer reviews** to catch problems early.

### Characteristics of Requirements Collections (Sets)

Sets of requirements grouped into a baseline for a specific release/iteration should exhibit these characteristics, whether in an SRS, RM tool, user stories + acceptance tests, or any other form.

#### Complete
No requirement or necessary information should be absent. In practice, you'll never document every requirement — there are always assumed/implied requirements (which carry more risk than explicit ones). **Missing requirements are hard to spot because they aren't there.** Any specification containing TBDs is incomplete.

#### Consistent
Consistent requirements don't conflict with other requirements of the same type or with higher-level business, user, or system requirements. If you don't resolve contradictions before construction, developers will have to. Recording the **originator** of each requirement lets you know who to talk to when conflicts arise. Inconsistencies are hard to spot when related information is stored in different locations.

#### Modifiable
You can always rewrite a requirement, but maintain a **history of changes**, especially after baselining. Know connections/dependencies between requirements so you can find all that must change together. Modifiability dictates:
- Each requirement uniquely labeled and expressed separately.
- **Avoid redundancy** — repeating a requirement in multiple places makes reading easier but maintenance harder (all instances must be modified together).
- Cross-reference related items to keep them synchronized.
- Storing individual requirements just once in an RM tool solves redundancy and facilitates reuse.

#### Traceable
A traceable requirement can be linked **backward** to its origin and **forward** to derived requirements, design elements, implementing code, and verifying tests. You don't have to define all trace links for a requirement to be traceable — it needs the *properties* that make tracing possible:
- Uniquely labeled with persistent identifiers.
- Written in a structured, fine-grained way (not long narrative paragraphs).
- Don't combine multiple requirements into a single statement (different requirements may trace to different components).

> You're never going to create a perfect specification in which all requirements demonstrate all ideal attributes. But keeping these characteristics in mind when writing and reviewing requirements produces better specifications and better software.

---

## Guidelines for Writing Requirements

There is no formulaic way to write excellent requirements; the best teachers are **experience and feedback** from recipients. Receiving constructive feedback from colleagues with sharp eyes is a great help — this is why **peer reviews** of requirements documents are critical. Buddy up with a fellow BA and begin exchanging requirements for review.

> Mentally translate "writing requirements" to **"representing requirements knowledge."** Alternative representation techniques can present information more effectively than straight text.

**Two overarching goals:**
1. Anyone who reads the requirement comes to the same interpretation as any other reader.
2. Each reader's interpretation matches what the author intended.

These outcomes matter more than purity of style or conforming dogmatically to arbitrary rules.

### System or User Perspective

Functional requirements can be written from either perspective; intermingle styles as clarity dictates. State requirements consistently: "The system shall" or "The user shall" + action verb + observable result. Specify the trigger action/condition.

**EARS template (Mavin et al. 2009) — system perspective:**

```
[optional precondition] [optional trigger event] the system shall [expected system response].
```

Example:
> If the requested chemical is found in the chemical stockroom, the system shall display a list of all containers of the chemical that are currently in the stockroom.

**User-perspective template (Alexander and Stevens 2002):**

```
The [user class or actor name] shall be able to [do something] [to some object] [qualifying conditions, response time, or quality statement].
```

Example:
> The Chemist shall be able to reorder any chemical he has ordered in the past by retrieving and editing the order details.

Using the **user class name** (Chemist) rather than generic "user" reduces misinterpretation.

### Writing Style

Requirements writing isn't like fiction or other nonfiction. The school style (main idea → supporting facts → conclusion) doesn't work. **Put the punch line first** — the statement of need/functionality — followed by supporting details (rationale, origin, priority, attributes). This helps skimmers while still serving thorough readers.

Don't practice creative writing in requirements documents:
- Don't interleave passive and active voice for "interest."
- Don't use multiple terms for the same concept (customer, account, patron, user, client) just for variety.
- Being **easy to read and understand** is essential; being interesting is less important.

**Clarity and conciseness:**
- Complete sentences, proper grammar/spelling/punctuation.
- Short, direct sentences and paragraphs.
- Simple, straightforward language appropriate to the user domain; avoid jargon.
- Define specialized terms in a glossary.
- Condense "needs to provide the user with the capability to" → "shall."
- For each piece of information, ask: "What would the reader do with this?" If no stakeholder would find it valuable, perhaps you don't need it.
- **Clarity is more important than conciseness.**

**The keyword "shall":**
- Traditional convention to describe system capability.
- Objectors say "that's not how people talk" — so what? "Shall" clearly indicates desired functionality.
- Be **consistent**. Don't mix shall, must, may, might, will, would, should, could, needs to, has to — readers won't know if the differences are meaningful.
- Avoid using different verbs to connote priority ("shall" = required, "should" = desired, "may" = optional). **Always say "shall" and explicitly assign high/medium/low priority.** Priorities change; don't tie them to phrasing.
- Avoid "shall" for requirements and "will" for design expectations — readers may not grasp the distinction.

> **Trap** — One consultant suggested mentally replacing each "should" with "probably won't." Would the resulting requirement be acceptable? If not, replace "should" with something more precise.

**Active voice:**
Write in active voice to make clear what entity takes the action. Passive voice (e.g., "will be updated") denotes the recipient but not the performer.

*Passive (unclear):*
> Upon product upgrade shipment, the serial number will be updated on the contract line.

*Active (clear):*
> When Fulfillment confirms that they shipped a product upgrade, the system shall update the customer's contract with the new product serial number.

**Individual requirements:**
- Don't write long narrative paragraphs containing multiple requirements.
- Clearly distinguish individual requirements from background/contextual information.
- Watch for "and," "or," "additionally," "also" — they may signal combined requirements. If you'd use different tests to verify the two parts, split them.
- **Avoid "and/or"** — it leaves interpretation up to the reader.
- Watch for "unless," "except," "but" — they indicate multiple requirements. Failing to specify what happens when the "unless" clause is true is a common source of **missing requirements**.

Example split:
> If the Buyer's credit card on file is active, the system shall charge the payment to that card.
>
> If the Buyer's credit card on file has expired, the system shall allow the Buyer to either update the current credit card information or enter a new credit card for payment.

### Level of Detail

**Appropriate detail:** Decompose high-level requirements into sufficient detail to clarify and flesh out. There's no single answer to "how detailed?" — provide enough to minimize risk of misunderstanding, based on the team's knowledge and experience. If a developer can think of several possible ways to satisfy a requirement and all are acceptable, specificity is about right.

**Include more detail when:**
- Work is for an external client
- Development or testing will be outsourced
- Team members are geographically dispersed
- System testing will be based on requirements
- Accurate estimates are needed
- Requirements traceability is needed

**Include less detail when:**
- Work is internal to your company
- Customers are extensively involved
- Developers have considerable domain experience
- Precedents are available (e.g., replacing a previous application)
- A package solution will be used

**Consistent granularity:** It's not necessary to specify all requirements to the same level of detail, but within a set of related requirements, try for consistent granularity. Guideline: **write individually testable requirements.** If you can think of a small number of related test cases, granularity is appropriate; if you envision numerous diverse tests, several requirements may be combined.

*Inconsistent example (same SRS):*
> 1. The system shall interpret the keystroke combination Ctrl+S as File Save.
> 2. The system shall interpret the keystroke combination Ctrl+P as File Print.
> 3. The product shall respond to editing directives entered by voice.

Requirements 1 and 2 are very fine-grained (few tests); requirement 3 stipulates a complex speech-recognition subsystem (hundreds of tests). The fine-grained pair would be better expressed as a **table** of keystroke shortcuts; the speech-recognition requirement demands much more detail.

### Representation Techniques

Readers' eyes glaze over at dense text or long lists of similar requirements. Consider the most effective way to communicate each requirement. Alternatives: lists, tables, visual analysis models, charts, mathematical formulas, photographs, sound clips, video clips. These supplement (often don't replace) written requirements.

**Table technique for combinatorial requirements:** Instead of 12 separate requirements of the form "The Text Editor shall be able to parse `<format>` documents that define `<jurisdiction>` laws," state a master requirement and use a table where cells contain only the ID suffix:

> **Editor.DocFormat** The Text Editor shall be able to parse documents in several formats that define laws in the jurisdictions shown in Table 11-1.

| Jurisdiction | Tagged format | Untagged format | ASCII format |
|---|---|---|---|
| Federal | .1 | .2 | .3 |
| State | .4 | .5 | .6 |
| Territorial | .7 | N/A | .8 |
| International | .9 | .10 | .11 |

E.g., `Editor.DocFormat.3` → "The Text Editor shall be able to parse ASCII documents that define federal laws."

- Put **N/A** in cells that don't apply (clearer than omitting and having readers wonder why).
- Ensures **completeness** — if something is in every cell, you haven't missed any.

### Avoiding Ambiguity

Requirements quality is in the eye of the **reader**, not the author. If a reader has questions, the requirement needs more work. **Peer reviews** are the best way to find places where requirements aren't clearly understood.

**Fuzzy words:** Use terms consistently and as defined in the glossary. Watch for synonyms/near-synonyms (one project used four different terms for the same item in a single document). Pick one term; place synonyms in the glossary. Ensure pronoun antecedents are crystal clear.

**Adverbs introduce subjectivity and ambiguity.** Avoid: reasonably, appropriately, generally, approximately, usually, systematically, quickly. Ambiguous language leads to **unverifiable** requirements.

**Ambiguous terms to avoid (Table 11-2):**

| Ambiguous term | How to improve |
|---|---|
| acceptable, adequate | Define what constitutes acceptability and how the system can judge this. |
| and/or | Specify whether you mean "and," "or," or "any combination of." |
| as much as practicable | Don't leave it to developers. Make it a TBD with a date to resolve. |
| at least, at a minimum, not more than, not to exceed | Specify minimum and maximum acceptable values. |
| best, greatest, most | State desired level and minimum acceptable level of achievement. |
| between, from X to Y | Define whether endpoints are included in the range. |
| depends on | Describe the nature of the dependency. |
| efficient | Define how efficiently the system uses resources or performs operations. |
| fast, quick, rapid | Specify minimum acceptable time for the action. |
| flexible, versatile | Describe how the system must adapt to changing conditions/platforms/needs. |
| i.e., e.g. | Use words in your native language, not confusing Latin abbreviations. |
| improved, better, faster, superior, higher quality | Quantify how much better/faster constitutes adequate improvement. |
| including, including but not limited to, and so on, etc., such as, for instance | List all possible values/functions, or refer reader to the full list. |
| in most cases, generally, usually, almost always | Clarify when conditions don't apply and what happens then. |
| match, equals, agree, the same | Define whether text comparison is case-sensitive; "contains," "starts with," or "exact"; degree of precision for real numbers. |
| maximize, minimize, optimize | State maximum and minimum acceptable values. |
| normally, ideally | Identify abnormal/non-ideal conditions and describe system behavior then. |
| optionally | Clarify whether this means a developer choice, system choice, or user choice. |
| probably, ought to, should | Will it or won't it? |
| reasonable, when necessary, where appropriate, if possible, as applicable | Explain how the developer or user can make this judgment. |
| robust | Define how the system handles exceptions and unexpected conditions. |

---

## Key Takeaways

- **Document requirements to communicate**, not to satisfy a process. The SRS is a shared agreement, not a pile of information.
- **Form follows function:** document, spreadsheet, Wiki, database, RM tool — choose based on your needs, but the *information categories* stay the same.
- **Label every requirement** with a unique, persistent identifier. Hierarchical textual tags (`Product.Cart.01`) balance meaning and stability.
- **Resolve TBDs** before implementation; track them to closure.
- **Write excellent requirements** by targeting the characteristics: Complete, Correct, Feasible, Necessary, Prioritized, Unambiguous, Verifiable (individuals); Complete, Consistent, Modifiable, Traceable (sets).
- **Put the punch line first.** Use active voice, "shall," consistent terminology, and consistent granularity.
- **Avoid fuzzy words** — they make requirements unverifiable.
- **Peer reviews** are the single best mechanism for catching ambiguity and incompleteness that authors cannot see in their own work.
- **Progressive refinement** applies on every project, agile or plan-driven: detail just-in-time, baseline before implementation.
- Even the finest requirements documentation doesn't replace ongoing **discussion** throughout the project.

---

## Next Steps (from the chapters)

**From Ch. 10:**
- Review your project's requirements against the template to see if you have requirements from all sections that pertain to your project.
- If your organization doesn't have a standard SRS template, convene a small working group to adopt one; begin with Figure 10-2 and adapt it. Agree on a convention for labeling individual requirements.
- If storing requirements in something other than a document, study the template and check whether you're eliciting/recording all categories; modify your repository to incorporate missing categories.

**From Ch. 11:**
- Begin exchanging requirements for peer review with a fellow BA.
- Use the characteristics of excellent requirements as a checklist during reviews.
- Scan your requirements for the ambiguous terms in Table 11-2 and replace them with precise, verifiable language.

---

## Sources

- Karl E. Wiegers & Joy Beatty. *Software Requirements* (2nd ed.), Chapters 10–11. Microsoft Press, 2013.
- Mavin et al. (2009) — Easy Approach to Requirements Syntax (EARS).
- Tom Gilb (1988) — hierarchical textual tagging scheme.
- Alexander & Stevens (2002) — user-perspective requirement template.
- ISO/IEC/IEEE 29148:2011 — systems and software engineering life cycle processes — requirements engineering.
- Robertson & Robertson (2013) — Volere Requirements Specification Template.


## Related

- [[Software Requirements Overview]] — All requirements topics
- [[04_Use_Cases_and_Business_Rules]] — Use cases feed into SRS
- [[06_Requirements_Modeling]] — Models complement the SRS
- [[08_Prioritization_Validation_and_Reuse]] — Validate and prioritize documented requirements
- [[10_Requirements_Management]] — Managing the SRS baseline
