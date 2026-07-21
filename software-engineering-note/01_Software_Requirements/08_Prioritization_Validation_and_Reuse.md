---
tags:
  - requirements
  - prioritization
  - validation
  - reuse
  - software-requirements
---

# Prioritization, Validation & Reuse

> **Source:** Karl Wiegers & Joy Beatty, *Software Requirements* (3rd ed.), Chapters 16–19
> **Theme:** Setting requirement priorities, validating the requirements, requirements reuse, and estimating.

---

## 1. Setting Requirement Priorities (Ch 16)

### 1.1 Why Prioritize?

- Few projects deliver **all** capabilities every stakeholder wants by the target date. Every resource-limited project must define relative priorities.
- Prioritization (also called **requirements triage**) reveals competing goals, resolves conflicts, plans staged/incremental deliveries, controls scope creep, and drives trade-off decisions.
- Establish priorities **early** — when you still have flexibility — and revisit them periodically. Prioritization is dynamic and ongoing.
- Customers sometimes resist prioritizing, fearing low-priority items will never be built. The reality: if you can't get everything, make sure you get what matters most to business objectives.
- If customers won't distinguish priorities, the project manager must — and customers may not agree with the result.
- **Warning sign:** if ~85% of requirements end up "high priority," you haven't actually prioritized. Scrub the requirements to eliminate non-essential ones and simplify unnecessarily complex ones. Studies show nearly **two-thirds of features are rarely or never used** (Standish Group 2009).

### 1.2 Prioritization Pragmatics

- Choose an appropriate **level of abstraction**: features, use cases, user stories, or functional requirements. Don't mix levels.
- Initial prioritization at feature level, then prioritize functional requirements within features.
- Various stakeholders should participate: customers, sponsors, PM, development. Need **one ultimate decision maker** when stakeholders disagree.
- Agree on **criteria** for judging priority: customer value, business value, business/technical risk, cost, difficulty, time to market, regulatory compliance, competitive advantage, contractual commitments.
- Alan Davis's six issues for successful prioritization:
  1. Customer needs
  2. Relative importance to customers
  3. Timing of delivery
  4. Predecessor/dependency relationships among requirements
  5. Requirements that must be implemented as a group
  6. Cost to satisfy each requirement

### 1.3 Games People Play with Priorities

- **"I need all of these"** — knee-jerk refusal to prioritize. Explain that not everything can be done simultaneously; the goal is to work on the right things first.
- **Inflated categories** — one company used "high," "super-high," and "incredibly high" because "low priority" was politically unacceptable.
- **Single-release trap** — if stakeholders believe there's only one release, everything becomes high priority, overloading capacity.
- Avoid **decibel prioritization** (loudest voice wins) and **threat prioritization** (most political power wins).

### 1.4 Prioritization Techniques

#### In or Out
- Simplest method: binary decision per requirement — is it in or out for this release?
- Refer to business objectives; pare to bare minimum for first release.
- Revisit "out" items for the next release.

#### Pairwise Comparison & Rank Ordering
- Assign unique priority sequence numbers via pairwise comparisons between all requirements.
- Becomes unwieldy beyond ~two dozen items.
- Rank ordering everything is **overkill** — group requirements into batches by release/timebox instead.

#### Three-Level Scale (High / Medium / Low)
- Based on two dimensions: **importance** and **urgency** (Covey 2004):

| Quadrant | Importance | Urgency | Priority |
|----------|-----------|---------|----------|
| 1 | Important | Urgent | **High** — must be in next release |
| 2 | Important | Not urgent | **Medium** — can wait for later release |
| 3 | Not important | Not urgent | **Low** — can wait, perhaps forever |
| 4 | Not important | Urgent (political) | **Drop** — doesn't add sufficient value |

- High priority = if you can wait without adverse consequences, it's **not** high priority.
- Include priority as a **requirement attribute** in the SRS or requirements database.
- **Multipass prioritization:** if too many "high," partition into high/higher/highest. "Highest" becomes the new top tier; "high" and "higher" merge with the original medium group.
- Watch for **dependencies**: a high-priority requirement depending on a lower-priority one creates problems.

#### MoSCoW
- Four categories (IIBA 2009):
  - **Must** — required for solution success
  - **Should** — important but not mandatory
  - **Could** — desirable, deferrable
  - **Won't** — not implemented now, possibly in future
- **Wiegers does not recommend MoSCoW.** Ambiguous on timing (especially "Won't" — next release or never?). No rationale for assigning ratings. In practice, everyone games it to get "Must."
- The three-level importance/urgency scale is crisper.

#### $100 Test (Play Money)
- Give the team 100 imaginary dollars to "buy" requirements they want implemented.
- Weight higher-priority items with more dollars. If requirement A is 3× as important as B, allocate e.g. $9 to A and $3 to B.
- Can have multiple participants allocate independently, then sum dollars per requirement.
- **Limitations:** participants can game it (put all $100 on one item); ignores effort/cost differences. Based solely on perceived value.
- **Objective chain technique** (Beatty & Chen 2012): assign estimated **real dollar value** each feature contributes to business objectives; compare relative value to select implementation order.

#### Value / Cost / Risk Spreadsheet (QFD-derived)
- A rigorous analytical method adapted from **Quality Function Deployment (QFD)**.
- Ranked in top tier of effectiveness across 17 prioritization methods (Kukreja et al. 2012).
- Feature attractiveness ∝ value provided; inversely ∝ cost and technical risk.
- **Priority formula:**

```
         value %
priority = ─────────────
          cost % + risk %
```

- **Inputs (each rated 1–9):**
  - **Relative benefit** — value to customer/business if feature is present (customer reps)
  - **Relative penalty** — downside if feature is absent (customer reps)
  - **Relative cost** — implementation effort (developers)
  - **Relative technical risk** — probability of not getting it right first try (developers)
- Total value = (benefit × weight) + (penalty × weight); spreadsheet computes % of total value, cost, risk.
- Apply to **discretionary requirements** only — not core functions, key differentiators, or regulatory items that absolutely must ship.
- **Weighting factors** adjustable (e.g., benefit 2×, penalty 1, cost 1, risk 0.5). Set weight to 0 to drop a term.
- **Process:**
  1. List all items at the same abstraction level in the spreadsheet.
  2. Customer reps estimate benefit (1–9) and penalty (1–9) per feature.
  3. Developers estimate cost (1–9) and technical risk (1–9).
  4. Spreadsheet calculates value %, cost %, risk %, then priority.
  5. Sort descending by priority — top items have best value/cost/risk balance.
- Supports **multiple stakeholders**: duplicate benefit/penalty columns per stakeholder group, assign weighting factors to favor influential user classes.
- Can also evaluate **proposed additions** against the existing baseline.
- **Caveat:** semi-quantitative, not mathematically rigorous. Don't over-interpret small differences — group items with approximately equal priority numbers.
- Use calculated priorities as a **guideline**; the framework of benefit/penalty/cost/risk is often more valuable for discussion than the final number.

---

## 2. Validating the Requirements (Ch 17)

### 2.1 Why Validate?

- Cost of fixing a requirement defect grows enormously downstream: ~30 minutes if found during requirements, vs. 5–17 hours if found during system testing (Kelly, Sherif, Hops 1992).
- Start **test planning and test-case development in parallel** with requirements development — this catches errors shortly after introduction.
- The **V model** shows test activities beginning in parallel with corresponding development activities:
  - Acceptance tests ← user requirements
  - System tests ← functional requirements
  - Integration tests ← system architecture
- Investing in requirements quality **shortens** delivery schedule (reduces rework, accelerates integration/testing), contrary to the intuition that it adds delay.

### 2.2 Validation vs. Verification

| | Verification | Validation |
|---|---|---|
| Question | Did we write the requirements **right**? | Did we write the **right** requirements? |
| Checks | Desired quality properties (Ch 11) | Trace back to business objectives |
| Meaning | "Doing the thing right" | "Doing the right thing" |

- The two are intertwined. This chapter's techniques contribute to both.

### 2.3 What Validation Ensures

- Requirements accurately describe intended system capabilities/properties satisfying stakeholder needs.
- Correctly derived from business requirements, system requirements, business rules, sources.
- Complete, feasible, verifiable.
- All necessary; the set is sufficient to meet business objectives.
- All representations consistent with each other.
- Adequate basis to proceed with design and construction.
- Validation is **not** a single discrete phase — it's threaded throughout iterative elicitation/analysis/specification.

### 2.4 Reviewing Requirements (Peer Reviews)

#### Informal Reviews
- **Peer deskcheck** — one colleague looks over the work product.
- **Passaround** — several colleagues examine concurrently.
- **Walkthrough** — author describes deliverable, solicits comments.
- Good for glaring errors, inconsistencies, gaps. Hard for one reviewer to catch all ambiguities alone.

#### Formal Peer Reviews / Inspections
- Well-defined process; produces a report identifying material, reviewers, and judgment on acceptability.
- **Inspection** (Fagan 1976) is the best-established formal peer review — one of the highest-leverage software quality techniques.
- Companies have avoided up to **10 hours of labor for every hour invested** in inspecting requirements (Grady & Van Slack 1994) — a 1,000% ROI.
- Use **risk analysis** to decide what to inspect formally vs. review informally when time is limited.

### 2.5 The Inspection Process

#### Participants (four perspectives)
1. **Author** (+ experienced BA peers) — knows what requirements-writing errors to look for.
2. **Information sources** — user reps, authors of predecessor specs, product champions.
3. **Downstream consumers** — developer (spots infeasible requirements), tester (spots unverifiable requirements), PM, doc writer.
4. **Interfacing system owners** — spot external interface problems and ripple effects.
- Limit to **≤7 inspectors**. Author's manager normally should not attend.

#### Inspection Roles
- **Author** — passive; listens, responds to (but doesn't debate) questions, thinks. Should not hold other roles.
- **Moderator** — plans with author, distributes materials, facilitates meeting, keeps focus on major defects (not problem-solving or style), follows up on changes.
- **Reader** — paraphrases each requirement in own words during meeting; reveals ambiguities/assumptions via differing interpretations.
- **Recorder** — documents issues/defects on standard forms; confirms accuracy with team.

#### Entry Criteria
- Document conforms to standard template; no obvious spelling/grammar/formatting issues.
- Line numbers or unique identifiers present.
- Open issues marked TBD or tracked in issue tool.
- Moderator found ≤3 major defects in a 10-minute sample examination.

#### Inspection Stages
1. **Planning** — author + moderator decide participants, materials, meeting time. **Inspection rate:** 2–4 pages/hour practical guideline (optimum for max defect detection ≈ half that). Adjust by team data, text density, complexity, criticality, author experience.
2. **Preparation** — author shares context; each inspector examines individually using defect checklists. **Up to 75% of defects found during preparation** (Humphrey 1989). Spend ≥ half the meeting time on preparation. *Don't hold the meeting if participants haven't prepared.*
3. **Inspection meeting** — reader leads through document; recorder captures defects. Meeting ≤2 hours (tired inspectors are ineffective). Outcome: accept as-is / accept with minor revisions / major revision needed. Consider retrospective if "major revision" — may indicate process shortcomings.
4. **Rework** — author resolves ambiguities, eliminates fuzziness.
5. **Follow-up** — moderator/designee verifies all issues resolved, errors corrected, exit criteria satisfied. May reveal incomplete fixes → additional rework.

#### Exit Criteria
- All issues addressed.
- Changes made correctly.
- Open issues resolved, or each has documented resolution process, target date, owner.

### 2.6 Defect Checklist
- Develop a checklist per requirements document type — focuses reviewers on historically frequent problems.
- Keep lists short (≤6–8 items) — reviewers won't use long lists effectively.
- **Scenario-based review** (assigning specific defect-detection responsibilities) is more effective than handing everyone the same checklist (Porter, Votta, Basili 1995).

### 2.7 Requirements Review Tips
- **Plan the examination** — don't read front-to-back; assign specific sections to specific reviewers.
- **Start early** — review when ~10% complete, not when "done." Catches systemic problems early.
- **Allocate sufficient time** — both effort hours and calendar time.
- **Provide context** — give reviewers document/project context; seek reviewers with useful perspectives.
- **Set review scope** — tell reviewers what to examine and what issues to look for; use checklists.
- **Limit re-reviews** — don't ask anyone to review the same material >3 times (reviewer fatigue). Highlight changes for re-reviews.
- **Prioritize review areas** — focus on high-risk, frequently-used functionality and areas with few logged issues (may mean unreviewed, not problem-free).

### 2.8 Review Challenges & Mitigations

| Challenge | Mitigation |
|-----------|------------|
| Large documents | Incremental reviews; inspect high-risk, informally review rest; start reviewers at different locations; sample to judge whether full inspection pays off |
| Large inspection teams | Ensure everyone is there to find defects (not be educated/protect position); pool same-perspective reps; use parallel small teams (research shows additive defect finding) |
| Geographically separated reviewers | Videoconferencing, web conferencing, shared repositories with inline comments, async tool-based reviews (less effective than meetings but better than nothing) |
| Unprepared reviewers | Mandatory preparation; don't proceed without it; boredom in unprepared meetings triggers future prep |

### 2.9 Prototyping Requirements (Validation Tool)
- Prototypes make requirements real — let users experience aspects of the system.
- **Paper mock-ups** can walk through use cases/processes to detect omitted/erroneous requirements.
- **Proof-of-concept** prototypes demonstrate feasibility.
- **Evolutionary prototypes** let users see how requirements work when implemented.
- Prototypes confirm shared understanding — if evaluators disagree with the implementer's interpretation, the requirement wasn't clear.
- See [[07_Risk_Reduction_through_Prototyping]] for full prototyping treatment.

### 2.10 Testing the Requirements

- Designing tests from requirements reveals problems **long before** code exists. Vague requirements jump out when you can't describe expected system response.
- BA writes functional requirements; tester writes tests — both from common source (user requirements). Inconsistencies reveal interpretation gaps.
- **Conceptual tests** are implementation-independent. Cover normal flows, alternative flows, exceptions of each use case; business process steps and decision paths.
- Example conceptual tests for "View a Stored Order":
  - Order exists, user placed it → show order details
  - Order doesn't exist → "Sorry, I can't find that order."
  - Order exists, different user placed it → "Sorry, that's not your order."
- **Tracing tests onto dialog maps** with a highlighter reveals:
  - Navigation lines never exercised → either remove (not permitted) or add missing test
  - Tests that can't execute → either test is wrong or requirement/dialog map is missing
- Use cases and tests are mutually debugging: complete use cases → straightforward test derivation; incomplete use cases → test derivation debugs the use cases (Collard 1999).

### 2.11 Acceptance Criteria

- The **customer is the final arbiter** — acceptance criteria define minimum conditions for "business-ready."
- Shift in perspective: from "What do you need?" to "How would you judge whether the solution meets your needs?"
- Use **SMART** mnemonic: Specific, Measurable, Attainable, Relevant, Time-sensitive.
- Criteria should let multiple objective observers reach the same conclusion about satisfaction.
- **Beyond "all requirements implemented / all tests passed"** — acceptance criteria may include:
  - Specific high-priority functionality that must be present and operating
  - Essential nonfunctional criteria / quality metrics (e.g., minimum uptime without failure)
  - Remaining open issues/defects (no defects above severity X against high-priority requirements)
  - Legal/regulatory/contractual conditions fully satisfied
  - Supporting transition/infrastructure/project requirements (training, data conversion)
- Consider **rejection criteria** — conditions that would lead stakeholders to deem the system not ready.
- Watch for **conflicting acceptance criteria** — finding them early surfaces conflicting requirements.
- **Agile:** acceptance criteria are *conditions of satisfaction* on the system (not functional/unit tests). If all criteria for a user story are met, the product owner accepts the story. Customers must be specific.
- **Risk of acceptance-tests-only agile:** requirements exist only in people's minds — you lose the cross-view error detection (requirements vs. models vs. tests).

#### Acceptance Tests
- Largest portion of acceptance criteria. Focus on most common/important usage scenarios: normal flows + key exceptions; less attention to infrequent alternative flows.
- Automate execution whenever possible — eases retesting after changes.
- Must address **nonfunctional requirements**: performance, usability, security.
- **UAT (User Acceptance Testing):** performed by users after functionality believed release-ready; uses plausible test data; selects highest-risk areas; lets users familiarize themselves before delivery.
- **UAT does not replace** comprehensive requirements-based system testing.

---

## 3. Requirements Reuse (Ch 18)

### 3.1 Why Reuse Requirements?

- Reuse = taking advantage of previously done work. Simplest: copy-paste a requirement. Most sophisticated: reuse an entire functional component (requirements → design → code → tests).
- **Benefits:** faster delivery, lower costs, consistency within/across applications, higher productivity, fewer defects, reduced rework, accelerated approval cycle, improved estimation (if data from prior implementation exists).
- **From user perspective:** functional consistency across product line members or business apps — minimizes learning curve and frustration.
- **Reuse is not free.** Creating high-quality reusable requirements takes more time than writing single-project requirements. Requires infrastructure and a culture that values reuse.
- Only ~half of organizations practice requirements reuse, primarily due to poor quality of existing requirements (Chernak 2012).

### 3.2 Three Dimensions of Reuse

| Dimension | Options |
|-----------|---------|
| **Extent of reuse** | Single requirement statement → + attributes → + context/associated info (data defs, glossary, tests, assumptions, constraints, business rules) → set of related requirements → + design elements → + design, code, test elements |
| **Extent of modification** | None → attributes only (priority, rationale, origin) → requirement statement itself → related info (tests, design constraints, data defs) |
| **Reuse mechanism** | Copy-and-paste from another spec → copy from reusable requirements library → refer to original source |

### 3.3 Reuse Mechanisms — Notes

- **Copy-and-paste** increases spec size (duplication); introduces context problems when context isn't carried with the paste. A warning bell should ring if you're populating a spec mostly by copy-paste.
- **Cross-referencing** (word processor feature) links copies back to a master instance; changes echo to all linked locations — but risks unwanted propagation if someone else alters the master.
- **Copy-by-reference (pointer)** stores only an identifier/hyperlink to the master source in a shared collection (file, spreadsheet, HTML/XML, database, requirements tool). Reader follows link to view master. Always current if collection maintained.
- **Requirements management tool** is the most effective reuse-by-reference mechanism — can reuse a requirement without replicating it, retain historical versions, and let projects tailor their own version without disrupting other reusers.

### 3.4 Types of Reusable Requirements Information

| Scope | Potentially Reusable Assets |
|-------|----------------------------|
| **Within a product/app** | User requirements, specific functional requirements within use cases, performance requirements, usability requirements, business rules |
| **Across a product line** | Business objectives, business rules, process models, context diagrams, ecosystem maps, user requirements, core features, stakeholder/user profiles, personas, usability/security/compliance/certification requirements, data models, acceptance tests, glossary |
| **Across an enterprise** | Business rules, stakeholder profiles, user class descriptions, personas, glossary, security requirements |
| **Across a business domain** | Business process models, product features, user requirements, user class descriptions, personas, acceptance tests, glossary, data models, business rules, security/compliance requirements |
| **Within an operating environment/platform** | Constraints, interfaces, infrastructure functionality (e.g., report generator) |

- **Sets of related requirements** offer more reuse value than isolated requirements. Examples:
  - Functionality + exceptions + acceptance tests
  - Data objects + attributes + validations
  - Compliance business rules (Sarbanes-Oxley, HIPAA, industry regulations)
  - Symmetrical functions (undo/redo)
  - CRUD operations on data objects
- **Security** is a prime reuse candidate — no reason every project should reinvent logon/authentication/password-reset requirements.

### 3.5 Common Reuse Scenarios

#### Software Product Lines
- Family of products with common functionality. Analyze features as:
  - **Common** — appear in all members (greatest reuse opportunity: requirements + design + code + tests)
  - **Optional** — appear in some members
  - **Variants** — different versions across members
- Watch for platform/hardware constraints that may limit reuse to requirements only (not designs/code).

#### Reengineered & Replacement Systems
- Always reuse some requirements from the original (even if never written down).
- Reverse-engineering may require moving to **higher abstraction** to escape old implementation characteristics.
- Harvest embedded business rules (regulatory/compliance) for reuse and updating.
- **Trap:** don't reuse too much of an old system — you'll miss opportunities offered by new platforms, architectures, and workflows.

#### Other Reuse Opportunities

| Opportunity | Examples |
|-------------|----------|
| Business processes | Common processes across organizations; reusable process descriptions |
| Distributed deployments | Same system deployed multiple times with slight variations (retail stores, warehouses) |
| Interfaces & integration | Hospital ancillary systems ↔ ADT system; financial interfaces to ERP |
| Security | Common single sign-on via Active Directory; authentication requirements |
| Common application features | Search, printing, file operations, user profiles, undo/redo, text formatting |
| Similar products, multiple platforms | Mac + Windows; iOS + Android — same core requirements, different UI/platform details |
| Standards & legal compliance | ADA Standards for Accessible Design; HIPAA privacy rules |

### 3.6 Requirement Patterns

- Pioneered by Stephen Withall (2007) — a **requirement pattern** packages knowledge about a particular type of requirement into a convenient template.
- A pattern defines content categories for common requirement types; populating the template yields more detailed specs than natural language alone.
- Pattern sections:
  1. **Guidance** — basic details, related patterns, applicability, approach
  2. **Content** — detailed explanation of what such a requirement ought to convey, item by item
- Structure and content facilitate reuse.

---

## 4. Estimating (Ch 19 — Beyond Requirements Development)

- Referenced throughout Ch 16 as the source of **cost ratings** for the value/cost/risk prioritization spreadsheet.
- Agile teams can base cost ratings on **story points** assigned to user stories.
- Developers estimate cost from: complexity, UI work extent, reuse potential, testing effort.
- Estimation quality improves when you have data from implementing the same requirements on a previous project — a direct benefit of requirements reuse.
- Chapter 19 covers estimation in the broader context of requirements-driven project activities beyond requirements development itself.

---

## 5. Key Takeaways

- **Prioritize early and often.** If everything is high priority, nothing is. Use the importance/urgency two-dimensional scale; reserve the value/cost/risk spreadsheet for contentious, discretionary requirements.
- **Validate continuously.** Thread reviews, inspections, prototyping, and conceptual testing throughout requirements development — not as a final gate. The V model makes test planning a parallel activity, not a late one.
- **Inspections are high-leverage.** Up to 10× ROI; preparation finds ~75% of defects; limit teams to ≤7; keep meetings ≤2 hours.
- **Tests debug requirements.** Writing conceptual tests from requirements reveals ambiguities, missing requirements, and unnecessary requirements before any code is written.
- **Acceptance criteria ≠ "all tests passed."** They define business-ready conditions across functionality, quality, defects, compliance, and transition.
- **Reuse is eternal but not free.** Requires quality artifacts, infrastructure (shared repository or RM tool), and cultural commitment. Reuse by reference > copy-and-paste. Sets of related requirements > isolated statements.
- **Requirement patterns** package expertise into reusable templates for common requirement types.

---

## 6. Next Steps (from Wiegers)

- Reevaluate your backlog using the importance/urgency definitions — do your priorities change?
- Apply the value/cost/risk spreadsheet to 10–15 features from a recent project; compare calculated priorities to your subjective sense. Calibrate weighting factors until results match expectations.
- Try one new prioritization technique you haven't used before.
- Randomly sample a page of your SRS; have multi-perspective reviewers examine it with the defect checklist. If error density is concerning, inspect the full SRS.
- Define conceptual tests for a use case that hasn't been coded yet; verify with user reps that tests reflect intended behavior.
- Work with product champions to define acceptance criteria and acceptance tests.
- Identify reusable requirements assets in your organization; consider accumulating them into a shared repository.

---

## Related Notes

- [[Software Requirements Overview]] — SWEBOK overview; this note deepens the Prioritization, Validation, and Management knowledge areas.
- [[07_Risk_Reduction_through_Prototyping]] — Prototyping as a validation tool (Ch 15); referenced in §2.9.
- Chapters 16–19 sit at the end of Part II (Requirements Development), bridging into requirements management and project estimation.
