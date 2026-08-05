---
title: Opportunity Assessment
parent: Problem Discovery
summary: Deciding if a problem is worth solving
tags:
  - discovery
  - prioritization
  - opportunity
  - validation
---

# Opportunity Assessment

> Opportunity assessment is the systematic evaluation of whether a problem is worth solving. It prevents investing in problems that don't align with strategy, lack evidence, or have insufficient impact.

## Why Opportunity Assessment Matters

**Without assessment:**
- Building solutions for low-impact problems
- Solving problems that don't align with strategy
- Investing in problems with no evidence
- Spreading resources too thin across many problems

**With assessment:**
- Focused investment on high-impact problems
- Strategic alignment
- Evidence-based decisions
- Clear prioritization criteria

## Opportunity Assessment Framework

### 1. Problem Validation

**Is this a real problem?**

**Evidence sources:**
- User interviews and quotes
- Analytics data (drop-off, errors, time)
- Support tickets and complaints
- Competitive analysis
- Market research

**Red flags:**
- "Users say they want this" without behavioral evidence
- Problem identified by stakeholders, not users
- No data on frequency or impact
- "It would be nice to have"

**Green flags:**
- Multiple users describe the same pain
- Analytics show the problem (high abandonment, errors)
- Users have created workarounds
- Problem affects key user journeys

### 2. Strategic Alignment

**Does this problem align with our strategy?**

**Questions:**
- Does solving this advance our product vision?
- Does it support our business objectives?
- Does it serve our target market?
- Does it leverage our unique capabilities?

**Alignment matrix:**
```
High alignment + High impact = Invest
High alignment + Low impact = Consider
Low alignment + High impact = Reconsider strategy
Low alignment + Low impact = Don't invest
```

### 3. Impact Assessment

**How big is the impact?**

**User impact:**
- How many users are affected?
- How severely are they affected?
- How often does the problem occur?
- What's the user value of solving it?

**Business impact:**
- Revenue impact (increase or protect)
- Cost impact (reduce or avoid)
- Risk impact (reduce or mitigate)
- Strategic impact (enable future opportunities)

**Impact calculation:**
```
Impact = (Users affected) × (Severity) × (Frequency)

Example:
- 10,000 users affected
- High severity (blocks key workflow)
- Occurs daily

Impact = 10,000 × High × Daily = Very High
```

### 4. Feasibility Assessment

**Can we solve this problem?**

**Technical feasibility:**
- Do we have the technical capability?
- Are there technical blockers or dependencies?
- What's the technical complexity?
- Do we need new technology or skills?

**Resource feasibility:**
- Do we have the people and time?
- What's the estimated effort?
- What are the opportunity costs?
- Do we have budget?

**Organizational feasibility:**
- Do we have stakeholder support?
- Are there political or cultural barriers?
- Do we have the authority to solve this?

### 5. Risk Assessment

**What could go wrong?**

**Execution risks:**
- Technical uncertainty
- Resource constraints
- Timeline pressure
- Dependency risks

**Market risks:**
- Users won't adopt the solution
- Competitors solve it first
- Market conditions change
- Regulations change

**Business risks:**
- Solution doesn't deliver expected impact
- Unintended consequences
- Reputational damage
- Financial loss

## Opportunity Scoring Model

### Simple Scoring (1-5 scale)

| Criteria | Weight | Score | Weighted Score |
|----------|--------|-------|----------------|
| Problem validation | 25% | 4 | 1.0 |
| Strategic alignment | 25% | 5 | 1.25 |
| User impact | 20% | 4 | 0.8 |
| Business impact | 20% | 3 | 0.6 |
| Feasibility | 10% | 3 | 0.3 |
| **Total** | **100%** | | **3.95** |

**Decision thresholds:**
- 4.0-5.0: Strong opportunity, invest
- 3.0-3.9: Good opportunity, consider
- 2.0-2.9: Weak opportunity, deprioritize
- 1.0-1.9: Poor opportunity, reject

### RICE Scoring

**Reach:** How many users will this affect?
- Score: Number of users per quarter

**Impact:** How much will it affect each user?
- 3 = Massive impact
- 2 = High impact
- 1 = Medium impact
- 0.5 = Low impact
- 0.25 = Minimal impact

**Confidence:** How confident are we in our estimates?
- 100% = High confidence
- 80% = Medium confidence
- 50% = Low confidence

**Effort:** How much work is required?
- Score: Person-months

**RICE Score = (Reach × Impact × Confidence) / Effort**

**Example:**
```
Opportunity: Unified customer view for support reps

Reach: 50 reps × 150 calls/day × 60 working days = 450,000 calls/quarter
Impact: 2 (High - saves 5 minutes per call)
Confidence: 80% (strong evidence from observation)
Effort: 6 person-months

RICE Score = (450,000 × 2 × 0.8) / 6 = 120,000
```

## Opportunity Assessment Process

### 1. Gather Evidence

**For each opportunity:**
- User research findings
- Analytics data
- Support ticket analysis
- Competitive intelligence
- Market research

### 2. Score the Opportunity

**Use your scoring model:**
- Rate each criterion
- Calculate weighted score
- Document assumptions

### 3. Compare Opportunities

**Create opportunity portfolio:**
```
┌─────────────────────────────────────────┐
│ High Impact                             │
│                                         │
│    ★ Quick Wins    │    ★ Big Bets      │
│    (Do first)      │    (Invest wisely) │
│                    │                    │
├────────────────────┼────────────────────┤
│                    │                    │
│    Fill-ins        │    Money Pit       │
│    (If time)       │    (Avoid)         │
│                    │                    │
│ Low Impact                              │
└─────────────────────────────────────────┘
         Low Effort         High Effort
```

### 4. Make Decisions

**Decision criteria:**
- Score above threshold
- Strategic alignment
- Resource availability
- Portfolio balance
- Timing and dependencies

### 5. Document and Communicate

**Opportunity brief:**
```
Opportunity: [Name]
Problem: [One-sentence problem statement]
Evidence: [Key evidence supporting the problem]
Impact: [Quantified user and business impact]
Alignment: [How it supports strategy]
Feasibility: [High/Medium/Low with key constraints]
Risks: [Top 3 risks]
Score: [Overall score and recommendation]
Decision: [Invest/Consider/Reject]
Next Steps: [What to do next]
```

## Common Mistakes

### 1. Solution Bias

**Mistake:** Assessing the solution, not the problem
**Fix:** Focus on problem impact, not solution features

### 2. Optimism Bias

**Mistake:** Overestimating impact, underestimating effort
**Fix:** Use ranges, not point estimates. Apply confidence levels.

### 3. Sunk Cost Fallacy

**Mistake:** Continuing to invest because we've already started
**Fix:** Assess based on future value, not past investment

### 4. HiPPO Effect

**Mistake:** Highest Paid Person's Opinion overrides evidence
**Fix:** Use structured scoring, require evidence

### 5. Analysis Paralysis

**Mistake:** Endless assessment without decisions
**Fix:** Set time boxes, accept uncertainty, decide and learn

## Senior-Level Opportunity Assessment

1. **Assess strategic opportunities**
   - Not just product features
   - Market opportunities
   - Partnership opportunities
   - Business model opportunities

2. **Build assessment capability**
   - Create assessment frameworks
   - Train teams in assessment
   - Establish assessment gates

3. **Manage opportunity portfolio**
   - Balance short-term and long-term
   - Balance high-risk and low-risk
   - Allocate resources strategically

4. **Connect to business outcomes**
   - Link opportunities to OKRs
   - Track opportunity-to-outcome conversion
   - Measure assessment accuracy

## Metrics

- Number of opportunities assessed
- Percentage of opportunities with evidence
- Time from opportunity identification to decision
- Opportunity-to-investment conversion rate
- Success rate of invested opportunities
- Assessment accuracy (predicted vs. actual impact)

## Resources

- [[body-of-knowledge/BABOK/04_Strategy_Analysis]] - Strategy analysis
- [[body-of-knowledge/BABOK/06_Solution_Evaluation]] - Solution evaluation
- Opportunity Canvas by Jeff Gothelf
- Inspired by Marty Cagan

## Checklist

Before investing in an opportunity:
- [ ] Problem validated with evidence
- [ ] Strategic alignment confirmed
- [ ] User impact quantified
- [ ] Business impact estimated
- [ ] Feasibility assessed
- [ ] Risks identified and mitigated
- [ ] Opportunity scored and compared
- [ ] Decision documented and communicated
- [ ] Success metrics defined
- [ ] Next steps clear
