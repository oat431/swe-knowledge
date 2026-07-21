---
tags:
  - legacy-code
  - dependency-breaking
  - techniques
  - catalog
  - software-maintenance
source: "Feathers, Working Effectively with Legacy Code, Ch 25"
created: 2026-07-21
---

# 06 — Dependency-Breaking Catalog

> Source: Michael Feathers, *Working Effectively with Legacy Code*, Chapter 25.  
> These techniques are **refactorings performed without tests** to get tests in place. They preserve behavior. Once tests exist, use test-supported refactorings to clean the design.

---

## Overview

The catalog below contains 24 dependency-breaking techniques. Each entry includes:

- **Purpose** — the dependency problem it addresses
- **Mechanism** — how it works at a high level
- **Steps** — the safe sequence to apply without tests
- **Trade-offs & Pitfalls** — when to prefer it, when to avoid it

> **Golden Rule:** Bias toward the change you feel most confident making, not the one that gives the best structure. Better structure comes *after* tests.

---

## 1. Adapt Parameter

**Purpose:** Replace a difficult-to-fake parameter type (e.g., `HttpServletRequest`) with a narrow, purpose-built interface.

**Mechanism:** Create a new interface that exposes only the methods the method actually needs. Wrap the original parameter in an adapter that implements the new interface.

**Steps:**
1. Create a new interface — as simple and communicative as possible.
2. Create a production implementer (adapter) wrapping the original type.
3. Create a fake implementer for testing.
4. Write a simple test case passing the fake.
5. Modify the method to use the new parameter type.
6. Run the test to verify.

**When to Use:** When `Extract Interface` is impossible (e.g., extracting from an existing interface in Java) or the parameter's class is low-level/implementation-specific.

**Pitfall:** If the simplified interface is too different from the original, subtle bugs can be introduced. Prefer small, safe changes.

---

## 2. Break Out Method Object

**Purpose:** Move a long, hard-to-test method into its own class so it can be tested independently.

**Mechanism:** Create a new "method object" class. Its constructor accepts the original class reference + the method's arguments. Copy the method body into an execution method (`run()`, `draw()`). Use `Extract Interface` on the original class dependency afterward.

**Steps:**
1. Create a new class to house the method code.
2. Create a constructor: copy the original method's arguments. If instance data/methods are used, add a reference to the original class as the first argument.
3. Declare instance variables for each constructor argument; assign them in the constructor.
4. Create an empty execution method (often `run()`).
5. Copy the body of the old method into the execution method; compile and **Lean on the Compiler**.
6. Fix errors by making methods public or introducing getters on the original class.
7. Change the original method to create an instance of the new class and delegate.
8. If needed, use `Extract Interface` to break the dependency on the original class.

**Variations:**
- If the method uses no instance data: no reference to original class needed.
- If the method only reads data: consider a data-holding class passed as argument.
- Worst case (uses methods on original class): combine with `Extract Interface`.

**Pitfall:** Private methods become public — this is a temporary state. Refactor back after tests are in place.

---

## 3. Definition Completion

**Purpose:** (C/C++ only) Provide alternative definitions of methods in a test file, replacing production definitions at link time.

**Mechanism:** Include the class header in the test file and define methods directly there. The test executable links against the test-file definitions instead of production ones.

**Steps:**
1. Identify a class with definitions you want to replace.
2. Verify method definitions are in a source file, not a header.
3. Include the header in the test source file.
4. Verify source files for the class are NOT part of the test build.
5. Build to find missing methods.
6. Add method definitions to the test source file until the build completes.

**Pitfall:** Creates duplicate definitions — maintenance burden. Confuses debuggers. Use only for initial dependency breaking, then bring the class under test and remove duplicates.

---

## 4. Encapsulate Global References

**Purpose:** Move global variables or free functions into a class to create an object seam.

**Mechanism:** Create a new class holding the globals as public members. Replace all direct references with `instance.member`. Then parameterize or use setters to inject fakes.

**Steps (for global data):**
1. Identify the globals to encapsulate.
2. Create a class to hold them.
3. Copy the globals into the class; handle initialization.
4. Comment out original declarations.
5. Declare a global instance of the new class.
6. **Lean on the Compiler** to find unresolved references.
7. Precede each reference with the global instance name.
8. Use `Introduce Static Setter`, `Parameterize Constructor`, `Parameterize Method`, or `Replace Global Reference with Getter` to inject fakes.

**Steps (for free functions):**
1. Create an interface class with pure virtual methods for each free function.
2. Create a production subclass that delegates to the real global functions.
3. Create a fake subclass for testing.
4. Parameterize the consuming class to accept the interface.

**Heuristic:** If several globals are always used/modified together, they belong in the same class.

---

## 5. Expose Static Method

**Purpose:** Test a method on a class that can't be instantiated, when the method doesn't use instance data.

**Mechanism:** Make the method `public static` (or extract its body to a new static method). Static methods can be called without instantiating the class.

**Steps:**
1. Write a test that accesses the target method as a public static method.
2. Extract the method body to a static method. **Preserve Signatures** — use a different name (e.g., `validate` → `validatePacket`).
3. Compile.
4. If errors arise from accessing instance data/methods, consider making those static too (if safe).

**Insight:** The static area of a class is effectively a separate "staging area." Making a method static signals it doesn't belong to the instance — a clue for future refactoring.

---

## 6. Extract and Override Call

**Purpose:** Break a dependency on a single method call (often static methods or globals).

**Mechanism:** Extract the problematic call into a new, overridable method on the same class. Override it in a testing subclass.

**Steps:**
1. Identify the call to extract. Copy its method signature.
2. Create a new method on the current class with the copied signature.
3. Copy the call body into the new method; replace the original call site with a call to the new method.

**When to Prefer:** When there's a single problematic call. If many calls exist against the same global, use `Replace Global Reference with Getter` instead.

---

## 7. Extract and Override Factory Method

**Purpose:** Replace object creation in a constructor by extracting it into an overridable factory method.

**Mechanism:** Move the `new` expression into a `protected` factory method. Subclass and override to return a fake.

**Steps:**
1. Identify object creation in a constructor.
2. Extract all creation work into a factory method.
3. Create a testing subclass and override the factory method.

**Language Note:** Does NOT work in C++ (virtual calls from constructors don't dispatch to derived classes). Use `Extract and Override Getter` or `Supersede Instance Variable` instead.

---

## 8. Extract and Override Getter

**Purpose:** Replace an object created in a constructor when the language doesn't support virtual calls in constructors (C++).

**Mechanism:** Introduce a lazy getter — creates the object on first call, returns the cached instance thereafter. Override the getter in a testing subclass.

**Steps:**
1. Identify the object needing a getter.
2. Extract all creation logic into a getter.
3. Replace all uses of the object with calls to the getter; initialize the reference to null in constructors.
4. Add lazy-initialization logic (create if null).
5. Subclass and override the getter to provide an alternative object.

**Pitfall:** Risk of using the variable before initialization. Ensure ALL code uses the getter. Watch object lifetime in non-GC languages (C++).

---

## 9. Extract Implementer

**Purpose:** Turn a class into an interface when the class name is the ideal name for the interface.

**Mechanism:** Copy the class declaration to a new production class. Strip the original down to pure virtual methods (an interface). Make the production class inherit from the new interface.

**Steps:**
1. Copy the source class declaration; rename it (e.g., `ProductionModelNode`).
2. Turn the source class into an interface: delete non-public methods and variables.
3. Make all remaining public methods abstract (pure virtual in C++).
4. Remove unnecessary imports/includes — **Lean on the Compiler**.
5. Make the production class implement the new interface.
6. Compile and fix.
7. Find all instantiations of the old class; replace with the production class.

**When Hierarchy Exists:** If the class has a superclass/subclass, repeat the pattern at each level, inserting production classes between the interface and subclasses.

**Alternative:** If you can pick a different name for the interface, `Extract Interface` is simpler.

---

## 10. Extract Interface

**Purpose:** Create an interface so a fake can be passed instead of a hard-to-instantiate dependency.

**Mechanism:** Create a new interface. Make the original class implement it. Change the client to use the interface type. Add methods to the interface incrementally as the compiler demands.

**Steps:**
1. Create a new empty interface.
2. Make the original class implement it (safe — interface is empty).
3. Change the client to use the interface type.
4. Compile. For each error, add a method declaration to the interface and an empty implementation to the fake.
5. Repeat until the build succeeds.

**Naming:** Avoid `I`-prefix unless it's already the convention. Think of a name that communicates the role. You don't need to extract ALL public methods — only the ones the client uses.

**C++ Non-Virtual Trap:** If the original class has non-virtual methods and subclasses that override them, making the method virtual (to add to the interface) changes dispatch behavior. Add a new virtual method that delegates to the non-virtual one instead.

---

## 11. Introduce Instance Delegator

**Purpose:** Break dependencies on static methods by providing instance methods that delegate to them.

**Mechanism:** Add an instance method that calls the static method. Parameterize the client to accept an instance, enabling substitution.

**Steps:**
1. Identify a problematic static method.
2. Create an instance method on the same class that delegates to the static method. **Preserve Signatures.**
3. Use `Parameterize Method` or similar to supply an instance where the static call was made.

**End State:** Eventually all calls go through instance methods. Then move static method bodies into the instance methods and delete the statics.

---

## 12. Introduce Static Setter

**Purpose:** Replace singletons or global factories with fakes for testing.

**Mechanism:** Add a static setter method that replaces the singleton instance. Make the constructor `protected` so a testing subclass can be created.

**Steps:**
1. Decrease constructor protection (private → protected).
2. Add a static setter that accepts a reference and destroys the old instance properly.
3. Subclass to override behavior, or extract an interface.

**For Global Factories:** Have the factory delegate to a replaceable `server` object. Use a static setter to swap the server.

**Cleanup:** Use `setUp`/`tearDown` to reset global state between tests.

**Pitfall:** Ugly — setters that change fundamental object dependencies make behavior history-dependent. This is a temporary surgical step. Move toward parameter passing over time.

---

## 13. Link Substitution

**Purpose:** (C and procedural languages) Replace function implementations at link time by linking against a fake library.

**Mechanism:** Create alternative function definitions (with the same signatures) in a separate file. Adjust the build to link the fake instead of the production version.

**Steps:**
1. Identify functions/classes to fake.
2. Produce alternative definitions.
3. Adjust the build to include alternatives instead of production versions.

**Use Case:** Faking external libraries, especially data sinks (graphics libraries). Also works in Java via classpath manipulation.

**Pitfall:** Creates separate executables. Can be a build maintenance burden.

---

## 14. Parameterize Constructor

**Purpose:** Externalize object creation from a constructor so a fake can be passed in.

**Mechanism:** Add a constructor parameter for the dependency. Keep the original constructor as a convenience that passes the default.

**Steps:**
1. Identify the constructor; make a copy of it.
2. Add a parameter for the object being created. Remove the `new` expression; assign from the parameter.
3. In the original constructor, call the new constructor with a `new` expression for the default.

**C++ Alternative:** Use a default argument in the existing constructor (but this forces header inclusion).

---

## 15. Parameterize Method

**Purpose:** Replace an object created inside a method by passing it as a parameter.

**Mechanism:** Add a parameter to the method. Keep the original signature as a forwarding method that passes the default.

**Steps:**
1. Identify the method; make a copy.
2. Add a parameter for the object. Remove creation; assign from parameter.
3. Delete the body of the copied method; make it call the parameterized method with a `new` expression.

**Alternative:** Use the parameter type in the method name (e.g., `runWithTestResult`) to avoid confusion when overloading.

---

## 16. Primitivize Parameter

**Purpose:** Work around a class that's too hard to instantiate by reducing the problem to primitive types.

**Mechanism:** Write a free function that operates on primitives (int, string, vector). Add a method to the original class that extracts the primitive representation and delegates to the free function.

**Steps:**
1. Develop a free function that does the work using an intermediate primitive-based representation.
2. Add a method to the class that builds the representation and delegates.

**Downsides (serious):**
- Exposes internal representation
- Duplicates data
- Pushes logic into free functions, hurting cohesion
- Prolongs the underlying dependency problem

**Use only as a last resort.** Plan to fold the function back into the class once tests are in place.

---

## 17. Pull Up Feature

**Purpose:** Test a cluster of methods without instantiating the class that has bad dependencies.

**Mechanism:** Create an abstract superclass. Pull up the methods you need (and their dependencies) into it. Create a concrete testing subclass.

**Steps:**
1. Identify methods to pull up.
2. Create an abstract superclass.
3. Copy methods to the superclass; compile.
4. Copy missing references (methods/variables) to the superclass as the compiler reports them. **Preserve Signatures.**
5. When both classes compile, create a testing subclass and add setup methods.

**Design Note:** This spreads a feature across two classes — not ideal. It's a step toward delegation. Make the superclass abstract to signal it's not meant for direct instantiation.

---

## 18. Push Down Dependency

**Purpose:** Separate problematic dependencies from the rest of a class by pushing them into a new subclass.

**Mechanism:** Make the original class abstract. Create a production subclass that contains all the problematic dependencies. Create a testing subclass that provides safe alternatives.

**Steps:**
1. Attempt to build the class in the test harness to identify problem dependencies.
2. Identify which dependencies cause build problems.
3. Create a new subclass named for the specific environment (e.g., `WindowsOffMarketTradeValidator`).
4. Copy instance variables and methods containing bad dependencies into the subclass. Make them `protected abstract` in the original; make the original abstract.
5. Create a testing subclass; change tests to instantiate it.
6. Build and verify.

**End State:** The original class holds pure logic; the production subclass holds environment-specific code. Eventually, delegate the environment-specific code to a separate collaborator.

---

## 19. Replace Function with Function Pointer

**Purpose:** (C) Break dependencies on C functions by replacing direct calls with calls through function pointers.

**Mechanism:** Declare a function pointer with the same name as the original function. Rename the original function. Initialize the pointer to the original at startup. Tests can reassign the pointer.

**Steps:**
1. Find declarations of functions to replace.
2. Create function pointers with the same names before each declaration.
3. Rename original function declarations (e.g., `db_store` → `db_store_production`).
4. Initialize pointers to original function addresses in a C file.
5. Build to find function bodies; rename them to match the new names.

**Advantage:** Happens entirely at compile time; minimal build system impact.  
**Pitfall:** Function pointers can be corrupted. Consider migrating to C++ for better seams.

---

## 20. Replace Global Reference with Getter

**Purpose:** Break dependency on a global/static access by routing access through an overridable getter.

**Mechanism:** Wrap the global access in a `protected` getter method. Replace all direct accesses with getter calls. Override the getter in a testing subclass.

**Steps:**
1. Identify the global reference.
2. Write a getter; ensure access protection allows overriding.
3. Replace all references to the global with getter calls.
4. Create a testing subclass and override the getter.

**Example (Java singleton):**
```java
protected Inventory getInventory() {
    return Inventory.getInventory();
}
// In test:
class TestingRegisterSale extends RegisterSale {
    Inventory inventory = new FakeInventory();
    protected Inventory getInventory() { return inventory; }
}
```

---

## 21. Subclass and Override Method

**Purpose:** The core OO dependency-breaking technique — use inheritance to nullify behavior or sense values during testing.

**Mechanism:** Change the access modifier of the target method (private → protected). Create a testing subclass that overrides it.

**Steps:**
1. Identify the smallest set of methods to override.
2. Make each method overridable (virtual in C++, non-final in Java, overridable in .NET).
3. Adjust visibility so the method can be overridden in a subclass.
4. Create a testing subclass; override the methods.
5. Verify it builds in the test harness.

**"Paper View" Metaphor:** Imagine placing translucent paper over the class — each extractable snippet can be replaced by an override in the subclass.

**Pitfall:** Overriding too-large methods can leave unwanted behavior intact. Aim for small, focused overrides.

---

## 22. Supersede Instance Variable

**Purpose:** (C++ primary) Replace an object created in a constructor when virtual calls can't be overridden in constructors.

**Mechanism:** Add a `supersedeXxx()` method that replaces the instance variable after construction. Call it in tests before using the object.

**Steps:**
1. Identify the instance variable to supersede.
2. Create a method named `supersedeXxx`.
3. In the method, destroy the previous instance and assign the new value. Verify no other references to the old object exist in the class.

**Naming:** Use the word "supersede" — it's unusual and searchable, making it easy to verify it's not used in production code.

**Pitfall:** Setters that change fundamental dependencies make behavior history-dependent. Prefer `Parameterize Constructor` or `Extract and Override Getter` when possible.

---

## 23. Template Redefinition

**Purpose:** (C++ generics) Break dependencies by parameterizing a class with a template parameter, then instantiating it with a fake type in tests.

**Mechanism:** Turn the class into a template parameterized by the types that need replacement. Use a `typedef` to preserve the original class name for production code.

**Steps:**
1. Identify features (variables) to replace.
2. Turn the class into a template, parameterizing by those variables. Copy method bodies into the header.
3. Rename the template (e.g., `AsyncReceptionPortImpl`).
4. Add `typedef` restoring the original name with the production type.
5. In tests, instantiate the template with fake types.

**Pitfall:** Code moves from `.cpp` to header files → increased compile-time dependencies. Prefer inheritance-based techniques in C++ unless the code is already templatized.

---

## 24. Text Redefinition

**Purpose:** (Ruby and other interpreted languages; C/C++ with preprocessor) Replace method definitions at the source level.

**Ruby Mechanism:** Reopen the class in the test file and redefine the method. The interpreter replaces the old definition.

**Steps (Ruby):**
1. Identify the class with definitions to replace.
2. Add `require` for the module at the top of the test file.
3. Provide alternative definitions for each method before the tests.

**Pitfall (Ruby):** The replacement persists for the entire program run. Can cause cross-test contamination.

**C/C++ Variant:** Use the preprocessor to redefine function names (see *Preprocessing Seam* in Chapter 4).

---

## Technique Selection Guide

| Situation | Recommended Technique |
|---|---|
| Can't fake a method parameter | **Adapt Parameter** |
| Long method, class hard to instantiate | **Break Out Method Object** |
| Global variables everywhere | **Encapsulate Global References** |
| Method uses no instance data | **Expose Static Method** |
| Single problematic method call | **Extract and Override Call** |
| Object created in constructor (Java/C#) | **Extract and Override Factory Method** |
| Object created in constructor (C++) | **Extract and Override Getter** or **Supersede Instance Variable** |
| Class name = perfect interface name | **Extract Implementer** |
| Need a new interface name | **Extract Interface** |
| Static methods on utility class | **Introduce Instance Delegator** |
| Singleton dependency | **Introduce Static Setter** |
| C functions, external library | **Link Substitution** or **Replace Function with Function Pointer** |
| Object created in constructor | **Parameterize Constructor** |
| Object created inside a method | **Parameterize Method** |
| Class impossible to instantiate, desperate | **Primitivize Parameter** |
| Cluster of methods, bad dependencies elsewhere | **Pull Up Feature** |
| Pervasive bad dependencies in a few methods | **Push Down Dependency** |
| Global/static access throughout class | **Replace Global Reference with Getter** |
| Any OO dependency (core technique) | **Subclass and Override Method** |
| Already templatized C++ code | **Template Redefinition** |
| Interpreted language (Ruby, Python) | **Text Redefinition** |
| C/C++ with separate headers/impl | **Definition Completion** |

---

## Key Principles

1. **Safety First.** These are performed *without* tests. Preserve signatures, lean on the compiler, and make the smallest possible change.
2. **Temporary Ugliness.** Many techniques make the design worse short-term. That's acceptable — tests enable cleaner refactoring later.
3. **Naming Matters.** Good names reinforce understanding. Poor names undermine it. Interfaces should communicate *responsibilities*, not implementation details.
4. **The Goal is Tests.** The endpoint is always: code under test → test-supported refactoring → cleaner design.
5. **Chapter 23 Companion.** Read *How Do I Know That I'm Not Breaking Anything?* (Ch 23) before applying these techniques.


## Related

- [[Software Maintenance Overview]] — All maintenance topics
- [[02_Sensing_and_Seams]] — Seam model foundation
- [[04_Getting_Tests_in_Place]] — Applying techniques to get tests
- [[05_Large_Scale_Changes]] — Large-scale refactoring patterns
