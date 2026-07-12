---
document_type: Pseudocode / PDL
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
tech_lead: "[Technical Lead]"
classification: "Internal / Confidential"
tags: [pseudocode, pdl, algorithm, swebok, iso-19501]
standard_ref:
  - SWEBOK v4 — Design
  - ISO/IEC 19501 — UML (Activity Diagrams)
---

# Pseudocode / PDL (Program Design Language)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document contains pseudocode for complex algorithms — language-independent descriptions of logic that bridge the gap between design and code.

## 2. Algorithm Index

| # | Algorithm | Complexity | Module | Status |
|---|---------|-----------|--------|--------|
| ALG-001 | [Auto-Approval Evaluation] | [O(n)] | [Processing Service] | ✅ Approved |
| ALG-002 | [Risk Score Calculation] | [O(n)] | [Processing Service] | ✅ Approved |
| ALG-003 | [Duplicate Detection] | [O(n log n)] | [Processing Service] | ✅ Approved |
| ALG-004 | [Request Classification] | [O(1)] | [Processing Service] | ✅ Approved |
| ALG-005 | [Notification Routing] | [O(n)] | [Notification Service] | ✅ Approved |
| ALG-006 | [Rate Limiting (Sliding Window)] | [O(1)] | [API Gateway] | ✅ Approved |

## 3. Algorithms

### ALG-001: Auto-Approval Evaluation

```pseudocode
ALGORITHM AutoApprovalEvaluation(request: Request) -> ApprovalResult

    // Step 1: Check amount threshold
    IF request.amount > AMOUNT_THRESHOLD THEN
        IF NOT IsVIPCustomer(request.customerId) THEN
            RETURN MANUAL_REVIEW
        END IF
        IF request.amount > VIP_THRESHOLD THEN
            RETURN MANUAL_REVIEW
        END IF
    END IF

    // Step 2: Check for duplicates
    IF HasDuplicateInLast30Days(request.customerId, request.type) THEN
        RETURN FLAG_FOR_REVIEW
    END IF

    // Step 3: Calculate risk score
    riskScore <- CalculateRiskScore(request)
    IF riskScore > RISK_THRESHOLD THEN
        RETURN MANUAL_REVIEW
    END IF

    // Step 4: All checks passed
    RETURN AUTO_APPROVE

END ALGORITHM

CONSTANTS:
    AMOUNT_THRESHOLD = 10000
    VIP_THRESHOLD = 25000
    RISK_THRESHOLD = 0.3
```

### ALG-002: Risk Score Calculation

```pseudocode
ALGORITHM CalculateRiskScore(request: Request) -> Float

    score <- 0.0

    // Factor 1: Amount risk (0-0.3)
    amountRisk <- Min(request.amount / 50000, 1.0) * 0.3
    score <- score + amountRisk

    // Factor 2: New customer risk (0-0.2)
    customerAge <- DaysSince(request.customer.createdAt)
    IF customerAge < 30 THEN
        score <- score + 0.2
    ELSE IF customerAge < 90 THEN
        score <- score + 0.1
    END IF

    // Factor 3: Previous rejection risk (0-0.2)
    rejectionCount <- CountRejections(request.customerId, lastDays=365)
    IF rejectionCount > 0 THEN
        score <- score + Min(rejectionCount * 0.1, 0.2)
    END IF

    // Factor 4: Request frequency risk (0-0.15)
    requestCount <- CountRequests(request.customerId, lastDays=30)
    IF requestCount > 5 THEN
        score <- score + 0.15
    ELSE IF requestCount > 3 THEN
        score <- score + 0.10
    END IF

    // Factor 5: Document completeness (0-0.15)
    IF request.documents IS EMPTY THEN
        score <- score + 0.15
    ELSE IF CountDocuments(request) < 2 THEN
        score <- score + 0.05
    END IF

    // Normalize to 0-1 range
    RETURN Min(score, 1.0)

END ALGORITHM
```

### ALG-003: Duplicate Detection

```pseudocode
ALGORITHM HasDuplicateInLast30Days(customerId: UUID, requestType: String) -> Boolean

    // Query for similar requests in last 30 days
    cutoffDate <- Now() - 30 days

    similarRequests <- Query(
        "SELECT id, amount, created_at 
         FROM requests 
         WHERE customer_id = :customerId 
           AND type = :requestType 
           AND created_at > :cutoffDate
           AND status NOT IN ('REJECTED', 'DRAFT')
         ORDER BY created_at DESC"
    )

    IF similarRequests IS EMPTY THEN
        RETURN FALSE
    END IF

    // Check for exact or near-duplicate
    FOR EACH similar IN similarRequests DO
        amountDiff <- Abs(request.amount - similar.amount) / similar.amount
        IF amountDiff < 0.05 THEN  // Within 5%
            RETURN TRUE
        END IF
    END FOR

    RETURN FALSE

END ALGORITHM
```

### ALG-004: Request Classification

```pseudocode
ALGORITHM ClassifyRequest(request: Request) -> Classification

    // Rule 1: VIP customers get VIP classification
    IF IsVIPCustomer(request.customerId) THEN
        RETURN Classification.VIP
    END IF

    // Rule 2: Corporate customers
    IF request.metadata.corporateAccount = TRUE THEN
        RETURN Classification.CORPORATE
    END IF

    // Rule 3: High-value requests
    IF request.amount >= 50000 THEN
        RETURN Classification.HIGH_VALUE
    END IF

    // Rule 4: Default
    RETURN Classification.STANDARD

END ALGORITHM

TYPE Classification = STANDARD | VIP | CORPORATE | HIGH_VALUE
```

### ALG-005: Notification Routing

```pseudocode
ALGORITHM RouteNotification(event: Event, recipient: Customer) -> Notification[]

    notifications <- []

    // Always send email
    emailNotification <- CreateNotification(
        recipient=recipient,
        channel=EMAIL,
        template=GetTemplate(event.type, EMAIL)
    )
    notifications.Add(emailNotification)

    // Send SMS for critical events
    IF event.type IN [APPROVED, REJECTED, ESCALATED] THEN
        IF recipient.preferences.smsEnabled THEN
            smsNotification <- CreateNotification(
                recipient=recipient,
                channel=SMS,
                template=GetTemplate(event.type, SMS)
            )
            notifications.Add(smsNotification)
        END IF
    END IF

    // Always create in-app notification
    inAppNotification <- CreateNotification(
        recipient=recipient,
        channel=IN_APP,
        template=GetTemplate(event.type, IN_APP)
    )
    notifications.Add(inAppNotification)

    RETURN notifications

END ALGORITHM
```

### ALG-006: Rate Limiting (Sliding Window)

```pseudocode
ALGORITHM CheckRateLimit(clientId: String, limit: Integer, windowSeconds: Integer) -> Boolean

    now <- CurrentTimestamp()
    windowStart <- now - windowSeconds

    // Get or initialize sliding window from cache
    key <- "ratelimit:" + clientId
    window <- Cache.Get(key)

    IF window IS NULL THEN
        window <- SlidingWindow(start=now, count=0)
    END IF

    // Remove expired entries
    WHILE window.entries.Front() < windowStart DO
        window.entries.PopFront()
        window.count <- window.count - 1
    END WHILE

    // Check limit
    IF window.count >= limit THEN
        RETURN FALSE  // Rate limited
    END IF

    // Add current request
    window.entries.PushBack(now)
    window.count <- window.count + 1

    // Update cache with TTL
    Cache.Set(key, window, ttl=windowSeconds)

    RETURN TRUE  // Allowed

END ALGORITHM

TYPE SlidingWindow:
    entries: Queue<Timestamp>
    count: Integer
```

## 4. Complexity Analysis

| Algorithm | Time Complexity | Space Complexity | Notes |
|-----------|---------------|-----------------|-------|
| [Auto-Approval] | [O(n) where n = rules] | [O(1)] | [Sequential rule evaluation] |
| [Risk Score] | [O(n) where n = factors] | [O(1)] | [Fixed number of factors] |
| [Duplicate Detection] | [O(n log n)] | [O(n)] | [DB query + comparison] |
| [Classification] | [O(1)] | [O(1)] | [Fixed rule set] |
| [Notification Routing] | [O(n) where n = channels] | [O(n)] | [Per-channel routing] |
| [Rate Limiting] | [O(1)] | [O(w) where w = window] | [Sliding window] |

## 5. Edge Cases

| Algorithm | Edge Case | Handling |
|-----------|----------|---------|
| [Auto-Approval] | [Amount = 0] | [Validation prevents — amount must be > 0] |
| [Risk Score] | [New customer — no history] | [Default risk = 0.2 for new customers] |
| [Duplicate Detection] | [Customer has 0 requests] | [Return FALSE — no duplicates] |
| [Classification] | [No metadata] | [Default to STANDARD] |
| [Rate Limiting] | [Cache miss] | [Initialize new window] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[LLD (Low-Level Design)]] | Design context |
| [[Class Diagrams]] | Class structure |
| [[Activity Diagrams (Design)]] | Visual workflow of algorithms |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC 19501
> **Usage:** Pseudocode is for *complex logic* — algorithms with multiple branches, loops, or calculations. Simple CRUD doesn't need pseudocode. Write it before coding, review it with peers, then implement.
