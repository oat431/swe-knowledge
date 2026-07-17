---
document_type: Class Diagrams
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
tech_lead: "[Technical Lead]"
classification: "Internal / Confidential"
tags: [class-diagrams, uml, static-structure, swebok, iso-19501]
standard_ref:
  - SWEBOK v4 — Design
  - ISO/IEC 19501 — UML
---

# Class Diagrams

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document contains UML class diagrams showing the static structure — classes, attributes, methods, and relationships.

## 2. Class Diagram: Domain Model

```mermaid
classDiagram
    class Customer {
        +UUID id
        +String name
        +String email
        +String phone
        +String address
        +Date createdAt
        +Date updatedAt
        +getProfile() Profile
        +updateProfile(data) void
    }

    class Request {
        +UUID id
        +RequestType type
        +Decimal amount
        +RequestStatus status
        +String description
        +Date createdAt
        +Date updatedAt
        +Date submittedAt
        +Date completedAt
        +submit() void
        +approve(approver: User) void
        +reject(approver: User, reason: String) void
        +escalate(escalator: User, reason: String) void
        +addDocument(doc: Document) void
        +getStatusHistory() StatusHistory[]
    }

    class Document {
        +UUID id
        +String filename
        +String contentType
        +Integer sizeBytes
        +String storageUrl
        +Date uploadedAt
        +validate() Boolean
        +delete() void
    }

    class User {
        +UUID id
        +String email
        +String name
        +Role role
        +Boolean isActive
        +Date lastLogin
        +authenticate(password) Boolean
        +hasPermission(permission) Boolean
    }

    class Role {
        +UUID id
        +String name
        +String description
        +String[] permissions
        +hasPermission(permission) Boolean
    }

    class AuditLog {
        +UUID id
        +String entityType
        +UUID entityId
        +String action
        +UUID actorId
        +JSON changes
        +Date createdAt
        +static log(entity, action, actor, changes) void
    }

    class ProcessingRule {
        +UUID id
        +String name
        +String condition
        +String action
        +Integer priority
        +Boolean isActive
        +evaluate(request: Request) RuleResult
    }

    class Notification {
        +UUID id
        +UUID recipientId
        +String channel
        +String template
        +JSON variables
        +NotificationStatus status
        +Date sentAt
        +send() void
        +retry() void
    }

    Customer "1" --> "*" Request : submits
    Request "1" --> "*" Document : contains
    Request "1" --> "*" AuditLog : tracks
    Request "1" --> "*" Notification : triggers
    User "1" --> "1" Role : has
    User "1" --> "*" Request : processes
    Request ..> ProcessingRule : evaluated by
```

## 3. Class Diagram: Service Layer

```mermaid
classDiagram
    class RequestController {
        -requestService: RequestService
        +create(dto: CreateRequestDTO) Request
        +getById(id: UUID) Request
        +list(filters: RequestFilters) Request[]
        +update(id: UUID, dto: UpdateRequestDTO) Request
        +getStatus(id: UUID) StatusResponse
    }

    class RequestService {
        -requestRepo: RequestRepository
        -eventPublisher: EventPublisher
        -ruleEngine: RuleEngine
        +create(dto: CreateRequestDTO) Request
        +getById(id: UUID) Request
        +list(filters: RequestFilters) Request[]
        +submit(id: UUID) void
        +approve(id: UUID, approver: User) void
        +reject(id: UUID, approver: User, reason: String) void
    }

    class RequestRepository {
        <<interface>>
        +save(request: Request) void
        +findById(id: UUID) Request
        +findByFilters(filters: RequestFilters) Request[]
        +update(request: Request) void
        +delete(id: UUID) void
    }

    class PostgresRequestRepository {
        -db: Database
        +save(request: Request) void
        +findById(id: UUID) Request
        +findByFilters(filters: RequestFilters) Request[]
        +update(request: Request) void
        +delete(id: UUID) void
    }

    class EventPublisher {
        <<interface>>
        +publish(event: DomainEvent) void
    }

    class RabbitMQEventPublisher {
        -connection: Connection
        +publish(event: DomainEvent) void
    }

    class RuleEngine {
        -rules: ProcessingRule[]
        +evaluate(request: Request) RuleResult
        +addRule(rule: ProcessingRule) void
        +removeRule(id: UUID) void
    }

    RequestController --> RequestService
    RequestService --> RequestRepository
    RequestService --> EventPublisher
    RequestService --> RuleEngine
    PostgresRequestRepository ..|> RequestRepository
    RabbitMQEventPublisher ..|> EventPublisher
```

## 4. Class Diagram: Value Objects

```mermaid
classDiagram
    class Money {
        +Decimal amount
        +String currency
        +add(other: Money) Money
        +subtract(other: Money) Money
        +multiply(factor: Decimal) Money
        +isGreaterThan(other: Money) Boolean
        +format() String
    }

    class EmailAddress {
        +String value
        +static validate(email: String) Boolean
        +toString() String
    }

    class PhoneNumber {
        +String countryCode
        +String number
        +static validate(phone: String) Boolean
        +format() String
    }

    class Address {
        +String street
        +String city
        +String state
        +String postalCode
        +String country
        +format() String
    }

    class DateRange {
        +Date start
        +Date end
        +contains(date: Date) Boolean
        +overlaps(other: DateRange) Boolean
        +durationInDays() Integer
    }

    Request --> Money : amount
    Customer --> EmailAddress : email
    Customer --> PhoneNumber : phone
    Customer --> Address : address
```

## 5. Design Patterns Applied

| Pattern | Classes | Purpose |
|---------|---------|---------|
| [Repository] | [RequestRepository, PostgresRequestRepository] | [Abstract data access] |
| [Strategy] | [RuleEngine, ProcessingRule] | [Different validation strategies] |
| [Observer] | [EventPublisher, DomainEvent] | [Decoupled event handling] |
| [Factory] | [Request.create()] | [Encapsulate object creation] |
| [Value Object] | [Money, EmailAddress, Address] | [Immutable, self-validating] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Low-Level-Design]] | Design details |
| [[Sequence-Diagrams]] | Behavioral interactions |
| [[ERD]] | Data model |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC 19501 (UML)
> **Usage:** Class diagrams show *structure* — what classes exist and how they relate. Use them for code design, documentation, and onboarding new developers.
