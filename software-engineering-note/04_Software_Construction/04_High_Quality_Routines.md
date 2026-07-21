---
tags:
  - construction
  - routines
  - defensive-programming
  - pseudocode
  - code-complete
source: "McConnell, Code Complete 2nd Ed., Chapters 7-9"
created: 2026-07-21
---

# 04 — High-Quality Routines

> *"Aside from the computer itself, the routine is the single greatest invention in computer science."* — Steve McConnell

---

## Ch 7: High-Quality Routines

### 7.1 Valid Reasons to Create a Routine

| Reason | Description |
|--------|-------------|
| **Reduce complexity** | The single most important reason. Hide information so you don't need to think about it after the routine is written. |
| **Introduce an intermediate abstraction** | Replace a block of inline code with a well-named routine — the name itself documents the intent. |
| **Avoid duplicate code** | The classic reason. Pull common code into one place for easier modification and fewer errors. |
| **Support subclassing** | Short, well-factored routines are easier to override correctly. |
| **Hide sequences** | Encapsulate ordering dependencies (e.g., `PopStack()` hides the read-then-decrement sequence). |
| **Hide pointer operations** | Isolate error-prone pointer manipulation. |
| **Improve portability** | Isolate non-portable capabilities (hardware, OS, language extensions). |
| **Simplify complicated boolean tests** | Put a complex condition into a named function so the calling code reads naturally. |
| **Improve performance** | Optimize in one place; profile and replace algorithms centrally. |
| **Isolate complexity** | Confine tricky algorithms, large data sets, or intricate protocols. |
| **Hide implementation details** | Whether complex (database access) or mundane (number vs. string). |
| **Limit effects of changes** | Isolate areas likely to change — hardware, I/O, business rules. |
| **Hide global data** | Access global data through routines rather than directly. |
| **Make central points of control** | One class/routine controls each task — devices, files, DB connections. |
| **Facilitate reusable code** | Well-factored routines are easier to reuse across programs. |
| **Accomplish a specific refactoring** | Many refactorings produce new routines (extract method, etc.). |

> **Key insight:** Small routines are worth creating even for 2–3 lines of code. They improve readability and tend to grow under maintenance (a one-liner that needs error handling becomes much easier to fix in one place).

---

### 7.2 Design at the Routine Level: Cohesion

Cohesion = how closely the operations in a routine are related. The goal: **each routine does one thing well and nothing else.**

| Level | Description | Verdict |
|-------|-------------|---------|
| **Functional** | Performs one and only one operation. e.g., `sin()`, `GetCustomerName()` | ✅ **Best** |
| **Sequential** | Operations must be done in order, sharing data step to step, but don't form a complete function together | ⚠️ Acceptable; split into separate routines where practical |
| **Communicational** | Operations use the same data but aren't otherwise related | ⚠️ Weak; refactor to separate routines |
| **Temporal** | Operations combined because they all happen at the same time, e.g., `Startup()`, `Shutdown()` | ⚠️ Acceptable if it *orchestrates* other routines rather than doing the work itself |
| **Procedural** | Operations done in a specified order for no reason other than matching input sequence | ❌ Unacceptable |
| **Logical** | Several operations stuffed into one routine, selected by a control flag (big `if`/`case`) | ❌ Unacceptable (except for pure event-handler dispatch) |
| **Coincidental** | No discernible relationship between operations | ❌ Worst — redesign |

**Hard data:** 50% of highly cohesive routines were fault-free vs. only 18% of low-cohesion routines. Low-cohesion routines had **7× more errors** and were **20× more costly to fix**.

---

### 7.3 Good Routine Names

> *If you have routines with side effects, you'll have many long, silly names. The cure is not less-descriptive names; the cure is to eliminate side effects.*

**Rules for naming:**

1. **Describe everything the routine does** — all outputs and side effects.
2. **Avoid meaningless verbs** — `HandleCalculation()`, `PerformServices()`, `ProcessInput()`, `DealWithOutput()` say nothing. Use `FormatAndPrintOutput()` instead.
3. **Don't differentiate solely by number** — `Part1`, `Part2` or `OutputUser1`, `OutputUser2` are worthless names.
4. **Make names as long as necessary** — clarity trumps brevity. Routine names tend to be longer than variable names.
5. **For functions:** name for the return value — `cos()`, `customerId.Next()`, `printer.IsReady()`.
6. **For procedures:** use a strong verb + object — `PrintDocument()`, `CalcMonthlyRevenues()`, `CheckOrderInfo()`. In OOP, the object is implied: `document.Print()`, not `document.PrintDocument()`.
7. **Use opposites precisely** — `first/last`, `open/close`, `add/remove`, `create/destroy`, `get/set`, `start/stop`, `begin/end`, `show/hide`.
8. **Establish conventions for common operations** — pick one pattern for getting IDs and stick with it project-wide.

---

### 7.4 How Long Can a Routine Be?

**Research summary:**

| Finding | Source |
|---------|--------|
| Errors per LOC decreased as routines grew up to 200 lines | Basili & Perricone 1984 |
| Routine size not correlated with errors; structural complexity was | Shen et al. 1985 |
| Routines 65+ lines were cheaper per LOC to develop | Card, Church, Agresti 1986 |
| Routines <143 lines had 23% more errors/LOC but 2.4× cheaper to fix | Selby & Basili 1991 |
| Code changed least when routines averaged 100–150 lines | Lind & Vairavan 1989 |
| Most error-prone: routines >500 lines | Jones 1986 |

**Practical guidance:**
- Let cohesion, nesting depth, number of variables, decision points, and comment needs dictate length — not an artificial limit.
- Routines up to **100–200 lines** are fine. Beyond 200, be careful. Beyond 500, you're in known high-error territory.
- In OOP, many routines are very short accessors; complex algorithms can grow organically.

---

### 7.5 How to Use Routine Parameters

> *39% of all errors are internal interface errors* (Basili & Perricone 1984).

**Guidelines:**

- **Put parameters in input-modify-output order**: input-only → input-and-output → output-only.
- **Consider `IN`/`OUT` keywords** for documentation (via macros or convention) — but prefer language-level `const` where available.
- **Similar parameters across routines → consistent order.** `strncpy(target, source, max)` and `memcpy(target, source, max)` are consistent. `fprintf(file, ...)` vs. `fputs(str, file)` are inconsistent and aggravating.
- **Use all parameters.** Unused parameters correlate with higher error rates. Remove them unless conditional compilation requires them.
- **Put status/error variables last.** They're incidental to the main purpose.
- **Don't use parameters as working variables.** Copy to a local variable: `int workingVal = inputVal;` This prevents accidentally losing the original input value.
- **Document interface assumptions:** input/modify/output status, units (inches, feet, meters), meaning of status codes, expected ranges, values that should never appear.
- **Limit parameters to about 7** (Miller's Law: 7±2 chunks). Use structured data/composite types to group related parameters.
- **Use an `i_`, `m_`, `o_` naming convention** if input/modify/output distinction matters.
- **Pass the right abstraction:** pass specific elements if the abstraction is about three specific data items; pass the whole object if the abstraction is about the object itself. If you find yourself "setting up" before a call and "taking down" after, the interface is wrong.
- **Use named parameters** (where the language supports it) — self-documenting and prevents mismatches.
- **Match actual to formal parameters.** Heed compiler warnings about type mismatches.

---

### 7.6 Functions vs. Procedures

- **Function** = routine that returns a value; **Procedure** = routine that does not.
- **Use a function** when the primary purpose is to return the value indicated by its name: `sin()`, `CustomerId()`, `ScreenHeight()`.
- **Use a procedure** when the primary purpose is an operation; return status via an explicit output parameter rather than a function return value.
- Prefer: `outputStatus = report.FormatOutput(formattedReport)` over `if (report.FormatOutput(formattedReport) = Success)` — separates the call from the status test, reducing complexity.

**Function return-value safety:**
- Check **all possible return paths** — mentally trace every branch.
- Initialize the return value to a safe default at the top.
- **Never return references or pointers to local data** — they become invalid when the routine exits.

---

### 7.7 Macro Routines and Inline Routines

**Macros (C/C++ preprocessor):**
- **Fully parenthesize** macro expressions: `#define Cube(a) ((a)*(a)*(a))`
- **Surround multi-statement macros with curly braces** `{ }` to avoid control-flow surprises.
- **Name macros that can be replaced by routines** using routine naming conventions, not ALL_CAPS.
- **Avoid macros as routine substitutes.** Use `const`, `inline`, `template`, `enum`, `typedef` instead.

> *"Almost every macro demonstrates a flaw in the programming language, in the program, or in the programmer."* — Bjarne Stroustrup

**Inline routines:**
- `inline` treats code as a routine at write time but compiles it inline. Use **sparingly**:
  - Violates encapsulation (implementation must be in the header).
  - Increases code size.
  - **Profile before inlining for performance** — don't guess.

---

## Ch 8: Defensive Programming

> *Defensive programming: if a routine is passed bad data, it won't be hurt, even if the bad data is another routine's fault.*

### 8.1 Protecting Your Program from Invalid Inputs

Bad input handling is no longer "garbage in, garbage out." Modern programs demand **"garbage in, nothing out"** or **"garbage in, error message out."**

**Three strategies:**
1. **Check all data from external sources** — files, users, network, other interfaces. Validate ranges, string lengths, restrictions. Watch for buffer overflows, SQL injection, HTML/XML injection, integer overflows.
2. **Check all routine input parameters** — similar to external data checking, but from internal callers.
3. **Decide how to handle bad inputs** — choose from the error-handling techniques below.

---

### 8.2 Assertions

> *Use error-handling code for conditions you expect; use assertions for conditions that should never occur.*

An assertion checks a boolean condition at runtime and halts/notifies if it's false. Assertions are primarily **development-time** tools and are typically **compiled out of production code**.

**What to assert:**
- Input parameter within expected range
- File/stream is open (or closed) at routine entry (or exit)
- File/stream is at beginning (or end)
- File/stream is read-only, write-only, or read-write
- Input-only variable is not modified
- Pointer is non-null
- Array/container has at least X elements
- Table has been initialized
- Container is empty (or full)
- Optimized routine's results match the reference implementation

**Guidelines:**
- **Assertions = executable documentation** of assumptions — more active than comments.
- **Never put executable code inside assertions** — it will be stripped in production: `Debug.Assert(PerformAction())` ❌
- **Use assertions to document preconditions and postconditions** (Design by Contract).
- **For highly robust code:** assert AND handle the error anyway. Real-world systems are too messy to rely solely on assertions (different designers, time periods, geographic regions, coding standards over 5–10 years).

---

### 8.3 Error-Handling Techniques

When you *do* expect errors, choose from these approaches:

| Technique | When to Use |
|-----------|-------------|
| **Return a neutral value** | Numeric → 0, string → empty, pointer → null. Safe for non-critical contexts. |
| **Substitute next piece of valid data** | Streaming data (readings, DB records). |
| **Return same answer as previous time** | High-frequency sensor readings where values don't change quickly. |
| **Substitute closest legal value** | Calibrated instruments, bounded ranges. Clamp to min/max. |
| **Log a warning message to a file** | Combine with other techniques. Consider security/privacy. |
| **Return an error code** | Delegate handling to higher-level code. Set status variable, return status, or throw exception. |
| **Call an error-processing routine/object** | Centralized error handling. Tradeoff: couples code to the central handler. Security risk after buffer overrun. |
| **Display an error message** | Minimizes overhead but scatters UI code and risks exposing internals. |
| **Handle locally, whatever works best** | Flexible per-developer but risks inconsistency. |
| **Shut down** | Safety-critical applications (radiation machines, security log overflow). |

#### Robustness vs. Correctness

| | Correctness | Robustness |
|---|------------|------------|
| **Goal** | Never return an inaccurate result | Always try to keep operating |
| **Tradeoff** | No result > wrong result | Any result > shutting down |
| **Use for** | Safety-critical (medical, aviation) | Consumer apps (word processors, games) |

> Error-handling strategy is an **architectural / high-level design decision**. Decide once, follow consistently.

---

### 8.4 Exceptions

Exceptions signal error conditions in a way that **cannot be ignored**. But used imprudently, they make code almost impossible to follow.

**Guidelines:**
- **Use exceptions to notify about errors that should not be ignored.**
- **Throw exceptions only for truly exceptional conditions** — not for expected, handleable errors.
- **Don't pass the buck** — handle locally if possible; don't throw just to avoid dealing with it.
- **Avoid throwing in constructors and destructors** — the rules about partial construction/cleanup are complex and error-prone.
- **Throw exceptions at the right level of abstraction** — a `GetTaxId()` method should throw `EmployeeDataNotAvailable`, not `EOFException`. Maintain interface consistency.
- **Include all diagnostic information** in the exception message — array bounds, illegal values, context.
- **Avoid empty `catch` blocks** — either fix the `try` block (it shouldn't throw) or fix the `catch` block (it should handle). At minimum, log the exception.
- **Know the exceptions your library code throws** — prototype and exercise libraries to discover hidden exceptions.
- **Consider a centralized exception reporter** for consistent formatting, logging, and handling.
- **Standardize project-wide:** what to throw, when to use try-catch locally vs. propagate, whether to use a centralized reporter, whether exceptions are allowed in constructors/destructors.
- **Consider alternatives** — error codes, logging, shutdown. Don't use exceptions just because the language provides them.

---

### 8.5 Barricade Your Program

> *Like a ship's bulkheads or a building's firewalls — contain the damage.*

**Strategy:** Designate "safe" zones. Classes/routines at the boundary sanitize dirty data; internal code assumes data is clean.

```
┌─────────────────────────────────────────────┐
│  DIRTY DATA (external inputs)               │
│  ┌─────────┐  ┌─────────┐  ┌────────────┐  │
│  │ GUI     │  │ Network │  │ File I/O   │  │
│  └────┬────┘  └────┬────┘  └─────┬──────┘  │
│       │            │             │          │
│  ┌────▼────────────▼─────────────▼──────┐   │
│  │        VALIDATION / SANITIZATION     │   │ ← Barricade
│  └────┬────────────┬─────────────┬──────┘   │
│       │            │             │          │
│  ┌────▼────┐  ┌────▼────┐  ┌────▼──────┐   │
│  │ Business│  │ Business│  │ Business  │   │
│  │ Logic 1 │  │ Logic 2 │  │ Logic 3   │   │ ← CLEAN ZONE (assertions)
│  └─────────┘  └─────────┘  └───────────┘   │
└─────────────────────────────────────────────┘
```

**Key rules:**
- **Outside barricade** → use error-handling (data is untrusted).
- **Inside barricade** → use assertions (data is supposed to be clean; any violation is a bug).
- **At class level:** public methods sanitize; private methods assume clean data.
- **Convert input data to proper type at input time** — don't carry strings representing booleans or enums through the system.

---

### 8.6 Debugging Aids

> *Don't automatically apply production constraints to the development version.*

**Principles:**
- The dev version can be slow and resource-hungry; the production version must be fast and lean.
- **Introduce debugging aids early** — don't wait until you've been bitten multiple times.
- **Offensive programming:** Make errors painful during development so they get fixed:
  - Assertions that abort (no "hit Enter to continue").
  - Fill allocated memory with junk to detect use-before-init.
  - Fill files/streams to flush out format errors.
  - Default `case`/`else` clauses that fail hard.
  - Fill deleted objects with junk data.
  - Auto-email error logs from production.

**Removing debug code — strategies:**
1. **Version-control / build tools** (`ant`, `make`) — build dev vs. production from same source.
2. **Built-in preprocessor** (`#if defined(DEBUG)`, `#define` with levels).
3. **Write your own preprocessor** — for languages without one (e.g., Java `//#BEGIN DEBUG`).
4. **Debugging stubs** — swap full-check routines with no-op stubs for production. Keep both versions.

---

### 8.7 How Much Defensive Programming to Leave in Production

| Action | Rationale |
|--------|-----------|
| **Leave in** checks for important errors | Calculation engine errors matter more than screen-update glitches. |
| **Remove** checks for trivial errors | Compile out (don't physically delete). Or log unobtrusively. |
| **Remove** code that causes hard crashes | Users need to save their work. Never lose user data for debugging. |
| **Leave in** code for graceful crashes | Pathfinder example: debug code left in enabled remote diagnosis and fix. |
| **Log errors** for tech support | Change assertions from abort to logging rather than removing entirely. |
| **Keep error messages user-friendly** | "Internal error — contact support@..." not "Bad pointer, Dog Breath!" |

---

### 8.8 Being Defensive About Defensive Programming

> *Too much defensive programming adds complexity and its own defects. Think about where you need to be defensive, and set priorities.*

---

## Ch 9: The Pseudocode Programming Process (PPP)

### 9.1 Summary of Steps

**Creating a class:**
1. Create a general design (responsibilities, secrets, abstraction, inheritance, key methods, data members). Iterate.
2. Construct each routine (uncovers need for additional routines; ripples back to class design).
3. Review and test the class as a whole.

**Creating a routine:**
1. Design the routine
2. Check the design
3. Code the routine
4. Check the code
5. Repeat as needed

---

### 9.2 Pseudocode for Pros

**Guidelines for effective pseudocode:**
- Use **English-like statements** describing specific operations.
- **Avoid target-language syntax** — design at a slightly higher level.
- Write at the **level of intent** — describe *what* and *why*, not *how*.
- Write at a **low enough level** that generating code is nearly automatic.

**Benefits:**
- Makes reviews easier (review design without code).
- Supports iterative refinement (high-level → pseudocode → source; catch errors at the cheapest stage).
- Makes changes easier (change a few lines of pseudocode, not a page of code).
- Minimizes commenting effort (pseudocode *becomes* the comments).
- Easier to maintain than separate design docs (comments stay in sync with code).

---

### 9.3 Constructing Routines Using the PPP

**Step-by-step process:**

**1. Design the routine:**
- **Check prerequisites** — is the routine well-defined and actually needed?
- **Define the problem** — what it hides, inputs, outputs, preconditions, postconditions.
- **Name the routine** — if you can't create a good name, the purpose isn't clear. Back up and improve the design.
- **Decide how to test** — plan test cases while designing (valid inputs, boundary conditions, invalid inputs).
- **Research standard libraries** — the biggest productivity boost: reuse existing, tested code. Don't reinvent what someone wrote a PhD dissertation on.
- **Think about error handling** — what can go wrong? Choose consciously how to handle it.
- **Think about efficiency** — in most systems, focus on clean interface and readability first, optimize later. In performance-critical systems, design to meet resource/speed budgets. Micro-optimizations come last.
- **Research algorithms and data types** — check algorithms books before writing from scratch.
- **Write the pseudocode** — start with a header comment summarizing purpose. Work from general to specific.

**2. Code the routine:**
- Write the routine declaration from the pseudocode header.
- Fill in the code below each pseudocode comment.
- Each comment becomes a block description; code implements that block.
- Leave the pseudocode as comments — removing them is more work than keeping them.

**3. Check the code:**
- Mentally trace each path.
- Review the code (yourself or with a peer).
- Compile (fix all warnings).
- Step through in a debugger.
- Unit test with the planned test cases.

**4. Clean up:**
- Remove any leftover scaffolding/test code.
- Verify all comments are accurate.
- Check routine against the quality checklist.

---

### 9.4 Alternatives to the PPP

| Alternative | Description |
|-------------|-------------|
| **Test-First Development (TDD)** | Write the test case before writing the routine. Write just enough code to pass the test. Refactor. |
| **Design by Contract** | Formal preconditions, postconditions, and class invariants. Assertions enforce the contract. |
| **Hack-and-fix** | Write code, debug, repeat. Not recommended for anything non-trivial. |
| **Stepwise refinement** | Decompose from high-level statements into detailed code in small, verified steps. |

---

## Checklists

### High-Quality Routine Checklist (Ch 7)

**Big-Picture:**
- [ ] Sufficient reason to create this routine?
- [ ] Could any parts benefit from being separate routines?
- [ ] Is the name a strong verb+object (procedure) or return-value description (function)?
- [ ] Does the name describe everything the routine does?
- [ ] Have you established naming conventions for common operations?
- [ ] Does the routine have **functional cohesion** (does one thing well)?
- [ ] Are the routine's connections loose (small, intimate, visible, flexible)?
- [ ] Is length determined naturally by function/logic, not an artificial limit?

**Parameters:**
- [ ] Does the parameter list present a consistent abstraction?
- [ ] Are parameters in a sensible, consistent order?
- [ ] Are interface assumptions documented?
- [ ] Seven or fewer parameters?
- [ ] Is each input parameter used? Each output?
- [ ] Avoid using input parameters as working variables?
- [ ] If a function, does it return a valid value under all circumstances?

### Defensive Programming Checklist (Ch 8)

**General:**
- [ ] Does the routine protect itself from bad input?
- [ ] Have assertions documented assumptions, preconditions, and postconditions?
- [ ] Are assertions used only for conditions that should never occur?
- [ ] Does the architecture specify a specific error-handling approach?
- [ ] Does the architecture specify robustness vs. correctness preference?
- [ ] Have barricades been created to contain error damage?
- [ ] Have debugging aids been installed (and can be easily activated/deactivated)?
- [ ] Is the amount of defensive code appropriate — neither too much nor too little?
- [ ] Have offensive-programming techniques been used to make errors obvious during development?

**Exceptions:**
- [ ] Standardized approach to exception handling across the project?
- [ ] Considered alternatives to exceptions?
- [ ] Error handled locally rather than thrown, if possible?
- [ ] Exceptions avoided in constructors and destructors?
- [ ] Exceptions at the correct level of abstraction?
- [ ] Each exception includes full diagnostic context?
- [ ] No empty catch blocks (or documented if truly appropriate)?

**Security:**
- [ ] Input checking covers buffer overflows, SQL injection, HTML injection, integer overflows?
- [ ] All error-return codes checked?
- [ ] All exceptions caught?
- [ ] Error messages avoid leaking information useful to attackers?

### Pseudocode Programming Process Checklist (Ch 9)

- [ ] Prerequisites checked — is the routine actually needed?
- [ ] Problem defined — hiding, inputs, outputs, preconditions, postconditions?
- [ ] Name clear, unambiguous, accurately descriptive?
- [ ] Test strategy planned?
- [ ] Standard libraries researched for existing implementations?
- [ ] Error-handling approach consciously chosen?
- [ ] Efficiency considered at the right level (high-level design, not micro-optimizations)?
- [ ] Algorithms researched (books, papers)?
- [ ] Pseudocode written — English-like, at the level of intent, detailed enough to code from?
- [ ] Header comment written first, summarizing purpose?
- [ ] Code written beneath each pseudocode comment?
- [ ] Code mentally traced, reviewed, compiled, stepped through, unit tested?
- [ ] Scaffolding removed, comments verified?

---

## Key Points

1. **The most important reason to create a routine is intellectual manageability** — reducing complexity. Saving space is minor; readability, reliability, and modifiability are major.
2. **Simple operations often benefit most from being routines** — they improve readability and grow gracefully under maintenance.
3. **Aim for functional cohesion** — one routine, one purpose, done well.
4. **A routine's name indicates its quality** — a bad name that's accurate means a poorly designed routine; a bad name that's inaccurate means the program is lying.
5. **Use functions only when returning the value is the primary purpose.**
6. **Production code should handle errors beyond "garbage in, garbage out."**
7. **Assertions catch errors early** — especially valuable in large systems, high-reliability systems, and fast-changing code.
8. **Error-handling strategy is a high-level design decision** — decide once, follow consistently.
9. **Exceptions are powerful but add complexity** — weigh against other error-processing techniques.
10. **Production constraints don't apply to development** — use extra debugging aids during development; remove/soften for production.
11. **Pseudocode is hard to beat for detailed design** — supports iterative refinement, ease of review, ease of change, and auto-documentation.
12. **Catch errors at the least-value stage** — much less is invested at the pseudocode stage than after full coding, testing, and debugging.
