---
title: "Problem Framing Before Solutioning"
role: Software Engineer
capability_area: Readiness for Senior
topic: Problem Framing Before Solutioning
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - senior-readiness
  - problem-framing
---

# Problem Framing Before Solutioning

> **Core skill:** Defining the real problem before proposing solutions

## What Changes from Junior to Mid

A junior engineer receives a ticket that already contains the problem and the proposed solution, and implements it faithfully. A mid-level engineer notices when the request is underspecified and drafts acceptance criteria before starting, but usually still accepts the framing that came with the work.

The senior shift is to treat the framing itself as part of the job. Before estimating or designing, you establish what problem is being solved, for whom, at what cost, and what would count as success. This is the cheapest place to prevent waste: a wrong solution to a well-framed problem is a setback; a right solution to the wrong problem is a quarter lost.

| Aspect | Junior | Mid |
|--------|--------|-----|
| Starting point | Ticket with steps | Request with a goal |
| Questions | "What exactly should it do?" | "What outcome are we trying to change?" |
| Ambiguity | Escalates it upward | Reduces it inside the task |
| Written artifact | Ticket update | Acceptance criteria and estimate |
| Solution space | Implements the named approach | Compares approaches when the first is questionable |
| Success check | Acceptance criteria met | User behavior or system outcome changed |
| Failure mode | Builds the wrong detail correctly | Builds the right feature for the stated request |

At senior, framing expands beyond a single request to the area: which problems are worth solving at all, and which ones should be declined or deferred.

## Core Frameworks

### The Framing Template

| Element | Question to answer | Quality bar |
|---------|--------------------|-------------|
| Problem statement | What is happening now, and why is it a problem? | Describes observed behavior and its cost, not a missing feature |
| Who is affected | Which users, teams, or systems feel it, and how often? | Names specific groups and the frequency or conditions |
| Current cost | What does the status quo cost in time, money, risk, or trust? | Concrete and comparable to the cost of a fix |
| Constraints | What must remain true: contracts, deadlines, compliance, staffing, compatibility? | Each constraint is real and has an owner |
| Success criteria | What observable change proves this worked? | Measurable from telemetry or a named review, not opinion |
| Non-goals | What are we explicitly not solving here? | At least one tempting adjacent problem is excluded |
| Open questions | What do we not know yet, and how will we learn it? | Each has an owner and a way to resolve it |

### Solution-First Tells

| What you hear | What it usually means | Framing response |
|---------------|----------------------|------------------|
| "Can we just add a cache?" | A performance symptom has been diagnosed as a mechanism | Ask what the latency budget is and where time is actually spent |
| "The ticket says to build X" | The solution predates the problem statement | Ask what happens if X does not exist; who is hurt and how |
| "We need to rewrite it" | Accumulated frustration, not a scoped problem | Ask which failures the rewrite fixes and which it preserves |
| "Just ship it and see" | Success criteria are undefined, so nobody can fail | Define one observable outcome and a review date |
| "This is urgent" | Priority is asserted without a cost comparison | Ask what it displaces and who decided the trade-off |
| "Everyone does it this way" | Pattern matching without constraints | Ask which of our constraints the pattern assumes |

Scale the artifact to the stakes: a two-sentence restatement for a small bug, a one-page problem doc for a feature, a day of conversations for a cross-team change, and several days plus an options memo for a new system or migration.

### The Do-Nothing Test

| Question | What it reveals |
|----------|-----------------|
| What happens if we do nothing for a quarter? | Whether the problem is urgent or merely annoying |
| Who notices first if nothing changes? | The real stakeholders, as opposed to the loudest requester |
| What workaround exists today? | The true baseline the solution must beat |
| What breaks if the workaround scales? | Whether the cost is growing or stable |
| What would make this problem disappear on its own? | Whether the fix is technical at all |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Receive a request or ticket"] --> B["Ask what happens if we do nothing"]
    B --> C["Name who is affected and the cost"]
    C --> D["Restate the problem in one paragraph"]
    D --> E["List constraints and non-goals"]
    E --> F["Define observable success criteria"]
    F --> G["Compare options against the criteria"]
```

## In Practice

**Restate the problem in your own words before you estimate.** A one-paragraph restatement catches more misunderstandings than any amount of clarifying questions in a meeting. Send it to the requester and ask them to correct it. If they cannot agree with the restatement, the work is not ready to estimate.

**Run the do-nothing test on every non-trivial request.** Ask what happens if the team does nothing for a quarter. Sometimes the answer is "nothing much", which is a legitimate reason to decline. Sometimes it is "the workaround breaks at the next launch", which converts a vague annoyance into a dated risk.

**Make non-goals explicit and visible.** Scope creep usually enters through problems that were never declared out of bounds. A short non-goals list gives everyone a polite way to say "that is a different problem" without appearing unhelpful. It also protects the success criteria from being quietly expanded.

**Catch solution-first conversations without becoming obstructive.** When a conversation jumps to a mechanism, ask what outcome the mechanism serves. You are not vetoing the solution; you are checking that it is being chosen against criteria rather than by momentum. If the solution survives the question, you have made the decision stronger.

**Write the framing down where decisions happen.** A problem doc of five short sections beats a long verbal alignment that evaporates by the next sprint. Put it in the ticket, the project doc, or the design review, and link it from everywhere the work is discussed. Framing that lives only in a meeting is framing that will be relitigated.

**Use success criteria that someone else could check.** "Better performance" is not a criterion; "p95 latency under the agreed budget during peak hours" is. The test is whether a reviewer could look at telemetry or a demo and declare the work done without asking you. If they cannot, the criterion is still a wish.

**Know when framing is done.** Framing is complete when the problem, the affected group, the constraints, the success criteria, and the non-goals are written and agreed. It is not complete when you have a solution you like. Stop when the document would let a different engineer propose a different reasonable option.

## Practical Exercise

1. Pick three tickets you worked on in the last month, including one that felt vaguely specified.
2. For each, write a five-sentence restatement: current behavior, who is affected, cost, desired outcome, and what is out of scope.
3. Run the do-nothing test on each and record the answer. Mark any request where the answer is "not much".
4. For the weakest ticket, list the constraints that were never written down and who owns each one.
5. Rewrite the success criteria so a reviewer could verify them from telemetry or a demo without asking you.
6. Take the rewritten framing to the original requester and ask one question: "Did I miss anything?"
7. Record what changed in your understanding, and add the diff to the ticket.

## Practical Applications

- [ ] Every feature I start has a written one-paragraph problem statement
- [ ] I can name who is affected and what the status quo costs
- [ ] I run the do-nothing test before estimating non-trivial work
- [ ] My tickets list at least one explicit non-goal
- [ ] Success criteria are observable by someone other than me
- [ ] I have caught at least one solution-first request and reframed it
- [ ] My problem docs are linked from the work they govern

Problem doc template:

```markdown
## Problem: <short name>

**Status:** draft / agreed
**Owner:** <name>
**Last updated:** <date>

### What is happening now
<observed behavior, with where it is visible>

### Who is affected and how often
<groups, conditions, frequency>

### What it costs
<time, money, risk, trust — compared with the cost of a fix>

### What we are trying to achieve
<one or two observable outcomes>

### Success criteria
- <criterion checkable from telemetry or a demo>

### Constraints
- <contract, deadline, compliance, staffing, compatibility> — owner: <name>

### Non-goals
- <adjacent problem we are deliberately not solving>

### Open questions
- <question> — owner: <name> — resolve by: <date>

## Options
<filled after the problem is agreed, not before>
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Accepting the ticket's framing as the problem | The solution was chosen before anyone defined success | Restate the problem and confirm it with the requester |
| Skipping framing because the fix is obvious | Obvious fixes often target symptoms; the cost reappears elsewhere | Spend ten minutes on the do-nothing test anyway |
| Writing a problem doc that is really a solution pitch | Options get evaluated against a preferred answer | Keep the problem section free of mechanisms |
| Framing alone in your head | Others cannot correct what they cannot see | Write it down and circulate a short version |
| Success criteria that only you can judge | The work can never be declared done or wrong | Make criteria observable from telemetry or a demo |
| No non-goals | Scope expands until the original outcome is diluted | Name one tempting adjacent problem as out of scope |
| Framing everything to death | Small work does not need a committee | Scale the artifact to the stakes |

## Success Indicators

- Your problem statements get quoted or reused by other people
- Requests arrive at you with better-defined success criteria over time
- You can decline or defer work with a reasoned cost comparison
- Scope changes get caught by the non-goals list instead of by surprise
- Your team finishes work and can prove it succeeded
- Solution-first conversations pause when you ask what outcome is being served

## Related

- [[00_overview]]
- [[01_What_Senior_Really_Means]]
- [[04_Design_Judgment_Basics]]
- [[career-path/02_Senior_Software_Engineer/02_Problem_Framing_and_Requirements/00_overview|Problem Framing and Requirements]]
- [[career-path/01_Software_Engineer/01_Leveling_and_Growth/02_Junior_vs_Mid_Level_Expectations|Junior vs Mid Level Expectations]]
- [[career-path/02_Senior_Software_Engineer/09_Promotion_Evidence_and_Capstone/07_Mid_to_Senior_Promotion|Mid to Senior Promotion]]

## Sources

- *A Philosophy of Software Design* (John Ousterhout)
- *The Staff Engineer's Path* (Tanya Reilly)
- *Software Engineering at Google* (Titus Winters, Tom Manshreck, Hyrum Wright)
