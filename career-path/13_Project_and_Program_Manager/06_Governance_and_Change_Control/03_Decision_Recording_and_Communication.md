---
title: "Decision Recording and Communication"
role: Project and Program Manager
capability_area: Governance and Change Control
topic: Decision Recording and Communication
status: complete
created: 2026-10-05
updated: 2026-10-05
tags:
  - career-path
  - project-manager
  - governance
  - decisions
  - communication
---

# Decision Recording and Communication

> **Core skill:** Capturing governance decisions completely and communicating them promptly to everyone affected — so decisions are not remade, forgotten, or misinterpreted, and the project has an authoritative record of what was decided, by whom, when, and why.

## Why This Matters

A decision not recorded is a decision that will be remade. The steering committee agreed on the scope reduction, but six weeks later a stakeholder who was not in the room demands the original scope — and there is no record of the decision to point to. The sponsor approved the schedule extension, but the executive team asks why the date moved — and the project manager reconstructs the rationale from memory.

Decision recording is the project's institutional memory. It answers, for every significant decision: what was decided, who decided it, when, why, what alternatives were considered, and who is affected. Communication ensures that the people affected by a decision know about it before they discover it through its consequences. Together, recording and communication prevent the project's most expensive waste: remaking decisions that were already made.

## What to Record

Not every conversation is a decision worth recording. The threshold: record decisions that change scope, schedule, cost, risk posture, or stakeholder commitments.

### The Decision Record

| Field | Content |
|-------|---------|
| **Decision ID** | Unique identifier, sequential |
| **Date** | When the decision was made |
| **Decision** | What was decided — one sentence, unambiguous |
| **Decision-maker** | Who made the decision (name and role) |
| **Forum** | Where the decision was made (steering committee, sponsor 1:1, emergency approval) |
| **Context** | Why the decision was needed — the situation that prompted it |
| **Options considered** | Alternatives that were evaluated and rejected |
| **Rationale** | Why this option was chosen over the alternatives |
| **Impact** | Effect on scope, schedule, cost, risk, benefits |
| **Conditions** | Any conditions attached to the decision |
| **Affected parties** | Who needs to know and act on this decision |
| **Implementation owner** | Who is accountable for executing the decision |
| **Review date** | When the decision will be revisited, if applicable |

### Decision Record Template

```markdown
## Decision Record — [DR-###]
- **Decision**: [One sentence — unambiguous]
- **Date**: [YYYY-MM-DD]
- **Decision-maker**: [Name, Role]
- **Forum**: [Steering Committee / Sponsor 1:1 / Emergency Approval]
- **Context**: [Why this decision was needed]
- **Options considered**:
  - Option A: [description] — pros/cons
  - Option B: [description] — pros/cons
  - Option C: [description] — pros/cons
- **Rationale**: [Why the chosen option]
- **Impact**: [Scope, schedule, cost, risk, benefits]
- **Conditions**: [Any conditions attached]
- **Affected parties**: [List of stakeholders]
- **Implementation owner**: [Name]
- **Review date**: [If applicable]
```

## The Decision Log

The decision log is the project's chronological decision register:

| DR# | Date | Decision | Decision-maker | Impact | Status |
|-----|------|----------|----------------|--------|--------|
| 001 | 2026-01-15 | Reduce scope: defer reporting module to Phase 2 | Steering Committee | Schedule: recover 6 weeks; Scope: 2 features deferred | Implemented |
| 002 | 2026-02-01 | Accept schedule risk: proceed without integration test environment | Sponsor | Risk: medium; Mitigation: vendor provides staging env by Feb 15 | Implemented; risk retired |
| 003 | 2026-02-20 | Approve budget increase: $150K for additional QA capacity | Sponsor | Cost: +$150K; Schedule: recovers 2 weeks | Implemented |

The log is the project's answer to "why did we do that?" — a question asked in every audit, every phase gate, and every post-project review.

## Communicating Decisions

A decision that affects stakeholders but is not communicated to them is a decision that will be discovered through its consequences — which is the worst way for a stakeholder to learn about a decision.

### Communication by Decision Type

| Decision Type | Communication Method | Timing |
|---------------|---------------------|--------|
| **Scope change** | Affected stakeholders: direct communication with rationale and impact; all stakeholders: change log update in next status report | Within 48 hours; before implementation |
| **Schedule change** | All stakeholders: status report headline; affected teams: direct communication with new dates and rationale | Within 48 hours |
| **Budget change** | Sponsor: confirmed in writing; steering committee: next meeting agenda; affected teams: direct if resource impact | Within 1 week |
| **Risk acceptance** | Steering committee: recorded in minutes; affected teams: direct if risk affects their work | Within 1 week |
| **Phase gate: go/no-go** | All stakeholders: formal communication with conditions and next phase plan | Within 48 hours of gate review |

### The Communication Principle

Every decision communication answers three questions for the recipient:

1. **What changed** — the decision, in one sentence
2. **What it means for you** — the impact on the recipient's work, plans, or commitments
3. **What happens next** — actions, dates, and who to contact with questions

Communication that answers only "what changed" leaves the recipient to infer the impact — and stakeholders infer impact pessimistically.

## Decision Communication in Distributed Teams

When stakeholders are distributed across time zones and organizations:

- **Push, don't post**: decisions are communicated directly to affected parties — not assumed read from a shared document
- **Multiple channels**: email for the formal record; chat or call for the human conversation; decision log for the permanent reference
- **Confirm receipt**: for critical decisions, confirm that affected parties have received and understood — "Can you confirm this doesn't create a conflict for your team?"
- **Cultural adaptation**: some cultures expect written decisions before verbal discussion; others expect the opposite — know your stakeholders

## Decision Recording in Programs

Program decision recording adds scale:

- **Program decision log**: decisions at the program level that affect multiple projects
- **Project decision logs**: maintained by project managers; reviewed by the program manager for cross-project impact
- **Decision traceability**: a program-level decision (e.g., "reduce all project contingency by 10%") must trace to project-level implementation decisions
- **Decision consistency**: the program manager ensures that decisions across projects are consistent in rationale and communication quality

## Practical Applications

### Decision Recording Checklist

- [ ] Every decision that changes scope, schedule, cost, or risk posture is recorded in the decision log
- [ ] Decision records include options considered, rationale, and impact
- [ ] The decision log is referenced in status reports and phase gate reviews
- [ ] Affected parties are identified for every decision
- [ ] Decisions are communicated to affected parties within 48 hours
- [ ] Communication answers: what changed, what it means for you, what happens next

## Common Pitfalls

| Pitfall | Why It Is a Problem | Better Approach |
|---------|---------------------|-----------------|
| **Verbal-only decisions** | Decisions made in meetings with no record; remade later | Every significant decision recorded in the decision log |
| **Rationale omission** | The decision is recorded but not why; future readers cannot assess it | Options, rationale, and impact recorded with every decision |
| **Communication delay** | Affected stakeholders discover the decision through its consequences | Communication within 48 hours; before implementation |
| **One-channel communication** | Decision posted to a shared drive; nobody reads it | Push to affected parties directly; log for reference |
| **Decision log neglect** | The log exists but is incomplete; the project's memory is fragmented | Log reviewed at every steering committee for completeness |

## Success Indicators

- The decision log is complete and referenced — it is the project's authoritative memory
- No decision is remade because the original was not recorded
- Affected stakeholders are informed of decisions before they experience the consequences
- Decision communications answer "what changed, what it means for you, what happens next"
- Auditors and phase gate reviewers reference the decision log as the primary record

## Related Topics

- [[01_Project_Governance_Design]]: the forums where decisions are made
- [[02_Change_Control_Process]]: change decisions are the most frequent decision type
- [[04_Configuration_Management]]: decisions often update baselines and artifacts
- [[03_Project_Communication_Management|Project Communication Management]]: decision communication is a communication plan element
- [[career-path/04_Principal_and_Distinguished_Engineer/03_Decision_Governance_and_Principles/00_overview|Decision Governance and Principles (Principal)]]: the enterprise decision-recording standard

## Summary

Decision recording and communication close the governance loop: every significant decision is captured with what was decided, by whom, when, why, what alternatives were considered, and who is affected — stored in a decision log that is the project's institutional memory. Communication ensures that affected parties learn about decisions within 48 hours, through a push not a post, with an explanation of what changed, what it means for them, and what happens next. A decision not recorded is a decision that will be remade; a decision not communicated is a decision that will be discovered through its consequences. The project manager who records and communicates decisions well builds a project that learns from its own choices; the one who does not builds a project that repeats them.