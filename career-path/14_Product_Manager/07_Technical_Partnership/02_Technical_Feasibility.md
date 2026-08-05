---
title: Technical Feasibility
parent: Technical Partnership
summary: Assessing what's technically possible and practical
tags:
  - technical-partnership
  - feasibility
  - assessment
  - planning
---

# Technical Feasibility

> Not everything possible is practical. Technical feasibility assessment determines what can be built, how long it takes, and what trade-offs exist, enabling realistic planning.

## Why Feasibility Matters

**Without feasibility assessment:**
- Unrealistic commitments
- Failed deliveries
- Budget overruns
- Team frustration

**With feasibility assessment:**
- Realistic planning
- Achievable commitments
- Accurate budgets
- Successful delivery

## Feasibility Dimensions

### 1. Technical Possibility

**Can it be built?**

**Questions:**
- Is the technology available?
- Do we have the skills?
- Are there technical blockers?
- What are the technical risks?

**Example:**
```
Feature: Real-time video collaboration

Technical possibility:
✓ Technology exists (WebRTC)
✓ Libraries available
? Need video expertise (may need to hire)
✓ No fundamental blockers

Feasibility: Possible with right expertise
```

### 2. Implementation Complexity

**How hard is it to build?**

**Complexity factors:**
- System changes required
- Integration points
- Data migrations
- Performance requirements
- Security considerations

**Complexity levels:**

**Low (days):**
- UI changes
- Simple features
- Existing patterns

**Medium (weeks):**
- New features
- System integrations
- Performance optimization

**High (months):**
- Architecture changes
- Platform migrations
- New capabilities

**Example:**
```
Feature: Customer search

Low complexity:
- Search existing customer table
- Simple text matching
- No performance requirements

Medium complexity:
- Search across multiple tables
- Advanced filtering
- Sub-second response time

High complexity:
- Full-text search
- Real-time indexing
- Millions of records
- Fuzzy matching
```

### 3. Resource Requirements

**What does it take to build?**

**Resources:**
- Engineering time
- Specialized skills
- Infrastructure
- Third-party services
- Budget

**Example:**
```
Feature: Mobile app

Resource requirements:
- 2 mobile developers (6 months)
- 1 designer (2 months)
- 1 QA engineer (2 months)
- Cloud services ($2K/month)
- App store fees ($100/year)

Total: ~$300K
```

### 4. Timeline

**How long will it take?**

**Timeline factors:**
- Development time
- Testing time
- Deployment time
- Dependencies
- Parallel work

**Example:**
```
Feature: Payment integration

Timeline:
- Development: 4 weeks
- Integration testing: 2 weeks
- Security review: 1 week
- Staging deployment: 1 week
- Production deployment: 1 week

Total: 9 weeks
```

### 5. Risks

**What could go wrong?**

**Risk types:**
- Technical risks (might not work)
- Schedule risks (might take longer)
- Resource risks (might not have people)
- Dependency risks (might block on others)

**Risk assessment:**
```
High risk:
- New technology (unproven)
- Complex integrations
- Tight deadlines
- Resource constraints

Medium risk:
- Some unknowns
- Moderate complexity
- Reasonable timeline
- Available resources

Low risk:
- Well-understood
- Simple implementation
- Comfortable timeline
- Resources available
```

## Feasibility Assessment Process

### 1. Define Requirements

**Clear problem and solution:**
- What problem are we solving?
- What solution are we considering?
- What are the must-haves?
- What are the nice-to-haves?

### 2. Engage Engineering

**Involve engineering early:**
- Share requirements
- Ask for feasibility assessment
- Discuss approaches
- Identify risks

### 3. Assess Feasibility

**Engineering evaluates:**
- Technical possibility
- Implementation complexity
- Resource requirements
- Timeline
- Risks

### 4. Document Findings

**Feasibility report:**
```
Feature: AI-powered recommendations

Feasibility Assessment:

Technical Possibility: Possible
- ML libraries available
- Need training data
- Expertise required

Complexity: High
- New capability for team
- Complex algorithms
- Performance requirements

Resources:
- 2 ML engineers (hire or train)
- Data scientist (3 months)
- Infrastructure ($10K/month)

Timeline: 6 months
- 2 months: Data preparation
- 2 months: Model development
- 2 months: Integration and testing

Risks:
- High: Model accuracy uncertain
- Medium: Need to hire ML engineers
- Medium: Performance at scale unknown

Recommendation: Feasible but high investment and risk
```

### 5. Make Decision

**Product decision based on feasibility:**
- Proceed as planned
- Simplify requirements
- Defer to later
- Explore alternatives

**Example:**
```
Decision options:

Option A: Proceed as planned
- 6 months, high risk
- Full AI recommendations
- $500K investment

Option B: Simplified version
- 3 months, medium risk
- Basic rule-based recommendations
- $150K investment

Option C: Defer
- Wait for more data
- Hire ML team first
- Reassess in 6 months

Decision: Option B
Rationale: Lower risk, faster learning, can evolve to full AI later
```

## Feasibility Assessment Techniques

### 1. Technical Spike

**Time-boxed exploration:**

**When to use:**
- High uncertainty
- New technology
- Multiple approaches

**Process:**
```
1. Define question (1 hour)
2. Time-box exploration (1-3 days)
3. Engineering investigates
4. Document findings
5. Share learnings
```

**Example:**
```
Spike: Can we integrate with Salesforce API?

Question: Is Salesforce API integration feasible?
Time-box: 2 days

Investigation:
- Reviewed API documentation
- Tested authentication
- Explored data access
- Assessed rate limits

Findings:
✓ Integration possible
✓ Supports our use cases
⚠ Rate limits require batching
⚠ Need enterprise license ($50K/year)

Recommendation: Feasible with budget for license
```

### 2. Proof of Concept

**Validate approach:**

**When to use:**
- Unproven technology
- High risk
- Need validation

**Process:**
```
1. Define success criteria
2. Build minimal implementation
3. Test against criteria
4. Evaluate results
5. Decide on full implementation
```

**Example:**
```
POC: Real-time collaboration

Success criteria:
- Multiple users edit simultaneously
- Changes sync in <1 second
- No data loss

Implementation:
- Built basic editor
- Integrated WebRTC
- Tested with 3 users

Results:
✓ Synchronous editing works
✓ Sync time: 200ms
✓ No data loss in testing

Decision: Proceed with full implementation
```

### 3. Architecture Review

**Assess system fit:**

**When to use:**
- Major feature
- System changes
- Integration complexity

**Process:**
```
1. Present requirements
2. Review current architecture
3. Identify changes needed
4. Assess impact
5. Plan approach
```

**Example:**
```
Feature: Global search

Architecture review:

Current architecture:
- Separate databases per service
- No centralized search
- 10 microservices

Changes needed:
- Implement search service
- Index data from all services
- Real-time updates

Impact:
- Major architecture change
- 3 months development
- Performance considerations

Approach:
- Phase 1: Search current service
- Phase 2: Add other services
- Phase 3: Real-time indexing
```

### 4. Estimation

**Size the effort:**

**Estimation techniques:**

**T-shirt sizing:**
- XS: <1 day
- S: 1-3 days
- M: 1-2 weeks
- L: 2-4 weeks
- XL: 1-2 months
- XXL: >2 months (break it down)

**Story points:**
- Relative sizing
- Fibonacci sequence (1, 2, 3, 5, 8, 13)
- Compare to known work

**Time-based:**
- Hours or days
- Include testing and review
- Add buffer for uncertainty

**Example:**
```
Feature breakdown:

User authentication: M (1 week)
Customer profiles: L (2 weeks)
Search functionality: XL (6 weeks)
Email notifications: S (3 days)
Admin dashboard: L (2 weeks)

Total: ~12 weeks
```

## Feasibility Best Practices

### 1. Involve Engineering Early

**Don't wait until requirements are final:**
- Share problem early
- Get feasibility input during discovery
- Iterate on requirements with feasibility feedback
- Finalize together

### 2. Be Honest About Uncertainty

**Acknowledge what you don't know:**
```
Good: "We're not sure about performance at scale,
      let's do a spike to validate"
Bad: "It should work fine" (without validation)
```

### 3. Consider Alternatives

**Explore different approaches:**
```
Approach A: Build custom solution
- 6 months, full control
- High development cost

Approach B: Use third-party service
- 2 months, limited customization
- Lower cost, faster

Approach C: Hybrid approach
- Start with third-party
- Migrate to custom later
```

### 4. Plan for Uncertainty

**Include buffer:**
```
Estimate: 4 weeks
Add 25% buffer: 5 weeks
Reasonable commitment: 5 weeks
```

**Plan contingencies:**
```
If feasibility fails:
- Alternative approach ready
- Scope reduction options
- Timeline adjustments
```

### 5. Learn and Improve

**Track estimates vs. actual:**
- How accurate were estimates?
- What was underestimated?
- How to improve next time?

**Example:**
```
Retrospective:

Estimated: 8 weeks
Actual: 12 weeks

Why:
- Integration more complex than expected
- Performance issues discovered late
- Scope creep

Learnings:
- Add more buffer for integrations
- Do performance testing earlier
- Stronger scope control
```

## Common Feasibility Mistakes

### 1. Assuming Feasibility

**Mistake:**
```
PM: "We'll add real-time notifications"
(Later discovers it requires major architecture changes)
```

**Fix:**
```
PM: "Is real-time notifications feasible with current architecture?"
Engineering: "No, requires significant changes"
Together: "Let's plan simpler version or architecture evolution"
```

### 2. Ignoring Complexity

**Mistake:**
```
PM: "It's just a form, should be quick"
(Actually requires complex validation, integrations, workflows)
```

**Fix:**
```
Engineering: "This form is complex because of X, Y, Z"
PM: "Help me understand the complexity"
Together: "Let's simplify requirements or plan for complexity"
```

### 3. Optimistic Estimates

**Mistake:**
```
Engineering: "Probably 2-3 weeks"
PM: "Great, we'll commit to 2 weeks"
(Actually takes 4 weeks)
```

**Fix:**
```
Engineering: "2-3 weeks, but could be 4 if complications"
PM: "Let's plan for 3 weeks with 1 week buffer"
Together: "Commit to 3 weeks, communicate 4 week possibility"
```

### 4. Not Revisiting Feasibility

**Mistake:**
- Assessed feasibility once
- Requirements changed
- Never reassessed

**Fix:**
- Reassess when requirements change
- Update estimates
- Adjust plans

### 5. Ignoring Technical Debt

**Mistake:**
```
PM: "Just make it work fast"
(Adds technical debt, slows future work)
```

**Fix:**
```
Engineering: "Fast approach adds technical debt"
PM: "What's the long-term impact?"
Together: "Balance speed and sustainability"
```

## Senior-Level Feasibility Assessment

1. **Strategic feasibility**
   - Not just feature feasibility
   - Strategic initiative feasibility
   - Long-term technical planning

2. **Feasibility leadership**
   - Establish feasibility processes
   - Train teams in assessment
   - Build feasibility culture

3. **Complex assessment**
   - Multi-system feasibility
   - Cross-team dependencies
   - Organizational constraints

4. **Continuous improvement**
   - Track estimation accuracy
   - Improve assessment methods
   - Share learnings

## Metrics

- Feasibility assessment coverage (% features assessed)
- Estimate accuracy (estimated vs. actual)
- Feasibility surprises (unexpected blockers)
- Assessment efficiency (time to assess)
- Decision quality (outcomes of feasibility-based decisions)

## Resources

- [[body-of-knowledge/SWEBOK/10_Software_Engineering_Management]] - Project planning
- Software Estimation by Steve McConnell
- The Pragmatic Programmer by David Thomas and Andrew Hunt

## Checklist

Before assessing feasibility:
- [ ] Requirements clearly defined
- [ ] Engineering engaged
- [ ] Assessment approach selected
- [ ] Time allocated for assessment

During feasibility assessment:
- [ ] Technical possibility evaluated
- [ ] Complexity assessed
- [ ] Resources estimated
- [ ] Timeline estimated
- [ ] Risks identified
- [ ] Alternatives considered

After feasibility assessment:
- [ ] Findings documented
- [ ] Decision made
- [ ] Plans updated
- [ ] Stakeholders communicated
- [ ] Learnings captured
