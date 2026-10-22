---
title: "Participating In Incidents"
role: Software Engineer
capability_area: Production Basics
topic: Participating In Incidents
status: complete
created: 2026-10-21
updated: 2026-10-21
tags:
  - career-path
  - software-engineer
  - production
  - incidents
---

# Participating In Incidents

> **Core skill:** Contributing to incident response without adding noise

## What Changes from Junior to Mid

A junior engineer in an incident either freezes or improvises loudly; both add load to the response. A mid-level engineer picks a role, communicates facts with timestamps, labels hypotheses as hypotheses, and stays useful under stress without being asked twice. The incident channel is a shared working memory with limited attention, and your job is to add signal to it.

| Aspect | Junior | Mid |
|--------|--------|-----|
| Role | Waits to be told what to do | Steps into a defined role: responder, scribe, or communicator |
| Communication | Vague impressions ("seems slow") | Facts with timestamps and named signals |
| Hypotheses | States guesses as conclusions | Labels hypotheses explicitly and tests cheaply |
| Fixes | Applies uncoordinated changes | Coordinates before acting; prefers reversible moves |
| Under stress | Goes quiet or goes noisy | Operates on a checklist: role, facts, next action |
| Channel use | Duplicates what others said | Adds only new information, each message earning its place |

## Core Frameworks

### Incident Roles

| Role | What It Owns | Best Filled By | Your Move as Junior/Mid |
|------|--------------|----------------|-------------------------|
| Responder | Diagnosis and mitigation actions in a scope | Engineers who know the system | Your main slot: investigate one named scope, run lookups, attempt bounded mitigations |
| Scribe | The timeline: observations, actions, decisions with times | Anyone disciplined about writing | The ideal first role: pure signal, no authority needed, huge learning value |
| Communicator | Updates to stakeholders outside the response | People with context and a calm register | Draft updates for the commander to send; learn the audience split |
| Commander | Coordination, priorities, deciding who does what | Senior engineers (see scope boundary) | Observe and follow; this role is owned elsewhere at your level |
| Subject expert | Deep answers about the affected component | The service's usual owners | That may be you for your own service — answer facts, not theories |

### Communication Discipline: The Message Contract

| Element | Do | Do Not |
|---------|----|--------|
| Facts | State what a signal showed, with the exact time and source | Summarize feelings about the system |
| Timestamps | Attach a clock time to every observation and action | Say "a while ago" or "recently" |
| Hypotheses | Prefix with "Hypothesis:" and name what would disprove it | Present guesses as findings |
| Actions | Announce before acting: "I am restarting instance X" | Apply changes silently and let others discover them |
| Questions | Ask targeted questions to the right person | Open vague questions to the whole channel |
| Duplicates | Read before posting; add only new information | Restate what the last three messages said |

### What Not to Do During an Incident

| Behavior | Why It Hurts | Instead |
|----------|--------------|---------|
| Uncoordinated fixes | Compounds unknowns; two changes make results uninterpretable | Announce, coordinate with the commander or primary, act on one change |
| Speculation in public channels | Sets a false narrative that stakeholders repeat | Keep theories in the response channel, clearly labeled |
| Fixing blame mid-incident | Diverts attention and poisons the record | Solve the problem; the review comes after |
| Debugging outside your assigned scope | Duplicate work and collisions with the responder | Stay in your lane; ask for a scope |
| Going silent when stuck | The team loses a sensor and cannot reassign you | Say "blocked on X" so someone can redirect you |
| Announcing recovery prematurely | Stakeholders stand down while the problem persists | State observed metrics, not conclusions |

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','background':'#1B1717','mainBkg':'#19362D','clusterBkg':'#161212','edgeLabelBackground':'#161212','titleColor':'#1FB854','fontSize':'14px'}}}%%
flowchart LR
    A["Page arrives"] --> B["Join the channel and claim a role"]
    B --> C["State facts with timestamps"]
    C --> D["Test labeled hypotheses cheaply"]
    D --> E["Coordinate before any action"]
    E --> F["Announce actions and results"]
    F --> G["Hand over cleanly at the end"]
    G --> H["Feed the postmortem timeline"]
```

## In Practice

**Claim a role explicitly within the first minutes.** "I am the scribe" or "I will investigate the checkout errors, no one else on that" prevents the two classic failures: duplicated work and unclaimed work. Roles make parallel effort safe because each person knows their lane. If no roles have been assigned, propose your own scope rather than waiting.

**Write every message as fact, time, and source.** "14:32: error rate on checkout jumped from 0.2% to 8%, dashboard link" is information the whole channel can build on; "checkout looks broken" is noise. Facts with timestamps become the incident timeline almost automatically, which is why good communicators are usually also good scribes. If you do not know the exact time, say when you noticed and label it as such.

**Label hypotheses out loud and cheaply.** Prefix theories with "Hypothesis:" and pair each with the smallest test that could kill it: "Hypothesis: the payment provider is timing out; test: check upstream status codes in the logs." The label keeps the channel from absorbing guesses as facts, and the cheap test keeps the response moving. Kill hypotheses fast and say when one dies.

**Announce before you act, always.** The single most dangerous incident behavior is the silent fix: it invalidates everyone else's reasoning and makes the timeline unreadable. Post "I am restarting instance X to test whether the problem is instance-local" and wait a beat for collision warnings. One visible action at a time, with the result reported back, keeps the response interpretable.

**Stay useful when you are blocked.** Stuck is a normal state in incidents; silence is not. Post "blocked: I cannot access the payment dashboard; can someone with access pull upstream latency since 14:30?" so the work can be reassigned or unblocked. A blocked-and-voiced contributor is worth more than a silent one making no progress.

**Keep stress responses on a checklist.** When adrenaline rises, fall back on mechanics: read the last ten messages, state your current task in one line, run the next bounded action, report the result. The checklist replaces the panic loop with a work loop. Calm is a skill built from procedure, not from temperament.

**Hand over cleanly and feed the timeline.** When the incident stabilizes or your shift ends, post a handover: scope covered, findings, open hypotheses, next action you would take. Then give your timestamped notes to the scribe or postmortem owner. The handover is where individual effort becomes team continuity.

## Practical Exercise

Practice incident participation with a realistic drill before the real thing:

1. Take a hypothetical scenario: "your service starts returning 500s fifteen minutes after a deploy." Write your first three channel messages as facts with timestamps and sources.
2. Assign yourself a role (responder, scribe, or communicator) and write one sentence defining what that role owns in this scenario.
3. Write two labeled hypotheses about the scenario, each with the cheapest test that would disprove it, and describe what result would kill each.
4. Script the "announce before act" message you would post before any mitigation, including what you are doing and why.
5. Write a stakeholder update in plain language with no jargon: what users see, what is being done, when the next update comes.
6. Write the handover message: scope covered, findings so far, open hypotheses, and the next action.
7. Review your messages against the contract: every claim sourced, every time stamped, every hypothesis labeled, no speculation presented as fact.

## Practical Applications

- [ ] I claim a role in the first minutes of every incident I join
- [ ] My channel messages carry a timestamp and a source for every claim
- [ ] Hypotheses are labeled as such and paired with a cheap disproof test
- [ ] I announce every action before taking it and report the result after
- [ ] When blocked, I say so immediately instead of going quiet
- [ ] I add only new information to the channel; I read before posting
- [ ] My stakeholder updates separate what users see from what we know
- [ ] I leave a clean handover and give my notes to the timeline owner

Incident message checklist:

```text
Before posting, confirm each of these in your draft:
1. Every claim has a time and a source (dashboard, log query, observation)
2. Hypotheses are prefixed "Hypothesis:" with a disproof test
3. Actions are announced before, results after
4. The message adds information not already in the channel
5. Questions are targeted at a named person or role
```

## Common Pitfalls

| Pitfall | Why It Fails | Better Approach |
|---------|--------------|-----------------|
| Uncoordinated fixes | Multiple simultaneous changes make results uninterpretable | Announce, coordinate, act one change at a time |
| Stating guesses as facts in any channel | The channel builds on a false narrative; public rumor outruns the fix and burns trust | Label hypotheses in the response channel; facts only to stakeholders |
| Silent fixes | Breaks everyone's shared model and the timeline | Announce before acting, always |
| Going quiet when blocked or lost | The team loses capacity without knowing it | Voice blockage so work can be reassigned |
| Reading nothing before posting | Duplicates noise the channel already carries | Read recent messages; post only what is new |
| Blame talk mid-incident | Diverts attention and poisons the record | Solve now, review later, blame never |
| Declaring recovery on a hunch | Stakeholders stand down while users still suffer | Cite the metrics that show recovery |

## Success Indicators

- You claim a role within minutes and stay inside it
- Your messages are so factual that the scribe can paste them into the timeline
- You label and kill hypotheses faster than you propose new ones
- You have never applied a change that another responder did not know about
- Incident commanders ask for you by name because you add signal, not noise
- Your handovers let another engineer continue without a call

## Related

- [[00_overview]]
- [[02_Reading_Dashboards_and_Logs]]
- [[06_Blameless_Postmortem_Participation]]
- [[career-path/05_Tech_Lead/07_Incident_Leadership_and_Production_Excellence/00_overview|Incident Leadership and Production Excellence]]
- [[career-path/07_SRE_and_Platform_Engineer/00_overview|SRE and Platform Engineer]]

## Sources

- *Incident Management for Operations* (Rob Schnepp, Jon Vidalis, Dave Ragaini)
- *Site Reliability Engineering* (Beyer, Jones, Petoff, Murphy)
- *The Site Reliability Workbook* (Beyer, Jones, Petoff, Murphy)
