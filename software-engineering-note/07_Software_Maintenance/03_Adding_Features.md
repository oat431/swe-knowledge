---
tags:
  - legacy-code
  - tdd
  - sprout-method
  - wrap-method
  - software-maintenance
source: "Feathers, Working Effectively with Legacy Code, Ch 6–8"
created: 2026-07-21
---

# Adding Features to Legacy Code

> How to safely add new functionality when you can't get existing code under test — and how to build the foundation that makes feature addition fast and safe.

---

## Chapter 6: I Don't Have Much Time and I Have to Change It

### The Core Dilemma

When you need to add a feature under time pressure, you face a choice:

- **Hack it in** — inline changes in all the places needed, done by 5:00. But the code gets worse.
- **Write tests first** — break dependencies, get coverage, then make the change. Costs time now, saves frustration later.

> "Code is your house, and you have to live in it."

Changes cluster. If you're changing code today, you'll likely touch nearby code again soon. The investment in tests pays off faster than you think.

### Four Techniques for Safe Feature Addition (Without Existing Tests)

When you can't afford to get the existing class under test, use one of these techniques. **All produce tested new code** but don't test the existing call site — use with caution.

---

#### 1. Sprout Method

**When to use:** The change can be formulated as a self-contained piece of code that you can write as a separate method.

**Steps:**
1. Identify where the change needs to go.
2. Write a call to a new method (comment it out initially).
3. Determine local variables needed — make them arguments.
4. Determine if the sprout needs to return a value.
5. Develop the sprout method using **TDD**.
6. Uncomment the call.

**Example (Java):**

Before (inline change — invasive):
```java
public void postEntries(List entries) {
    List entriesToAdd = new LinkedList();
    for (Iterator it = entries.iterator(); it.hasNext(); ) {
        Entry entry = (Entry)it.next();
        if (!transactionBundle.getListManager().hasEntry(entry)) {
            entry.postDate();
            entriesToAdd.add(entry);
        }
    }
    transactionBundle.getListManager().add(entriesToAdd);
}
```

After (Sprout Method — clean separation):
```java
public void postEntries(List entries) {
    List entriesToAdd = uniqueEntries(entries);  // ← sprout
    for (Iterator it = entriesToAdd.iterator(); it.hasNext(); ) {
        Entry entry = (Entry)it.next();
        entry.postDate();
    }
    transactionBundle.getListManager().add(entriesToAdd);
}

// Developed with TDD
List uniqueEntries(List entries) {
    List result = new ArrayList();
    for (Iterator it = entries.iterator(); it.hasNext(); ) {
        Entry entry = (Entry)it.next();
        if (!transactionBundle.getListManager().hasEntry(entry)) {
            result.add(entry);
        }
    }
    return result;
}
```

**When the class is too hard to instantiate:** Make the sprout a `public static` method. Treat statics as a staging area — later, move them to instance methods on a new class (or back to the original class once it's under test).

| Advantages | Disadvantages |
|---|---|
| Clearly separates new code from old code | Gives up on the source method/class for now |
| Clean interface between new and old | Source method may look odd with a single sprout |
| See all variables affected — easier to verify correctness | Doesn't make the existing code better |

---

#### 2. Sprout Class

**When to use:** You can't even instantiate the class in a test harness (too many creational/hidden dependencies), OR the change introduces a genuinely new responsibility.

**Steps:**
1. Identify where the change goes.
2. Name a new class that does the work. Write object creation + method call (comment out).
3. Pass needed local variables as constructor arguments.
4. Provide a method for return values if needed.
5. Develop the sprout class **test-first (TDD)**.
6. Uncomment the creation and calls.

**Example (C++):**

```cpp
// Original: QuarterlyReportGenerator::generate() builds an HTML table.
// New requirement: add a header row.

// Sprout Class — developed with TDD
class QuarterlyReportTableHeaderGenerator {
public:
    string generate();
};

string QuarterlyReportTableHeaderGenerator::generate() {
    return "<tr><td>Department</td><td>Manager</td>"
           "<td>Profit</td><td>Expenses</td>";
}

// Usage in the source class:
QuarterlyReportTableHeaderGenerator producer;
pageText += producer.generate();
```

**Design evolution:** A sprouted class may later fold into existing concepts (e.g., both `QuarterlyReportGenerator` and `QuarterlyReportTableHeaderGenerator` implement `HTMLGenerator`). Or it may become a new concept entirely. Don't expect perfect design on day one — sprouted classes evolve over months.

| Advantages | Disadvantages |
|---|---|
| Move forward with confidence (no invasive changes) | Conceptual complexity — new classes proliferate |
| In C++: no need to modify existing headers | Things that belong in one class end up in sprouts |
| New header file = less compilation load on source class | |

---

#### 3. Wrap Method

**When to use:** You want to add behavior that happens before or after an existing method, and you want to keep the same method name for callers.

**Two forms:**

**Form 1 — Rename and wrap (same name for callers):**
```java
// Original
public void pay() {
    Money amount = new Money();
    for (...) { amount.add(...); }
    payDispatcher.pay(this, date, amount);
}

// Wrapped
private void dispatchPayment() {  // ← renamed old code
    Money amount = new Money();
    for (...) { amount.add(...); }
    payDispatcher.pay(this, date, amount);
}

public void pay() {               // ← new, same name
    logPayment();                  // ← new behavior
    dispatchPayment();             // ← delegate to old
}

private void logPayment() { ... }
```

**Form 2 — New method (explicit choice for callers):**
```java
public void makeLoggedPayment() {
    logPayment();
    pay();
}
```

**Steps (Form 1):**
1. Identify the method to change.
2. Rename the old method; create a new method with the **same name and signature** (Preserve Signatures).
3. Call the old method from the new method.
4. Develop the new feature method with TDD; call it from the new method.

**Steps (Form 2):**
1. Identify the method.
2. Develop the new method with TDD.
3. Create another method that calls both new and old.

| Advantages | Disadvantages |
|---|---|
| Does NOT increase the size of existing methods | Can lead to poor names for the renamed old method |
| Explicitly separates new from old functionality | Requires further extraction for clean naming |
| Good when you can't test the calling code | |

**Sprout vs. Wrap:** Use **Sprout Method** when the existing method already communicates a clear algorithm to the reader. Use **Wrap Method** when the new feature is as important as the old work — after wrapping, you often get a clean high-level algorithm:

```java
public void pay() {
    logPayment();
    Money amount = calculatePay();
    dispatchPayment(amount);
}
```

---

#### 4. Wrap Class (Decorator Pattern)

**When to use:** You need to add behavior transparently to many existing callers, OR the class is so large you refuse to make it worse by adding anything.

**Decorator approach** — wrap the class, implement the same interface, delegate + add behavior:

```java
class LoggingEmployee extends Employee {
    private Employee employee;

    public LoggingEmployee(Employee e) {
        employee = e;
    }

    public void pay() {
        logPayment();
        employee.pay();
    }

    private void logPayment() { ... }
}
```

This is the **Decorator Pattern**: compose behaviors at runtime by nesting wrappers:

```java
ToolController controller = new StepNotifyingController(
    new AlarmingController(new ACMEController()), notifyees);
```

**Non-decorator approach** — wrap only where needed:

```java
class LoggingPayDispatcher {
    private Employee e;

    public LoggingPayDispatcher(Employee e) { this.e = e; }

    public void pay() {
        employee.pay();
        logPayment();
    }

    private void logPayment() { ... }
}
```

**Steps:**
1. Identify the method to change.
2. Create a class that accepts the wrapped class as a constructor argument. Use **Extract Implementer** or **Extract Interface** if needed.
3. Develop the new method with TDD. Write another method that calls new + old.
4. Instantiate the wrapper where the new behavior is needed.

**Two tipping points for Wrap Class:**
1. The new behavior is completely independent — don't pollute the existing class.
2. The class is so large you can't stand to make it worse. Wrapping is a stake in the ground.

> "If you consistently do these little improvements, your system will start to look significantly different over the course of a couple of months."

---

### Summary: Sprout vs. Wrap Decision Matrix

| | Method-level | Class-level |
|---|---|---|
| **Add + call new** | Sprout Method | Sprout Class |
| **Rename old, wrap** | Wrap Method | Wrap Class (Decorator) |

---

## Chapter 7: It Takes Forever to Make a Change

### Why Changes Take So Long

**1. Understanding** — Legacy code requires deep context; changes are painful even after you figure out what to do. Well-maintained systems: figuring out takes time, but the change is easy.

**2. Lag Time** — The delay between making a change and getting feedback. Like driving the Mars rover with a 14-minute round-trip delay.

> In most mainstream languages, you can always break dependencies so that you recompile and run tests in **under 10 seconds**.

The human mind works differently with fast feedback — we try out approaches quickly, concentration is more intense, and mistakes are caught faster.

### Breaking Dependencies for Fast Builds

**Goal:** Compile every class/module separately in its own test harness.

**Technique:** Extract interfaces/implementers on dependencies to create "compilation firewalls."

Before:
```
AddOpportunityFormHandler → ConsultantSchedulerDB (concrete)
AddOpportunityFormHandler → OpportunityItem (concrete)
```

After Extract Implementer:
```
AddOpportunityFormHandler → «interface» ConsultantSchedulerDB
                           → «interface» OpportunityItem
ConsultantSchedulerDBImpl implements ConsultantSchedulerDB
OpportunityItemImpl implements OpportunityItem
```

**Result:** Changes to `ConsultantSchedulerDBImpl` or `OpportunityItemImpl` do NOT force `AddOpportunityFormHandler` to recompile.

### Package Restructuring

Move extracted clusters into separate packages:

```
OpportunityProcessing/          (depends only on interfaces)
  AddOpportunityFormHandler
  AddOpportunityXMLGenerator

DatabaseGateway/                (interfaces only)
  ConsultantSchedulerDB
  OpportunityItem

DatabaseImplementation/         (concrete implementations)
  ConsultantSchedulerDBImpl
  OpportunityItemImpl
```

### Dependency Inversion Principle

> Depend on interfaces or abstract classes, not concrete classes. Interfaces change far less often than their implementations.

**Trade-off:** More interfaces/packages = slightly longer full rebuilds, but dramatically shorter **average** builds (incremental, based on what actually changed).

> "You have to do it only once for that set of classes; afterward, you get to reap the benefits forever."

---

## Chapter 8: How Do I Add a Feature?

### Test-Driven Development (TDD) in Legacy Code

**Standard TDD algorithm:**
1. Write a failing test.
2. Get it to compile.
3. Make it pass.
4. Remove duplication.
5. Repeat.

**Extended for legacy code:**
> **0. Get the class you want to change under test.**
> 1. Write a failing test case.
> 2. Get it to compile.
> 3. Make it pass. **(Try not to change existing code as you do this.)**
> 4. Remove duplication.
> 5. Repeat.

The key insight: TDD lets you concentrate on **one thing at a time** — writing code OR refactoring, never both. In legacy code, this means writing new code independently of old code, then refactoring to remove duplication.

### Programming by Difference

**Use inheritance to introduce features without modifying a class directly.** After the feature works, refactor to a cleaner structure.

**Example:** Adding anonymous forwarding to `MessageForwarder`:

```java
// Step 1: Subclass to pass the test quickly
public class AnonymousMessageForwarder extends MessageForwarder {
    protected InternetAddress getFromAddress(Message message) {
        String anonymousAddress = "anon-" + listAddress;
        return new InternetAddress(anonymousAddress);
    }
}
```

**Step 2: Refactor away from inheritance** — make it a configuration option:

```java
// Configuration-based approach
Properties configuration = new Properties();
configuration.setProperty("anonymous", "true");
MessageForwarder forwarder = new MessageForwarder(configuration);
```

**Step 3: Extract a proper class** — evolve from `Properties` to `MailingConfiguration` to `MailingList`:

```
MessageForwarder → MailingList
                     + getFromAddress(Message)
                     + buildRecipientList(List)
```

> Rename Class is the most powerful refactoring. It changes how people see code and lets them notice new possibilities.

### Liskov Substitution Principle (LSP)

> Objects of subclasses should be substitutable for objects of their superclasses throughout the code. If they aren't, silent errors result.

**Classic violation:** `Square extends Rectangle`. `setWidth(3); setHeight(4)` on a `Square` → area = 16, not 12.

**Rules of thumb:**
1. **Avoid overriding concrete methods** whenever possible.
2. If you must override, call the overridden method in the overriding method.

### Normalized Hierarchy

A hierarchy where **no class overrides a concrete method inherited from a superclass**. Every method is either defined in exactly one concrete class or is abstract.

```
{abstract} MessageForwarder
  # getFromAddress(Message) {abstract}

AddressPreservingForwarder          AnonymousForwarder
  # getFromAddress(Message)           # getFromAddress(Message)
```

When you ask "How does this class do X?", the answer is in exactly one place — either the class itself or its abstract definition.

---

## Key Takeaways

1. **Don't hack inline.** Even when time-pressed, use Sprout/Wrap techniques to keep new code tested and separated from old code.

2. **Sprout Method** is the first resort — extract new behavior into a TDD-developed method. **Sprout Class** when the class can't be instantiated.

3. **Wrap Method/Class** when you need to add behavior around existing calls without modifying the old code.

4. **Break dependencies for fast feedback.** Extract interfaces to create compilation firewalls. Aim for <10 second edit-compile-test cycles.

5. **TDD in legacy code** means: get the class under test first (step 0), then write failing tests, make them pass without touching existing code, and refactor to remove duplication.

6. **Programming by Difference** — subclass to add features quickly, then refactor toward configuration or extracted classes. **Don't leave the subclass in place permanently.**

7. **Watch for LSP violations** when overriding concrete methods. Prefer **normalized hierarchies** where each method has exactly one concrete implementation.


## Related

- [[Software Maintenance Overview]] — All maintenance topics
- [[02_Sensing_and_Seams]] — Seams and fakes
- [[04_Getting_Tests_in_Place]] — Getting classes under test
- [[06_Dependency_Breaking_Catalog]] — Dependency breaking techniques
