---
title: Data Analysis
parent: Product Analytics
summary: Quantitative analysis techniques for product decisions
tags:
  - analytics
  - data-analysis
  - quantitative
  - statistics
---

# Data Analysis

> Raw data is noise. Analysis transforms data into insights. The right analysis technique reveals patterns, validates hypotheses, and supports better decisions.

## Why Data Analysis Matters

**Without analysis:**
- Data without insights
- Misinterpretation of patterns
- Decisions based on anecdotes
- Missed opportunities

**With analysis:**
- Insights from data
- Validated hypotheses
- Evidence-based decisions
- Discovered opportunities

## Fundamental Analysis Techniques

### 1. Descriptive Statistics

**Summarize data:**

**Central tendency:**
- Mean (average)
- Median (middle value)
- Mode (most common)

**Dispersion:**
- Range (min to max)
- Variance (spread)
- Standard deviation (typical deviation)

**Distribution:**
- Histograms
- Percentiles
- Quartiles

**Example:**
```
User session duration analysis:

Mean: 8.5 minutes
Median: 6 minutes
Mode: 3 minutes

Insight: Distribution is right-skewed
Most users have short sessions (3 min)
Some users have very long sessions
Median (6 min) better represents typical user

Action: Investigate why some sessions are very long
(May indicate confusion or deep engagement)
```

### 2. Trend Analysis

**Patterns over time:**

**Time series analysis:**
- Daily/weekly/monthly trends
- Seasonal patterns
- Growth rates
- Anomalies

**Example:**
```
Daily active users (last 90 days):

Week 1-4: 1,000 DAU (baseline)
Week 5-8: 1,200 DAU (+20% after feature launch)
Week 9-12: 1,150 DAU (-4% from peak, still +15% from baseline)

Insight: Feature launch increased engagement
Initial spike, then stabilized at higher level
Sustained improvement, not just novelty

Action: Feature is working as intended
Continue monitoring for long-term retention
```

### 3. Segmentation

**Break down by groups:**

**Common segments:**
- User type (new vs. returning)
- Demographics (age, location)
- Behavior (high vs. low engagement)
- Cohort (signup date)

**Example:**
```
Feature adoption by user type:

New users (< 1 month): 30% adoption
Established users (1-6 months): 60% adoption
Power users (> 6 months): 85% adoption

Insight: Adoption increases with tenure
New users may not discover feature
Or may not need it yet

Action: Improve feature discovery for new users
Consider onboarding flow that highlights feature
```

### 4. Correlation Analysis

**Relationships between variables:**

**Correlation coefficient:**
- -1 to +1 scale
- Positive correlation (both increase)
- Negative correlation (one increases, other decreases)
- No correlation (no relationship)

**Important:** Correlation ≠ causation

**Example:**
```
Correlation analysis:

Feature usage vs. retention: r = 0.72 (strong positive)
Session duration vs. satisfaction: r = 0.15 (weak)
Page load time vs. bounce rate: r = 0.65 (moderate positive)

Insight: Feature usage strongly correlates with retention
But does feature cause retention, or do retained users
use feature more?

Action: Run experiment to test causation
Randomly assign users to feature vs. no feature
Measure retention difference
```

## Advanced Analysis Techniques

### 1. Cohort Analysis

**Track groups over time:**

**Process:**
- Group users by shared characteristic (signup date)
- Track behavior over time
- Compare cohorts

**Example:**
```
Retention by signup month:

Jan cohort: 100% → 80% → 70% → 65% → 62%
Feb cohort: 100% → 82% → 72% → 68% → 65%
Mar cohort: 100% → 85% → 75% → 70% → ?

Insight: Each cohort retains better than previous
Product improvements working
Retention improving over time

Action: Continue current strategy
Investigate what changed between Jan and Mar
```

### 2. Funnel Analysis

**Track user progression:**

**Process:**
- Define funnel steps
- Measure conversion at each step
- Identify drop-off points

**Example:**
```
Onboarding funnel:

Step 1: Sign up - 1,000 users (100%)
Step 2: Verify email - 800 users (80%)
Step 3: Complete profile - 600 users (60%)
Step 4: Create first project - 400 users (40%)
Step 5: Invite team member - 200 users (20%)

Drop-off analysis:
- Email verification: 20% drop-off (acceptable)
- Profile completion: 25% drop-off (investigate)
- First project: 33% drop-off (major issue!)
- Team invite: 50% drop-off (investigate)

Insight: Major drop-off at "create first project"
Users sign up but don't use core feature

Action: Investigate why users don't create projects
- Is it hard to find?
- Is it confusing?
- Do they not understand value?

Run user research and usability testing
```

### 3. Regression Analysis

**Predict outcomes:**

**Process:**
- Identify dependent variable (outcome)
- Identify independent variables (predictors)
- Build model
- Validate and use

**Example:**
```
Predicting customer lifetime value:

Dependent: LTV (customer lifetime value)
Independent: 
- Initial purchase amount
- Purchase frequency
- Support tickets
- Feature usage

Model: LTV = 50 + 2.5*(initial purchase) + 20*(frequency) 
       - 15*(support tickets) + 10*(feature usage)

R² = 0.75 (explains 75% of variance)

Insight: Purchase frequency and feature usage drive LTV
Support tickets reduce LTV

Action: Focus on increasing purchase frequency
Improve feature adoption
Reduce support tickets through better UX
```

### 4. Statistical Significance

**Is difference real or random?**

**Concepts:**
- p-value (probability result is random)
- Confidence interval (range of likely values)
- Sample size (larger = more reliable)

**Example:**
```
A/B test results:

Control: 10% conversion (n=1000)
Variant: 12% conversion (n=1000)

p-value: 0.03 (< 0.05 threshold)
95% CI for difference: 0.5% to 3.5%

Insight: Statistically significant difference
Variant likely truly better
Expected improvement: 0.5% to 3.5%

Action: Implement variant
Expected impact: 0.5-3.5% conversion improvement
```

## Data Analysis Process

### 1. Define Question

**What are you trying to learn?**
- Hypothesis to test
- Pattern to understand
- Decision to support

**Example:**
```
Question: Does the new search feature improve user retention?
Hypothesis: Users who use search retain better
Decision: Should we invest more in search features?
```

### 2. Gather Data

**Ensure:**
- Relevant data collected
- Sufficient sample size
- Data quality
- Representative sample

### 3. Clean Data

**Address:**
- Missing values
- Outliers
- Errors
- Inconsistencies

### 4. Analyze

**Choose appropriate technique:**
- Descriptive (what happened?)
- Diagnostic (why did it happen?)
- Predictive (what will happen?)
- Prescriptive (what should we do?)

### 5. Interpret

**Consider:**
- Statistical significance
- Practical significance
- Context and limitations
- Alternative explanations

### 6. Act

**Turn insights into action:**
- Clear recommendations
- Prioritized actions
- Measurable outcomes

## Common Analysis Mistakes

### 1. Correlation vs. Causation

**Mistake:** Assuming correlation means causation
**Fix:** Use experiments to test causation

### 2. Small Sample Size

**Mistake:** Drawing conclusions from small samples
**Fix:** Ensure adequate sample size

### 3. Ignoring Context

**Mistake:** Numbers without context
**Fix:** Always provide context and benchmarks

### 4. Cherry-Picking

**Mistake:** Selecting data that supports preconception
**Fix:** Look at all relevant data

### 5. Over-Analysis

**Mistake:** Analysis paralysis
**Fix:** Good enough analysis, timely decisions

## Senior-Level Data Analysis

1. **Advanced techniques**
   - Not just basic statistics
   - Machine learning applications
   - Causal inference methods

2. **Analysis culture**
   - Build analytical capabilities
   - Train teams
   - Establish standards

3. **Strategic analysis**
   - Not just feature analysis
   - Business outcome analysis
   - Market analysis

4. **Analysis quality**
   - Ensure rigor
   - Validate findings
   - Reproduce results

## Metrics

- Analysis frequency (analyses per month)
- Analysis quality (validation rate)
- Action rate (% of analyses drive action)
- Decision quality (outcomes of data-driven decisions)
- Analysis speed (time from question to insight)

## Resources

- [[body-of-knowledge/DMBOK/11_Data_Analytics]] - Data analytics
- Naked Statistics by Charles Wheelan
- Thinking, Fast and Slow by Daniel Kahneman

## Checklist

Before analyzing:
- [ ] Question clearly defined
- [ ] Data gathered and cleaned
- [ ] Appropriate technique selected
- [ ] Sample size adequate
- [ ] Limitations understood

After analyzing:
- [ ] Results interpreted correctly
- [ ] Statistical significance checked
- [ ] Practical significance considered
- [ ] Context provided
- [ ] Recommendations clear
- [ ] Actions defined

