---
tags:
  - programming
  - fundamental
  - oop
---

# 06 Object-Oriented Programming

Object-Oriented Programming (OOP) models software as interacting objects that bundle data and behaviour together. It is the dominant paradigm in enterprise Java, C#, and C++ development. Understanding its four pillars — and its limitations — is critical for designing maintainable systems.

---

## The Four Pillars

### 1. Encapsulation

**Definition:** Bundling data (fields) and the methods that operate on that data into a single unit (class), and restricting direct access to internal state.

**Why it matters:** Prevents external code from putting an object into an invalid state. Changes to internal representation don't break callers.

```java
// ✅ GOOD — encapsulated
public class BankAccount {
    private double balance;

    public void deposit(double amount) {
        if (amount <= 0) throw new IllegalArgumentException("Amount must be positive");
        balance += amount;
    }

    public double getBalance() {
        return balance;
    }
}

// ❌ BAD — exposed state
public class BankAccount {
    public double balance;  // anyone can set balance = -999999
}
```

### 2. Abstraction

**Definition:** Exposing only the essential features of an object while hiding implementation details behind a clean interface.

**Why it matters:** Reduces complexity. Callers don't need to know *how* something works, only *what* it does.

```java
// Abstraction — the caller doesn't care about the sorting algorithm
public interface Sortable {
    void sort();
}

public class QuickSort implements Sortable {
    @Override
    public void sort() { /* quicksort implementation */ }
}
```

### 3. Inheritance

**Definition:** A mechanism where a subclass (child) inherits fields and methods from a superclass (parent), establishing an "is-a" relationship.

**Why it matters:** Promotes code reuse and models hierarchical relationships.

```java
public class Animal {
    protected String name;

    public void eat() {
        System.out.println(name + " is eating.");
    }
}

public class Dog extends Animal {
    public void bark() {
        System.out.println(name + " says Woof!");
    }
}

Dog dog = new Dog();
dog.name = "Rex";
dog.eat();   // inherited
dog.bark();  // own method
```

### 4. Polymorphism

**Definition:** The ability for different types to be treated as a common supertype. The correct method is invoked at runtime based on the actual object type.

**Why it matters:** Enables writing flexible, extensible code. New types can be added without modifying existing logic.

```java
// Polymorphism — same method call, different behaviour
public abstract class Shape {
    public abstract double area();
}

public class Circle extends Shape {
    private double radius;
    public Circle(double r) { this.radius = r; }
    @Override
    public double area() { return Math.PI * radius * radius; }
}

public class Rectangle extends Shape {
    private double w, h;
    public Rectangle(double w, double h) { this.w = w; this.h = h; }
    @Override
    public double area() { return w * h; }
}

// Client code works with any Shape
List<Shape> shapes = List.of(new Circle(5), new Rectangle(3, 4));
for (Shape s : shapes) {
    System.out.println(s.area());  // correct method called at runtime
}
```

---

## Class vs Object

| Concept | Description |
|---|---|
| **Class** | A blueprint/template — defines fields and methods |
| **Object** | An instance of a class — occupies memory, has actual values |

```java
// Class = blueprint
public class Car {
    private String model;
    private int year;

    public Car(String model, int year) {
        this.model = model;
        this.year = year;
    }
}

// Objects = instances
Car car1 = new Car("Civic", 2023);  // instance 1
Car car2 = new Car("Model 3", 2024);  // instance 2
```

---

## Constructors

| Type | Description | Example |
|---|---|---|
| **Default** | No-arg constructor (compiler-generated if none defined) | `new Car()` |
| **Parameterized** | Accepts arguments to initialise fields | `new Car("Civic", 2023)` |
| **Copy** | Creates a new object from an existing one | `new Car(other)` |

```java
public class User {
    private String name;
    private int age;

    // Default (explicit)
    public User() { this("Unknown", 0); }

    // Parameterized
    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Copy constructor
    public User(User other) {
        this.name = other.name;
        this.age = other.age;
    }
}
```

> Java has no built-in copy constructor. C++ and Python have idiomatic support (`clone()`, copy constructors, `copy.deepcopy()`).

---

## Access Modifiers (Java)

| Modifier | Class | Package | Subclass | World |
|---|---|---|---|---|
| `public` | ✅ | ✅ | ✅ | ✅ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| *(package-private)* | ✅ | ✅ | ❌ | ❌ |
| `private` | ✅ | ❌ | ❌ | ❌ |

---

## Abstract Classes vs Interfaces

| Aspect | Abstract Class | Interface |
|---|---|---|
| **Can have state (fields)?** | ✅ Instance fields | ❌ Only constants (pre Java 8) |
| **Can have constructors?** | ✅ | ❌ |
| **Multiple inheritance?** | ❌ (one class only) | ✅ (implement many) |
| **Method implementations?** | ✅ Concrete methods | ✅ (default methods since Java 8) |
| **Use when** | Shared base with state and partial implementation | Defining a contract / capability |

```java
// Interface — defines a contract
public interface Drawable {
    void draw(Graphics g);
    default void clear() { /* default implementation */ }
}

// Abstract class — shared base with state
public abstract class Vehicle {
    protected int speed;
    public abstract void accelerate();
    public void brake() { speed = 0; }  // concrete method
}
```

> **Rule of thumb:** Start with an interface. Add an abstract class only when you need shared state or partial implementation.

---

## Composition vs Inheritance

| Aspect | Inheritance | Composition |
|---|---|---|
| **Relationship** | "is-a" (Dog *is an* Animal) | "has-a" (Car *has an* Engine) |
| **Coupling** | Tight (changes in parent affect child) | Loose (delegate to owned objects) |
| **Flexibility** | Static (determined at compile time) | Dynamic (can swap at runtime) |
| **Fragile base class?** | ✅ Risk | ❌ Not affected |

```java
// ❌ Inheritance — fragile, tight coupling
public class ArrayList extends AbstractList { }

// ✅ Composition — flexible, loose coupling
public class UserRepository {
    private final Database db;  // "has-a"

    public UserRepository(Database db) {
        this.db = db;
    }

    public User findById(int id) {
        return db.query("SELECT * FROM users WHERE id = ?", id);
    }
}
```

> **Favor composition over inheritance** is a foundational design principle. See [[01 Decomposition Patterns]] for structural decomposition strategies.

---

## SOLID Principles (Brief Overview)

| Principle | Meaning | Core Idea |
|---|---|---|
| **S** — Single Responsibility | A class should have one reason to change | One job per class |
| **O** — Open/Closed | Open for extension, closed for modification | Use interfaces/polymorphism |
| **L** — Liskov Substitution | Subtypes must be substitutable for their base types | Don't break the contract |
| **I** — Interface Segregation | Many specific interfaces > one fat interface | Don't force unused methods |
| **D** — Dependency Inversion | Depend on abstractions, not concretions | Program to interfaces |

---

## Static vs Instance Members

| Aspect | Static | Instance |
|---|---|---|
| **Belongs to** | The class itself | Each object |
| **Accessed via** | `ClassName.member` | `object.member` |
| **Can access instance fields?** | ❌ | ✅ |
| **Use case** | Utility methods, constants, factories | Stateful behaviour |

```java
public class MathUtils {
    public static final double PI = 3.14159265358979;  // static constant

    public static int clamp(int value, int min, int max) {  // static utility
        return Math.max(min, Math.min(max, value));
    }
}
```

---

## Common Pitfalls

### God Classes

❌ A single class doing everything — 2000+ lines, 50+ methods, impossible to test.
✅ Split into focused classes with single responsibilities.

### Deep Inheritance Hierarchies

❌ `Dog extends Mammal extends Animal extends LivingThing extends Object` — fragile, hard to reason about.
✅ Prefer 1-2 levels of inheritance. Use composition for shared behaviour.

### Tight Coupling

❌ Every class directly instantiates its dependencies (`new MySqlDatabase()`).
✅ Depend on interfaces; inject implementations (Dependency Injection).

---

## Sources

- *Clean Code* — Robert C. Martin, Chapters 6 & 10
- *Effective Java* (3rd ed.) — Joshua Bloch, Items 19-21
- *Head First Design Patterns* — Freeman & Robson
- SOLID principles — Robert C. Martin (butunclebob.com)
