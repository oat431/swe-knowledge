---
tags:
  - clean-code
  - naming
  - functions
  - programming
---

# Naming & Functions

Names are everywhere in code — variables, functions, classes, packages. Good names eliminate the need for comments. Good functions are small, focused, and composable. This is the most impactful clean code skill to master.

---

## Naming Rules

### 1. Intention-Revealing Names

A name should answer: *why does it exist, what does it do, how is it used?*

| ❌ Bad | ✅ Good | Why |
|--------|---------|-----|
| `int d;` | `int elapsedTimeInDays;` | Reveals unit and meaning |
| `int temp;` | `int daysSinceCreation;` | Clear purpose |
| `List<String> data;` | `List<String> activeUsernames;` | Specific and searchable |
| `boolean flag;` | `boolean isEmailVerified;` | Self-documenting |
| `var list;` | `var pendingOrders;` | Domain-specific |
| `String s;` | `String customerFullName;` | No ambiguity |

### 2. Pronounceable Names

Code is discussed in meetings. If you can't say it, you can't communicate about it.

| ❌ Bad | ✅ Good |
|--------|---------|
| `genymdhms` | `generationTimestamp` |
| `class DtaRcrd102` | `class CustomerRecord` |
| `Date yyyymmdd` | `Date isoFormattedDate` |

### 3. Searchable Names

Single-letter names and magic numbers are impossible to grep.

| ❌ Bad | ✅ Good |
|--------|---------|
| `if (a == 3)` | `if (retryCount == MAX_RETRIES)` |
| `for (int i = 0; i < 5; i++)` | `for (int i = 0; i < MAX_LOGIN_ATTEMPTS; i++)` |
| `return 86400;` | `return SECONDS_PER_DAY;` |

### 4. No Encodings (Hungarian Notation)

Don't encode type information into names — that's what types are for.

| ❌ Bad | ✅ Good |
|--------|---------|
| `String strName` | `String name` |
| `int iCount` | `int count` |
| `IUserRepository repo` | `UserRepository userRepository` |
| `boolean bIsActive` | `boolean isActive` |

### 5. Class Names — Nouns, Not Verbs

| ❌ Bad | ✅ Good |
|--------|---------|
| `class ManageOrder` | `class OrderManager` |
| `class ProcessPayment` | `class PaymentProcessor` |
| `class Calculate` | `class TaxCalculator` |

### 6. Method Names — Verbs

| ❌ Bad | ✅ Good |
|--------|---------|
| `order.total()` | `order.calculateTotal()` |
| `user.email()` | `user.getEmailAddress()` |
| `payment.valid()` | `payment.isValid()` |

---

## Function Design

### The Golden Rule: Small Functions

A function should be **under 20 lines**. If it's longer, it's probably doing too much.

```
✅ Ideal:  3-10 lines
⚠️ OK:     10-20 lines
❌ Too big: 20+ lines — extract sub-functions
```

### Do One Thing

A function should do **one thing**, do it well, and do it only.

```java
// ❌ Does too many things
public void processOrder(Order order) {
    validateOrder(order);
    calculateTax(order);
    applyDiscount(order);
    saveToDatabase(order);
    sendConfirmationEmail(order);
    logOrderMetrics(order);
}

// ✅ Orchestrator does one thing: orchestrate
public void processOrder(Order order) {
    Order validated = validate(order);
    Order priced = price(validated);
    Order saved = persist(priced);
    notify(saved);
}
```

```typescript
// ❌ Does too many things
async function handleUser(data: UserData) {
  const user = parseUser(data);
  if (user.age < 18) throw new Error("Too young");
  await db.save(user);
  await emailService.sendWelcome(user.email);
  analytics.track("signup", { userId: user.id });
  return user;
}

// ✅ One thing per function
async function handleUser(data: UserData): Promise<User> {
  const user = parseUser(data);
  validateUser(user);
  const saved = await saveUser(user);
  await notifyUserCreated(saved);
  return saved;
}
```

### One Level of Abstraction

Don't mix high-level orchestration with low-level details in the same function.

```java
// ❌ Mixed abstraction levels
public void createUser(String name, String email) {
    if (!email.matches("^[\\w.-]+@[\\w.-]+\\.\\w+$")) {
        throw new InvalidEmailException(email);
    }
    String sql = "INSERT INTO users (name, email) VALUES (?, ?)";
    jdbcTemplate.update(sql, name, email);
    sendWelcomeEmail(email, name);
}

// ✅ Consistent abstraction
public void createUser(String name, String email) {
    validateEmail(email);
    persistUser(name, email);
    sendWelcomeEmail(email, name);
}
```

---

## Function Parameters

### Parameter Count

| Count | Verdict | Notes |
|-------|---------|-------|
| 0 | ✅ Ideal | Niladic — `now()`, `random()` |
| 1 | ✅ Great | Monadic — `find(id)`, `sqrt(x)` |
| 2 | ✅ Fine | Dyadic — `max(a, b)`, `copy(src, dst)` |
| 3 | ⚠️ Acceptable | Triadic — consider if one param can be an object |
| 4+ | ❌ Too many | Use parameter objects, builder pattern |

### Avoid Boolean Parameters

Booleans in parameters usually mean the function does two things.

```java
// ❌ What does true mean?
void renderText(String text, boolean uppercase) {
    if (uppercase) text = text.toUpperCase();
    // ...
}

// ✅ Two clear functions
void renderText(String text) { /* ... */ }
void renderTextUppercase(String text) { renderText(text.toUpperCase()); }
```

### Prefer Objects for 3+ Parameters

```java
// ❌ Parameter soup
User createUser(String name, String email, int age, String city, String country);

// ✅ Parameter object
User createUser(CreateUserRequest request);

record CreateUserRequest(String name, String email, int age, Address address) {}
```

```typescript
// ❌ Hard to remember order
function createUser(name: string, email: string, age: number, city: string): User;

// ✅ Destructured options object
function createUser({ name, email, age, address }: CreateUserRequest): User;
```

---

## Function Naming Patterns

| Pattern | Examples | Use When |
|---------|----------|----------|
| `verb + noun` | `calculateTotal`, `sendEmail`, `findUser` | Most functions |
| `get + noun` | `getName`, `getStatus` | Simple getters (no side effects) |
| `set + noun` | `setName`, `setStatus` | Simple setters |
| `is/has/can` | `isActive`, `hasPermission`, `canEdit` | Boolean queries |
| `to + noun` | `toString`, `toJSON`, `toDTO` | Conversions |
| `from + noun` | `fromJSON`, `fromCSV` | Factory / parsing |

---

## Checklist

- [ ] Every name reveals intent — no `temp`, `data`, `obj`, `flag`
- [ ] Names are pronounceable and searchable
- [ ] No Hungarian notation or type prefixes
- [ ] Functions under 20 lines
- [ ] Each function does exactly one thing
- [ ] Consistent abstraction level within each function
- [ ] 0-3 parameters; use objects for more
- [ ] No boolean parameters (split into two functions)
- [ ] Function names follow `verb + noun` pattern

---

## Book Deep Dive

For full chapter-level coverage:
- [[Naming Conventions]] — comprehensive naming rules with examples
- [[Function Design]] — function length, arguments, side effects

---

**Sources:** Robert C. Martin, *Clean Code*, Ch. 2-3 (2008); Steve McConnell, *Code Complete*, Ch. 7-9 (2004)
