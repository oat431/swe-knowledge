---
title: "Getting Tests in Place"
source: "Feathers, Working Effectively with Legacy Code — Chapters 9–17"
tags:
  - legacy-code
  - test-harness
  - characterization-tests
  - dependencies
  - software-maintenance
  - effect-analysis
  - dependency-breaking
  - interception-points
  - pinch-points
created: 2026-07-21
aliases:
  - Feathers Ch 9-17
  - WELC Part II — Changing Software
---

# Getting Tests in Place

> **Source:** Michael Feathers, *Working Effectively with Legacy Code*, Part II: Changing Software (Chapters 9–17).
> **Theme:** The hardest part of changing legacy code is getting it into a test harness. This note covers the dependency-breaking techniques, effect-analysis heuristics, characterization tests, and architectural strategies needed to create a safety net before you make changes.

---

## Overview of Chapters

| Chapter | Focus |
|---------|-------|
| 9 | Getting a class into a test harness — irritating parameters, hidden dependencies, globals, onion parameters |
| 10 | Running a method in a test harness — hidden methods, sealed/final classes, undetectable side effects |
| 11 | Finding *where* to test — effect sketches, forward reasoning, propagation rules |
| 12 | Making many changes in one area — interception points, pinch points |
| 13 | Writing characterization tests — documenting actual behavior before changing it |
| 14 | Library dependencies — the "once" dilemma, restricted override |
| 15 | Applications that are all API calls — Skin & Wrap, Responsibility-Based Extraction |
| 16 | Understanding unknown code — notes, sketching, listing markup |
| 17 | Application has no structure — referenced; covered in later chapters of WELC |

---

## Chapter 9 — I Can't Get This Class into a Test Harness

This is the central hard problem. If instantiating a class in a test harness were always easy, this book would be much shorter. The four most common problems:

1. Objects of the class can't be created easily.
2. The test harness won't easily build with the class in it.
3. The constructor has bad side effects.
4. Significant work happens in the constructor, and we need to **sense** it.

> **Heuristic:** The best way to see if you have trouble instantiating a class is to **just try it**. Write a construction test — no assertions needed; just attempt `new Foo()` and let the compiler tell you what you need.

---

### Case of the Irritating Parameter

**Problem:** A constructor depends on an object that is expensive, slow, or unreliable to create (e.g., a network connection).

**Example:** `CreditValidator(RGHConnection connection, CreditMaster master, String validatorID)`. Creating an `RGHConnection` connects to a live server — slow and unreliable in tests.

**Solutions (in order of preference):**

1. **Extract Interface (362)** — Pull an interface from the problematic dependency, create a fake implementation for tests.

   ```java
   // Before: CreditValidator depends on RGHConnection (concrete, connects to server)
   // After:  CreditValidator depends on IRGHConnection (interface)
   //         FakeConnection implements IRGHConnection — no server needed
   ```

2. **Subclass and Override Method (401)** — If the dependency isn't hard-coded in the constructor, override the problematic method in a testing subclass.

3. **Pass Null (111)** — If the parameter isn't actually used in the code path under test, pass `null`. The test harness catches the NPE if you're wrong.

   > **Warning:** Works in Java/C# but not in C/C++ (silent memory corruption). Never pass `null` in production code — consider the Null Object Pattern instead.

**Design principle:** Fake/test classes don't need to follow production-code rules. Making variables `public` in test fakes is acceptable if it simplifies tests.

---

### Case of the Hidden Dependency

**Problem:** A constructor uses `new` internally to create a dependency we can't control, with no way to substitute it.

**Example (C++):** `mailing_list_dispatcher` constructor does `service(new mail_service)` in its initializer list, then connects and registers with the mail system. Testing it actually sends real mail.

**Primary Solution — Parameterize Constructor (379):**

1. Extract the body of the constructor into a new `initialize(...)` method.
2. Add a new constructor that accepts the dependency as a parameter.
3. Keep the original no-arg constructor; it calls `initialize(new RealDependency())`.
4. Tests use the parameterized constructor with a fake.

   ```cpp
   // Original constructor stays for production clients
   mailing_list_dispatcher::mailing_list_dispatcher()
       { initialize(new mail_service); }

   // Tests use this one
   mailing_list_dispatcher::mailing_list_dispatcher(mail_service *service)
       { initialize(service); }
   ```

In Java/C#, this is even cleaner — constructors can chain:

```csharp
public MailingListDispatcher() : this(new MailService()) { }
public MailingListDispatcher(MailService service) { ... }
```

**Alternatives:** Extract and Override Getter (352), Extract and Override Factory Method (350), Supersede Instance Variable (404).

---

### Case of the Construction Blob

**Problem:** A constructor creates a **large number** of objects internally or chains object creation (object creates object creates object). Parameterizing everything leads to an explosion of constructor arguments.

**Primary Solution — Supersede Instance Variable (404):**

Add a setter that allows swapping a dependency *after* construction. Use only in tests.

```cpp
void supersedeCursor(FocusWidget *newCursor) {
    delete cursor;
    cursor = newCursor;
}
```

**C++ caveat:** Must be careful with `delete` — understand what the destructor does. In GC languages (Java, C#), this is straightforward.

**Prefer Extract and Override Factory Method** when possible, but it doesn't work in C++ constructors (virtual dispatch doesn't resolve to derived class in base constructors).

---

### Case of the Irritating Global Dependency

**Problem:** Classes depend on singletons or global variables that are hard to fake in tests.

**Example:** `PermitRepository.getInstance().findAssociatedPermit(notice)` — used in 10+ classes throughout the system.

**Solution — Introduce Static Setter (372):**

1. Add a `setTestingInstance(...)` static method to the singleton.
2. Relax the singleton property — make the constructor `protected` instead of `private`.
3. Create a testing subclass that overrides problematic methods.

```java
public class PermitRepository {
    private static PermitRepository instance = null;

    protected PermitRepository() { }  // was private

    public static void setTestingInstance(PermitRepository newInstance) {
        instance = newInstance;
    }

    public static PermitRepository getInstance() {
        if (instance == null) instance = new PermitRepository();
        return instance;
    }
}
```

**Alternative — `resetForTesting()`:** If the singleton's public methods allow full state setup, null out the static instance in `setUp()`/`tearDown()`.

**When to keep the singleton property:**
- Modeling real-world singletons (hardware controllers, single database).
- Preventing resource conflicts (two nuclear control-rod controllers).
- Resource limits (licenses, memory).

**When to relax it:** When the singleton exists only to avoid passing a parameter around.

> **Heuristic:** Global variables are usually *globally accessible* but not *globally used*. Trace how many classes actually need the global — you may find natural layering opportunities.

---

### Case of the Horrible Include Dependencies (C++)

**Problem:** C++'s `#include` mechanism creates massive transitive dependencies. Building a test for one class may require including tens of thousands of lines.

**Solutions:**

1. **Minimal includes** — Add `#include` directives one at a time; decide whether each dependency is truly needed.
2. **Alternative definitions** — Provide stub implementations of problematic methods in the test file itself (e.g., empty `SchedulerDisplay::displayEntry`).
3. **Separate test executable** — Build a standalone test program with its own `main` and fakes in a shared `Fakes.h`.

> **This is a last-resort technique** — reserved for very large classes with severe dependency problems that you plan to break up over time.

---

### Case of the Onion Parameter

**Problem:** To create object A you need B, to create B you need C, to create C you need D... The constructor parameter chain is a deep onion.

**Solution:** Start from the **most immediate** dependency. Use Extract Interface on it to create a fake. In Java, an interface can declare methods from a superclass — you don't need to extract interfaces for the entire hierarchy.

In C++, where there's no `interface` keyword, create an abstract class with pure virtual functions and have the concrete class delegate:

```cpp
class SchedulingTask : public SerialTask, public ISchedulingTask {
public:
    virtual void run() { SerialTask::run(); }
};
```

---

### Case of the Aliased Parameter

**Problem:** A parameter is hard to create (database access, side effects), but you can't use Extract Interface because the parameter type is passed or stored as its supertype, and creating interfaces for the entire hierarchy is excessive.

**Solution — Subclass and Override Method (401):** Instead of extracting interfaces for the whole hierarchy, subclass the problematic class and override only the methods that cause side effects.

```java
// OriginationPermit.validate() talks to a database. Override it:
class AlwaysValidPermit extends FakeOriginationPermit {
    public void validate() { becomeValid(); }  // no database
}
```

This preserves the type compatibility (e.g., can be assigned to a `Permit` field) without the interface explosion.

---

## Chapter 10 — I Can't Run This Method in a Test Harness

Even after instantiating a class, four problems can block method-level testing:

- Method is **private** or inaccessible.
- **Hard to construct** parameters.
- **Bad side effects** (database, missile launch).
- Need to **sense through** an object the method uses.

---

### Case of the Hidden Method

**Problem:** The method you need to change is `private`.

**Heuristic:** First, try testing through a **public** method that calls it. This tests the method as it's actually used. If that's impractical:

**Solution:** Make it `protected`, then create a testing subclass that exposes it:

```cpp
class TestingCCAImage : public CCAImage {
public:
    using CCAImage::setSnapRegion;  // C++ using-declaration
};
```

> **Corollary:** Good design is testable. If making a method public bothers you because it could corrupt state, the class has too many responsibilities — extract the method to a new class. (See Chapter 20.)

---

### Case of the "Helpful" Language Feature

**Problem:** Library classes are `sealed` (C#) or `final` (Java) — can't subclass, can't instantiate directly. E.g., `HttpPostedFile` and `HttpFileCollection` in .NET.

**Solutions:**

1. **Adapt Parameter (326)** — Change the method signature to accept a superclass/interface you *can* control.

2. **Skin and Wrap the API (205)** — Create your own interface + wrapper:

   ```csharp
   public interface IHttpPostedFile { int ContentLength { get; } ... }
   public class HttpPostedFileWrapper : IHttpPostedFile { ... }  // delegates to real
   public class FakeHttpPostedFile : IHttpPostedFile { ... }      // test fake
   ```

> **Lesson:** Isolate library classes behind your own interfaces/wrappers *proactively*, before they become a testing problem. See Ch 14–15.

---

### Case of the Undetectable Side Effect

**Problem:** The method modifies GUI components, creates windows, or has other side effects with no return value to sense.

**Approach (example: Java GUI class `AccountDetailFrame`):**

1. **Extract Method** repeatedly to separate business logic from GUI mechanics.
2. Name extracted methods by *what they compute*, not *how they display*.
3. Follow **Command/Query Separation** — methods should be commands (modify state, no return) OR queries (return value, no side effects), not both.
4. **Subclass and Override** the GUI-specific methods in tests:

   ```java
   class TestingAccountDetailFrame extends AccountDetailFrame {
       String displayText = "";
       void setDisplayText(String text) { displayText = text; }  // capture, don't display
       void setDescription(String desc) { }                       // suppress window creation
   }
   ```

5. After tests are in place, identify groups of methods that belong together and **extract them into new classes** (e.g., `SymbolSource`, `AccountDetailDisplay`).

> **Principle:** Safety first — it's okay to extract methods with poor names or structure to get tests in place. Refine the design *after* the safety net is established.

---

## Chapter 11 — I Need to Make a Change. What Methods Should I Test?

When code is tangled, a change in one place can affect behavior elsewhere. You need to **reason about effects** to find the right test locations.

---

### Reasoning About Effects — Effect Sketches

An **effect sketch** is a bubble-and-arrow diagram showing:

- **Variables/data** that can change (bubbles)
- **Methods whose return values can change** as a result (bubbles)
- Arrows = "affects" relationships

**Example sketch for `CppClass`:**

```
declarations ──→ getDeclarationCount
    │
    ├──→ getDeclaration
    │
    └──→ getInterface
         ↑
    any declaration in declarations
```

**Three ways effects propagate:**

1. **Return values** used by callers.
2. **Modification of objects passed as parameters** that are used later.
3. **Modification of static/global data** that is used later.

**Heuristic for finding effects:**

1. Identify the method that will change.
2. If it has a return value, look at its callers.
3. If it modifies any values, trace to methods that use those values.
4. Check superclasses and subclasses for additional clients.
5. Look at parameters — can the method modify them or objects they return?
6. Look for global/static data modified in any identified method.

---

### Reasoning Forward

When writing characterization tests, invert the process: start from the **change points** and trace forward to find where effects can be **detected** (interception points).

> **Know your language's "firewalls":**
> - `private` in Java = effects contained within the class.
> - Package-scope variables in Java = effects can leak to package neighbors.
> - `const` in C++ = prevents modification of instance variables... unless `mutable` is used.
> - `final`/`sealed` = subclassing barrier.
> - String immutability in Java = `getName()` can't be affected after construction.

---

### Simplifying Effect Sketches

When a method uses another method internally (e.g., `getInterface` calls `getDeclaration` instead of accessing the list directly), the effect sketch collapses — testing `getInterface` now also exercises `getDeclaration`. **Removing tiny duplications simplifies the effect graph and reduces the number of test endpoints needed.**

---

## Chapter 12 — I Need to Make Many Changes in One Area

When a feature requires changing 3–4 closely related classes, breaking all their dependencies individually is expensive. Instead, test **"one level back."**

---

### Interception Points

An **interception point** is anywhere you can detect the effects of a change. When choosing one:

- **Prefer points close to the change** — fewer reasoning steps between change and detection = less uncertainty.
- The best interception point is a **public method on the class being changed**.

---

### Pinch Points

A **pinch point** is a narrowing in an effect sketch — a place where tests against a **couple of methods** can detect changes across **many methods**.

**Properties:**
- A pinch point is determined by your **change points** (not an intrinsic property of the design).
- A pinch point is a **natural encapsulation boundary** — it tells you where to look when something breaks.
- **Pinch points can guide design:** The same narrowing that makes testing easier often reveals where responsibilities should be separated.

**Pinch Point Traps:**
- Don't let pinch-point tests become bloated mini-integration tests.
- After establishing cover at the pinch point, break down classes and write **narrower unit tests**.
- Eventually, the pinch-point tests can be deleted — they served as scaffolding.

> **Metaphor:** Tests at pinch points are like walking into a forest, drawing a line, and saying "I own this area." Then you develop it, adding finer tests. The perimeter tests can eventually be removed.

---

## Chapter 13 — I Need to Make a Change, but I Don't Know What Tests to Write

---

### Characterization Tests

A **characterization test** documents the *actual* behavior of the system — not what it *should* do, but what it *does* do. The goal isn't bug finding; it's creating a safety net for future changes.

**Algorithm:**

1. Use a piece of code in a test harness.
2. Write an assertion you know will **fail**.
3. Let the failure message tell you what the code actually produces.
4. Change the assertion to expect that value.
5. Repeat.

**Example:**

```java
// Step 1-2: Write a test that will fail
void testGenerator() {
    PageGenerator generator = new PageGenerator();
    assertEquals("fred", generator.generate());  // will fail
}
// Failure says: expected:<fred> but was:<>
// Step 4: Fix the assertion
assertEquals("", generator.generate());
```

**Key insight:** These tests have no "moral authority" — they just record reality. When you later change behavior intentionally, the test fails, telling you something changed. That's exactly what you want.

> **When you find bugs during characterization:** Mark the test as suspicious. Investigate whether the buggy behavior has downstream dependents before fixing it.

---

### Characterizing Classes — Heuristics

1. Look for **tangled logic** — use sensing variables to verify which paths execute.
2. List things that can **go wrong**; write tests that trigger them.
3. Test **extreme input values**.
4. Identify **invariants** — conditions that should always hold; test them.
5. Start with easy "sunny day" cases that show intent, then explore idiosyncrasies.

**The Method Use Rule:** Before using a method in a legacy system, check if there are tests for it. If not, write them.

---

### Targeted Testing

After characterization, focus on the **specific change** you're making:

- Which **branches** will be modified? Ensure tests exercise them.
- Which **conversions** happen along the path? (E.g., `double` → `int` truncation). Test with values that expose conversion bugs (fractional values).
- When **extracting or moving** code, verify that behavior exists *and* is connected correctly.

> **Refactoring tool quirk:** Some tools extract methods using instance variables directly rather than as parameters (e.g., extracting `x + y` yields `add(y)` if `x` is an instance variable). Hand-check extractions.

---

### Heuristic for Writing Characterization Tests

1. Write tests for the area you'll change — as many as needed to understand behavior.
2. Write tests for the **specific things** you're changing.
3. When extracting/moving functionality, verify existence and connection on a case-by-case basis. Exercise all conversions.

---

## Chapter 14 — Dependencies on Libraries Are Killing Me

**Core problem:** Every hard-coded use of a library class is a place where you *could have had* a seam. Over-reliance on a library makes the code rigid.

**Two key dilemmas:**

### The "Once" Dilemma
If a library assumes only one instance of a class can exist (singleton pattern), faking becomes difficult. **Wrap the singleton** if Introduce Static Setter isn't possible.

### The Restricted Override Dilemma
Library classes marked `final`/`sealed` or with non-virtual methods can't be overridden for testing.

> **Advice to library designers:** Constraints that enforce good design in production often make testing impossible. Coding conventions can achieve the same safety without blocking testability. *Think about what your tests need.*

**Mitigation strategies (see Ch 15 for details):**
- Write thin wrappers over library classes.
- Use interfaces to define your own seams.
- Complain to vendors about sealed/non-virtual key classes.

---

## Chapter 15 — My Application Is All API Calls

Systems that are nothing but repeated API calls have two problems:

1. **No visible design** — the structure is hidden behind API noise.
2. **You don't own the API** — can't rename, restructure, or add methods for clarity.

**Two approaches:**

---

### Skin and Wrap the API

Create interfaces that mirror the API and wrappers that delegate:

```
YourCode → IMyService (interface) → MyServiceWrapper → RealAPI
                                  → FakeService       (for tests)
```

**Best when:**
- The API is relatively small.
- You want to completely isolate from a third-party library.
- You can't write tests through the API directly (final/sealed classes).

---

### Responsibility-Based Extraction

Identify the **computational core** — what is this code really doing? Separate responsibilities:

1. Identify distinct jobs (e.g., for a mailing list server: receive mail, create forward messages, send mail, polling loop).
2. Extract methods for each responsibility.
3. Move them to new classes behind interfaces.

**Best when:**
- The API is large and complex (skinning everything is impractical).
- You have a refactoring tool.
- You can safely extract methods by hand.

> **Many teams use both:** a thin wrapper for testability + higher-level abstractions for clarity.

---

## Chapter 16 — I Don't Understand the Code Well Enough to Change It

Understanding legacy code is often the biggest time-sink. Two low-tech but high-impact techniques:

---

### Notes/Sketching

- Draw **informal diagrams** while browsing code. They don't need UML precision — they're tools for conversation and memory.
- Write down important names; draw lines for relationships.
- Sketches are **infectious** — when pairing, sketch as you explain; the other person will engage and contribute.

---

### Listing Markup

Print the code and mark it up physically:

1. **Separating Responsibilities** — use colored markers to group related symbols.
2. **Understanding Method Structure** — draw lines from block openings to closings; comment closing braces with what they terminate.
3. Start **inside-out**: find the first `}`, mark it, then backtrack to its matching `{`.

---

## Chapter 17 — My Application Has No Structure (Referenced)

This chapter (referenced throughout Ch 9–16 but not fully covered in this source extract) deals with applications that have no discernible architecture. Key ideas mentioned:

- **Layering emerges from separating responsibilities** — when you extract classes from API-heavy code, higher/lower-level distinctions naturally appear.
- **Global variables that are "used everywhere" signal zero layering.** Narrowing the scope of globals forces architectural boundaries.
- The same effect-analysis and responsibility-separation techniques from earlier chapters apply at the application scale.

---

## Key Techniques Referenced (Cross-Reference)

| Refactoring | Purpose |
|-------------|---------|
| Extract Interface (362) | Break dependency on concrete class |
| Extract Implementer (356) | Alternative to Extract Interface |
| Parameterize Constructor (379) | Externalize hidden constructor dependencies |
| Parameterize Method (383) | Replace global references with parameters |
| Subclass and Override Method (401) | Replace behavior in testing subclass |
| Extract and Override Getter (352) | Break dependency on object obtained via getter |
| Extract and Override Factory Method (350) | Break dependency on object created by factory method |
| Supersede Instance Variable (404) | Swap dependency after construction |
| Introduce Static Setter (372) | Make singleton replaceable for testing |
| Adapt Parameter (326) | Change method signature to accept a controllable type |
| Skin and Wrap the API (205) | Isolate library behind your own interface |
| Expose Static Method (345) | Access method without instantiating class |
| Break Out Method Object (330) | Move complex method to its own testable class |
| Pass Null (111) | Supply null for unused parameters in tests |
| Lean on the Compiler (315) | Use compiler errors to guide safe refactoring |
| Preserve Signatures (312) | Keep method signatures stable during extraction |

---

## Core Principles

1. **Safety first** — get tests in place before refactoring, even if the extracted code is ugly.
2. **Test code has different standards** — public variables in test fakes are fine.
3. **Encapsulation is a tool for understanding, not an end in itself** — break it when tests demand it; restore it later.
4. **Good design is testable; untestable design is bad.**
5. **Characterization tests document reality, not ideals.**
6. **Pinch points are natural encapsulation boundaries** — they guide both testing and design.
7. **Every hard-coded library call is a missed seam.**
8. **Know your language's effect firewalls** (`private`, `const`, `final`, immutability).


## Related

- [[Software Maintenance Overview]] — All maintenance topics
- [[02_Sensing_and_Seams]] — Seam model
- [[03_Adding_Features]] — Safe feature additions
- [[06_Dependency_Breaking_Catalog]] — All dependency-breaking techniques
