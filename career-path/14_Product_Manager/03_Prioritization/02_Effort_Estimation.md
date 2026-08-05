---
title: Effort Estimation
parent: Prioritization
summary: Understanding the cost of delivery
tags:
  - prioritization
  - estimation
  - effort
  - cost
  - t-shirt-sizing
---

# Effort Estimation

> Effort estimation quantifies the resources required to deliver an opportunity. Combined with value assessment, it enables ROI-based prioritization: highest value per unit of effort.

## Why Effort Estimation Matters

**Without effort estimation:**
- Prioritizing by value alone
- Surprises when work takes longer
- Resource conflicts
- Unrealistic commitments

**With effort estimation:**
- ROI-based prioritization
- Realistic planning
- Better resource allocation
- Informed trade-offs

## Estimation Approaches

### 1. T-Shirt Sizing

**Relative sizing for quick comparison:**

| Size | Relative Effort | Example |
|------|-----------------|---------|
| XS | Very small | 1-2 days |
| S | Small | 1 week |
| M | Medium | 2-4 weeks |
| L | Large | 1-2 months |
| XL | Extra large | 3-6 months |
| XXL | Huge | 6+ months |

**When to use:**
- Early prioritization
- Comparing many opportunities
- Quick decisions

**Example:**
```
Opportunity A: M (2-4 weeks)
Opportunity B: L (1-2 months)
Opportunity C: S (1 week)

Priority: C, A, B (based on effort alone)
```

### 2. Story Points

**Relative complexity estimation:**

**Scale:** 1, 2, 3, 5, 8, 13, 21 (Fibonacci)

**Process:**
1. Identify reference story (e.g., "this is a 3")
2. Compare other stories to reference
3. Assign points based on relative complexity

**Factors:**
- Complexity (how hard)
- Uncertainty (how unknown)
- Effort (how much work)

**When to use:**
- Sprint planning
- Team capacity planning
- Release planning

### 3. Time-Based Estimation

**Absolute time estimates:**

**Units:**
- Hours (for small tasks)
- Days (for features)
- Weeks (for initiatives)
- Months (for programs)

**Process:**
1. Break down into tasks
2. Estimate each task
3. Add contingency (20-30%)
4. Include all roles (dev, design, QA, etc.)

**When to use:**
- Detailed planning
- Resource allocation
- Timeline commitments

**Example:**
```
Feature: Unified customer view

Breakdown:
- Design: 2 weeks
- Frontend: 3 weeks
- Backend: 4 weeks
- Integration: 2 weeks
- Testing: 2 weeks
- Deployment: 1 week

Total: 14 weeks
Contingency (20%): 3 weeks
Total with contingency: 17 weeks
```

### 4. Parametric Estimation

**Formula-based estimation:**

**Example:**
```
Integration effort = (Number of systems × 2 weeks) + 
                    (Complexity factor × 1 week)

For 3 systems with high complexity:
= (3 × 2) + (3 × 1) = 9 weeks
```

**When to use:**
- Repetitive work
- Historical data available
- Quick estimates needed

## Estimation Techniques

### 1. Bottom-Up

**Break down and sum up:**

```
Initiative: Customer service platform

Epic 1: Unified customer view
  Story 1: Customer profile page (5 points)
  Story 2: Order history integration (8 points)
  Story 3: Knowledge base integration (5 points)
  Epic total: 18 points

Epic 2: Real-time search
  Story 1: Search interface (3 points)
  Story 2: Search indexing (13 points)
  Story 3: Search results (5 points)
  Epic total: 21 points

Total: 39 points
```

**Pros:** Detailed, accurate
**Cons:** Time-consuming, requires breakdown

### 2. Top-Down

**Estimate whole, then allocate:**

```
Initiative: Customer service platform
Total estimate: 40 points (based on similar projects)

Allocation:
- Epic 1: 18 points (45%)
- Epic 2: 15 points (37%)
- Epic 3: 7 points (18%)
```

**Pros:** Fast, good for early planning
**Cons:** Less accurate, assumes similarity

### 3. Analogous

**Compare to similar past work:**

```
Past project: CRM integration
Actual effort: 8 weeks

New project: Order management integration
Similarities: Same complexity, same team
Differences: Slightly simpler API

Estimate: 6-7 weeks (slightly less than past)
```

**Pros:** Fast, uses historical data
**Cons:** Assumes similarity, may miss differences

### 4. Three-Point (PERT)

**Optimistic, pessimistic, most likely:**

```
Feature: Real-time search

Optimistic (O): 4 weeks
Most likely (M): 6 weeks
Pessimistic (P): 10 weeks

Expected = (O + 4M + P) / 6
         = (4 + 24 + 10) / 6
         = 6.3 weeks
```

**Pros:** Accounts for uncertainty
**Cons:** Requires three estimates

## Estimation Best Practices

### 1. Include All Work

**Don't forget:**
- Design and research
- Development (frontend, backend, integration)
- Testing and QA
- Documentation
- Deployment and release
- Training and support
- Project management

### 2. Account for Uncertainty

**Use:**
- Ranges (4-6 weeks, not 5 weeks)
- Confidence levels (80% confident)
- Contingency (20-30% buffer)
- Risk adjustments

### 3. Use Multiple Estimators

**Process:**
1. Independent estimates
2. Discuss differences
3. Converge on consensus

**Techniques:**
- Planning poker
- Affinity estimation
- Wideband Delphi

### 4. Track and Learn

**Process:**
1. Estimate before starting
2. Track actual effort
3. Compare estimate to actual
4. Learn and improve

**Metrics:**
- Estimation accuracy (estimate vs. actual)
- Estimation bias (consistently over/under)
- Confidence calibration

## Common Estimation Mistakes

### 1. Optimism Bias

**Mistake:** Best-case scenario estimation
**Fix:** Use ranges, add contingency, track accuracy

### 2. Ignoring Non-Development Work

**Mistake:** Only estimating coding time
**Fix:** Include design, testing, deployment, support

### 3. Not Accounting for Context Switching

**Mistake:** Assuming 100% focus
**Fix:** Account for meetings, interruptions, multitasking

### 4. Anchoring

**Mistake:** First estimate becomes baseline
**Fix:** Independent estimates, discuss before converging

### 5. False Precision

**Mistake:** "This will take 47.3 hours"
**Fix:** Use appropriate precision (ranges, t-shirt sizes)

## Estimation Artifacts

### Estimation Brief

```
Opportunity: [Name]

Estimation Approach: [T-shirt/Points/Time]

Effort Estimate: [X weeks/points]
Range: [X-Y weeks/points]
Confidence: [High/Medium/Low]%

Breakdown:
- [Component 1]: [X weeks]
- [Component 2]: [X weeks]
- [Component 3]: [X weeks]

Assumptions:
- [Assumption 1]
- [Assumption 2]

Risks:
- [Risk 1]
- [Risk 2]

Contingency: [X weeks]

Total with Contingency: [X weeks]

Basis:
- [How estimate was derived]
- [Similar past projects]
```

## ROI Calculation

**Combine value and effort:**

```
ROI = Value / Effort

Example:
Opportunity A:
- Value: $500,000
- Effort: 10 weeks
- ROI: $50,000 per week

Opportunity B:
- Value: $300,000
- Effort: 4 weeks
- ROI: $75,000 per week

Priority: B (higher ROI)
```

**Value/Effort Matrix:**
```
          Low Effort    High Effort
High      Quick Wins    Big Bets
Value     (Do first)    (Plan carefully)

Low       Fill-ins      Money Pit
Value     (If time)     (Avoid)
```

## Senior-Level Estimation

1. **Estimate strategically**
   - Not just tactical features
   - Programs and initiatives
   - Strategic bets

2. **Build estimation capability**
   - Train teams in estimation
   - Create estimation processes
   - Build historical databases

3. **Manage uncertainty**
   - Communicate uncertainty clearly
   - Plan for multiple scenarios
   - Adapt as you learn

4. **Connect estimation to outcomes**
   - Track estimation accuracy
   - Learn from estimates
   - Improve over time

## Metrics

- Estimation accuracy (% within range)
- Estimation bias (over vs. under)
- Time to estimate
- Estimation coverage (% of work estimated)
- Confidence calibration

## Resources

- [[body-of-knowledge/PMBOK/06_Schedule_Performance_Domain]] - Schedule estimation
- Software Estimation by Steve McConnell
- Agile Estimating and Planning by Mike Cohn

## Checklist

Before prioritizing:
- [ ] Estimation approach selected
- [ ] Effort estimated (with range)
- [ ] All work included (not just development)
- [ ] Uncertainty accounted for
- [ ] Confidence level assigned
- [ ] Assumptions documented
- [ ] Risks identified
- [ ] Contingency added
- [ ] Estimate validated with team
- [ ] ROI calculated (value/effort)
