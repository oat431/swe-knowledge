---
document_type: Risk Management Plan
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
risk_owner: "[Risk Manager / PM]"
classification: "Internal / Confidential"
tags: [risk-management, risk-process, pmbok, iso-31000]
standard_ref:
  - PMBOK v8 — Planning (Risk Management)
  - ISO 31000 — Risk Management
---

# Risk Management Plan

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Baselined]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Project Manager | [Name / Role] |

### Approvals

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Project Sponsor | | | |
| Project Manager | | | |

---

## 1. Purpose

> This plan defines how risks will be identified, analyzed, responded to, and monitored throughout the project.

## 2. Risk Management Approach

| Aspect | Approach |
|--------|---------|
| **Methodology** | [ISO 31000 + PMBOK risk management] |
| **Risk Appetite** | [Medium — willing to accept moderate risks for higher returns] |
| **Risk Threshold** | [🔴 Critical risks require immediate action; 🟠 High risks require mitigation plan] |
| **Risk Categories** | [Technical, Schedule, Cost, Quality, Resource, External] |
| **Risk Tool** | [Risk register in Confluence + Jira for risk-related tasks] |

## 3. Risk Identification

### 3.1 Identification Methods

| Method | When | Participants | Output |
|--------|------|-------------|--------|
| [Brainstorming] | [Project kickoff, phase gates] | [Full team] | [Initial risk list] |
| [Expert judgment] | [Ongoing] | [Subject matter experts] | [Risk insights] |
| [Checklist analysis] | [Each phase] | [PM, BA] | [Known risk categories] |
| [SWOT analysis] | [Planning phase] | [PM, BA, TL] | [Strengths, weaknesses, opportunities, threats] |
| [Lessons learned review] | [Project start] | [PM] | [Risks from similar projects] |
| [Assumption analysis] | [Each phase gate] | [PM, BA] | [Invalidated assumptions → risks] |

### 3.2 Risk Categories (RBS)

| Category | Subcategories |
|----------|-------------|
| **Technical** | [Architecture, integration, performance, security, data migration] |
| **Schedule** | [Estimation, dependencies, resource availability, scope creep] |
| **Cost** | [Budget overrun, vendor escalation, currency, contingency] |
| **Quality** | [Defects, requirements quality, testing coverage] |
| **Resource** | [Team skills, availability, turnover, knowledge gaps] |
| **External** | [Vendor, regulatory, market, competitive, economic] |

## 4. Risk Analysis

### 4.1 Probability Scale

| Level | Rating | Description | Likelihood |
|-------|--------|-------------|-----------|
| 5 | Almost Certain | Expected to occur | >80% |
| 4 | Likely | Probable occurrence | 60-80% |
| 3 | Possible | Could occur | 40-60% |
| 2 | Unlikely | Improbable | 20-40% |
| 1 | Rare | Highly improbable | <20% |

### 4.2 Impact Scale

| Level | Rating | Cost Impact | Schedule Impact | Quality Impact |
|-------|--------|------------|----------------|---------------|
| 5 | Critical | >$100K | >2 months | Project failure |
| 4 | Major | $50-100K | 1-2 months | Significant rework |
| 3 | Moderate | $10-50K | 2-4 weeks | Partial rework |
| 2 | Minor | $1-10K | 1-2 weeks | Minor adjustments |
| 1 | Insignificant | <$1K | <1 week | Negligible |

### 4.3 Risk Score

| Score | Level | Response |
|-------|-------|---------|
| 20-25 | 🔴 Critical | [Immediate action, escalate to sponsor] |
| 12-19 | 🟠 High | [Mitigation plan required, track weekly] |
| 6-11 | 🟡 Medium | [Monitor and manage, track bi-weekly] |
| 1-5 | 🟢 Low | [Accept and monitor, track monthly] |

## 5. Risk Response Strategies

### 5.1 Response Types

| Strategy | Description | When to Use |
|----------|------------|------------|
| **Avoid** | [Eliminate the threat by changing plan] | [When risk is too high and avoidable] |
| **Mitigate** | [Reduce probability or impact] | [Most risks — proactive reduction] |
| **Transfer** | [Shift impact to third party] | [Insurance, contracts, outsourcing] |
| **Accept** | [Acknowledge and monitor] | [Low risks, unavoidable risks] |
| **Escalate** | [Pass to higher authority] | [Risks beyond PM authority] |

### 5.2 Response Planning

| Risk Level | Response Required | Owner | Timeline |
|-----------|------------------|-------|----------|
| 🔴 Critical | [Avoid or mitigate — mandatory] | [PM + Sponsor] | [Within 24 hours] |
| 🟠 High | [Mitigate — plan required] | [Risk Owner] | [Within 1 week] |
| 🟡 Medium | [Mitigate or accept] | [Risk Owner] | [Within 2 weeks] |
| 🟢 Low | [Accept] | [PM] | [Monitor only] |

## 6. Risk Monitoring

### 6.1 Monitoring Activities

| Activity | Frequency | Participants | Output |
|----------|-----------|-------------|--------|
| [Risk Review Meeting] | Bi-weekly | [PM, BA, TL] | [Updated risk register] |
| [Risk Dashboard Update] | Weekly | [PM] | [Status report] |
| [Trigger Monitoring] | Continuous | [Risk Owners] | [Early warning alerts] |
| [Risk Retrospective] | Monthly | [Full team] | [Lessons learned, new risks] |
| [Risk Audit] | Per phase gate | [PM, Sponsor] | [Risk process effectiveness] |

### 6.2 Risk Metrics

| Metric | Target | Measurement | Frequency |
|--------|--------|-------------|-----------|
| [Open critical risks] | [0] | [Risk register] | [Weekly] |
| [Open high risks] | [<3] | [Risk register] | [Weekly] |
| [Risks with mitigation plan] | [100% of 🟠🔴] | [Risk register] | [Weekly] |
| [Overdue risk actions] | [0] | [Risk register] | [Weekly] |
| [Risk trend] | [Decreasing] | [Risk count over time] | [Monthly] |

### 6.3 Escalation Matrix

| Risk Level | Escalation To | Timeline | Communication |
|-----------|--------------|----------|---------------|
| 🔴 Critical | [Steering Committee] | [Within 24 hours] | [Emergency meeting] |
| 🟠 High | [Sponsor] | [Within 48 hours] | [Email + meeting] |
| 🟡 Medium | [PM] | [Weekly review] | [Risk register update] |
| 🟢 Low | [Risk Owner] | [Monthly review] | [Dashboard update] |

## 7. Risk Budget

| Reserve | Amount | Purpose | Authority |
|---------|--------|---------|----------|
| [Contingency Reserve] | $[X] (15% of budget) | [Known risks — PM controlled] | [PM] |
| [Management Reserve] | $[X] (5% of budget) | [Unknown risks — sponsor controlled] | [Sponsor] |

## 8. Roles & Responsibilities

| Role | Risk Responsibility |
|------|-------------------|
| [Project Manager] | [Overall risk management, risk register maintenance, reporting] |
| [Risk Owners] | [Monitor assigned risks, implement mitigation, report status] |
| [Project Sponsor] | [Approve risk responses, access management reserve, escalation point] |
| [Team Members] | [Identify risks, participate in risk reviews, report new risks] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Risk-Register]] | Detailed risk tracking |
| [[Risk-Report]] | Risk status reporting |
| [[Risk-Analysis-Results]] | Initial risk analysis |
| [[Assumption-Log]] | Invalidated assumptions become risks |
| [[Project-Management-Plan]] | Parent plan |

---

> **Template Standard:** Based on PMBOK v8, ISO 31000
> **Usage:** Risk management is *proactive*, not reactive. Review risks bi-weekly. Every 🟠/🔴 risk must have a mitigation plan and an owner. Track risk metrics weekly and report to the steering committee monthly.
