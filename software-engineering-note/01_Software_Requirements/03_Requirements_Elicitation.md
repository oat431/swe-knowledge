---
tags:
  - requirements
  - elicitation
  - interviews
  - workshops
  - software-requirements
---

# Requirements Elicitation

> **Source:** Karl Wiegers, *Software Requirements* — Chapter 7
> **Purpose:** Identify the needs and constraints of diverse stakeholders for a software system through a collaborative, analytical, and cyclic process.

## What Is This?

Requirements elicitation is the heart of requirements development — the process of identifying the needs and constraints of the various stakeholders for a software system. It is **not** "gathering requirements," nor is it transcribing what users say. Elicitation is a *collaborative and analytical* process that collects, discovers, extracts, and defines requirements across all levels: business, user, functional, and nonfunctional. Wiegers calls it "perhaps the most challenging, critical, error-prone, and communication-intensive aspect of software development."

The nature of elicitation is **cyclic** (Figure 7-1): elicit → study → write requirements → discover gaps → elicit again. A couple of workshops does not constitute "done." Resist designing the system before understanding the problem; emphasize user *tasks* over user *interfaces*, and true *needs* over expressed *desires*.

The BA must create an environment conducive to thorough exploration: use business-domain vocabulary (not technical jargon), maintain a glossary, and make clear that brainstorming possibilities is separate from committing to priorities or feasibility.

## Elicitation Techniques

No project should rely on a single elicitation technique. Techniques split into two families:

- **Facilitated activities** — interact with stakeholders (interviews, workshops, focus groups, observations). Primarily surface business and user requirements.
- **Independent activities** — work on your own (interface analysis, document analysis, questionnaires). Supplement user-presented requirements and reveal functionality end users may not know about.

Most projects blend both. Each technique offers a different exploration and may reveal entirely different requirements.

### Interviews

The most obvious way to find out what users need is to ask them. Interviews are a traditional source of requirements input across all development approaches, including agile.

**When useful:**
- Getting up to speed quickly in a new application domain (interview domain experts)
- Establishing rapport — people feel safer one-on-one or in small groups than in large workshops, especially on touchy topics
- Eliciting business requirements from time-pressed executives
- Easier to schedule and lead than large-group workshops

**Tips:**
- **Establish rapport** — introduce yourself, review the agenda, remind attendees of objectives, address preliminary concerns.
- **Stay in scope** — even one-on-one, discussions can drift off topic.
- **Prepare questions and straw man models ahead of time** — draft a question list; people critique content more easily than they create it from scratch.
- **Suggest ideas** — a creative BA proposes alternatives; users may not realize what's possible. When users can't articulate needs, observe them work.
- **Listen actively** — lean forward, show patience, give verbal feedback, and *paraphrase* (restate the speaker's main idea to confirm understanding).

### Workshops

A **requirements workshop** (Gottesdiener 2002) is "a structured meeting in which a carefully selected group of stakeholders and content experts work together to define, create, refine, and reach closure on deliverables (such as models and documents) that represent user requirements." Workshops are facilitated sessions with formal roles — **facilitator** and **scribe** at minimum — and often include users, developers, and testers.

**When useful:**
- Resolving disagreements (group work is more effective than individual conversations)
- Quick elicitation turnaround under schedule constraints
- Eliciting from multiple stakeholders concurrently

**Tips:**
- **Establish and enforce ground rules** — start/end on time, silence devices, one conversation at a time, everyone contributes, criticize issues not individuals.
- **Fill all team roles** — note taking, time keeping, scope management, ground-rule management, ensuring everyone is heard.
- **Plan an agenda** — communicate it in advance so participants can prepare.
- **Stay in scope** — refer to business requirements; groups easily dive into distracting detail. Reel them back periodically.
- **Use parking lots** — capture off-topic but important items (quality attributes, business rules, UI ideas) on flipcharts so nothing is lost; describe what will happen with them later.
- **Timebox discussions** — allocate fixed time per topic to avoid spending the whole session on the first topic. Summarize status and next steps when closing each timebox.
- **Keep the team small but include the right stakeholders** — more than 5–6 active participants leads to side trips and bickering. Run parallel workshops for different user classes if needed.
- **Keep everyone engaged** — read body language (lack of eye contact, fidgeting, sighing, clock-watching); re-engage withdrawn participants; on teleconferences, listen for who is silent and the tones used.

> **Trap — Too many cooks:** A 12-person use-case workshop for a website crawled until the team was cut to ~6 (analyst, customer, architect, developer, visual designer). Progress accelerated; the lost input was more than compensated for.

### Focus Groups

A **focus group** is a representative group of users convened in a facilitated activity to generate input and ideas on a product's functional and quality requirements. Sessions must be **interactive**, letting all users voice their thoughts. Particularly valuable for **commercial products** where end users aren't readily accessible inside the company.

**Tips:**
- Select members carefully — include users of previous versions or similar products; either same-type users (multiple groups per class) or a spectrum representing all user classes.
- Must be facilitated — keep on topic *without* influencing opinions; consider recording the session.
- Don't expect quantitative analysis — focus groups yield subjective feedback to be evaluated and prioritized later.
- Participants normally **do not** have decision-making authority for requirements.
- Handle conflict immediately — look for nonverbal clues; if someone won't participate productively, talk privately and, if necessary, continue without them.

### Observations

When asked to describe their jobs, users often miss or misstate details — tasks are complex, habitual, or so familiar they're invisible. **Watching** users work can reveal what interviews cannot.

**When useful:**
- Validating information from other sources
- Identifying new interview topics
- Seeing current-system problems firsthand
- Spotting ways the new system can better support the workflow

**Guidance:**
- Time consuming — not suitable for every user or task. Limit each session to **≤ 2 hours** to avoid disrupting regular work.
- Select important or high-risk tasks and multiple user classes.
- In agile, have the user demonstrate only tasks related to the forthcoming iteration.
- **Abstract and generalize** beyond the observed individual — requirements must apply to the user class as a whole.
- **Silent observations** — for busy users who can't be interrupted.
- **Interactive observations** — interrupt mid-task to ask *why* a choice was made or what the user was thinking.
- Document observations for later analysis; consider video recording if policies allow.

> **Illustration — "Watch me bake a cake":** Telling someone to bake a cake from a mix, you'll likely forget to say "open the bag" or "crack the eggshell." Seemingly obvious steps aren't obvious to newcomers. Observations surface the tacit.

### Questionnaires

Questionnaires survey large user groups inexpensively, across geographical boundaries. Analyzed results feed into other techniques — e.g., identify biggest pain points, then discuss prioritization in a workshop.

**Biggest challenge:** writing good questions. Key tips:
- Provide answer options covering the full set of possible responses.
- Make choices **mutually exclusive** (no overlapping ranges) and **exhaustive** (all choices listed + write-in).
- Don't phrase questions to imply a "correct" answer.
- Use scales **consistently** throughout.
- Use **closed questions** for statistical analysis; **open-ended** for free-form responses (harder to aggregate).
- Consider consulting a questionnaire-design expert.
- **Always test** before distributing.
- Don't ask too many questions — people won't respond.

### System Interface Analysis

An **independent** technique: examine the systems to which yours connects. Reveals functional requirements about data and service exchange between systems.

**How:**
- Begin with **context diagrams** and **ecosystem maps** (Ch 5). If an interface with associated requirements isn't represented, the diagrams are incomplete.
- For each interfacing system, identify functionality that might lead to requirements for yours: what data to pass/receive, validation rules, etc.
- You may discover existing functionality you **don't** need to reimplement (e.g., an order-management system already validates orders from multiple sources).

### User Interface (UI) Analysis

An **independent** technique: study existing systems to discover user and functional requirements. Best to interact directly with the existing system; otherwise use screenshots (vendor user manuals often work).

**When useful:**
- Identifying a complete list of screens and potential features in packaged-solution or replacement projects
- Learning common user steps and drafting use cases for review
- Revealing data users need to see
- Getting up to speed on an existing system without extensive user interviewing

> **Caution:** Don't assume functionality is needed in the new system just because it exists in the old one. Don't assume the current UI flow *must* be replicated.

### Document Analysis

Examine existing documentation for potential requirements: requirements specifications, business processes, lessons-learned collections, user manuals for existing/similar applications, corporate/industry standards, regulations, problem reports, and enhancement requests.

**When useful:**
- Getting up to speed on an existing system or new domain
- Reducing elicitation meeting time (research and draft beforehand)
- Revealing information people don't volunteer — because they don't think of it or aren't aware of it (e.g., complex business logic buried in a user manual)

**Risk:** available documents may not be current. Requirements may have changed without spec updates; documented functionality may not be needed in the new system.

## Planning Elicitation

Early in a project, the BA should plan the elicitation approach. An **elicitation plan** includes techniques, timing, and purpose — and serves as a guide that may evolve. Explicit stakeholder commitment on resources, schedule, and deliverables prevents participants being pulled away.

The plan should address:

| Item | Description |
|---|---|
| **Elicitation objectives** | For the entire project and each planned activity |
| **Strategy & techniques** | Which techniques with which stakeholder groups, based on access, time, and system knowledge |
| **Schedule & resource estimates** | Customer and development participants; effort and calendar time; include BA prep and follow-up analysis time |
| **Documents & systems needed** | Materials for independent elicitation (documents, interface access) |
| **Expected products** | Use-case list, SRS, questionnaire analysis, quality-attribute specs — targets the right stakeholders, topics, and detail |
| **Elicitation risks** | Factors that could impede activities; severity; mitigation (see Ch 32 and Appendix B) |

> **Pitfall:** Many BAs default to interviews and workshops and never consider other techniques that could reduce resource needs or improve quality. Rarely does one technique yield the best results. Technique selection should be based on **project characteristics**, not habit.

Figure 7-3 suggests techniques by project type — e.g., new applications favor interviews + workshops + system interface analysis; mass-market software favors focus groups over workshops due to large external user base and limited representative access. These are suggestions, not rules.

## Preparing for Elicitation

Facilitated sessions require preparation — the larger the group, the more critical. Prepare by:

- **Plan session scope and agenda** — define scope via topics, questions, or specific process flows/use cases; align with project scope; itemize topics, time per topic, and targeted objectives; share the agenda in advance.
- **Prepare resources** — rooms, projectors, teleconference/video equipment; schedule participants sensitively to time zones (rotate meeting times for dispersed groups); collect documentation; gain system access; take online training on existing systems.
- **Learn about the stakeholders** — identify relevant stakeholders (Ch 6); learn cultural/regional preferences; provide supporting materials (slides) ahead of time for non-native speakers; avoid "us vs. them" tension.
- **Prepare questions** — go in with prepared questions; use uncertainties in straw man models as a source; use results from other techniques to identify open questions.
  - Avoid **leading** questions.
  - Probe beneath stated requirements to understand true needs.
  - *"What do you want?"* generates random information; *"What do you need to do?"* is far better.
  - Ask **"why"** repeatedly to move from presented solution to underlying problem.
  - Ask **open-ended** questions about current processes and improvement opportunities.
  - **Probe exceptions**: "What else could…", "What happens when…", "Would you ever need to…", "Where do you get…", "Why do/don't you…", "Does anyone ever…"
  - For replacement projects: *"What three things annoy you most about the existing system?"*
  - Document the **source** of each requirement for traceability.
  - End with: *"Is there anything else you expected me to ask?"*
- **Prepare straw man models** — draft use cases, process flows, or other models ahead of sessions. A straw man is a starting point that inspires critique; revising a draft is easier than creating from scratch. Tell the group: *"This model will probably be wrong. Please tear it apart."*

## Performing Elicitation

Executing the activity is the obvious part — talk to people (interviews), read documents (document analysis). When facilitating:

- **Educate stakeholders** — explain your approach and why you chose it; describe exploration techniques (use cases, process flows) and how they help; explain how you'll capture information and share it for review.
- **Take good notes** — assign a dedicated scribe who isn't actively participating. Notes should include: attendee list, absent invitees, decisions, actions with owners, outstanding issues, and key discussion highlights. If you must scribe yourself, use shorthand, type fast, or record (with consent). Audio pens link handwritten notes to recorded audio. Photograph whiteboards and wall sketches — don't try to capture diagrams in complex software live.
- **Exploit the physical space** — use all four walls for diagrams and lists; attach paper sheets if no whiteboards; provide sticky notes and markers; invite participants to contribute to the wall (Gottesdiener's "Wall of Wonder"). Project existing artifacts. For multi-location sessions, use online conferencing and video to show remote participants what's on the walls.
- **Use toys** (if culturally appropriate) — modeling clay, props — to stimulate creative thinking and give hands something to do.

## Following Up After Elicitation

After each activity: organize notes, document open issues, and classify the gathered information.

### Organizing and Sharing Notes

- Consolidate input from multiple sources; review and update notes **soon** while fresh.
- **Editing is a risk** — you may misremember and change meaning. Keep the raw notes.
- Share consolidated notes with participants promptly for review — only those who supplied the requirements can judge whether they were captured correctly.
- Hold additional discussions to resolve inconsistencies and fill blanks.
- Consider sharing with non-attending stakeholders so they can flag issues early.

### Documenting Open Issues

- Items needing later exploration, knowledge gaps, and new questions from note review go into an **issue-tracking tool**.
- For each issue: relevant notes, progress made, owner, due date.
- Consider using the same tool the dev/test teams use.
- Review parking lots from sessions for still-open items.

### Classifying Customer Input

Customers won't present a neatly organized list. Analysts must classify bits of information into categories (Figure 7-7 illustrates nine). During sessions, make quick notations (e.g., circle "DD" for a data definition). The nine categories include:

- **Business requirements** — financial, marketplace, or business benefit. Listen for value statements: *"Increase market share in region X by Y% within Z months."*
- **User requirements** — user goals or business tasks. *"I need to \<do something\>."* Typically represented as use cases, scenarios, or user stories.
- **Business rules** — conditions on who can do what under which circumstances. Not software requirements themselves, but may derive functional requirements. *"Must comply with…", "If \<condition\>, then \<action\>."*
- **Functional requirements** — observable system behaviors and user actions. *"If pressure exceeds 40 psi, the warning light should come on."* (How users *present* them, not how to *write* them — see Ch 11.)
- *(Plus quality attributes, data definitions, external interface, constraints, and design ideas — completing the nine.)*

Information that doesn't fit these buckets may be: project requirements (e.g., user training), project constraints (cost/schedule), assumptions/dependencies, historical/contextual info, or extraneous noise.

## Key Takeaways

- Elicitation is **collaborative and analytical**, not transcription. The cyclic nature means you'll iterate: elicit → study → write → discover gaps → elicit again.
- Use **multiple techniques** — facilitated (interviews, workshops, focus groups, observations) and independent (questionnaires, system/UI/document analysis). No single technique suffices.
- **Prepare** for facilitated sessions: scope, agenda, resources, stakeholder knowledge, prepared questions, and straw man models.
- **Follow up** promptly: organize and share notes, track open issues, and classify input into the nine categories so it can be documented and used appropriately.
- Separate **brainstorming** (imagining possibilities) from **commitment** (prioritizing, assessing feasibility). It's never too early for stakeholders to prioritize blue-sky wish lists.
- Resist the temptation to **design** before understanding the problem — premature design invites rework.

## Relationship to Other Notes

- **[[Software Requirements Overview]]** — Elicitation is the first and most error-prone activity of requirements development; this note implements the "Requirements Elicitation" knowledge area flagged as missing in the overview.
- *Finding the voice of the user (Ch 6)* — Product champions and user classes feed directly into who you elicit from.
- *Understanding user requirements (Ch 8)* — Use cases, scenarios, and user stories are the primary representation of user requirements elicited here.
- *Playing by the rules (Ch 9)* — Business rules surface during elicitation and are classified separately from functional requirements.
- *Writing excellent requirements (Ch 11)* — The functional requirements users *state* during elicitation must be crafted into precise specifications.
- *Establishing the business requirements (Ch 5)* — Context diagrams and ecosystem maps from business requirements guide system interface analysis.

## What's Missing

- The remaining categories of the **nine-bucket classification** (quality attributes, data definitions, external interface requirements, constraints, design ideas) — only four are detailed above; the source lists the full set in Figure 7-7.
- Specifics of **Figure 7-3** (suggested techniques by project characteristic) — reproduced here only as guidance, not the full matrix.
- **Elicitation traps** section — the chapter mentions cautions about traps during elicitation and suggestions for identifying missing requirements, but the source excerpt cut off before fully detailing them.
- Detailed guidance on **conflict management** during workshops/focus groups (Fisher, Ury, & Patton 2011; Patterson et al. 2011 are referenced).
