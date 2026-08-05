---
title: Customer Interviews
parent: Problem Discovery
summary: Structured conversations to understand needs and context
tags:
  - discovery
  - research
  - interviews
  - customers
---

# Customer Interviews

> Customer interviews are structured conversations designed to understand user needs, behaviors, and context. They reveal the "why" behind requests and uncover problems users may not articulate directly.

## When to Use Customer Interviews

**Use interviews when:**
- Exploring a new problem space
- Understanding user workflows and context
- Validating hypotheses about user needs
- Gathering qualitative insights
- Building empathy with users

**Don't use interviews when:**
- You need statistical validation (use surveys)
- You want to test a specific UI (use usability testing)
- You need behavioral data (use analytics)
- The problem is already well-understood

## Interview Types

### 1. Exploratory Interviews

**Purpose:** Understand the problem space
**Questions:**
- "Walk me through how you currently handle X"
- "What's the hardest part about X?"
- "When did you last encounter this problem?"
- "What would make this easier?"

### 2. Validation Interviews

**Purpose:** Test specific hypotheses
**Questions:**
- "How important is X to you on a scale of 1-10?"
- "What would you do if X didn't exist?"
- "How much time/money does X cost you?"

### 3. Follow-up Interviews

**Purpose:** Dig deeper on previous insights
**Questions:**
- "Last time we talked about X. Can you tell me more?"
- "You mentioned Y. What did you mean by that?"

## Interview Structure

### Before the Interview

1. **Define objectives**
   - What do you want to learn?
   - What hypotheses are you testing?

2. **Recruit participants**
   - 5-8 users per segment is usually sufficient
   - Recruit actual users, not just stakeholders
   - Offer appropriate incentives

3. **Prepare interview guide**
   - Opening: Build rapport, explain purpose
   - Context: Understand their role and situation
   - Core questions: Explore the problem
   - Closing: Wrap up and next steps

### During the Interview

1. **Build rapport** (2-3 minutes)
   - Thank them for their time
   - Explain the purpose (learning, not selling)
   - Get permission to record

2. **Understand context** (5-10 minutes)
   - "Tell me about your role"
   - "What does a typical day look like?"
   - "How does X fit into your workflow?"

3. **Explore the problem** (20-30 minutes)
   - Ask open-ended questions
   - Listen more than you talk
   - Follow up on interesting points

4. **Close** (5 minutes)
   - "Is there anything else I should know?"
   - "Who else should I talk to?"
   - Thank them again

### After the Interview

1. **Debrief immediately**
   - What surprised you?
   - What confirmed your hypotheses?
   - What contradicted your assumptions?

2. **Transcribe and annotate**
   - Note key quotes
   - Mark interesting moments
   - Identify patterns

3. **Share insights**
   - Summarize key findings
   - Share with team
   - Update problem understanding

## Interview Techniques

### The Five Whys

Ask "why" repeatedly to get to root causes:

```
User: "I need a faster search"
PM: "Why is search speed important?"
User: "Because I'm looking for specific records"
PM: "Why do you need specific records?"
User: "To answer customer questions"
PM: "Why is that difficult now?"
User: "Because customers call and I can't find info quickly"
Root cause: Need quick access to customer-specific information
```

### Critical Incident Technique

Ask about specific past events:

```
Bad: "Do you find the system easy to use?"
Good: "Tell me about the last time you struggled with the system"
```

### Jobs to Be Done

Frame questions around the job, not the solution:

```
Bad: "Do you like feature X?"
Good: "When you're trying to accomplish Y, what do you do?"
```

## Common Mistakes

### 1. Leading Questions

**Wrong:** "Don't you think the new design is better?"
**Right:** "How does the new design compare to the old one?"

### 2. Solution-Focused Questions

**Wrong:** "Would you like a mobile app?"
**Right:** "How do you currently access information when you're away from your desk?"

### 3. Talking Too Much

**Wrong:** Explaining your product for 10 minutes
**Right:** Asking questions and listening 80% of the time

### 4. Ignoring Context

**Wrong:** "Do you use feature X?"
**Right:** "Walk me through your workflow when you need to do Y"

## Analyzing Interview Data

### Affinity Mapping

1. Write each insight on a sticky note
2. Group related insights
3. Name each group
4. Identify themes and patterns

### Insight Synthesis

For each theme, ask:
- What does this tell us about the user?
- What problem does this reveal?
- How does this change our understanding?
- What should we do differently?

### Sharing Findings

**Format:**
```
Insight: Users need quick access to customer-specific information
Evidence: "I spend 10 minutes searching for records when customers call"
Problem: Current search doesn't support customer-specific queries
Opportunity: Customer-focused search could save 2+ hours per day
```

## Senior-Level Practices

1. **Interview at scale**
   - Build interview programs, not one-off sessions
   - Train others to conduct interviews
   - Create reusable interview guides

2. **Synthesize across sources**
   - Combine interview insights with analytics
   - Look for patterns across user segments
   - Connect qualitative and quantitative data

3. **Drive decisions**
   - Translate insights into product decisions
   - Challenge assumptions with evidence
   - Build organizational understanding of users

4. **Mentor others**
   - Teach interview techniques
   - Review interview guides
   - Provide feedback on interview skills

## Metrics

- Number of interviews conducted
- Insights generated per interview
- Decisions influenced by interview insights
- Time from insight to product change

## Resources

- [[body-of-knowledge/BABOK/02_Elicitation_and_Collaboration]] - Elicitation techniques
- Mom Test by Rob Fitzpatrick - Interview techniques
- Interviewing Users by Steve Portigal - Advanced methods

## Checklist

Before conducting interviews:
- [ ] Clear objectives defined
- [ ] Interview guide prepared
- [ ] Participants recruited and scheduled
- [ ] Recording permission obtained
- [ ] Team aligned on what we're learning

After interviews:
- [ ] Debriefed with team
- [ ] Insights documented and shared
- [ ] Patterns identified
- [ ] Next steps defined
