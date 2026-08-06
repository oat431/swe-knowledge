---
title: "Access Control for Data"
note_type: capability-topic
capability_area: data-security-and-privacy
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
  - "[[01_Data_Classification_and_Sensitivity]]"
tags:
  - career-path
  - data-engineering
  - access-control
  - RBAC
  - row-level-security
---

# Access Control for Data

> Controlling who can see what data, at what granularity, and under what conditions.

## Why This Is a Senior Skill

Mid-level engineers grant permissions when asked. Senior engineers design access control models that scale across hundreds of tables and thousands of users without creating a permission-management bottleneck or accidentally exposing sensitive data through overly broad grants.

The senior challenge is balancing the principle of least privilege with the practical need for analysts and data scientists to do their jobs without filing a ticket for every new table.

## Core Frameworks

### Access Control Models Compared

| Model | How It Works | Scales When | Breaks When |
|-------|-------------|-------------|-------------|
| RBAC: Role-Based | Permissions attached to roles, users get roles | Roles are well-defined and stable | Roles multiply beyond management |
| ABAC: Attribute-Based | Rules evaluate user, resource, and environment attributes | Attributes are consistent and governed | Attribute sources disagree or drift |
| Row-Level Security | Database filters rows based on user context | Filter predicates are indexable | Complex predicates degrade query performance |
| Column Masking | Sensitive columns return masked values | Masking rules are simple and uniform | Business logic depends on seeing real values |
| Policy-as-Code | Rules defined in code, versioned and tested | Policies are shared across systems | Policy language is unfamiliar to data teams |

### Row-Level Security Design

| Design Choice | Trade-off |
|---------------|-----------|
| Static predicates: department = user.dept | Simple, fast, but rigid |
| Dynamic predicates: lookup table join | Flexible, but adds join cost to every query |
| Session variable injection | No schema change, but depends on connection setup |
| View-based RLS | Portable across engines, but view proliferation |

### Granularity Escalation

| Granularity | Example | When to Use |
|-------------|---------|-------------|
| Database-level | Full DB access | System administrators only |
| Schema-level | Read schema, no write | Analyst teams |
| Table-level | Read specific tables | Data pipeline consumers |
| Column-level | Mask or hide columns | PII protection for analysts |
| Row-level | Filter rows by context | Multi-tenant systems, regional data |
| Cell-level | Mask individual cells | Rare, high-security environments |

## In Practice

**Start with RBAC, graduate to ABAC when roles explode.** If you have more than 15 roles per system, the model is breaking down. ABAC with attributes like department, region, and data classification reduces role count while maintaining least privilege.

**Implement RLS at the database layer, not the application layer.** Application-level filtering creates gaps: ad-hoc queries, BI tools, and data exports bypass the app. Database-level RLS enforces the policy regardless of access path.

**Use column masking for analysts who need structure but not values.** An analyst studying churn patterns needs to see that a column exists and has a distribution. They do not need the actual email address. Dynamic masking returns hashed or partial values.

**Test access control like code.** Write integration tests that verify: user A cannot see rows belonging to department B, masked columns return masked values, role changes take effect within the expected window. Access control without tests is security theater.

**Audit permission grants.** The most common data breach vector is over-privileged accounts. Run monthly reports on who has what access, flag accounts with access to more than N Restricted tables, and require re-justification.

## Practical Exercise

Design an access control model for a multi-tenant SaaS analytics system:
1. Define 5 roles with their permission boundaries
2. Write RLS predicates for tenant isolation: each tenant sees only their own data
3. Define column masking rules for PII columns visible to support staff
4. Write 3 integration tests that verify access denial for unauthorized access
5. Sketch a monthly access review report: what metrics would you track?

## Knowledge Connections

- [[05_Data_Security]]: DMBOK access control guidance
- [[01_Data_Classification_and_Sensitivity]]: tiers drive access rules
- [[02_Encryption_and_Tokenization]]: encryption complements access control
- [[05_Audit_and_Lineage_for_Compliance]]: auditing who accessed what
- [[04_Privacy_Engineering]]: access control enforces privacy policies

## Common Pitfalls

- Application-only access control: ad-hoc queries, BI tools, and exports bypass app-level filters
- Role explosion: more than 15 roles per system means the model has broken down
- Static permissions without review: accounts accumulate access over time without losing old grants
- RLS predicates that cause full table scans: poorly indexed filter predicates degrade query performance by 10-100x

## Key Takeaways

- RBAC scales until roles multiply beyond management; ABAC handles complex attribute-based decisions
- Row-level security must be enforced at the database layer, not the application layer
- Column masking lets analysts work with data structure without exposing sensitive values
- Access control needs automated tests: untested permissions are unverified security
- Permission grants must be audited regularly; over-privileged accounts are the top breach vector
- Senior engineers design access models that balance least privilege with analyst productivity
