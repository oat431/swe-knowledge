---
tags:
- hci
- computing
- foundations
- usability
---

# 01 Usability Principles

Jakob Nielsen's 10 usability heuristics — the minimum every developer should know about making interfaces usable. These aren't laws of physics, but violating them almost always creates user frustration.

---

## Nielsen's 10 Heuristics

| # | Heuristic | What It Means | Developer Example |
|---|-----------|---------------|-------------------|
| 1 | **Visibility of system status** | Always show what's happening | Loading spinners, progress bars, save confirmations |
| 2 | **Match between system and real world** | Use the user's language, not yours | "Delete" not "rm -rf", "Shopping Cart" not "Order Buffer" |
| 3 | **User control and freedom** | Undo, redo, escape. Users make mistakes. | Ctrl+Z, "Cancel" buttons, confirm before destructive actions |
| 4 | **Consistency and standards** | Same thing should look/work the same everywhere | Don't reinvent the back button, follow platform conventions |
| 5 | **Error prevention** | Prevent errors before they happen | Disable invalid buttons, date pickers vs free text, confirmations |
| 6 | **Recognition rather than recall** | Show options, don't make users remember | Dropdowns > free text, recent items, breadcrumbs |
| 7 | **Flexibility and efficiency of use** | Shortcuts for experts, simplicity for novices | Keyboard shortcuts, command palette, favorites |
| 8 | **Aesthetic and minimalist design** | Every extra element competes with relevant ones | Remove noise. If it doesn't help the task, it hurts. |
| 9 | **Help users recognize, diagnose, and recover from errors** | Error messages should explain what happened and what to do | "Email invalid. Example: user@domain.com" not "Error 422" |
| 10 | **Help and documentation** | Ideally not needed, but when needed: searchable and task-focused | Tooltips, contextual help, good docs |

---

## The 3 Most Violated (by developers)

1. **No feedback** — User clicks, nothing happens. Is it loading? Broken? They click again.
2. **Jargon** — Technical terms in user-facing messages. Users don't know what a "null reference" is.
3. **No undo** — Destructive actions without confirmation or recovery path.

---

## Quick Test

Before shipping any interface, ask:
- Can the user tell what state the system is in? (#1)
- Can the user recover from mistakes? (#3)
- Would a non-technical person understand every message? (#2, #9)

---

## Sources

- Nielsen, Jakob. "10 Usability Heuristics for User Interface Design" — nngroup.com (1994, updated 2020)
