---
tags:
  - requirements
  - management
  - change-control
  - traceability
  - software-requirements
source: "Wiegers & Beatty, Software Requirements (Ch 27-29)"
chapter: "Ch 27: Requirements Management Practices; Ch 28: Change Happens; Ch 29: Links in the Requirements Chain"
created: 2026-07-21
aliases:
  - Requirements Management
  - Change Control
  - Requirements Baseline
  - Version Control
  - Requirement Attributes
  - Change Control Board
  - CCB
  - Impact Analysis
  - Traceability Matrix
  - RTM
---

# Requirements Management

> **From development to management.** Great requirements get you only partway to a solution — they also have to be well managed and effectively communicated. Requirements management maintains the integrity, accuracy, and currency of requirements agreements throughout the project lifecycle: version control, change control, status tracking, and traceability.

---

## 1. Requirements Management Process (Ch 27)

Requirements **development** elicits, analyzes, specifies, and validates requirements. Requirements **management** keeps those agreements intact as the project evolves. The two run concurrently: a project may be managing agreed-upon baselines while still developing other portions of the requirements.

### 1.1 Four Core Activities

| Activity | Purpose |
|----------|---------|
| **Version control** | Uniquely identify versions of individual requirements and requirements sets. |
| **Change control** | Manage how proposed changes are evaluated, approved, and incorporated. |
| **Requirements status tracking** | Monitor the lifecycle state of each requirement. |
| **Requirements tracing** | Document dependencies and links between requirements and other system elements. |

### 1.2 Process Ownership

- The **business analyst (BA)** typically leads requirements management: sets up storage, defines attributes, coordinates status/trace updates, monitors change activity.
- Process descriptions should identify the team role that owns each activity, who can modify the process, how exceptions are handled, and the escalation path.
- Document: tools/conventions for versioning, baselining, change proposal/evaluation, impact assessment, attribute definitions, status-tracking procedures, trace update responsibility, issue tracking, commitment renegotiation, and RM tool usage.

> [!warning] **Trap:** If *no one* has responsibility for requirements management, don't expect it to get done. If *"everyone"* has responsibility, each person may assume someone else is covering it — so activities get overlooked.

---

## 2. The Requirements Baseline

A **requirements baseline** is a set of requirements that stakeholders have agreed to, often defining the contents of a specific planned release or development iteration.

- After review and approval, a defined subset of requirements deliverables (business requirements, user requirements, functional/nonfunctional requirements, data dictionary, analysis models) constitutes a baseline.
- At baselining, requirements are placed under **configuration (change) management** — subsequent changes flow only through the defined change control procedure.
- Prior to baselining, requirements are still evolving — no need to impose process overhead on those modifications.
- A baseline could be: some/all requirements in an SRS, a designated set in an RM tool, or an agreed-on set of user stories for a single agile iteration.

### 2.1 Accommodating Changes to Commitments

If scope changes, the project manager must renegotiate commitments with affected managers, customers, and stakeholders. Options:

- Defer lower-priority requirements to later iterations or cut them
- Obtain additional staff or outsource some work
- Extend the delivery schedule or add iterations
- Sacrifice quality to ship by the original date

> [!important] No single approach is universally correct — projects differ in flexibility of features, staff, budget, schedule, and quality. The choice should be based on business objectives and stakeholder priorities. Accept the reality of adjusting expectations; it is better than imagining all new features will fit the original date without overruns, burnout, or quality compromises.

---

## 3. Requirements Version Control

Version control — uniquely identifying different versions of an item — applies at **both** levels: individual requirements and requirements sets (documents).

### 3.1 Principles

- Begin version control as soon as a requirement or document is drafted.
- Every version must be uniquely identified; every team member must access the current version.
- Changes must be clearly documented and communicated to all affected parties.
- Only designated individuals may update requirements; the version identifier changes on every update.
- Each circulated version should include a **revision history**: changes made, date, individual, and reason.

### 3.2 Mechanisms

| Mechanism | Description |
|-----------|-------------|
| **RM tool** (most robust) | Tracks history of every requirement; supports revert, rationale comments, attribute queries. |
| **Word processor revision marks** | Visually highlights additions (underscore) and deletions (strikethrough). On baselining: archive marked-up version, accept all revisions, store clean version as new baseline. |
| **Version control tool** (e.g., source code SCM) | Check-out/check-in; logs who/when/why; supports revert. Works for documents too. |
| **Manual labeling convention** | E.g., "Version 1.0 draft 1" → "Version 1.0 draft 2" → "Version 1.0 approved" → "Version 1.1 draft 1" (minor) or "Version 2.0 draft 1" (major). Requires manual discipline. |

> [!note] Schemes that try to differentiate document versions based on dates are prone to confusion. Use a structured version label instead.

### 3.3 "It's Not a Bug; It's a Feature!"

A contract team delivered a release and received a flood of bug reports. Investigation revealed the customer was testing against an **obsolete version of the SRS** — what testers reported as bugs truly were features. They wasted considerable time rewriting tests against the correct SRS and retesting, all because of a version control problem.

> *"We probably wasted four to six hours of effort that our department had to absorb and couldn't spend on actual billable hours. I think software professionals would be shocked if they multiplied out these wasted hours times their bill rate to see what the loss in revenue is."*

---

## 4. Requirement Attributes

Think of each requirement as an **object** with properties that distinguish it from other requirements. In addition to its textual description, each requirement should have supporting attributes that establish context and background.

### 4.1 Potential Attributes

| Attribute | Description |
|-----------|-------------|
| Date created | When the requirement was first written. |
| Current version | Version number of the requirement. |
| Author | Who wrote the requirement. |
| Priority | Relative importance. |
| Status | Lifecycle state (see §5). |
| Origin / source | Where the requirement came from. |
| Rationale | Why the requirement is included. |
| Release number | Iteration/release to which it is allocated. |
| Stakeholder contact | Person to consult with questions or change decisions. |
| Validation method | Acceptance criteria or verification approach. |

### 4.2 Using Attributes Effectively

- Store attribute values in a document, spreadsheet, database, or — most effectively — an RM tool.
- RM tools provide system-generated attributes, let you define custom ones (some auto-populated), and allow querying subsets (e.g., "all high-priority requirements assigned to Shari for release 2.3 with status Approved").
- **Rationale attribute** is powerful: if a requirement exists only because a competitor's product has the same capability, or to serve a user group marketing no longer cares about, the rationale helps people decide whether to omit it.

> [!warning] **Trap:** Selecting too many attributes can overwhelm a team — they won't supply all values and won't use the information effectively. **Start with 3–4 key attributes.** Add others only when you know how they will add value.

### 4.3 Managing Release Allocation

- Use a **Release Number** attribute to track which requirements belong to which baseline.
- Deferring a requirement = changing its planned release (just update the release number).
- Handle deleted/rejected requirements via a **status** attribute, not by removing them (preserves history).

> [!example] One company periodically generated a report showing which of 750 requirements from 3 related specifications were assigned to each designer. One designer discovered several requirements she didn't realize were hers — saving an estimated 1–2 months of rework. The larger the project, the easier it is to experience time-wasting miscommunications.

---

## 5. Tracking Requirements Status

Status tracking means comparing where you really are against the expectation of what "complete" means for this development cycle. Monitor only the functional requirements committed for the current release.

### 5.1 Suggested Requirement Statuses (Table 27-1)

| Status | Definition |
|--------|------------|
| **Proposed** | The requirement has been requested by an authorized source. |
| **In Progress** | A BA is actively crafting the requirement. |
| **Drafted** | The initial version has been written. |
| **Approved** | Analyzed, impact estimated, allocated to a baseline for a specific release; key stakeholders agreed; development committed to implement. |
| **Implemented** | Code designed, written, and unit tested; traced to design/code elements; ready for verification. |
| **Verified** | Acceptance criteria satisfied; correct functioning confirmed; traced to tests; considered complete. |
| **Deferred** | An approved requirement now planned for a later release. |
| **Deleted** | An approved requirement removed from the baseline (record why and by whom). |
| **Rejected** | Proposed but never approved; not planned for any release (record why and by whom). |

> [!tip] Some practitioners add **Designed** (design elements created and reviewed) and **Delivered** (software in users' hands for acceptance/beta testing). Keep rejected requirements available for future reference without cluttering the committed set.

### 5.2 Visualizing Status Distribution

A chart showing the **percentage of all requirements having each status value** at the end of each month illustrates how the project approaches its goal of complete verification. A body of work is done when all requirements allocated to it have status **Verified**, **Deleted**, or **Deferred**.

> [!warning] **Trap — "90 percent done" syndrome:** The first half of a software project consumes the first 90% of resources, and the second half consumes the other 90%. Overoptimistic estimation and overgenerous status tracking are a reliable formula for project overruns.

---

## 6. Resolving Requirements Issues

Numerous questions, decisions, and issues arise during a project: TBD items, pending decisions, information needed, conflicts awaiting resolution. Record issues in an **issue-tracking tool** so all affected stakeholders have access.

### 6.1 Common Requirement Issue Types (Table 27-2)

| Issue type | Description |
|------------|-------------|
| **Requirement question** | Something isn't understood or decided about a requirement. |
| **Missing requirement** | Developers uncovered a missed requirement during design or implementation. |
| **Incorrect requirement** | A requirement was wrong; correct or remove it. |
| **Implementation question** | Developers have questions about how something should work or design alternatives. |
| **Duplicate requirement** | Two or more equivalent requirements discovered; delete all but one. |
| **Unneeded requirement** | A requirement simply isn't needed anymore. |

### 6.2 Benefits of Issue Tracking

- Issues from multiple reviews are collected so nothing gets lost.
- The project manager can see the current status of all issues.
- A single owner can be assigned to each issue.
- Discussion history is retained.
- The team can begin development earlier with a known set of open issues rather than waiting until the SRS is complete.

> [!example] A stakeholder mentioned a "portal" component early on. A teammate jotted the issue on a whiteboard that was later erased instead of recording it in the tracking tool. Six months later, the executive stakeholder was furious that no one had elicited portal requirements. Recording the issue would have prevented scrambling at the last minute.

A **burndown chart** of remaining issues and the closure rate helps predict when all issues will be closed, so the team can accelerate resolution if necessary.

---

## 7. Measuring Requirements Effort

Track how much effort is spent on requirements development and management to evaluate whether it was too little, about right, or too much — and adjust future planning.

### 7.1 Requirements Development Effort

- Planning requirements-related activities
- Holding workshops, interviews, analyzing documents, elicitation
- Writing specifications, creating analysis models, prioritizing
- Creating/evaluating prototypes for requirements development
- Reviewing requirements and performing validation

### 7.2 Requirements Management Effort

- Configuring an RM tool for the project
- Submitting requirements changes and proposing new requirements
- Evaluating proposed changes (impact analysis, decisions)
- Updating the requirements repository
- Communicating changes to affected stakeholders
- Tracking and reporting requirements status
- Creating requirements trace information

> [!important] Work effort ≠ elapsed calendar time. Tasks can be interrupted or require interactions that cause delays. Total effort (labor hours) may not change, but calendar duration increases. Consider separating BA-role time from other participants' time to plan future BA effort. Compare time invested with time spent dealing with issues that arose because these activities *weren't* done — the cost of poor quality.

---

## 8. Managing Requirements on Agile Projects

Agile projects accommodate change through a series of development iterations and a **dynamic product backlog** of work remaining.

### 8.1 Agile Story Statuses

| Status | Description |
|--------|-------------|
| **In backlog** | Not yet allocated to an iteration. |
| **Defined** | Details discussed and understood; acceptance tests written. |
| **In progress** | Being implemented. |
| **Completed** | Fully implemented. |
| **Accepted** | Acceptance tests passed. |
| **Blocked** | Developer cannot proceed until something else is resolved. |

### 8.2 Iteration Burndown Chart

The team estimates total work in **story points** (proportional to implementation effort). The team allocates stories to each iteration based on priority and estimated size. The team's **velocity** (past or average story points delivered per iteration) dictates planned delivery.

- Chart story points remaining in the backlog at the end of each iteration.
- The total changes as work is completed, stories are re-estimated, new stories are added, or work is removed.
- Scope remaining can *increase* in some iterations — more functionality was added than completed.
- The slope reveals the projected end date (when no work remains).

> [!tip] The burndown chart helps avoid the "90 percent done" syndrome by making visible the work *remaining*, not the work *completed* (which doesn't reflect inevitable scope increases).

---

## 9. Change Happens (Ch 28)

> *"I'm not as far along as I'd planned to be. I'm adding a new catalog query function for Harumi, and it's taking a lot longer than I expected… I thought this would take about six hours, but I've spent almost three days on it so far."*

Uncontrolled change is a common source of project chaos, schedule slips, quality problems, and hard feelings. Developers who agree to add enhancements that users request let requirements changes slip in through the **back door** instead of being approved by the right stakeholders.

### 9.1 Why Manage Changes?

Software change isn't bad — it's necessary. The world changes as development progresses: new market opportunities, regulatory changes, evolving business needs. An organization serious about managing projects must ensure:

- Proposed changes are thoughtfully evaluated before commitment.
- Appropriate individuals make informed business decisions.
- Change activity is visible to affected stakeholders.
- Approved changes are communicated to all affected participants.
- The project incorporates changes consistently and effectively.

> [!important] Change always has a price. Revising a webpage may be quick; changing an IC design can cost tens of thousands of dollars. Even a *rejected* change request consumes time to submit, evaluate, and decide. Without change management, you won't really know what will be delivered — leading to an expectation gap.

### 9.2 Beware Subversive Changes

A vendor and customer once colluded to bypass the change process — renting a hotel room and making code changes in secret. When testers found the deliverable didn't match requirements, the whole story came out. Backtracking cost considerable time and effort. The lead customer later became a BA and apologized, having finally understood how her actions undermined the team.

### 9.3 Managing Scope Creep

Requirements **growth** includes new functionality and significant modifications presented after baselining. Software requirements typically grow **1–3% per calendar month** (Jones 2006).

**Scope creep** = the project continuously incorporates more functionality without adjusting resources, schedules, or quality goals. The problem is not that requirements change, but that late changes have a big impact on work already performed.

**Techniques to control scope creep:**

1. Document business objectives, product vision, project scope, and limitations.
2. Evaluate every proposed requirement against the business requirements.
3. Engage customers in elicitation to reduce overlooked requirements.
4. Use prototyping to share a clear understanding of needs and solutions.
5. Use short development cycles to release incrementally and adjust frequently.
6. **The most effective technique: the ability to say "no."** "Not now" is more palatable than a simple rejection — it holds the promise of a future release.

> [!warning] **Trap:** Freezing requirements too soon after initial elicitation is unwise and unrealistic. Establish a baseline when requirements are well enough defined for construction to begin, then *manage changes* to minimize adverse impact.

---

## 10. Change Control Policy

Management should communicate a policy stating expectations for how teams handle proposed changes. Policies are meaningful only if realistic, value-adding, and enforced.

### 10.1 Sample Policy Statements

- All changes must follow the process. If a change request is not submitted in accordance with this process, it will not be considered.
- No design or implementation work other than feasibility exploration will be performed on unapproved changes.
- Simply requesting a change does not guarantee it will be made. The **CCB** decides which changes to implement.
- The contents of the change database must be visible to all project stakeholders.
- **Impact analysis must be performed for every change.**
- Every incorporated change must be traceable to an approved change request.
- The rationale behind every approval or rejection must be recorded.

> [!tip] Include a "fast path" to expedite low-risk, low-investment change requests in a compressed decision cycle. No change affecting more than one individual's work should bypass your process.

---

## 11. Change Control Process

A sensible change control process lets project leaders make informed business decisions that provide the greatest customer and business value while controlling life-cycle cost and schedule. It is a **funneling and filtering mechanism**, not an obstacle.

### 11.1 Process Description Components

Every process description should include:

1. **Entry criteria** — conditions that must be satisfied before process execution begins.
2. **Tasks** — the activities, the responsible role, and other participants.
3. **Verification** — steps to confirm tasks were completed correctly.
4. **Exit criteria** — conditions indicating successful completion.

### 11.2 Roles and Responsibilities (Table 28-1)

| Role | Description |
|------|-------------|
| **CCB Chair** | Chairperson of the CCB; generally has final decision-making authority if the CCB doesn't reach agreement; identifies the Evaluator and Modifier for each request. |
| **CCB** | The group that decides to approve or reject proposed changes for a specific project. |
| **Evaluator** | Person asked by the CCB Chair to analyze the impact of a proposed change. |
| **Modifier** | Person responsible for making changes in a work product in response to an approved change request. |
| **Originator** | Person who submits a new change request. |
| **Request Receiver** | Person who initially receives newly submitted change requests. |
| **Verifier** | Person who determines whether the change was made correctly. |

> [!note] Different individuals need not be required for each role. The same person can fill several — perhaps all — roles on a small project. What matters is that the CCB representation can speak to the needs of diverse stakeholders: *Do we need it? Can we sell it? Can we build it?*

### 11.3 Change Request Lifecycle (State-Transition)

A change request passes through a defined lifecycle of states (represented via state-transition diagram). Update status only when specified transition criteria are met. For example, set state to "Change Made" only after *all* affected work products have been modified.

### 11.4 Process Tasks

1. **Evaluate change request** — assess technical feasibility, cost, alignment with business requirements and resource constraints. The CCB Chair may assign an Evaluator for impact analysis, risk/hazard analysis. Consider implications of *rejecting* the request too.
2. **Make change decision** — the CCB decides approve/reject, assigns priority or target date, allocates to a release/iteration, or adds to the product backlog. Updates status and notifies affected team members.
3. **Implement the change** — the Modifier updates affected work products. Use **trace information** to find all parts the change touches; revise trace information as needed.
4. **Verify the change** — typically through peer review to ensure modified deliverables correctly address all aspects. Multiple team members may verify via testing or review. After verification, store updated work products per project conventions.

### 11.5 Exit Criteria

- The status of the request is **Rejected**, **Closed**, or **Canceled**.
- All modified work products are updated and stored in the correct locations.
- Relevant stakeholders have been notified of the change details and request status.

### 11.6 Change Request Attributes (Table 28-2)

| Attribute | Description |
|-----------|-------------|
| Change origin | Functional area that requested the change (marketing, management, customer, development, testing). |
| Change request ID | Unique identifier. |
| Change type | Requirement change, proposed enhancement, defect report. |
| Date submitted | When the Originator submitted the request. |
| Date updated | When the request was most recently modified. |
| Description | Free-form text of the change being requested. |
| Implementation priority | Relative importance as determined by the CCB (low/medium/high). |
| Modifier | Person primarily responsible for implementing the change. |
| Originator | Person who submitted the request. |
| Originator priority | Relative importance from the Originator's point of view (low/medium/high). |
| Planned release | Product release/iteration for which an approved change is scheduled. |
| Project | Name of the project. |
| Response | Free-form text of responses (multiple over time; don't change existing responses). |
| Status | Current status from the state-transition model. |
| Title | One-line summary of the proposed change. |
| Verifier | Person responsible for determining whether the change was made correctly. |

---

## 12. The Change Control Board (CCB)

The **CCB** is the body of people — whether one individual or a diverse group — that decides which proposed changes and new requirements to accept, which to accept with revisions, and which to reject. It also decides which reported defects to correct and when.

> [!important] Projects *always* have some de facto group that makes change decisions. Establishing a CCB formalizes this group's composition, authority, and operating procedures — it is not bureaucratic overhead, but a valuable structure to help manage even a small project.

### 12.1 CCB Composition

Select representatives from:

- Project or program management
- Business analysis or product management
- Development
- Testing or quality assurance
- Marketing, the business, or customer representatives
- Technical support or help desk
- (For hardware/software projects) hardware engineering, systems engineering, manufacturing

> [!tip] Keep the CCB **small** so it can respond promptly and efficiently. Invite other individuals to meetings as necessary for technical/business information. Make sure members understand and accept their responsibilities.

### 12.2 CCB Charter

Each project should create a brief charter (possibly part of the project management plan) describing:

- Purpose
- Scope of authority
- Membership
- Operating procedures
- Decision-making process
- Frequency of regularly scheduled meetings
- Conditions that trigger a special meeting or decision
- Which decisions the CCB can make vs. which must be escalated

### 12.3 Making Decisions

Each CCB defines its decision-making process, indicating:

- The number of members or key roles that constitute a quorum.
- The decision rules to be used.
- Whether the CCB Chair can overrule the collective decision.
- Whether a higher level of CCB or management must ratify the decision.

The CCB balances **anticipated benefits** (financial savings, increased revenue, higher customer satisfaction, competitive advantage) against **estimated impact** (increased development/support costs, delayed delivery, degraded quality).

> [!warning] **Trap:** Because people don't like to say "no," it's easy to accumulate a huge backlog of approved change requests that will never get done. Before accepting a proposed change, make sure you understand the rationale and the business value.

### 12.4 Multi-Level CCBs

Very large projects or programs may have several levels of CCBs:

- **Program-level CCB** — handles issues affecting multiple projects and changes exceeding specified cost/schedule impact.
- **Project-level CCBs** — resolve issues and changes affecting only that project.

### 12.5 Communicating Status & Renegotiating Commitments

After a decision, a designated individual updates the request's status in the change database. Some tools auto-generate email to the Originator and affected parties.

> [!important] Stakeholders can't stuff more and more functionality into a project with schedule, staff, budget, or quality constraints and still expect to succeed. Before accepting a significant change, **renegotiate commitments** with management and customers — ask for more time or defer lower-priority requirements. If you don't obtain adjustments, document threats to success in the project's risk list.

---

## 13. Measuring Change Activity

Measuring change activity assesses the **stability** of requirements and reveals opportunities for process improvements. Consider tracking:

- Total number of change requests received, currently open, and closed.
- Cumulative number of added, deleted, and modified requirements.
- Number of requests from each change origin.
- Number of changes received against each requirement since baselining.
- Total effort devoted to processing and implementing change requests.

### 13.1 Requirements Volatility Chart

Tracks the rate at which new proposals for requirements changes arrive after a baseline was established.

- **Should trend toward zero** as you approach release.
- A sustained high frequency implies risk of failing schedule commitments and likely indicates the original requirements set was incomplete — better elicitation practices may be in order.

### 13.2 Change Origins Chart

Shows the number of change requests from different sources. Discussing this data constructively (e.g., "marketing has requested the most changes") can lead to actions to reduce future changes. Using data as a starting point is more constructive than a confrontational debate fueled by emotion.

---

## 14. Change Impact Analysis

> *"Skipping impact analysis doesn't change the size of the task. It just turns the size into a surprise. Software surprises are rarely good news."*

**Impact analysis** provides an accurate understanding of the implications of a proposed change, helping the team make informed business decisions. It examines the request to identify components that might have to be created, modified, or discarded, and estimates the effort required.

> [!example] A company had to change the text of one error message. In English, no problem. In German, the new message exceeded the maximum character length for both the message box and a database. A seemingly simple change turned out to be far more work than anticipated.

### 14.1 Three-Step Procedure

1. **Understand possible implications** — a requirement change often produces a large ripple effect: modifications to other requirements, architectures, designs, code, and tests. Changes can conflict with other requirements or compromise quality attributes (performance, security).
2. **Identify all affected work products** — requirements, files, models, and documents that might need modification. Requirements trace information helps greatly here.
3. **Identify tasks and estimate effort** — identify the tasks required to implement the change and estimate the effort for each.

### 14.2 Detailed Steps

1. Work through the implications checklist (Figure 28-5).
2. Work through the affected-work-products checklist (Figure 28-6). Some RM tools include an impact analysis report that follows traceability links.
3. Use the effort-estimation worksheet (Figure 28-7). Most changes need only a portion of the listed tasks.
4. Sum the effort estimates.
5. Identify the sequence in which tasks must be performed and how they interleave with currently planned tasks.
6. Estimate the impact on project schedule and cost.
7. Evaluate the change's priority compared to other pending requirements.
8. Report the impact analysis results to the CCB.

> [!tip] For substantial changes, use a **small team** — not just one developer — to do the analysis and effort estimation to avoid overlooking important tasks. The procedure shouldn't take more than a couple of hours for a single change request — a small investment to ensure the project wisely invests its limited resources. Compare actual effort with estimated effort to improve future analyses.

### 14.3 Money Down the Drain

Two developers estimated 4 weeks for an enhancement. After 2 months, it was only half done and the customer canceled it: *"If I'd known how long this was really going to take and how much it was going to cost, I wouldn't have approved it."* In the rush to begin implementation, the developers didn't do enough impact analysis — wasting several hundred hours of work that a few hours of analysis could have avoided.

---

## 15. Change Management on Agile Projects

> *"Welcome changing requirements, even late in development. Agile processes harness change for the customer's competitive advantage."* — Agile Manifesto Principle

Agile projects are structured to respond to — and even welcome — scope changes. They manage change by maintaining a **dynamic backlog** of work: user stories, defects, business process changes, training, and other activities.

### 15.1 Iteration Scope Management

- Each iteration implements the highest-priority work items at that time.
- New work goes into the backlog and is prioritized against existing contents.
- A new, high-priority story allocated to the forthcoming iteration may force a lower-priority story of similar size to be deferred.
- Carefully managing each iteration's scope ensures on-time, high-quality completion.

### 15.2 Freeze or Adjust?

| Approach | Benefit |
|----------|---------|
| **Freeze iteration contents** while underway | Stability for developers; predictability for stakeholders. |
| **Adjust iteration contents** | More responsive to customer needs. |

> [!note] There is no single "correct" approach. The basic principle is to avoid both **excessive change** (churning requirements) and **excessive rigidity** (frozen requirements) within an iteration. If changes need to be introduced too often, the standard iteration length may need to be shortened.

### 15.3 Agile Change Authority

- In **Scrum**, the **product owner** has primary responsibility for prioritizing the backlog and accepts proposed changes based on alignment with the product vision and business value.
- In **Extreme Programming**, the **customer** role serves this function.
- The agile team is already configured like a CCB (collaborative, cross-functional: developers, testers, BA, PM, others).
- Short iterations and small increments allow frequent but limited-scale change control.
- Scope changes affecting overall cost or duration must be escalated to a higher-level change authority (e.g., project sponsor).

> [!important] The purpose of change control is **not** to inhibit change or stakeholder proposals. It is to provide visibility into change activity and mechanisms by which the right people can consider proposed changes and incorporate appropriate ones at the right time — maximizing business value and minimizing negative impact.

---

## 16. Links in the Requirements Chain (Ch 29)

> *"The logic for the seniority rules is sprinkled throughout the scheduling system. I can't give you a decent estimate yet. It's going to take hours just to scan through the code and try to find all the places where those rules show up."*

**Requirements tracing** (traceability) documents the dependencies and logical links between individual requirements and other system elements: other requirements, business rules, architecture/design components, source code modules, tests, and help files. Trace information facilitates **impact analysis** by identifying all work products that might need modification.

### 16.1 Traceability vs. Traced

- **Traceable** = having the properties to facilitate tracing (each requirement uniquely and persistently labeled; fine-grained rather than large paragraphs).
- **Traced** = actually having logical links between requirements and other elements recorded.

> [!important] For requirements to be traceable, each one must be **uniquely and persistently labeled** so it can be referred to unambiguously throughout the project. Write requirements in a fine-grained fashion.

### 16.2 Four Types of Trace Links (Figure 29-1)

```
Customer Needs  ←──────→  Requirements  ←──────→  System Elements
(business objectives,      (functional &           (design, code,
 user requirements)          nonfunctional)          tests, help files)
```

| Direction | From → To | Purpose |
|-----------|-----------|---------|
| **Forward** (top) | Customer needs → Requirements | Know which requirements are affected if needs change; confirm all needs are addressed. |
| **Backward** (top) | Requirements → Customer needs | Identify the origin of each software requirement. |
| **Forward** (bottom) | Requirements → System elements | Confirm every requirement is satisfied (know which design/code/test elements address each). |
| **Backward** (bottom) | System elements → Requirements | Know *why* each element was created (detect orphan code / gold-plating). |

> [!tip] If use cases represent customer needs, the top half traces between use cases and functional requirements. Tracing tests *back* to requirements provides a mechanism for detecting unimplemented requirements — the expected functionality will be missing from the system being tested.

---

## 17. Motivations for Tracing Requirements

Requirements tracing is an **investment** that increases chances of delivering a maintainable product satisfying all stated customer requirements. It pays dividends whenever you modify, extend, or replace the product.

| Motivation | Description |
|------------|-------------|
| **Finding missing requirements** | Business requirements that don't trace to any user requirements; user requirements that don't trace to any functional requirements. |
| **Finding unnecessary requirements** | Functional requirements that don't trace back to user or business requirements — might not be needed. |
| **Certification & compliance** | Demonstrate all requirements were implemented for safety-critical products; show regulatory compliance (health care, financial services). |
| **Change impact analysis** | Without trace information, you'll likely overlook a system element affected by adding, deleting, or modifying a requirement. |
| **Maintenance** | When corporate policies or government regulations change, a table showing where each business rule was addressed makes changes easier and more reliable. |
| **Project tracking** | Trace data recorded during development gives an accurate record of implementation status; absent links indicate work products not yet created. |
| **Reengineering** | List functions in an existing system being replaced and trace them to where they're addressed in the new system. |
| **Reuse** | Identify packages of related requirements, designs, code, and tests for reuse. |
| **Testing** | When a test fails, links between tests, requirements, and code point developers toward likely areas to examine for the defect. |

> [!warning] Keeping trace information current takes discipline and time. If it becomes obsolete, you'll probably never reconstruct it — and obsolete/inaccurate trace data wastes time by sending developers down the wrong path, destroying trust. Adopt tracing for the **right reasons**, and perform a cost-benefit analysis to decide which links contribute to project success. Don't ask team members to record information unless you know how they can use it.

---

## 18. The Requirements Traceability Matrix (RTM)

The most common way to represent links between requirements and other system elements. Also called a **requirements trace matrix** or **traceability table**. A related tool, the **requirements mapping matrix** (Beatty & Chen, 2012), shows relationships between multiple types of objects.

### 18.1 Sample RTM (Table 29-1)

| User requirement | Functional requirement | Design element | Code element | Test |
|------------------|------------------------|----------------|--------------|------|
| UC-28 | catalog.query.sort | Class `catalog` | `CatalogSort()` | search.7, search.8 |
| UC-29 | catalog.query.import | Class `catalog` | `CatalogImport()`, `CatalogValidate()` | search.12, search.13, search.14 |

- Each functional requirement links **backward** to a specific use case and **forward** to one or more design, code, and test elements.
- **Design elements** can be architectural components, data model tables, or object classes.
- **Code references** can be class methods, stored procedures, source file names, or modules within a source file.
- More detail = more work, but gives precise locations of related software elements.

> [!important] **Fill in information as work gets done, not as it gets planned.** Enter `CatalogSort()` in the "Code element" column only when that code has been written. Populated cells indicate work that's been completed. Listing test cases for each requirement does *not* indicate the software passed those tests — only that tests were written to verify the requirement. Tracking testing status is a separate matter.

### 18.2 Two-Way Traceability Matrix (Table 29-2)

An alternative format defines links between **pairs** of system elements (e.g., use cases × functional requirements). Each cell at the intersection of two linked components contains a symbol indicating the connection.

| Functional requirement | UC-1 | UC-2 | UC-3 | UC-4 |
|-------------------------|------|------|------|------|
| FR-1 | | | | |
| FR-2 | → | | | |
| FR-3 | | | | |
| FR-4 | | | | |
| FR-5 | | → | | → |
| FR-6 | | | | |

> [!example] FR-5 is traced from both UC-2 and UC-4, indicating the functional requirement is **reused** across two use cases. Possible relationships include "specifies/is specified by," "is dependent on," "is parent of," and "is constrains/is constrained by."

### 18.3 Trace Link Cardinalities

| Cardinality | Example |
|-------------|---------|
| **One-to-one** | One design element is implemented in one code module. |
| **One-to-many** | One functional requirement is verified by multiple tests. |
| **Many-to-many** | Multiple requirements traced to multiple design elements. |

The Table 29-1 format accommodates these by allowing several items in each table cell.

> [!tip] You don't need to define and manage *all* possible trace link types. On many projects, you can gain most traceability benefits for a fraction of the potential effort — maybe you only need to trace system tests back to functional requirements or user requirements. Establishing traces is not much work if you collect information as development proceeds, but it's tedious and expensive to do on a completed system.

---

## 19. Key Takeaways

> [!summary] **Requirements management is an essential activity** whether your project follows a sequential, agile, or hybrid lifecycle. It ensures the effort invested in requirements development isn't squandered and reduces the expectation gap by keeping all stakeholders informed about the current state of requirements throughout development.

- **Baselining** freezes an agreed-upon set of requirements under change management; changes flow only through the defined process.
- **Version control** applies at both individual requirement and document levels — begin as soon as you draft.
- **Requirement attributes** provide context; start with 3–4 key attributes and add more only when value is clear.
- **Status tracking** replaces "90% done" guessing with precise, categorical progress measurement.
- **Change control** is a funneling/filtering mechanism — not an obstacle. The CCB makes informed business decisions balancing benefits against impact.
- **Impact analysis** is mandatory for every change: understand implications, identify affected work products, estimate effort. Skipping it turns size into a surprise.
- **Traceability** documents the links between requirements and all downstream elements — the road map that makes impact analysis, maintenance, compliance, and reuse possible.
- **Agile projects** manage change through a dynamic backlog, short iterations, and a product owner/customer role as change authority — but still must evaluate cost and escalate scope-affecting changes.

---

## 20. Next Steps

- [ ] Document the processes your organization will follow to manage requirements on each project. Engage several BAs to draft, review, pilot, and approve.
- [ ] If not using an RM tool, define a version labeling scheme and educate BAs.
- [ ] Select the statuses to describe your functional requirements/user stories lifecycle. Draw a state-transition diagram showing transition conditions.
- [ ] Define the current status for each requirement in your baseline; keep it current.
- [ ] Identify decision makers and set them up as a CCB; have the CCB adopt a charter.
- [ ] Define a state-transition diagram for change request lifecycle; write a process to handle proposed changes.
- [ ] Select an issue-tracking tool compatible with your environment; tailor it to your process.
- [ ] Next time you evaluate a change request, estimate effort with your old method *and* the impact analysis approach; compare both to actual effort and refine your checklists/worksheet.

---

## Related Notes

- [[02_Business_and_User_Requirements]] — Business requirements, vision/scope, user classes
- [[Software Requirements Overview]] — Top-level overview of the requirements domain

---

*Source: Wiegers, K. & Beatty, J. (2013). Software Requirements (3rd ed.). Microsoft Press. Chapters 27–29.*
