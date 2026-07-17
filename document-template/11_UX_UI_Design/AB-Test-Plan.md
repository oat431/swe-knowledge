---
document_type: A/B Test Plan
version: "1.0"
status: Draft
author: "[UX Researcher]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [ab-testing, experimentation, ux-research]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
---

# A/B Test Plan

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> A/B testing compares two versions of a design to determine which performs better for a specific metric.

## 2. Test Framework

| Aspect | Approach |
|--------|---------|
| [Tool] | [Optimizely / Google Optimize / LaunchDarkly] |
| [Traffic Split] | [50/50 default] |
| [Duration] | [Minimum 2 weeks or 1000 conversions] |
| [Significance] | [95% confidence level (p < 0.05)] |
| [Primary Metric] | [Conversion rate / task completion rate] |

## 3. Test Register

| # | Test | Hypothesis | Metric | Status |
|---|------|-----------|--------|--------|
| AB-001 | [Submit Button Color] | [Green CTA converts higher than blue] | [Click rate] | ⬜ Planned |
| AB-002 | [Form Steps] | [3-step form has higher completion than single page] | [Completion rate] | ⬜ Planned |
| AB-003 | [Status Display] | [Timeline view is clearer than status badge] | [Support tickets] | ⬜ Planned |

## 4. Test Template

### AB-XXX: [Test Name]

| Field | Detail |
|-------|--------|
| **Test Name** | [Descriptive name] |
| **Hypothesis** | [If we change X, then Y will improve because Z] |
| **Control (A)** | [Current version] |
| **Variant (B)** | [New version] |
| **Primary Metric** | [What we're measuring] |
| **Secondary Metrics** | [Additional metrics to monitor] |
| **Sample Size** | [Calculated based on effect size] |
| **Duration** | [Minimum 2 weeks] |
| **Success Criteria** | [Specific threshold for winner] |

#### Variants

| Variant | Description | Screenshot |
|---------|-----------|-----------|
| [Control A] | [Current design] | [Link] |
| [Variant B] | [New design] | [Link] |

#### Results

| Metric | Control (A) | Variant (B) | Difference | Confidence | Winner |
|--------|-----------|-----------|-----------|-----------|--------|
| [Primary] | [X%] | [Y%] | [Z%] | [95%] | [A/B] |
| [Secondary 1] | [X%] | [Y%] | [Z%] | — | — |
| [Secondary 2] | [X%] | [Y%] | [Z%] | — | — |

#### Decision

| Field | Detail |
|-------|--------|
| [Winner] | [Control A / Variant B / No clear winner] |
| [Action] | [Implement winner / Run longer / Redesign] |
| [Learning] | [What we learned] |

## 5. Sample Size Calculator

| Parameter | Value |
|-----------|-------|
| [Baseline Conversion] | [X%] |
| [Minimum Detectable Effect] | [Y%] |
| [Statistical Significance] | [95%] |
| [Statistical Power] | [80%] |
| **Required Sample Size** | **[N per variant]** |

## 6. Testing Guidelines

| Rule | Rationale |
|------|----------|
| [Test one variable at a time] | [Isolate cause of change] |
| [Run for full business cycles] | [Account for day-of-week effects] |
| [Don't peek at results early] | [Avoid false significance] |
| [Document all tests] | [Build institutional knowledge] |
| [Share learnings with team] | [Everyone benefits] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Analytics-Dashboard-Spec]] | Metrics tracking |
| [[Usability-Test-Report]] | Qualitative complement |
| [[Survey-Questionnaire]] | User feedback |

---

> **Template Standard:** Based on ISO 9241-210
> **Usage:** A/B testing is *evidence-based design*. Don't guess — test. But test wisely: not everything needs an A/B test. Use it for high-impact decisions where data matters.
