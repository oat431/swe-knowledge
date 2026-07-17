---
document_type: Sitemap
version: "1.0"
status: Draft
author: "[UX Designer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [sitemap, navigation, ia, ux]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
---

# Sitemap

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> The sitemap provides a visual overview of all pages and their hierarchical relationships.

## 2. Customer Portal Sitemap

```mermaid
flowchart TD
    HOME[🏠 Home] --> NEW[📝 New Request]
    HOME --> MY[📋 My Requests]
    HOME --> PROF[👤 My Profile]
    HOME --> HELP[❓ Help]

    NEW --> FORM[Request Form<br>/requests/new]

    MY --> LIST[Request List<br>/requests]
    LIST --> DETAIL[Request Detail<br>/requests/:id]
    DETAIL --> STATUS[Status Tracker]
    DETAIL --> DOCS[Documents]
    DETAIL --> HIST[History]

    PROF --> P_EDIT[Edit Profile<br>/profile]
    PROF --> P_NOTIF[Notification Settings<br>/profile/notifications]

    HELP --> H_FAQ[FAQ<br>/help/faq]
    HELP --> H_CONTACT[Contact Support<br>/help/contact]

    style HOME fill:#4CAF50,color:#fff
    style NEW fill:#2196F3,color:#fff
    style MY fill:#2196F3,color:#fff
    style PROF fill:#FF9800,color:#fff
    style HELP fill:#9C27B0,color:#fff
```

## 3. Admin Portal Sitemap

```mermaid
flowchart TD
    ADMIN[🏠 Dashboard] --> QUEUE[📋 Work Queue]
    ADMIN --> SEARCH[🔍 Search]
    ADMIN --> REPORTS[📊 Reports]
    ADMIN --> SETTINGS[⚙️ Settings]

    QUEUE --> Q_LIST[Request List<br>/admin/queue]
    Q_LIST --> Q_DETAIL[Request Detail<br>/admin/requests/:id]
    Q_DETAIL --> Q_REVIEW[Review Panel]
    Q_DETAIL --> Q_DOCS[Documents]
    Q_DETAIL --> Q_HISTORY[History]
    Q_DETAIL --> Q_ACTIONS[Actions<br>Approve/Reject/Escalate]

    SEARCH --> S_RESULTS[Search Results<br>/admin/search]

    REPORTS --> R_STD[Standard Reports<br>/admin/reports/standard]
    REPORTS --> R_CUSTOM[Custom Reports<br>/admin/reports/custom]
    REPORTS --> R_EXPORT[Export]

    SETTINGS --> S_USERS[Users<br>/admin/settings/users]
    SETTINGS --> S_ROLES[Roles<br>/admin/settings/roles]
    SETTINGS --> S_RULES[Rules<br>/admin/settings/rules]
    SETTINGS --> S_CONFIG[System Config<br>/admin/settings/config]

    style ADMIN fill:#2196F3,color:#fff
    style QUEUE fill:#4CAF50,color:#fff
    style SEARCH fill:#FF9800,color:#fff
    style REPORTS fill:#9C27B0,color:#fff
    style SETTINGS fill:#607D8B,color:#fff
```

## 4. Page Count Summary

| Section | Pages | Depth |
|---------|-------|-------|
| [Customer Portal] | [8] | [3 levels] |
| [Admin Portal] | [14] | [4 levels] |
| [Shared] | [4] | [2 levels] |
| **Total** | **[26]** | **[4 levels max]** |

## 5. Page Inventory

| # | Page | URL | Template | Auth | Priority |
|---|------|-----|---------|------|---------|
| 1 | [Home] | [/] | [Landing] | [Public] | 🔴 |
| 2 | [New Request] | [/requests/new] | [Multi-step form] | [Customer] | 🔴 |
| 3 | [My Requests] | [/requests] | [List] | [Customer] | 🔴 |
| 4 | [Request Detail] | [/requests/:id] | [Detail] | [Customer] | 🔴 |
| 5 | [My Profile] | [/profile] | [Form] | [Customer] | 🟡 |
| 6 | [Help / FAQ] | [/help] | [Content] | [Public] | 🟡 |
| 7 | [Contact Support] | [/help/contact] | [Form] | [Public] | 🟡 |
| 8 | [Admin Dashboard] | [/admin] | [Dashboard] | [Admin] | 🔴 |
| 9 | [Work Queue] | [/admin/queue] | [List] | [Staff] | 🔴 |
| 10 | [Admin Request Detail] | [/admin/requests/:id] | [Detail + Actions] | [Staff] | 🔴 |
| 11 | [Search] | [/admin/search] | [Search results] | [Staff] | 🔴 |
| 12 | [Standard Reports] | [/admin/reports/standard] | [Report] | [Manager] | 🟡 |
| 13 | [Custom Reports] | [/admin/reports/custom] | [Report builder] | [Manager] | 🟢 |
| 14 | [User Management] | [/admin/settings/users] | [CRUD list] | [Admin] | 🔴 |
| 15 | [Role Management] | [/admin/settings/roles] | [CRUD list] | [Admin] | 🟡 |
| 16 | [Rule Management] | [/admin/settings/rules] | [CRUD list] | [Admin] | 🟡 |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Information-Architecture]] | IA structure this sitemap visualizes |
| [[User-Flows]] | Flows through these pages |
| [[Wireframes-Low-fi]] | Page layouts |

---

> **Template Standard:** Based on ISO 9241-210
> **Usage:** The sitemap is a *communication tool* — use it to align the team on scope and structure. Every page in the sitemap needs a wireframe.
