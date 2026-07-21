---
title: Continual Learning
tags:
  - devops
  - learning
  - postmortems
  - just-culture
  - resilience
  - software-engineering-operations
source: "DevOps Handbook Part V: The Third Way — The Technical Practices of Learning"
date: 2026-07-21
---

# 05 Continual Learning

## Overview

Part V of the DevOps Handbook presents **The Third Way: The Technical Practices of Learning**. While Part III (Flow) enables fast delivery and Part IV (Feedback) amplifies signals from the system, Part V creates a culture where learning happens as quickly, frequently, and cheaply as possible — from both successes and failures. The result is higher resilience and an ever-growing collective knowledge of how the system actually works.

> "The only sustainable competitive advantage is an organization's ability to learn faster than the competition." — Peter Senge

---

## Chapter 19: Enable and Inject Learning into Daily Work

In complex systems, it is impossible to predict all outcomes. Organizations must become skilled at **self-diagnostics and self-improvement** — detecting problems, solving them, and multiplying the effect by making solutions available organization-wide. This creates what Dr. Steven Spear calls a **resilient organization**: one that can heal itself.

### Establish a Just, Learning Culture

**Dr. Sidney Dekker** codified the key elements of safety culture and coined the term **just culture**:

> "When responses to incidents and accidents are seen as unjust, it can impede safety investigations, promoting fear rather than mindfulness in people who do safety-critical work, making organizations more bureaucratic rather than more careful, and cultivating professional secrecy, evasion, and self-protection."

#### The Bad Apple Theory (and why it's wrong)

Traditional management assumes errors can be eliminated by removing the people who cause them. Dekker argues this is invalid:

> "Human error is not our cause of troubles; instead, human error is a consequence of the design of the tools that we gave them."

Rather than **naming, blaming, and shaming**, the goal should always be to maximize opportunities for organizational learning.

#### Key Principles of Just Culture

- When engineers feel safe giving details about mistakes, they are willing to be held accountable AND enthusiastic about helping others avoid the same error
- Punishing engineers dis-incentivizes providing necessary details — guaranteeing the failure will recur
- Balance the needs for **safety and accountability**
- View mistakes, errors, slips, and lapses with a **perspective of learning** (John Allspaw, CTO of Etsy)

### Schedule Blameless Post-Mortem Meetings

**Blameless post-mortems** (term coined by John Allspaw) examine mistakes by focusing on **situational aspects** of a failure's mechanism and the decision-making process of individuals proximate to the failure.

#### When and Who

- Schedule **as soon as possible** after the accident — before memories fade, but after the problem is resolved
- Required attendees:
  - People involved in decisions that contributed to the problem
  - People who identified, responded to, and diagnosed the problem
  - People affected by the problem
  - Anyone else interested

#### Meeting Structure

1. **Construct a timeline** of relevant events — actions taken, effects observed (backed by metrics, not subjective narratives), investigation paths, and resolutions considered
2. **Gather multiple perspectives** without punishing people for mistakes
3. **Disallow counterfactual statements** — explicitly ban "would have" and "could have" (these frame problems as "system as imagined" not "system as it actually is")
4. **Enable those who made mistakes to become the experts** who educate the rest of the organization
5. **Brainstorm countermeasures** — prioritize, assign owners, and set timelines

#### Countermeasures Must Be Real

Dan Milstein (HubSpot) begins every post-mortem: "We're trying to prepare for a future where we're as stupid as we are today."

Countermeasures cannot be "be more careful" — they must be concrete:
- New automated tests in deployment pipeline
- Additional production telemetry
- Categories of changes requiring additional peer review
- Game Day rehearsals for that failure category

### Publish Post-Mortems Widely

- Announce meeting notes and artifacts (timelines, chat logs) centrally
- Consider prohibiting incident closure until post-mortem is complete
- Transforms **local learnings into global improvements**
- At Google: all post-mortem documents are searchable — "when any group has an incident that sounds similar to something that happened before, these post-mortem documents are among the first documents being read and studied" (Randy Shoup)

#### Etsy's Morgue Tool

Etsy built **Morgue** to make post-mortem documentation easier (replacing wiki pages), recording:
- Scheduled vs. unscheduled incident
- Post-mortem owner
- Relevant IRC chat logs
- JIRA tickets for corrective actions with due dates
- Links to customer forum posts

Result: significantly more post-mortems recorded, especially for lower-severity (P2-P4) incidents.

### Decrease Incident Tolerances to Find Ever-Weaker Failure Signals

As organizations learn to solve problems efficiently, they must **lower the threshold** of what constitutes a problem.

**Alcoa example (Paul O'Neill):** When workplace accidents became rare, he began tracking near-misses — then discovered that safety problems reflected process ignorance that also manifested as quality, timeliness, and yield issues.

**NASA Columbia disaster (2003):** Foam shedding was treated as a maintenance nuisance — not an experimental signal. The organization applied a **standardized model** (routine, compliance-driven) instead of an **experimental model** (every piece of information debated and evaluated). The absence of continuous learning had dire consequences.

> Technology work should be approached as a fundamentally **experimental endeavor**, not routine application of past practice.

### Redefine Failure and Encourage Calculated Risk-Taking

Roy Rapoport (Netflix): "High-performing DevOps organizations will fail and make mistakes more often. Not only is this okay, it's what organizations need!"

- High performers deploy 30× more frequently with half the change failure rate — they experience **more total failures**
- A Netflix engineer who caused two major outages in 18 months also "moved the state of operations and automation forward by light-years"
- Leaders must reinforce that everyone should feel **comfortable with and responsible for** surfacing and learning from failures

### Inject Production Failures to Enable Resilience and Learning

> "Like building crumple zones into cars to absorb impacts and keep passengers safe, you can decide what features of the system are indispensable and build in failure modes that keep cracks away from those features. If you do not design your failure modes, then you will get whatever unpredictable — and usually dangerous — ones happen to emerge." — Michael Nygard

#### Netflix and Chaos Monkey

- **Chaos Monkey:** Constantly and randomly kills production servers
- Goal: make "engineering teams used to a constant level of failure in the cloud" so services "automatically recover without any manual intervention"
- **2011 AWS US-EAST outage:** Netflix was unaffected while Reddit, Quora went down — due to architectural decisions made in 2009 to be "cloud native"
- **2014 Great Amazon Reboot (10% of EC2 servers):** 218 of 2700+ Cassandra nodes rebooted, 22 didn't come back — Netflix experienced **zero downtime**

### Institute Game Days to Rehearse Failures

**Game Days** (term popularized by Jesse Robbins, Amazon's "Master of Disaster"): **resilience engineering** exercises involving large-scale fault injection across critical systems.

> "A service is not really tested until we break it in production." — Jesse Robbins

#### Process

1. Schedule a catastrophic event (e.g., simulated data center destruction)
2. Give teams time to prepare — eliminate single points of failure, create monitoring and failover procedures
3. Execute the outage — at Amazon they would "literally power off a facility — without notice"
4. Expose latent defects: monitoring systems that get turned off as part of the failure, unknown single points of failure
5. Progressively increase intensity and complexity

#### Google's DiRT (Disaster Recovery Program)

Kripa Krishnan led the program for 7+ years, simulating:
- Earthquakes disconnecting the entire Mountain View campus
- Complete data center power loss
- "Alien attacks" on cities where engineers lived

**Learnings included:**
- Failover to engineer workstations didn't work when connectivity was lost
- Conference call bridges had insufficient capacity
- No one knew emergency diesel purchasing procedures — someone used a personal credit card for $50,000 of fuel

> "An often-overlooked area of testing is business process and communications. Systems and processes are highly intertwined." — Kripa Krishnan

---

## Chapter 20: Convert Local Discoveries into Global Improvements

New learnings and improvements discovered locally must be captured and shared globally — multiplying the effect of knowledge and elevating the state of practice across the entire organization.

### Use Chat Rooms and Chat Bots to Automate and Capture Organizational Knowledge

**ChatOps at GitHub (Hubot):** Put automation tools into the middle of chat room conversations.

Benefits:
- **Everyone sees everything** happening
- New engineers on day one see how daily work is performed
- People more apt to ask for help when they see others helping
- **Rapid organizational learning** enabled and accumulated
- Chat rooms are **public by default** (vs. email which is private by default)
- At GitHub (all remote): "The chat room was the water cooler"

Hubot could: check service health, deploy to production, mute alerts, pull smoke test logs, take servers out of rotation, revert to master — all from chat, including from a phone.

### Automate Standardized Processes in Software for Re-Use

Instead of codifying standards in Word documents that engineers never find or implement:

> "The actual compliance of an organization is in direct proportion to the degree to which its policies are expressed as code." — Justin Arbuckle, GE Capital

**ArchOps at GE Capital:** Design standards encoded into automated blueprints — "enabled our engineers to be builders, not bricklayers. By putting our design standards into automated blueprints that were able to be used easily by anyone, we achieved consistency as a byproduct."

### Create a Single, Shared Source Code Repository

**Google (2015):** Single shared repository — over 1 billion files, 2 billion lines of code, used by 25,000+ engineers across all Google properties.

> "The most powerful mechanism for preventing failures at Google is the single code repository." — Randy Shoup

Benefits:
- Everything built from source, statically linked — single version of each library
- Write a tool once, usable for all projects
- 100% accurate knowledge of who depends on a library
- Safe refactoring with full impact analysis
- Library owners responsible for migrating all projects to new versions

**Tom Limoncelli (former Google SRE):** "I can't express in words how much of a competitive advantage this is for Google."

Artifacts to include in the shared repo:
- Configuration standards (Chef, Puppet)
- Deployment tools
- Testing standards and tools (including security)
- Deployment pipeline tools
- Monitoring and analysis tools
- Tutorials and standards

### Spread Knowledge Through Automated Tests as Documentation

- TDD practices turn test suites into **living, up-to-date specifications**
- Any engineer can look at test suites to find working examples of API usage
- Each library has a single owner/team — knowledge and expertise reside there
- Only one version used in production — leverages the best collective knowledge
- Create discussion groups/chat rooms for each library

### Design for Operations Through Codified Non-Functional Requirements

As Development participates in production incident resolution, applications become better designed for Operations. Codify these non-functional requirements:
- Sufficient production telemetry in applications and environments
- Ability to accurately track dependencies
- Services that are resilient and degrade gracefully
- Forward and backward compatibility between versions
- Ability to archive data to manage production data set size
- Easy search and understanding of log messages across services
- Request tracing from users through multiple services
- Centralized runtime configuration (feature flags)

### Build Reusable Operations User Stories

For Ops work that cannot be fully automated:
- Standardize, automate as much as possible, document clearly
- Define handoffs to reduce lead times and errors
- Create **"Ops user stories"** (e.g., deployment, capacity, security) that appear alongside Development work in backlogs
- Know for all recurring work: what, who, steps required, and historical completion times

### Ensure Technology Choices Help Achieve Organizational Goals

**"Buoys, not boundaries"** (Ralph Loura, CIO of HP):
> "Instead of drawing hard boundaries that everyone has to stay within, we put buoys that indicate deep areas of the channel where you're safe and supported. You can go past the buoys as long as you follow the organizational principles."

Identify technologies that:
- Impede or slow down the flow of work
- Disproportionately create high levels of unplanned work
- Disproportionately create large numbers of support requests
- Are most inconsistent with desired architectural outcomes

**Etsy example:** Deliberately reduced supported technologies in production — migrated entirely to PHP and MySQL, retired lighttpd, Postgres, MongoDB, Scala, CoffeeScript, Python, and others. Goal: both Dev and Ops could understand the full stack.

---

## Chapter 21: Reserve Time to Create Organizational Learning and Improvement

### Improvement Blitzes (Kaizen Blitz)

From Toyota Production System: a dedicated, concentrated period (days to a week) to address a particular issue. Cross-functional teams from Dev, Ops, and Infosec self-organize to fix problems — **no feature work allowed**.

Other terms: spring/fall cleanings, ticket queue inversion weeks, hack days, hackathons, 20% innovation time.

> "No wonder then that spiders repair rips and tears in the web as they occur, not waiting for the failures to accumulate." — Dr. Steven Spear

The complex system is like a spider web — command-and-control cannot direct workers to fix each broken strand. The organization must create norms where everyone continually finds and fixes broken strands as part of daily work.

#### Facebook's HipHop Compiler

During a hack day, Haiping Zhao began converting PHP to compilable C++. Over two years, the HipHop compiler enabled Facebook to handle **6× higher production loads**. It was described as a "Hail Mary pass that worked out" — without it, Facebook would have needed more machines than they could procure in time.

#### Target DevOps Dojo

Ross Clanton's **30-Day Challenges**: internal teams work with dedicated coaches for a month to solve a specific problem in two-day sprints.

- "Not uncommon for teams to achieve in days what would usually take them three to six months"
- 200+ learners, 14 challenges completed
- Also offers: Flash Builds (1-3 day MVP events), Open Labs (bi-weekly coaching)

### Enable Everyone to Teach and Learn

**Nationwide Insurance — Teaching Thursday:** Two hours every week where 5,000 technology associates are expected to either teach or learn. Topics: technology, process improvement, career management.

**Cross-training approach:**
- Ops and Test engineers learn Development skills (version control, automated testing, deployment pipelines, configuration management)
- Joint code reviews include both Dev and Ops
- Work together on small problems to build skills organically

### Share Experiences from DevOps Conferences

- **DevOpsDays:** Vibrant self-organized conference series — free or nearly free
- **DevOps Enterprise Summit (2014+):** Experience reports from technology leaders in large, complex organizations

**Internal conferences:**
- **Nationwide TechCon (2011+):** Internal technology conference — "create a better way to teach ourselves, with Nationwide context"
- **Capital One (2015):** 13 learning tracks, 52 sessions, 1,200+ internal attendees, expo hall with 28 internal team booths — "no vendors, keep focus on Capital One goals"
- **Target:** 6+ internal DevOpsDays events since 2014, 975+ followers in internal technology community

### Create Internal Consulting and Coaches

**Google Testing Grouplet (2005+):**
- Formed during 20% innovation time — no budget, no formal authority
- **Testing on the Toilet (TotT):** Weekly newsletter in every bathroom in nearly every Google office worldwide
- **Test Certified (TC):** 3-level roadmap (baseline metrics → coverage goals → long-term goals)
- **Test Mercenaries:** Full-time internal coaches working hands-on with teams
- **Fixits:** Company-wide one-day intensive sprints of code reform and tool adoption — the last involved 100+ volunteers across 20+ offices in 13 countries

---

## Netflix: The Exemplar of Continual Learning

Netflix demonstrates all Part V principles in action:

1. **Architectural resilience:** Re-architected from monolithic J2EE to cloud-native (2009), designed to survive entire AWS availability zone failures
2. **Chaos Monkey:** Continuously injects failures into production
3. **Graceful degradation:** During CPU spikes, serves cached/un-personalized content instead of failing
4. **Aggressive timeouts:** Failing components don't bring down the entire system
5. **Just culture:** Engineer responsible for two major outages in 18 months was retained because their automation work advanced operations "by light-years"

Result: survived the 2011 AWS US-EAST outage when most other customers went down, and the 2014 Great Amazon Reboot with zero downtime.

---

## Conclusion to Part V

The Third Way practices create:
- A **just culture** where people feel safe surfacing problems
- **Blameless post-mortems** that convert failures into organizational learning
- **Resilience engineering** through Game Days and fault injection
- Mechanisms to convert **local discoveries into global improvements**
- Dedicated time for **improvement and learning**

The goal is not just to learn faster than the competition, but to create a safer, more resilient work culture where people are excited to contribute and achieve their highest potential.

---

## Key Concepts Summary

| Concept | Description |
|---------|-------------|
| **Just Culture** | Response to incidents balances safety and accountability; focuses on system design, not blaming individuals |
| **Blameless Post-Mortem** | Structured incident review focusing on situational factors and decision-making context, with concrete countermeasures |
| **Bad Apple Theory** | The invalid notion that errors are caused by bad people rather than bad system design |
| **Weak Failure Signals** | Amplifying near-misses and minor issues to prevent catastrophic failures |
| **Chaos Monkey** | Netflix tool that randomly kills production servers to ensure resilience |
| **Game Day** | Scheduled disaster rehearsals that simulate catastrophic failures |
| **ChatOps** | Automating operations through chat room commands — makes work visible and searchable |
| **Improvement Blitz / Kaizen Blitz** | Dedicated time for cross-functional teams to fix problems — no feature work |
| **Shared Source Repository** | Organization-wide single repo enabling global propagation of improvements |
| **Non-Functional Requirements** | Codified operations requirements (telemetry, graceful degradation, compatibility) |
| **Resilience Engineering** | Designing failure modes and regularly testing them through fault injection |


## Related

- [[Software Engineering Operations Overview]] — All operations topics
- [[01_The_Three_Ways]] — The Three Ways framework
- [[03_Accelerating_Flow]] — Flow practices
- [[06_DevSecOps_and_Compliance]] — Security integration
