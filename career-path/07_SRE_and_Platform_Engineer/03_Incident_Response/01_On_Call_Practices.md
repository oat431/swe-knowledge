---
title: "On-Call Practices"
note_type: capability-topic
capability_area: incident-response
career_path: sre-and-platform-engineer
prerequisite:
  - "[[career-path/02_Senior_Software_Engineer/05_Quality_Reliability_Security/04_Incident_Response]]"
tags:
  - career-path
  - sre
  - platform-engineering
  - incident-response
  - on-call
---

# On-Call Practices

> **One-line definition:** Designing sustainable on-call rotations with fair compensation, clear escalation paths, and effective runbooks.

## Why This Is a Specialist Skill

A senior software engineer may participate in on-call rotations. An SRE or platform engineer **designs on-call systems that are sustainable**, **negotiates compensation and time-off policies**, and **ensures on-call engineers have the tools and documentation they need to respond effectively**.

On-call is not just "being available." It is a **structured system** that balances reliability needs with engineer well-being.

## On-Call Rotation Design

### Rotation models

| Model | Description | Pros | Cons | Use when |
|---|---|---|---|---|
| **Weekly rotation** | One person on-call for 7 days | Predictable; less context switching | Long commitment; burnout risk | Stable services; experienced teams |
| **Daily rotation** | One person on-call for 1 day | Short commitment; frequent rotation | More context switching; less ownership | High-incident services; large teams |
| **Primary + secondary** | Two people on-call; primary responds first, secondary escalates | Backup; shared load | More complex scheduling | Critical services; small teams |
| **Team-based** | Entire team on-call; anyone can respond | Flexibility; shared ownership | Unclear accountability | Mature teams; low incident rate |

### Rotation size

**Minimum:** 5 people for weekly rotation (1 week on, 4 weeks off)

**Rationale:**
- 1 week on-call
- 4 weeks off before next rotation
- Sustainable long-term
- Allows for vacations, sick days

**Smaller rotations (3-4 people):**
- More frequent on-call (every 3-4 weeks)
- Higher burnout risk
- Requires higher compensation

### Scheduling tools

| Tool | Strengths | Use when |
|---|---|---|
| **PagerDuty** | Industry standard; integrations | Most organizations |
| **OpsGenie** | Cost-effective; good features | Budget-conscious teams |
| **VictorOps** | Incident management focus | Complex incident workflows |
| **Custom scheduling** | Full control | Unique requirements |

## On-Call Compensation

### Compensation models

| Model | Description | Example |
|---|---|---|
| **Flat rate** | Fixed pay per on-call shift | $200/week |
| **Hourly rate** | Pay for time spent responding | $50/hour for incident response |
| **Time off** | Compensatory time off | 1 day off per week on-call |
| **Hybrid** | Combination of pay and time off | $100/week + 0.5 day off |

### Compensation guidelines

| Factor | Recommendation |
|---|---|
| **Frequency** | Higher compensation for more frequent rotations |
| **Incident rate** | Higher compensation for high-incident services |
| **Off-hours** | Higher compensation for nights, weekends, holidays |
| **Experience level** | Higher compensation for senior engineers |

**Industry benchmark:** $200-500/week or 1-2 days off per rotation

### Time-off policies

| Policy | Description |
|---|---|
| **Post-incident time off** | Time off after responding to a major incident |
| **Rotation time off** | Time off after completing an on-call rotation |
| **Swap policy** | Allow engineers to swap shifts with teammates |
| **Vacation coverage** | Plan for vacation coverage without overloading individuals |

## On-Call Tools and Access

### Required access

On-call engineers need:

| Tool | Purpose |
|---|---|
| **Monitoring dashboards** | See system health and alerts |
| **Log aggregation** | Query logs for diagnosis |
| **Tracing tools** | Trace requests across services |
| **Runbooks** | Documented procedures for common issues |
| **Deployment tools** | Roll back deployments if needed |
| **Communication tools** | Slack, PagerDuty, phone |
| **Escalation contacts** | Who to call for help |

### Access verification

Before on-call starts, verify access:

- [ ] Can access monitoring dashboards
- [ ] Can query logs
- [ ] Can view traces
- [ ] Can access runbooks
- [ ] Can deploy or roll back
- [ ] PagerDuty/OpsGenie is configured
- [ ] Escalation contacts are up to date

## Escalation Paths

### Escalation levels

| Level | Who | When to escalate |
|---|---|---|
| **L1: On-call engineer** | Primary on-call | First responder for all alerts |
| **L2: Secondary on-call** | Backup on-call | Primary is unavailable or needs help |
| **L3: Team lead** | Engineering manager | Incident affects multiple teams or requires coordination |
| **L4: Director** | Engineering director | Major incident with business impact |
| **L5: VP/CTO** | Executive leadership | Critical incident affecting customers or revenue |

### Escalation triggers

Escalate when:

- Incident affects multiple services or teams
- Incident has been ongoing for >30 minutes without progress
- Incident requires cross-team coordination
- Incident has significant business impact
- On-call engineer is unsure how to proceed

## Runbooks

### Runbook structure

Every service should have runbooks for common issues:

```markdown
## Runbook: [Issue Name]

### Symptoms
- [What alerts fire?]
- [What do users experience?]

### Impact
- [Who is affected?]
- [What is the business impact?]

### Investigation Steps
1. [Step 1]
2. [Step 2]
3. [Step 3]

### Resolution Steps
1. [Step 1]
2. [Step 2]
3. [Step 3]

### Escalation
- [Who to contact if stuck?]
- [When to escalate?]

### Post-Incident
- [What to check after resolution?]
- [What to document in postmortem?]
```

### Runbook maintenance

| Practice | Frequency |
|---|---|
| **Review runbooks** | Quarterly |
| **Test runbooks** | During game days |
| **Update after incidents** | After every postmortem |
| **Add new runbooks** | When new issues are discovered |

## On-Call Anti-Patterns

| Anti-pattern | Problem | What to do instead |
|---|---|---|
| **No compensation** | Burnout; resentment | Provide fair compensation (pay or time off) |
| **Too-frequent rotations** | Burnout; fatigue | Minimum 5 people for weekly rotation |
| **No runbooks** | On-call doesn't know what to do | Write runbooks for common issues |
| **No escalation path** | On-call is stuck with complex issues | Define clear escalation levels |
| **No access** | On-call can't investigate or fix issues | Verify access before on-call starts |
| **Hero culture** | Relies on individuals, not systems | Build systems and runbooks |

## Practical Exercise

**For your on-call rotation:**

1. **Audit the rotation:**
   - How many people are in the rotation?
   - How often is each person on-call?
   - Is it sustainable long-term?

2. **Review compensation:**
   - What compensation is provided (pay, time off)?
   - Is it fair for the incident rate?
   - How does it compare to industry benchmarks?

3. **Check runbooks:**
   - Are there runbooks for the top 5 most common issues?
   - Are runbooks accurate and up to date?
   - Can a new engineer follow the runbooks?

4. **Test escalation:**
   - Is the escalation path documented?
   - Are escalation contacts up to date?
   - Has escalation been tested recently?

**Bonus:** Conduct a "shadow on-call" where a new engineer shadows an experienced on-call for a week. What did they learn? What was confusing?

## Knowledge Connections

- [[02_Incident_Management]] : on-call is the first step in incident response
- [[03_Blameless_Postmortems]] : postmortems improve on-call processes
- [[05_War_Games]] : war games test on-call readiness
- [[02_Observability/04_Alerting_Strategy]] : alerts trigger on-call response
- [[career-path/02_Senior_Software_Engineer/05_Quality_Reliability_Security/04_Incident_Response]] : incident response foundations

## Key Takeaways

- On-call is a structured system, not just "being available"
- Use sustainable rotations (minimum 5 people for weekly rotation)
- Provide fair compensation (pay or time off) for on-call burden
- Ensure on-call engineers have access to tools, runbooks, and escalation paths
- Write and maintain runbooks for common issues
- Define clear escalation levels and triggers
- Avoid hero culture: build systems and runbooks, not reliance on individuals
