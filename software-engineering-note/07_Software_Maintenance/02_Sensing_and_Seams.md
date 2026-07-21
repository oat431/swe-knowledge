---
tags:
  - legacy-code
  - seams
  - mocking
  - fakes
  - software-maintenance
source: "Feathers, Working Effectively with Legacy Code, Ch 3–5"
created: 2026-07-21
---

# 02 — Sensing and Seams

> *"A seam is a place where you can alter behavior in your program without editing in that place."* — Michael Feathers

## Overview

Chapters 3–5 of *Working Effectively with Legacy Code* lay the conceptual foundation for all legacy-code work. Feathers introduces three critical ideas:

1. **Sensing and Separation** — the two fundamental reasons we break dependencies.
2. **The Seam Model** — a language-independent way to think about *where* and *how* behavior can be replaced.
3. **Tools** — the testing harnesses and refactoring tools that make dependency-breaking safe.

---

## Chapter 3: Sensing and Separation

### The Two Reasons to Break Dependencies

When we want to get tests around a piece of legacy code, we encounter two distinct problems:

| Problem | Definition | Example |
|---|---|---|
| **Sensing** | We can't access the values our code computes | Can't tell what `NetworkBridge` wrote to hardware |
| **Separation** | We can't even get the code into a test harness to run | Creating a `NetworkBridge` requires real hardware present |

In an ideal system, you could instantiate any class in a test harness. In reality, dependencies cascade: creating one object pulls in its dependencies, which pull in theirs — eventually half the system ends up in the harness. In languages like C++, even *link time* makes rapid turnaround nearly impossible without breaking dependencies.

### The NetworkBridge Example

```java
public class NetworkBridge {
    public NetworkBridge(EndPoint[] endpoints) { ... }
    public void formRouting(String sourceID, String destID) { ... }
}
```

- `NetworkBridge` talks to `EndPoint` objects that open sockets to real devices.
- We can't sense what it does (sensing problem).
- We can't instantiate it without real hardware (separation problem).
- The class is a **closed box**.

### Faking Collaborators

The dominant technique for *sensing*: put a fake object in place of a real collaborator.

#### The Sale / Display Example

**Before:** `Sale` calls the cash-register display API directly — impossible to sense.

**After:** Extract a `Display` interface, provide a `FakeDisplay`.

```java
public interface Display {
    void showLine(String line);
}

public class Sale {
    private Display display;
    public Sale(Display display) { this.display = display; }

    public void scan(String barcode) {
        // ... compute itemLine ...
        display.showLine(itemLine);
    }
}

public class FakeDisplay implements Display {
    private String lastLine = "";
    public void showLine(String line) { lastLine = line; }
    public String getLastLine() { return lastLine; }
}
```

**Test:**
```java
public void testDisplayAnItem() {
    FakeDisplay display = new FakeDisplay();
    Sale sale = new Sale(display);
    sale.scan("1");
    assertEquals("Milk $3.99", display.getLastLine());
}
```

Key insight: this test doesn't prove pixels appear on real hardware — but it *does* prove that `Sale` sends the right text to the display abstraction. That lets us **divide and conquer** — eliminate `Sale` as a suspect when display bugs arise.

### The Two Sides of a Fake Object

Every fake has two "faces":

| Face | Who sees it | Example |
|---|---|---|
| **Collaborator side** | The class under test | `showLine(String)` — matches the `Display` interface |
| **Test side** | The test code | `getLastLine()` — enables sensing; not part of `Display` |

The test holds the reference as `FakeDisplay` (not `Display`) so it can call the test-side methods. The class under test only sees the interface.

### Fakes vs. Mock Objects

| | **Fake** | **Mock** |
|---|---|---|
| **How it works** | Records values; test inspects afterward | Performs assertions *internally* |
| **Example** | `display.getLastLine()` → `assertEquals(...)` | `display.setExpectation("showLine", "Milk $3.99")` then `display.verify()` |
| **Complexity** | Simple, hand-written | Requires a mocking framework |
| **When to use** | Most situations suffice | When you write many fakes, or need sophisticated call verification |

Feathers' advice: *simple fakes suffice in most situations.* Mock object frameworks are not available in all languages.

---

## Chapter 4: The Seam Model

### What Is a Seam?

> **Seam:** *A place where you can alter behavior in your program without editing in that place.*

The seam model gives us a language-agnostic way to see where we can break dependencies. Instead of seeing code as "a huge sheet of text," we see it as a graph with identifiable seams.

### Enabling Points

Every seam has an **enabling point** — the place *outside* the seam where you decide which behavior to use.

| Seam type | Enabling point |
|---|---|
| Preprocessing seam | `#define TESTING` or `#ifdef` macro definition |
| Link seam | Build script, makefile, classpath setting |
| Object seam | Constructor call site (which subclass/implementation to instantiate) |

Key insight: **the enabling point is always somewhere else.** You don't edit the seam itself — you toggle behavior from outside.

### The Three Seam Types

#### 1. Preprocessing Seams (C / C++)

The C preprocessor runs *before* the compiler, giving us text-replacement seams.

**Example — replacing `db_update` at test time:**

```c
// localdefs.h
#ifdef TESTING
struct DFHLItem *last_item = NULL;
int last_account_no = -1;

#define db_update(account_no, item) \
    { last_item = (item); last_account_no = (account_no); }
#endif
```

```c
// In the source file:
#include "localdefs.h"

void account_update(int account_no, struct DHLSRecord *record, int activated) {
    if (activated) {
        if (record->dateStamped && record->quantity > MAX_ITEMS) {
            db_update(account_no, record->item);   // ← seam
        } else {
            db_update(account_no, record->backup_item);
        }
    }
    db_update(MASTER_ACCOUNT, record->item);
}
```

**When `TESTING` is defined**, `db_update` becomes a macro that records parameters instead of hitting the database. The enabling point is the `#define TESTING` directive.

**Caveats:**
- Preprocessing decreases code clarity — you effectively maintain multiple programs in one file.
- `#ifdef` hell is real; use sparingly.
- But in C/C++, preprocessing seams are sometimes the *only* practical option.

#### 2. Link Seams

Exploit the linker (or classpath) to substitute entire implementations.

**Java — classpath seam:**

```java
// FitFilter.java
import fit.Parse;
import fit.Fixture;
// ...
tables = new Parse(input);   // ← seam
fixture.doTables(tables);    // ← seam
```

- **Seam:** the `new Parse(...)` and `fixture.doTables(...)` calls.
- **Enabling point:** the classpath — point it at test versions of `fit.Parse` and `fit.Fixture` that record calls.

**C/C++ — static linking seam:**

Create a stub library that replaces the production graphics library:

```cpp
// Stub library
std::queue<GraphicsAction> actions;

void drawLine(int firstX, int firstY, int secondX, int secondY) {
    actions.push_back(GraphicsAction(LINE_DRAW, firstX, firstY, secondX, secondY));
}

void drawText(int x, int y, char *text, int textLength) {
    actions.push_back(GraphicsAction(TEXT_DRAW, ...));
}
```

Then link against the stub in test builds. The enabling point is the **makefile or build configuration**.

**Usage tip:** Make the difference between test and production link environments *obvious*.

#### 3. Object Seams (OOP languages)

The most useful and explicit seam type. A method call is a seam when polymorphism can change *which* method executes.

**Seam or not?**

```java
// NOT a seam — no enabling point:
Cell cell = new FormulaCell(this, "A1", "=A2+A3");
cell.Recalculate();  // class fixed at creation; can't change without editing

// IS a seam — enabling point is the argument list:
public Spreadsheet buildMartSheet(Cell cell) {
    cell.Recalculate();  // can pass any Cell subclass in a test
}
```

**Even static methods can become seams** if you remove `static`, make them `protected`, and override in a testing subclass:

```java
// Before: static — not a seam
private static void Recalculate(Cell cell) { ... }

// After: protected non-static — now a seam
protected void Recalculate(Cell cell) { ... }

// In test:
class TestingCustomSpreadsheet extends CustomSpreadsheet {
    protected void Recalculate(Cell cell) {
        // override to sense or suppress behavior
    }
}
```

### The `CAsyncSslRec::Init` Example — All Three Seams

Feathers shows a single line of code and enumerates every seam available:

```cpp
bool CAsyncSslRec::Init() {
    // ...
    PostReceiveError(SOCKETCALLBACK, SSL_FAILURE);  // ← target line
    // ...
}
```

| # | Seam type | How | Enabling point |
|---|---|---|---|
| 1 | **Link seam** | Create a stub library with an empty `PostReceiveError`; link against it in test builds | Makefile / IDE build config |
| 2 | **Preprocessing seam** | `#define PostReceiveError(...)` to a no-op or recording macro | `#ifdef TESTING` |
| 3 | **Object seam** | Add a `virtual` method `PostReceiveError` to `CAsyncSslRec`; override in a testing subclass | Constructor call site (which subclass to instantiate) |

### Choosing the Right Seam

Feathers' preference hierarchy:

1. **Object seams** — best choice in OO languages; explicit, easy to understand, well-supported by tools.
2. **Link seams** — useful when dependencies are pervasive (e.g., a graphics library called everywhere).
3. **Preprocessing seams** — last resort; powerful but obscure, hard to maintain.

---

## Chapter 5: Tools

### Automated Refactoring Tools

Refactoring tools must **preserve behavior**. The Smalltalk Refactoring Browser set the standard: a change is only a refactoring if it does not change behavior.

**Danger example** — a tool that doesn't check side effects:

```java
// Before:
int v = getValue();     // getValue() increments alpha as a side effect
int total = 0;
for (int n = 0; n < 10; n++) {
    total += v;         // uses cached value; alpha incremented once
}

// After "inline variable" refactoring:
int total = 0;
for (int n = 0; n < 10; n++) {
    total += getValue(); // alpha now incremented 10 times — behavior changed!
}
```

**Best practice:** Have tests around code before using automated refactorings. Test the tool itself — e.g., verify it catches name collisions when extracting methods.

### Unit-Testing Harnesses

#### xUnit Family

The most effective testing tools are free. xUnit (originally Smalltalk, Kent Beck) has been ported to nearly every language. Key features:

- Write tests in the same language as production code.
- Tests run in **isolation** — each test method gets its own object.
- Tests can be grouped into **suites**.

**JUnit (Java):**
```java
public class EmployeeTest extends TestCase {
    private Employee employee;

    protected void setUp() {
        employee = new Employee("Fred", 0, 10);
        employee.addTimeCard(new TimeCard(new TDate(10, 10, 2000), 40));
    }

    public void testNormalPay() {
        assertEquals(400, employee.getPay());
    }

    public void testOvertime() {
        employee.addTimeCard(new TimeCard(new TDate(11, 10, 2000), 50));
        assertTrue(employee.hasOvertimeFor(newCardDate));
    }
}
```

- `setUp()` runs *before each test method* on a fresh object — **no shared mutable state.**
- `tearDown()` runs after each test.
- JUnit uses reflection to discover `test*` methods; CppUnit requires manual registration.

**CppUnitLite (C++):**
Uses macros instead of reflection (C++ lacks it):

```cpp
TEST(testNormalPay, Employee) {
    auto_ptr<Employee> employee(new Employee("Fred", 0, 10));
    LONGS_EQUALS(400, employee->getPay());
}
```

The `TEST` macro creates a static subclass instance that auto-registers with the test runner.

**NUnit (.NET):** Uses attributes (`<TestFixture()>`, `<Test()>`) instead of naming conventions.

### FIT and Fitnesse

**FIT (Framework for Integrated Tests)**, by Ward Cunningham:
- Write HTML documents with embedded tables describing inputs/outputs.
- FIT runs them as tests; colors cells **green** (pass) or **red** (fail).
- Bridges the gap between business specifications and developer tests.

**Fitnesse:** FIT hosted in a wiki (Robert Martin, Micah Martin). Enables collaborative, living-documentation-style testing.

---

## Key Takeaways

1. **Sensing = can't see outputs.** Break dependencies to observe what code computes.
2. **Separation = can't even run it.** Break dependencies to get code into a test harness.
3. **Fakes have two sides** — the collaborator side (interface) and the test side (sensing methods). Hold the reference as the fake type in tests.
4. **A seam is a place where behavior can change without editing that place.** Every seam has an *enabling point* elsewhere.
5. **Three seam types, in order of preference:** object seams > link seams > preprocessing seams.
6. **Object seams are the best in OOP** — explicit, tool-friendly, easy to reason about.
7. **The enabling point is always outside the seam** — build config, constructor call site, `#define` directive.
8. **Tests before automated refactoring** — tools can silently change behavior (e.g., side-effect re-execution).
9. **xUnit is universal** — write tests in your language, run in isolation, group into suites.
10. **Simple fakes suffice** — you rarely need a full mock object framework.

---

## See Also

- [[01_Legacy_Code_Overview]] — Introduction to legacy code and the change algorithm
- [[03_Dependency_Breaking_Techniques]] — Catalog of specific techniques from later chapters
- [[Testing_Strategies]] — Unit testing strategies for legacy code
