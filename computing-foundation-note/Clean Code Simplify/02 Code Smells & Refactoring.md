---
tags:
  - clean-code
  - refactoring
  - code-smells
  - programming
---

# Code Smells & Refactoring

Code smells are surface-level indicators that something deeper is wrong with your design. They're not bugs — the code works. But they make maintenance painful. Refactoring is the disciplined process of improving code structure without changing behavior.

---

## Top 15 Code Smells

| # | Smell | Symptom | Fix |
|---|-------|---------|-----|
| 1 | **Long Method** | Functions > 30 lines, hard to name | **Extract Method** |
| 2 | **Large Class** | God classes with 10+ fields, many responsibilities | Extract Class, SRP |
| 3 | **Long Parameter List** | 4+ parameters in a method | Introduce Parameter Object |
| 4 | **Duplicated Code** | Same logic in 2+ places | Extract shared function/class |
| 5 | **Feature Envy** | Method uses another class's data more than its own | **Move Method** |
| 6 | **Data Clumps** | Same groups of variables passed together everywhere | Extract to object |
| 7 | **Shotgun Surgery** | One change requires edits in many classes | Move Method / Inline Class |
| 8 | **Divergent Change** | One class modified for many different reasons | Extract Class |
| 9 | **Primitive Obsession** | Using primitives instead of small objects | Introduce Value Object |
| 10 | **Switch Statements** | Long if/else or switch on type | Polymorphism / Strategy pattern |
| 11 | **Speculative Generality** | Unused abstractions "for the future" | YAGNI — remove it |
| 12 | **Dead Code** | Commented-out code, unreachable branches | Delete it (it's in git) |
| 13 | **Temporary Field** | Fields only used in certain contexts | Extract to a new class |
| 14 | **Message Chains** | `a.getB().getC().getD().doSomething()` | Hide Delegate, Law of Demeter |
| 15 | **Inappropriate Intimacy** | Two classes know too much about each other | Move Method, Extract shared logic |

---

## Essential Refactoring Moves

### Extract Method

Turn a code fragment into its own method with a descriptive name.

```java
// ❌ Before
void printOwing(double amount) {
    printBanner();
    // print details
    System.out.println("name: " + name);
    System.out.println("amount: " + amount);
}

// ✅ After
void printOwing(double amount) {
    printBanner();
    printDetails(name, amount);
}

void printDetails(String name, double amount) {
    System.out.println("name: " + name);
    System.out.println("amount: " + amount);
}
```

```typescript
// ❌ Before
function processOrder(order: Order) {
  // validation
  if (!order.items.length) throw new Error("Empty order");
  if (order.total < 0) throw new Error("Invalid total");
  // save
  db.orders.insert(order);
  logger.info(`Order ${order.id} saved`);
}

// ✅ After
function processOrder(order: Order) {
  validateOrder(order);
  persistOrder(order);
}

function validateOrder(order: Order): void {
  if (!order.items.length) throw new Error("Empty order");
  if (order.total < 0) throw new Error("Invalid total");
}

function persistOrder(order: Order): void {
  db.orders.insert(order);
  logger.info(`Order ${order.id} saved`);
}
```

### Inline Method

The reverse — when a method's body is as clear as its name, just inline it.

```java
// ❌ Unnecessary indirection
int getFivePercentRating() {
    return rating * 0.05;
}

// ✅ Inline where used
double bonus = rating * 0.05;
```

### Move Method

If a method uses data from another class more than its own, it belongs there.

```java
// ❌ Feature envy — Account uses mostly Bank data
class Account {
    double overdraftCharge() {
        if (bank.isPremium(this)) return 0;
        return daysOverdrawn * bank.getDailyRate();
    }
}

// ✅ Move to Bank
class Bank {
    double overdraftCharge(Account account) {
        if (isPremium(account)) return 0;
        return account.daysOverdrawn * getDailyRate();
    }
}
```

### Replace Temp with Query

Replace temporary variables with method calls to make logic reusable.

```java
// ❌ Temp variable
double basePrice = quantity * itemPrice;
if (basePrice > 1000) {
    return basePrice * 0.95;
}
return basePrice * 0.98;

// ✅ Query method
double basePrice() {
    return quantity * itemPrice;
}

double getPrice() {
    if (basePrice() > 1000) return basePrice() * 0.95;
    return basePrice() * 0.98;
}
```

### Introduce Parameter Object

Group related parameters into a cohesive object.

```java
// ❌ Too many params
List<Entry> getEntriesBetween(Date start, Date end, int minAge, int maxAge);

// ✅ Parameter object
record DateRange(Date start, Date end) {}
record AgeRange(int min, int max) {}

List<Entry> getEntriesBetween(DateRange dates, AgeRange ages);
```

---

## When to Refactor

| Trigger | Action | Rationale |
|---------|--------|-----------|
| **Rule of Three** | Refactor on 3rd duplication | 1st = write, 2nd = wince, 3rd = refactor |
| **Before adding a feature** | Clean the area first | Easier to add to clean code |
| **During code review** | Suggest refactorings | Catch smells before they compound |
| **Bug fix reveals mess** | Refactor after fixing | Understand the code, then clean it |
| **Red-green-refactor** | After tests pass | TDD cycle mandates refactoring |

> ⚠️ **Never refactor without tests.** If the code doesn't have tests, write characterization tests first. Refactoring without tests is not refactoring — it's just changing things and hoping.

---

## Refactoring Checklist

- [ ] All existing tests pass before starting
- [ ] Refactoring is a separate commit from feature/fix work
- [ ] Each step is small and verifiable
- [ ] Run tests after every step
- [ ] No behavior changes — only structure
- [ ] Commit message says "refactor: ..." not "fix: ..."

---

## Architectural Refactoring

For larger-scale structural changes, see [[01 Decomposition Patterns]] — breaking monoliths, extracting services, and restructuring module boundaries.

---

## Book Deep Dive

For the full code smells catalog with detailed examples and refactoring moves:
- [[Code Smells Catalog]] — comprehensive smell list
- [[Emergent Design]] — when design emerges from refactoring

---

**Sources:** Martin Fowler, *Refactoring* (2018); Robert C. Martin, *Clean Code*, Ch. 3, 17 (2008); refactoring.com/catalog
