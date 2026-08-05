---
title: "Blameless Postmortems"
note_type: capability-topic
capability_area: incident-response
career_path: sre-and-platform-engineer
prerequisite:
  - "[[02_Incident_Management]]"
tags:
  - career-path
  - sre
  - platform-engineering
  - incident-response
  - postmortem
---

# Blameless Postmortems

> **One-line definition:** Conducting structured reviews of incidents that focus on systemic causes and learning, not individual blame.

## Why This Is a Specialist Skill

A senior software engineer may participate in incident reviews. An SRE or platform engineer **leads postmortem facilitation**, **creates a culture of psychological safety**, and **ensures postmortems drive systemic improvements rather than finger-pointing**.

The difference is not technical skill. It is **facilitation and culture**: creating an environment where people share what happened honestly because they trust the process.

## What "Blameless" Means

Blameless does not mean **accountability-free**. It means:

| Blameless | Not blameless |
|---|---|
| "What systemic factors allowed this to happen?" | "Who made the mistake?" |
| "How can we prevent this class of error?" | "How do we punish the person?" |
| "What did the system allow?" | "What did the person do wrong?" |
| "What information was missing?" | "Why didn't they know better?" |

**Key principle:** People make decisions that seem reasonable at the time, given the information they have. If a decision leads to an incident, the question is: what made that decision seem reasonable?

## The Postmortem Timeline

### When to conduct a postmortem

| Severity | Postmortem required? | Timeline |
|---|---|---|
| **SEV-1 (Critical)** | Always | Within 48 hours |
| **SEV-2 (Major)** | Always | Within 1 week |
| **SEV-3 (Minor)** | Optional (for learning) | Within 2 weeks |
| **SEV-4 (Low)** | No | N/A |

### Postmortem participants

| Role | Why they attend |
|---|---|
| **Incident Commander** | Provides coordination perspective |
| **Technical Lead** | Provides technical details and root cause |
| **On-call engineer** | Provides first-responder perspective |
| **Affected team members** | Provide context on what they experienced |
| **SRE/Platform engineer** | Facilitates; identifies systemic issues |
| **Engineering manager** | Ensures action items are resourced |

## Postmortem Structure

### Postmortem template

```markdown
# Postmortem: [Incident Title]

**Date:** [YYYY-MM-DD]
**Severity:** SEV-1 / SEV-2 / SEV-3
**Duration:** [start time to resolution]
**Impact:** [who was affected, how, for how long]
**Author:** [postmortem author]
**Reviewers:** [who reviewed this postmortem]

## Summary
[2-3 sentence summary of what happened]

## Impact
- **Users affected:** [number or percentage]
- **Duration:** [how long]
- **Business impact:** [revenue, data, reputation]
- **Error budget consumed:** [how much]

## Timeline
| Time (UTC) | Event |
|---|---|
| 10:00 | Alert fired: error rate > 5% |
| 10:05 | On-call acknowledged alert |
| 10:10 | Incident declared; IC assigned |
| 10:15 | Investigation started |
| 10:30 | Root cause identified: bad config deployment |
| 10:35 | Rollback initiated |
| 10:40 | Rollback complete; service recovering |
| 10:50 | Service fully recovered |
| 11:00 | Incident closed |

## Root Cause
[What caused the incident? Focus on systemic factors, not individual actions]

## Contributing Factors
- [Factor 1]: [how it contributed]
- [Factor 2]: [how it contributed]
- [Factor 3]: [how it contributed]

## Detection
- How was the incident detected? (alert, user report, manual)
- Could detection have been faster? How?

## Resolution
- What action resolved the incident?
- Why was this action chosen?
- What alternatives were considered?

## Action Items
| Action | Owner | Priority | Due date |
|---|---|---|---|
| [Action 1] | [@name] | High | [date] |
| [Action 2] | [@name] | Medium | [date] |
| [Action 3] | [@name] | Low | [date] |

## Lessons Learned
- [Lesson 1]
- [Lesson 2]
- [Lesson 3]

## Appendix
- [Links to dashboards, logs, traces, alerts]
- [Related incidents]
- [Related postmortems]
```

## The Five Whys

Use the "Five Whys" technique to dig past symptoms to systemic causes:

```
Problem: Service was down for 30 minutes.

Why 1: Why was the service down?
→ Database connection pool was exhausted.

Why 2: Why was the connection pool exhausted?
→ A new deployment increased database queries by 10x.

Why 3: Why did the deployment increase queries?
→ A new feature was missing an index on a frequently queried column.

Why 4: Why was the missing index not caught?
→ Load testing was not part of the deployment process.

Why 5: Why was load testing not part of the deployment process?
→ No automated load testing in the CI/CD pipeline.

Root cause: No automated load testing in CI/CD pipeline.
Action: Add automated load testing to deployment process.
```

**Key insight:** The first "why" leads to a symptom. The fifth "why" leads to a systemic fix.

## Facilitating a Blameless Postmortem

### Ground rules

Start every postmortem with these ground rules:

1. **We assume everyone did their best** given the information they had at the time.
2. **We focus on systems, not individuals.** What allowed this to happen?
3. **We share openly.** Hiding information prevents learning.
4. **We disagree constructively.** Different perspectives are valuable.
5. **We commit to action.** Postmortems without action items are just meetings.

### Facilitation techniques

| Technique | How to use |
|---|---|
| **Timeline review** | Walk through the timeline together; fill in gaps |
| **Five Whys** | Dig past symptoms to systemic causes |
| **"What would you do differently?"** | Ask participants what they would change |
| **"What surprised you?"** | Identify assumptions that were wrong |
| **"What would have helped?"** | Identify missing tools, documentation, or processes |

### Handling blame

If someone starts blaming an individual:

**Redirect:** "Let's focus on what allowed this to happen, not who did it."

**Reframe:** "What information was missing that made this decision seem reasonable?"

**Systematize:** "What process or tool could prevent this class of error?"

## Postmortem Quality

### Signs of a good postmortem

| Quality | Indicator |
|---|---|
| **Blameless** | No names in root cause; focus on systems |
| **Specific** | Concrete timeline with timestamps |
| **Thorough** | Five Whys used; multiple contributing factors identified |
| **Actionable** | Action items with owners and due dates |
| **Shared** | Distributed to the organization; not hidden |
| **Followed up** | Action items tracked and completed |

### Signs of a bad postmortem

| Problem | Indicator |
|---|---|
| **Blameful** | "John made a mistake" in root cause |
| **Vague** | "Something went wrong" without details |
| **Shallow** | Root cause is a symptom, not a systemic issue |
| **No actions** | No action items or no owners |
| **Hidden** | Not shared with the organization |
| **Ignored** | Action items never completed |

## Postmortem Anti-Patterns

| Anti-pattern | Problem | What to do instead |
|---|---|---|
| **Blameful postmortem** | People hide mistakes; no learning | Use blameless language; focus on systems |
| **No postmortem** | Same issues recur | Always conduct postmortem for SEV-1 and SEV-2 |
| **Postmortem too late** | Details forgotten; lessons lost | Conduct within 48 hours for SEV-1 |
| **No action items** | No improvement | Define action items with owners and due dates |
| **Action items ignored** | No improvement | Track action items; review in team meetings |
| **Postmortem not shared** | Other teams don't learn | Share postmortems across the organization |

## Practical Exercise

**For a recent incident:**

1. **Write a postmortem:**
   - Use the postmortem template above
   - Include a detailed timeline
   - Use the Five Whys to find the root cause

2. **Facilitate the postmortem meeting:**
   - Start with ground rules
   - Walk through the timeline together
   - Identify contributing factors and action items

3. **Share the postmortem:**
   - Distribute to the team and related teams
   - Add to your postmortem archive
   - Follow up on action items

**Bonus:** Review a postmortem from 6 months ago. Were the action items completed? Did the same issue recur?

## Knowledge Connections

- [[02_Incident_Management]] : postmortems follow incident response
- [[04_Post_Incident_Action_Items]] : action items are the output of postmortems
- [[05_War_Games]] : war games test whether postmortem lessons are effective
- [[01_Service_Objectives/03_Error_Budget_Policy]] : incidents consume error budget
- [[career-path/02_Senior_Software_Engineer/05_Quality_Reliability_Security/04_Incident_Response]] : incident response foundations

## Key Takeaways

- Blameless means focusing on systems, not individuals; it does not mean accountability-free
- Conduct postmortems within 48 hours for SEV-1, within 1 week for SEV-2
- Use the Five Whys to dig past symptoms to systemic causes
- Every postmortem must have action items with owners and due dates
- Share postmortems across the organization so other teams can learn
- Track action items and verify they are completed
