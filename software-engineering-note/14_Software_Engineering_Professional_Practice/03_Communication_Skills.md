---
tags:
  - professionalism
  - communication
  - writing
  - presentation
  - reading
  - software-engineering
source: "SWEBOK v4 Chapter 14 — Professional Practice, Communication Skills"
created: 2026-07-21
---

# Communication Skills for Software Engineers

> Code is not the only artifact software engineers produce. Requirements documents, design specifications, test plans, user manuals, project proposals, technical reports, conference presentations, and team communications are all professional deliverables — and all require disciplined communication skills.

## 1. Technical Reading

### Purpose and Context

Software engineers read continuously: code reviews, API documentation, research papers, standards (IEEE, ISO), incident reports, email threads. Effective reading is active, not passive.

### Reading Strategies

| Strategy | When to Use | Technique |
|---|---|---|
| **Skimming** | Determining relevance, getting overview | Read title, abstract, headings, conclusions |
| **Scanning** | Finding specific information | Search for keywords, patterns, code snippets |
| **Deep Reading** | Understanding complex material | Slow, annotate, question, summarize after each section |
| **Comparative Reading** | Evaluating alternatives | Read multiple sources on same topic, note disagreements |

### Reading Code as a Communication Skill

Code reading is a distinct literacy skill:
- **Trace execution mentally** before running — predict state changes
- **Identify patterns** — recognize idioms and anti-patterns in unfamiliar codebases
- **Read tests first** — they document expected behavior more reliably than comments
- **Annotate** — mark up printed code or use IDE bookmarks for navigation

### Reading Research Papers (Three-Pass Method)

| Pass | Depth | Goal |
|---|---|---|
| **First Pass** (5-10 min) | Title, abstract, introduction, conclusions, references | Decide whether to read further |
| **Second Pass** (1 hour) | Read full paper, skip proofs | Understand contributions, note key figures |
| **Third Pass** (hours) | Reconstruct from scratch, critique assumptions | Deep understanding, identify flaws |

## 2. Technical Writing

### The Writing Process

Effective technical writing follows a structured process:

1. **Define audience and purpose** — Who will read this? What decision should they make?
2. **Outline** — Structure before prose; ensures logical flow
3. **Draft** — Write quickly without editing (separate creation from criticism)
4. **Revise** — Restructure for clarity; cut unnecessary content
5. **Edit** — Fix grammar, spelling, style
6. **Review** — Have someone else read it (fresh eyes catch gaps)

### Types of Technical Documents

| Document | Audience | Key Qualities |
|---|---|---|
| **Requirements Specification (SRS)** | Developers, QA, stakeholders | Unambiguous, testable, complete, traceable |
| **Design Document** | Developers, architects | Clear rationale for decisions; alternatives considered |
| **Test Plan** | QA, developers | Coverage strategy, traceability to requirements |
| **User Manual** | End users | Task-oriented, minimal jargon, screenshots |
| **Technical Report** | Management, peers | Data-driven, actionable conclusions |
| **Incident Postmortem** | Engineering org | Blameless, timeline-based, action items with owners |
| **API Documentation** | Other developers | Parameter types, return values, error conditions, examples |

### Writing Principles

**Clarity over Cleverness:** The reader's understanding is the only metric that matters.

| Principle | Anti-pattern | Better |
|---|---|---|
| **Be concise** | "It is important to note that the system utilizes a distributed architecture" | "The system uses a distributed architecture" |
| **Use active voice** | "The bug was introduced by the recent change" | "The recent change introduced the bug" |
| **One idea per paragraph** | Dense paragraphs covering multiple topics | Each paragraph advances exactly one point |
| **Define terms first** | Using "the orchestrator" without defining it | "The orchestrator (the service that coordinates deployments)..." |
| **Prefer concrete over abstract** | "The performance improved" | "Latency dropped from 240ms to 85ms (p99)" |

### Document Structure Patterns

**Good documents follow predictable structure:**

| Element | Purpose |
|---|---|
| **Title** | What the document is |
| **Metadata** | Author, date, version, status (draft/reviewed/approved) |
| **Summary/Abstract** | What this document covers; who should read it |
| **Context** | Why this was written (problem statement, background) |
| **Body** | The structured content |
| **Conclusions/Decisions** | What was decided and why |
| **References** | Links to related documents, standards, prior art |

### Writing for Different Media

| Medium | Best For | Pitfalls |
|---|---|---|
| **Email** | Quick updates, decisions, meeting follow-ups | Long threads lose context; use for broadcast not discussion |
| **Chat (Slack/Teams)** | Real-time coordination | Ephemeral — important decisions must be captured elsewhere |
| **Wiki/Confluence** | Living documentation | Gets stale without ownership and review cycles |
| **ADR (Architecture Decision Record)** | Recording architectural decisions | Must include context and consequences, not just choice |
| **RFC (Request for Comments)** | Proposing significant changes | Must have clear decision deadline and designated decider |

## 3. Presentation Skills

### Preparation

**Know your audience:** What do they already know? What do they need to know? What decision should they make after your presentation?

**Structure matters more than slides:**
1. **Opening:** State the problem and why it matters (1-2 minutes)
2. **Body:** 3-5 key points with evidence (80% of time)
3. **Closing:** Summary, call to action, Q&A (10-15% of time)

### Slide Design Principles

| Do | Don't |
|---|---|
| One idea per slide | Walls of text |
| Visuals over bullet points | Reading slides verbatim |
| High-contrast, large fonts | Tiny text, low contrast |
| Code snippets: 15 lines max, syntax highlighted | Full source files on slides |
| Build diagrams incrementally if complex | Static dense diagrams |

### Types of Technical Presentations

| Format | Context | Preparation Strategy |
|---|---|---|
| **Design Review** | Proposing architecture/design to peers | Focus on trade-offs and alternatives rejected, not just the chosen design |
| **Sprint Demo** | Showing working software to stakeholders | Show, don't tell; prepare the happy path but be ready for questions |
| **Conference Talk** | Teaching/sharing with the community | Rehearse timing; have backup for live demos |
| **Incident Review** | Sharing postmortem findings | Timeline format; focus on system improvements, not blame |
| **Project Pitch** | Securing resources/buy-in | Lead with problem, quantify impact, propose concrete plan |

### Handling Q&A

> [!tip] **The Q&A Rule:** Repeat the question before answering. Ensures everyone heard it, gives you time to think, and confirms you understood correctly.

- Don't bluff — "I don't know, but I'll find out and follow up" is professional
- Park off-topic questions — "That's a good question about X, let's discuss after the presentation"
- Pause before answering — silence signals confidence, not uncertainty

## 4. Team Communication

### Communication Patterns

| Pattern | Example | Use |
|---|---|---|
| **Daily Standup** | 15 min, what I did/will do/blockers | Synchronization, not status reporting |
| **Retrospective** | What went well/what to improve/actions | Continuous improvement |
| **One-on-One** | Weekly manager/mentor meeting | Coaching, career growth, blockers |
| **Written RFC** | Async proposal with comment period | Significant technical decisions requiring broad input |
| **Decision Log** | Brief entries: decision + context + consequences | Institutional memory |

### Managing Communication Overhead

With n team members, communication paths = n(n-1)/2. Mitigations:
- **Information radiators** (dashboards, kanban boards, CI status) — pull, not push
- **Written over verbal** for decisions — async, searchable, referenceable
- **Designated communication channels** — not everything goes to everyone
- **Documentation as code** — living alongside the codebase, reviewed in PRs

## Key Takeaways

1. **Technical writing is a deliverable, not an afterthought** — your documents are as important as your code
2. **Write for your audience** — what decision should they make after reading?
3. **Presentations convince; documents record** — use the right medium for the right purpose
4. **Code reading is a distinct and learnable skill** — tests are the best documentation
5. **Written over verbal** for decisions — creates institutional memory and enables async collaboration
6. **Never read your slides** — if they can read them, you're redundant

## Related

- [[Professionalism of Software Engineering Overview]] — All professional practice topics
- [[01_Professionalism_Ethics_and_Legal]] — Professional ethics and legal obligations
- [[02_Group_Dynamics_and_Psychology]] — Team psychology and dynamics
- [[clean-coder/clean-coder Overview]] — Clean Coder: practical communication discipline
- [[Software Design Overview]] — Design documents and ADRs
