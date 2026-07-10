---
tags:
  - design-patterns
  - creational
  - programming
---

# 01 Creational Patterns

Creational patterns deal with **object creation mechanisms**. Instead of instantiating objects directly with `new`, these patterns give you flexibility in *what* gets created, *when*, and *how*.

---

## Quick Reference

| Pattern | Intent | When to Use | Analogy |
|---------|--------|-------------|---------|
| **Factory Method** | Let subclasses decide which class to instantiate | You don't know the exact type at compile time | Restaurant menu — you order "pizza," kitchen decides the rest |
| **Abstract Factory** | Create families of related objects | Multiple product variants that must be consistent | IKEA furniture series — all pieces match one style |
| **Builder** | Construct complex objects step by step | Object has many optional parameters | Building a house — foundation, walls, roof, in order |
| **Singleton** | Ensure only one instance exists | Global resource: config, connection pool, logger | Government — only one president at a time |
| **Prototype** | Clone existing objects | Creating from scratch is expensive | Photocopier — clone the document instead of rewriting |

---

## Factory Method

**Problem:** You need to create objects, but the exact class depends on context (config, user input, environment).

```java
// Creator declares the factory method
abstract class NotificationService {
    abstract Notification createNotification();

    void send(String message) {
        Notification n = createNotification();
        n.deliver(message);
    }
}

// Concrete creators override the factory method
class EmailService extends NotificationService {
    Notification createNotification() { return new EmailNotification(); }
}
class SmsService extends NotificationService {
    Notification createNotification() { return new SmsNotification(); }
}
```

```typescript
// TypeScript: function-based factory
interface Notification { deliver(msg: string): void }

function createNotification(type: "email" | "sms"): Notification {
    if (type === "email") return new EmailNotification();
    return new SmsNotification();
}
```

---

## Abstract Factory

**Problem:** You need to create *families* of related objects (e.g., UI components for dark/light theme) that must be consistent.

```java
interface UIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

class DarkThemeFactory implements UIFactory {
    public Button createButton() { return new DarkButton(); }
    public Checkbox createCheckbox() { return new DarkCheckbox(); }
}

class LightThemeFactory implements UIFactory {
    public Button createButton() { return new LightButton(); }
    public Checkbox createCheckbox() { return new LightCheckbox(); }
}

// Client code works with any factory — no concrete types
void buildUI(UIFactory factory) {
    Button btn = factory.createButton();
    Checkbox cb = factory.createCheckbox();
}
```

---

## Builder

**Problem:** Object has many parameters (some optional). Constructor telescoping is unreadable. Think of `StringBuilder` — you append piece by piece, then call `build()`.

```java
// ❌ Bad: telescoping constructor
Order order = new Order("John", "123 Main St", null, null, "USD", true, null);
```

```java
// ✅ Good: Builder pattern
public class Order {
    private final String customer;
    private final String address;
    private final String currency;
    private final boolean express;

    private Order(Builder b) {
        this.customer = b.customer;
        this.address = b.address;
        this.currency = b.currency;
        this.express = b.express;
    }

    public static class Builder {
        private String customer;
        private String address;
        private String currency = "USD";
        private boolean express = false;

        public Builder(String customer) { this.customer = customer; }
        public Builder address(String a)  { this.address = a; return this; }
        public Builder currency(String c) { this.currency = c; return this; }
        public Builder express(boolean e) { this.express = e; return this; }
        public Order build() { return new Order(this); }
    }
}

// Usage — readable, flexible
Order order = new Order.Builder("John")
    .address("123 Main St")
    .currency("EUR")
    .express(true)
    .build();
```

```typescript
// TypeScript: same idea, method chaining
const order = new OrderBuilder("John")
    .address("123 Main St")
    .currency("EUR")
    .express(true)
    .build();
```

---

## Singleton

**Problem:** Exactly one instance must exist (config manager, connection pool, logging).

```java
// ❌ Bad: not thread-safe, can be broken with reflection/serialization
public class ConfigManager {
    private static ConfigManager instance;
    public static ConfigManager getInstance() {
        if (instance == null) instance = new ConfigManager(); // race condition
        return instance;
    }
}
```

```java
// ✅ Good: enum singleton — thread-safe, serialization-safe, reflection-proof
public enum ConfigManager {
    INSTANCE;

    private final Map<String, String> settings = new HashMap<>();

    public String get(String key) { return settings.get(key); }
    public void set(String key, String value) { settings.put(key, value); }
}

// Usage
ConfigManager.INSTANCE.set("timeout", "30");
```

**When to avoid Singleton:** It's a global state in disguise. Hard to test, creates hidden coupling. Prefer dependency injection (Spring `@Scope("singleton")` is the framework-managed version).

---

## Prototype

**Problem:** Creating an object is expensive (deep graph, DB fetch, complex initialization). Clone an existing one instead.

```java
public class GameCharacter implements Cloneable {
    private String name;
    private Stats stats;          // complex object
    private List<Item> inventory;  // deep structure

    @Override
    public GameCharacter clone() {
        GameCharacter copy = (GameCharacter) super.clone();
        copy.stats = this.stats.clone();              // deep copy
        copy.inventory = new ArrayList<>(this.inventory);
        return copy;
    }
}

// Usage: clone a template character, then customize
GameCharacter warrior = templateCharacter.clone();
warrior.setName("Conan");
```

**Caveat:** `Cloneable` is considered a broken API — prefer copy constructors or a static factory method (`GameCharacter.of(template)`).

---

## Decision Flow

```
Need to create objects?
├─ Multiple possible types? → Factory Method
├─ Families of related types? → Abstract Factory
├─ Many optional config params? → Builder
├─ Must be exactly one instance? → Singleton
└─ Expensive to create from scratch? → Prototype
```

---

## Sources

- Gamma et al. — *Design Patterns* (1994), Chapters 3-7
- Joshua Bloch — *Effective Java*, Items 1-7 (Builder, Singleton)
- Refactoring Guru — https://refactoring.guru/design-patterns/creational-patterns
