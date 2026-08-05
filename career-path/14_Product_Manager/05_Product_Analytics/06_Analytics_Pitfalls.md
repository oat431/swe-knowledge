---
title: Analytics Pitfalls
parent: Product Analytics
summary: Common mistakes in product analytics and how to avoid them
tags:
  - analytics
  - pitfalls
  - mistakes
  - best-practices
---

# Analytics Pitfalls

> Analytics is powerful, but easy to get wrong. Understanding common pitfalls helps you avoid misleading conclusions and make better decisions.

## Why Pitfalls Matter

**Ignoring pitfalls:**
- Misleading conclusions
- Bad decisions based on bad analysis
- Wasted resources
- Loss of trust in data

**Understanding pitfalls:**
- Accurate insights
- Better decisions
- Efficient resource use
- Trusted analytics

## Pitfall 1: Vanity Metrics

**Problem:**
- Metrics that look good but don't help
- Measure activity, not outcomes
- No connection to goals

**Examples:**
```
Vanity metrics:
- Total registered users (doesn't show active users)
- Page views (doesn't show engagement)
- Downloads (doesn't show usage)
- Social media followers (doesn't show impact)

Problem: These can grow while product fails
1 million registered users means nothing if 99% inactive
```

**Solution:**
- Focus on actionable metrics
- Measure outcomes, not outputs
- Connect to goals

**Example:**
```
Actionable metrics:
- Weekly active users (shows engagement)
- Feature adoption rate (shows value delivery)
- Retention rate (shows long-term value)
- Customer lifetime value (shows business impact)
```

## Pitfall 2: Survivorship Bias

**Problem:**
- Only analyzing users who stayed
- Ignoring users who left
- Missing why people churn

**Example:**
```
Survey of active users:
- 90% satisfied with product
- Conclusion: Product is great!

But:
- Ignored 50% of users who churned
- Churned users might have been very dissatisfied
- Real satisfaction might be much lower
```

**Solution:**
- Include churned users in analysis
- Analyze why users leave
- Compare active vs. churned users

**Example:**
```
Complete analysis:
- Active users: 90% satisfied
- Churned users: 30% satisfied
- Overall: 60% satisfied

Insight: Satisfaction varies greatly between groups
Need to understand why users churn
```

## Pitfall 3: Correlation vs. Causation

**Problem:**
- Assuming correlation means causation
- Not testing causal relationships
- Making changes based on correlation

**Example:**
```
Observation:
Users who use feature X have 50% higher retention

Conclusion (wrong):
Feature X causes higher retention
→ Force everyone to use feature X

Reality:
Power users use feature X AND have high retention
Feature X doesn't cause retention
Both caused by user engagement level

Forcing feature X won't improve retention
```

**Solution:**
- Use experiments to test causation
- Consider confounding variables
- Don't assume correlation = causation

**Example:**
```
Proper approach:
1. Observe correlation (feature X and retention)
2. Form hypothesis (feature X causes retention)
3. Run experiment (randomly assign feature X)
4. Measure actual causal effect

Result: Experiment shows no causal effect
Feature X doesn't cause retention
Don't force feature X
```

## Pitfall 4: Simpson's Paradox

**Problem:**
- Overall trend reverses when broken into groups
- Aggregate data hides important patterns
- Misleading conclusions from aggregates

**Example:**
```
Overall conversion rate:
- Last month: 10%
- This month: 8%
- Conclusion: Conversion getting worse!

But broken by segment:
Enterprise: 15% → 18% (improving)
SMB: 8% → 9% (improving)
Startup: 5% → 6% (improving)

What happened?
Mix shifted: More startups (low conversion) this month
Each segment improving, but overall down due to mix
```

**Solution:**
- Segment your data
- Look for Simpson's paradox
- Understand composition changes

**Example:**
```
Proper analysis:
1. Check overall trend (8% vs 10%)
2. Segment by user type
3. Analyze each segment
4. Check segment mix changes

Insight: All segments improving
Overall down due to mix shift toward lower-converting segments
Action: Focus on improving startup conversion
```

## Pitfall 5: Sample Size Issues

**Problem:**
- Drawing conclusions from small samples
- Not enough statistical power
- Over-interpreting small differences

**Example:**
```
A/B test with small sample:
Control: 10% conversion (n=100)
Variant: 12% conversion (n=100)

Conclusion: Variant is 20% better!

Reality:
With n=100, this difference could easily be random
p-value: 0.65 (not significant)
Need n=3,800 per group for reliable results
```

**Solution:**
- Calculate required sample size
- Ensure statistical significance
- Don't over-interpret small samples

**Example:**
```
Proper approach:
1. Calculate required sample size before test
2. Run test until sample size reached
3. Check statistical significance
4. Only act on significant results

Sample size calculation:
Baseline: 10% conversion
Minimum detectable effect: 2%
Power: 80%
Significance: 5%
Required: 3,800 per group
```

## Pitfall 6: Metric Manipulation

**Problem:**
- Optimizing metric without improving reality
- Gaming the system
- Metric goes up, value doesn't

**Example:**
```
Goal: Increase user engagement
Metric: Session duration

Optimization:
- Add auto-play videos
- Make navigation confusing (users spend more time)
- Add pop-ups that take time to dismiss

Result:
- Session duration up 50%
- User satisfaction down 30%
- Retention down 20%

Metric improved, product got worse
```

**Solution:**
- Use multiple metrics
- Include guardrail metrics
- Check for unintended consequences

**Example:**
```
Better approach:
Primary metric: Session duration
Guardrail metrics: User satisfaction, retention, task completion

Optimization:
- Improve content quality (increases duration AND satisfaction)
- Add valuable features (increases duration AND retention)

Result:
- Session duration up 20%
- Satisfaction up 10%
- Retention up 15%

All metrics improve together
```

## Pitfall 7: Ignoring Context

**Problem:**
- Numbers without context
- Not understanding why metrics changed
- Missing external factors

**Example:**
```
Observation:
Traffic down 30% this week

Conclusion:
Product is failing!

Reality (context):
- Holiday week (expected decline)
- Competitor launched (market noise)
- Seasonal pattern (happens every year)

Without context, wrong conclusion
```

**Solution:**
- Always provide context
- Compare to benchmarks
- Consider external factors
- Look at historical patterns

**Example:**
```
Contextual analysis:
Traffic down 30% this week

Context:
- Holiday week (historically down 25-35%)
- Same time last year: down 32%
- Competitor also down 28%

Insight: Normal seasonal decline
No action needed
Monitor for return to normal after holiday
```

## Pitfall 8: Data Quality Issues

**Problem:**
- Inaccurate or incomplete data
- Tracking bugs
- Inconsistent definitions

**Example:**
```
Problem:
Revenue looks like it doubled overnight!

Investigation:
- Tracking bug counted refunds as revenue
- Data pipeline duplicated records
- Different teams defined "revenue" differently

Reality:
Revenue actually flat
Data quality issues created false signal
```

**Solution:**
- Validate data quality
- Standardize definitions
- Monitor for anomalies
- Fix tracking issues

**Example:**
```
Data quality practices:
1. Validate data collection
2. Standardize metric definitions
3. Monitor for anomalies
4. Investigate unexpected changes
5. Fix issues before analysis

Definition standardization:
Revenue: Completed transactions, excluding refunds, 
         after discounts, before tax
```

## Avoiding Pitfalls

### 1. Question Everything

**Critical thinking:**
- Does this make sense?
- What could be wrong?
- What am I missing?
- What would change my mind?

### 2. Use Multiple Methods

**Triangulation:**
- Quantitative and qualitative
- Multiple data sources
- Different analysis techniques
- Cross-validation

### 3. Peer Review

**Get feedback:**
- Share analysis with others
- Ask for challenges
- Consider alternative explanations
- Review assumptions

### 4. Document Assumptions

**Be explicit:**
- What you assumed
- Why you assumed it
- How to validate
- When to revisit

### 5. Learn from Mistakes

**Continuous improvement:**
- Track prediction accuracy
- Analyze wrong conclusions
- Share learnings
- Improve processes

## Senior-Level Pitfall Avoidance

1. **Recognize patterns**
   - Spot pitfalls early
   - Understand root causes
   - Apply appropriate solutions

2. **Build quality processes**
   - Establish analytics standards
   - Create review processes
   - Train teams in best practices

3. **Organizational influence**
   - Address systemic issues
   - Build analytics maturity
   - Create data quality culture

4. **Continuous improvement**
   - Learn from mistakes
   - Improve methodologies
   - Share best practices

## Metrics

- Pitfall occurrence rate
- Data quality score
- Analysis accuracy (validated vs. predicted)
- Decision quality (outcomes of data-driven decisions)
- Learning rate (improvements from mistakes)

## Resources

- [[body-of-knowledge/DMBOK/11_Data_Analytics]] - Data analytics
- Calling Bullshit by Carl Bergstrom and Jevin West
- The Signal and the Noise by Nate Silver

## Checklist

Before analyzing:
- [ ] Data quality verified
- [ ] Sample size adequate
- [ ] Context understood
- [ ] Assumptions documented
- [ ] Pitfalls considered

After analyzing:
- [ ] Results make sense
- [ ] Alternative explanations considered
- [ ] Peer reviewed
- [ ] Limitations acknowledged
- [ ] Conclusions validated
- [ ] Learnings documented

