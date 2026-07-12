---
document_type: Error State Specifications
version: "1.0"
status: Draft
author: "[UX Designer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [error-states, error-handling, ux]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
  - NN/g Error Message Guidelines
---

# Error State Specifications

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines how errors are displayed — visual treatment, messaging, and recovery actions for every error type.

## 2. Error Display Patterns

| Pattern | When to Use | Visual |
|---------|-----------|--------|
| [Inline Error] | [Form field validation] | [Red border + message below field] |
| [Toast Notification] | [Action errors (save, submit)] | [Red toast, auto-dismiss 5s] |
| [Banner Error] | [Page-level errors] | [Red banner at top of page] |
| [Error Page] | [404, 500, network error] | [Full page with illustration] |
| [Empty State Error] | [Data loading failure] | [Illustration + retry button] |

## 3. Form Validation Errors

| Field Type | Error Message | Visual |
|-----------|--------------|--------|
| [Required field] | ["{Label} is required"] | [Red border + message] |
| [Invalid email] | ["Please enter a valid email address"] | [Red border + message] |
| [Min length] | ["{Label} must be at least {N} characters"] | [Red border + message] |
| [Max length] | ["{Label} must be no more than {N} characters"] | [Red border + message] |
| [Invalid format] | ["Please enter a valid {format}"] | [Red border + message] |
| [Number range] | ["{Label} must be between {min} and {max}"] | [Red border + message] |

## 4. System Error Messages

| Error | User Message | Action |
|-------|-------------|--------|
| [Network error] | ["Unable to connect. Please check your internet connection."] | [Retry button] |
| [Server error] | ["Something went wrong. Please try again."] | [Retry button] |
| [Timeout] | ["The request took too long. Please try again."] | [Retry button] |
| [Unauthorized] | ["Your session has expired. Please log in again."] | [Login button] |
| [Forbidden] | ["You don't have permission to do this."] | [Contact support link] |
| [Not found] | ["The page you're looking for doesn't exist."] | [Home button] |
| [Rate limited] | ["Too many requests. Please wait a moment."] | [Auto-retry countdown] |

## 5. Error Page Designs

### 404 — Not Found

```
┌─────────────────────────────────────────────────────────────┐
│                                                              │
│                    [404 Illustration]                        │
│                                                              │
│              Page Not Found                                  │
│                                                              │
│    The page you're looking for doesn't exist or has          │
│    been moved.                                               │
│                                                              │
│              [Go Home]  [Go Back]                            │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### 500 — Server Error

```
┌─────────────────────────────────────────────────────────────┐
│                                                              │
│                    [Error Illustration]                      │
│                                                              │
│              Something Went Wrong                            │
│                                                              │
│    We're experiencing technical difficulties.                │
│    Please try again in a few minutes.                        │
│                                                              │
│              [Try Again]  [Contact Support]                  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Network Error

```
┌─────────────────────────────────────────────────────────────┐
│                                                              │
│                    [Offline Illustration]                    │
│                                                              │
│              No Internet Connection                          │
│                                                              │
│    Please check your network settings and try again.         │
│                                                              │
│              [Try Again]                                     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## 6. Error Message Guidelines

| Rule | Example |
|------|---------|
| [Be specific] | ✅ "Email is required" ❌ "Field required" |
| [Be helpful] | ✅ "Please enter a valid email (e.g., name@example.com)" ❌ "Invalid input" |
| [Be human] | ✅ "Something went wrong" ❌ "Error 500 Internal Server Error" |
| [Provide actions] | ✅ "Try again" button ❌ No recovery option |
| [Don't blame user] | ✅ "Unable to save" ❌ "You entered wrong data" |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[State Variations]] | Error states as part of overall states |
| [[Component Library]] | Error component specs |
| [[Accessibility Audit]] | Accessible error requirements |

---

> **Template Standard:** Based on ISO 9241-210, NN/g
> **Usage:** Every error needs: (1) what happened, (2) why, (3) what to do next. Never show raw technical errors to users.
