---
tags:
  - devops
  - value-stream
  - org-design
  - adoption
  - software-engineering-operations
---

# 02 — Where to Start

> **Source:** DevOps Handbook Part II (Kim, Humble, Debois, Willis)
> **Purpose:** How to begin a DevOps transformation — organizational design via Conway's Law, team structures, integrating Ops into daily Dev work, and choosing where to start.

## What Is This?

Part II of the DevOps Handbook addresses the critical question: **where do we start a DevOps transformation?** The answer is not purely technical — it begins with _how we organize our teams_. Conway's Law dictates that system architecture mirrors organizational communication structures. To achieve fast flow from Development to Operations with high quality, we must design our organizations so Conway's Law works _for_ us, not against us.

The chapters cover: organizational archetypes (functional vs. market-oriented), designing team boundaries around business outcomes, creating loosely-coupled architectures, embedding Operations capabilities into Dev teams, and integrating Ops into daily Dev rituals (standups, retrospectives, kanban boards).

## Key Concepts

### Conway's Law

> "The organization of the software and the organization of the software team will be congruent; commonly stated as 'if you have four groups working on a compiler, you'll get a 4-pass compiler.'"

How we organize teams has a powerful effect on software architecture and production outcomes. Done poorly, Conway's Law creates tightly-coupled teams all waiting on each other, where even small changes cause potentially global, catastrophic consequences. Done well, small teams can work safely and independently.

### Organizational Archetypes

| Type | Optimizes For | Characteristics | DevOps Fit |
|------|---------------|-----------------|------------|
| **Functional** | Expertise, division of labor, cost reduction | Centralized expertise, tall hierarchies, specialized silos (DBAs, network admins, server admins) | Traditional Ops model; can work with high-trust culture + self-service platforms (Etsy, Google, GitHub) |
| **Matrix** | Combination of functional + market | Dual reporting lines, complex structures | Often achieves neither goal; complicated org structures |
| **Market** | Speed, responding to customer needs | Flat, cross-functional teams, potential redundancies | Primary DevOps pattern; each service team owns feature delivery + production support (Amazon, Netflix) |

### Problems of Functional Orientation ("Optimizing for Cost")

- **Long lead times** — complex activities require tickets across multiple siloed groups, work waits in long queues
- **Low visibility** — workers don't see how their work relates to value stream goals ("I'm just configuring servers because someone told me to")
- **Resource contention** — multiple Dev teams compete for scarce Ops cycles, requiring constant escalation
- **Poor handoffs** — rework, quality issues, bottlenecks, delays as work passes between silos
- **Escalation gridlock** — when every team expedites, all projects move at the same slow crawl

### Market-Oriented Teams ("Optimizing for Speed")

To achieve DevOps outcomes, reduce functional orientation and enable market orientation — many small teams working safely and independently, quickly delivering customer value.

**Characteristics:**
- Cross-functional and independent — each team handles features, testing, security, deployment, and production support
- Responsible from idea conception to retirement
- No manual dependencies on other teams
- Functional skills (Ops, QA, Infosec) are either **embedded** into service teams or provided through **automated self-service platforms**

> **Key insight from Amazon/Netflix:** This model is touted as the primary reason behind their ability to move fast even as they grow.

### Making Functional Orientation Work

It is possible to achieve high-velocity DevOps outcomes with functional orientation if:

- Everyone in the value stream views customer/organizational outcomes as a shared goal
- High-trust culture enables all departments to work together effectively
- All work is transparently prioritized
- Sufficient slack exists for high-priority work to be completed quickly
- Automated self-service platforms build quality into products

> **The Second Toyota Paradox:** Toyota is largely organized in traditional functional departments, yet achieved legendary continuous improvement. "What is decisive is not the form of the organization, but how people act and react."

### Testing, Operations, and Security as Everyone's Job, Every Day

In high-performing organizations, quality, availability, and security aren't the responsibility of individual departments — they are **everyone's job, every day**.

**Jody Mulkey (CTO, Ticketmaster) metaphor:**
> "Ops are the offensive linemen, and Dev are the 'skill' positions... the job of Ops is to help make sure Dev has enough time to properly execute the plays."

**Facebook on-call rotation (2009):** All engineers, managers, and architects rotated through on-call for the services they built. This created visceral feedback on upstream architectural and coding decisions, dramatically improving downstream outcomes.

### Generalists Over Specialists

Siloization occurs when departments "operate more like sovereign states" (Dr. Spear). Countermeasures:

- **Enable every team member to be a generalist** — provide opportunities to learn all skills needed to build and run their systems
- **Rotate people through different roles** regularly
- **Full-stack engineer** — familiar with the entire application stack (code, databases, OS, networking, cloud)
- **Foster a growth mindset** (Carol Dweck) — value ability to acquire new skills, not just existing ones

**CSG International (Scott Prugh):** "By cross-training and growing engineering skills, generalists can do orders of magnitude more work than their specialist counterparts, and it also improves our overall flow of work by removing queues and wait time."

**Disney (Jason Cox):** "We looked for people who had 'curiosity, courage, and candor,' who were not only capable of being generalists but also renegades."

### Fund Services/Products, Not Projects

| Project-Based Funding | Product-Based Funding |
|-----------------------|----------------------|
| Teams assigned, then reassigned after completion | Stable, ongoing service teams with dedicated engineers |
| Developers can't see long-term consequences of decisions | Teams own strategy and roadmap of initiatives |
| Values only earliest stages of software lifecycle | Values organizational and customer outcomes (revenue, adoption rate) |
| Measured by budget, time, scope | Measured by outcome, with minimum output (effort, LOC) |

### Team Boundaries and Conway's Law

**Poor patterns** that create excessive coordination:
- Splitting teams by function (developers and testers in different locations, outsourced testers)
- Splitting teams by architectural layer (application, database)
- Primary communication via tickets and change requests
- Teams separated by contractual boundaries (outsourcing)

**Goal:** Software architecture should enable small teams to be independently productive, sufficiently decoupled so work can be done without excessive or unnecessary communication.

### Loosely-Coupled Architectures

| Tightly-Coupled | Loosely-Coupled |
|-----------------|-----------------|
| Small changes → large-scale failures | Services update independently |
| Constant coordination across teams | No shared databases (or no common schemas) |
| Complex bureaucratic change management | Services interact strictly through APIs |
| Scarce integration test environments (weeks to obtain) | Well-defined interfaces, easier testing |

**Key architectural properties:**

- **Loosely-coupled:** Services update in production independently; decoupled from other services and shared databases
- **Bounded contexts (Eric Evans, Domain-Driven Design):** Developers can understand and update service code without knowing internals of peer services; services interact strictly through APIs; no shared data structures, database schemata, or internal object representations

**Randy Shoup (Google App Engine):** "Organizations with these types of service-oriented architectures, such as Google and Amazon, have incredible flexibility and scalability. These organizations have tens of thousands of developers where small teams can still be incredibly productive."

### The "Two-Pizza Team" Rule

Amazon's rule: a team only as large as can be fed with two pizzas (5–10 people).

**Four effects:**

1. **Shared understanding** — as teams grow, communication required scales combinatorially
2. **Limits growth rate** — by limiting team size, we limit the rate at which their system can evolve, maintaining shared understanding
3. **Decentralizes power, enables autonomy** — each 2PT owns a key business metric (fitness function) and acts autonomously to maximize it
4. **Leadership development** — leading a 2PT provides leadership experience where failure doesn't have catastrophic consequences

**Amazon CTO Werner Vogels:** "Small teams are fast... Each group assigned to a particular business is completely responsible for it.... The team scopes the fix, designs it, builds it, implements it and monitors its ongoing use."

### Three Strategies to Integrate Ops into Dev

| Strategy | Description | When to Use |
|----------|-------------|-------------|
| **Create Self-Service Capabilities** | Centralized platforms and tooling services any Dev team can use (production-like environments, deployment pipelines, automated testing, telemetry dashboards) | Always — the foundation. Available on-demand, no tickets required. |
| **Embed Ops Engineers** | Ops engineers join product teams full-time, attending all Dev rituals, influencing architecture, creating platform capabilities | For new large projects or teams that need deep Ops knowledge |
| **Assign Ops Liaisons** | Designated Ops engineer responsible for understanding product functionality, operability, monitoring needs, infrastructure capacity, launch plans | When embedding isn't possible due to cost/scarcity; supports more teams with fewer people |

**Netflix (Dianne Marsh):** "We don't build, bake, or deploy anything for these teams, nor do we manage their configurations. Instead, we build tools to enable self-service. It's okay for people to be dependent on our tools, but it's important that they don't become dependent on us."

**Disney (Jason Cox):** "In traditional Ops, we merely drove the train that someone else built. But in modern Operations Engineering, we not only help build the train, but also the bridges that the trains roll on."

### Integrate Ops into Dev Rituals

| Ritual | Purpose | Ops Integration |
|--------|---------|-----------------|
| **Daily Standups** | Radiate information: what was done, what's planned, what's blocking | Ops gains awareness of Dev activities; can plan resources, flag risks early, offer help (e.g., tuning database instead of optimizing code) |
| **Retrospectives** | Discuss successes, improvements, experiments | Ops presents deployment/release outcomes and learnings; feedback on downstream impact of Dev decisions; identifies architectural issues |
| **Shared Kanban Boards** | Make all work visible | Put relevant Ops work on the same board as Dev work; see all work required to move code into production; identify where Ops work is blocked |

> **Important:** Reserve dedicated capacity for improvement work (e.g., 20% of cycles, one day/week, one week/month). Without this, team productivity will grind to a halt under technical and process debt.

## Case Studies

### Etsy — Sprouter Elimination (2007–2011)

**Problem:** Sprouter ("stored procedure router") sat between PHP application and Postgres database. Adding business logic required coordination between Dev, DBA, and Sprouter teams — three teams for what should have been one-layer changes. Almost every deployment caused a mini-outage.

**Solution (2009–2011):**
- New CTO Chad Dickerson invested in site stability
- Developers performed their own deployments
- Moved all business logic from database layer to application layer
- Created PHP ORM layer enabling direct database calls
- Two-year migration to eliminate Sprouter entirely

**Result:** Eliminated multi-team coordination, decreased handoffs, increased deployment speed and success, improved site stability, higher developer productivity. Etsy became one of the most admired DevOps organizations ($200M revenue, 2015 IPO).

### Target — API Enablement (2012–2015)

**Problem:** Core data (inventory, pricing, stores) locked in legacy/mainframe systems. Multiple sources of truth between e-commerce and physical stores. New development teams needed 3–6 months for data integrations plus 3–6 months for manual testing. Required 20–30 team interactions per project, heavy project management overhead.

**Solution:**
- API Enablement team led by Heather Mickman
- Owned the entire stack (including Ops)
- Brought in CI/CD tools, Cassandra, Kafka
- "When we asked for permission, we were told no, but we did it anyway, because we knew we needed it."

**Result:**
- 53 new business capabilities in two years (Ship to Store, Gift Registry, Pinterest/Instacart integrations)
- 1.5B → 17B API calls/month across 90 APIs
- 80 deployments per week
- Digital sales: +42% (2014 holidays), +32% (Q2 2015)
- 280K in-store pickup orders on Black Friday 2015

### Big Fish Games — Ops Liaison Model

**Problem:** Centralized Ops supporting many autonomous business units with wildly different technologies. Scarce Ops resources competed for by multiple Dev teams. Unreliable test/integration environments, cumbersome release processes.

**Solution (Paul Farrall):**
- **Business Relationship Manager:** Worked with product management, line-of-business owners, Dev management; intimately familiar with product roadmaps; advocated for product owners inside Operations
- **Dedicated Release Engineer:** Familiar with product's Dev/QA issues; executed needed Ops work; pulled in specialists as needed; helped prioritize self-service tooling

**Result:** Improved Ops relationships, increased code release velocity, embedded Ops expertise without adding headcount.

## Key Takeaways

1. **Conway's Law is destiny** — your org chart shapes your architecture. Design teams for the outcomes you want, not around functional silos.

2. **Optimize for speed, not just cost** — market-oriented, cross-functional teams outperform functionally-siloed organizations for DevOps outcomes.

3. **But functional orgs can work** — if built on high trust, transparent prioritization, sufficient slack, and automated self-service platforms (Etsy, Google, GitHub prove it).

4. **Quality, security, and availability are everyone's job** — not delegated to separate departments. Shared goals and shared pain (e.g., on-call rotations for devs) drive better outcomes.

5. **Invest in generalists** — cross-train engineers across the full stack; it removes queues and wait time while making work more fun.

6. **Fund products, not projects** — stable, ongoing service teams with dedicated engineers produce better long-term outcomes than transient project teams.

7. **Loosely-coupled architectures enable autonomy** — services with bounded contexts, strict API interfaces, and no shared databases let small teams move fast independently.

8. **Keep teams small (two-pizza rule)** — 5–10 people max; limits communication overhead, enables autonomy, and creates leadership opportunities.

9. **Embed or liaison Ops into Dev** — self-service platforms, embedded Ops engineers, or Ops liaisons; all reduce dependency on centralized Operations.

10. **Integrate Ops into Dev rituals** — standups, retrospectives, and shared kanban boards make Ops work visible and create fast feedback loops.

## Related Notes

- [[01_The_Three_Ways]] — Flow, Feedback, Continual Learning, Lean origins, DevOps history
- [[03_Accelerating_Flow]] — Deployment pipeline, CI, CD, automated testing, low-risk releases
- [[04_Amplifying_Feedback]] — Telemetry, monitoring, A/B testing, Dev/Ops collaboration
- [[05_Continual_Learning]] — Blameless postmortems, just culture, org learning, resilience
- [[06_DevSecOps_and_Compliance]] — Security integration, compliance as code, change management
- [[Software Engineering Operations Overview]] — SWEBOK v4 Chapter 06 overview
