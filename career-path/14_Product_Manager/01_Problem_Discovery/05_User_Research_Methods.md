---
title: User Research Methods
parent: Problem Discovery
summary: Choosing the right research approach
tags:
  - discovery
  - research
  - methods
  - qualitative
  - quantitative
---

# User Research Methods

> Different research methods answer different questions. Choosing the right method prevents wasted effort and ensures you get the insights you need.

## Research Method Categories

### Qualitative vs. Quantitative

**Qualitative research:**
- Answers "why" and "how"
- Small sample sizes (5-20 participants)
- Rich, detailed insights
- Exploratory and generative
- Examples: Interviews, observation, usability testing

**Quantitative research:**
- Answers "what" and "how much"
- Large sample sizes (100+ participants)
- Statistical insights
- Confirmatory and evaluative
- Examples: Surveys, analytics, A/B tests

### Attitudinal vs. Behavioral

**Attitudinal:** What people say
- Surveys, interviews, focus groups
- Reveals beliefs, opinions, preferences

**Behavioral:** What people do
- Observation, analytics, A/B tests
- Reveals actual behavior and patterns

## Research Methods by Question

### "What do users need?"

**Exploratory research:**

| Method | When to Use | Sample Size | Time |
|--------|-------------|-------------|------|
| Customer interviews | Understanding needs and context | 5-10 | 2-4 weeks |
| Contextual inquiry | Understanding workflows | 3-5 | 2-3 weeks |
| Diary studies | Understanding behavior over time | 5-15 | 2-4 weeks |
| Ethnographic study | Deep cultural understanding | 1-5 | 1-3 months |

### "How do users behave?"

**Observational research:**

| Method | When to Use | Sample Size | Time |
|--------|-------------|-------------|------|
| Contextual inquiry | Natural environment observation | 3-5 | 2-3 weeks |
| Fly on the wall | Unobtrusive observation | Varies | 1-2 weeks |
| Session recording | Remote behavior analysis | 50-100 | Ongoing |
| Heatmaps | Visual behavior patterns | 1000+ | Ongoing |

### "Can users use it?"

**Usability research:**

| Method | When to Use | Sample Size | Time |
|--------|-------------|-------------|------|
| Moderated usability | Detailed feedback | 5-8 | 1-2 weeks |
| Unmoderated usability | Quick validation | 10-20 | 1 week |
| Guerrilla testing | Rapid feedback | 5-10 | 1-2 days |
| A/B testing | Statistical comparison | 1000+ | 2-4 weeks |

### "What do users think?"

**Attitudinal research:**

| Method | When to Use | Sample Size | Time |
|--------|-------------|-------------|------|
| Surveys | Broad feedback | 100-1000 | 1-2 weeks |
| Focus groups | Group dynamics | 6-10 | 2-3 weeks |
| Card sorting | Information architecture | 15-30 | 1-2 weeks |
| Tree testing | Navigation testing | 50-100 | 1 week |

### "What's happening?"

**Analytical research:**

| Method | When to Use | Sample Size | Time |
|--------|-------------|-------------|------|
| Product analytics | Behavior patterns | All users | Ongoing |
| Funnel analysis | Conversion issues | All users | 1 week |
| Cohort analysis | Retention patterns | All users | 1-2 weeks |
| Segmentation | User group differences | All users | 1-2 weeks |

## Choosing a Method

### Decision Framework

```
1. What question are you trying to answer?
   → Choose method category

2. What stage are you in?
   Discovery → Qualitative, exploratory
   Design → Qualitative, evaluative
   Validation → Quantitative, confirmatory
   Optimization → Quantitative, experimental

3. What resources do you have?
   Time: Days → Guerrilla testing
         Weeks → Formal usability
         Months → Longitudinal studies
   
   Budget: Low → Analytics, surveys
           Medium → Usability testing
           High → Ethnographic studies
   
   Access: Easy → Online methods
           Hard → Remote methods

4. What confidence do you need?
   Directional → Qualitative (5-10 users)
   Statistical → Quantitative (100+ users)
```

### Method Selection Matrix

| Question | Discovery | Design | Validation | Optimization |
|----------|-----------|--------|------------|--------------|
| What do users need? | Interviews, Contextual inquiry | - | Surveys | - |
| How do users behave? | Observation, Diary studies | - | Analytics | A/B tests |
| Can users use it? | - | Usability testing | Usability testing | A/B tests |
| What do users think? | Interviews, Focus groups | Card sorting | Surveys | NPS, CSAT |
| What's happening? | - | - | Analytics | Funnel analysis |

## Research Planning

### Research Plan Template

```
Research Objective:
[What do you want to learn?]

Research Questions:
1. [Specific question 1]
2. [Specific question 2]
3. [Specific question 3]

Method:
[Chosen method and rationale]

Participants:
- Who: [Target participants]
- How many: [Sample size]
- Recruitment: [How to find them]

Timeline:
- Preparation: [X days]
- Execution: [X days]
- Analysis: [X days]
- Total: [X weeks]

Deliverables:
- [Insight report]
- [Recommendations]
- [Presentation]

Resources:
- People: [Who's involved]
- Tools: [What tools needed]
- Budget: [Cost estimate]
```

### Sample Size Guidelines

**Qualitative research:**
- 5 users: Find 85% of usability problems
- 8-10 users: Reach saturation for interviews
- 15-20 users: Card sorting, tree testing

**Quantitative research:**
- 100 responses: Survey with ±10% margin of error
- 400 responses: Survey with ±5% margin of error
- 1000+ responses: Statistical significance for A/B tests

## Combining Methods

### Triangulation

Use multiple methods to validate findings:

```
Example: Understanding checkout abandonment

1. Analytics (Quantitative):
   - 70% abandon at payment step
   - Average time on payment page: 3 minutes

2. Interviews (Qualitative):
   - "I'm not sure if my payment is secure"
   - "I don't know what the total will be"

3. Usability testing (Qualitative):
   - Users struggle to find security badges
   - Users don't see order summary

Triangulated insight:
Users abandon because they lack confidence in payment security 
and can't see the total cost before committing.
```

### Sequential Methods

Use methods in sequence:

```
Phase 1: Discovery (Qualitative)
- Interviews to understand needs
- Observation to understand workflows

Phase 2: Design (Qualitative)
- Card sorting for information architecture
- Usability testing of prototypes

Phase 3: Validation (Quantitative)
- Surveys to validate findings at scale
- Analytics to measure behavior

Phase 4: Optimization (Quantitative)
- A/B testing to optimize solutions
- Funnel analysis to measure impact
```

## Common Research Mistakes

### 1. Method Mismatch

**Mistake:** Using surveys to understand "why"
**Fix:** Use interviews for "why", surveys for "what"

### 2. Small Sample Quantitative

**Mistake:** Running A/B test with 50 users
**Fix:** Ensure statistical power (100+ per variant)

### 3. Large Sample Qualitative

**Mistake:** Interviewing 100 users
**Fix:** 8-10 interviews reach saturation, save resources

### 4. Confirmation Bias

**Mistake:** Only seeking evidence that supports hypothesis
**Fix:** Actively seek disconfirming evidence

### 5. Ignoring Context

**Mistake:** Lab testing without real-world context
**Fix:** Use contextual inquiry, field studies

## Senior-Level Research Practices

1. **Build research capability**
   - Train team members in research methods
   - Create research repositories
   - Establish research operations

2. **Design research programs**
   - Continuous research, not one-off projects
   - Mixed methods approach
   - Longitudinal studies

3. **Influence with research**
   - Translate research into decisions
   - Build research-driven culture
   - Connect research to business outcomes

4. **Advance research practice**
   - Develop new methods
   - Share learnings externally
   - Mentor junior researchers

## Metrics

- Research studies conducted (by method)
- Time from research question to insight
- Insights generated per study
- Decisions influenced by research
- Research-to-action conversion rate
- Research repository usage

## Resources

- [[body-of-knowledge/BABOK/02_Elicitation_and_Collaboration]] - Elicitation methods
- Just Enough Research by Erika Hall
- Interviewing Users by Steve Portigal
- Rocket Surgery Made Easy by Steve Krug

## Checklist

Before starting research:
- [ ] Research questions clearly defined
- [ ] Appropriate method selected
- [ ] Sample size determined
- [ ] Participants recruited
- [ ] Research plan documented
- [ ] Team aligned on objectives

After research:
- [ ] Data analyzed systematically
- [ ] Insights documented
- [ ] Findings shared with stakeholders
- [ ] Recommendations made
- [ ] Next steps defined
- [ ] Learnings added to repository
