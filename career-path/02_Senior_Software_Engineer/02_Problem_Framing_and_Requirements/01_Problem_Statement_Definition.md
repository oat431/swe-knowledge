---
title: "Problem Statement Definition"
note_type: capability-topic
capability_area: problem-framing
career_path: senior-software-engineer
prerequisite:
  - "[[software-engineering-note/01_Software_Requirements/01_Requirements_Fundamentals]]"
tags:
  - career-path
  - senior-engineer
  - problem-framing
  - problem-statement
---

# Problem Statement Definition

> **One-line definition:** Writing a clear, solution-free description of the problem to be solved, so the team can evaluate multiple approaches before committing to one.

## Why This Is a Senior Skill

A mid-level engineer receives a problem statement and begins designing a solution. A senior engineer **questions the problem statement** before anyone starts designing.

The most common failure mode is not building the wrong solution. It is solving the wrong problem. A problem statement that embeds a solution ("we need a microservice for notifications") locks the team into an approach before they understand the underlying need ("users are not receiving timely updates about their orders").

A senior engineer ensures the team is solving the **right problem**, not just the **stated problem**.

## The Problem Statement Template

A good problem statement has four parts:

| Component | Question it answers | Example |
|---|---|---|
| **Current state** | What is happening now? | "Users receive order confirmation emails 24-48 hours after placing an order" |
| **Impact** | Why does this matter? | "This causes 15% of users to contact support, costing $50K/month in support tickets" |
| **Desired state** | What should happen instead? | "Users receive confirmation within 5 minutes of placing an order" |
| **Constraints** | What limits our options? | "The solution must work with our existing email provider and handle 10K orders/hour" |

Notice what is **not** in this template: the solution. The problem statement describes the gap between current and desired state, not how to close it.

## Common Problem Statement Failures

### 1. The solution-in-disguise

**Bad:** "We need to migrate to Kubernetes to improve scalability"

This is not a problem statement. It is a solution statement. The problem might be:

- "Our current infrastructure cannot handle traffic spikes above 5x baseline, causing 3 outages per quarter"
- "Our deployment process takes 4 hours, preventing us from shipping more than once per week"
- "Our infrastructure costs have grown 200% while traffic grew only 50%"

Each of these problems might lead to Kubernetes, but they might also lead to different solutions. The problem statement keeps options open.

### 2. The vague complaint

**Bad:** "The system is slow"

This is not actionable. A senior engineer asks:

- Slow for whom? (all users, or specific user segments?)
- Slow compared to what? (previous version, competitors, user expectations?)
- Slow in what way? (page load, API response, batch processing?)
- How slow is too slow? (what is the acceptable threshold?)

**Better:** "Mobile users in Southeast Asia experience 8-second page load times (p95), exceeding our 3-second target and correlating with a 40% drop in conversion rate compared to users in North America"

### 3. The blame statement

**Bad:** "The ops team is not deploying our code fast enough"

This frames the problem as someone else's fault, which blocks collaboration. A senior engineer reframes:

**Better:** "Our deployment cycle time is 4 hours, which prevents us from responding to production issues within our 1-hour SLA"

This focuses on the outcome, not the blame, and opens the door for cross-team collaboration.

### 4. The symptom, not the cause

**Bad:** "Users are abandoning the checkout flow at step 3"

This describes a symptom. A senior engineer investigates the underlying cause:

- Is step 3 asking for information users do not have?
- Is step 3 taking too long to load?
- Is step 3 presenting an unexpected cost (shipping, tax)?
- Is step 3 not working on mobile devices?

**Better:** "Users abandon checkout at step 3 because the shipping cost calculator takes 12 seconds to respond, and 60% of users leave before seeing the result"

## The Five Whys Technique

When you receive a problem statement, apply the **Five Whys** to find the root cause:

```
Problem: "We need to add a cache layer to the API"

Why? → "The API is too slow"
Why? → "The database queries take 2-3 seconds"
Why? → "The queries are not using indexes"
Why? → "The schema was designed without considering query patterns"
Why? → "The schema was designed by a different team that did not consult us"

Root cause: Cross-team schema changes happen without involving the teams that query the data
```

The solution to the root cause (establish a schema change review process) is different from the solution to the stated problem (add a cache). Both might be needed, but addressing only the symptom leaves the underlying problem intact.

## Problem Framing in Practice

### The problem framing workshop

For significant initiatives, a senior engineer facilitates a problem framing workshop before design begins:

**Participants:** Product manager, key stakeholders, engineering team (2-3 people)

**Agenda (60-90 minutes):**

1. **Current state mapping** (20 min): What is happening now? What data do we have?
2. **Impact quantification** (15 min): Who is affected? How much does it cost? What is the opportunity cost?
3. **Desired state definition** (15 min): What should happen instead? How will we know we succeeded?
4. **Constraint identification** (10 min): What limits our options (technical, budget, timeline, regulatory)?
5. **Assumption surfacing** (15 min): What are we assuming? What do we need to validate?
6. **Problem statement drafting** (15 min): Write the problem statement together, iterate until aligned

**Output:** A written problem statement that everyone agrees on, posted in the project documentation

### The problem statement review

Before any significant design work begins, a senior engineer reviews the problem statement:

```markdown
## Problem Statement Review Checklist

- [ ] The problem statement does not mention a specific solution
- [ ] The current state is described with data, not opinions
- [ ] The impact is quantified (cost, users affected, time lost)
- [ ] The desired state is measurable (how will we know we succeeded?)
- [ ] Constraints are explicit (what limits our options?)
- [ ] Assumptions are listed (what are we taking for granted?)
- [ ] Key stakeholders have reviewed and agreed on the statement
```

## When to Reframe

Problem framing is not a one-time activity. A senior engineer revisits the problem statement when:

- **New information emerges:** User research, market changes, or technical discoveries that change the context
- **The solution is not working:** If the chosen approach is not delivering the expected outcomes, the problem may be misframed
- **Stakeholders disagree:** If the team is building something but stakeholders are not satisfied, the problem statement may not reflect their actual need
- **The scope is creeping:** If features keep getting added, the original problem statement may be too narrow

Reframing is not failure. It is learning. A senior engineer says: "Based on what we have learned, we need to update our problem statement" rather than continuing to build toward a problem that no longer exists.

## Practical Exercise

**Take a current project or feature you are working on and:**

1. Write the problem statement as it was originally stated to you
2. Identify which of the four failures it has (solution-in-disguise, vague, blame, symptom)
3. Apply the Five Whys to find the root cause
4. Rewrite the problem statement using the four-component template
5. Share it with a stakeholder and ask: "Does this accurately describe the problem we are solving?"

**Bonus:** Find a project from the past year that did not deliver the expected outcome. Write a retrospective problem statement. What was the actual problem? Did the team solve it?

## Knowledge Connections

- [[02_Current_and_Future_State]] : the problem statement defines the gap between current and future state
- [[03_Stakeholder_Management]] : stakeholders often have different views of what the problem is
- [[06_Ambiguity_Reduction]] : problem framing is the first step in reducing ambiguity
- [[software-engineering-note/01_Software_Requirements/01_Requirements_Fundamentals]] : requirements fundamentals
- [[body-of-knowledge/BABOK/04_Strategy_Analysis]] : BABOK strategy analysis and business needs

## Key Takeaways

- A problem statement describes the gap between current and desired state, not the solution
- The four components: current state, impact, desired state, constraints
- Watch for the four failures: solution-in-disguise, vague complaint, blame statement, symptom-not-cause
- Use the Five Whys to find the root cause behind the stated problem
- Problem framing is continuous: revisit the problem statement when new information emerges or the solution is not working
- A problem framing workshop aligns the team before design begins
