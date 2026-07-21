---
tags:
  - legacy-code
  - refactoring
  - extract-class
  - monster-methods
  - software-maintenance
source: "Feathers, M. — Working Effectively with Legacy Code, Chapters 19–24"
created: 2026-07-21
---

# Large-Scale Changes in Legacy Code

> **Source:** Michael Feathers, *Working Effectively with Legacy Code*, Chapters 19–24
> **Topics:** Extract Class, changing same code everywhere, monster methods, hyperaware editing, overwhelmed teams, not breaking anything.

---

## Chapter 19 — My Project Is Not Object Oriented. How Do I Make Safe Changes?

- Even procedural code systems are essentially "one big object." Using **Encapsulate Global References** subdivides the system for easier testing.
- Languages with OO extensions let you incrementally move toward better object design — group related functions into classes, extract methods to break apart tangled responsibilities.
- **Seams matter.** Object seams are good for more than just testing; link and preprocessing seams get code under test but don't improve design beyond that.

---

## Chapter 20 — This Class Is Too Big and I Don't Want It to Get Any Bigger

### Problems with Big Classes
1. **Confusion** — 50–60 methods make it hard to know what to change and what's affected.
2. **Task scheduling** — multiple programmers modifying the same class concurrently leads to merge thrashing.
3. **Testing pain** — over-encapsulation hides too much; testers resort to Edit and Pray.

### Key Tactics
- **Sprout Class** and **Sprout Method** keep new code out of the big class.
- Refactoring is the remedy; the hard part is knowing what the smaller classes should look like.

### Single-Responsibility Principle (SRP)
> Every class should have a single purpose and only one reason to change.

A class can violate SRP at two levels:
- **Interface level** — the public API presents too many unrelated methods.
- **Implementation level** — the class actually does all the work internally (vs. delegating).

**Interface Segregation Principle (ISP):** When a class is large, create focused interfaces for different client groups. This hides information and decreases dependencies.

### Heuristics for Seeing Responsibilities

| # | Heuristic | Description |
|---|-----------|-------------|
| 1 | **Group Methods** | Write down all methods with access types; find natural groupings. Great as a team exercise (poster boards). |
| 2 | **Look at Hidden Methods** | Many private/protected methods signal a separate class trying to get out. Private methods you want to test belong on another class. |
| 3 | **Look for Decisions That Can Change** | Identify hard-coded assumptions (database, API, etc.). Extract methods to encapsulate these before extracting classes. |
| 4 | **Look for Internal Relationships** | Use **feature sketches** — draw circles for instance variables, lines for method usage. Find clusters ("lumps") and extract them at pinch points. |
| 5 | **Look for the Primary Responsibility** | Describe the class in one sentence. If you need "and… and… and…", factor out the extras. |
| 6 | **When All Else Fails, Do Scratch Refactoring** | Experiment artificially to discover structure; what you see during scratch isn't necessarily final. |
| 7 | **Focus on the Current Work** | The change you're making right now tells you which responsibility matters most. |

### Strategy and Tactics

- **Strategy:** Identify responsibilities, share understanding with the team, break down classes on an **as-needed basis** (spread risk, keep shipping).
- **Tactics:** Start with implementation-level SRP — extract classes and delegate. Interface-level SRP requires changing clients (needs tests).
- **After Extract Class:** Don't get overambitious. The current structure *works*. Move in a better direction, not necessarily toward the ideal design.

### Safe Extract Class Without Tests
1. Identify the responsibility to separate.
2. Move affected instance variables to a separate section of the class.
3. Extract method bodies with a unique `MOVING` prefix; Preserve Signatures.
4. Use the prefix for any partial methods that should move.
5. **Manually search** for variable uses (don't Lean on the Compiler — shadowing can hide usage).
6. Move variables and methods to the new class; create an instance in the old class.
7. After the move compiles, remove the `MOVING` prefix.

> ⚠️ **Inheritance pitfalls:** Moving a method that overrides a base class method silently changes behavior. Use the body-extraction approach (create new methods, don't move originals).

---

## Chapter 21 — I'm Changing the Same Code All Over the Place

### The Power of Removing Duplication

A step-by-step case study using `AddEmployeeCmd` and `LoginCommand`:

1. **Start small** — extract tiny pieces of duplication (e.g., writing a null-terminated string → `writeField`).
2. **Create a superclass** — pull shared data (header, footer, constants) and shared methods up.
3. **Abstract differences** — use abstract methods for what varies (e.g., `getCommandChar`, `writeBody`).
4. **Generalize further** — replace per-subclass field lists with a `List fields` in the base class.
5. **Result:** Subclasses become thin shells. Shared logic lives in one place.

### Key Insights

- **Orthogonality:** One knob per behavior. Changing how fields are written requires editing only one method.
- **Design emerges** from zealously removing duplication — you don't plan the knobs; they just happen.
- **Open/Closed Principle:** Code should be open for extension but closed for modification. Duplication removal naturally leads here.
- **Consistent naming matters** — avoid abbreviation chaos (e.g., `Mgr` vs. `Mngr`).

> The order of refactoring steps doesn't matter much. Start small, pay attention to names, and the big picture clarifies as you go.

---

## Chapter 22 — I Need to Change a Monster Method and I Can't Write Tests for It

### Varieties of Monster Methods

| Type | Description |
|------|-------------|
| **Bulleted Method** | Nearly no indentation; sequence of code chunks like a bulleted list. Sections may share temporary variables. |
| **Snarled Method** | Dominated by one large, deeply indented block. "If you start to feel vertigo, you've run into a snarled method." |

### With Automated Refactoring Support
- Let the tool do all structural changes; avoid manual edits (reordering, renaming).
- Goals: (1) separate logic from awkward dependencies, (2) introduce seams for testing.
- Coarse safe extractions first; handle details after tests are in place.

### Manual Refactoring Techniques

#### 1. Introduce Sensing Variable
Add a temporary instance variable (e.g., `nodeAdded = true`) to sense conditions inside the monster method. Write targeted tests against the sensing variable, extract with confidence, then remove the variable and tests.

#### 2. Extract What You Know
Extract small chunks (2–5 lines) that you can name confidently. Key metric: **coupling count** — the number of values passing in and out.
- Zero-count methods (no params, no return) are safest — they're just commands.
- Avoid large extractions; low coupling count means fewer type-conversion errors.

#### 3. Gleaning Dependencies
Write tests for the **critical** behavior. Extract the **non-critical** (e.g., display) code without tests. Accept that not all behaviors are equally important.

#### 4. Break Out a Method Object
Create a class whose sole responsibility is the monster method's work. Parameters become constructor args; local variables become instance variables. This lets you sense through production variables and keep tests.

### Strategy: Skeletonize vs. Find Sequences
- **Skeletonize:** Extract condition and body *separately* — leaves control structure visible, easier to reorganize.
- **Find Sequences:** Extract condition and body *together* — reveals overarching sequences of operations.
- You'll oscillate between both. Bulleted methods → find sequences. Snarled methods → skeletonize.

### Additional Rules
- **Extract to the current class first** — even if the name is awkward (e.g., `recalculateOrder`). Move to the right class later.
- **Extract small pieces** — they compound into insight.
- **Be prepared to redo extractions** — undoing and re-extracting isn't wasted effort; it's gained design insight.

---

## Chapter 23 — How Do I Know That I'm Not Breaking Anything?

### Hyperaware Editing
Every keystroke either changes behavior or doesn't. Knowing *exactly* what each edit does is the heart of safe programming. Tests and pair programming create this hyperaware flow state — it's not exhausting, it's *refreshing*. Programming without feedback is what's truly draining.

### Single-Goal Editing
> "Programming is the art of doing one thing at a time."

- Resist the urge to "clean this up too" while making a change. Write it down; finish one edit first.
- When pairing, constantly ask: "What are you doing?" If the answer is more than one thing, pick one.
- Thrashing (jumping between edits) is slower and more error-prone than deliberate single-goal work.

### Preserve Signatures
When breaking dependencies without tests, **don't change method signatures.** Copy/paste argument lists verbatim:
1. Copy the entire argument list.
2. Declare the new method with empty parens.
3. Paste args into the declaration.
4. Type the call with empty parens.
5. Paste args into the call.
6. Delete types, leaving only argument names.

This mechanical approach minimizes signature errors during dependency-breaking refactorings.

### Lean on the Compiler
In statically typed languages, let the compiler find change sites:
1. Alter a declaration to cause compile errors (e.g., wrap globals in a class).
2. Navigate to each error and fix it.

> ⚠️ **Inheritance trap:** Commenting out a method in a subclass may silently fall back to a superclass implementation. The compiler won't catch this. Know your language's limits.

### Pair Programming
Essential for dependency-breaking work. Legacy code is surgery — surgeons never operate alone.

---

## Chapter 24 — We Feel Overwhelmed. It Isn't Going to Get Any Better.

### Finding Motivation
- Legacy code work is hard. Connect with what makes programming *fun* for you — mastery, learning, doing neat things with any system.
- Green-field projects have their own problems. The classic trap: best people move to "the replacement system," old system stagnates, new system inherits unknown complexity.

### Practical Mindset
- Accept that the situation won't magically improve — but your skills and the codebase *can* improve incrementally.
- Every small refactoring, every test added, every dependency broken makes the system a little better.
- The techniques in this book are designed precisely for this reality: you don't need permission or a green-field project to start improving things today.

---

## Cross-Chapter Themes

| Theme | Chapters |
|-------|----------|
| Break down big things into small pieces | 20, 22 |
| Duplication removal drives design | 21 |
| Tests enable confident refactoring | 20, 22, 23 |
| Mechanical safety (preserve signatures, lean on compiler) | 23 |
| One thing at a time | 22, 23 |
| Incremental improvement over big rewrites | 20, 24 |
| Pairing and collaboration | 23, 24 |


## Related

- [[Software Maintenance Overview]] — All maintenance topics
- [[04_Getting_Tests_in_Place]] — Characterization tests
- [[06_Dependency_Breaking_Catalog]] — Dependency breaking techniques
