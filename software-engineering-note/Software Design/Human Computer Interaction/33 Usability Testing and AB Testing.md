---
tags: [usability-testing, ab-testing, analytics, ux-ui-design]
---

# 03 — Usability Testing and A/B Testing

> *Source: Rocket Surgery Made Easy by Steve Krug, Lean UX by Gothelf & Seiden, Nielsen Norman Group*

## Purpose

Testing validates that your designs actually work for real users. Usability testing finds problems; A/B testing optimizes solutions. Both are essential — you can't design well without testing.

## Usability Testing

### What It Is
Observing real users attempting real tasks on your product to identify usability issues.

### When to Test
- **Early** — Test wireframes and prototypes before development
- **During** — Test working features during sprints
- **After** — Test live product for continuous improvement
- **Rule of thumb:** Test early, test often, test with 5 users

### Testing Methods

#### Moderated Usability Testing
- **What:** Facilitator guides user through tasks, observes, asks questions
- **When:** Deep understanding of user behavior, complex tasks
- **How:** In-person or remote (Zoom, Lookback), 30–60 minutes per user
- **Sample size:** 5 users per round (finds 85% of issues)
- **Key questions:**
  - "What do you think this does?"
  - "What would you do next?"
  - "What were you expecting?"

#### Unmoderated Usability Testing
- **What:** Users complete tasks independently, recorded for review
- **When:** Quick feedback, large sample needed, simple tasks
- **How:** Tools provide tasks, record screen + audio
- **Sample size:** 20–50 users for quantitative data
- **Tools:** UserTesting, Maze, Hotjar, Lookback

#### Guerrilla Testing
- **What:** Quick, informal testing with anyone available (coffee shop, office)
- **When:** Early validation, quick feedback, limited budget
- **How:** 5–10 minutes per person, 5–10 people
- **Key insight:** Better than no testing at all

#### Heuristic Evaluation
- **What:** Experts evaluate the interface against usability principles
- **When:** Quick assessment, complement to user testing
- **Heuristics (Nielsen's 10):**
  1. Visibility of system status
  2. Match between system and real world
  3. User control and freedom
  4. Consistency and standards
  5. Error prevention
  6. Recognition rather than recall
  7. Flexibility and efficiency of use
  8. Aesthetic and minimalist design
  9. Help users recognize, diagnose, and recover from errors
  10. Help and documentation

### Usability Test Planning

#### Task Design
- **Realistic tasks** — Based on actual user goals, not feature descriptions
- **Clear success criteria** — Define what "completing the task" looks like
- **Avoid leading** — Don't tell users what to click
- **Example:**
  - ❌ "Click the blue Submit button"
  - ✅ "You want to save your changes. How would you do that?"

#### Metrics
| Metric | What It Measures | Target |
|---|---|---|
| **Task success rate** | % of users completing the task | > 80% |
| **Time on task** | How long users take | Shorter is better |
| **Error rate** | How often users make mistakes | < 20% |
| **Satisfaction** | User's perceived ease (SUS, CSAT) | > 70 SUS |
| **First-click accuracy** | Where users click first | > 80% correct |

#### Reporting
- **Severity ratings** — Critical, major, minor, cosmetic
- **Frequency** — How many users encountered the issue
- **Screenshots/video clips** — Evidence of the issue
- **Recommendations** — Specific fixes, not vague suggestions

## A/B Testing

### What It Is
Comparing two versions of a design to see which performs better on a specific metric.

### When to A/B Test
- **After launch** — Optimizing existing features
- **High-traffic pages** — Enough data for statistical significance
- **Clear hypothesis** — "We believe changing X will improve Y"
- **Measurable outcomes** — Conversion rate, click rate, time on task

### A/B Test Process
1. **Hypothesis** — "Changing the CTA button from blue to green will increase click-through rate"
2. **Design variants** — Version A (control) and Version B (variant)
3. **Split traffic** — Random 50/50 split (or other ratio)
4. **Run test** — Until statistical significance (usually 1–4 weeks)
5. **Analyze results** — Which version performed better?
6. **Implement winner** — Roll out the winning version

### Sample Size Calculation
- **Minimum detectable effect (MDE)** — Smallest difference you care about
- **Statistical significance** — Usually 95% confidence (p < 0.05)
- **Power** — Usually 80% (probability of detecting a real effect)
- **Traffic** — Higher traffic = faster results
- **Tools:** Optimizely calculator, VWO calculator, Evan Miller's calculator

### A/B Test Metrics
| Metric | Description | Example |
|---|---|---|
| **Conversion rate** | % completing desired action | Sign-up, purchase |
| **Click-through rate (CTR)** | % clicking a specific element | CTA button |
| **Bounce rate** | % leaving after one page | Landing page |
| **Time on page** | How long users stay | Content pages |
| **Revenue per visitor** | Average revenue generated | E-commerce |

### Common A/B Tests
- **CTA buttons** — Color, text, size, placement
- **Headlines** — Value proposition wording
- **Forms** — Number of fields, layout, validation
- **Pricing** — Price points, display format
- **Images** — Hero images, product photos
- **Navigation** — Menu structure, labels
- **Social proof** — Testimonials, reviews, badges

### A/B Testing Anti-Patterns
| Anti-Pattern | What It Looks Like | Fix |
|---|---|---|
| **Testing without hypothesis** | "Let's try this and see" | Always start with a clear hypothesis |
| **Stopping too early** | Declaring a winner after 2 days | Wait for statistical significance |
| **Testing too many things** | A/B/C/D/E tests simultaneously | Test one variable at a time |
| **Ignoring small effects** | "2% improvement isn't worth it" | 2% on 1M users = 20K more conversions |
| **Not testing** | "We know what users want" | Test everything; assumptions are wrong |

## Analytics & Measurement

### Key Metrics (HEART Framework - Google)
| Category | Metric | Description |
|---|---|---|
| **Happiness** | CSAT, NPS | User satisfaction |
| **Engagement** | Sessions, time on site | How much users interact |
| **Adoption** | New users, feature adoption | How many start using |
| **Retention** | Return rate, churn | How many keep using |
| **Task success** | Completion rate, time on task | How well users achieve goals |

### Analytics Tools
- **Google Analytics** — Traffic, behavior, conversions
- **Mixpanel** — Event-based analytics, funnels
- **Amplitude** — Product analytics, user behavior
- **Hotjar** — Heatmaps, session recordings, surveys
- **FullStory** — Session replay, frustration signals
- **LogRocket** — Session replay with console logs

### Funnel Analysis
Tracking users through multi-step processes:
```
Landing Page: 1000 visitors
  ↓
Sign-up Form: 400 visitors (40% drop-off)
  ↓
Email Verification: 300 visitors (25% drop-off)
  ↓
Onboarding: 250 visitors (17% drop-off)
  ↓
First Action: 200 visitors (20% drop-off)
```
Each drop-off point is an optimization opportunity.

## Iteration Cycle

```
Test → Analyze → Hypothesize → Design → Implement → Test
```

1. **Test** — Usability test or A/B test current design
2. **Analyze** — What worked? What didn't? Why?
3. **Hypothesize** — "We believe changing X will improve Y because Z"
4. **Design** — Create the solution
5. **Implement** — Build and ship
6. **Test** — Validate the improvement

## Related

- [[30 User Research Methods]] — Research methods that complement testing
- [[31 UX Design Process]] — Testing wireframes and prototypes
- [[32 UI Design Process]] — Testing visual designs
- [[UX UI Essential Documents]] — Testing deliverables checklist
