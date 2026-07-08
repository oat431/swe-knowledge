---
tags: [overview, body-of-knowledge, software-engineering, project-management, systems-engineering]
---

# Body of Knowledge — Overview

> **Purpose:** A curated collection of three major Bodies of Knowledge covering the full spectrum of modern engineering disciplines — Software Engineering, Project Management, and Systems Engineering. Each BOK is a comprehensive, chapter-organized vault with deep cross-linking via [[wikilinks]].

## The Three Bodies of Knowledge

| BOK | Source | Edition | Files | Size | Focus |
|---|---|---|---|---|---|
| 💻 **SWEBOK** | IEEE Computer Society | v4 (2024) | 20 | ~200 KB | Software Engineering — 18 Knowledge Areas |
| 📋 **PMBOK** | Project Management Institute | v8 (2025) | 19 | ~274 KB | Project Management — 7 Performance Domains |
| ⚙️ **SEBoK** | BKCASE | v2 (2025) | 23 | ~366 KB | Systems Engineering — 8 Parts, 30+ Knowledge Areas |

## SWEBOK v4 — Software Engineering

[[SWEBOK v4 - Overview|→ Full Overview]]

The **Software Engineering Body of Knowledge** defines the generally accepted knowledge of the software engineering discipline. 18 Knowledge Areas organized in three tiers:

| Tier | Knowledge Areas |
|---|---|
| 🔧 **Core Engineering** | Requirements, Architecture, Design, Construction, Testing, Operations, Maintenance |
| 📋 **Management & Process** | Configuration Management, Engineering Management, Engineering Process |
| 🎯 **Quality & Cross-Cutting** | Models & Methods, Quality, Security |
| 👤 **Professional Practice** | Professional Practice, Economics |
| 🧮 **Foundations** | Computing, Mathematical, Engineering |

**Vault:** `SWEBOK\` — 20 files | **Essential Docs:** [[../Essential Document/SWEBOK Essential Documents|Document checklist]]

## PMBOK® Guide v8 — Project Management

[[PMBOK v8 - Overview|→ Full Overview]]

The **Project Management Body of Knowledge** is the ANSI-accredited global standard for project management. Two parts in one volume:

| Part | Content |
|---|---|
| **The Standard** | Introduction, Value Delivery System, 6 Principles, Project Life Cycles & Focus Areas |
| **The Guide** | 7 Performance Domains (Governance, Scope, Schedule, Finance, Stakeholders, Resources, Risk), Tailoring, I/O Catalog, Tools Catalog |
| **Appendices** | PMO, AI in Projects, Procurement, Evolution of PMBOK |

**Vault:** `PMBOK\` — 19 files | **Essential Docs:** [[../Essential Document/PMBOK Essential Documents|Document checklist]]

## SEBoK v2 — Systems Engineering

[[SEBoK v2 - Overview|→ Full Overview]]

The **Systems Engineering Body of Knowledge** is the definitive reference for the systems engineering discipline. 8 Parts spanning foundations to emerging knowledge:

| Part | Content |
|---|---|
| **Part 1** | SEBoK Introduction |
| **Part 2** | Foundations (Fundamentals, Nature of Systems, Systems Science, Thinking, Models, Approach) |
| **Part 3** | SE & Management (Life Cycles, Processes, Architecture, Realization, Standards) |
| **Part 4** | Applications (Product, Service, Enterprise, SoS, Healthcare) |
| **Part 5** | Enabling SE (Businesses, Teams, Individuals) |
| **Part 6** | Related Disciplines (PM, SWE, Quality, Engineering Disciplines) |
| **Part 7** | Implementation Examples (Case Studies) |
| **Part 8** | Emerging Knowledge (AI, Digital Engineering, MBSE, SE Transformation) |

**Vault:** `System Engineer BOK\` — 23 files | **Essential Docs:** [[../Essential Document/SEBOK Essential Documents|Document checklist]]

## How These BOKs Relate

```
┌──────────────────────────────────────────────────┐
│              SYSTEMS ENGINEERING                  │
│  (SEBoK — the big picture: hardware + software    │
│   + people + processes as an integrated system)   │
│  ┌──────────────────────────────────────────────┐ │
│  │         SOFTWARE ENGINEERING                  │ │
│  │  (SWEBOK — building the software within       │ │
│  │   the system: requirements → architecture     │ │
│  │   → design → code → test → deploy → maintain) │ │
│  │  ┌──────────────────────────────────────────┐ │ │
│  │  │        PROJECT MANAGEMENT                │ │ │
│  │  │  (PMBOK — managing the work: initiating   │ │ │
│  │  │   → planning → executing → monitoring     │ │ │
│  │  │   → closing, across all disciplines)      │ │ │
│  │  └──────────────────────────────────────────┘ │ │
│  └──────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────┘
```

- **PMBOK** tells you *how to manage the work*
- **SWEBOK** tells you *how to build the software*
- **SEBoK** tells you *how to engineer the whole system*

They're complementary — a large software project typically draws from all three.

## Reading Paths

- **New software engineer:** SWEBOK → PMBOK (essentials)
- **Senior engineer / tech lead:** SWEBOK (architecture, quality, security) → SEBoK (foundations, systems thinking)
- **Project manager:** PMBOK → SEBoK (SE & PM) → SWEBOK (understanding the work being managed)
- **Systems engineer:** SEBoK → SWEBOK (SE & SWE) → PMBOK (SE & PM)
- **Quick document checklist:** [[../Essential Document/Essential Documents - Overview|Essential Documents →]]

## Related

- [[../Essential Document/Essential Documents - Overview|Essential Documents]] — Document-only checklists extracted from these BOKs
