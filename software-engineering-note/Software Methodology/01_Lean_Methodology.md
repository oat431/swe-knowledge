---
tags: [lean, kanban, waste, continuous-improvement, methodology, software-methodology]
---

# Lean Methodology

> *Source: Lean Software Development by Mary & Tom Poppendieck (2003); Toyota Production System concepts*

## Purpose

Lean Software Development adapts the Toyota Production System's principles to software. It focuses on eliminating waste, amplifying learning, and delivering value as quickly as possible. Lean is the philosophical foundation that underpins both Agile and Kanban.

## The Seven Principles

### 1. Eliminate Waste
Waste in software development includes:
- **Partially done work** — Uncommitted code, untested features, unvalidated requirements
- **Extra features** — Gold plating, features nobody asked for
- **Relearning** — Knowledge lost through turnover, inadequate documentation, poor handoffs
- **Handoffs** — Context switching, waiting for approvals, queueing between teams
- **Task switching** — Multitasking destroys productivity; prefer single-piece flow
- **Delays** — Waiting for decisions, environments, dependencies, approvals
- **Defects** — Bugs found late cost exponentially more to fix

### 2. Amplify Learning
- **Iterative development** — Build a little, learn a lot
- **Set-based development** — Explore multiple solutions simultaneously, then converge
- **Feedback loops** — Short iterations, demos, retrospectives, A/B testing
- **Knowledge sharing** — Pair programming, communities of practice, documentation

### 3. Decide as Late as Possible
- **Delay irreversible decisions** — Keep options open until the last responsible moment
- **Real options** — Treat decisions like options: they have value, expiration dates, and costs
- **Iterative design** — Architecture emerges through refactoring, not big upfront design
- **Just-in-time planning** — Plan in detail only for the next iteration

### 4. Deliver as Fast as Possible
- **Short iterations** — Deliver working software every 1–4 weeks
- **Single-piece flow** — One item at a time, finished before starting the next
- **Pull systems** — Start work only when there's capacity (Kanban)
- **Limit work in progress (WIP)** — Reducing WIP increases throughput
- **Cycle time** — Measure and minimize time from start to done

### 5. Empower the Team
- **Respect people** — Trust the team to make technical decisions
- **Self-organization** — Teams that plan their own work outperform directed teams
- **Motivation** — Autonomy, mastery, purpose (Daniel Pink's Drive)
- **Leadership as service** — Remove impediments, don't micromanage

### 6. Build Integrity In
- **Perceived integrity** — The system feels right to the customer (UX, consistency)
- **Conceptual integrity** — The system has a coherent design (architecture, patterns)
- **Code quality** — TDD, refactoring, code reviews, static analysis
- **Continuous integration** — Integrate and test frequently to catch problems early

### 7. Optimize the Whole
- **Systems thinking** — Optimize the whole value stream, not individual components
- **Sub-optimization** — Optimizing one team's output can harm overall throughput
- **Value stream mapping** — Visualize end-to-end flow to find bottlenecks
- **Theory of Constraints** — Focus improvement on the current bottleneck

## Lean Concepts in Practice

### Value Stream Mapping
Visualizing the flow of work from request to delivery:
- **Lead time** — Time from request to delivery (customer's perspective)
- **Cycle time** — Time from start to finish (team's perspective)
- **Wait time** — Time items spend waiting between steps
- **Process time** — Time actually being worked on

### The Eight Wastes (DOWNTIME)
| Waste | Software Equivalent |
|---|---|
| **D**efects | Bugs, production incidents |
| **O**verproduction | Gold plating, extra features |
| **W**aiting | Waiting for approvals, environments, dependencies |
| **N**on-utilized talent | Underusing team skills, micromanagement |
| **T**ransportation | Handoffs between teams, context loss |
| **I**nventory | Partially done work, backlog bloat |
| **M**otion | Task switching, unnecessary meetings |
| **E**xtra processing | Unnecessary documentation, redundant reviews |

### Kaizen (Continuous Improvement)
- Small, incremental improvements every day
- Blameless postmortems and retrospectives
- The improvement kata: understand direction → grasp current condition → establish target condition → experiment toward target
- Everyone is responsible for improvement, not just managers

## Lean vs. Agile vs. Waterfall

| Aspect | Lean | Agile (Scrum) | Waterfall |
|---|---|---|---|
| **Focus** | Flow optimization | Iteration & feedback | Phase completion |
| **WIP limits** | Core practice | Not explicit | N/A |
| **Planning** | Just-in-time | Sprint planning | Upfront |
| **Iterations** | Continuous flow | Fixed sprints | None |
| **Metrics** | Lead time, cycle time, throughput | Velocity, burndown | Planned vs. actual |
| **Roles** | Minimal | Product Owner, Scrum Master | Project Manager |
| **Best for** | Continuous delivery, operations | Product development | Fixed scope |

## Related

- [[00_Agile_Methodology]] — Agile practices that build on Lean principles
- [[03_Kanban_and_Flow]] — Kanban as a Lean implementation
- [[02_Methodologies_Overview]] — Comparison across all methodologies
