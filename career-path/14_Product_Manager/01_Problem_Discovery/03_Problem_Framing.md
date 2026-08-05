---
title: Problem Framing
parent: Problem Discovery
summary: Defining the problem before jumping to solutions
tags:
  - discovery
  - problem-framing
  - problem-definition
---

# Problem Framing

> Problem framing is the discipline of clearly defining what problem you're solving before deciding how to solve it. A well-framed problem prevents wasted effort on the wrong solution.

## Why Problem Framing Matters

**Poorly framed problems lead to:**
- Building features nobody uses
- Solving symptoms instead of root causes
- Scope creep and endless iterations
- Misaligned team understanding

**Well-framed problems enable:**
- Focused solution exploration
- Clear success criteria
- Better prioritization decisions
- Shared team understanding

## Problem Framing Framework

### 1. Current State

**What is happening now?**
- Who is affected?
- What are they trying to do?
- Where does the problem occur?
- When does it happen?
- How often does it happen?

**Example:**
```
Current State:
Customer service representatives spend an average of 8 minutes 
searching for customer information across 3 different systems 
when handling support calls. This happens on every call 
(150 calls per day per rep).
```

### 2. Desired State

**What should be happening?**
- What would success look like?
- How would users feel?
- What metrics would improve?

**Example:**
```
Desired State:
Customer service representatives can access all relevant 
customer information in one view within 30 seconds, enabling 
them to handle calls efficiently and provide personalized service.
```

### 3. The Gap

**What's preventing the desired state?**
- What constraints exist?
- What's missing?
- What's broken?

**Example:**
```
Gap:
Customer information is scattered across CRM, order management, 
and knowledge base systems. There's no unified customer view, 
forcing reps to manually search and switch between systems.
```

### 4. Impact

**Why does this matter?**
- Business impact (revenue, cost, risk)
- User impact (time, frustration, satisfaction)
- Frequency and scale

**Example:**
```
Impact:
- User: 8 minutes of frustration per call, 20 hours per week
- Business: Longer call times, lower customer satisfaction, 
  higher support costs
- Scale: 150 calls/day × 50 reps = 7,500 calls/day affected
```

## Problem Statement Template

```
[User type] needs a way to [user need] because [insight].

Currently, they [current behavior] which results in [negative outcome].

The impact is [quantified impact] affecting [scale].

Success would mean [desired outcome] measured by [metrics].
```

**Example:**
```
Customer service representatives need a way to quickly access 
complete customer information because they handle time-sensitive 
support calls.

Currently, they search across 3 separate systems which results 
in 8-minute average search times and frustrated customers on hold.

The impact is 20 hours per week per rep in search time, affecting 
50 reps handling 7,500 calls per day.

Success would mean finding customer information in under 30 seconds, 
measured by average search time and customer satisfaction scores.
```

## Problem Framing Techniques

### 1. The Five Whys

Drill down to root causes:

```
Problem: Users don't complete the checkout process
Why? They abandon their carts at the payment step
Why? They're surprised by shipping costs
Why? Shipping costs aren't shown until payment
Why? The system calculates shipping based on address
Why? We don't ask for address until payment
Root cause: Shipping cost transparency issue, not checkout flow issue
```

### 2. Problem Reframing

Challenge the initial problem statement:

```
Initial: "We need to make the app faster"
Reframe: "Users feel the app is slow"
Reframe: "Users abandon tasks that take more than 3 seconds"
Reframe: "Users don't know how long tasks will take"

Different problems, different solutions
```

### 3. Jobs to Be Done

Frame around the job, not the solution:

```
Solution-focused: "Users need a search feature"
Job-focused: "Users need to find specific information quickly"

The job reveals multiple possible solutions:
- Search
- Better navigation
- Filters
- Saved searches
- Recent items
```

### 4. Stakeholder Mapping

Identify who defines the problem:

```
Problem: "Reporting is too slow"

Different stakeholders, different problems:
- Finance: "Monthly close takes 5 days instead of 3"
- Sales: "Can't get real-time pipeline data"
- Executives: "Dashboards take 10+ seconds to load"
- IT: "Reports crash the database"

One "problem" is actually four different problems
```

## Common Problem Framing Mistakes

### 1. Solution in Disguise

**Wrong:** "We need a mobile app"
**Right:** "Sales reps need to access customer data while visiting clients"

### 2. Too Broad

**Wrong:** "Improve user experience"
**Right:** "Reduce time to complete checkout from 5 minutes to 2 minutes"

### 3. Too Narrow

**Wrong:** "Add a filter for product category"
**Right:** "Users struggle to find relevant products in large catalogs"

### 4. Missing Context

**Wrong:** "Users want faster search"
**Right:** "Support agents need to find customer records in under 30 seconds during live calls"

### 5. Assuming the User

**Wrong:** "Users need better onboarding"
**Right:** "New users (first 7 days) abandon the product at 60% rate because they can't complete their first task"

## Validating Problem Frames

### 1. Evidence Check

**Ask:**
- What evidence supports this problem frame?
- What evidence contradicts it?
- What assumptions are we making?
- How can we test those assumptions?

### 2. Stakeholder Alignment

**Ask:**
- Do all stakeholders agree on the problem?
- Are we solving the right problem for the right people?
- What would success look like to each stakeholder?

### 3. Scope Check

**Ask:**
- Is this problem solvable with our resources?
- Is this problem worth solving (impact vs. effort)?
- Are we solving a symptom or the root cause?

## Problem Framing Artifacts

### Problem Canvas

```
┌─────────────────────────────────────────────────────────┐
│ Problem: [One-sentence problem statement]               │
├─────────────────────────────────────────────────────────┤
│ Current State           │ Desired State                 │
│ • What's happening now  │ • What should happen          │
│ • Who's affected        │ • How would users feel        │
│ • How often             │ • What metrics improve        │
├─────────────────────────────────────────────────────────┤
│ Impact                  │ Constraints                   │
│ • Business impact       │ • Technical constraints       │
│ • User impact           │ • Resource constraints        │
│ • Frequency/scale       │ • Time constraints            │
├─────────────────────────────────────────────────────────┤
│ Evidence                │ Assumptions                   │
│ • Data supporting this  │ • What we're assuming         │
│ • User quotes           │ • How to test                 │
└─────────────────────────────────────────────────────────┘
```

### User Story Map

```
Activity: [High-level user goal]
  ↓
Tasks: [Steps to achieve goal]
  ↓
Stories: [Specific features/needs]
  ↓
Priority: [Release planning]
```

## Senior-Level Problem Framing

1. **Frame strategic problems**
   - Not just product features
   - Business model problems
   - Market positioning problems
   - Organizational problems

2. **Challenge problem ownership**
   - "Is this our problem to solve?"
   - "Should we solve this or partner?"
   - "Is this the highest-leverage problem?"

3. **Build problem-framing capability**
   - Train teams in problem framing
   - Create problem-framing templates
   - Establish problem validation gates

4. **Connect problems to strategy**
   - Link problems to business objectives
   - Prioritize problems by strategic impact
   - Build problem portfolios

## Metrics

- Time spent in problem framing vs. solution building
- Number of problems reframed after initial definition
- Percentage of problems validated with evidence
- Team alignment on problem definition (survey)
- Success rate of solutions (did they solve the framed problem?)

## Resources

- [[career-path/02_Senior_Software_Engineer/02_Problem_Framing_and_Requirements]] - Technical problem framing
- Are Your Lights On? by Donald Gause and Gerald Weinberg
- Problem Solving 101 by Ken Watanabe

## Checklist

Before moving to solutions:
- [ ] Problem statement written and clear
- [ ] Current state documented with evidence
- [ ] Desired state defined with metrics
- [ ] Impact quantified
- [ ] Root causes identified (not just symptoms)
- [ ] Stakeholders aligned on problem
- [ ] Assumptions documented and testable
- [ ] Scope is appropriate (not too broad/narrow)
- [ ] Evidence supports the problem frame
- [ ] Team understands the problem
