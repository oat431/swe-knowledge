---
document_type: Deviation / Waiver Records
version: "1.0"
status: Active
author: "[Configuration Manager]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [deviation, waiver, exception, swebok]
standard_ref:
  - SWEBOK v4 — Configuration Management
---

# Deviation / Waiver Records

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Records deviations (temporary departures) and waivers (permanent exceptions) from specified requirements or standards.

## 2. Deviation vs Waiver

| Type | Definition | Duration | Approval | Example |
|------|-----------|---------|---------|--------|
| [Deviation] | [Temporary departure from spec] | [Limited time/quantity] | [CCB] | [Use placeholder for missing API] |
| [Waiver] | [Permanent exception to spec] | [Indefinite] | [CCB + Sponsor] | [Lower performance target accepted] |

## 3. Deviation/Waiver Template

| Field | Value |
|-------|-------|
| **DW ID** | [DW-XXX] |
| **Type** | [Deviation / Waiver] |
| **Title** | [Brief description] |
| **Requirement** | [Requirement being waived] |
| **Reason** | [Why the deviation/waiver is needed] |
| **Impact** | [What's affected] |
| **Duration** | [For deviations — when it expires] |
| **Conditions** | [Any conditions for the exception] |
| **Status** | [Requested / Approved / Active / Expired / Revoked] |
| **Requested By** | [Name, Role] |
| **Request Date** | [YYYY-MM-DD] |
| **Approved By** | [CCB / Sponsor] |
| **Approval Date** | [YYYY-MM-DD] |

### Justification

> [Clear justification for why this exception is necessary]

### Risk Assessment

| Risk | Probability | Impact | Mitigation |
|------|-----------|--------|-----------|
| [Risk 1] | [Low/Med/High] | [Low/Med/High] | [Mitigation] |
| [Risk 2] | [Low/Med/High] | [Low/Med/High] | [Mitigation] |

### Conditions & Monitoring

| # | Condition | Monitoring | Owner | Status |
|---|----------|-----------|-------|--------|
| 1 | [Condition 1] | [How monitored] | [Name] | ⬜ |
| 2 | [Condition 2] | [How monitored] | [Name] | ⬜ |

## 4. Deviation/Waiver Register

| ID | Type | Requirement | Reason | Status | Requested | Expiry |
|----|------|-----------|--------|--------|---------|--------|
| [DW-001] | [Deviation] | [NFR-001: Response < 2s] | [Initial load higher than expected] | ✅ Expired | [YYYY-MM-DD] | [YYYY-MM-DD] |
| [DW-002] | [Waiver] | [FR-303: Ad-hoc reports] | [Deferred to Phase 2] | 🟡 Active | [YYYY-MM-DD] | [Phase 2] |
| [DW-003] | [Deviation] | [SEC-005: MFA required] | [MFA rollout delayed] | 🟡 Active | [YYYY-MM-DD] | [YYYY-MM-DD] |

## 5. Deviation/Waiver Metrics

| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| [Active deviations] | [X] | [< 5] | 🟢🟡🔴 |
| [Active waivers] | [X] | [< 3] | 🟢🟡🔴 |
| [Expired deviations] | [X] | [0 active past expiry] | 🟢🟡🔴 |
| [Deviations per release] | [X] | [Decreasing] | 🟢🟡🔴 |

## 6. Expiration Tracking

| ID | Type | Expiry Date | Status | Action |
|----|------|-----------|--------|--------|
| [DW-001] | [Deviation] | [YYYY-MM-DD] | ✅ Expired | [Requirement now met] |
| [DW-003] | [Deviation] | [YYYY-MM-DD] | 🟡 Active | [MFA rollout in progress] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SCMP]] | CM plan |
| [[Baseline-Records]] | Baselines with deviations |
| [[Change-Request]] | Related changes |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Deviations and waivers are *controlled exceptions* — not "we decided to skip it." Every exception needs justification, approval, and monitoring.
