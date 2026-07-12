---
document_type: User Personas
version: "1.0"
status: Draft
author: "[UX Researcher]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [personas, user-research, ux, iso-9241]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
  - Nielsen Norman Group — Persona Best Practices
---

# User Personas

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Personas are fictional characters representing key user segments. They help the team design for real people with real goals, not abstract "users."

## 2. Persona Summary

| Persona | Role | Age | Tech Savvy | Primary Goal | Frequency |
|---------|------|-----|-----------|-------------|-----------|
| 👤 Sarah | [Customer] | [32] | [Medium] | [Submit request quickly] | [Daily] |
| 👤 James | [Operations Staff] | [45] | [High] | [Process requests efficiently] | [Daily] |
| 👤 Linda | [Operations Manager] | [52] | [Medium] | [Monitor team performance] | [Weekly] |
| 👤 David | [IT Administrator] | [38] | [Very High] | [Maintain system health] | [Daily] |

## 3. Persona Cards

### 👤 Sarah — The Customer

```
┌─────────────────────────────────────────────────────────────┐
│  SARAH CHEN                                                  │
│  Age: 32 | Customer | Tech Savvy: Medium                    │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  📋 Bio                                                      │
│  Small business owner, submits 2-3 requests per week.        │
│  Uses smartphone for most tasks. Values speed and             │
│  simplicity over features.                                    │
│                                                              │
│  🎯 Goals                                                    │
│  • Submit requests in under 5 minutes                        │
│  • Track status without calling support                       │
│  • Upload documents from phone                                │
│                                                              │
│  😤 Frustrations                                             │
│  • Long forms with redundant fields                           │
│  • No mobile-friendly interface                               │
│  • Having to call to check status                             │
│                                                              │
│  💡 Needs                                                    │
│  • Simple, step-by-step form                                  │
│  • Mobile-responsive design                                   │
│  • Real-time status tracking                                  │
│  • Push notifications for updates                             │
│                                                              │
│  🔧 Tech Stack                                               │
│  • iPhone 14, Chrome mobile                                   │
│  • Uses: WhatsApp, banking apps, Google Drive                 │
│  • Comfort level: Can navigate apps, struggles with complex   │
│    forms                                                      │
│                                                              │
│  📊 Behavior Pattern                                         │
│  • Submits requests during lunch or evening                   │
│  • Checks status 2-3 times per request                        │
│  • Prefers email notifications over SMS                       │
│  • Abandons forms that take > 10 minutes                      │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Design Implications:**

| Need | Design Response |
|------|----------------|
| [Quick submission] | [Progressive disclosure, save draft] |
| [Mobile-first] | [Responsive design, touch-friendly] |
| [Status tracking] | [Real-time status page, notifications] |
| [Simple forms] | [Smart defaults, auto-fill, validation] |

---

### 👤 James — The Operations Staff

```
┌─────────────────────────────────────────────────────────────┐
│  JAMES OKONKWO                                                │
│  Age: 45 | Operations Staff | Tech Savvy: High               │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  📋 Bio                                                      │
│  10 years experience in operations. Processes 15-20           │
│  requests per day. Values efficiency and accuracy.            │
│                                                              │
│  🎯 Goals                                                    │
│  • Process requests quickly and accurately                    │
│  • Minimize back-and-forth with customers                     │
│  • Meet SLA targets                                           │
│                                                              │
│  😤 Frustrations                                             │
│  • Switching between multiple screens                         │
│  • Manual data entry that could be auto-filled                │
│  • Unclear request priorities                                 │
│                                                              │
│  💡 Needs                                                    │
│  • Dashboard with queue and priorities                        │
│  • Side-by-side view: request + documents                     │
│  • Quick actions (approve/reject/escalate)                    │
│  • Keyboard shortcuts for power users                         │
│                                                              │
│  🔧 Tech Stack                                               │
│  • Desktop, dual monitor, Chrome                              │
│  • Uses: CRM, ERP, email all day                              │
│  • Comfort level: Very comfortable, expects efficiency        │
│                                                              │
│  📊 Behavior Pattern                                         │
│  • Works 8am-5pm, peak processing 9-11am                      │
│  • Processes high-value requests first                        │
│  • Uses search/filters extensively                            │
│  • Frustrated by unnecessary clicks                           │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Design Implications:**

| Need | Design Response |
|------|----------------|
| [Efficient queue] | [Priority-sorted dashboard, bulk actions] |
| [Side-by-side view] | [Split panel: request + documents] |
| [Quick actions] | [One-click approve/reject, keyboard shortcuts] |
| [Clear priorities] | [Color-coded priority, SLA indicators] |

---

### 👤 Linda — The Operations Manager

```
┌─────────────────────────────────────────────────────────────┐
│  LINDA MARTINEZ                                               │
│  Age: 52 | Operations Manager | Tech Savvy: Medium           │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  📋 Bio                                                      │
│  Manages team of 8 operations staff. Focuses on               │
│  performance, compliance, and reporting.                      │
│                                                              │
│  🎯 Goals                                                    │
│  • Monitor team performance and SLA compliance                │
│  • Identify bottlenecks and trends                            │
│  • Generate reports for leadership                            │
│                                                              │
│  😤 Frustrations                                             │
│  • Manual report generation                                   │
│  • Lack of real-time visibility                               │
│  • Difficulty identifying process bottlenecks                 │
│                                                              │
│  💡 Needs                                                    │
│  • Real-time dashboard with KPIs                              │
│  • Automated report generation                                │
│  • Team performance metrics                                   │
│  • Escalation alerts                                          │
│                                                              │
│  🔧 Tech Stack                                               │
│  • Laptop, Chrome, occasional tablet                          │
│  • Uses: Excel, PowerPoint, email                             │
│  • Comfort level: Moderate, prefers visual dashboards         │
│                                                              │
│  📊 Behavior Pattern                                         │
│  • Checks dashboard first thing in morning                    │
│  • Reviews escalations before team standup                    │
│  • Generates weekly reports every Friday                      │
│  • Uses mobile for alerts, desktop for reports                │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Design Implications:**

| Need | Design Response |
|------|----------------|
| [Real-time KPIs] | [Dashboard with live metrics] |
| [Automated reports] | [Scheduled report generation, export] |
| [Team metrics] | [Per-staff performance view] |
| [Escalation alerts] | [Push notifications for escalations] |

---

### 👤 David — The IT Administrator

```
┌─────────────────────────────────────────────────────────────┐
│  DAVID PARK                                                   │
│  Age: 38 | IT Administrator | Tech Savvy: Very High          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  📋 Bio                                                      │
│  Manages system health, integrations, and security.           │
│  On-call for production issues.                               │
│                                                              │
│  🎯 Goals                                                    │
│  • Ensure system uptime and performance                       │
│  • Manage user access and security                            │
│  • Monitor integrations and data quality                      │
│                                                              │
│  😤 Frustrations                                             │
│  • Lack of monitoring and alerting                            │
│  • Manual user management                                     │
│  • No audit trail for changes                                 │
│                                                              │
│  💡 Needs                                                    │
│  • System health dashboard                                    │
│  • User management interface                                  │
│  • Audit log viewer                                           │
│  • Integration monitoring                                     │
│                                                              │
│  🔧 Tech Stack                                               │
│  • Desktop, multiple monitors, terminal                       │
│  • Uses: Grafana, AWS Console, Slack                          │
│  • Comfort level: Expert, expects power tools                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## 4. Persona Priority Matrix

| Persona | Business Value | Frequency | Design Priority |
|---------|---------------|-----------|----------------|
| 👤 Sarah (Customer) | 🔴 High | Daily | 🔴 Primary |
| 👤 James (Staff) | 🔴 High | Daily | 🔴 Primary |
| 👤 Linda (Manager) | 🟡 Medium | Weekly | 🟡 Secondary |
| 👤 David (IT Admin) | 🟡 Medium | Daily | 🟡 Secondary |

## 5. Persona Usage Guidelines

| When To Use | How To Use |
|------------|-----------|
| [Design decisions] | [Ask "Would Sarah understand this?"] |
| [Feature prioritization] | [Map features to persona goals] |
| [Usability testing] | [Recruit participants matching personas] |
| [Stakeholder presentations] | [Humanize user needs] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[User Research Report]] | Research data behind personas |
| [[Journey Map]] | Persona journeys |
| [[User Flows]] | Persona task flows |

---

> **Template Standard:** Based on ISO 9241-210, Nielsen Norman Group
> **Usage:** Reference personas in every design discussion. "Sarah would struggle with this" is more persuasive than "users might find this confusing."
