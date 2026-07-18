---
tags: [measurement, metrics, gqm, engineering-foundations, swebok]
---

# Measurement Theory

> *Source: SWEBOK v4 Engineering Foundations KA; Fenton & Pfleeger Software Metrics*

## Purpose

Measurement theory and practice are fundamental to engineering. Without measurement, you can't know if you're improving, compare alternatives, or make evidence-based decisions. This note covers the theoretical foundations that make software measurement valid and useful.

## Operational Definitions

**The most important concept in measurement:** An operational definition specifies exactly how a measurement is performed — so anyone can reproduce it.

**Bad:** "Code complexity is how hard the code is to understand."
**Good:** "Cyclomatic complexity = number of decision points + 1, counted per method using the control flow graph."

**Three requirements for an operational definition:**
1. **Procedure** — How to perform the measurement (step by step)
2. **Criteria** — How to interpret the result (pass/fail, thresholds)
3. **Context** — Under what conditions the measurement applies

> **Without operational definitions, measurements are meaningless.** Two people measuring the same thing will get different results.

## The Four Measurement Scales

Measurement scales form a strict hierarchy — each level inherits all valid operations of levels below, plus new ones.

| Scale | Properties | Valid Operations | Examples |
|---|---|---|---|
| **Nominal** | Categories only | Count, mode, chi-square | Language (Java/Python), defect type (logic/UI/data) |
| **Ordinal** | Ordered categories | Median, percentile, rank correlation | Severity (low/medium/high), CMMI maturity level |
| **Interval** | Equal intervals, no true zero | Mean, standard deviation, correlation | Temperature (°C), calendar dates |
| **Ratio** | Equal intervals + true zero | All arithmetic, geometric mean | Lines of code, execution time, defect count |
| **Absolute** | Ratio + only one meaningful scale | All operations | Count of modules, count of defects |

### Critical Rules

> **Never compute means on ordinal data.** Averaging CMMI levels (2 + 4) / 2 = 3 is mathematically meaningless — the distance between levels 1-2 ≠ distance between 3-4.

> **Programming languages ignore measurement theory.** `int` and `float` are ratio scales in the language — nothing prevents you from averaging ordinal data in code. Watch for this in data processing.

**Valid operations by scale:**

| Operation | Nominal | Ordinal | Interval | Ratio |
|---|---|---|---|---|
| Count (frequency) | ✅ | ✅ | ✅ | ✅ |
| Mode | ✅ | ✅ | ✅ | ✅ |
| Median | ❌ | ✅ | ✅ | ✅ |
| Percentile | ❌ | ✅ | ✅ | ✅ |
| Mean | ❌ | ❌ | ✅ | ✅ |
| Standard deviation | ❌ | ❌ | ✅ | ✅ |
| Coefficient of variation | ❌ | ❌ | ❌ | ✅ |
| Geometric mean | ❌ | ❌ | ❌ | ✅ |

## Direct vs Derived Measures

**Direct measures** — Obtained by direct observation or counting:
- Lines of code (LOC)
- Number of defects
- Execution time
- Number of test cases

**Derived measures** — Computed from direct measures:
- Defect density = defects / KLOC
- Test coverage = tested branches / total branches
- Productivity = LOC / person-month
- Mean time to failure (MTTF) = total time / number of failures

> **Derived measures can have different scales than their components.** Defect density (ratio) / LOC (ratio) = ratio. But be careful with ratios of ratios.

## Reliability and Validity

### Reliability (Consistency)

**Question:** Does the measurement method produce consistent results?

**Types:**
- **Test-retest reliability** — Same method, same subject, different times → correlate results
- **Inter-rater reliability** — Different measurers, same subject → correlate results
- **Internal consistency** — Do items in a multi-item measure correlate with each other?

**Variation index:** Standard deviation / mean — quantifies measurement reliability.

**Example:** If two developers count cyclomatic complexity of the same code and get 12 and 18, the method has low inter-rater reliability.

### Validity (Measuring What We Intend)

**Question:** Does the measurement actually measure what we think it measures?

**Types:**
- **Content validity** — Does the measure cover all aspects of the concept?
- **Construct validity** — Does the measure correlate with related concepts?
- **Predictive validity** — Does the measure predict future outcomes?
- **Concurrent validity** — Does the measure correlate with existing measures?

**Example:** LOC (lines of code) is a reliable measure but has poor validity for "program complexity" — a 1000-line program may be simpler than a 100-line one.

## Goal-Question-Metric (GQM) Paradigm

**Core principle:** Only measure to support decisions. Avoid "measurement for the merely curious."

### GQM Structure

```
Goal: What do we want to achieve?
  ↓
Question: What do we need to know to achieve the goal?
  ↓
Metric: What data do we need to answer the question?
```

### GQM Example

**Goal:** Improve the reliability of the payment service

| Question | Metric | Scale |
|---|---|---|
| How often does it fail? | Failure count per week | Ratio |
| How long until it recovers? | Mean time to recovery (MTTR) | Ratio |
| What causes failures? | Failure mode distribution | Nominal |
| Are we improving? | Failure trend over time | Interval (time series) |

### GQM Anti-Patterns

| Anti-Pattern | What It Looks Like | Fix |
|---|---|---|
| **Metric without goal** | "Let's track LOC" | What decision will LOC inform? |
| **Goal without question** | "We want quality" | What specific quality aspect? |
| **Too many metrics** | Tracking 50 metrics | Focus on 5-7 that drive decisions |
| **Vanity metrics** | "We have 10K tests" | What do test counts tell you about quality? |
| **Punishing metrics** | Using defect counts to penalize developers | Measure process, not people |

## Common Software Metrics

### Product Metrics

| Metric | What It Measures | Scale | Valid Use |
|---|---|---|---|
| **Cyclomatic complexity** | Decision complexity | Ratio | Identify complex methods for refactoring |
| **Coupling** | Inter-module dependency | Ratio | High coupling → maintenance risk |
| **Cohesion** | Intra-module relatedness | Ratio | Low cohesion → poor design |
| **LOC** | Size | Ratio | Effort estimation (with caveats) |
| **Defect density** | Quality | Ratio | Compare quality across modules |

### Process Metrics

| Metric | What It Measures | Scale | Valid Use |
|---|---|---|---|
| **Defect removal efficiency** | Testing effectiveness | Ratio | Compare test approaches |
| **Lead time** | Delivery speed | Ratio | Measure process efficiency |
| **Cycle time** | Work item duration | Ratio | Identify bottlenecks |
| **Change failure rate** | Release quality | Ratio | Measure deployment risk |

### Measurement in Practice

**Do:**
- Define operational definitions before measuring
- Use GQM to tie metrics to decisions
- Validate that your metrics measure what you think
- Track trends, not absolutes
- Use metrics to improve processes, not punish people

**Don't:**
- Measure without a purpose
- Average ordinal data
- Use a single metric as a proxy for complex concepts
- Compare metrics across different contexts without normalization
- Trust metrics without understanding their reliability

## Related

- [[Engineering Foundation Overview]] — All engineering foundation topics
- [[02 SWE Process/18_Evaluation_and_Improvement|18_Evaluation_and_Improvement]] — Using metrics for process improvement
- [[02 SWE Process/16_Testing_Strategies|16_Testing_Strategies]] — Test metrics and coverage
- [[01 Physics and Math/09_Math_Stats_and_Economics|09_Math_Stats_and_Economics]] — Statistical foundations
