---
document_type: User Flows
version: "1.0"
status: Draft
author: "[UX Designer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [user-flows, task-flows, ux]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
---

# User Flows

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> User flows diagram the step-by-step paths users take to complete key tasks — showing decisions, branches, and error handling.

## 2. Flow Index

| # | Flow | Persona | Steps | Status |
|---|------|---------|-------|--------|
| UF-001 | [Submit Request] | [Sarah] | [6] | ✅ Approved |
| UF-002 | [Process Request] | [James] | [5] | ✅ Approved |
| UF-003 | [Track Status] | [Sarah] | [3] | ✅ Approved |
| UF-004 | [Login] | [All] | [4] | ✅ Approved |
| UF-005 | [Generate Report] | [Linda] | [4] | ✅ Approved |

## 3. User Flows

### UF-001: Submit Request (Sarah)

```mermaid
flowchart TD
    START([Start]) --> AUTH{Logged<br>in?}
    AUTH -->|No| LOGIN[Login Page]
    LOGIN --> AUTH
    AUTH -->|Yes| DASH[Dashboard]
    DASH --> NEW[Click New Request]
    NEW --> STEP1[Step 1: Personal Info]
    STEP1 --> FILL1[Fill/Verify Fields]
    FILL1 --> VALID1{Valid?}
    VALID1 -->|No| ERR1[Show Errors]
    ERR1 --> FILL1
    VALID1 -->|Yes| STEP2[Step 2: Request Details]
    STEP2 --> FILL2[Fill Type, Amount, Description]
    FILL2 --> VALID2{Valid?}
    VALID2 -->|No| ERR2[Show Errors]
    ERR2 --> FILL2
    VALID2 -->|Yes| STEP3[Step 3: Documents]
    STEP3 --> UPLOAD[Upload Documents]
    UPLOAD --> VALID3{Valid<br>Files?}
    VALID3 -->|No| ERR3[Show File Errors]
    ERR3 --> UPLOAD
    VALID3 -->|Yes| REVIEW[Review Page]
    REVIEW --> CONFIRM{Confirm?}
    CONFIRM -->|Edit| STEP1
    CONFIRM -->|Submit| SUBMIT[Submit Request]
    SUBMIT --> SUCCESS[Success Page<br>+ Reference #]
    SUCCESS --> END_NODE([End])

    style START fill:#4CAF50,color:#fff
    style END_NODE fill:#4CAF50,color:#fff
    style ERR1 fill:#f44336,color:#fff
    style ERR2 fill:#f44336,color:#fff
    style ERR3 fill:#f44336,color:#fff
    style SUCCESS fill:#4CAF50,color:#fff
```

### UF-002: Process Request (James)

```mermaid
flowchart TD
    START([Dashboard]) --> QUEUE[Open Work Queue]
    QUEUE --> SELECT[Select Request]
    SELECT --> REVIEW[Review Details]
    REVIEW --> DOCS[View Documents]
    DOCS --> DECIDE{Decision?}
    DECIDE -->|Need Info| REQUEST[Request Info from Customer]
    DECIDE -->|Escalate| ESCALATE[Escalate to Manager]
    DECIDE -->|Approve| APPROVE[Approve Request]
    DECIDE -->|Reject| REJECT[Reject Request]

    REQUEST --> COMMENT1[Add Comment]
    COMMENT1 --> NOTIFY1[Notify Customer]
    NOTIFY1 --> END_NODE([Done])

    ESCALATE --> COMMENT2[Add Reason]
    COMMENT2 --> NOTIFY2[Notify Manager]
    NOTIFY2 --> END_NODE

    APPROVE --> CONFIRM[Confirm Approval]
    CONFIRM --> NOTIFY3[Notify Customer]
    NOTIFY3 --> END_NODE

    REJECT --> REASON[Enter Rejection Reason]
    REASON --> CONFIRM_R[Confirm Rejection]
    CONFIRM_R --> NOTIFY4[Notify Customer]
    NOTIFY4 --> END_NODE

    style START fill:#2196F3,color:#fff
    style END_NODE fill:#2196F3,color:#fff
    style APPROVE fill:#4CAF50,color:#fff
    style REJECT fill:#f44336,color:#fff
    style ESCALATE fill:#FF9800,color:#fff
```

### UF-003: Track Status (Sarah)

```mermaid
flowchart TD
    START([My Requests]) --> LIST[View Request List]
    LIST --> SELECT[Select Request]
    SELECT --> STATUS[View Status Page]
    STATUS --> TIMELINE[View Status Timeline]
    TIMELINE --> ACTION{Action?}
    ACTION -->|View Details| DETAIL[View Full Details]
    ACTION -->|Download Docs| DOWNLOAD[Download Documents]
    ACTION -->|Contact Support| SUPPORT[Open Support Ticket]
    ACTION -->|Back to List| LIST

    DETAIL --> END_NODE([Done])
    DOWNLOAD --> END_NODE
    SUPPORT --> END_NODE

    style START fill:#4CAF50,color:#fff
    style END_NODE fill:#4CAF50,color:#fff
```

### UF-004: Login (All Users)

```mermaid
flowchart TD
    START([Login Page]) --> ENTER[Enter Email + Password]
    ENTER --> VALIDATE{Valid?}
    VALIDATE -->|No| ERR[Show Error]
    ERR --> ENTER
    VALIDATE -->|Yes| CHECK_2FA{2FA<br>Enabled?}
    CHECK_2FA -->|Yes| MFA[Enter MFA Code]
    MFA --> MFA_VALID{Valid?}
    MFA_VALID -->|No| MFA_ERR[Show Error]
    MFA_ERR --> MFA
    MFA_VALID -->|Yes| REDIRECT[Redirect to Dashboard]
    CHECK_2FA -->|No| REDIRECT
    REDIRECT --> END_NODE([Logged In])

    START -->|Forgot Password| RESET[Reset Password Flow]
    RESET --> END_NODE

    style START fill:#607D8B,color:#fff
    style END_NODE fill:#4CAF50,color:#fff
    style ERR fill:#f44336,color:#fff
```

## 4. Flow Statistics

| Flow | Steps | Decisions | Error Paths | Avg Time |
|------|-------|----------|------------|---------|
| [Submit Request] | [6] | [5] | [3] | [5-10 min] |
| [Process Request] | [5] | [1] | [0] | [3-5 min] |
| [Track Status] | [3] | [1] | [0] | [1-2 min] |
| [Login] | [4] | [3] | [2] | [30 sec] |
| [Generate Report] | [4] | [2] | [1] | [2-3 min] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Information-Architecture]] | Pages these flows traverse |
| [[Wireframes-Low-fi]] | Screen layouts for each step |
| [[Journey-Map]] | Emotional journey behind the flow |

---

> **Template Standard:** Based on ISO 9241-210
> **Usage:** User flows answer "How does the user get from A to B?" Every flow needs a wireframe for each step. Test flows with prototypes before development.
