---
title: "SRE and Platform Engineer"
note_type: career-path-overview
career_family: specialist-engineering
level: senior-specialist
entry_from:
  - "[[02_Senior_Software_Engineer]]"
next_paths:
  - "[[03_Staff_Engineer]]"
  - "[[06_Software_Architect]]"
  - "[[11_Engineering_Manager]]"
source_frameworks:
  - "[[SWEBOK v4 - Overview]]"
  - "[[CyBOK v1 - Overview]]"
  - "[[SEBoK v2 - Overview]]"
tags:
  - career-path
  - sre
  - platform-engineering
  - reliability
---

# SRE and Platform Engineer

> **Positioning:** A specialist engineering path focused on making production systems reliable, observable, scalable, secure, and easier for other engineers to operate.

## What This Path Is

Site Reliability Engineering combines a job function, a mindset, and engineering practices for running reliable production systems. Platform engineering applies similar thinking to internal platforms, developer workflows, deployment systems, and paved roads that reduce unnecessary cognitive load for product teams.

The role is not only infrastructure administration. It treats operations as an engineering problem: define service objectives, automate repetitive work, measure reliability, reduce toil, and design safe paths for change.

## Primary Outcomes

- Reliable services with measurable service objectives
- Safe, repeatable, and observable delivery
- Reduced toil and faster recovery from failure
- Platforms that improve developer productivity without hiding important trade-offs
- Capacity, resilience, and disaster-recovery readiness
- Shared operational practices across product teams

## Capability Areas

| Capability | Specialist behavior | Existing vault anchor |
|---|---|---|
| Service objectives | Defines SLIs, SLOs, and error-budget policies | [[software-engineering-note/02_Software_Architecture/Microservice/05 Observability/054 SLOs & Error Budgets]] |
| Observability | Uses logs, metrics, traces, and events to understand systems | [[software-engineering-note/02_Software_Architecture/Microservice/05 Observability/051 Logging & Monitoring]] |
| Incident response | Coordinates response and turns incidents into learning | [[06_Software_Engineering_Operations/08_Service_Operations_and_Support]] |
| Delivery automation | Builds safe CI/CD, progressive delivery, and rollback | [[06_Software_Engineering_Operations/Software Engineering Operations Overview]] |
| Capacity and resilience | Plans for load, failure, recovery, and growth | [[06_Software_Engineering_Operations/07_Capacity_and_Disaster_Recovery]] |
| Developer platform | Provides reusable paths that balance standardization and autonomy | [[software-engineering-note/02_Software_Architecture/Microservice/07 Deployment/074 GitOps & CI-CD Pipelines]] |

## Typical Progression

```mermaid
flowchart LR
    SENIOR["Senior Software Engineer"] --> OPERATE["Own production operation"]
    OPERATE --> RELIABILITY["Define reliability objectives"]
    RELIABILITY --> PLATFORM["Build reusable platform capabilities"]
    PLATFORM --> ORG["Improve reliability across the organization"]
```

## Signals for Moving Forward

- You are interested in how systems behave after deployment, not only how they are built.
- You use operational data to guide engineering decisions.
- You can balance reliability, delivery speed, and cost.
- You prefer automating recurring operational work over handling it manually.
- Other teams can adopt your platform without needing constant support.

## Evidence to Build

- SLI and SLO definitions
- Error-budget policy
- Observability dashboard
- Incident response playbook
- Post-incident review
- Capacity plan and load-test report
- Disaster-recovery exercise
- Platform service catalog
- Progressive-delivery or rollback design

## Nearby Paths

- [[06_Software_Architect|Software Architect]]: broader system structure and trade-offs
- [[08_Security_Engineer|Security Engineer]]: security risk and controls
- [[03_Staff_Engineer|Staff Engineer]]: cross-team technical influence
- [[11_Engineering_Manager|Engineering Manager]]: people and delivery leadership

## Suggested Future Note Route

1. [[06_Software_Engineering_Operations/Software Engineering Operations Overview]]
2. [[02_Software_Architecture/Microservice/Microservice Overview]]
3. [[06_Software_Engineering_Operations/07_Capacity_and_Disaster_Recovery]]
4. [[06_Software_Engineering_Operations/08_Service_Operations_and_Support]]
5. [[13_Software_Security/Software Security Overview]]
6. [[CyBOK v1 - Overview]]

## Sources

- [Google Cloud: Site Reliability Engineering](https://cloud.google.com/sre)
- [[SWEBOK v4 - Overview]]
- [[CyBOK v1 - Overview]]

## Related

- [[00_Career_Path_Overview]]
- [[02_Senior_Software_Engineer]]
- [[06_Software_Architect]]
- [[08_Security_Engineer]]
