---
tags:
  - construction
  - design
  - heuristics
  - code-complete
  - complexity
  - abstraction
  - coupling
  - cohesion
  - information-hiding
source: "McConnell, Code Complete 2nd Edition, Chapter 5"
---

# Design in Construction

> *"Design is the activity that links requirements to coding and debugging."* — Steve McConnell

Design isn't just an upstream prerequisite — on small projects, much of it happens *during* construction. On larger projects, even when formal architecture exists, the programmer still designs significant portions. Recognizing design as an explicit activity maximizes its benefit regardless of project size.

---

## 5.1 Design Challenges

Design is inherently difficult. Understanding *why* helps you approach it with the right mindset.

### Design Is a Wicked Problem

A **wicked problem** (Rittel & Webber, 1973) is one that can be clearly defined only by solving it — or by solving part of it. You essentially have to solve the problem once to define it, then solve it again to create a working solution.

> **Tacoma Narrows Bridge (1940):** Engineers didn't know aerodynamics mattered until wind-induced harmonic ripples collapsed the bridge. Only by building it (solving the problem) did they discover the additional consideration needed.

School assignments move you in a beeline from start to finish. Professional programming is the opposite — requirements change mid-design, again mid-coding, and again before delivery.

### Design Is a Sloppy Process (Even If the Result Is Tidy)

The finished design looks clean, but the path to it is messy:
- You take many false steps and go down blind alleys
- Making mistakes is *the point* of design — it's cheaper to make and correct design mistakes than code mistakes
- A good solution is often only subtly different from a poor one
- The most common answer to "When are you done designing?" is **"When you're out of time."**

### Design Is About Tradeoffs and Priorities

In the real world, a designer must weigh competing characteristics and strike a balance. Fast response vs. minimized development time — different priorities yield different designs.

### Design Involves Restrictions

The point of design is partly to **create** possibilities and partly to **restrict** them. Constraints force simplifications that ultimately improve the solution. Without deliberately imposed restrictions, software sprawls uncontrollably.

### Design Is Nondeterministic

Three designers given the same problem will produce three vastly different, equally acceptable designs. There are usually *dozens* of ways to design a program.

### Design Is a Heuristic Process

Because design is nondeterministic, techniques are **heuristics** ("rules of thumb") rather than repeatable processes guaranteeing predictable results. Design involves trial and error. No tool is right for everything.

### Design Is Emergent

Designs don't spring fully formed from someone's brain. They **evolve and improve** through design reviews, informal discussions, experience writing the code, and experience revising it.

---

## 5.2 Key Design Concepts

### Software's Primary Technical Imperative: Managing Complexity

Fred Brooks's "No Silver Bullets" (1987) distinguishes two classes of difficulty:

| Type | Definition | Status |
|------|-----------|--------|
| **Essential** | Properties a thing *must* have to be that thing. The inherent intricacy of the real-world problem. | Remains hard — no silver bullet |
| **Accidental** | Properties that just happen to be. Clumsy syntax, noninteractive computers, poorly integrated tools. | Largely solved over time |

**The root of all essential difficulties is complexity.** Dijkstra observed that computing spans from a bit to hundreds of megabytes — a ratio of 1:10^9 (now closer to 1:10^15). No one's skull is big enough to hold a modern program.

> **Software's Primary Technical Imperative: Manage complexity.**

The goal of all software design techniques is to break a complicated problem into simple, independent pieces. The more independent the subsystems, the safer it is to focus on one thing at a time.

#### How to Attack Complexity

Overly costly designs arise from three sources:
1. A complex solution to a simple problem
2. A simple, incorrect solution to a complex problem
3. An inappropriate, complex solution to a complex problem

**Two-prong approach:**
- Minimize the amount of *essential* complexity anyone has to deal with at any one time
- Keep *accidental* complexity from needlessly proliferating

---

### Desirable Characteristics of a Design

| Characteristic | Description |
|---------------|-------------|
| **Minimal Complexity** | Avoid "clever" designs. Make simple, easy-to-understand designs. If you can't safely ignore other parts of the program while working on one part, the design isn't doing its job. |
| **Ease of Maintenance** | Design for the maintenance programmer as your audience. Make the system self-explanatory. |
| **Loose Coupling** | Hold connections among different parts to a minimum. Use abstraction, encapsulation, and information hiding. |
| **Extensibility** | Enhance the system without violence to the underlying structure. The most likely changes cause the least trauma. |
| **Reusability** | Design pieces so they can be reused in other systems. |
| **High Fan-In** | Many classes use a given utility class — good use of lower-level utilities. |
| **Low-to-Medium Fan-Out** | A given class uses a low-to-medium number of other classes (< ~7). High fan-out indicates excessive complexity. |
| **Portability** | Easy to move to another environment. |
| **Leanness** | No extra parts. *"A book is finished not when nothing more can be added but when nothing more can be taken away"* (Voltaire). |
| **Stratification** | Keep levels of decomposition stratified so you can view the system at any single level and get a consistent view. |
| **Standard Techniques** | Use standardized, common approaches. A system relying on exotic pieces intimidates newcomers. |

---

### Levels of Design

Design happens at five distinct levels of granularity:

```
Level 1: Software System           ← Entire system
Level 2: Subsystems / Packages     ← Major partitions (UI, DB, business rules, etc.)
Level 3: Classes                   ← Individual classes within packages
Level 4: Routines                  ← Methods/functions within classes
Level 5: Internal Routine Design   ← Pseudocode, algorithms, control flow
```

#### Level 1: Software System
The entire system. Don't jump straight to classes — think through higher-level combinations first.

#### Level 2: Division into Subsystems or Packages
Identify all major subsystems (database, user interface, business rules, command interpreter, report engine, etc.) and define how each can communicate with others.

**Critical rule:** Restrict intersubsystem communication. Without rules, entropy increases and every subsystem communicates with every other — defeating the purpose of partitioning.

| Relationship Type | Coupling Level |
|-------------------|----------------|
| One subsystem calls routines in another | Simplest — preferred |
| One subsystem contains classes from another | More involved |
| Classes in one subsystem inherit from classes in another | Most involved — use sparingly |

> **A system-level diagram should be an acyclic graph.** No circular relationships.

**Common subsystems:** Business rules, User interface, Database access, System dependencies (OS wrapper).

#### Level 3: Division into Classes
Identify all classes in the system. Define each class's interface. Key distinction:

| Class | Object |
|-------|--------|
| Static — what you see in the program listing | Dynamic — specific instance at runtime |
| The cookie cutter | The cookie |

#### Level 4: Division into Routines
Divide each class into routines. The class interface defines public routines; this level details private routines. Often clarifies the class interface, causing iterative changes back at Level 3.

#### Level 5: Internal Routine Design
Writing pseudocode, looking up algorithms, organizing paragraphs of code. Always done — but sometimes unconsciously and poorly rather than consciously and well.

---

## 5.3 Design Building Blocks: Heuristics

Since design is nondeterministic, skillful application of heuristics is the core activity. Each heuristic is described in terms of the Primary Technical Imperative: **managing complexity**.

### Major Heuristics

#### 1. Find Real-World Objects
The "by the book" object-oriented approach:
1. Identify objects and their attributes (methods and data)
2. Determine what can be done to each object
3. Determine what each object is allowed to do to other objects (containment, inheritance)
4. Determine public vs. private parts of each object
5. Define each object's public (and protected) interface

Iterate on both the top-level organization and the details of each class.

#### 2. Form Consistent Abstractions
**Abstraction** = engaging with a concept while safely ignoring some of its details. Handle different details at different levels.

| Level | Abstraction |
|-------|-------------|
| Routine interface | Doorknob level |
| Class interface | Door level |
| Package interface | House level |

> Good programmers create abstractions at *all* levels. Without them, you're building at the "wood-fiber and varnish-molecule" level — overly complex and intellectually unmanageable.

#### 3. Encapsulate Implementation Details
**Abstraction** says: "You're *allowed* to look at a high level."
**Encapsulation** says: "You're *not allowed* to look at any other level."

Encapsulation manages complexity by *forbidding* you to look at the complexity. A class interface is like an iceberg: seven-eighths is underwater and invisible.

#### 4. Inherit — When Inheritance Simplifies the Design
Inheritance works synergistically with abstraction. Define general types, then specialize. Write general routines for general properties, specific routines for specific cases.

> **Polymorphism** = the ability to call `Open()` or `Close()` without knowing until runtime what kind of object you're dealing with.

**Caution:** Inheritance is powerful when used well and damaging when used naively.

#### 5. Hide Secrets (Information Hiding)
*David Parnas, 1972* — one of the seminal ideas in software development. Each class (or package or routine) is characterized by the **design and implementation decisions it hides** from everything else.

> Fred Brooks (1995): "Parnas was right, and I was wrong about information hiding."

**The class's job:** Keep its secrets hidden. Minor changes might affect several routines within a class but must not ripple beyond the class interface.

##### Two Categories of Secrets

| Category | Examples |
|----------|----------|
| **Hiding complexity** | Complicated data types, file structures, boolean tests, involved algorithms |
| **Hiding sources of change** | Areas likely to change — isolate so changes are localized |

##### Example: Unique ID Generation

| Approach | Problem |
|----------|---------|
| `id = ++g_maxId` scattered everywhere | Changing ID allocation strategy requires touching every instance. Not thread-safe. |
| `id = NewId()` centralized | Change happens in one place. |
| `int id` declared everywhere | Changing ID type to string requires hundreds of changes. |
| `IdType id` (typedef/class) | Type change happens in one place. |

##### Barriers to Information Hiding
- **Excessive distribution of information** — hard-coded literals, interleaved user interaction, global data accessed directly
- **Circular dependencies** — Class A calls B, B calls A. Makes testing impossible until both are ready.
- **Class data mistaken for global data** — Class data in small, well-designed classes avoids global-data problems
- **Perceived performance penalties** — Premature optimization. Create a modular design first; optimize hot spots later.

##### Proven Value
> Large programs using information hiding were found to be easier to modify **by a factor of 4** than programs that don't (Korson & Vaishnavi, 1986).

> **Key habit:** Get into the habit of asking *"What should I hide?"* You'll be surprised at how many difficult design issues dissolve.

#### 6. Identify Areas Likely to Change
Great designers share one attribute: the ability to **anticipate change** (Glass, 1995).

1. **Identify** items likely to change
2. **Separate** volatile components into their own classes
3. **Isolate** — design interfaces insensitive to changes. Changes stay inside the class.

**Areas likely to change on any project:**

| Area | Strategy |
|------|----------|
| **Business rules** | Hide in a single dark corner. Use table-driven methods. |
| **Hardware dependencies** | Isolate in own subsystem. Use simulator during unstable hardware phases. |
| **Input and output** | File formats, field positions, screen layouts will change. Examine all external interfaces. |
| **Nonstandard language features** | Hide behind your own interface. Replace when moving environments. |
| **Difficult design/construction areas** | Compartmentalize. Minimize impact of poor design on rest of system. |
| **Status variables** | Use enumerated types instead of booleans. Use access routines instead of direct checks. |
| **Data-size constraints** | Hide sizes behind named constants (`MAX_EMPLOYEES` instead of literal `100`). |

> **Proportionality principle:** Design so the effect or scope of change is proportional to the chance that the change will occur.

#### 7. Keep Coupling Loose
**Coupling** = how tightly a class or routine is related to others. Goal: small, direct, visible, and flexible relationships.

> Make modules detached, like business associates, not attached, like Siamese twins.

##### Coupling Criteria

| Criterion | Good | Bad |
|-----------|------|-----|
| **Size** | Few connections. Small interface (1 parameter, 4 public methods). | Many connections. Large interface. |
| **Visibility** | Obvious connections. Parameter passing. | Sneaky connections. Modifying global data. |
| **Flexibility** | Easy to reconnect. Like a USB connector. | Hard-coded dependencies. Like bare wire and solder. |

##### Kinds of Coupling

| Kind | Description | Verdict |
|------|-------------|---------|
| **Simple-data-parameter** | Only primitive types passed through parameter lists | ✅ Normal and acceptable |
| **Simple-object** | Module instantiates an object | ✅ Fine |
| **Object-parameter** | Object1 requires Object2 to pass Object3 | ⚠️ Tighter — Object2 must know about Object3 |
| **Semantic** | One module uses semantic knowledge of another's inner workings | ❌ Most insidious — breaks in undetectable ways |

**Semantic coupling examples:**
- Passing control flags that assume internal workings
- Using global data modified by another module at "the right time"
- Skipping `Initialize()` because you "know" `Routine()` calls it anyway
- Partially initializing an object because you "know" which methods will be called
- Casting a base object to a derived type because you "know" what was really passed

> Semantic coupling turns debugging into a Sisyphean task — changes in the used module break the using module in ways completely undetectable by the compiler.

#### 8. Look for Common Design Patterns
Patterns provide ready-made abstractions for common problems (Gamma et al., 1995).

**Benefits:**

| Benefit | Description |
|---------|-------------|
| **Reduce complexity** | Ready-made abstractions. "Factory Method" communicates a rich set of relationships in two words. |
| **Reduce errors** | Institutionalize details of common solutions — embody years of accumulated wisdom and corrections. |
| **Heuristic value** | Allow cycling through familiar alternatives vs. creating from whole cloth. |
| **Streamline communication** | Move design dialog to higher level of granularity. |

**Common patterns:**

| Pattern | Essence |
|---------|---------|
| **Abstract Factory** | Create sets of related objects without specifying concrete types |
| **Adapter** | Convert one interface to another |
| **Bridge** | Separate interface from implementation so both can vary independently |
| **Composite** | Treat individual objects and compositions uniformly |
| **Decorator** | Attach responsibilities dynamically without subclassing |
| **Facade** | Provide a unified interface to a set of interfaces |
| **Factory Method** | Instantiate derived classes without tracking them everywhere |
| **Iterator** | Sequential access to elements of a collection |
| **Observer** | Keep multiple objects in sync via notifications |
| **Singleton** | Global access to exactly one instance |
| **Strategy** | Interchangeable algorithms/behaviors |
| **Template Method** | Define algorithm skeleton, let subclasses fill in details |

**Traps:**
- Force-fitting code to match a pattern can *increase* complexity
- Feature-itis: using a pattern to try it out, not because it's appropriate

---

### Other Heuristics (Sometimes Useful)

| Heuristic | Key Idea |
|-----------|----------|
| **Aim for Strong Cohesion** | All routines in a class support a central purpose. The more focused, the easier to remember what the code does. |
| **Build Hierarchies** | Tiered structures let you focus on only the level of detail you currently need. Aristotle used them; humans naturally organize this way. |
| **Formalize Class Contracts** | Each interface is a contract: preconditions (what clients promise) and postconditions (what the class promises). |
| **Assign Responsibilities** | Ask what each object should be responsible for — broader than "what should it hide?" |
| **Design for Test** | Design to facilitate testing → more formalized interfaces, minimized dependencies. |
| **Avoid Failure** | Study failure modes, not just successes. Many bridge collapses came from copying successes without considering failure. |
| **Choose Binding Time Consciously** | Earlier binding = simpler; later binding = more flexible. Ask "What if I bound this earlier/later?" |
| **Make Central Points of Control** | One Right Place for any nontrivial piece of code; One Right Place to make a likely change. |
| **Consider Using Brute Force** | A brute-force solution that works > an elegant solution that doesn't. |
| **Draw a Diagram** | Pictures represent problems at higher levels of abstraction. |
| **Keep Your Design Modular** | Each routine/class as a black box: known inputs, known outputs, unknown internals. |

---

### Guidelines for Using Heuristics

G. Polya's approach to mathematical problem solving (1957) applies to software design:

```
1. UNDERSTAND THE PROBLEM — What is the unknown? What are the data?
   What is the condition? Draw a figure. Separate the parts.

2. DEVISE A PLAN — Have you seen this before? A related problem?
   Can you restate it? Solve a simpler version first.

3. CARRY OUT THE PLAN — Check each step. Can you prove it's correct?

4. LOOK BACK — Can you check the result? Derive it differently?
   Can you use the result or method for another problem?
```

> **Don't get stuck on a single approach.** If UML isn't working, write English. Try a brute-force solution. Walk away and come back. You don't have to solve everything at once — leaving issues unresolved until you have more information becomes natural with experience.

---

## 5.4 Design Practices

### Iterate
Design is an iterative process. You go from A to B and back to A. The tug and pull between high-level and low-level considerations creates a *stressed structure* more stable than one built purely top-down or bottom-up.

> The second design attempt is nearly always better than the first. After 1,000 failed filament materials, Edison said: "I have discovered a thousand things that don't work."

### Divide and Conquer
No one's skull is big enough for all the details of a complex program. Divide into areas of concern, tackle each individually. If you hit a dead end, iterate.

### Top-Down and Bottom-Up Design

| Approach | Starts With | Works Toward |
|----------|-------------|--------------|
| **Top-Down** | High-level abstractions, base classes | Increasing detail, derived classes |
| **Bottom-Up** | Concrete objects, specifics | Generalizations, aggregations, base classes |

Both have merit. The tension between them creates better designs.

### Experimental Prototyping
When you can't decide among design alternatives, write the minimal code needed to answer the specific question. Prototyping is *design work*, not production code — you can and should throw it away once the question is answered.

### Collaborative Design
- **Pair design:** Two minds produce fewer blind spots than one.
- **Design reviews / inspections:** Formal or informal, having other people look at your design catches issues early.
- **Whiteboard sessions:** Walk through the design with a colleague. Explaining it out loud often reveals flaws.

### How Much Design Is Enough?

> *"When you're out of time"* is common — but there's a better answer.

| Factor | More Design Needed | Less Design Needed |
|--------|--------------------|--------------------|
| **Project size** | Large, many people | Small, solo |
| **Complexity** | High algorithmic/domain complexity | Well-understood domain |
| **Team experience** | Junior team | Senior team, already solved similar problems |
| **Requirements stability** | Volatile (design isolates change) | Stable |
| **Risk tolerance** | High-reliability, safety-critical | Low stakes, prototype |
| **Life expectancy** | Long-lived, many versions | Short-lived, throwaway |

A good rule of thumb: spend about **10-15% of total project effort on design** before construction, doing further design during construction as needed.

### Capturing Design Work

| Medium | Best For |
|--------|----------|
| **Whiteboard photos** | Quick capture during brainstorming |
| **UML diagrams** (class, sequence, state) | Formal communication of structure and behavior |
| **CRC cards** (Class-Responsibility-Collaborator) | Exploring object interactions physically |
| **Design documents** (wikis, markdown) | Rationale, tradeoffs, decisions |
| **Code itself** | The most precise design specification. The design lives in the code. |
| **Design patterns by name** | Communicating intent compactly |

> The design isn't done until it's communicated. If it's only in your head, it doesn't count.

---

## 5.5 Comments on Popular Methodologies

McConnell briefly discusses several approaches, noting that methodology debates often miss the point — **good design comes from applying heuristics effectively, not from following a methodology rigidly.**

| Methodology / Approach | Essence |
|------------------------|---------|
| **Structured Design** | Top-down functional decomposition. Data flow diagrams, structure charts. Still valuable for certain problem types. |
| **Object-Oriented Design** | Model the world as interacting objects. Encapsulation, inheritance, polymorphism. Dominant paradigm. |
| **Design Patterns** | Catalog of proven solutions to recurring problems. Language for design communication. |
| **Agile / Evolutionary Design** | Design emerges incrementally. Refactor continuously. YAGNI (You Ain't Gonna Need It). |
| **Design by Contract** | Formal preconditions, postconditions, invariants. Bertrand Meyer, Eiffel. |
| **Aspect-Oriented Design** | Separate cross-cutting concerns (logging, security) from core logic. |

> The methodology matters less than the **designer's skill** in applying heuristics. Good designers adapt their approach to the problem, not the other way around.

---

## Key Takeaways

1. **Design is a wicked, sloppy, nondeterministic, heuristic, emergent process.** Accepting this reality is the first step to doing it well.

2. **Software's Primary Technical Imperative is managing complexity.** Every design decision should be evaluated against this standard. Dijkstra's point stands: no one's skull is big enough — organize programs so you can safely focus on one part at a time.

3. **Information hiding is the most powerful design heuristic.** Proven to make programs easier to modify by a factor of 4. Ask *"What should I hide?"* at every level.

4. **Good designs exhibit: minimal complexity, loose coupling, strong cohesion, extensibility, leanness, and stratification.**

5. **Design at multiple levels:** System → Subsystems → Classes → Routines → Internal routine design. Iterate across levels.

6. **Anticipate change.** Identify volatile areas (business rules, hardware, I/O, nonstandard features) and isolate them behind stable interfaces.

7. **Loose coupling with visible connections.** Semantic coupling is the most insidious — it breaks in ways undetectable by the compiler.

8. **Use patterns, but don't force-fit.** Patterns reduce complexity, errors, and communication overhead. But forcing code to fit a pattern can increase complexity.

9. **Iterate.** The second design is nearly always better than the first. Don't try to solve everything at once — leave issues unresolved until you have more information.

10. **Capture the design.** If it's only in your head, it doesn't count. Use diagrams, documents, pattern names, and the code itself.

---

## Related Notes

- [[Software Construction Overview|Software Construction Overview]]
- Working Classes (Ch 6)
- High-Quality Routines (Ch 7)
- Defensive Programming (Ch 8)
- Refactoring (Ch 24)
- [[../03_Software_Design/|Software Design]]
