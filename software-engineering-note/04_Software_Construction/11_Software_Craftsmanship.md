---
tags:
  - construction
  - craftsmanship
  - character
  - professionalism
  - code-complete
source: "McConnell, Steve. Code Complete (2nd Edition), Chapters 33–35"
---

# Software Craftsmanship

> **Source:** McConnell, *Code Complete* (2nd Ed.), Ch 33–35
> **Focus:** The personal character, themes, and continuous learning that distinguish superior programmers from the rest.

Programming is one of the most purely mental activities — your primary tool is *you*, and your raw material is human intellect. When software engineers study their tools, they're studying people: intellect, character, and habits. Study after study finds 10:1 differences in productivity, speed, error rate, and quality between programmers. Intelligence matters less than character.

---

## Ch 33 — Personal Character

### 33.1 Why Character Matters

Programming is essentially unsupervisable. No one really knows what you're working on moment to moment. Your employer can't force you to be a good programmer — you must choose to be one.

### 33.2 Intelligence and Humility

> *"The people who are best at programming are the people who realize how small their brains are."* — Edsger Dijkstra, "The Humble Programmer" (1972)

Nobody is really smart enough to program computers. The way you *focus* your intelligence matters more than how much you have. Humble programmers compensate for their fallibilities:

- **Decomposition** — breaks systems into understandable pieces
- **Reviews, inspections, tests** — augment your limited intellect with others' perspectives ("egoless programming")
- **Short routines** — reduce cognitive load per unit of code
- **Problem-domain terms** — work at higher abstraction, not implementation-level details
- **Conventions** — free your brain from arbitrary, low-payoff decisions

Humility isn't weakness; it's the engine of improvement. The more humble you are, the faster you'll improve.

### 33.3 Curiosity

Technical information changes every 5–10 years. Without curiosity, you become a dinosaur. Specific actions:

- **Build process awareness** — understand *how* software is developed, not just how to code
- **Experiment** — write short programs to test language features; make mistakes quickly and learn from them
- **Read about problem solving** — strategies can be taught; don't reinvent the square
- **Analyze before acting** — the pendulum is far on "acting"; pull it toward analysis
- **Study great programmers' work** — read their code, ask for their criticism, compare to your own
- **Read documentation** — RTFM. Skim library docs every couple of months
- **Read books and periodicals** — one good book every two months (~35 pages/week) distinguishes you from the mainstream
- **Affiliate with professionals** — conferences, user groups, online communities

**Professional Development Ladder:**

| Level | Description |
|-------|-------------|
| 1 — Beginning | Basic capabilities of one language (classes, routines, loops) |
| 2 — Introductory | Basic capabilities of multiple languages; comfortable in at least one |
| 3 — Competency | Deep expertise in a language/environment; many never move past this |
| 4 — Leadership | Recognizes programming is ~15% communicating with computer, ~85% communicating with people. Writes crystal-clear code for *people* first. |

The sin isn't being a beginner — it's staying one after you know how to improve.

### 33.4 Intellectual Honesty

Uncompromising intellectual honesty manifests as:

- Refusing to pretend expertise you don't have
- Readily admitting mistakes (not admitting them makes you look worse, not better)
- Actually understanding a compiler warning — not suppressing it
- Clearly understanding your program — not compiling just "to see if it works"
- Providing realistic status reports (not "90% done" for the last 50% of the project)
- Providing realistic estimates and **holding your ground** — estimates aren't negotiable, they're laws of nature

> "If management asks for an estimate and you know they'll reject an honest one, *don't lie*. Management's job is to decide whether the cost is worth it. Tricking them steals their authority and can cost hundreds of thousands of dollars."

### 33.5 Communication and Cooperation

Writing readable code is part of being a team player. Programming is communicating with *another programmer first*, and the computer second.

### 33.6 Creativity and Discipline

> "Form is liberating."

Discipline and conventions don't stifle creativity — they *focus* it. Great artists (Leonardo, Michelangelo) worked within self-imposed structures. Without conventions on large projects, completion itself is impossible and creativity unimaginable. Don't waste creative energy on arbitrary formatting decisions.

### 33.7 Laziness

Three kinds:

| Type | Description | Value |
|------|-------------|-------|
| **True laziness** | Deferring unpleasant tasks; compiling to avoid thinking | ❌ Harmful |
| **Enlightened laziness** | Doing unpleasant tasks quickly to get them out of the way | ✅ Good |
| **Long-term laziness** | Writing a tool so you never have to do the task again | ✅✅ Best |

Hustle ≠ productivity. Thinking is the most important work — and people don't look busy when they're thinking.

### 33.8 Characteristics That Don't Matter As Much As You Think

- **Persistence** — usually "pigheadedness." If stuck for 15 minutes, try an alternative approach, walk away, or rewrite from scratch. Fighting computer problems is no virtue; *avoiding* them is better.
- **Experience** — 10 years can mean 10 years of experience, or 1 year repeated 10 times. Information changes so fast that old habits can be worse than no habits. "We want 5 years of C experience" is silly — if you haven't learned it in 1–2 years, the next 3 won't help.
- **Gonzo programming** — all-nighters feel heroic but install defects you'll fix for weeks. Excitement is no substitute for competency.

### 33.9 Habits

> *"We are what we repeatedly do. Excellence, then, is not an act, but a habit."* — Aristotle

Most of what you do as a programmer is habit — formatting, error-checking patterns, readability vs. speed tradeoffs. You can't replace a bad habit with nothing; you must *substitute* a good one. Bill Gates: any programmer who will ever be good is good in the first few years. Learn it the right way the first time, while you're still actively thinking about it.

---

## Ch 34 — Themes in Software Craftsmanship

These themes account for the difference between hacking and software craftsmanship.

### 34.1 Conquer Complexity

Managing complexity is **Software's Primary Technical Imperative**. No one's brain spans nine orders of magnitude of detail. Intellectual tools for this:

- Divide systems into subsystems at the architecture level
- Carefully define class interfaces to hide internals
- Avoid global data, deep inheritance, deep nesting, gotos
- Keep routines short; use clear variable names
- Minimize routine parameters
- Use conventions to eliminate arbitrary differences
- Abstract complicated tests into boolean functions; use table lookups instead of complex logic chains
- **Abstraction** is the most powerful tool: from machine language → high-level languages → routines → classes → packages. Every step counts.

### 34.2 Pick Your Process

On solo projects, individual talent dominates. On teams, **organizational characteristics matter more than individual skills combined**. The process determines whether abilities add or subtract.

- **Requirements stability**: unstable requirements → poor design → degraded quality. Use incremental development to manage this.
- **Design foundation**: rush to coding = emotional investment in bad architecture. Hard to throw away after you've started building.
- **Quality must be built in from the first step**: testing only tells you what's defective; it won't make code more usable, faster, or more readable.
- **Premature optimization** is a process error: rough out the general shape before polishing individual features.

> *"Spend a part of your working day examining and refining your own methods."* — Robert W. Floyd

### 34.3 Write Programs for People First, Computers Second

The computer doesn't care whether your code is readable. Readable code improves: comprehensibility, reviewability, error rate, debugging, modifiability, development time, external quality.

- Readable code doesn't take longer to write (in the long run)
- Write-time convenience over read-time convenience is a **false economy** — you write once, others read many times
- **Private vs. Public programs** (Comer 1981): private programs can be sloppy; public programs must be readable, documented, reliable. But private programs often become public — write for that eventuality.
- 10 generations of maintenance programmers work on an average program before rewrite. They spend 50–60% of their time *understanding* the code.

### 34.4 Program into Your Language, Not in It

Don't limit your thinking to what the language supports automatically. The best programmers decide what they want to do, then figure out how to accomplish it with available tools.

- Language doesn't support assertions? Write your own `assert()` routine.
- No enumerated types? Use disciplined global variables + naming conventions.
- Avoid hazardous features (global data, gotos) even when the language allows them.
- Programming into the most obvious path = "programming *in* a language." Think about your technical goals first.

### 34.5 Focus Your Attention with the Help of Conventions

Conventions are intellectual tools for managing complexity:

- **Save arbitrary decisions** — answer formatting/commenting/naming questions once
- **Convey information concisely** — a single character differentiates local/class/global variables
- **Protect against known hazards** — prohibit global variables, require `NULL` after pointer deletion
- **Add predictability** — conventional error handling, memory management, class interfaces reduce uncertainty
- **Compensate for language weaknesses** — named constants via conventions in Python, Perl, shell

Balance: large projects tend to go *overboard* with conventions; small projects tend to go *underboard*.

### 34.6 Program in Terms of the Problem Domain

Work at the highest possible level of abstraction. Top-level code should describe the *problem*, not the implementation.

**Levels of Abstraction:**

| Level | Description |
|-------|-------------|
| 0 | OS operations & machine instructions |
| 1 | Language structures & tools (primitives, control structures, libraries) |
| 2 | Low-level implementation structures (stacks, queues, linked lists, trees) |
| 3 | Low-level problem-domain terms (business objects, services — the "glue" layer) |
| 4 | High-level problem-domain terms (readable by nontechnical customers) |

Implementation details should be hidden two layers below your working level. Use classes for problem-domain structures, hide low-level data types, use named constants, assign intermediate variables, and use boolean functions for complex tests.

### 34.7 Watch for Falling Rocks

Programming requires sensitivity to warning signs — subtle indications of problems:

- **"This is really tricky code"** = "This is bad code." Rewrite it.
- **Error-prone classes** — a class with more errors than average will continue to have more errors. Consider rewriting.
- **Excessive debugging** — lots of debugging means people aren't working smart. A good process eliminates most errors before testing.
- **Design metrics as heuristics**: >7 members in a class, >10 decision points in a routine, >3 levels of nesting, high coupling, low cohesion — none prove bad design, but all warrant skepticism.
- **Difficulty writing comments or naming variables** = the design isn't clear enough. When design is clear, low-level details come easily.
- **Repetitious modifications** — may indicate control hasn't been centralized.
- **Hard to create test scaffolding** — classes may be too tightly coupled.

> *"Doubt is an uneasy and dissatisfied state from which we struggle to free ourselves."* — Charles Saunders Peirce

**Create your own warning signs.** Set freed pointers to `NULL` so misuse causes obvious crashes. Fix compiler warnings — you can't notice subtle signs when ignoring ones labeled "WARNING."

### 34.8 Iterate, Repeatedly, Again and Again

Software development is inherently iterative. You don't get it right the first time:

- Requirements evolve as users see early versions
- Design improves through successive refinement — first attempt is rarely optimal
- Code quality improves through refactoring cycles
- Testing reveals gaps that prompt redesign

The key is to **plan for iteration** rather than fight it. Incremental and iterative development approaches (evolutionary prototyping, staged delivery, Agile) acknowledge this reality. Short iteration cycles produce faster feedback and better outcomes than long, linear phases.

Top-level design → detailed design → code → review → refactor → retest. Each pass deepens understanding. The first version is a rough draft; subsequent versions approach the ideal.

### 34.9 Thou Shalt Rend Software and Religion Asunder

Software development attracts dogma. Programmers debate methodologies, languages, and practices with religious fervor. McConnell's advice:

- **No single approach is universally best** — the right answer depends on context (project size, team, domain, constraints)
- **Beware of zealotry** — anyone claiming their way is the *only* way is selling ideology, not engineering
- **Use empirical evidence** — studies, data, and experience over rhetoric
- **Be pragmatic** — borrow what works from multiple paradigms
- **Separate the essential from the accidental** — focus on what genuinely reduces complexity and improves quality, not on stylistic preferences

Good software engineering is about *judgment*, not *doctrine*.

---

## Ch 35 — Where to Find More Information

The final chapter of *Code Complete* provides a curated reading plan and resources for continuous professional growth. A superior programmer never stops learning.

### 35.1 Information Sources

**Books and Periodicals:**
- *IEEE Software*, *Communications of the ACM*, *Dr. Dobb's Journal*
- Classic books: *The Mythical Man-Month* (Brooks), *The Psychology of Computer Programming* (Weinberg), *Software Project Survival Guide* (McConnell)
- Read broadly: requirements, design, construction, testing, management, human factors

**Websites and Online Communities:**
- SourceForge and open-source repositories — read real production code
- Professional forums, mailing lists, discussion groups
- Construx website for templates and coding conventions

**Conferences and User Groups:**
- Attend to network, learn new practices, stay current
- Local user groups provide regular peer interaction

### 35.2 A Software Developer's Reading Plan

McConnell advocates deliberate, continuous reading. Key recommendations:

- **Read at least one technical book every 2 months** (~35 pages/week)
- Mix deep books (study thoroughly) with survey books (skim for awareness)
- Cover multiple areas: construction, design, testing, management, process, human factors
- Build a personal library of go-to references — books you return to repeatedly

### 35.3 Self-Assessment and Growth

- Periodically assess your skills against frameworks like the Professional Development Ladder (Section 33.3)
- Seek code reviews from programmers you respect
- Keep a learning journal — record insights, mistakes, and resolutions
- Set concrete learning goals: "This year I'll master X technology / read Y books / contribute to Z open-source project"

### 35.4 Professional Organizations

- **IEEE Computer Society** — standards, publications, conferences
- **ACM (Association for Computing Machinery)** — journals, SIGs (Special Interest Groups)
- Membership provides access to research, networking, and continuing education

### 35.5 Key Message

The book closes by emphasizing that **great programmers are made, not born**. The difference between average and exceptional is:
- Continuous learning and curiosity
- Intellectual honesty and humility
- Disciplined practice of proven techniques
- Deliberate attention to process, not just product

---

## Summary: The Craftsmanship Mindset

| Trait | What It Means |
|-------|---------------|
| **Humility** | Acknowledge your brain's limits; compensate with process and collaboration |
| **Curiosity** | Never stop learning — read, experiment, ask for criticism |
| **Intellectual Honesty** | Admit mistakes, give realistic estimates, understand before compiling |
| **Discipline** | Conventions and structure *enable* creativity on large-scale work |
| **Communication** | Write code for people first; programming is a social act |
| **Laziness (the right kind)** | Automate drudgery; thinking beats hustle |
| **Complexity Management** | Abstraction, decomposition, conventions — Software's Primary Technical Imperative |
| **Process Consciousness** | Quality must be built in; process determines whether team abilities add or subtract |
| **Problem-Domain Focus** | Work at the highest abstraction level — code should read like the problem, not the implementation |
| **Pragmatism over Dogma** | Context determines best approach; borrow from multiple paradigms |
| **Continuous Growth** | Read, review, reflect, repeat — great programmers are made, not born |

---

## Key Quotes

> *"The purpose of many good programming practices is to reduce the load on your gray cells."*

> *"If you can't learn at your job, find a new one."*

> *"Programming is communicating with another programmer first and communicating with the computer second."*

> *"Form is liberating."*

> *"Readable code doesn't take any longer to write than confusing code does, at least not in the long run."*

> *"If it's hard, it's wrong. Make it simpler."*

> *"You can't negotiate laws of nature. You can, however, negotiate other aspects of the project that affect the schedule."*
