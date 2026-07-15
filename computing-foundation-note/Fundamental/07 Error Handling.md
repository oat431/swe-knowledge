---
tags:
  - programming
  - fundamental
  - error-handling
---

# 07 Error Handling

Errors are inevitable — files don't exist, networks drop, users input garbage. Robust programs don't crash; they detect, report, and recover gracefully. Good error handling is the difference between software that works in the lab and software that survives production.

---

## Exceptions: Checked vs Unchecked (Java)

| Type | Superclass | Compiler Enforces Handling? | Examples |
|---|---|---|---|
| **Checked** | `Exception` (not `RuntimeException`) | ✅ Must catch or declare | `IOException`, `SQLException`, `FileNotFoundException` |
| **Unchecked** | `RuntimeException` | ❌ Optional | `NullPointerException`, `ArrayIndexOutOfBoundsException`, `IllegalArgumentException` |
| **Error** | `Error` | ❌ Should never catch | `OutOfMemoryError`, `StackOverflowError` |

```java
// Checked — compiler forces you to handle it
try {
    FileReader file = new FileReader("data.txt");
} catch (FileNotFoundException e) {
    System.err.println("File not found: " + e.getMessage());
}

// Unchecked — no compiler enforcement, but still catchable
int[] arr = {1, 2, 3};
try {
    int val = arr[10];  // ArrayIndexOutOfBoundsException
} catch (ArrayIndexOutOfBoundsException e) {
    System.err.println("Index out of bounds");
}
```

> Python, JavaScript, and TypeScript have only unchecked exceptions — everything is a `RuntimeException` equivalent.

---

## try / catch / finally Mechanics

```java
try {
    // risky code
    String data = readFile("config.json");
    process(data);
} catch (FileNotFoundException e) {
    // handle specific exception
    log.error("Config file missing", e);
    useDefaults();
} catch (IOException e) {
    // handle broader exception
    log.error("I/O error reading config", e);
} finally {
    // ALWAYS executes (whether exception occurred or not)
    cleanup();
}
```

| Block | Executes When |
|---|---|
| `try` | Always (until an exception is thrown) |
| `catch` | Only if a matching exception was thrown |
| `finally` | Always — used for cleanup (closing files, releasing locks) |

---

## Exception Hierarchy (Java)

```
Throwable
├── Error (don't catch)
│   ├── OutOfMemoryError
│   ├── StackOverflowError
│   └── ...
└── Exception
    ├── IOException (checked)
    ├── SQLException (checked)
    ├── ReflectiveOperationException (checked)
    └── RuntimeException (unchecked)
        ├── NullPointerException
        ├── IllegalArgumentException
        ├── ArrayIndexOutOfBoundsException
        ├── ClassCastException
        └── ...
```

---

## Custom Exceptions

Create domain-specific exceptions to express business errors clearly.

```java
// Custom checked exception
public class InsufficientFundsException extends Exception {
    private final double deficit;

    public InsufficientFundsException(double balance, double amount) {
        super(String.format("Cannot withdraw %.2f: balance is %.2f", amount, balance));
        this.deficit = amount - balance;
    }

    public double getDeficit() { return deficit; }
}

// Usage
public void withdraw(double amount) throws InsufficientFundsException {
    if (amount > balance) {
        throw new InsufficientFundsException(balance, amount);
    }
    balance -= amount;
}
```

---

## The "Catch Specific, Throw Early" Principle

### Throw Early (fail fast)

```java
// ✅ GOOD — validate immediately, throw at the source
public void setAge(int age) {
    if (age < 0 || age > 150) {
        throw new IllegalArgumentException("Invalid age: " + age);
    }
    this.age = age;
}

// ❌ BAD — let invalid data propagate, fail much later
public void setAge(int age) {
    this.age = age;  // stores garbage, fails somewhere downstream
}
```

### Catch Specific

```java
// ❌ BAD — catches everything, hides real bugs
try {
    parseConfig(file);
} catch (Exception e) {
    log.error("Something went wrong");  // swallowed silently
}

// ✅ GOOD — catches specific, handles appropriately
try {
    parseConfig(file);
} catch (FileNotFoundException e) {
    log.warn("Config file missing, using defaults");
} catch (JsonParseException e) {
    log.error("Config file is malformed", e);
    throw new ConfigurationException("Invalid config", e);
}
```

---

## Resource Management

### Java — try-with-resources

```java
// ✅ GOOD — resource auto-closed when block exits
try (BufferedReader reader = new BufferedReader(new FileReader("data.txt"))) {
    String line;
    while ((line = reader.readLine()) != null) {
        process(line);
    }
}  // reader.close() called automatically, even if exception occurs

// ❌ BAD — manual close, risks resource leak
BufferedReader reader = new BufferedReader(new FileReader("data.txt"));
try {
    // ... use reader
} finally {
    reader.close();  // what if an exception is thrown in close()?
}
```

### Python — context managers

```python
# ✅ GOOD — with statement ensures cleanup
with open("data.txt") as f:
    for line in f:
        process(line)
# f.close() called automatically

# Custom context manager
from contextlib import contextmanager

@contextmanager
def managed_resource():
    resource = acquire()
    try:
        yield resource
    finally:
        release(resource)
```

### C# — using statement

```csharp
using (var reader = new StreamReader("data.txt"))
{
    string line;
    while ((line = reader.ReadLine()) != null)
    {
        Process(line);
    }
}  // reader.Dispose() called automatically
```

---

## Error Handling Patterns

### Result Type (Rust / Go)

Instead of exceptions, return a value that encodes success or failure.

```rust
// Rust — Result<T, E>
fn read_file(path: &str) -> Result<String, io::Error> {
    fs::read_to_string(path)
}

match read_file("data.txt") {
    Ok(content) => println!("{}", content),
    Err(e) => eprintln!("Error: {}", e),
}
```

```go
// Go — multiple return values
content, err := os.ReadFile("data.txt")
if err != nil {
    log.Fatal(err)
}
fmt.Println(string(content))
```

### Option / Maybe Monad

Represents a value that might be absent — eliminates null.

```java
// Java — Optional<T>
Optional<String> name = findUser(id)
    .map(User::getName)
    .filter(n -> !n.isBlank());

name.orElse("Anonymous");
```

```typescript
// TypeScript — null-safe patterns
const name = user?.profile?.name ?? "Anonymous";
```

---

## Defensive Programming

### Null Checks

```java
// ✅ Validate inputs
public void process(String input) {
    Objects.requireNonNull(input, "input must not be null");
    // ...
}
```

### Input Validation

```java
public void setRange(int start, int end) {
    if (start < 0) throw new IllegalArgumentException("start must be >= 0");
    if (end < start) throw new IllegalArgumentException("end must be >= start");
    this.start = start;
    this.end = end;
}
```

### Assertions

```java
// Assertions — for development/testing, not production
assert list.size() > 0 : "List should not be empty at this point";
// Disabled by default in JVM; enable with -ea flag
```

---

## Logging vs Throwing

| Situation | Action |
|---|---|
| Caller can recover | **Throw** — let the caller decide |
| You're the final handler (e.g., top-level request handler) | **Log + handle** — log the error, return error response |
| Unexpected invariant violated | **Throw** (or assert) — this is a bug, not a user error |
| Informational / diagnostic | **Log** only — no exception needed |
| Caught, handled, and re-thrown | **Log + throw** — log context, then re-throw |

---

## Common Pitfalls

### ❌ Swallowing Exceptions

```java
// ❌ NEVER DO THIS
try {
    riskyOperation();
} catch (Exception e) {
    // silent failure — the worst kind of bug
}
```

### ✅ At Minimum, Log It

```java
// ✅ Log with context
try {
    riskyOperation();
} catch (SpecificException e) {
    log.error("Failed during riskyOperation: {}", e.getMessage(), e);
    throw new ServiceException("Operation failed", e);
}
```

### ❌ Catching `Exception` or `Throwable`

```java
// ❌ Hides real bugs — NullPointerException, ClassCastException, etc.
try {
    // 100 lines of code
} catch (Exception e) {
    // "handles" everything, including programmer errors
}
```

### ✅ Catch Specific Exceptions

```java
// ✅ Catch only what you expect and can handle
try {
    readFile(path);
} catch (FileNotFoundException e) {
    createDefaultFile(path);
} catch (IOException e) {
    throw new DataLoadException("Cannot read: " + path, e);
}
```

### ❌ Missing finally / try-with-resources

```java
// ❌ Resource leak
Connection conn = dataSource.getConnection();
conn.query("SELECT * FROM users");
// what if an exception is thrown? Connection never closed.

// ✅ Safe
try (Connection conn = dataSource.getConnection()) {
    conn.query("SELECT * FROM users");
}
```

> For security implications of error handling (information leakage, denial of service), see [[01 Common Web Attacks]].

---

## Sources

- *Effective Java* (3rd ed.) — Joshua Bloch, Items 69-73
- *Clean Code* — Robert C. Martin, Chapter 7: Error Handling
- Oracle Java Tutorials — Exceptions
- Rust Book — Error Handling (doc.rust-lang.org)
