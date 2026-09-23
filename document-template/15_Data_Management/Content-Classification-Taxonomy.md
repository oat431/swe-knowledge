---
document_type: Content Classification & Taxonomy
version: "1.0"
status: Draft
author: "[Data Architect]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [content-classification, taxonomy, dmbok]
standard_ref:
  - DMBOK v2 — Document & Content Management
---

# Content Classification & Taxonomy

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines how content is categorized, tagged, and organized — enabling search, retrieval, and governance.

## 2. Content Taxonomy

```mermaid
flowchart TB
    R(("Content<br>Taxonomy"))
    n1["Customer"]
    R --> n1
    n2["Contracts"]
    n1 --> n2
    n3["Applications"]
    n1 --> n3
    n4["Correspondence"]
    n1 --> n4
    n5["ID Documents"]
    n1 --> n5
    n6["Operations"]
    R --> n6
    n7["Requests"]
    n6 --> n7
    n8["Approvals"]
    n6 --> n8
    n9["Reports"]
    n6 --> n9
    n10["Policies"]
    n6 --> n10
    n11["Financial"]
    R --> n11
    n12["Invoices"]
    n11 --> n12
    n13["Payments"]
    n11 --> n13
    n14["Receipts"]
    n11 --> n14
    n15["Statements"]
    n11 --> n15
    n16["Technical"]
    R --> n16
    n17["Architecture"]
    n16 --> n17
    n18["Specifications"]
    n16 --> n18
    n19["Runbooks"]
    n16 --> n19
    n20["Logs"]
    n16 --> n20
    n21["Compliance"]
    R --> n21
    n22["Audit Reports"]
    n21 --> n22
    n23["Certifications"]
    n21 --> n23
    n24["Policies"]
    n21 --> n24
    n25["Evidence"]
    n21 --> n25
```

## 3. Content Categories

| Category | Description | Examples | Classification | Retention |
|---------|-----------|---------|---------------|----------|
| [Customer Documents] | [Customer-submitted content] | [Contracts, ID, applications] | 🔴 L1 | [7 years] |
| [Operational Documents] | [Business process content] | [Requests, approvals, reports] | 🟡 L2 | [5 years] |
| [Financial Documents] | [Financial records] | [Invoices, payments, receipts] | 🔴 L1 | [7 years] |
| [Technical Documents] | [Technical documentation] | [Architecture, specs, runbooks] | 🟡 L2 | [3 years] |
| [Compliance Documents] | [Regulatory content] | [Audit reports, certifications] | 🔴 L1 | [7 years] |

## 4. Metadata Schema

| Field | Required | Type | Description |
|-------|---------|------|-----------|
| [title] | ✅ | [String] | [Document title] |
| [category] | ✅ | [Enum] | [Content category] |
| [classification] | ✅ | [Enum] | [L1/L2/L3/L4] |
| [author] | ✅ | [String] | [Who created] |
| [created_date] | ✅ | [Date] | [Creation date] |
| [retention_date] | ✅ | [Date] | [When to dispose] |
| [tags] | 🟡 | [Array] | [Searchable tags] |
| [related_entity] | 🟡 | [UUID] | [Linked entity ID] |
| [version] | ✅ | [String] | [Document version] |

## 5. Tagging Guidelines

| Guideline | Description | Example |
|----------|-----------|---------|
| [Use controlled vocabulary] | [Predefined tags only] | [contract, invoice, report] |
| [Be specific] | [Use specific tags] | [customer-contract, not just document] |
| [Use consistent naming] | [Lowercase, hyphenated] | [customer-contract, not Customer Contract] |
| [Tag at creation] | [Tag when content is created] | [Auto-tag + manual review] |

## 6. Search & Retrieval

| Feature | Implementation | Coverage |
|---------|---------------|---------|
| [Full-text search] | [Elasticsearch] | [All content] |
| [Metadata search] | [Database queries] | [All metadata] |
| [Category filter] | [Faceted search] | [All categories] |
| [Classification filter] | [Faceted search] | [All classifications] |
| [Date range filter] | [Faceted search] | [All dates] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[ECM-Strategy]] | Content management strategy |
| [[Data-Classification-Schema]] | Data classification |
| [[Business-Glossary]] | Terminology |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Taxonomy is the *filing system*. Without it, content is a pile of files nobody can find.
