---
tags:
  - overview
  - swebok
  - software-operations
  - devops
  - sre
  - ci-cd
---

# Software Engineering Operations — Overview

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 06 — Software Engineering Operations *(NEW in v4)*
> **Purpose:** Manage the deployment, operation, monitoring, and continuous improvement of software systems in production.

## What Is This?

Software Engineering Operations encompasses the practices, tools, and processes that keep software systems running reliably in production. It covers everything from deploying code to servers, monitoring system health, responding to incidents, and continuously improving the delivery pipeline. Operations is where software meets reality — where theoretical correctness confronts real-world load, hardware failures, network partitions, and human error.

The DevOps movement, which began around 2009, fundamentally changed how organizations think about operations. Rather than treating development and operations as separate silos with a "throw it over the wall" handoff, DevOps advocates for shared responsibility, automation, and continuous feedback. SWEBOK v4 formalizes this shift by giving operations its own Knowledge Area.

Modern operations is heavily automated: Infrastructure as Code (IaC), continuous integration/continuous delivery (CI/CD) pipelines, container orchestration, and observability platforms. Site Reliability Engineering (SRE), pioneered at Google, applies software engineering principles to operations problems. Platform engineering builds internal developer platforms that abstract away infrastructure complexity. These are not competing philosophies — they are complementary approaches to the same challenge: delivering reliable software at speed.

## The 7 Topic Areas

### 1. [[DevOps]]
- Culture and principles: shared ownership, automation, continuous feedback, measurement
- The Three Ways: flow, feedback, continual learning and experimentation
- CALMS framework: Culture, Automation, Lean, Measurement, Sharing
- DevOps toolchain: version control → build → test → deploy → monitor → feedback
- Organizational models: embedded ops, platform teams, SRE teams
- **Book:** *The DevOps Handbook, 2nd Ed.* — Gene Kim et al.

### 2. [[CI/CD Pipelines]]
- Continuous Integration: frequent merges, automated builds, fast feedback
- Continuous Delivery: every change is releasable, manual approval for production
- Continuous Deployment: every passing change goes to production automatically
- Pipeline design: stages, gates, parallel execution, caching
- Build artifacts, artifact repositories, and supply chain security
- **Book:** *Accelerate* — Forsgren, Humble & Gene Kim

### 3. [[Infrastructure as Code]]
- Declarative vs. imperative infrastructure management
- Tools: Terraform, Pulumi, CloudFormation, Ansible, Chef, Puppet
- Immutable infrastructure vs. configuration management
- Version-controlled infrastructure, drift detection, and reconciliation
- Environment parity: dev, staging, production consistency
- **Book:** *Infrastructure as Code, 2nd Ed.* — Kief Morris

### 4. [[Site Reliability Engineering (SRE)]]
- Error budgets: balancing reliability and feature velocity
- Service Level Objectives (SLOs), Service Level Indicators (SLIs), SLAs
- Toil reduction and automation as engineering work
- Capacity planning and load management
- Eliminating toil: manual, repetitive, automatable work
- **Book:** *Site Reliability Engineering* — Beyer, Jones, Petoff & Murphy

### 5. [[Platform Engineering]]
- Internal Developer Platforms (IDPs): self-service infrastructure
- Golden paths and paved roads for developer workflows
- Platform as a product: user research, adoption metrics, developer experience
- Service catalogs, templates, and scaffolding
- Relationship to DevOps and SRE: evolution, not replacement
- **Book:** *The DevOps Handbook, 2nd Ed.* — Gene Kim et al.

### 6. [[Incident Management]]
- Incident severity levels and escalation procedures
- Incident response: detect, triage, mitigate, resolve, review
- Post-incident reviews (blameless postmortems) and learning
- On-call practices, runbooks, and playbooks
- Chaos engineering: proactive failure injection (Chaos Monkey, Litmus)
- **Book:** *Site Reliability Engineering* — Beyer, Jones, Petoff & Murphy

### 7. [[Monitoring and Observability]]
- Three pillars: logs, metrics, traces
- Observability vs. monitoring: asking unknown questions vs. watching known signals
- Dashboards, alerting, and alert fatigue management
- Distributed tracing: OpenTelemetry, Jaeger, Zipkin
- Application Performance Monitoring (APM) and Real User Monitoring (RUM)
- **Book:** *Seeking SRE* — David Blank-Edelman (ed.)

## Recommended Books (Priority Order)

| #   | Book                                     | Author(s)                     | Pages |     Priority     |
| --- | ---------------------------------------- | ----------------------------- | :---: | :--------------: |
| 1   | *The DevOps Handbook, 2nd Ed.* (2021)    | Gene Kim et al.               |  480  |   🔴 Essential   |
| 2   | *Site Reliability Engineering* (2016)    | Beyer, Jones, Petoff & Murphy |  558  |   🔴 Essential   |
| 3   | *Accelerate* (2018)                      | Forsgren, Humble & Gene Kim   |  288  |  🟡 Recommended  |
| 4   | *Infrastructure as Code, 2nd Ed.* (2020) | Kief Morris                   |  400  |  🟡 Recommended  |
| 5   | *Seeking SRE* (2018)                     | David Blank-Edelman (ed.)     |  584  | 🟢 Supplementary |

## Relationship to Other KAs

- **[[Software Construction Overview|Software Construction]]** — Build systems, CI pipelines, and deployment scripts are construction artifacts that bridge to operations. Developers who understand operations write better code.
- **[[Software Configuration Management Overview|Software Configuration Management]]** — Version control, release management, and environment configuration are SCM concerns that operations depends on.
- **[[Software Testing Overview|Software Testing]]** — Continuous testing in pipelines, canary deployments, and production monitoring blur the boundary between testing and operations.
- **[[Software Architecture Overview|Software Architecture]]** — Deployment architecture (containers, microservices, service mesh) is an architectural concern shaped by operational needs.
- **[[Software Security Overview|Software Security]]** — DevSecOps integrates security into the operational pipeline. Vulnerability scanning, secret management, and runtime security are operational concerns.
- **[[Software Quality Overview|Software Quality]]** — Operational metrics (uptime, latency, error rates) are quality measures. Reliability is both a quality attribute and an operational concern.

## Related
- [[SWEBOK v4 - Overview]]
- [[08_Software_Configuration_Management/Software Configuration Management Overview|SCM Overview]]
- [[13_Software_Security/Software Security Overview|Software Security Overview]]
