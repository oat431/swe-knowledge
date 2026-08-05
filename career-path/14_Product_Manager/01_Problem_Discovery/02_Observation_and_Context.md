---
title: Observation and Context
parent: Problem Discovery
summary: Learning from behavior, not just words
tags:
  - discovery
  - research
  - observation
  - context
  - ethnography
---

# Observation and Context

> What people do often differs from what they say they do. Observation and contextual inquiry reveal actual behaviors, workflows, and constraints that interviews alone cannot uncover.

## Why Observation Matters

**The Say-Do Gap:**
- Users say they follow process X, but actually do Y
- Users claim feature A is important, but never use it
- Users describe ideal workflows, not real ones

**Context reveals:**
- Environmental constraints (noise, interruptions, time pressure)
- Workarounds and hacks
- Social dynamics and politics
- Tool ecosystems (what else they use)
- Emotional states (frustration, confusion, satisfaction)

## Observation Methods

### 1. Contextual Inquiry

**What:** Observe users in their natural environment while they work
**When:** Understanding complex workflows, discovering hidden problems
**How:**
- Sit with users as they do their actual work
- Ask questions in real-time: "What are you doing now?"
- Note workarounds, interruptions, and pain points
- Observe for 1-2 hours minimum

**Example:**
```
Observation: Customer service rep uses 3 monitors
- Monitor 1: CRM system
- Monitor 2: Knowledge base
- Monitor 3: Chat window

Insight: Constant context-switching between systems
Problem: Information scattered across multiple tools
Opportunity: Unified customer view
```

### 2. Fly on the Wall

**What:** Observe without interacting
**When:** Understanding natural behavior without observer effect
**How:**
- Position yourself where you can see and hear
- Take detailed notes
- Don't interrupt or ask questions
- Useful for public spaces, meetings, support calls

### 3. Shadowing

**What:** Follow a user through their day
**When:** Understanding end-to-end workflows
**How:**
- Follow user for half or full day
- Observe multiple contexts and transitions
- Note handoffs between people and systems
- Understand the full journey

### 4. Diary Studies

**What:** Users self-report over time
**When:** Understanding behavior over days or weeks
**How:**
- Provide template for logging activities
- Ask users to record when they use your product
- Include context, emotions, and outcomes
- Review diaries and look for patterns

## What to Observe

### Behaviors
- What do they actually do? (not what they say they do)
- What order do they do things in?
- What shortcuts or workarounds do they use?
- When do they get stuck or frustrated?

### Environment
- Physical space (desk, office, home, mobile)
- Tools and devices (monitors, phones, paper)
- Interruptions and distractions
- Time pressures and deadlines

### Social Context
- Who do they interact with?
- How do they collaborate?
- What are the power dynamics?
- What information do they share or hide?

### Emotional State
- When do they seem frustrated?
- When do they seem satisfied?
- What causes stress or anxiety?
- What gives them confidence?

## Observation Framework

### Before Observation

1. **Define focus**
   - What workflow are you studying?
   - What questions are you trying to answer?

2. **Get permission**
   - Explain what you're doing and why
   - Assure them you're studying the system, not evaluating them
   - Get permission to take notes or record

3. **Prepare tools**
   - Notebook or tablet for notes
   - Camera (if permitted)
   - Observation template

### During Observation

**Take notes on:**
```
Time: 9:15 AM
Activity: Processing customer refund
Steps:
1. Opened CRM, searched for customer
2. Found order, clicked refund button
3. Got error message "Contact support"
4. Opened chat, waited 5 minutes
5. Explained situation to support agent
6. Agent processed refund manually
7. Copied confirmation number
8. Pasted into CRM notes

Observations:
- Frustrated by error message
- Wait time caused visible annoyance
- Manual copy-paste seems error-prone

Questions to ask:
- How often does this error happen?
- What's the impact of the wait time?
```

### After Observation

1. **Debrief immediately**
   - What surprised you?
   - What patterns did you see?
   - What questions remain?

2. **Synthesize findings**
   - Identify pain points
   - Map workflows
   - Note opportunities

3. **Validate with user**
   - Share your observations
   - Ask if you understood correctly
   - Get their interpretation

## Analyzing Observation Data

### Workflow Mapping

Create visual maps of actual workflows:
```
Current State:
Customer calls → Rep searches CRM → Rep checks knowledge base → 
Rep puts customer on hold → Rep asks colleague → Rep calls back

Pain Points:
- Multiple system switching
- Long hold times
- Manual knowledge transfer

Opportunity:
- Integrated knowledge base
- Real-time collaboration tools
```

### Pain Point Analysis

For each pain point, document:
- **What:** The specific problem
- **Who:** Who experiences it
- **When:** When it occurs in the workflow
- **Impact:** How it affects the user and business
- **Frequency:** How often it happens

### Opportunity Identification

For each opportunity:
- **Problem:** What pain point does it address?
- **Solution:** High-level approach (not detailed design)
- **Value:** What would improve?
- **Evidence:** What observation supports this?

## Common Pitfalls

### 1. Observer Effect

**Problem:** Users change behavior when watched
**Solutions:**
- Spend enough time that they forget you're there
- Observe multiple users to find patterns
- Compare observations with analytics data

### 2. Confirmation Bias

**Problem:** Seeing what you expect to see
**Solutions:**
- Take detailed, objective notes
- Review notes with others
- Look for disconfirming evidence

### 3. Surface-Level Observation

**Problem:** Noting what without understanding why
**Solutions:**
- Ask "why" during observation (contextual inquiry)
- Follow up with interviews
- Observe multiple times in different contexts

### 4. Ignoring Context

**Problem:** Focusing only on the product
**Solutions:**
- Observe the full environment
- Note interruptions and constraints
- Understand the broader workflow

## Senior-Level Practices

1. **Build observation programs**
   - Regular observation sessions
   - Train team members to observe
   - Create observation repositories

2. **Combine methods**
   - Observations + interviews + analytics
   - Triangulate findings across sources
   - Build comprehensive user understanding

3. **Drive systemic change**
   - Identify systemic issues, not just product issues
   - Influence organizational processes
   - Advocate for user-centered design

## Metrics

- Hours of observation conducted
- Pain points identified
- Workflows mapped
- Opportunities generated from observation
- Changes made based on observation insights

## Resources

- [[body-of-knowledge/BABOK/02_Elicitation_and_Collaboration]] - Contextual inquiry
- Contextual Design by Hugh Beyer and Karen Holtzblatt
- Observing the User Experience by Elizabeth Goodman

## Checklist

Before observation:
- [ ] Focus and questions defined
- [ ] Permission obtained
- [ ] Observation template prepared
- [ ] Team briefed on what to look for

During observation:
- [ ] Detailed notes taken
- [ ] Photos/diagrams created (if permitted)
- [ ] Questions asked (contextual inquiry)
- [ ] User debriefed

After observation:
- [ ] Notes reviewed and synthesized
- [ ] Workflows mapped
- [ ] Pain points documented
- [ ] Opportunities identified
- [ ] Findings shared with team
