---
tags:
  - programming
  - fundamental
  - control-flow
---

# 04 Control Flow

Control flow determines the order in which statements execute. It is the skeleton of every algorithm — conditionals decide *which* path to take, loops decide *how many times* to repeat, and recursion decides *how deep* to go. Mastering control flow means mastering the logic of programs.

---

## Conditionals

### if / else if / else

```java
if (score >= 90) {
    grade = 'A';
} else if (score >= 80) {
    grade = 'B';
} else if (score >= 70) {
    grade = 'C';
} else {
    grade = 'F';
}
```

```python
if score >= 90:
    grade = 'A'
elif score >= 80:
    grade = 'B'
elif score >= 70:
    grade = 'C'
else:
    grade = 'F'
```

### switch / match

```java
// Java 17+ switch expression
String dayType = switch (day) {
    case "Mon", "Tue", "Wed", "Thu", "Fri" -> "Weekday";
    case "Sat", "Sun" -> "Weekend";
    default -> "Unknown";
};
```

```python
# Python 3.10+ structural pattern matching
match command:
    case "start":
        start_engine()
    case "stop":
        stop_engine()
    case _:
        print("Unknown command")
```

```typescript
// TypeScript — traditional switch
switch (day) {
    case "Mon":
    case "Tue":
    case "Wed":
    case "Thu":
    case "Fri":
        console.log("Weekday");
        break;
    case "Sat":
    case "Sun":
        console.log("Weekend");
        break;
    default:
        console.log("Unknown");
}
```

---

## Loops

### Loop Types

| Loop | When to Use | Example |
|---|---|---|
| `for` | Known iteration count, index needed | Iterating 0 to n |
| `while` | Condition-based, count unknown | Read until EOF |
| `do-while` | At least one execution guaranteed | Menu prompts |
| `for-each` / enhanced `for` | Iterate over collection elements | List traversal |

### for

```java
for (int i = 0; i < 10; i++) {
    System.out.println(i);
}
```

```typescript
for (let i = 0; i < 10; i++) {
    console.log(i);
}
```

### while

```java
while (scanner.hasNextLine()) {
    String line = scanner.nextLine();
    process(line);
}
```

### do-while

```java
int choice;
do {
    choice = showMenu();
} while (choice != 0);
```

### for-each / enhanced for

```java
for (String name : names) {
    System.out.println(name);
}
```

```python
for name in names:
    print(name)
```

```typescript
for (const name of names) {
    console.log(name);
}
// or functional style
names.forEach(name => console.log(name));
```

---

## Loop Control

| Statement | Effect |
|---|---|
| `break` | Exit the innermost loop immediately |
| `continue` | Skip to the next iteration |
| `return` | Exit the entire method/function |
| Labeled `break` | Exit a specific outer loop (Java) |

```java
// Labeled break — break out of an outer loop
outer:
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        if (matrix[i][j] == target) {
            System.out.println("Found at [" + i + "][" + j + "]");
            break outer;
        }
    }
}
```

```python
# Python — use for/else to detect "no break"
for item in items:
    if item.is_match():
        break
else:
    print("No match found")  # runs only if loop completed without break
```

---

## Pattern Matching

### Java 17+ Switch Expressions with Pattern Matching

```java
// Java 21+ pattern matching in switch
static String describe(Object obj) {
    return switch (obj) {
        case Integer i  -> "Integer: " + i;
        case String s   -> "String: " + s;
        case int[] arr  -> "Array of length " + arr.length;
        case null       -> "Null!";
        default         -> "Unknown type";
    };
}
```

### Python match/case with Guards

```python
match command:
    case {"action": "move", "direction": d, "speed": s} if s > 0:
        move(d, s)
    case {"action": "stop"}:
        stop()
    case _:
        pass  # ignore unknown commands
```

---

## Guard Clauses / Early Returns

Guard clauses flatten deeply nested conditionals by handling edge cases first and returning early.

```java
// ❌ BAD — deep nesting
public double calculateDiscount(Customer customer) {
    if (customer != null) {
        if (customer.isActive()) {
            if (customer.getOrderCount() > 10) {
                return 0.15;
            } else {
                return 0.05;
            }
        } else {
            return 0.0;
        }
    } else {
        return 0.0;
    }
}

// ✅ GOOD — guard clauses, flat structure
public double calculateDiscount(Customer customer) {
    if (customer == null) return 0.0;
    if (!customer.isActive()) return 0.0;
    if (customer.getOrderCount() > 10) return 0.15;
    return 0.05;
}
```

---

## Recursion Basics

Recursion is a function that calls itself with a smaller sub-problem until it reaches a base case.

| Component | Purpose |
|---|---|
| **Base case** | Terminates recursion (returns a concrete value) |
| **Recursive case** | Breaks the problem down and calls itself |

```java
// Factorial
static long factorial(int n) {
    if (n <= 1) return 1;           // base case
    return n * factorial(n - 1);    // recursive case
}
```

```python
def factorial(n: int) -> int:
    if n <= 1:
        return 1
    return n * factorial(n - 1)
```

> For advanced recursion patterns (backtracking, memoisation, tail recursion), see [[02 Recursion & Backtracking]].

---

## Common Pitfalls

### Off-by-One Errors

```java
// ❌ WRONG — skips last element or goes out of bounds
for (int i = 0; i <= array.length; i++) { }  // ArrayIndexOutOfBoundsException

// ✅ CORRECT
for (int i = 0; i < array.length; i++) { }
```

### Infinite Loops

```java
// ❌ WRONG — i never changes
int i = 0;
while (i < 10) {
    System.out.println(i);
    // forgot: i++
}

// ✅ CORRECT
int i = 0;
while (i < 10) {
    System.out.println(i);
    i++;
}
```

### Floating-Point Loop Counters

```java
// ❌ WRONG — may loop 10 or 11 times due to floating-point imprecision
for (double x = 0.0; x != 1.0; x += 0.1) { }

// ✅ CORRECT — use integer counters
for (int i = 0; i <= 10; i++) {
    double x = i / 10.0;
}
```

---

## Sources

- Oracle Java Tutorials — Control Flow Statements
- Oracle Java Language Specification §14 — Blocks, Statements, and Patterns
- Python Docs — Compound Statements (docs.python.org)
- MDN — Control Flow (developer.mozilla.org)
