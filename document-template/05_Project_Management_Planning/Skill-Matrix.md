---
document_type: Skill Matrix
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
classification: "Internal / Confidential"
tags: [skill-matrix, competency, training, pmbok]
standard_ref:
  - PMBOK v8 — Planning (Resource Management)
---

# Skill Matrix

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This matrix maps team member skills against project requirements, identifying strengths, gaps, and training needs.

## 2. Skill Proficiency Levels

| Level | Rating | Description |
|-------|--------|-------------|
| 4 | **Expert** | [Can teach others, deep knowledge, leads decisions] |
| 3 | **Proficient** | [Works independently, handles complex tasks] |
| 2 | **Competent** | [Works with guidance, handles standard tasks] |
| 1 | **Beginner** | [Learning, needs support, handles simple tasks] |
| 0 | **None** | [No experience with this skill] |

## 3. Skill Matrix

### 3.1 Technical Skills

| Skill | PM | BA | TL | Dev 1 | Dev 2 | Dev 3 | QA Lead | QA Eng | Required |
|-------|-----|-----|-----|-------|-------|-------|---------|--------|----------|
| [React / Next.js] | 0 | 0 | 3 | 4 | 3 | 2 | 1 | 0 | 3 |
| [Node.js] | 0 | 0 | 4 | 3 | 3 | 2 | 1 | 0 | 3 |
| [PostgreSQL] | 0 | 1 | 3 | 3 | 2 | 2 | 2 | 1 | 2 |
| [REST API Design] | 0 | 1 | 4 | 3 | 3 | 2 | 2 | 1 | 3 |
| [Cloud (AWS/Azure)] | 1 | 0 | 3 | 2 | 2 | 1 | 1 | 0 | 2 |
| [CI/CD] | 0 | 0 | 3 | 2 | 2 | 1 | 2 | 1 | 2 |
| [Docker / K8s] | 0 | 0 | 3 | 2 | 2 | 1 | 1 | 0 | 2 |
| [Security] | 1 | 1 | 2 | 1 | 1 | 0 | 2 | 1 | 2 |
| [Git] | 1 | 1 | 4 | 4 | 3 | 3 | 3 | 2 | 3 |
| [Testing Automation] | 0 | 0 | 2 | 2 | 1 | 0 | 4 | 3 | 3 |

### 3.2 Domain & Soft Skills

| Skill | PM | BA | TL | Dev 1 | Dev 2 | Dev 3 | QA Lead | QA Eng | Required |
|-------|-----|-----|-----|-------|-------|-------|---------|--------|----------|
| [Business Domain Knowledge] | 2 | 4 | 2 | 1 | 1 | 0 | 2 | 1 | 3 |
| [Requirements Analysis] | 2 | 4 | 3 | 1 | 1 | 0 | 2 | 1 | 3 |
| [Stakeholder Management] | 4 | 3 | 1 | 0 | 0 | 0 | 0 | 0 | 3 |
| [Project Management] | 4 | 2 | 1 | 0 | 0 | 0 | 0 | 0 | 4 |
| [Communication] | 3 | 4 | 3 | 2 | 2 | 2 | 3 | 2 | 3 |
| [Problem Solving] | 3 | 3 | 4 | 3 | 3 | 2 | 3 | 2 | 3 |
| [Agile / Scrum] | 3 | 3 | 3 | 3 | 2 | 2 | 3 | 2 | 3 |

### 3.3 Skill Gap Analysis

| Skill | Required Level | Available Level | Gap | Risk | Mitigation |
|-------|---------------|----------------|-----|------|-----------|
| [Cloud (AWS/Azure)] | 2 | 2 (TL only) | [Team-wide gap] | 🟡 Medium | [Cloud training for DevOps + Devs] |
| [Security] | 2 | 2 (TL only) | [Team-wide gap] | 🟡 Medium | [Security training + consultant] |
| [Business Domain] | 3 | 4 (BA only) | [Knowledge concentrated] | 🟡 Medium | [Domain workshops for dev team] |
| [Testing Automation] | 3 | 4 (QA Lead only) | [Single point of knowledge] | 🟡 Medium | [Cross-train QA Engineer] |

## 4. Training Plan

| # | Skill Gap | Training | Audience | Duration | Timing | Cost | Expected Outcome |
|---|----------|---------|----------|----------|--------|------|-----------------|
| 1 | [Cloud] | [AWS/Azure Fundamentals] | [DevOps, Devs] | [2 days] | [Before Sprint 1] | $[X] | [Level 1 → Level 2] |
| 2 | [Security] | [OWASP Top 10 Workshop] | [Dev team] | [1 day] | [Before Sprint 3] | $[X] | [Level 0-1 → Level 2] |
| 3 | [Business Domain] | [Domain Knowledge Sessions] | [Dev team] | [4 × 1hr] | [Sprint 1-2] | $0 | [Level 0-1 → Level 2] |
| 4 | [Test Automation] | [Automation Framework Training] | [QA Engineer] | [3 days] | [Before Testing] | $[X] | [Level 3 → Level 4] |
| 5 | [CRM Platform] | [Vendor Training] | [Dev team, BA] | [3 days] | [Before Sprint 1] | $[X] | [Level 0 → Level 2] |

## 5. Skill Coverage Assessment

| Coverage Level | Count | Percentage | Status |
|---------------|-------|-----------|--------|
| [All skills covered at ≥required level] | [X skills] | [Y%] | 🟢 Good |
| [Covered but single point of knowledge] | [X skills] | [Y%] | 🟡 At Risk |
| [Gap — below required level] | [X skills] | [Y%] | 🔴 Needs Action |
| **Total Skills Assessed** | **[X]** | **100%** | |

## 6. Cross-Training Matrix

| Skill | Primary Expert | Backup | Cross-Training Needed |
|-------|---------------|--------|---------------------|
| [React] | Dev 1 | Dev 2 | ✅ Covered |
| [Node.js] | TL | Dev 1 | ✅ Covered |
| [PostgreSQL] | TL | Dev 1 | ⚠️ Train Dev 2 |
| [Cloud] | TL | DevOps | ⚠️ Train Devs |
| [Business Domain] | BA | PM | ⚠️ Train Devs |
| [Test Automation] | QA Lead | QA Engineer | ⚠️ In progress |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Resource-Management-Plan]] | Resource management context |
| [[Resource-Requirements]] | Required skills per activity |
| [[Skill-Matrix]] | Training delivery details |

---

> **Template Standard:** Based on PMBOK v8
> **Usage:** Review the skill matrix at project start and update when skills change. Address gaps through training, mentoring, or external support. A team with 2+ people per critical skill has no single points of failure.
