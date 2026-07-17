---
tags: [user-research, ux, interviews, personas, journey-maps, ux-ui-design]
---

# 00 — User Research Methods

> *Source: Nielsen Norman Group, IDEO Design Thinking, Lean UX principles*

## Purpose

User research is the foundation of good design. It answers: **Who are our users? What do they need? How do they behave?** Without research, you're designing based on assumptions — and assumptions are often wrong.

## Research Methods

### Qualitative Methods (Why & How)

#### User Interviews
- **What:** One-on-one conversations with users about their needs, behaviors, and frustrations
- **When:** Early discovery, post-launch feedback
- **How:** 30–60 minutes, open-ended questions, record with permission
- **Sample size:** 5–8 users per segment (diminishing returns after 5)
- **Key questions:**
  - "Tell me about the last time you [task]..."
  - "What was the hardest part?"
  - "What would have made it easier?"

#### Contextual Inquiry
- **What:** Observing users in their natural environment while they perform tasks
- **When:** Understanding real workflows, not just stated preferences
- **How:** Shadow users, take notes, ask questions in context
- **Key insight:** What people *do* often differs from what they *say they do*

#### Card Sorting
- **What:** Users organize topics/content into groups that make sense to them
- **When:** Designing navigation, information architecture
- **How:** Open sort (users create categories) or closed sort (predefined categories)
- **Tools:** OptimalSort, Miro, sticky notes

#### Diary Studies
- **What:** Users record their experiences over time (days/weeks)
- **When:** Understanding long-term behavior, habits, frustrations
- **How:** Users log entries daily with prompts
- **Key insight:** Reveals behaviors that don't surface in one-time interviews

### Quantitative Methods (How Many & How Much)

#### Surveys & Questionnaires
- **What:** Structured questions to a large audience
- **When:** Validating hypotheses, measuring satisfaction, gathering demographics
- **How:** Keep short (< 10 questions), mix multiple-choice and open-ended
- **Sample size:** 100+ for statistical significance
- **Tools:** Google Forms, Typeform, SurveyMonkey

#### Analytics
- **What:** Data from actual user behavior (clicks, paths, conversions)
- **When:** Continuous, post-launch
- **Key metrics:**
  - **Bounce rate** — % leaving after one page
  - **Conversion rate** — % completing desired action
  - **Time on task** — How long users take
  - **Error rate** — How often users make mistakes
- **Tools:** Google Analytics, Mixpanel, Hotjar, Amplitude

#### A/B Testing
- **What:** Comparing two versions to see which performs better
- **When:** Optimizing existing designs, testing hypotheses
- **How:** Random split, measure conversion/success metric
- **Sample size:** Depends on expected effect size (use calculators)
- **Tools:** Optimizely, VWO, Google Optimize, LaunchDarkly

#### Heatmaps & Session Recordings
- **What:** Visual representation of where users click, scroll, and focus
- **When:** Understanding actual behavior on existing pages
- **How:** Tools track mouse movement, clicks, scroll depth
- **Tools:** Hotjar, Crazy Egg, FullStory, LogRocket

### Synthesis Methods

#### Personas
- **What:** Fictional characters representing user segments
- **When:** Aligning the team on who we're designing for
- **Components:**
  - Name, photo, demographics
  - Goals and motivations
  - Frustrations and pain points
  - Behaviors and preferences
  - Quote capturing their essence
- **How many:** 3–5 personas (more becomes unwieldy)
- **Anti-pattern:** Don't create personas and never reference them

#### Empathy Maps
- **What:** Visual representation of what users think, feel, say, and do
- **When:** Synthesizing interview data, building shared understanding
- **Quadrants:**
  - **Think** — What's on their mind?
  - **Feel** — What emotions are they experiencing?
  - **Say** — What do they tell others?
  - **Do** — What actions do they take?

#### Journey Maps
- **What:** Visual timeline of a user's experience with a product/service
- **When:** Understanding end-to-end experience, identifying pain points
- **Components:**
  - Stages of the journey
  - User actions at each stage
  - Thoughts and feelings
  - Pain points and opportunities
  - Touchpoints (where they interact with the product)
- **Key insight:** Reveals gaps between what users expect and what they experience

#### Affinity Diagrams
- **What:** Grouping observations/insights into themes
- **When:** Synthesizing large amounts of qualitative data
- **How:** Write observations on sticky notes → group by theme → label groups
- **Tools:** Miro, FigJam, physical sticky notes

## Research Planning

### When to Use Which Method

| Research Goal | Method | Timing |
|---|---|---|
| Understand user needs | Interviews, Contextual Inquiry | Discovery |
| Validate assumptions | Surveys, A/B Testing | Validation |
| Design navigation | Card Sorting | Design |
| Measure satisfaction | Surveys, NPS | Ongoing |
| Find usability issues | Usability Testing | Testing |
| Understand behavior | Analytics, Heatmaps | Post-launch |
| Align team on users | Personas, Empathy Maps | Anytime |
| Map experience | Journey Maps | Discovery/Design |

### Research Cadence

| Phase | Research Type | Frequency |
|---|---|---|
| Discovery | Interviews, Contextual Inquiry | 1–2 weeks |
| Design | Card Sorting, Usability Testing | Weekly sprints |
| Validation | A/B Testing, Surveys | Continuous |
| Post-launch | Analytics, Heatmaps, NPS | Ongoing |

## Anti-Patterns

| Anti-Pattern | What It Looks Like | Fix |
|---|---|---|
| **No research** | "We know our users" | Start with 5 user interviews |
| **Research theater** | Personas created once, never used | Reference personas in every design decision |
| **Confirmation bias** | Only asking questions that confirm assumptions | Include neutral and opposing questions |
| **Too many surveys** | Survey fatigue, low response rates | Keep surveys short, incentivize |
| **Ignoring analytics** | Only doing qualitative research | Combine qualitative (why) with quantitative (how many) |

## Related

- [[31 UX Design Process]] — Using research to inform wireframes and prototypes
- [[33 Usability Testing and AB Testing]] — Validating designs with users
- [[UX UI Essential Documents]] — Research deliverables checklist
