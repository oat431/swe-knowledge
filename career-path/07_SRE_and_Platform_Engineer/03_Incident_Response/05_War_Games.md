---
title: "War Games"
note_type: capability-topic
capability_area: incident-response
career_path: sre-and-platform-engineer
prerequisite:
  - "[[04_Post_Incident_Action_Items]]"
tags:
  - career-path
  - sre
  - platform-engineering
  - incident-response
  - war-games
  - game-days
---

# War Games

> **One-line definition:** Practicing incident response through controlled simulations to test readiness, identify gaps, and improve team coordination.

## Why This Is a Specialist Skill

A senior software engineer may participate in incident response. An SRE or platform engineer **designs and facilitates war games**, **identifies systemic gaps before they cause real incidents**, and **builds organizational muscle memory for incident response**.

The difference is not technical skill. It is **proactive preparation**: testing your incident response process before a real incident occurs.

## What Are War Games?

War games (also called game days) are controlled simulations of production incidents:

| Aspect | Description |
|---|---|
| **Purpose** | Test incident response process, tools, and team coordination |
| **Environment** | Staging or production (with safety controls) |
| **Scenarios** | Realistic failure modes (service down, database failure, dependency timeout) |
| **Participants** | On-call engineers, incident commanders, technical leads |
| **Outcome** | Identify gaps in process, tools, documentation, and skills |

### War game vs chaos engineering

| Aspect | War Game | Chaos Engineering |
|---|---|---|
| **Focus** | Incident response process | System resilience |
| **Participants** | People (on-call, IC, tech lead) | Automated systems |
| **Goal** | Test coordination and communication | Test automatic recovery |
| **Frequency** | Quarterly | Continuous |
| **Scope** | End-to-end incident response | Specific failure modes |

**Key insight:** War games test **people and process**. Chaos engineering tests **systems and automation**. Both are valuable.

## War Game Scenarios

### Common scenarios

| Scenario | What it tests | How to simulate |
|---|---|---|
| **Service down** | Detection, escalation, recovery | Kill service in staging |
| **Database failure** | Failover, data consistency | Stop database; verify failover |
| **Dependency timeout** | Circuit breakers, fallbacks | Block network to dependency |
| **High traffic** | Auto-scaling, capacity planning | Load test at 2x expected peak |
| **Bad deployment** | Rollback process | Deploy broken code; test rollback |
| **Certificate expiration** | Certificate management | Use near-expired certificate |
| **Data corruption** | Backup and recovery | Corrupt test data; test restore |

### Scenario design

**Good scenarios are:**

| Quality | Example |
|---|---|
| **Realistic** | Based on real incidents or known risks |
| **Specific** | "Database connection pool exhaustion" not "database problem" |
| **Testable** | Can be safely simulated in staging or production |
| **Measurable** | Clear success criteria (e.g., "recovery within 30 minutes") |

**Bad scenarios:**

| Problem | Example | Better version |
|---|---|---|
| Vague | "Something breaks" | "Payment service becomes unresponsive" |
| Unsafe | "Delete production database" | "Stop staging database; test failover" |
| Unrealistic | "Aliens attack data center" | "Region becomes unavailable; test multi-region failover" |
| No success criteria | "See what happens" | "Recovery within 30 minutes; no data loss" |

## War Game Process

### Before the war game

#### 1. Define objectives

What do you want to test?

| Objective | Example |
|---|---|
| **Detection** | Do alerts fire? How fast? |
| **Escalation** | Does the on-call get paged? |
| **Coordination** | Is an incident commander assigned? |
| **Communication** | Are stakeholders updated? |
| **Recovery** | Can the team restore service? |
| **Documentation** | Are runbooks accurate? |

#### 2. Choose scenarios

Select 2-3 scenarios based on:

- **Recent incidents:** Test whether postmortem action items work
- **Known risks:** Test failure modes you haven't experienced yet
- **Critical services:** Test your most important services

#### 3. Prepare the environment

| Task | How |
|---|---|
| **Set up staging** | Ensure staging mirrors production |
| **Safety controls** | Add safeguards to prevent real impact |
| **Monitoring** | Ensure dashboards and alerts work |
| **Communication** | Notify participants; schedule time |

#### 4. Prepare participants

| Role | Preparation |
|---|---|
| **On-call engineers** | Review runbooks; verify access |
| **Incident commanders** | Review incident response process |
| **Observers** | Prepare to document timeline and gaps |

### During the war game

#### 1. Inject the failure

Use one of these methods:

| Method | Example |
|---|---|
| **Manual** | Stop service, block network, deploy bad code |
| **Automated** | Chaos engineering tools (Chaos Monkey, Gremlin) |
| **Simulated** | Describe scenario; team walks through response |

#### 2. Observe and document

| What to observe | How to document |
|---|---|
| **Detection time** | How long from failure to alert? |
| **Response time** | How long from alert to on-call acknowledgment? |
| **Coordination** | Was an IC assigned? Were roles clear? |
| **Communication** | Were stakeholders updated? How often? |
| **Diagnosis** | How long to identify root cause? |
| **Recovery** | How long to restore service? |
| **Gaps** | What was missing (tools, runbooks, access)? |

#### 3. Time the response

```
Failure injected: 10:00
Alert fired: 10:02 (2 minutes)
On-call acknowledged: 10:05 (5 minutes)
Incident declared: 10:07 (7 minutes)
Root cause identified: 10:20 (20 minutes)
Fix implemented: 10:25 (25 minutes)
Service recovered: 10:30 (30 minutes)
```

### After the war game

#### 1. Conduct a retrospective

Ask these questions:

- What worked well?
- What was confusing or slow?
- What was missing (tools, runbooks, access)?
- What would you do differently?
- What action items do we have?

#### 2. Document findings

```markdown
## War Game Report: [Scenario Name]

**Date:** [YYYY-MM-DD]
**Participants:** [list of participants]
**Scenario:** [description of failure injected]

### Timeline
| Time | Event |
|---|---|
| 10:00 | Failure injected |
| 10:02 | Alert fired |
| ... | ... |

### What Worked Well
- [Item 1]
- [Item 2]

### Gaps Identified
- [Gap 1]: [impact]
- [Gap 2]: [impact]

### Action Items
| Action | Owner | Priority | Due Date |
|---|---|---|---|
| [Action 1] | [@name] | High | [date] |
| [Action 2] | [@name] | Medium | [date] |

### Lessons Learned
- [Lesson 1]
- [Lesson 2]
```

#### 3. Implement action items

- Add action items to tracking tool (Jira, GitHub Issues)
- Assign owners and due dates
- Review in team standup
- Verify action items work in next war game

## War Game Frequency

| Frequency | Scope | Use when |
|---|---|---|
| **Quarterly** | Full war game (2-3 scenarios) | Mature teams; critical services |
| **Monthly** | Tabletop exercise (walk through scenario) | New teams; building skills |
| **Ad-hoc** | After major incident; test action items | Verify postmortem lessons |

## War Game Anti-Patterns

| Anti-pattern | Problem | What to do instead |
|---|---|---|
| **No war games** | Unprepared for real incidents | Conduct quarterly war games |
| **Unsafe scenarios** | Real production impact | Use staging; add safety controls |
| **No observers** | Can't document gaps | Assign observers to document timeline |
| **No retrospective** | Don't learn from gaps | Conduct retrospective after every war game |
| **No action items** | Gaps not fixed | Define action items with owners and due dates |
| **Punitive** | People afraid to participate | Make war games blameless; focus on learning |

## Practical Exercise

**For your team:**

1. **Plan a war game:**
   - Choose 2-3 realistic scenarios
   - Set up staging environment
   - Notify participants; schedule time

2. **Conduct the war game:**
   - Inject failures
   - Observe and document response
   - Time detection, diagnosis, and recovery

3. **Conduct a retrospective:**
   - What worked well?
   - What gaps were identified?
   - What action items do we have?

4. **Implement action items:**
   - Add to tracking tool
   - Assign owners and due dates
   - Review in team standup

**Bonus:** After implementing action items, conduct another war game to verify they work. Did detection improve? Did recovery time decrease?

## Knowledge Connections

- [[01_On_Call_Practices]] : war games test on-call readiness
- [[02_Incident_Management]] : war games test incident response process
- [[03_Blameless_Postmortems]] : war games test whether postmortem lessons work
- [[04_Post_Incident_Action_Items]] : war games verify action items are effective
- [[05_Capacity_and_Resilience/04_Chaos_Engineering]] : chaos engineering tests system resilience
- [[career-path/02_Senior_Software_Engineer/05_Quality_Reliability_Security/07_Chaos_Engineering]] : chaos engineering foundations

## Key Takeaways

- War games test people and process; chaos engineering tests systems and automation
- Conduct quarterly war games with realistic scenarios
- Observe and document detection time, response time, coordination, and gaps
- Conduct retrospectives after every war game; define action items
- Verify action items work in the next war game
- Make war games blameless; focus on learning, not punishment
