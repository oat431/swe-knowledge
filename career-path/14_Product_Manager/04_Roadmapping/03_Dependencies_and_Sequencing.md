---
title: Dependencies and Sequencing
parent: Roadmapping
summary: Logical ordering of work
tags:
  - roadmapping
  - dependencies
  - sequencing
  - planning
---

# Dependencies and Sequencing

> Roadmap items don't exist in isolation. Dependencies constrain what can be built when, and sequencing determines the order. Understanding dependencies enables realistic roadmaps.

## Why Dependencies Matter

**Ignoring dependencies:**
- Unrealistic timelines
- Blocked teams
- Rework and waste
- Missed commitments

**Managing dependencies:**
- Realistic sequencing
- Smooth execution
- Efficient resource use
- Achievable commitments

## Types of Dependencies

### 1. Technical Dependencies

**Work that requires prior technical work:**

**Examples:**
- API requires data model
- Frontend requires backend
- Integration requires both systems
- Search requires indexing

**Example:**
```
Dependency chain:
1. Customer data model (foundation)
2. Customer API (exposes data)
3. Customer profile page (uses API)
4. Customer search (searches via API)

Cannot build search before API exists
```

### 2. Business Dependencies

**Work that requires prior business work:**

**Examples:**
- Launch requires beta testing
- GA requires pilot customers
- Expansion requires localization
- Pricing requires market research

**Example:**
```
Dependency chain:
1. Market research (understand pricing)
2. Pricing model (define tiers)
3. Billing system (implement pricing)
4. Sales enablement (train on pricing)
5. Launch (sell with pricing)

Cannot launch before pricing defined
```

### 3. Resource Dependencies

**Work that requires specific resources:**

**Examples:**
- Mobile app requires mobile developers
- AI features require ML engineers
- Security audit requires security team
- Design requires UX designers

**Example:**
```
Resource constraints:
- 2 mobile developers available
- Mobile app requires 4 months
- Can only work on one mobile initiative at a time

Sequencing:
Q2: Customer mobile app
Q3: Partner mobile app
Q4: Admin mobile app
```

### 4. External Dependencies

**Work that requires external parties:**

**Examples:**
- Integration requires partner API
- Compliance requires certification
- Launch requires partner readiness
- Feature requires third-party service

**Example:**
```
External dependencies:
- Salesforce integration requires Salesforce approval (4 weeks)
- Payment processing requires PCI certification (8 weeks)
- App store listing requires Apple review (1 week)

Must plan for external timelines
```

## Dependency Mapping

### 1. Identify Dependencies

**For each roadmap item:**
- What must be done first?
- What resources are needed?
- What external parties involved?
- What business prerequisites exist?

**Example:**
```
Item: Unified customer view

Technical dependencies:
- Customer data model
- CRM integration API
- Order management API
- Knowledge base API

Resource dependencies:
- Backend developers (3)
- Frontend developers (2)
- Integration specialist (1)

External dependencies:
- Salesforce API access (2 weeks)
- Zendesk integration approval (1 week)

Business dependencies:
- Customer data privacy review
- Security assessment
```

### 2. Visualize Dependencies

**Dependency diagram:**
```
Data Model → CRM API ─────┐
           → Order API ────┼→ Unified View → Search → AI
           → KB API ───────┘
```

**Critical path:**
```
Longest path through dependencies:
Data Model (2w) → CRM API (3w) → Unified View (4w) → Search (3w) → AI (4w)
Total: 16 weeks minimum
```

### 3. Document Dependencies

**Dependency register:**
```
| Item | Depends On | Type | Status | Impact |
|------|------------|------|--------|--------|
| Unified View | Data Model | Technical | Complete | Blocks start |
| Unified View | CRM API | Technical | In Progress | Blocks integration |
| Unified View | Security Review | Business | Not Started | Blocks launch |
```

## Sequencing Strategies

### 1. Critical Path Method

**Identify longest path:**
- Map all dependencies
- Calculate path durations
- Identify critical path
- Focus on critical path items

**Example:**
```
Path A: Data Model → Unified View → Launch (10 weeks)
Path B: Search Design → Search Build → Launch (8 weeks)
Path C: Mobile Design → Mobile Build → Launch (12 weeks)

Critical path: C (12 weeks)
Focus: Accelerate mobile work
```

### 2. Parallel Execution

**Work on independent items simultaneously:**

**Example:**
```
Independent work streams:
Stream 1: Unified customer view (backend focus)
Stream 2: Mobile app design (design focus)
Stream 3: Documentation (technical writing)

Can execute in parallel, different resources
```

### 3. Phased Delivery

**Deliver in logical phases:**

**Example:**
```
Phase 1: Foundation
- Data model
- Basic APIs
- Core infrastructure

Phase 2: Core Features
- Unified view
- Basic search
- Customer profiles

Phase 3: Advanced Features
- AI recommendations
- Advanced analytics
- Mobile app
```

### 4. Risk-Based Sequencing

**Address high-risk items first:**

**Example:**
```
High risk:
- New technology (AI engine)
- Uncertain requirements (compliance)
- External dependencies (partner API)

Sequence: Resolve risks early
Q1: AI proof of concept
Q1: Compliance research
Q1: Partner API exploration
```

## Managing Dependencies

### 1. Reduce Dependencies

**Strategies:**
- Modular architecture
- API-first design
- Feature flags
- Mock services

**Example:**
```
Instead of: Frontend waits for backend
Use: Backend API contract defined early
     Frontend builds against contract
     Backend implements to contract
     Integrate when both ready
```

### 2. Coordinate Dependencies

**For unavoidable dependencies:**
- Clear interfaces and contracts
- Regular coordination meetings
- Shared visibility into progress
- Escalation paths for blockers

### 3. Monitor Dependencies

**Track:**
- Dependency status
- Progress against plan
- Emerging blockers
- Timeline impacts

**Example:**
```
Dependency: CRM API integration
Status: In progress
Progress: 60% complete
ETA: March 15 (on track)
Blockers: None
Impact if delayed: Unified view delayed
```

### 4. Mitigate Dependency Risks

**Risk mitigation:**
- Identify fallback options
- Build buffer time
- Have backup resources
- Plan for delays

**Example:**
```
Dependency: External partner API
Risk: Partner delays delivery
Mitigation:
- Start integration early
- Build mock for testing
- Have alternative partner identified
- Buffer 2 weeks in timeline
```

## Common Dependency Mistakes

### 1. Ignoring Dependencies

**Mistake:** Planning without dependency analysis
**Fix:** Map dependencies before sequencing

### 2. Hidden Dependencies

**Mistake:** Dependencies not identified until blocking
**Fix:** Thorough dependency discovery

### 3. Over-Optimization

**Mistake:** No buffer between dependent items
**Fix:** Add buffer for uncertainty

### 4. Single Point of Failure

**Mistake:** Everything depends on one item
**Fix:** Reduce coupling, add alternatives

### 5. Not Monitoring

**Mistake:** Dependencies tracked at start only
**Fix:** Ongoing dependency monitoring

## Senior-Level Dependency Management

1. **Architectural dependencies**
   - Not just task dependencies
   - System and architecture dependencies
   - Technical debt dependencies

2. **Cross-team coordination**
   - Manage dependencies across teams
   - Coordinate releases
   - Align roadmaps

3. **Strategic dependencies**
   - Market timing dependencies
   - Partnership dependencies
   - Organizational dependencies

4. **Dependency reduction**
   - Architect for loose coupling
   - Build platform capabilities
   - Reduce systemic dependencies

## Metrics

- Dependency count per item
- Critical path length
- Dependency-related delays
- Dependency resolution time
- Parallel execution rate

## Resources

- [[body-of-knowledge/PMBOK/06_Schedule_Performance_Domain]] - Schedule dependencies
- [[body-of-knowledge/PMBOK/10_Risk_Performance_Domain]] - Risk management
- The Principles of Product Development Flow by Don Reinertsen

## Checklist

Before sequencing:
- [ ] All dependencies identified
- [ ] Dependencies documented
- [ ] Critical path calculated
- [ ] Resource dependencies understood
- [ ] External dependencies tracked

During execution:
- [ ] Dependencies monitored
- [ ] Blockers escalated
- [ ] Progress tracked
- [ ] Risks mitigated
- [ ] Timeline impacts assessed

