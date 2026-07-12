---
document_type: CRC Cards
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
tech_lead: "[Technical Lead]"
classification: "Internal / Confidential"
tags: [crc-cards, class-responsibility-collaborator, swebok]
standard_ref:
  - SWEBOK v4 — Design
---

# CRC Cards (Class-Responsibility-Collaborator)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> CRC cards are a lightweight design tool — each card represents a class with its responsibilities and collaborators. They're used for brainstorming and validating object-oriented design.

## 2. CRC Card Format

```
+----------------------------------+
| Class Name                       |
+----------------------------------+
| Responsibilities:                |
|   • Responsibility 1             |
|   • Responsibility 2             |
|   • Responsibility 3             |
+----------------------------------+
| Collaborators:                   |
|   • Collaborator 1               |
|   • Collaborator 2               |
+----------------------------------+
```

## 3. CRC Cards

### CRC-001: Request

```
+------------------------------------------+
| Request                                   |
+------------------------------------------+
| Responsibilities:                         |
|   • Store request data                    |
|   • Validate business rules               |
|   • Track status transitions              |
|   • Calculate processing metrics          |
|   • Manage documents                      |
+------------------------------------------+
| Collaborators:                            |
|   • Customer (owner)                      |
|   • Document (contains)                   |
|   • ProcessingRule (evaluated by)         |
|   • AuditLog (tracked by)                 |
|   • Notification (triggers)               |
+------------------------------------------+
```

### CRC-002: RequestService

```
+------------------------------------------+
| RequestService                            |
+------------------------------------------+
| Responsibilities:                         |
|   • Orchestrate request operations        |
|   • Apply business rules                  |
|   • Publish domain events                 |
|   • Coordinate with repositories          |
|   • Handle transactions                   |
+------------------------------------------+
| Collaborators:                            |
|   • RequestRepository (data access)       |
|   • EventPublisher (event publishing)     |
|   • RuleEngine (rule evaluation)          |
|   • AuditService (logging)                |
+------------------------------------------+
```

### CRC-003: ProcessingService

```
+------------------------------------------+
| ProcessingService                         |
+------------------------------------------+
| Responsibilities:                         |
|   • Validate requests against rules       |
|   • Classify and route requests           |
|   • Determine auto-approval eligibility   |
|   • Process manual review decisions       |
|   • Handle escalations                    |
+------------------------------------------+
| Collaborators:                            |
|   • RuleEngine (rule evaluation)          |
|   • RequestRepository (data access)       |
|   • NotificationService (notifications)   |
|   • AuditService (logging)                |
+------------------------------------------+
```

### CRC-004: RuleEngine

```
+------------------------------------------+
| RuleEngine                                |
+------------------------------------------+
| Responsibilities:                         |
|   • Evaluate business rules               |
|   • Determine auto-approval eligibility   |
|   • Calculate risk scores                 |
|   • Check for duplicates                  |
+------------------------------------------+
| Collaborators:                            |
|   • ProcessingRule (individual rules)     |
|   • Request (input)                       |
|   • CustomerData (customer info)          |
+------------------------------------------+
```

### CRC-005: AuthService

```
+------------------------------------------+
| AuthService                               |
+------------------------------------------+
| Responsibilities:                         |
|   • Authenticate users                    |
|   • Generate and validate tokens          |
|   • Manage user sessions                  |
|   • Enforce authorization                 |
+------------------------------------------+
| Collaborators:                            |
|   • UserRepository (user data)            |
|   • TokenService (JWT generation)         |
|   • CacheService (session storage)        |
+------------------------------------------+
```

### CRC-006: NotificationService

```
+------------------------------------------+
| NotificationService                       |
+------------------------------------------+
| Responsibilities:                         |
|   • Process notification events           |
|   • Render notification templates         |
|   • Send via appropriate channel          |
|   • Track delivery status                 |
|   • Handle retries                        |
+------------------------------------------+
| Collaborators:                            |
|   • TemplateEngine (content rendering)    |
|   • EmailProvider (email delivery)        |
|   • SMSProvider (SMS delivery)            |
|   • NotificationRepository (persistence)  |
+------------------------------------------+
```

### CRC-007: Customer

```
+------------------------------------------+
| Customer                                  |
+------------------------------------------+
| Responsibilities:                         |
|   • Store customer profile                |
|   • Validate customer data                |
|   • Track customer requests               |
|   • Determine VIP status                  |
+------------------------------------------+
| Collaborators:                            |
|   • Request (submits)                     |
|   • Email (contact info)                  |
|   • Address (location)                    |
+------------------------------------------+
```

### CRC-008: AuditService

```
+------------------------------------------+
| AuditService                              |
+------------------------------------------+
| Responsibilities:                         |
|   • Log all entity changes                |
|   • Record actor and timestamp            |
|   • Store before/after values             |
|   • Provide audit trail queries           |
+------------------------------------------+
| Collaborators:                            |
|   • AuditLogRepository (persistence)      |
|   • All services (event source)           |
+------------------------------------------+
```

## 4. CRC Design Session Notes

| Session | Date | Participants | Classes Discussed | Key Decisions |
|---------|------|-------------|------------------|--------------|
| [Session 1] | [YYYY-MM-DD] | [TL, Dev 1, Dev 2] | [Request, RequestService, ProcessingService] | [Separate validation from processing] |
| [Session 2] | [YYYY-MM-DD] | [TL, Dev 1, Dev 3] | [RuleEngine, AuthService, NotificationService] | [Strategy pattern for rules] |

## 5. Responsibility Distribution

| Class | # Responsibilities | Complexity | Status |
|-------|-------------------|-----------|--------|
| [Request] | [5] | [Medium] | ✅ Well-balanced |
| [RequestService] | [5] | [Medium] | ✅ Well-balanced |
| [ProcessingService] | [5] | [High] | ⚠️ Consider splitting |
| [RuleEngine] | [4] | [High] | ✅ Well-focused |
| [AuthService] | [4] | [Medium] | ✅ Well-balanced |
| [NotificationService] | [5] | [Medium] | ✅ Well-balanced |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Class Diagrams]] | Formal UML version of CRC |
| [[LLD (Low-Level Design)]] | Implementation details |
| [[Design Pattern Catalog]] | Patterns applied |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** CRC cards are *design thinking tools* — use them in design sessions to explore object responsibilities. They're quick to create, easy to modify, and great for team collaboration. Move to Class Diagrams when the design stabilizes.
