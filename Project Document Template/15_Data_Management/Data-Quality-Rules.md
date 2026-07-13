---
document_type: Data Quality Rules
version: "1.0"
status: Active
author: "[Data Steward]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [data-quality, rules, validation, dmbok]
standard_ref:
  - DMBOK v2 — Data Quality
---

# Data Quality Rules

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines specific data quality rules — automated checks that validate data meets business requirements.

## 2. Rule Categories

| Category | Description | Examples |
|---------|-----------|---------|
| [Completeness] | [Required fields populated] | [No nulls in required fields] |
| [Validity] | [Data conforms to format/rules] | [Email format, amount > 0] |
| [Uniqueness] | [No duplicate records] | [Unique email, unique ID] |
| [Referential] | [FK references exist] | [Customer exists, category exists] |
| [Consistency] | [Same data across systems] | [Cross-system comparison] |
| [Timeliness] | [Data available when needed] | [Freshness < 1 hour] |

## 3. Quality Rules

### Customer Rules

| Rule ID | Rule | Dimension | Field | Threshold | Implementation |
|---------|------|----------|-------|----------|---------------|
| [DQ-C01] | [Email format valid] | [Validity] | [email] | [100%] | `z.string().email()` |
| [DQ-C02] | [Email unique] | [Uniqueness] | [email] | [100%] | `UNIQUE constraint` |
| [DQ-C03] | [Name not null] | [Completeness] | [name] | [100%] | `NOT NULL constraint` |
| [DQ-C04] | [Phone E.164 format] | [Validity] | [phone] | [100%] | `z.string().regex(/^\+[1-9]\d{1,14}$/)` |
| [DQ-C05] | [Type valid enum] | [Validity] | [type] | [100%] | `CHECK (type IN ('STANDARD', 'VIP', 'CORPORATE'))` |

### Request Rules

| Rule ID | Rule | Dimension | Field | Threshold | Implementation |
|---------|------|----------|-------|----------|---------------|
| [DQ-R01] | [Amount > 0] | [Validity] | [amount] | [100%] | `CHECK (amount > 0)` |
| [DQ-R02] | [Customer exists] | [Referential] | [customer_id] | [100%] | `FOREIGN KEY` |
| [DQ-R03] | [Category exists] | [Referential] | [category_id] | [100%] | `FOREIGN KEY` |
| [DQ-R04] | [Status exists] | [Referential] | [status_id] | [100%] | `FOREIGN KEY` |
| [DQ-R05] | [Description not empty] | [Completeness] | [description] | [100%] | `CHECK (length(description) > 0)` |
| [DQ-R06] | [Priority valid enum] | [Validity] | [priority] | [100%] | `CHECK (priority IN ('LOW', 'NORMAL', 'HIGH', 'URGENT'))` |
| [DQ-R07] | [Submitted date set] | [Completeness] | [submitted_at] | [100%] | `NOT NULL DEFAULT now()` |

### Transaction Rules

| Rule ID | Rule | Dimension | Field | Threshold | Implementation |
|---------|------|----------|-------|----------|---------------|
| [DQ-T01] | [Amount > 0] | [Validity] | [amount] | [100%] | `CHECK (amount > 0)` |
| [DQ-T02] | [Request exists] | [Referential] | [request_id] | [100%] | `FOREIGN KEY` |
| [DQ-T03] | [Currency valid] | [Validity] | [currency] | [100%] | `CHECK (length(currency) = 3)` |
| [DQ-T04] | [Type valid] | [Validity] | [type] | [100%] | `CHECK (type IN ('PAYMENT', 'REFUND', 'ADJUSTMENT'))` |

## 4. Rule Implementation

| Layer | Implementation | Enforcement |
|-------|---------------|-----------|
| [Database] | [Constraints, triggers] | [Hard enforcement] |
| [Application] | [Zod schemas] | [Validation at input] |
| [ETL] | [Quality checks] | [Quarantine on fail] |
| [Monitoring] | [Great Expectations] | [Alert on violation] |

## 5. Rule Monitoring

| Rule ID | Pass Rate | Trend | Last Check | Status |
|---------|----------|-------|-----------|--------|
| [DQ-C01] | [100%] | → | [YYYY-MM-DD] | ✅ |
| [DQ-C02] | [100%] | → | [YYYY-MM-DD] | ✅ |
| [DQ-R01] | [100%] | → | [YYYY-MM-DD] | ✅ |
| [DQ-R02] | [100%] | → | [YYYY-MM-DD] | ✅ |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Data-Quality-Strategy]] | Quality framework |
| [[Data-Profiling-Report]] | Profiling results |
| [[Data-Quality-Scorecard]] | Quality metrics |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Quality rules are *automated checks*. If a rule can't be automated, it's a guideline, not a rule.
