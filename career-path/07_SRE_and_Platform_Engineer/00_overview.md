---
title: "SRE and Platform Engineer"
note_type: career-path-overview
career_family: specialist-engineering
level: senior-specialist
entry_from:
  - "[[career-path/02_Senior_Software_Engineer/00_overview|Senior Software Engineer]]"
next_paths:
  - "[[career-path/03_Staff_Engineer/00_overview|Staff Engineer]]"
  - "[[career-path/06_Software_Architect/00_overview|Software Architect]]"
  - "[[career-path/11_Engineering_Manager/00_overview|Engineering Manager]]"
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

| Capability | Specialist behavior | Existing vault anchor | Status | Files |
|---|---|---|---|---|
| [[01_Service_Objectives/00_overview\|Service Objectives]] | Defines SLIs, SLOs, and error-budget policies | [[software-engineering-note/02_Software_Architecture/Microservice/05 Observability/054 SLOs & Error Budgets]] | ✅ Complete | 6 files |
| [[02_Observability/00_overview\|Observability]] | Uses logs, metrics, traces, and events to understand systems | [[software-engineering-note/02_Software_Architecture/Microservice/05 Observability/051 Logging & Monitoring]] | ✅ Complete | 6 files |
| [[03_Incident_Response/00_overview\|Incident Response]] | Coordinates response and turns incidents into learning | [[software-engineering-note/06_Software_Engineering_Operations/08_Service_Operations_and_Support]] | ✅ Complete | 6 files |
| [[04_Delivery_Automation/00_overview\|Delivery Automation]] | Builds safe CI/CD, progressive delivery, and rollback | [[software-engineering-note/06_Software_Engineering_Operations/Software Engineering Operations Overview]] | ✅ Complete | 6 files |
| [[05_Capacity_and_Resilience/00_overview\|Capacity and Resilience]] | Plans for load, failure, recovery, and growth | [[software-engineering-note/06_Software_Engineering_Operations/07_Capacity_and_Disaster_Recovery]] | ✅ Complete | 6 files |
| [[06_Developer_Platform/00_overview\|Developer Platform]] | Provides reusable paths that balance standardization and autonomy | [[software-engineering-note/02_Software_Architecture/Microservice/07 Deployment/074 GitOps & CI-CD Pipelines]] | ✅ Complete | 6 files |

**Total:** 36 files across 6 capability areas (1 overview + 5 topics each)

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

- [[career-path/06_Software_Architect/00_overview|Software Architect]]: broader system structure and trade-offs
- [[career-path/08_Security_Engineer/00_overview|Security Engineer]]: security risk and controls
- [[career-path/03_Staff_Engineer/00_overview|Staff Engineer]]: cross-team technical influence
- [[career-path/11_Engineering_Manager/00_overview|Engineering Manager]]: people and delivery leadership

## Suggested Future Note Route

1. [[software-engineering-note/06_Software_Engineering_Operations/Software Engineering Operations Overview]]
2. [[software-engineering-note/02_Software_Architecture/Microservice/Microservice Overview]]
3. [[software-engineering-note/06_Software_Engineering_Operations/07_Capacity_and_Disaster_Recovery]]
4. [[software-engineering-note/06_Software_Engineering_Operations/08_Service_Operations_and_Support]]
5. [[software-engineering-note/13_Software_Security/Software Security Overview]]
6. [[CyBOK v1 - Overview]]

## Sources

- [Google Cloud: Site Reliability Engineering](https://cloud.google.com/sre)
- [[SWEBOK v4 - Overview]]
- [[CyBOK v1 - Overview]]

## Related

- [[00_Career_Path_Overview]]
- [[career-path/02_Senior_Software_Engineer/00_overview|Senior Software Engineer]]
- [[career-path/06_Software_Architect/00_overview|Software Architect]]
- [[career-path/08_Security_Engineer/00_overview|Security Engineer]]
