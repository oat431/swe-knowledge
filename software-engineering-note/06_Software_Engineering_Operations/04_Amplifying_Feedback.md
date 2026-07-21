---
tags:
  - devops
  - feedback
  - monitoring
  - telemetry
  - ab-testing
  - software-engineering-operations
---

# Amplifying Feedback — The Second Way

> **Source:** The DevOps Handbook (Kim, Humble, Debois, Willis) — Part IV
> **Chapters:** 17 (A/B Testing & Hypothesis-Driven Development), 18 (Review & Coordination Processes)
> **The Second Way:** Create right-to-left feedback loops from production back to development, enabling continuous improvement.

---

## Part IV Overview

The Second Way of DevOps is about creating fast, amplified feedback from every stage of the delivery pipeline — especially from production — back to Development. Feedback loops enable:

- **Detecting problems** as they occur in production through telemetry
- **Validating features** achieve their intended business outcomes via user research
- **Improving quality** through integrated peer review instead of external approval boards
- **Speeding up learning** so organizations out-experiment their competition

---

## Chapter 17: Hypothesis-Driven Development & A/B Testing

### Why We Need to Validate Features Before Building

Jez Humble's observation: *"The most inefficient way to test a business model or product idea is to build the complete product to see whether the predicted demand actually exists."*

Before building any feature, rigorously ask: **"Should we build it, and why?"** Then perform the cheapest, fastest experiments possible.

### The Intuit TurboTax Story

| Metric | Before (2009) | After (2010+) |
|---|---|---|
| Experiments per tax season | ~7 | **165** (in 3 months) |
| Website conversion rate | Baseline | **+50%** |
| Culture | Boss's vote decides | Real experiments with real users |

Scott Cook (founder): *"Instead of focusing on the boss's vote...the emphasis is on getting real people to really behave in real experiments, and basing your decisions on that."*

Key insight: **TurboTax ran experiments during peak traffic season** (not during change freezes). Because deployments were fast and safe, online experimentation became a low-risk activity even during highest-revenue periods. Had they waited until after tax season, they'd have lost customers to competition.

### A Brief History of A/B Testing

A/B testing originated in **direct response marketing** — sending thousands of mailers and measuring which offers generated the highest conversion rates. Each experiment cost tens of thousands of dollars and took weeks.

| Era | Method | Cost per Experiment | Cycle Time |
|---|---|---|---|
| Direct mail marketing | Print + postal mail | $10,000+ | Weeks/months |
| Modern web A/B testing | Software toggles | Near-zero | Hours/days |

Well-documented applications include campaign fundraising, internet marketing, Lean Startup methodology, and even British government tax collection letters.

### How A/B Testing Works

Visitors are randomly assigned to either:
- **Control (A):** Existing version
- **Treatment (B):** Modified version

Statistical analysis determines whether there's a **significant causal difference** between outcomes of the two cohorts.

Examples of what can be tested:
- Button text, color, or placement
- Page layout changes
- Performance degradation (artificial delay to measure revenue impact)
- Feature presence/absence

**Multivariate testing** extends this to multiple variables simultaneously to detect interaction effects.

### The Shocking Data on Feature Success

> Ronny Kohavi (Microsoft): After evaluating well-designed experiments intended to improve a key metric, **only ~1/3 were successful**. Two-thirds of features either had negligible impact or made things worse.

Implications:
- **2/3 of features we build deliver zero or negative value**
- They make the codebase more complex, increasing maintenance costs
- They represent massive opportunity cost (effort that could have gone to value-delivering features)
- Jez Humble: *"The organization and customers would have been better off giving the entire team a vacation."*

### Integrating A/B Testing into Releases

Requirements for fast, iterative A/B testing:
1. **On-demand production deployments** (fast and safe)
2. **Feature toggles** to control which users see the treatment
3. **Production telemetry** at all levels of the application stack
4. **Multiple versions** served simultaneously to customer segments

| Tool/Framework | Capabilities |
|---|---|
| Etsy Feature API (open-source) | A/B testing, online ramp-ups, throttling exposure |
| Optimizely | Commercial experimentation platform |
| Google Analytics | Content experiments |

Etsy's Lacy Rhoades: *"A/B testing allows us to...say a feature is worth working on as soon as it's underway."*

### Hypothesis-Driven Feature Planning

Barry O'Reilly's hypothesis framework:

> **We Believe** that *[doing this change]*
> **Will Result in** *[this outcome]*
> **We Will Have Confidence To Proceed When** *[we see this measurable signal]*

Example:
> We Believe that **increasing the size of hotel images on the booking page**
> Will Result in **improved customer engagement and conversion**
> We Will Have Confidence To Proceed When we see a **5% increase in customers who review hotel images who then proceed to book in 48 hours.**

This forces product owners to:
- Treat each feature as a **hypothesis**
- Define **measurable success criteria** before building
- Modify the roadmap when features don't deliver expected outcomes
- Break work into small, independently testable units

### Case Study: Yahoo! Answers (2010)

| Before | After (12 months) |
|---|---|
| 1 release every 6 weeks | Multiple releases per week |
| 140M monthly visitors, flat growth | **+72% monthly visits** |
| Declining engagement | **3× user engagement** |
| Stagnant revenue | **Doubled revenue** |

Top metrics they optimized:
1. **Time to first answer** — how quickly a question got its first answer
2. **Time to best answer** — how quickly the community selected the best answer
3. **Upvotes per answer** — community validation signals
4. **Answers/week/person** — user participation rate
5. **Second search rate** — how often visitors had to search again (lower = better)

Jim Stoneham (GM): *"When you move at that speed, and are looking at the numbers and results daily, your investment level radically changes. We transformed from a team of employees to a team of owners."*

---

## Chapter 18: Review & Coordination Processes

### The Goal

Shift from **periodic external approvals** (change advisory boards, CAB) to **integrated peer review** performed continually as part of daily work — ensuring Dev, Ops, and InfoSec continuously collaborate so changes operate reliably, securely, and safely.

### GitHub Flow: The Pull Request Model

| Step | Action |
|---|---|
| 1 | Create a descriptively named branch off `master` |
| 2 | Commit locally, regularly push to the same branch on the server |
| 3 | **Open a pull request** when feedback is needed or branch is ready |
| 4 | Get reviews and approvals, then **merge into master** |
| 5 | Once merged and pushed to master, **deploy to production** |

GitHub's 2012 results using this model:
- **12,602 total deployments**
- Busiest day: **563 builds, 175 production deployments**
- All enabled by the pull request process, not external approvals

### The Knight Capital Failure — Two Narratives

The 2012 Knight Capital disaster: a 15-minute deployment error caused **$440M in trading losses**, forcing the company to be sold over a weekend.

When such incidents occur, two counterfactual narratives emerge:

| Narrative | Proposed Fix | Hidden Risk |
|---|---|---|
| **Change control failure** | Add more approvals, more questions, more lead time | Increases friction, batch sizes, and lead times — *worsens* outcomes |
| **Testing failure** | Do more testing (often manual) | Slower releases, larger batches — *worsens* outcomes |

John Allspaw, Jez Humble, and Gene Kim concluded that in **low-trust, command-and-control cultures**, these countermeasures often *increase* the likelihood of problems recurring, with potentially worse outcomes.

### Why Over-Controlling Changes Backfires

Traditional change control reactions:
1. Add more questions to change request forms
2. Require more authorizations (VP → CIO, more stakeholders)
3. Require more lead time for approvals

These controls:
- **Multiply steps and approvals** — more friction
- **Increase batch sizes** — more risk per deployment
- **Increase deployment lead times** — slower feedback
- **Reduce likelihood of success** for both Dev and Ops

> Toyota Production System principle: *"People closest to a problem typically know the most about it."*

The further the distance between the **change implementer** and the **change authorizer**, the worse the outcome — especially in complex, dynamic systems.

### Evidence: Peer Review vs. Change Approvals

| Approach | IT Performance (Stability) | IT Performance (Throughput) |
|---|---|---|
| **Peer review** | Better MTTR, lower change fail rate | Faster lead times, higher frequency |
| **External change approvals** | Worse on both dimensions | Worse on both dimensions |

Source: Puppet Labs 2014 State of DevOps Report — the more organizations rely on change approvals, the worse their IT performance.

Why CAB review fails in complex systems:
- A 100-word description can't predict whether a change will succeed
- Scrutinizing thousands of lines of code reveals few new insights
- Even engineers working daily in the codebase are surprised by side effects

### Coordination and Scheduling of Changes

| Architecture Type | Coordination Need | Approach |
|---|---|---|
| **Loosely coupled (service-oriented)** | Minimal — local changes don't disrupt globally | Chat rooms to announce changes, proactively find collisions |
| **Tightly coupled** | Higher — changes need sequencing | Representatives from teams meet to schedule and sequence changes (not authorize them) |
| **Global infrastructure** (core switches, etc.) | Always high risk | Technical countermeasures: redundancy, failover, comprehensive testing, simulation |

### Peer Review of Changes — Guidelines

1. **Everyone** must have someone review their changes before committing to trunk
2. **Everyone** should monitor the commit stream for potential conflicts
3. **Define** which changes are high-risk and require SME review (database, security-sensitive modules)
4. **If a change is too large to reason about easily**, split it into multiple smaller changes

> Randy Shoup: *"There is a non-linear relationship between the size of the change and the potential risk — a 100-line change is more than 10× riskier than a 10-line change."*

> Giray Özil: *"Ask a programmer to review 10 lines of code, he'll find 10 issues. Ask him to do 500 lines, and he'll say it looks good."*

### Forms of Code Review

| Form | Description |
|---|---|
| **Pair programming** | Two engineers at one workstation — driver + navigator |
| **Over-the-shoulder** | Author walks reviewer through the code |
| **Email pass-around** | SCM system auto-emails code to reviewers after check-in |
| **Tool-assisted review** | Gerrit, GitHub pull requests, Atlassian Crucible/Stash |

### Case Study: Code Reviews at Google (2010)

| Metric | Value |
|---|---|
| Developers on single trunk | **13,000+** |
| Code commits per week | **5,500+** |
| Changes checked in per minute | **20+** |
| Codebase changed per month | **50%** |
| Production deployments per week | **Hundreds** |

Mandatory code reviews covered:
- **Code readability** — enforced style guide per language
- **Ownership assignments** — maintain consistency in code sub-trees
- **Code transparency** — enable contributions across teams

Key finding: **Larger changes → longer review lead times.** The most complex/risky changes required the most deliberation.

Randy Shoup's lesson: Submitted a 3,000-line change that took a reviewer days to process. The reviewer said, *"Please don't do that to me again."* That's when he learned to make code reviews part of daily work.

### The Danger of More Manual Testing & Change Freezes

When testing failures occur, the instinct is "do more testing." But:

- **Manual testing is slower** than automated testing
- "Additional testing" → longer test cycles → less frequent deployments → **larger batch sizes**
- Larger batch sizes → **lower change success rates, more incidents, higher MTTR**

Instead: integrate testing into daily work, deploy continuously in smaller batches, build quality in.

### Pair Programming

| Aspect | Finding |
|---|---|
| Productivity | **~15% slower** than two independent programmers |
| Defect rate | Error-free code increased from **70% → 85%** |
| Design quality | Pairs consider more alternatives, produce simpler, more maintainable designs |
| Job satisfaction | **96%** preferred pair programming over programming alone |

Source: Dr. Laurie Williams study (2001)

Jeff Atwood (Stack Exchange): *"Pair programming is code review on steroids. Its gripping immediacy — impossible to ignore the reviewer when he or she is sitting right next to you."*

### Case Study: Pivotal Labs (2011)

**Problem:** Gerrit code review process required two senior engineer "+1" approvals. Result: **one-week delays** for code reviews, senior engineers were bottlenecks, junior developers constantly merging others' changes while waiting.

**Solution:** Dismantled Gerrit process, required pair programming.

**Result:** Code review time reduced from **weeks → hours**.

Elisabeth Hendrickson: Code reviews work when the culture values reviewing code as highly as writing it. When that culture isn't in place, pair programming serves as an interim forcing function.

### Evaluating Pull Request Effectiveness

Ryan Tomayko's criteria for pull request quality:

| Bad Pull Request | Good Pull Request |
|---|---|
| Vague description ("Fixing issue #3616") | **Why** the change is being made |
| No specific reviewers @mentioned | **How** the change was made |
| No context for the reader | **Identified risks** and countermeasures |
| No discussion | **Active discussion** — additional risks, better approaches |
| No follow-up | If something bad happens, it's added to the PR with a link to the issue |

Ultimate example of a great PR: database migration with pages of risk discussion, followed by an outage, followed by a detailed post-mortem linked back to the PR, with specific countermeasures proposed.

### Fearlessly Cut Bureaucratic Processes

> Adrian Cockcroft: *"A great metric to publish widely is how many meetings and work tickets are mandatory to perform a release — the goal is to relentlessly reduce the effort required for engineers to perform work and deliver it to the customer."*

Real-world examples of dismantling bureaucracy:

| Organization | Process Removed | Method |
|---|---|---|
| **Capital One** ("Got Goo?") | Tools, processes, approvals impeding work | Dedicated team removing obstacles |
| **Disney** ("Join The Rebellion") | Toil and obstacles from daily work | DevOps Enterprise Summit initiative |
| **Target** (TEAP-LARB) | Technology approval board requiring months | "Five Why's" traced it to a forgotten disaster; Development took ops responsibility instead |

Target's Heather Mickman: *"No one knew why TEAP-LARB existed, outside of a vague notion that we needed some sort of governance process...no one could remember exactly what that disaster was."* Cassandra was successfully introduced, the TEAP-LARB process was dismantled, and Mickman received a Lifetime Achievement Award for removing barriers.

### Trust Over Approvals — The Culture Shift

John Allspaw's story about a junior engineer asking if she could deploy a small HTML change:

> *"I don't know, is it? Did you have someone review your change? Do you know who the best person to ask is? Did you do everything you absolutely could to assure yourself that this change operates in production as designed? If you did, then don't ask me — just make the change!"*

Creating the conditions where **change implementers fully own the quality of their changes** is essential to building a high-trust, generative culture.

---

## Part IV Conclusion: Key Takeaways

| Principle | Practice |
|---|---|
| **Out-experiment the competition** | Hypothesis-driven development, A/B testing, fast release cycles |
| **Validate before building** | Customer acquisition funnels, user research, pretotypes before features |
| **Fast + safe deploys enable experimentation** | Feature toggles, production telemetry, small batch sizes |
| **Peer review > external approvals** | Pull requests, pair programming, tool-assisted code review |
| **People closest to the work know it best** | Reduce distance between implementer and authorizer |
| **Cut bureaucratic processes** | "Five Why's" to find obsolete governance; take operational responsibility |
| **Small batches = lower risk** | Non-linear relationship between change size and risk |
| **Build a high-trust, just culture** | Blameless approach to failures; ownership without fear |

---

## Related Notes

- [[01_The_Three_Ways]] — Flow, Feedback, Continual Learning foundations
- [[03_Accelerating_Flow]] — Deployment pipeline, CI/CD, automated testing
- [[05_Continual_Learning]] — Blameless postmortems, chaos engineering, just culture
- [[Software Engineering Operations Overview]] — Operations KA overview

---

## Sources

- Kim, G., Humble, J., Debois, P., & Willis, J. (2016). *The DevOps Handbook: How to Create World-Class Agility, Reliability, & Security in Technology Organizations*. IT Revolution Press. Part IV, Chapters 17–18.
