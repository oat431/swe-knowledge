---
tags:
  - programming
  - functional
  - fundamental
  - paradigm
---

# 09 Functional Programming

Functional Programming (FP) is a paradigm that treats computation as the evaluation of mathematical functions — no mutable state, no side effects, no surprises. You don't need to write Haskell to benefit from FP. Modern Java, TypeScript, Python, and most languages now support FP patterns natively. Understanding FP makes you a better programmer in *any* language.

---

## Core Principles

| Principle | Meaning | Why It Matters |
|---|---|---|
| **Pure functions** | Same input → same output, no side effects | Predictable, testable, cacheable |
| **Immutability** | Data doesn't change after creation | No race conditions, safe to share |
| **First-class functions** | Functions are values — pass, return, store | Enables composition and abstraction |
| **Declarative style** | Describe *what*, not *how* | More readable, less error-prone |

---

## Pure Functions

A function is **pure** if:
1. It always returns the same output for the same input
2. It has no observable side effects (no I/O, no mutation, no global state)

```java
// ❌ IMPURE — depends on external state, mutates it
int discount = 10;

int applyDiscount(int price) {
    return price - discount;  // what if discount changes?
}

// ✅ PURE — all dependencies are explicit parameters
int applyDiscount(int price, int discount) {
    return price - discount;  // always same result for same inputs
}
```

```typescript
// ❌ IMPURE — mutates input array
function addItem(cart: string[], item: string) {
    cart.push(item);  // caller's array is modified!
    return cart;
}

// ✅ PURE — returns new array, original unchanged
function addItem(cart: readonly string[], item: string): string[] {
    return [...cart, item];  // new array, original untouched
}
```

### Why Purity Matters

| Aspect | Pure Functions | Impure Functions |
|---|---|---|
| **Testing** | Just assert output for input | Must mock state, verify side effects |
| **Debugging** | Input → output, done | Trace state changes across calls |
| **Parallelism** | Safe to run anywhere | Race conditions, locks needed |
| **Caching** | Memoize by input | Cache may be stale |

---

## Immutability

Once created, data never changes. Instead of modifying, you create new versions.

### Java

```java
// ❌ Mutable — risky
List<String> names = new ArrayList<>();
names.add("Alice");
names.add("Bob");
names.clear();  // oops, lost everything

// ✅ Immutable — safe
List<String> names = List.of("Alice", "Bob");
// names.add("Charlie");  // COMPILE ERROR — UnsupportedOperationException

// Create modified copies
List<String> moreNames = new ArrayList<>(names);
moreNames.add("Charlie");
List<String> immutable = List.copyOf(moreNames);
```

### TypeScript

```typescript
// ❌ Mutable
const user = { name: "Alice", age: 30 };
user.age = 31;  // original object changed

// ✅ Immutable — spread operator creates new object
const user = { name: "Alice", age: 30 } as const;
const olderUser = { ...user, age: 31 };  // new object, original unchanged

// Deep freeze for strict immutability
Object.freeze(user);
```

### Why Immutability Matters

| Concern | Mutable | Immutable |
|---|---|---|
| **Thread safety** | Requires locks/synchronisation | Inherently safe |
| **Predictability** | Any reference can change state | State is fixed at creation |
| **Undo/history** | Must manually track changes | Old versions still exist |
| **Performance** | Modify in-place (fast) | Copy overhead (acceptable in most cases) |

> Immutability has a cost: creating new objects. For hot loops with large collections, use `StringBuilder` (Java), `immer` (TypeScript), or structural sharing (persistent data structures).

---

## First-Class Functions

Functions are values — you can assign them to variables, pass them as arguments, return them from other functions.

```java
// Java — function references and lambdas
Predicate<String> isLong = s -> s.length() > 10;
Function<String, Integer> length = String::length;
BiFunction<Integer, Integer, Integer> add = Integer::sum;

// Pass function as argument
List<String> filtered = names.stream()
    .filter(isLong)
    .collect(Collectors.toList());
```

```typescript
// TypeScript — functions are first-class citizens
const isLong = (s: string): boolean => s.length > 10;
const length = (s: string): number => s.length;

// Pass function as argument
const filtered = names.filter(isLong);
const lengths = names.map(length);
```

---

## Higher-Order Functions

A function that takes a function as input, or returns a function as output.

```java
// Java — higher-order function that returns a configured validator
Function<String, Predicate<String>> minLength = (min) ->
    (s) -> s.length() >= min;

Predicate<String> atLeast3 = minLength.apply(3);
Predicate<String> atLeast8 = minLength.apply(8);

atLeast3.test("hi");      // false
atLeast3.test("hello");   // true
atLeast8.test("hello");   // false
atLeast8.test("password"); // true
```

```typescript
// TypeScript — higher-order function
function retry<T>(fn: () => T, attempts: number): T {
    let lastError: Error;
    for (let i = 0; i < attempts; i++) {
        try {
            return fn();
        } catch (e) {
            lastError = e as Error;
        }
    }
    throw lastError!;
}

// Usage
const data = retry(() => fetchFromAPI(), 3);
```

---

## Map / Filter / Reduce

The three pillars of data transformation. Every language with FP support has these.

```java
// Java Streams
List<String> names = List.of("Alice", "Bob", "Charlie", "David");

// map — transform each element
List<Integer> lengths = names.stream()
    .map(String::length)
    .collect(Collectors.toList());  // [5, 3, 7, 5]

// filter — keep elements matching condition
List<String> longNames = names.stream()
    .filter(name -> name.length() > 4)
    .collect(Collectors.toList());  // ["Alice", "Charlie", "David"]

// reduce — combine all elements into one value
int totalLength = names.stream()
    .map(String::length)
    .reduce(0, Integer::sum);  // 20
```

```typescript
// TypeScript — array methods
const names = ["Alice", "Bob", "Charlie", "David"];

// map
const lengths = names.map(name => name.length);  // [5, 3, 7, 5]

// filter
const longNames = names.filter(name => name.length > 4);  // ["Alice", "Charlie", "David"]

// reduce
const totalLength = names
    .map(name => name.length)
    .reduce((sum, len) => sum + len, 0);  // 20
```

```python
# Python — list comprehensions + built-ins
names = ["Alice", "Bob", "Charlie", "David"]

# map
lengths = list(map(len, names))  # [5, 3, 7, 5]

# filter
long_names = list(filter(lambda n: len(n) > 4, names))  # ["Alice", "Charlie", "David"]

# reduce (must import)
from functools import reduce
total_length = reduce(lambda acc, n: acc + len(n), names, 0)  # 20

# Pythonic — list comprehensions (preferred)
lengths = [len(n) for n in names]
long_names = [n for n in names if len(n) > 4]
```

### Pipeline Pattern

Chain operations for readable data transformation:

```java
// Java — stream pipeline
double avgPrice = orders.stream()
    .filter(o -> o.getStatus() == Status.COMPLETED)
    .filter(o -> o.getDate().isAfter(LocalDate.now().minusDays(30)))
    .mapToDouble(Order::getTotal)
    .average()
    .orElse(0.0);
```

```typescript
// TypeScript — chained pipeline
const avgPrice = orders
    .filter(o => o.status === "completed")
    .filter(o => o.date > thirtyDaysAgo)
    .map(o => o.total)
    .reduce((sum, total, _, arr) => sum + total / arr.length, 0);
```

---

## Function Composition

Building complex functions by combining simple ones.

```typescript
// TypeScript — compose small functions into a pipeline
const trim = (s: string) => s.trim();
const toLowerCase = (s: string) => s.toLowerCase();
const split = (sep: string) => (s: string) => s.split(sep);

// Manual composition
const normalizeAndSplit = (s: string) => split(" ")(toLowerCase(trim(s)));
normalizeAndSplit("  Hello World  ");  // ["hello", "world"]

// Generic compose helper
const compose = <T>(...fns: Array<(arg: T) => T>) =>
    (x: T) => fns.reduceRight((acc, fn) => fn(acc), x);

const normalize = compose(
    (s: string) => s.toLowerCase(),
    (s: string) => s.trim()
);
normalize("  Hello  ");  // "hello"
```

```java
// Java — Function composition
Function<String, String> trim = String::trim;
Function<String, String> lower = String::toLowerCase;
Function<String, String[]> split = s -> s.split(" ");

Function<String, String[]> normalizeAndSplit = trim.andThen(lower).andThen(split);
normalizeAndSplit.apply("  Hello World  ");  // ["hello", "world"]
```

---

## Closures & Partial Application

### Closures

A function that captures variables from its enclosing scope.

```typescript
// TypeScript — closure captures `multiplier`
function createMultiplier(multiplier: number) {
    return (x: number) => x * multiplier;  // captures multiplier
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

double(5);  // 10
triple(5);  // 15
```

```java
// Java — effectively final variables are captured by lambdas
Function<Integer, Integer> createMultiplier(int multiplier) {
    return (x) -> x * multiplier;  // multiplier is effectively final
}

Function<Integer, Integer> double = createMultiplier(2);
Function<Integer, Integer> triple = createMultiplier(3);

double.apply(5);  // 10
triple.apply(5);  // 15
```

### Partial Application

Fixing some arguments of a function, producing a new function with fewer parameters.

```typescript
// TypeScript — partial application
function log(level: string, timestamp: Date, message: string) {
    console.log(`[${level}] ${timestamp.toISOString()}: ${message}`);
}

// Partial: fix level and timestamp
const errorLog = (msg: string) => log("ERROR", new Date(), msg);
const debugLog = (msg: string) => log("DEBUG", new Date(), msg);

errorLog("Database connection failed");  // [ERROR] 2026-07-11T...: Database connection failed
debugLog("Query executed in 42ms");      // [DEBUG] 2026-07-11T...: Query executed in 42ms
```

---

## Option / Either — Handling Missing & Error Values

FP replaces `null` and exceptions with explicit types that force the caller to handle both cases.

### Option (Maybe) — Handles Absence

```java
// Java — Optional<T>
public Optional<User> findById(int id) {
    return Optional.ofNullable(userMap.get(id));
}

// Caller MUST handle the empty case
String name = findById(42)
    .map(User::getName)
    .orElse("Anonymous");

// Chaining — clean pipeline, no null checks
String upperName = findById(42)
    .map(User::getName)
    .map(String::toUpperCase)
    .orElse("UNKNOWN");
```

```typescript
// TypeScript — null-safe chaining (built-in)
const name = findById(42)?.name?.toUpperCase() ?? "UNKNOWN";
```

### Either — Handles Errors

```typescript
// TypeScript — Result type pattern
type Result<T, E> = { ok: true; value: T } | { ok: false; error: E };

function divide(a: number, b: number): Result<number, string> {
    if (b === 0) return { ok: false, error: "Division by zero" };
    return { ok: true, value: a / b };
}

const result = divide(10, 2);
if (result.ok) {
    console.log(result.value);  // 5 — TypeScript narrows the type
} else {
    console.error(result.error);
}
```

```rust
// Rust — Result<T, E> is built into the language
fn divide(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 { Err("Division by zero".into()) }
    else { Ok(a / b) }
}

match divide(10.0, 2.0) {
    Ok(value) => println!("{}", value),
    Err(e) => eprintln!("{}", e),
}
```

---

## FP vs OOP — When to Use Which

| Aspect | FP | OOP |
|---|---|---|
| **State** | Immutable, passed explicitly | Mutable, encapsulated in objects |
| **Behaviour** | Functions transform data | Objects own data + methods |
| **Composition** | Function composition | Inheritance / interfaces |
| **Best for** | Data transformation, pipelines, concurrency | Complex domain models, GUI, frameworks |
| **Testing** | Easy (pure functions) | Requires mocking |
| **Modelling** | "What to do with data" | "What things are and do" |

### They're Not Enemies

Modern software uses **both**. The best code mixes paradigms:

- **Domain logic** → OOP (entities, value objects, rich models)
- **Data transformation** → FP (streams, pipelines, mapping)
- **Service layer** → FP patterns (pure functions, immutability)
- **Infrastructure** → OOP (repositories, adapters, DI containers)

```java
// ✅ Mixed paradigm — OOP structure, FP logic
public class OrderService {
    private final OrderRepository repo;  // OOP — dependency injection

    public BigDecimal calculateTotal(List<OrderItem> items) {
        return items.stream()                    // FP — stream pipeline
            .filter(item -> item.isValid())      // FP — pure predicate
            .map(OrderItem::getSubtotal)         // FP — method reference
            .reduce(BigDecimal.ZERO, BigDecimal::add);  // FP — reduce
    }
}
```

---

## Common Pitfalls

### ❌ Overusing Streams / Pipelines

```java
// ❌ Unreadable chain — when does it end?
return items.stream()
    .filter(i -> i.getPrice() > 0)
    .map(Item::getCategory)
    .flatMap(cat -> cat.getSubcategories().stream())
    .filter(sub -> sub.isActive())
    .map(Subcategory::getName)
    .distinct()
    .sorted()
    .collect(Collectors.toList());

// ✅ Break into named intermediate variables
List<String> activeCategories = items.stream()
    .filter(i -> i.getPrice() > 0)
    .map(Item::getCategory)
    .toList();

List<String> subcategoryNames = activeCategories.stream()
    .flatMap(cat -> cat.getSubcategories().stream())
    .filter(Subcategory::isActive)
    .map(Subcategory::getName)
    .distinct()
    .sorted()
    .toList();
```

### ❌ Ignoring Performance

```java
// ❌ Creating 1000 intermediate lists
List<Integer> result = numbers;
for (int i = 0; i < 1000; i++) {
    result = result.stream().map(n -> n + 1).collect(Collectors.toList());
}

// ✅ Single stream pipeline
List<Integer> result = numbers.stream()
    .map(n -> n + 1000)
    .collect(Collectors.toList());
```

### ❌ Using FP for Side-Effect-Heavy Code

```java
// ❌ forEach with side effects — not really FP
names.stream().forEach(name -> {
    database.save(name);        // side effect
    emailService.send(name);    // side effect
    logger.info("Saved: " + name);  // side effect
});

// ✅ Just use a for-loop — side effects aren't FP
for (String name : names) {
    database.save(name);
    emailService.send(name);
    logger.info("Saved: " + name);
}
```

> **Rule of thumb:** Use `forEach` only for terminal side effects (logging, collecting). If you're mutating state inside a stream, you're fighting the paradigm — use a loop.

---

## Sources

- *Functional Programming in Scala* — Paul Chiusano & Rúnar Bjarnason
- *Java Functional Programming* — Oracle Java Tutorials (docs.oracle.com)
- *Functional-Light JavaScript* — Kyle Simpson (github.com/getify/Functional-Light-JS)
- *Haskell Programming from First Principles* — Christopher Allen & Julie Moronuki
- MDN — Array methods (developer.mozilla.org)
- Martin Fowler — "Collection Pipeline" pattern (martinfowler.com)
