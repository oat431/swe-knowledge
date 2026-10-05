---
title: Expectation and Scope Management
role: Forward Deployed Engineer
capability_area: Customer Communication and Executive Influence
topic: Expectation and Scope Management
status: complete
created: 2026-10-05
updated: 2026-10-05
tags:
  - career-path
  - forward-deployed
  - scope-management
  - expectation-setting
  - commitments
---

# Expectation and Scope Management

> **Core skill:** Keeping promises aligned with engineering reality — committing in writing, logging assumptions, and surfacing drift early enough that changes remain agreed decisions instead of accumulating arguments.

## Why This Matters

Deployments do not usually die from bad engineering; they die from mismatched expectations. The customer heard "this will replace the manual review process" while engineering meant "this will assist the reviewer on one document type." Nobody lied — the gap grew one friendly sentence at a time, in hallways and kickoff slides, until the first status meeting where the two versions of reality met. By then the customer has already budgeted staff changes against their version, and the FDE is not defending a scope, but a misunderstanding. Expectation management is the FDE's insurance against exactly this failure, and it costs nothing but discipline.

The core mechanism is writing things down. A commitment that exists in writing — with its assumptions attached — can be checked, tracked, and either met or renegotiated honestly. A commitment that exists as a memory is elastic, and elastic commitments always stretch in the direction of the more optimistic party. The assumptions matter as much as the commitments: "we will deliver the integration by the rollout date, assuming the customer's identity provider is available and the data extract format is fixed by week three" is a contract that everyone can operate against. Most scope pain comes not from broken promises but from unstated conditions that were silently violated.

Scope change is normal and should be welcomed, not resisted. Real deployments learn things that make the original scope wrong, and a customer whose needs have shifted is a customer who is engaged. The FDE's job is to convert change from a creeping erosion into an explicit trade: this addition costs this much time, displaces this much of the plan, or needs this decision from this person. Changes handled that way strengthen trust because they demonstrate that scope is a managed quantity rather than a fight waiting to happen. The failure mode is not change itself — it is unmanaged change, where each addition seems small, the total is enormous, and nobody can name the moment the plan stopped being true.

## The Vocabulary of Commitment

| Phrasing | What It Commits | Safer Practice |
|----------|-----------------|----------------|
| "Should be easy" | An unquantified promise the customer will remember as a date | State the actual effort or decline to size it verbally |
| "We will deliver by Friday" | A date with no conditions attached | "Friday, assuming the test data arrives Wednesday; otherwise the following Tuesday" |
| "It works" | A claim about the scenario just shown | "It works for this workflow; here is what is not yet covered" |
| "I will look into it" | A ticket the customer assumes is being fixed | Say what will happen, by when, and through which channel |
| "That is out of scope" | A wall that invites escalation | "That is not in the current phase; here is what adding it would take" |

## Handling Change

| Change Type | Process | Who Decides |
|-------------|---------|-------------|
| Clarification within agreed scope | Answer it, record it in the log | FDE with the customer's working lead |
| Addition with clear value | Size it, present the trade, get an explicit decision | Customer's product owner of the deployment |
| Replacement of an agreed item | Present what it displaces before agreeing | Both sides' leads, in writing |
| Change to acceptance criteria | Revisit success measures together | Sponsor level if the business case moves |
| Pressure to absorb work silently | Name the trade-off explicitly | Never resolved at the FDE's level alone |

```mermaid
flowchart LR
    COMMIT["Commit in writing"] --> ASSUME["Log the assumptions"]
    ASSUME --> DRIFT["Watch for drift early"]
    DRIFT --> RENEGO["Renegotiate with options"]
    RENEGO --> RECORD["Record the change"]
    RECORD --> RESET["Reset expectations together"]
```

The assumptions log is the quiet hero of this loop. Reviewed in every status cycle, it turns "we assumed X" from a defensive excuse discovered after failure into an early warning the whole team acts on: when an assumption starts looking shaky, the timeline conversation happens weeks before the timeline breaks.

## Practical Applications

### Scope Discipline Checklist

- [ ] Every commitment made to the customer exists in writing with its assumptions attached
- [ ] Assumptions are reviewed at every status cycle and flagged when they wobble
- [ ] Scope changes are sized and presented as explicit trades, never absorbed silently
- [ ] The acceptance criteria are written and agreed before the work they measure completes
- [ ] No date is promised without its conditions stated alongside it
- [ ] Both sides can point to the same current version of the scope, dated
- [ ] Settled decisions are not reopened without new information, and new information reopens them formally

### Assumptions and Commitments Log Template

```markdown
## Assumptions and Commitments — <deployment, date>

| Item | Type | Owner | Status | Impact if broken |
|------|------|-------|--------|------------------|
| <statement> | assumption or commitment | <name> | <holding, at risk, broken> | <what changes and by when we would know> |
```

## Common Pitfalls

| Pitfall | Why It Is a Problem | Better Approach |
|---------|---------------------|-----------------|
| **Verbal optimism** | Enthusiastic estimates become remembered promises with no conditions | Say the number with its assumptions, or say when you will have one |
| **Silent scope creep** | Small additions compound until the plan is fiction | Size and trade every addition explicitly, however small it looks |
| **Dates without engineering input** | A date promised above the engineer's head becomes the engineer's problem | Only commit to dates the people doing the work have endorsed |
| **"Should be easy"** | The customer hears a date; you meant a shrug | Never size work verbally without stating it is a guess |
| **Undocumented changes** | Disputes later cannot be resolved because nothing was written | Record every change with the same rigor as the original scope |
| **Re-litigating settled scope** | Reopening decisions without new information burns goodwill | Reopen only with new information, formally, at the right level |

## Success Indicators

- The customer and the FDE describe the current scope identically, from the same dated document
- Changes arrive as decisions with trades attached, not as accumulating surprises
- Broken assumptions are caught by the log before they become broken timelines
- Disagreements about "what was agreed" essentially stop occurring
- The relationship survives saying no, because saying no comes with alternatives

## Related Topics

- [[05_Difficult_Conversations]]
- [[02_Executive_Communication]]
- [[01_Field_Discovery_and_Problem_Framing/00_overview|Field Discovery and Problem Framing]]
- [[career-path/12_Technical_Program_Manager/00_overview|Technical Program Manager]]

## Summary

Expectation and scope management is the FDE's discipline of making promises that can be kept: commit in writing with assumptions attached, keep the assumptions log alive so drift is caught early, convert every change into an explicit trade with a named decider, and state dates with conditions rather than hopes. Deployments rarely fail because the engineering was impossible; they fail because two organizations carried two different versions of the plan — and the FDE's job is to keep the deployment living in exactly one.
