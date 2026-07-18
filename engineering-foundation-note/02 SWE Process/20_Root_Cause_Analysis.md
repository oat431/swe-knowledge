---
tags: [root-cause-analysis, problem-solving, engineering-foundations, swebok]
---

# Root Cause Analysis

> *Source: SWEBOK v4 Engineering Foundations KA; Industry standards (IEC 60812, IEC 61025)*

## Purpose

Root Cause Analysis (RCA) is a class of problem-solving methods to identify the underlying causes of undesirable outcomes — preventing recurrence rather than just treating symptoms. RCA serves four roles in software engineering:

1. **Identifying real problems** — distinguishing symptoms from causes
2. **Exposing risk drivers** — understanding what could go wrong
3. **Revealing process improvement opportunities** — finding systemic weaknesses
4. **Discovering sources of recurring defects** — breaking the cycle

## Core Principle

> **"Ask why five times"** — Taiichi Oho (Toyota Production System)
> 
> Every problem has a chain of causes. Dig deep enough and you find the root — the one fix that prevents the problem from happening again.

## RCA Techniques

### 1. 5-Whys

The simplest RCA technique — repeatedly ask "Why?" until you reach the root cause.

**Example:**
```
Problem: The server crashed
Why 1: It ran out of memory
Why 2: A memory leak in the payment service
Why 3: The connection pool wasn't releasing connections
Why 4: The cleanup code had a race condition
Why 5: No concurrency testing was done for the connection pool
Root cause: Missing concurrency test coverage → Add concurrency tests to CI
```

**Rules:**
- Stop when you reach a cause you can fix
- Each "why" must be grounded in evidence, not speculation
- Branch when multiple causes contribute (use Ishikawa for this)
- Document the chain for future reference

### 2. Ishikawa Diagram (Fishbone / Cause-and-Effect)

Visual tool for mapping multiple potential causes of a problem, grouped by category.

**Structure:**
```
                    ┌─ People ──────────┐
                    │                   │
                    ├─ Process ─────────┤
                    │                   │
    Effect ◄────────├─ Technology ──────┤
   (Problem)        │                   │
                    ├─ Environment ─────┤
                    │                   │
                    └─ Materials ───────┘
```

**Categories (6M for manufacturing, adapted for software):**

| Category | Software Equivalent | Examples |
|---|---|---|
| **People** | Team, skills, communication | Skill gaps, miscommunication, turnover |
| **Process** | Methods, workflows, standards | Missing reviews, unclear requirements, no code reviews |
| **Technology** | Tools, infrastructure, code | Bug in framework, misconfigured server, legacy code |
| **Environment** | External factors, constraints | Regulatory changes, vendor issues, market pressure |
| **Materials** | Inputs, data, resources | Bad data, incomplete specs, outdated documentation |
| **Management** | Leadership, culture, priorities | Unrealistic deadlines, insufficient resources, unclear vision |

**When to use:** When a problem has multiple potential causes and you need to explore them systematically before diving deep.

### 3. Fault Tree Analysis (FTA)

Formal, top-down deductive method using Boolean logic (AND/OR gates) to trace from an undesirable event to its root causes.

**Symbols:**
```
    ┌─────────┐
    │  Event  │  — Undesirable event or intermediate cause
    └────┬────┘
         │
    ┌────┴────┐
    │  AND    │  — All input events must occur for output
    │  OR     │  — Any input event triggers output
    └────┬────┘
         │
    ┌────┴────┐
    │ Basic   │  — Root cause (no further decomposition)
    │ Event   │
    └─────────┘
```

**Example:**
```
              [Data Loss]
                  │
                 OR
              ┌───┴───┐
              │       │
        [Disk Failure] [Accidental Delete]
              │               │
             AND              OR
          ┌───┴───┐       ┌───┴───┐
          │       │       │       │
    [No Backup] [Hardware] [No Confirm] [Bulk Delete]
     [Failure]   [Age]     [Dialog]    [Enabled]
```

**When to use:** Safety-critical systems, when you need formal documentation of failure paths, when causes interact (AND gates).

**Standards:** IEC 61025 (Fault Tree Analysis)

### 4. Failure Modes and Effects Analysis (FMEA)

Forward-chaining, inductive method that systematically examines each component to identify how it can fail and what the consequences are.

**FMEA Worksheet:**

| Component | Failure Mode | Effect | Severity | Cause | Occurrence | Detection | RPN |
|---|---|---|---|---|---|---|---|
| Database connection | Connection timeout | Service unavailable | 8 | Network outage | 3 | Health check | 24 |
| Payment API | Invalid response | Payment failed | 9 | API version mismatch | 2 | Validation | 18 |
| Cache | Cache miss | Slow response | 4 | Key expiration | 7 | Monitoring | 28 |

**Risk Priority Number (RPN):**
$$RPN = Severity \times Occurrence \times Detection$$

- **Severity (S):** Impact if failure occurs (1–10)
- **Occurrence (O):** Likelihood of failure (1–10)
- **Detection (D):** Likelihood of detecting before impact (1–10, higher = harder to detect)

**Priority:** RPN > 100 → immediate action; 50–100 → planned improvement; < 50 → monitor

**When to use:** Proactive risk assessment during design, before release, when evaluating system components.

**Standards:** IEC 60812 (FMEA), SAE J1739 (FMEA for automotive)

### 5. Cause Maps (Root Cause Map)

Evidence-based, structured cause-effect chains that combine backward (why did it happen?) and forward (what will happen?) analysis.

**Structure:**
```
[Evidence] → [Cause 1] → [Cause 2] → [Root Cause]
                    ↓
              [Effect 1] → [Effect 2] → [Impact]
```

**Difference from 5-Whys:**
- 5-Whys: linear chain, single path
- Cause maps: branching chains, multiple evidence nodes, includes forward effects

### 6. Pareto Analysis (80/20 Rule)

Prioritize problems by frequency and resource consumption.

**Principle:** 80% of effects come from 20% of causes.

**Application:**
1. Collect data on defect types and frequencies
2. Sort by frequency (descending)
3. Calculate cumulative percentage
4. Focus on the top 20% that account for 80% of defects

**Example:**

| Defect Type | Count | Cumulative % |
|---|---|---|
| Null pointer | 45 | 30% |
| SQL injection | 30 | 50% |
| Timeout | 20 | 64% |
| Race condition | 15 | 74% |
| Memory leak | 12 | 82% |
| Other (15 types) | 28 | 100% |

→ Focus on null pointer, SQL injection, and timeout first (covers 64% of defects).

## RCA Process (6 Steps)

1. **Select the problem** — Define the undesirable outcome clearly
2. **Gather evidence** — Collect data, logs, reports, interviews
3. **Identify root cause(s)** — Apply appropriate technique(s)
4. **Select corrective actions** — Choose fixes that address the root cause
5. **Implement** — Apply the fixes
6. **Observe effectiveness** — Monitor to confirm the problem doesn't recur

> **RCA must lead to action.** An RCA that produces a report but no changes is wasted effort.

## Choosing the Right Technique

| Situation | Best Technique |
|---|---|
| Simple problem, single chain | 5-Whys |
| Multiple potential causes | Ishikawa |
| Safety-critical, formal documentation needed | FTA |
| Proactive risk assessment during design | FMEA |
| Complex problem with evidence chains | Cause Maps |
| Prioritizing which problems to fix first | Pareto Analysis |

## Related

- [[Engineering Foundation Overview]] — All engineering foundation topics
- [[02 SWE Process/18_Evaluation_and_Improvement|18_Evaluation_and_Improvement]] — Process improvement and metrics
- [[02 SWE Process/16_Testing_Strategies|16_Testing_Strategies]] — Finding defects before RCA is needed
- [[02 SWE Process/11_Project_Planning_and_Management|11_Project_Planning_and_Management]] — Risk management
