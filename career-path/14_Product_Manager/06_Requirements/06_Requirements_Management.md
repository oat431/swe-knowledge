---
title: Requirements Management
parent: Requirements
summary: Managing requirements through delivery
tags:
  - requirements
  - management
  - traceability
  - change
---

# Requirements Management

> Requirements change during delivery. Management ensures changes are controlled, traceable, and aligned with goals throughout the project lifecycle.

## Why Requirements Management Matters

**Without management:**
- Scope creep
- Lost requirements
- Misalignment
- Rework and delays

**With management:**
- Controlled changes
- Complete traceability
- Aligned delivery
- Efficient execution

## Requirements Management Activities

### 1. Baselining

**Establish baseline:**

**Baseline definition:**
- Approved requirements
- Fixed scope
- Reference point for changes
- Agreement on what to build

**Process:**
```
1. Finalize requirements
2. Review with stakeholders
3. Get formal approval
4. Document baseline
5. Communicate to team
```

**Example:**
```
Baseline v1.0 (January 15):
- 25 user stories approved
- Acceptance criteria defined
- Estimates confirmed
- Sprint 1-3 planned

Any changes require:
- Change request
- Impact assessment
- Stakeholder approval
- Baseline update
```

### 2. Change Control

**Manage changes:**

**Change request process:**
```
1. Submit change request
   - What changes?
   - Why change?
   - Impact assessment

2. Review change
   - Evaluate necessity
   - Assess impact
   - Consider alternatives

3. Approve/reject
   - Decision by product owner
   - Document rationale
   - Communicate decision

4. Implement change
   - Update requirements
   - Adjust plans
   - Notify stakeholders
```

**Change request template:**
```
Change Request #123

What: Add two-factor authentication

Why: Security audit requirement

Impact:
- Scope: +2 user stories
- Effort: +5 days
- Schedule: Delays sprint 2 by 1 day
- Cost: +$2,000

Alternatives considered:
- Defer to next release (rejected: security risk)
- Simplified implementation (rejected: not secure enough)

Decision: Approved
Rationale: Security requirement, cannot defer
```

### 3. Traceability

**Track relationships:**

**Traceability matrix:**
```
Business Goal → User Requirement → User Story → Task → Test
    ↓                ↓                ↓           ↓        ↓
Improve        Customer needs     As a rep,    Develop  Test case
efficiency     quick search       I want to    search   verifies
                                  search       feature  search
```

**Benefits:**
- Impact analysis
- Coverage verification
- Change management
- Compliance evidence

**Example:**
```
Requirement: Customer search

Traces to:
- Business goal: Improve call handling efficiency
- User need: Find customers quickly
- User stories: US-001, US-002, US-003
- Tasks: TASK-101, TASK-102, TASK-103
- Tests: TC-201, TC-202, TC-203

If business goal changes:
- Check all traced items
- Assess impact
- Update or remove as needed
```

### 4. Version Control

**Track requirement versions:**

**Versioning approach:**
```
v1.0: Initial baseline (January 15)
v1.1: Added two-factor auth (February 1)
v1.2: Clarified search criteria (February 10)
v2.0: Major scope change (March 1)
```

**Change log:**
```
v1.1 (February 1):
- Added: Two-factor authentication
- Reason: Security requirement
- Impact: +2 stories, +5 days

v1.2 (February 10):
- Updated: Search acceptance criteria
- Reason: Clarified performance requirement
- Impact: No schedule impact
```

### 5. Status Tracking

**Monitor requirements:**

**Status categories:**
- Not Started
- In Progress
- In Review
- Completed
- Deferred
- Cancelled

**Tracking dashboard:**
```
Sprint 2 Status:

Completed: 5 stories (62%)
In Progress: 2 stories (25%)
In Review: 1 story (13%)
Not Started: 0 stories (0%)

On track: Yes
Risks: None
```

## Requirements Management Process

### 1. Setup

**Establish management:**
- Define baseline process
- Set up change control
- Create traceability matrix
- Choose tools

### 2. Execute

**During delivery:**
- Track status
- Manage changes
- Maintain traceability
- Update versions

### 3. Monitor

**Ongoing oversight:**
- Review progress
- Identify issues
- Assess risks
- Report status

### 4. Close

**At completion:**
- Verify all requirements met
- Document lessons learned
- Archive requirements
- Update traceability

## Requirements Management Tools

### 1. Requirements Management Tools

**Specialized tools:**
- Jira (user stories, tracking)
- Azure DevOps (requirements, traceability)
- Jama Connect (traceability, compliance)
- IBM DOORS (enterprise requirements)

**Features:**
- Requirement storage
- Traceability
- Change tracking
- Status reporting
- Version control

### 2. Traceability Tools

**Matrix tools:**
- Spreadsheets (simple projects)
- Requirements tools (complex projects)
- Custom databases (specialized needs)

**Example matrix:**
```
| Business Goal | User Req | Story | Task | Test | Status |
|---------------|----------|-------|------|------|--------|
| Efficiency    | Search   | US-01 | T-01 | TC-1 | Done   |
| Efficiency    | Search   | US-02 | T-02 | TC-2 | Done   |
| Efficiency    | Filter   | US-03 | T-03 | TC-3 | In Prog|
```

### 3. Change Management Tools

**Change tracking:**
- Issue trackers (Jira, GitHub Issues)
- Change request forms
- Approval workflows
- Impact assessment templates

## Requirements Management Best Practices

### 1. Establish Clear Processes

**Documented processes:**
```
Baseline Process:
1. Product owner finalizes requirements
2. Team reviews and estimates
3. Stakeholders approve
4. Baseline documented and communicated

Change Control Process:
1. Submit change request
2. Impact assessment
3. Product owner decision
4. Update baseline if approved
```

### 2. Maintain Traceability

**Always trace:**
- Why requirement exists (business goal)
- What implements it (stories, tasks)
- How to verify (tests)
- What depends on it (dependencies)

**Benefits:**
- Impact analysis
- Coverage verification
- Compliance evidence
- Change management

### 3. Control Changes

**Change discipline:**
- No informal changes
- All changes through process
- Impact always assessed
- Stakeholders informed

**Avoid:**
- Scope creep
- Gold plating
- Unauthorized changes
- Lost requirements

### 4. Communicate Status

**Regular updates:**
- Sprint reviews: Detailed status
- Stakeholder meetings: High-level progress
- Dashboards: Real-time visibility
- Reports: Formal documentation

### 5. Learn and Improve

**Continuous improvement:**
- Track requirement changes
- Analyze change reasons
- Identify patterns
- Improve processes

**Example:**
```
Retrospective findings:
- 40% of changes due to unclear requirements
- Solution: Better elicitation and analysis
- 30% of changes due to scope creep
- Solution: Stronger change control
- 20% of changes due to market shifts
- Solution: More frequent prioritization
```

## Common Requirements Management Mistakes

### 1. No Baseline

**Mistake:**
- Requirements keep changing
- No agreement on scope
- Moving target

**Fix:**
- Establish formal baseline
- Get stakeholder approval
- Control changes

### 2. Informal Changes

**Mistake:**
- "Just add this small thing"
- No impact assessment
- Scope creep

**Fix:**
- Formal change process
- Always assess impact
- Document decisions

### 3. Lost Traceability

**Mistake:**
- Don't know why requirement exists
- Can't assess change impact
- Compliance issues

**Fix:**
- Maintain traceability matrix
- Update with changes
- Regular reviews

### 4. Poor Communication

**Mistake:**
- Changes not communicated
- Team working on old requirements
- Stakeholder surprises

**Fix:**
- Communicate all changes
- Update all stakeholders
- Regular status updates

### 5. No Version Control

**Mistake:**
- Don't know what changed
- Can't revert if needed
- Confusion about current state

**Fix:**
- Version all requirements
- Maintain change log
- Archive old versions

## Requirements Management for Different Contexts

### Agile Projects

**Approach:**
- Product backlog as requirements
- Sprint backlog as baseline
- Flexible within sprint
- Adapt between sprints

**Tools:**
- Jira (backlog management)
- User stories (requirements format)
- Sprint planning (baselining)
- Sprint review (status)

### Waterfall Projects

**Approach:**
- Requirements document as baseline
- Formal change control
- Traceability matrix
- Stage gates

**Tools:**
- Requirements documents
- Change request forms
- Traceability matrices
- Stage gate reviews

### Regulated Industries

**Approach:**
- Strict traceability
- Compliance evidence
- Audit trail
- Formal approvals

**Tools:**
- Requirements management tools (DOORS, Jama)
- Traceability matrices
- Compliance checklists
- Audit reports

## Senior-Level Requirements Management

1. **Strategic management**
   - Not just tactical tracking
   - Strategic alignment
   - Portfolio management

2. **Management leadership**
   - Establish management processes
   - Train teams
   - Build management culture

3. **Complex management**
   - Multiple projects
   - Cross-team dependencies
   - Organizational changes

4. **Continuous improvement**
   - Analyze management effectiveness
   - Improve processes
   - Share best practices

## Metrics

- Requirements stability (changes after baseline)
- Traceability coverage (% requirements traced)
- Change request cycle time
- Requirements completion rate
- Stakeholder satisfaction with management

## Resources

- [[body-of-knowledge/BABOK/07_Requirements_Life_Cycle_Management]] - Requirements management
- Software Requirements by Karl Wiegers
- Mastering the Requirements Process by Suzanne Robertson

## Checklist

Before delivery:
- [ ] Baseline established
- [ ] Change control process defined
- [ ] Traceability matrix created
- [ ] Tools set up
- [ ] Team trained

During delivery:
- [ ] Changes controlled
- [ ] Traceability maintained
- [ ] Status tracked
- [ ] Stakeholders informed
- [ ] Versions controlled

After delivery:
- [ ] All requirements verified
- [ ] Lessons learned documented
- [ ] Requirements archived
- [ ] Traceability complete
- [ ] Management effectiveness reviewed
