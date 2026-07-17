---
tags:
- hci
- computing
- foundations
- cognitive-load
---

# 02 Cognitive Load

Cognitive load is how much mental effort your interface demands. Humans have limited working memory — overload it and users make mistakes, feel frustrated, or abandon the task entirely.

---

## Miller's Law

> The average person can hold **7 ± 2** items in working memory at once.

| Implication | Design Rule |
|-------------|-------------|
| Phone numbers are chunked: 091-234-5678 | Group related items into chunks |
| Navigation with 15 items overwhelms | Keep menu items to 5-7 per level |
| Too many form fields = abandonment | Break long forms into steps |

---

## Three Types of Cognitive Load

| Type | Source | What to Do |
|------|--------|------------|
| **Intrinsic** | Complexity of the task itself | Can't eliminate. Simplify where possible. |
| **Extraneous** | Bad design adding unnecessary thinking | Eliminate. This is the developer's job. |
| **Germane** | Effort to learn and build mental models | Support with consistency, patterns, progressive disclosure. |

> **Goal:** Minimize extraneous load so the user's brain is free for the actual task.

---

## Hick's Law

> Decision time increases **logarithmically** with the number of choices.

- 2 options: fast decision
- 20 options: paralysis
- **Fix:** Progressive disclosure — show only what's needed now, reveal more on demand.

---

## Reducing Cognitive Load (Practical)

| Technique | Example |
|-----------|---------|
| **Chunking** | Break content into digestible groups. White space between sections. |
| **Progressive disclosure** | Show advanced options only when requested. |
| **Defaults** | Pre-fill the most common choice. |
| **Recognition > Recall** | Show options in a list, don't make users type from memory. |
| **Consistent patterns** | Same layout, same interaction model = less learning per page. |
| **Reduce noise** | Every unnecessary element steals attention from the necessary ones. |
| **Clear hierarchy** | Size, color, position signal what's important. |

---

## For Developers Specifically

Cognitive load applies to **code** too — not just UIs:

| High cognitive load code | Low cognitive load code |
|---|---|
| Nested ternaries | Named variables with `if/else` |
| 500-line functions | Small functions with clear names |
| Implicit state mutations | Explicit, traceable state changes |
| Clever one-liners | Readable, obvious code |

> "Any fool can write code that a computer can understand. Good programmers write code that humans can understand." — Martin Fowler

---

## Sources

- Miller, George A. "The Magical Number Seven, Plus or Minus Two" (1956)
- Hick, W.E. "On the Rate of Gain of Information" (1952)
- Sweller, John. Cognitive Load Theory (1988)
