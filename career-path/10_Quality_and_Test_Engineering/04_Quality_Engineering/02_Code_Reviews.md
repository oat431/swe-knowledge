---
title: Code Reviews
parent: Quality Engineering
topic: Catching issues through peer review
difficulty: specialist
created: 2026-08-05
tags:
  - career-path
  - quality-engineering
  - code-reviews
---

# Code Reviews

> **Core Principle:** Code reviews catch defects, share knowledge, and improve code quality. They are a critical quality practice, not a bureaucratic hurdle.

## What Code Reviews Are

Code reviews are:
- **Peer inspection:** Developers review each other's code
- **Knowledge sharing:** Learn from each other's approaches
- **Quality gate:** Catch defects before they merge
- **Mentoring:** Senior developers guide junior developers
- **Standard enforcement:** Ensure coding standards are followed

## Why Code Reviews Matter

**Benefits:**
- Catch defects early (cheaper than testing)
- Improve code quality
- Share knowledge across team
- Maintain coding standards
- Reduce bus factor (multiple people understand code)
- Mentoring opportunity

**Research shows:**
- Code reviews find 60-65% of defects
- Each hour of review saves 10+ hours of debugging
- Teams with reviews have 30-50% fewer production defects

## Code Review Process

### 1. Preparation (Author)

**Before requesting review:**
- Code compiles and runs
- Tests pass locally
- Static analysis clean
- Self-review completed
- Changes are focused (one feature/fix)
- Documentation updated
- Commit message is clear

**Self-review checklist:**
- [ ] Code is readable and clear
- [ ] Functions are small and focused
- [ ] Names are meaningful
- [ ] No duplicated code
- [ ] Error handling is appropriate
- [ ] Tests cover the changes
- [ ] No TODOs without tickets
- [ ] No commented-out code

### 2. Request Review

**Create pull request with:**
- Clear title and description
- Link to issue/ticket
- Screenshots (for UI changes)
- Testing notes
- Areas of concern

**PR template:**
```markdown
## Description
[What does this change do and why?]

## Related Issue
Closes #[issue-number]

## Changes
- [Change 1]
- [Change 2]
- [Change 3]

## Testing
- [How was this tested?]
- [Any special testing considerations?]

## Screenshots
[If UI changes, add screenshots]

## Checklist
- [ ] Tests added/updated
- [ ] Documentation updated
- [ ] Self-review completed
- [ ] No breaking changes (or documented)
```

### 3. Review (Reviewer)

**Review approach:**
1. **Understand context:** Read PR description and issue
2. **High-level review:** Architecture, approach, design
3. **Detailed review:** Code logic, edge cases, error handling
4. **Test review:** Test coverage, test quality
5. **Documentation review:** Is it clear and accurate?

**What to look for:**

**Correctness:**
- Does the code do what it's supposed to?
- Are edge cases handled?
- Is error handling appropriate?
- Are there any bugs?

**Readability:**
- Is the code easy to understand?
- Are names clear and meaningful?
- Is the structure logical?
- Are complex parts commented?

**Maintainability:**
- Is the code easy to modify?
- Is there duplicated code?
- Are dependencies managed?
- Is it testable?

**Performance:**
- Are there any obvious performance issues?
- Are resources used efficiently?
- Are there any memory leaks?

**Security:**
- Are there any security vulnerabilities?
- Is input validated?
- Are credentials handled safely?
- Are there injection risks?

**Testing:**
- Are there sufficient tests?
- Do tests cover edge cases?
- Are tests readable and maintainable?
- Do tests actually verify behavior?

### 4. Feedback

**Types of feedback:**

**Blocking (must fix):**
- Bugs
- Security issues
- Missing error handling
- Missing tests
- Architecture problems

**Non-blocking (should consider):**
- Style preferences
- Alternative approaches
- Optimization suggestions
- Documentation improvements

**Praise:**
- Good solutions
- Clean code
- Thorough testing
- Clear documentation

**Feedback format:**
```markdown
# Blocking Issues

## Bug: Off-by-one error in loop
```python
# Line 45
for i in range(len(items)):
    process(items[i+1])  # Will fail on last item
```
**Suggestion:** Use `range(len(items) - 1)` or handle last item separately.

## Missing error handling
The API call on line 78 doesn't handle network errors.
**Suggestion:** Add try-except block with retry logic.

# Non-blocking Suggestions

## Consider using a context manager
```python
# Current
file = open(filename)
data = file.read()
file.close()

# Suggested
with open(filename) as file:
    data = file.read()
```
This ensures the file is always closed, even if an exception occurs.

## Great work!
The refactoring of the payment processing logic is much cleaner.
The test coverage is excellent.
```

### 5. Address Feedback (Author)

**Respond to feedback:**
- Fix blocking issues
- Consider non-blocking suggestions
- Explain decisions if you disagree
- Update PR with changes
- Re-request review if significant changes

**Response format:**
```markdown
## Responses to Review Comments

### Bug: Off-by-one error
Fixed in commit abc123. Used `range(len(items) - 1)` as suggested.

### Missing error handling
Added try-except block with retry logic (3 attempts with exponential backoff).
See commit def456.

### Context manager suggestion
Good catch! Updated to use `with` statement in commit ghi789.
```

### 6. Approve and Merge

**Reviewer approves when:**
- All blocking issues resolved
- Code meets quality standards
- Tests pass
- Documentation is adequate

**Merge strategy:**
- Squash commits (clean history)
- Rebase on main (linear history)
- Merge commit (preserve history)

## Code Review Best Practices

### For Authors

**1. Keep PRs small:**
- < 400 lines of code
- One feature or fix per PR
- Easier to review thoroughly
- Faster feedback

**2. Write clear commit messages:**
```
Bad: "Fix bug"
Good: "Fix off-by-one error in pagination calculation"

Bad: "Update login"
Good: "Add rate limiting to login endpoint to prevent brute force attacks"
```

**3. Provide context:**
- Explain the "why" not just the "what"
- Link to issue or ticket
- Mention any special considerations

**4. Be responsive:**
- Respond to feedback promptly
- Be open to suggestions
- Explain your reasoning if you disagree

### For Reviewers

**1. Review promptly:**
- Don't block the author
- Review within 24 hours
- If you can't review, say so

**2. Be constructive:**
- Focus on the code, not the person
- Explain why something is a problem
- Suggest solutions, not just problems

**3. Be thorough but pragmatic:**
- Don't nitpick style (use linters)
- Focus on correctness, security, performance
- Approve when good enough, not perfect

**4. Ask questions:**
- "What happens if this fails?"
- "Have you considered this edge case?"
- "Why did you choose this approach?"

**5. Approve with confidence:**
- Only approve if you understand the code
- Only approve if you'd be comfortable maintaining it
- Ask for help if you're unsure

### For Teams

**1. Establish standards:**
- Code review checklist
- Style guide
- Review process
- Turnaround time expectations

**2. Rotate reviewers:**
- Don't always use the same reviewer
- Spread knowledge
- Different perspectives

**3. Use tools:**
- GitHub, GitLab, Bitbucket
- Automated checks (CI, linters)
- Review bots (CodeClimate, SonarQube)

**4. Track metrics:**
- Review turnaround time
- Defects found in review
- Review coverage (% of code reviewed)

## Code Review Checklist

### Correctness
- [ ] Code does what it's supposed to
- [ ] Edge cases handled
- [ ] Error handling appropriate
- [ ] No obvious bugs
- [ ] Logic is sound

### Readability
- [ ] Code is easy to understand
- [ ] Names are clear and meaningful
- [ ] Structure is logical
- [ ] Complex parts are commented
- [ ] No magic numbers

### Maintainability
- [ ] No duplicated code
- [ ] Functions are small and focused
- [ ] Dependencies are managed
- [ ] Easy to modify
- [ ] Follows SOLID principles

### Testing
- [ ] Tests added for new code
- [ ] Tests updated for changed code
- [ ] Tests cover edge cases
- [ ] Tests are readable
- [ ] Tests actually verify behavior

### Performance
- [ ] No obvious performance issues
- [ ] Resources used efficiently
- [ ] No unnecessary loops or allocations
- [ ] Database queries are efficient

### Security
- [ ] Input validated
- [ ] No SQL injection risks
- [ ] No XSS risks
- [ ] Credentials handled safely
- [ ] No sensitive data in logs

### Documentation
- [ ] Public APIs documented
- [ ] Complex logic explained
- [ ] README updated if needed
- [ ] Commit message is clear

## Common Code Review Anti-Patterns

| Anti-Pattern | Problem | Solution |
|--------------|---------|----------|
| **Rubber stamp** | Approve without reviewing | Actually review the code |
| **Nitpicking** | Focus on style over substance | Use linters for style |
| **Drive-by review** | Quick glance, not thorough | Allocate time for review |
| **Ego-driven** | "My way is better" | Focus on objective criteria |
| **Bikeshedding** | Argue over trivial details | Agree on standards, move on |
| **Gatekeeping** | Block for personal preferences | Approve if it works and is maintainable |
| **Slow reviews** | Block author for days | Review within 24 hours |

## Advanced Code Review Techniques

### 1. Pair Reviewing

**Two reviewers together:**
- More thorough
- Different perspectives
- Learning opportunity
- Faster decisions

### 2. Mob Reviewing

**Whole team reviews together:**
- For critical code
- Knowledge sharing
- Team alignment
- Complex changes

### 3. Automated Review

**Use tools to assist:**
- Linters for style
- Static analysis for bugs
- Security scanners
- Test coverage tools
- Performance profilers

### 4. Retrospective Reviews

**Review old code:**
- Find technical debt
- Identify patterns
- Plan refactoring
- Learn from mistakes

## Code Review Metrics

### Track These Metrics

**Review coverage:**
```
% of code that goes through review
Target: 100%
```

**Review turnaround time:**
```
Time from PR creation to approval
Target: < 24 hours
```

**Defects found in review:**
```
Number of defects found per review
Track trends over time
```

**Defect escape rate:**
```
% of defects that escape to testing/production
Target: < 10%
```

**Review participation:**
```
% of team members participating in reviews
Target: 100%
```

### Dashboard Example

```
Code Review Dashboard
═══════════════════════════════════

This Sprint:
  PRs reviewed: 45
  Average turnaround: 8 hours ✓
  Defects found: 23
  Review coverage: 100% ✓

Trends:
  [Graph: Review turnaround time]
  [Graph: Defects found in review]
  [Graph: Defect escape rate]

Top Reviewers:
  Alice: 15 reviews, 8 defects found
  Bob: 12 reviews, 6 defects found
  Charlie: 10 reviews, 5 defects found
```

## Key Takeaways

1. **Code reviews catch defects early:** Cheaper than testing or production
2. **Be constructive:** Focus on the code, not the person
3. **Keep PRs small:** Easier to review thoroughly
4. **Review promptly:** Don't block the author
5. **Use checklists:** Ensure consistent, thorough reviews

## Related Topics

- [[01_Defect_Prevention]]: Reviews as prevention
- [[03_Static_Analysis]]: Automated review assistance
- [[06_Quality_Culture]]: Building a review culture

## Existing Vault Connections

- [[software-engineering-note/12_Software_Quality/04_Code_Reviews]]: Code review practices
