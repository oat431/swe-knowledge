---
title: Quality Metrics
parent: Quality Engineering
topic: Measuring quality effectively
difficulty: specialist
created: 2026-08-05
tags:
  - career-path
  - quality-engineering
  - metrics
  - measurement
---

# Quality Metrics

> **Core Principle:** You cannot improve what you do not measure. Use metrics to understand quality, identify problems, and track improvement.

## What Quality Metrics Are

Quality metrics are:
- **Quantitative measures:** Numbers that describe quality
- **Objective:** Based on data, not opinions
- **Actionable:** Lead to decisions and actions
- **Contextual:** Interpreted in context
- **Tracked over time:** Show trends, not just snapshots

## Why Metrics Matter

**Without metrics:**
- Subjective quality assessments
- Cannot identify problems
- Cannot track improvement
- Cannot justify investments
- Decisions based on gut feeling

**With metrics:**
- Objective quality assessment
- Early problem detection
- Track improvement over time
- Justify quality investments
- Data-driven decisions

## Types of Quality Metrics

### 1. Product Metrics

**Measure the product itself:**

**Defect density:**
```
Defects per 1000 lines of code (KLOC)
Target: < 1 defect per KLOC
Good indicator of code quality
```

**Defect severity distribution:**
```
% of defects by severity (critical, major, minor)
Target: < 5% critical, < 20% major, > 75% minor
Shows if serious issues are being addressed
```

**Code complexity:**
```
Cyclomatic complexity (average and max)
Target: Average < 10, Max < 20
High complexity indicates maintainability issues
```

**Code duplication:**
```
% of duplicated code
Target: < 5%
High duplication indicates DRY violations
```

**Technical debt:**
```
Time to fix all known issues
Target: Decreasing over time
Shows accumulation of shortcuts
```

### 2. Process Metrics

**Measure the development process:**

**Defect injection rate:**
```
Defects introduced per sprint or release
Target: Decreasing over time
Shows if prevention is working
```

**Defect detection efficiency:**
```
% of defects found before production
Target: > 95%
Shows effectiveness of testing
```

**Defect escape rate:**
```
% of defects that reach production
Target: < 5%
Shows quality of quality gates
```

**Cycle time:**
```
Time from start to completion
Target: Decreasing over time
Shows process efficiency
```

**Rework effort:**
```
% of time spent fixing defects
Target: < 20%
Shows cost of poor quality
```

### 3. Testing Metrics

**Measure testing effectiveness:**

**Test coverage:**
```
% of code covered by tests
Target: > 80% for unit tests
Shows how much code is tested
```

**Test execution time:**
```
Time to run test suite
Target: < 10 minutes for unit tests
Shows test efficiency
```

**Test pass rate:**
```
% of tests that pass
Target: > 98%
Shows test reliability
```

**Flaky test rate:**
```
% of tests that are flaky
Target: < 1%
Shows test stability
```

**Defect detection rate:**
```
Defects found per test hour
Target: Depends on context
Shows testing effectiveness
```

### 4. Customer Metrics

**Measure customer experience:**

**Customer-reported defects:**
```
Defects reported by customers
Target: Decreasing over time
Shows if quality meets expectations
```

**Customer satisfaction:**
```
Customer satisfaction score (CSAT)
Target: > 4.0 out of 5.0
Shows if customers are happy
```

**Net Promoter Score (NPS):**
```
Likelihood to recommend
Target: > 50
Shows customer loyalty
```

**Mean time to resolution (MTTR):**
```
Average time to fix customer issues
Target: < 24 hours for critical issues
Shows responsiveness
```

## Choosing the Right Metrics

### SMART Criteria

**Good metrics are:**
- **Specific:** Clear what is being measured
- **Measurable:** Can be quantified
- **Achievable:** Realistic targets
- **Relevant:** Related to goals
- **Time-bound:** Measured over time

### Avoid Vanity Metrics

**Vanity metrics:**
- Look good but don't help
- Examples: Total lines of code, total tests
- Problem: More is not always better

**Actionable metrics:**
- Lead to decisions and actions
- Examples: Defect density, test coverage
- Benefit: Show what to improve

### Metric Selection Framework

**Ask these questions:**

1. **What are we trying to achieve?**
   - Goal: Reduce production defects
   - Metric: Defect escape rate

2. **What will we do with this metric?**
   - If escape rate > 5%: Improve testing
   - If escape rate < 5%: Maintain current practices

3. **Who will use this metric?**
   - QA team: Track and improve
   - Management: Monitor and resource

4. **How often will we measure?**
   - Daily: For operational metrics
   - Weekly: For sprint metrics
   - Monthly: For strategic metrics

## Metrics Dashboard

### Operational Dashboard (Daily)

**For development teams:**
```
Daily Quality Dashboard
═══════════════════════════════════

Build Health:
  Builds today: 15
  Passing: 14 (93%)
  Failed: 1 (investigating)

Test Results:
  Tests run: 2,450
  Passing: 2,445 (99.8%) ✓
  Failed: 5 (3 flaky, 2 real)

Code Quality:
  New code: 500 lines
  Static analysis issues: 2 minor
  Test coverage: 87% ✓

Alerts:
  ⚠ Build #1234 failed (API tests)
  ⚠ Flaky test: test_payment_processing
```

### Sprint Dashboard (Weekly)

**For sprint reviews:**
```
Sprint 12 Quality Dashboard
═══════════════════════════════════

Defects:
  Found in sprint: 23
  Critical: 2
  Major: 8
  Minor: 13
  Escaped to production: 1 (4.3%) ✓

Testing:
  Test cases executed: 150
  Pass rate: 97%
  Coverage: 85%
  New tests added: 35

Code Quality:
  Code committed: 5,000 lines
  Defect density: 4.6 per KLOC
  Technical debt: 3 days
  Static analysis: 0 critical, 5 major

Improvements:
  Implemented: 3
  In progress: 2
  Planned: 4
```

### Strategic Dashboard (Monthly)

**For management:**
```
Monthly Quality Dashboard
═══════════════════════════════════

Quality Trends:
  [Graph: Defect density - decreasing]
  [Graph: Test coverage - increasing]
  [Graph: Customer satisfaction - stable]

Key Metrics:
  Defect density: 3.2 per KLOC (target: < 5) ✓
  Defect escape rate: 4% (target: < 5%) ✓
  Test coverage: 87% (target: > 80%) ✓
  Customer satisfaction: 4.3/5.0 (target: > 4.0) ✓
  Technical debt: 15 days (target: < 20) ✓

Cost of Quality:
  Prevention: $50,000
  Appraisal (testing): $40,000
  Internal failure (rework): $20,000
  External failure (production): $10,000
  Ratio: 4.5:1 ✓

Improvements:
  Completed: 12
  ROI: 340%
  Hours saved: 200
```

## Using Metrics Effectively

### 1. Set Baselines

**Know where you are:**
- Measure current state
- Establish baseline metrics
- Compare to industry standards
- Identify gaps

**Example:**
```
Current state:
- Defect density: 8 per KLOC
- Industry average: 5 per KLOC
- Target: < 5 per KLOC
- Gap: 3 per KLOC

Action: Improve code quality practices
```

### 2. Set Targets

**Define where you want to be:**
- Realistic but challenging
- Based on business goals
- Time-bound
- Reviewed regularly

**Example:**
```
Target: Reduce defect density from 8 to 5 per KLOC

Timeline: 6 months

Actions:
- Month 1-2: Implement code reviews
- Month 3-4: Add static analysis
- Month 5-6: Improve testing practices
```

### 3. Track Trends

**Look at direction, not just numbers:**
- Are metrics improving?
- Are we on track?
- Any unexpected changes?
- What caused changes?

**Example:**
```
Defect density trend:
- Jan: 8.0 per KLOC
- Feb: 7.5 per KLOC
- Mar: 7.0 per KLOC
- Apr: 6.5 per KLOC
- May: 6.0 per KLOC
- Jun: 5.5 per KLOC

Trend: Improving by 0.5 per month
On track to reach target in July
```

### 4. Investigate Anomalies

**When metrics change unexpectedly:**
- Investigate root cause
- Understand if good or bad
- Take corrective action if needed
- Learn from the change

**Example:**
```
Anomaly: Test coverage dropped from 85% to 70%

Investigation:
- New module added with no tests
- Old tests deleted during refactoring
- Coverage tool configuration changed

Action:
- Add tests for new module
- Restore deleted tests
- Fix coverage tool configuration
```

### 5. Avoid Gaming

**Prevent metric manipulation:**
- Focus on outcomes, not outputs
- Use multiple metrics
- Investigate suspicious patterns
- Reward quality, not just numbers

**Example:**
```
Gaming: Developer inflates test coverage
- Adds trivial tests (assert true)
- Tests don't verify behavior
- Coverage looks good but quality doesn't improve

Prevention:
- Review test quality, not just coverage
- Measure defect escape rate
- Focus on outcomes (fewer bugs)
```

## Common Metrics Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| **Too many metrics** | Overwhelm, confusion | Focus on 5-10 key metrics |
| **No context** | Numbers meaningless | Provide context and trends |
| **Gaming metrics** | Manipulation | Focus on outcomes, investigate anomalies |
| **Vanity metrics** | Look good, don't help | Use actionable metrics |
| **No action** | Measure but don't act | Define actions for each metric |
| **Inconsistent measurement** | Cannot compare | Standardize measurement methods |

## Metrics for Different Audiences

### For Developers

**Focus on:**
- Test coverage
- Code complexity
- Static analysis issues
- Build success rate
- Code review turnaround

**Frequency:** Daily

**Purpose:** Improve code quality

### For QA Team

**Focus on:**
- Defect detection rate
- Test execution time
- Test pass rate
- Flaky test rate
- Defect escape rate

**Frequency:** Daily/Weekly

**Purpose:** Improve testing effectiveness

### For Management

**Focus on:**
- Defect density
- Customer satisfaction
- Cost of quality
- Technical debt
- Improvement ROI

**Frequency:** Monthly/Quarterly

**Purpose:** Strategic decisions and resource allocation

### For Customers

**Focus on:**
- Defect escape rate
- Mean time to resolution
- Customer satisfaction
- Net Promoter Score

**Frequency:** Quarterly

**Purpose:** Build trust and transparency

## Key Takeaways

1. **Measure what matters:** Focus on metrics that drive improvement
2. **Use multiple metrics:** No single metric tells the whole story
3. **Track trends:** Direction matters more than absolute numbers
4. **Avoid gaming:** Focus on outcomes, not just numbers
5. **Act on metrics:** Measurement without action is waste

## Related Topics

- [[01_Defect_Prevention]]: Prevention metrics
- [[02_Code_Reviews]]: Review metrics
- [[04_Continuous_Improvement]]: Improvement metrics

## Existing Vault Connections

- [[software-engineering-note/12_Software_Quality/07_Quality_Metrics]]: Quality metrics and measurement
