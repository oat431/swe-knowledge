---
title: Requirements Prioritization
parent: Requirements
summary: Deciding which requirements to build first
tags:
  - requirements
  - prioritization
  - trade-offs
  - planning
---

# Requirements Prioritization

> Not all requirements are equal. Prioritization decides what to build first, ensuring you deliver maximum value with limited resources.

## Why Prioritization Matters

**Without prioritization:**
- Build everything at once (impossible)
- Waste resources on low-value features
- Miss critical requirements
- Stakeholder conflicts

**With prioritization:**
- Focus on highest value first
- Efficient resource use
- Deliver what matters most
- Clear trade-off decisions

## Prioritization Frameworks

### 1. MoSCoW Method

**Four categories:**

**Must Have:**
- Critical for success
- Cannot launch without
- Legal/regulatory requirements
- Core functionality

**Should Have:**
- Important but not critical
- Significant value
- Can work around temporarily
- Planned for this release

**Could Have:**
- Nice to have
- Small value
- Only if time/resources allow
- Consider for next release

**Won't Have (This Time):**
- Explicitly excluded
- Acknowledged but deferred
- Future consideration
- Out of scope

**Example:**
```
Must Have:
- User authentication
- Customer search
- Order placement
- Payment processing

Should Have:
- Email notifications
- Order tracking
- Product recommendations

Could Have:
- Wishlist feature
- Social media sharing
- Advanced filters

Won't Have:
- Mobile app (future release)
- International shipping (next year)
- AI recommendations (needs data)
```

### 2. Value vs. Effort Matrix

**Two dimensions:**

**Value:**
- Business value
- User value
- Strategic alignment
- Revenue impact

**Effort:**
- Development time
- Complexity
- Resources needed
- Risk level

**Four quadrants:**

```
High Value, Low Effort → Do First (Quick Wins)
High Value, High Effort → Plan Carefully (Major Projects)
Low Value, Low Effort → Consider (Fill-ins)
Low Value, High Effort → Avoid (Time Wasters)
```

**Example:**
```
Quick Wins (Do First):
- Password reset (high value, 2 days)
- Search filters (high value, 3 days)

Major Projects (Plan):
- Mobile app (high value, 3 months)
- Payment integration (high value, 2 months)

Fill-ins (Consider):
- Dark mode (low value, 2 days)
- Export to CSV (low value, 1 day)

Time Wasters (Avoid):
- Custom themes (low value, 2 weeks)
- Admin dashboard redesign (low value, 1 month)
```

### 3. Weighted Scoring

**Multiple criteria:**

**Criteria examples:**
- Business value (40%)
- User value (30%)
- Strategic alignment (20%)
- Technical feasibility (10%)

**Scoring process:**
1. Define criteria and weights
2. Score each requirement (1-5)
3. Calculate weighted score
4. Rank by score

**Example:**
```
Requirement: Customer search

Business value: 5 × 0.40 = 2.0
User value: 5 × 0.30 = 1.5
Strategic alignment: 4 × 0.20 = 0.8
Technical feasibility: 5 × 0.10 = 0.5

Total score: 4.8 out of 5.0
```

### 4. Kano Model

**Customer satisfaction:**

**Must-Be (Basic):**
- Expected features
- Dissatisfied if missing
- Not satisfied if present (expected)
- Example: Login, search

**Performance (Linear):**
- More is better
- Satisfaction proportional to quality
- Example: Speed, reliability

**Delighters (Excitement):**
- Unexpected features
- Not dissatisfied if missing
- Very satisfied if present
- Example: Smart suggestions, surprises

**Indifferent:**
- Don't care
- No impact on satisfaction
- Example: Internal code refactoring

**Reverse:**
- Dissatisfied if present
- Example: Too many notifications

**Example:**
```
Must-Be:
- User authentication
- Data security
- Basic search

Performance:
- Fast page loads
- Accurate search results
- Responsive design

Delighters:
- Smart autocomplete
- Personalized recommendations
- Proactive notifications

Indifferent:
- Database optimization
- Code refactoring

Reverse:
- Excessive pop-ups
- Too many emails
```

## Prioritization Process

### 1. Gather Requirements

**Complete list:**
- All identified requirements
- Business requirements
- User requirements
- Technical requirements

### 2. Select Framework

**Choose based on:**
- Team maturity
- Stakeholder needs
- Project context
- Decision complexity

**Recommendations:**
- Simple projects: MoSCoW
- Resource constraints: Value vs. Effort
- Multiple stakeholders: Weighted Scoring
- Customer focus: Kano Model

### 3. Score Requirements

**Apply framework:**
- Score each requirement
- Use consistent criteria
- Involve stakeholders
- Document rationale

### 4. Rank and Sequence

**Create order:**
- Rank by priority score
- Consider dependencies
- Plan releases
- Communicate decisions

### 5. Validate and Adjust

**Review priorities:**
- Check with stakeholders
- Validate assumptions
- Adjust for constraints
- Get buy-in

## Prioritization Best Practices

### 1. Involve Stakeholders

**Collaborative process:**
- Product owner leads
- Developers estimate effort
- Users provide value input
- Business defines strategy

**Benefits:**
- Better decisions
- Shared understanding
- Buy-in and alignment
- Reduced conflicts

### 2. Be Explicit About Trade-offs

**Clear decisions:**
```
"We're building X before Y because:
- X has higher business value (5 vs 3)
- X is needed for launch (Must Have)
- Y can wait until next release"
```

**Document:**
- What was chosen
- What was deferred
- Why (rationale)
- When deferred items will be reconsidered

### 3. Consider Dependencies

**Logical ordering:**
```
Must build first:
- User authentication (foundation)
- Database schema (data layer)

Then:
- User profiles (depends on auth)
- Search (depends on data)

Finally:
- Advanced features (depend on basics)
```

**Dependency types:**
- Technical (A requires B)
- Business (launch requires beta)
- User (onboarding before features)

### 4. Revisit Regularly

**Priorities change:**
- Market shifts
- New information
- Learning from users
- Resource changes

**Cadence:**
- Sprint planning: Detailed priorities
- Release planning: Release scope
- Quarterly: Strategic priorities

### 5. Balance Short and Long Term

**Avoid extremes:**

**Too short-term:**
- Only quick wins
- No strategic investment
- Technical debt accumulates
- Miss big opportunities

**Too long-term:**
- Only big projects
- No quick value
- Stakeholder frustration
- Market changes

**Balanced approach:**
```
70% Short-term value (this quarter)
20% Medium-term investment (next quarter)
10% Long-term strategic (this year)
```

## Common Prioritization Mistakes

### 1. HiPPO (Highest Paid Person's Opinion)

**Mistake:**
- Executive decides without data
- Ignores user/business value
- Political decisions

**Fix:**
- Use data and frameworks
- Make value explicit
- Show trade-offs

### 2. Everything is Priority 1

**Mistake:**
- No real prioritization
- Can't say no
- Resource overcommitment

**Fix:**
- Force ranking
- Limited capacity
- Explicit trade-offs

### 3. Ignoring Effort

**Mistake:**
- Only consider value
- Ignore cost and complexity
- Unrealistic plans

**Fix:**
- Include effort in scoring
- Value vs. Effort matrix
- Developer estimates

### 4. Not Revisiting

**Mistake:**
- Set priorities once
- Never adjust
- Miss changing needs

**Fix:**
- Regular reviews
- Adapt to learning
- Flexible planning

### 5. Stakeholder Conflicts

**Mistake:**
- Different priorities
- No resolution process
- Political battles

**Fix:**
- Common framework
- Transparent criteria
- Executive alignment

## Prioritization for Different Contexts

### Startup

**Focus:**
- Speed to market
- Learning and validation
- Core value proposition

**Approach:**
```
Must Have: MVP features (launch)
Should Have: Basic improvements
Could Have: Nice-to-haves
Won't Have: Scale features (premature)
```

### Enterprise

**Focus:**
- Strategic alignment
- Risk management
- Stakeholder management

**Approach:**
```
Weighted scoring with:
- Strategic alignment (40%)
- Business value (30%)
- Risk reduction (20%)
- User value (10%)
```

### Maintenance Mode

**Focus:**
- Stability
- Technical debt
- Incremental improvements

**Approach:**
```
30% New features
40% Bug fixes and improvements
30% Technical debt
```

## Senior-Level Prioritization

1. **Strategic prioritization**
   - Not just feature prioritization
   - Strategic initiative prioritization
   - Portfolio prioritization

2. **Prioritization leadership**
   - Establish prioritization processes
   - Train teams in frameworks
   - Build prioritization culture

3. **Complex trade-offs**
   - Navigate competing interests
   - Balance short and long term
   - Make tough decisions

4. **Organizational alignment**
   - Align priorities across teams
   - Manage dependencies
   - Coordinate releases

## Metrics

- Prioritization accuracy (predicted vs. actual value)
- Value delivered (business impact)
- Stakeholder satisfaction with priorities
- Priority stability (changes per quarter)
- Delivery against priorities

## Resources

- [[body-of-knowledge/BABOK/06_Strategy]] - Strategic prioritization
- The Lean Startup by Eric Ries
- Escaping the Build Trap by Melissa Perri

## Checklist

Before prioritizing:
- [ ] All requirements identified
- [ ] Framework selected
- [ ] Criteria defined
- [ ] Stakeholders involved
- [ ] Effort estimated

After prioritizing:
- [ ] Requirements ranked
- [ ] Trade-offs documented
- [ ] Rationale explained
- [ ] Dependencies considered
- [ ] Communicated to all
- [ ] Review scheduled
