---
title: "Shadowing On Call"
role: Software Engineer
capability_area: Production Basics
topic: Shadowing On Call
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - production
  - on-call
---

# Shadowing On Call

> **Core skill:** Learning on-call by observing and assisting safely

## What Changes from Junior to Mid

A junior engineer hears about incidents after the fact and learns on-call as a spectator sport at best. A mid-level engineer shadows deliberately: preparing before the shift, staying a safe second pair of hands during pages, and writing up what they learned afterward. Shadowing is the rehearsal that turns production from an abstraction into a skill you can be trusted with, and it is how the team decides you are ready for the pager.

| Aspect | Junior | Mid |
|--------|--------|-----|
| Before the shift | Shows up cold | Reads the runbooks, tests access, knows the escalation path |
| During a page | Freezes or grabs the keyboard | Observes, takes notes, runs bounded assigned tasks |
| Scope | Wants to fix everything | Stays inside the tasks the primary explicitly hands over |
| Afterward | Forgets by Monday | Writes up the timeline, the questions, and the doc gaps |
| Access | Discovers missing permissions mid-incident | Verifies dashboards, logs, and deploy access before the shift |
| Readiness signal | "I think I'm ready" | A track record of shadowed pages and closed-up learning loops |

## Core Frameworks

### What Shadowing Is and Is Not

| Shadowing Is | Shadowing Is Not |
|--------------|------------------|
| Observing a live page end to end with an experienced primary | Being the responsible responder |
| Running bounded tasks the primary assigns: lookups, log pulls, timelines | Making remediation decisions on your own |
| Asking clarifying questions at safe moments, after the acute phase | Interrupting the response with learning questions mid-crisis |
| A structured apprenticeship with a written output | Passive listening with nothing captured |
| The evidence base for your on-call readiness decision | A rubber-stamp before carrying the pager |

### Preparation Checklist by Timing

| When | What to Do | Why It Pays Off During the Page |
|------|------------|--------------------------------|
| Days before | Read the runbooks for the systems in rotation; run the diagnosis steps against real dashboards | You recognize the failure modes instead of seeing them cold |
| Days before | Confirm access: dashboards, logs, deploy tooling, alert console, incident channel | Nothing burns response time like permission errors mid-page |
| Days before | Learn the escalation path: who is primary, who is secondary, when each is paged | You know exactly who to wake and when |
| Day of | Open the main dashboards and log queries in tabs before anything happens | The first minute of a page is for acting, not for navigating |
| Day of | Agree with the primary on your role: note-taker, lookup runner, or observer | Ambiguity during a page turns into collision |

### During the Page: The Shadow's Job

| Task | How to Do It | Bound to Stay Safe |
|------|--------------|--------------------|
| Observe | Watch how the primary forms and tests hypotheses | You are watching a decision process, not just commands |
| Take notes | Timestamp every observation, command, and decision | Notes only; do not act on them without the primary |
| Run lookups | Log queries, metric pulls, recent-deploy checks as asked | Exactly the task assigned, nothing adjacent |
| Communicate | Repeat back what you heard before acting on it | Bounded tasks, confirmed instructions |
| Learn | Note every question; do not ask mid-critical-path | Ask after the acute phase or in the write-up |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Read runbooks and verify access"] --> B["Agree on the shadow role"]
    B --> C["Page arrives"]
    C --> D["Observe and timestamp notes"]
    D --> E["Run bounded assigned tasks"]
    E --> F["Debrief with the primary"]
    F --> G["Write up learnings and doc gaps"]
    G --> H["Close gaps and shadow again"]
    H --> I["Carry the pager with support"]
```

## In Practice

**Prepare like you are already on call.** The shift is won or lost before the first page: runbooks read, access tested, dashboards open, escalation path memorized. A shadow who fumbles for login screens during a page is not just unhelpful; they pull attention from the incident. Preparation is also how you earn trust fast, because the primary can hand you real tasks immediately.

**Agree on your exact role before anything pages.** "I will take the timeline and run whatever lookups you ask for" is a role; "I will help somehow" is not. The primary needs to know what they can delegate without thinking about it. Repeating back assignments before acting on them prevents the classic shadow failure of confidently doing the wrong task.

**Stay inside your lane during the acute phase.** Your job is to make the primary faster and to learn, in that order. Run the assigned lookup, keep the timeline, and resist the pull of unassigned fixes: an uncoordinated second actor in an incident is a second incident. After the acute phase, there is usually room to discuss what happened and why.

**Timestamp everything you observe.** Note what was checked, what was found, and what was decided, with times attached; this becomes the raw material of the postmortem timeline and the incident record. Good notes turn the fog of a page into an ordered account someone else can reason from. Keep the notes factual: what you saw, not what you concluded.

**Ask your questions at the debrief, not in the critical path.** Every shadow has a stream of "why did you check that?" questions; saving them is a discipline. After the page (or after the acute phase), the primary is usually glad to walk through their reasoning, and that conversation is where the real learning happens.

**Write up what you learned within a day.** The write-up is the output that justifies the whole exercise: the failure mode, the decision process you observed, the queries that worked, and every gap in the runbooks you hit. Gaps go into tickets or direct doc fixes. A shadow shift with no write-up is entertainment; one with a write-up is training.

**Let readiness be earned, not declared.** Carrying the pager is justified by a track record: several shadowed pages across different failure modes, working access, calm communication, and runbook improvements you have shipped. When you and the primary both say you are ready, plan a supported first rotation (primary backup, explicit escalation comfort) rather than a cold start.

## Practical Exercise

Run one complete shadow cycle and produce the artifacts that make it count:

1. Pick a rotation you want to join and read the top three runbooks for its services; note anything you cannot verify without live access.
2. Test every credential and permission a page would need: dashboards, logs, deploy tooling, alert console, and the incident channel; fix what fails before the shift.
3. Write down the escalation path from memory: primary, secondary, when to wake each, and how to reach them.
4. Agree with the primary on your exact role for the shift and say it back to them in one sentence.
5. During at least one page (live or replayed), keep a timestamped log of every observation, command, and decision you witnessed.
6. Within 24 hours, write a one-page debrief: what failed, how the primary diagnosed it, what you did, and what confused you.
7. Turn every confusion into a concrete fix (a runbook edit, a saved query, or an access request) and track at least one to completion.

## Practical Applications

- [ ] I have read the runbooks for every service in the rotation before my first shift
- [ ] All access is tested and working before the shift, not discovered mid-page
- [ ] My role for the shift is agreed with the primary and written down
- [ ] During pages I keep a timestamped factual log of observations and decisions
- [ ] I run only assigned, bounded tasks during the acute phase
- [ ] My questions are saved for the debrief, not asked mid-crisis
- [ ] Every shadow shift produces a written debrief within a day
- [ ] The doc gaps I found are fixed or ticketed before the next shift

Shadow debrief template:

```text
# Shadow debrief: <date> — <service or failure mode>

## What happened
<one paragraph: what broke, when it was noticed, when it resolved>

## How the primary handled it
<the decision sequence: what they checked, in order, and what each result ruled out>

## What I did
<bounded tasks I ran, with timestamps where relevant>

## What I learned
<failure mode, tooling, or reasoning I can now use myself>

## Gaps found
<runbook steps that were wrong or missing; access issues; saved queries to add>

## Fixes and tickets
<each gap with its owner and link>
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Showing up without reading the runbooks | You cannot follow the response, so you cannot learn from it | Read and rehearse the docs before the shift |
| Discovering missing access mid-page | Delays the response exactly when minutes matter | Test every credential days in advance |
| Grabbing for the keyboard on the first page | Uncoordinated action adds chaos to the incident | Observe and run assigned bounded tasks only |
| Asking learning questions during the acute phase | Pulls the primary's attention off the incident | Save questions for the debrief |
| Taking no notes | The page blurs together and teaches nothing durable | Timestamp observations as they happen |
| Skipping the write-up | Learning stays personal and doc gaps persist | Ship the debrief within a day and fix the gaps |
| Declaring readiness by feeling | The first solo page becomes the team's problem | Earn it through shadowed pages and shipped fixes |

## Success Indicators

- You arrive at shifts with access verified and runbooks rehearsed
- During pages you are the calm note-taker the primary can delegate to
- Your debriefs are timely, factual, and specific enough to write a postmortem from
- Doc gaps you found have been fixed by you, not just reported
- The primary volunteers you for more responsibility without being asked
- You carry your first pager shift with support and notice the difference preparation makes

## Related

- [[00_overview]]
- [[03_Runbooks_and_Operational_Documentation]]
- [[05_Participating_In_Incidents]]
- [[career-path/07_SRE_and_Platform_Engineer/00_overview|SRE and Platform Engineer]]

## Sources

- *Incident Management for Operations* (Rob Schnepp, Jon Vidalis, Dave Ragaini)
- *The Site Reliability Workbook* (Beyer, Jones, Petoff, Murphy)
- *Site Reliability Engineering* (Beyer, Jones, Petoff, Murphy)
