---
tags: [construction, classes, adt, oop, code-complete]
source: McConnell, Code Complete (2nd ed.), Chapter 6
created: 2026-07-21
---

# Working Classes

> *"A key to being an effective programmer is maximizing the portion of a program that you can safely ignore while working on any one section of code. Classes are the primary tool for accomplishing that objective."* — Steve McConnell

---

## 6.1 Class Foundations: Abstract Data Types (ADTs)

An **abstract data type** is a collection of data and operations that work on that data. The operations both describe the data to the rest of the program and allow the rest of the program to change the data.

Understanding ADTs is essential to understanding OOP. Without ADT understanding, programmers create classes that are "classes" in name only — little more than convenient carrying cases for loosely related data and routines.

### The Problem ADTs Solve

Without ADTs, client code manipulates data directly:

```cpp
currentFont.size = 16;                          // ambiguous: pixels or points?
currentFont.attribute = currentFont.attribute | 0x02;  // magic numbers
```

With ADTs, the interface becomes self-documenting:

```cpp
currentFont.SetSizeInPoints(12);
currentFont.SetBoldOn();
```

### Benefits of ADTs

| Benefit | Description |
|---------|-------------|
| **Hide implementation details** | Change the data type in one place without affecting the whole program |
| **Changes don't ripple** | Extend functionality (small caps, superscripts) by changing one module |
| **More informative interface** | `SetSizeInPoints(12)` beats `size = 16` for clarity |
| **Easier performance tuning** | Recode a few well-defined routines instead of searching the whole program |
| **More obviously correct** | `SetBoldOn()` eliminates multiple failure modes (`or` vs `and`, wrong constant, wrong field) |
| **Self-documenting** | Routine names communicate intent |
| **Don't pass data everywhere** | ADT encapsulates data; non-ADT routines don't need to know about it |
| **Work in the problem domain** | Operate on "fonts" and "employees", not arrays and bit flags |

> **Hard Data:** A study by Woodfield, Dunsmore, and Shen (1981) found that students using an ADT-structured program scored **30% higher** on comprehension questions than students using a functionally decomposed version.

### Guidelines for Defining ADTs

1. **Build low-level data types as ADTs** — Stacks, lists, queues can all be ADTs. But ask: *"What does this stack represent?"* If it represents employees, treat the ADT as `Employees`, not as `Stack`. **Treat yourself to the highest possible level of abstraction.**

2. **Treat common objects as ADTs** — Files, streams, GUI windows are already ADTs provided by the OS/language. Layer your own ADTs on top.

3. **Treat even simple items as ADTs** — A `Light` with only `TurnOn()` and `TurnOff()` still benefits from encapsulation, self-documentation, and change isolation.

4. **Refer to an ADT independently of its storage medium** — Call it `rateTable.Read()` not `RateFile.Read()`. If you change storage from disk to memory, the interface remains correct.

### ADTs and Classes

> A class is an abstract data type **plus inheritance and polymorphism**.

---

## 6.2 Good Class Interfaces

> *"The first and probably most important step in creating a high-quality class is creating a good interface."*

### Good Abstraction

A class interface should offer a group of routines that **clearly belong together**, all working toward a consistent end.

**Good abstraction** (Employee class):
```cpp
class Employee {
public:
    FullName GetName() const;
    String GetAddress() const;
    String GetWorkPhone() const;
    TaxId GetTaxIdNumber() const;
    JobClassification GetJobClassification() const;
};
```
→ Every routine serves the Employee abstraction.

**Poor abstraction** (mishmash of unrelated concerns):
```cpp
class Program {
public:
    void InitializeCommandStack();
    void PushCommand( Command command );
    void FormatReport( Report report );   // unrelated to command stack
    void InitializeGlobalData();          // unrelated to reports or commands
};
```

### Guidelines for Good Abstraction

1. **Present a consistent level of abstraction** — Each class should implement one and only one ADT. Don't mix `Employee`-level routines with `ListContainer`-level routines. If your class inherits from `ListContainer` but is supposed to be an `EmployeeCensus`, hide the container and expose only employee-level operations.

2. **Understand what abstraction the class is implementing** — If you wrap a 150-routine spreadsheet control to provide a 15-routine grid control, **expose only the 15 grid routines**, not all 150. Exposing everything defeats encapsulation.

3. **Provide services in pairs with their opposites** — `On`/`Off`, `Add`/`Remove`, `Activate`/`Deactivate`. Don't create opposites gratuitously, but check whether one is needed.

4. **Move unrelated information to another class** — If half the routines work with half the data and the other half with the other half, you have two classes in one. Split them.

5. **Make interfaces programmatic rather than semantic** — Semantic dependencies ("`RoutineA` must be called before `RoutineB`") can't be enforced by the compiler. Use asserts to convert semantic requirements into programmatic ones.

6. **Beware of interface erosion under maintenance** — Don't add `IsZipCodeValid()` or `GetQueryToCreateNewEmployee()` to an `Employee` class. They violate the abstraction.

7. **Don't add public members inconsistent with the interface abstraction** — Each time you add a routine, ask: *"Is this consistent with the existing abstraction?"*

8. **Focus on abstraction first, cohesion second** — Abstraction at the interface level provides more design insight than cohesion analysis.

### Good Encapsulation

> *"Encapsulation is the enforcer that prevents you from looking at the details even if you want to."*

**Abstraction** helps manage complexity by providing models. **Encapsulation** is the stronger concept — it *prevents* you from looking at implementation details. You either have both or neither; there is no middle ground.

#### Key Encapsulation Rules

1. **Minimize accessibility of classes and members** — Favor the strictest level of privacy that's workable. The real question: *"What best preserves the integrity of the interface abstraction?"*

2. **Don't expose member data in public** — Getters/setters maintain encapsulation even when they expose individual fields:
   ```cpp
   float GetX();     // Encapsulation preserved — implementation unknown
   void SetX(float x);
   ```
   vs.
   ```cpp
   float x;          // Encapsulation broken — client monkeys directly with data
   ```

3. **Avoid putting private implementation details into the class interface** — In C++, use the **Pointer to Implementation (pimpl)** idiom:
   ```cpp
   class Employee {
   private:
       EmployeeImplementation *m_implementation;  // hides all implementation
   };
   ```

4. **Don't make assumptions about the class's users** — Design to the contract, not to how you think the class will be used. Comments like *"initialize to 1.0 because DerivedClass blows up otherwise"* indicate the class knows too much about its users.

5. **Avoid friend classes** — They violate encapsulation. Use only in disciplined patterns like State.

6. **Don't expose a routine just because it uses only public routines** — Ask whether exposing it is consistent with the abstraction.

7. **Favor read-time convenience over write-time convenience** — Code is read far more times than written. Don't add a convenient-but-inconsistent routine just because it helps one client.

8. **Be very wary of semantic violations of encapsulation** — Examples of semantic encapsulation breakage:
   - Skipping `InitializeOperations()` because you *know* `PerformFirstOperation()` calls it
   - Using `ClassB.MAXIMUM_ELEMENTS` instead of `ClassA.MAXIMUM_ELEMENTS` because you *know* they're equal
   - Using a reference after its creating object goes out of scope because you *know* about static storage

   > *"If you can't figure out how to use a class based solely on its interface documentation, the right response is NOT to look at the implementation. It's to contact the author and get the documentation fixed."*

9. **Watch for coupling that's too tight** — Tight coupling follows leaky abstraction or broken encapsulation. Observe the Law of Demeter.

---

## 6.3 Design and Implementation Issues

### Containment ("has a" Relationships)

> *"Containment is the workhorse technique in object-oriented programming."*

- **"Has a" is implemented through containment** — An employee *has a* name, phone number, tax ID → make them member data.
- **Private inheritance as a last resort** — Only for accessing protected members of the contained class. Creates an overly cozy relationship; usually indicates a design error.
- **The 7±2 rule for data members** — If a class has more than about 7 data members, consider decomposition. Err toward the high end for primitives, low end for complex objects.

### Inheritance ("is a" Relationships)

> *"The single most important rule in object-oriented programming with C++ is this: public inheritance means 'is a.'"* — Scott Meyers

Inheritance creates simpler code by centralizing common elements (interfaces, implementations, data) in a base class.

#### Key Inheritance Decisions

For each member routine: visible to derived classes? default implementation? overridable?
For each data member: visible to derived classes?

#### Inheritance Rules

1. **Implement "is a" through public inheritance** — If the derived class won't adhere to the base class's interface contract, use containment instead.

2. **Design and document for inheritance — or prohibit it** — Make members `final`/non-virtual if the class isn't designed for inheritance.

3. **Adhere to the Liskov Substitution Principle (LSP)** — *"Subclasses must be usable through the base class interface without the need for the user to know the difference."* If `InterestRate()` means "interest paid to customer" in `CheckingAccount` but "interest paid by customer" in `AutoLoanAccount`, LSP is violated — don't inherit.

4. **Be sure to inherit only what you want to inherit** — Three flavors of inherited routines:

| Type | Inherits Interface? | Inherits Implementation? | Overridable? |
|------|---------------------|-------------------------|--------------|
| Abstract overridable | Yes | No | Yes |
| Overridable | Yes | Default provided | Yes |
| Non-overridable | Yes | Default provided | No |

5. **Don't "override" a non-overridable member function** — Creating a same-named function in a derived class when the base class's version is private creates confusion; it looks polymorphic but isn't.

6. **Move common interfaces/data/behavior as high as possible** — But don't break the higher object's abstraction.

7. **Be suspicious of single-instance classes** — May indicate confusion between objects and classes. Can the variation be represented in data instead? (Singleton is the exception.)

8. **Be suspicious of base classes with only one derived class** — Probably "designing ahead." Make current work as simple as possible.

9. **Be suspicious of routines that override and do nothing** — Indicates an error in the base class design. Fix the root cause (e.g., not all cats scratch → add a `Claws` component) rather than creating `ScratchlessCat`.

10. **Avoid deep inheritance trees** — Limit to 2–3 levels in practice. Deep trees are associated with increased fault rates (Basili, Briand, and Melo 1996).

11. **Prefer polymorphism to extensive type checking** — Repeated `switch`/`case` on type codes often signals a need for polymorphic dispatch:

    ```cpp
    // Instead of this:
    switch (shape.type) {
        case Shape_Circle: shape.DrawCircle(); break;
        case Shape_Square: shape.DrawSquare(); break;
    }
    
    // Prefer this:
    shape.Draw();  // polymorphic
    ```

---

## Key Points Summary

> 💡 **Classes are the primary tool for maximizing the portion of a program you can safely ignore.**

| Principle | Core Idea |
|-----------|-----------|
| **ADTs** | Bundle data with the operations that work on it. Work at the problem-domain level, not the implementation level. |
| **Abstraction** | Present a consistent, focused interface. One class = one ADT. |
| **Encapsulation** | Hide implementation details. Don't let clients peek behind the curtain. |
| **Containment** | The workhorse. "Has a" → member data. |
| **Inheritance** | Use sparingly. "Is a" → public inheritance. Must satisfy LSP. |
| **Interface integrity** | Every public routine must be consistent with the class abstraction. Say no to convenience additions. |

---

## Chapter Outline (Source Coverage)

| Section | Topic | Status |
|---------|-------|--------|
| 6.1 | Class Foundations: Abstract Data Types (ADTs) | ✅ Complete |
| 6.2 | Good Class Interfaces | ✅ Complete |
| 6.3 | Design and Implementation Issues | ✅ Complete |
| 6.4 | Reasons to Create a Class | ⚠️ Not in source |
| 6.5 | Language-Specific Issues | ⚠️ Not in source |
| 6.6 | Beyond Classes: Packages | ⚠️ Not in source |

> **Note:** The source file covers sections 6.1–6.3 in full. Sections 6.4–6.6 were not included in the extracted text. Refer to *Code Complete, 2nd Edition* directly for those topics.
