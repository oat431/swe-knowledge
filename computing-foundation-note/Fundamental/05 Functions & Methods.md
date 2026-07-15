---
tags:
  - programming
  - fundamental
  - functions
---

# 05 Functions & Methods

Functions are the primary unit of abstraction in programming. They encapsulate a piece of logic, give it a name, and let you reuse it without repeating yourself. Understanding how functions work — from parameters to the call stack — is essential for writing clean, testable code.

---

## Function Anatomy

```
┌─────────────────────────────────────────────┐
│  access_modifier return_type name(params) { │
│      // body                                │
│      return value;                          │
│  }                                          │
└─────────────────────────────────────────────┘
```

| Component | Description |
|---|---|
| **Name** | Identifies the function; should describe what it does |
| **Parameters** | Input variables (the "slots") |
| **Arguments** | Actual values passed to parameters |
| **Return type** | The type of value the function produces |
| **Body** | The statements that execute when the function is called |

```java
// Java — method in a class
public static int add(int a, int b) {
    return a + b;
}
```

```typescript
// TypeScript — standalone function
function add(a: number, b: number): number {
    return a + b;
}

// Arrow function
const add = (a: number, b: number): number => a + b;
```

```python
# Python
def add(a: int, b: int) -> int:
    return a + b
```

---

## Parameters vs Arguments

```java
// 'a' and 'b' are PARAMETERS (declaration)
int multiply(int a, int b) {
    return a * b;
}

// 3 and 7 are ARGUMENTS (call site)
int result = multiply(3, 7);
```

---

## Pass by Value vs Pass by Reference

| Approach | What Gets Copied | Languages |
|---|---|---|
| **Pass by value** | A copy of the actual value | Java (primitives), C# (value types), Go |
| **Pass by reference** | A reference to the original variable | C++ (`&`), C# (`ref`/`out`) |
| **Pass by value of reference** | A copy of the reference (pointer), but the object is shared | Java (objects), JavaScript, Python |

```java
// Java — always pass-by-value, but references are copied
void modify(int x, int[] arr) {
    x = 999;       // modifies local copy only
    arr[0] = 999;  // modifies the ORIGINAL array (same reference)
}

int num = 1;
int[] data = {1, 2, 3};
modify(num, data);
// num is still 1, but data[0] is now 999
```

```cpp
// C++ — true pass by reference
void swap(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}
```

---

## Default Parameters, Rest / Spread

### Default Parameters

```typescript
// TypeScript
function greet(name: string, greeting: string = "Hello"): string {
    return `${greeting}, ${name}!`;
}

greet("Alice");           // "Hello, Alice!"
greet("Alice", "Hi");    // "Hi, Alice!"
```

```python
# Python
def greet(name: str, greeting: str = "Hello") -> str:
    return f"{greeting}, {name}!"
```

```java
// Java — NO default parameters (use overloading instead)
String greet(String name) {
    return greet(name, "Hello");
}
String greet(String name, String greeting) {
    return greeting + ", " + name + "!";
}
```

### Rest / Spread Parameters

```typescript
// TypeScript — rest parameters
function sum(...numbers: number[]): number {
    return numbers.reduce((acc, n) => acc + n, 0);
}
sum(1, 2, 3, 4);  // 10
```

```python
# Python — *args and **kwargs
def sum_all(*args: int) -> int:
    return sum(args)
```

```java
// Java — varargs
int sum(int... numbers) {
    int total = 0;
    for (int n : numbers) total += n;
    return total;
}
```

---

## Function Overloading

Multiple functions with the **same name** but **different parameter lists** (type, number, or order).

```java
// Java — overloading
int add(int a, int b) { return a + b; }
double add(double a, double b) { return a + b; }
int add(int a, int b, int c) { return a + b + c; }
```

> Python and JavaScript do **not** support overloading — the last definition wins. Use default parameters or `*args` instead.

---

## Pure Functions vs Side Effects

| Aspect | Pure Function | Function with Side Effects |
|---|---|---|
| Same input → same output? | ✅ Always | ❌ Not guaranteed |
| Modifies external state? | ❌ Never | ✅ May do |
| Testable? | Trivially (input → output) | Requires mocking/setup |
| Cacheable? | ✅ (memoisation) | ❌ |
| Example | `Math.abs(x)` | `file.write(data)` |

```java
// ✅ Pure — no side effects, deterministic
static int square(int x) {
    return x * x;
}

// ❌ Impure — modifies external state
static int counter = 0;
static int increment() {
    return ++counter;
}
```

---

## Higher-Order Functions

A function that takes another function as a parameter, or returns a function.

```java
// Java — passing a function (via lambda)
List<String> names = List.of("Alice", "Bob", "Charlie");
List<String> upper = names.stream()
    .map(name -> name.toUpperCase())  // .map() is higher-order
    .toList();
```

```typescript
// TypeScript — higher-order function
function applyTwice(f: (x: number) => number, x: number): number {
    return f(f(x));
}
applyTwice(n => n * 2, 3);  // 12
```

```python
# Python — higher-order function
def apply_twice(f, x):
    return f(f(x))

apply_twice(lambda n: n * 2, 3)  # 12
```

---

## Lambda / Arrow Functions

Anonymous (unnamed) functions defined inline.

```java
// Java lambda
Comparator<String> byLength = (a, b) -> a.length() - b.length();
names.sort(byLength);
```

```typescript
// TypeScript arrow function
const double = (x: number) => x * 2;
const greet = (name: string) => {
    console.log(`Hello, ${name}`);
};
```

```python
# Python lambda (single expression only)
double = lambda x: x * 2
```

---

## Closure Basics

A **closure** is a function that captures variables from its surrounding scope, even after that scope has finished executing.

```typescript
// TypeScript — closure
function makeCounter(): () => number {
    let count = 0;
    return () => ++count;  // captures 'count' from enclosing scope
}

const counter = makeCounter();
counter();  // 1
counter();  // 2
counter();  // 3
// 'count' is private — only accessible through the returned function
```

```java
// Java — effectively final variables in lambdas
String prefix = "Hello";  // must be effectively final
Function<String, String> greeter = name -> prefix + ", " + name;
```

---

## Recursion vs Iteration

| Aspect | Recursion | Iteration |
|---|---|---|
| **Readability** | Natural for tree/graph/recursive problems | Natural for sequential/linear problems |
| **Memory** | Uses call stack (risk of stack overflow) | Constant stack usage |
| **Performance** | Overhead of function calls | Generally faster (no call overhead) |
| **Tail-call optimisable?** | Yes (in languages that support it: JS, Scala) | N/A |

> When in doubt, prefer iteration for simple loops. Use recursion when the problem is naturally recursive (trees, graphs, divide-and-conquer). See [[04 Control Flow]] for recursion basics.

---

## Stack Frames and the Call Stack

Every function call creates a **stack frame** containing:
- Local variables
- Parameters
- Return address (where to continue after the function returns)

```
┌──────────────────┐
│  main()          │  ← bottom of stack
├──────────────────┤
│  calculate()     │
├──────────────────┤
│  validate()      │  ← top of stack (currently executing)
└──────────────────┘
```

When `validate()` returns, its frame is popped off and execution resumes in `calculate()`. A **stack overflow** occurs when recursion is too deep (no base case or problem too large for the stack).

---

## Sources

- *Clean Code* — Robert C. Martin, Chapter 3: Functions
- *Effective Java* (3rd ed.) — Joshua Bloch, Items 19-21 (Lambdas)
- MDN — Closures (developer.mozilla.org)
- Oracle Java Tutorials — Lambda Expressions
