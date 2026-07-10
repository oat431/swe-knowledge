---
tags:
  - programming
  - fundamental
  - memory
---

# 08 Memory Management

Every program needs memory to store data — variables, objects, function calls. How that memory is allocated, used, and reclaimed varies dramatically across languages and directly impacts performance, correctness, and security. Understanding memory management prevents the most insidious category of bugs: those that crash silently or degrade over hours.

---

## Stack vs Heap

| Aspect | Stack | Heap |
|---|---|---|
| **Stores** | Local variables, function parameters, return addresses | Objects, arrays, dynamically allocated data |
| **Allocation** | Automatic (push/pop on function call/return) | Manual or GC-managed |
| **Speed** | Very fast (LIFO, pointer bump) | Slower (allocation + possible GC pause) |
| **Size limit** | Small (typically 1-8 MB per thread) | Large (limited by available RAM) |
| **Lifetime** | Scope-bound (dies when function returns) | Until explicitly freed or garbage collected |
| **Thread safety** | Each thread has its own stack | Shared across threads (requires synchronisation) |

```
Memory Layout:
┌──────────────────────┐ High address
│        Stack         │ ← grows downward
│  (local vars, frames)│
├──────────────────────┤
│          ↓           │
│    (free space)      │
│          ↑           │
├──────────────────────┤
│        Heap          │ ← grows upward
│  (objects, arrays)   │
├──────────────────────┤
│   Static / Global    │
├──────────────────────┤
│   Code (Text)        │
└──────────────────────┘ Low address
```

```java
// Stack vs Heap in Java
void example() {
    int x = 10;                // x is on the stack
    String name = "Alice";     // reference on stack, String object on heap
    int[] arr = new int[100];  // reference on stack, array on heap
}
// When example() returns: x, name, arr references are popped from stack
// The String and array on heap become eligible for garbage collection
```

---

## Value Types vs Reference Types in Memory

| Type | What's Stored Where | Assignment Behaviour |
|---|---|---|
| **Value type** (`int`, `double`, `boolean`) | Directly on the stack | Copies the value |
| **Reference type** (`String`, `Object`, arrays) | Reference (pointer) on stack, object on heap | Copies the reference (both point to same object) |

```java
// Value type — independent copies
int a = 42;
int b = a;
b = 99;
// a is still 42 — separate memory locations

// Reference type — shared object
int[] arr1 = {1, 2, 3};
int[] arr2 = arr1;
arr2[0] = 99;
// arr1[0] is now 99 — same heap object
```

```typescript
// TypeScript — same concept
let a = 42;
let b = a;
b = 99;        // a is still 42

let arr1 = [1, 2, 3];
let arr2 = arr1;
arr2[0] = 99;  // arr1[0] is now 99 — same reference
```

---

## Garbage Collection Basics

Garbage collection (GC) automatically reclaims heap memory that is no longer reachable. It eliminates manual `free`/`delete` calls and prevents most memory leaks — but introduces pause-time trade-offs.

### Mark-and-Sweep

The fundamental GC algorithm:

1. **Mark** — Starting from GC roots (local variables, static fields, active threads), traverse all reachable objects and mark them as "alive."
2. **Sweep** — Iterate over all heap objects; anything not marked is unreachable and gets reclaimed.

```
GC Roots (local vars, statics)
       │
       ▼
   [Object A] ──→ [Object B]    ← marked (reachable)
       │
       ▼
   [Object C]                   ← marked

   [Object D]                   ← UNMARKED → collected
```

### Generational GC (JVM, .NET)

Based on the observation that most objects die young:

| Generation | Contains | GC Frequency |
|---|---|---|
| **Young (Eden + Survivor)** | Newly created objects | Frequent (minor GC) |
| **Old (Tenured)** | Long-lived objects (survived multiple minor GCs) | Infrequent (major GC) |
| **Permanent / Metaspace** | Class metadata, method info | Rare or never |

```
New object → Eden → (survives minor GC) → Survivor → (survives more GCs) → Old
```

---

## Manual Memory Management

### C/C++ — malloc/free, new/delete

```c
// C — manual allocation and deallocation
int* arr = (int*)malloc(100 * sizeof(int));
if (arr == NULL) {
    // handle allocation failure
    return -1;
}
// use arr...
free(arr);  // MUST free when done
arr = NULL; // avoid dangling pointer
```

```cpp
// C++ — RAII (Resource Acquisition Is Initialization)
{
    std::vector<int> data = {1, 2, 3};
    // data automatically freed when it goes out of scope
}

// Raw pointers (avoid in modern C++)
int* p = new int(42);
delete p;  // must manually delete
```

### Rust — Ownership

Rust eliminates manual memory management without a GC through its ownership system:

```rust
fn main() {
    let s1 = String::from("hello");  // s1 owns the string
    let s2 = s1;                      // ownership moves to s2
    // println!("{}", s1);            // COMPILE ERROR: s1 no longer valid

    let s3 = s2.clone();             // deep copy — both valid
    println!("{} {}", s2, s3);       // both valid
}
// s2 and s3 automatically freed here (Rust's Drop trait)
```

---

## Memory Leaks

A memory leak occurs when allocated memory is never freed, even though it's no longer needed.

### Common Causes

| Cause | Language | Example |
|---|---|---|
| Missing `free`/`delete` | C, C++ | Forgot to release allocated memory |
| Circular references | Python (pre-3.4), JavaScript | Two objects reference each other, preventing GC |
| Event listeners not removed | JavaScript | DOM listeners hold references to objects |
| Static collections growing | Java | Adding to a static `List` that's never cleared |
| Uncleared caches | Any | Cache that grows without eviction policy |

### Detection

| Tool | Language | What It Detects |
|---|---|---|
| **Valgrind** | C, C++ | Uninitialised reads, leaks, invalid frees |
| **AddressSanitizer** | C, C++, Rust | Buffer overflows, use-after-free |
| **VisualVM / JConsole** | Java | Heap dumps, GC activity, memory usage |
| **Chrome DevTools** | JavaScript | Heap snapshots, detached DOM nodes |
| **memory_profiler** | Python | Line-by-line memory usage |

---

## References: Strong, Weak, Soft (Java)

| Reference Type | Prevents GC? | Use Case |
|---|---|---|
| **Strong** | ✅ Yes | Normal object references (default) |
| **Weak** | ❌ No | Caches, observer patterns — GC can reclaim at any time |
| **Soft** | ⚠️ Only under memory pressure | Memory-sensitive caches — GC reclaims only when memory is low |

```java
// Weak reference — doesn't prevent GC
WeakReference<BigObject> weak = new WeakReference<>(new BigObject());
BigObject obj = weak.get();  // may return null if GC'd

// WeakHashMap — entries are auto-removed when key is GC'd
WeakHashMap<CacheKey, Data> cache = new WeakHashMap<>();
```

---

## String Interning and String Pool

Java maintains a special memory region called the **String Pool** (in the heap) for string literals.

```java
String a = "hello";          // stored in string pool
String b = "hello";          // reuses the same pooled object
System.out.println(a == b);  // true — same reference

String c = new String("hello");  // NEW object on heap, NOT pooled
System.out.println(a == c);      // false — different references
System.out.println(a.equals(c)); // true — same content

String d = c.intern();           // returns pooled reference
System.out.println(a == d);      // true
```

> String interning saves memory when many identical strings exist (e.g., parsing CSV files with repeated values). Be careful — the pool is a form of cache that consumes memory too.

---

## Immutability and Memory Implications

Immutable objects cannot change state after creation. This has direct memory consequences:

| Aspect | Mutable | Immutable |
|---|---|---|
| **Modifications** | Modify in-place (no new allocation) | Must create a new object |
| **Thread safety** | Requires synchronisation | Inherently thread-safe |
| **Sharing** | Risky (side effects) | Safe (share freely) |
| **Memory overhead** | Lower for frequent updates | Higher for frequent updates |

```java
// Mutable — modifies in place
StringBuilder sb = new StringBuilder("hello");
sb.append(" world");  // same object modified

// Immutable — creates new object
String s = "hello";
s = s + " world";  // new String object created (old one may be GC'd)
```

> For frequently concatenated strings in loops, use `StringBuilder` to avoid creating many intermediate `String` objects.

---

## Common Pitfalls

### Dangling Pointers (C/C++)

```c
// ❌ DANGEROUS — pointer to freed memory
int* p = (int*)malloc(sizeof(int));
*p = 42;
free(p);
printf("%d", *p);  // UNDEFINED BEHAVIOUR — dangling pointer

// ✅ CORRECT — nullify after free
free(p);
p = NULL;
if (p != NULL) {
    printf("%d", *p);  // safe — never reached
}
```

### Memory Leaks from Event Listeners (JavaScript)

```javascript
// ❌ LEAK — listener holds reference to large data
function setup() {
    const largeData = new Array(1000000).fill("*");
    document.getElementById("btn").addEventListener("click", () => {
        console.log(largeData.length);  // largeData never GC'd
    });
}

// ✅ FIX — remove listener when done
function setup() {
    const largeData = new Array(1000000).fill("*");
    const handler = () => console.log(largeData.length);
    const btn = document.getElementById("btn");
    btn.addEventListener("click", handler);
    // Later:
    btn.removeEventListener("click", handler);
}
```

### Circular References (Python)

```python
# ❌ Potential leak (pre-Python 3.4 without cyclic GC)
class Node:
    def __init__(self):
        self.parent = None
        self.children = []

a = Node()
b = Node()
a.children.append(b)
b.parent = a  # circular reference
del a, b  # may not be collected without cyclic GC

# ✅ Use weakref for back-references
import weakref
class Node:
    def __init__(self):
        self._parent = None  # weak reference
        self.children = []

    @property
    def parent(self):
        return self._parent() if self._parent else None

    @parent.setter
    def parent(self, node):
        self._parent = weakref.ref(node) if node else None
```

---

## Memory Management Strategies by Language

| Language | Strategy | Developer Responsibility |
|---|---|---|
| **C** | Manual (`malloc`/`free`) | Must allocate and free explicitly |
| **C++** | RAII + smart pointers | Use `unique_ptr`, `shared_ptr`; avoid raw `new`/`delete` |
| **Rust** | Ownership + borrowing (no GC) | Compiler enforces correctness at compile time |
| **Java** | Garbage Collection (generational) | Generally none; use `WeakReference` for caches |
| **Go** | Garbage Collection (concurrent tri-color) | Generally none; avoid unnecessary allocations |
| **Python** | Reference counting + cyclic GC | Generally none; use `weakref` for circular refs |
| **JavaScript** | Mark-and-sweep GC | Remove unused event listeners; avoid closures over large data |
| **C#** | GC (generational, concurrent) + `IDisposable` | Implement `IDisposable` for unmanaged resources |

---

## Sources

- *Effective Java* (3rd ed.) — Joshua Bloch, Items 6-7 (Cleaners, try-with-resources)
- JVM Specification — Run-Time Data Areas
- The Rust Book — Understanding Ownership (doc.rust-lang.org)
- MDN — Memory Management (developer.mozilla.org)
- Python Docs — `weakref` module
- *What Every Programmer Should Know About Memory* — Ulrich Drepper
