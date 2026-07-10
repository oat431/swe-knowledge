---
tags:
  - design-patterns
  - behavioral
  - programming
---

# 03 Behavioral Patterns

Behavioral patterns are about **object communication and responsibility**. They define how objects interact, distribute work, and manage complex control flow. If creational patterns are about *making* things and structural patterns about *assembling* things, behavioral patterns are about *coordinating* things.

---

## Quick Reference

| Pattern | Intent | When to Use |
|---------|--------|-------------|
| **Strategy** | Encapsulate interchangeable algorithms | Sorting, payment methods, validation rules |
| **Observer** | Notify dependents on state change | Event systems, UI data binding, pub/sub |
| **Command** | Encapsulate a request as an object | Undo/redo, job queues, macro recording |
| **State** | Behavior changes with internal state | Order lifecycle, media player states |
| **Template Method** | Define algorithm skeleton, defer steps | Data processing pipelines, test frameworks |
| **Iterator** | Sequential access without exposing internals | Custom collections, lazy sequences |
| **Chain of Responsibility** | Pass request along handler chain | Servlet filters, middleware, approval workflows |

---

## Strategy

**Problem:** You have multiple algorithms for the same task and want to swap them at runtime without conditionals.

```java
// ❌ Bad: if/else spaghetti
double calculatePrice(Order order, String type) {
    if (type.equals("regular"))    return order.total * 1.0;
    else if (type.equals("vip"))   return order.total * 0.8;
    else if (type.equals("promo")) return order.total * 0.5;
}
```

```java
// ✅ Good: Strategy pattern
interface PricingStrategy {
    double calculate(Order order);
}

class RegularPricing implements PricingStrategy {
    public double calculate(Order o) { return o.getTotal(); }
}
class VipPricing implements PricingStrategy {
    public double calculate(Order o) { return o.getTotal() * 0.8; }
}
class PromoPricing implements PricingStrategy {
    public double calculate(Order o) { return o.getTotal() * 0.5; }
}

// Context uses strategy — swap at runtime
class OrderService {
    private PricingStrategy strategy;
    void setStrategy(PricingStrategy s) { this.strategy = s; }
    double checkout(Order o) { return strategy.calculate(o); }
}
```

```typescript
// TypeScript: functions as strategies
type PricingStrategy = (total: number) => number;

const vipPricing: PricingStrategy = (total) => total * 0.8;
const promoPricing: PricingStrategy = (total) => total * 0.5;
```

**Real-world:** Java `Comparator` — pass different comparators to `Collections.sort()` to change sorting behavior.

---

## Observer

**Problem:** One object changes state and multiple others need to know — without tight coupling.

```java
// Subject
class EventBus {
    private Map<String, List<Consumer<Object>>> listeners = new HashMap<>();

    void subscribe(String event, Consumer<Object> listener) {
        listeners.computeIfAbsent(event, k -> new ArrayList<>()).add(listener);
    }

    void publish(String event, Object data) {
        listeners.getOrDefault(event, List.of())
            .forEach(listener -> listener.accept(data));
    }
}

// Usage — loose coupling between publisher and subscribers
eventBus.subscribe("orderPlaced", data -> sendEmail(data));
eventBus.subscribe("orderPlaced", data -> updateInventory(data));
eventBus.subscribe("orderPlaced", data -> trackAnalytics(data));

eventBus.publish("orderPlaced", order);
```

**Framework examples:**
- **Spring Events:** `ApplicationEventPublisher.publishEvent()` + `@EventListener`
- **Java:** `PropertyChangeListener`, `javax.swing` event model
- **JavaScript:** `addEventListener`, RxJS Observables

---

## Command

**Problem:** You need to parameterize objects with operations, queue them, log them, or support undo/redo.

```java
// Command interface
interface Command {
    void execute();
    void undo();  // optional: for undo support
}

// Concrete commands
class InsertTextCommand implements Command {
    private Editor editor;
    private String text;
    private int position;

    InsertTextCommand(Editor editor, String text, int position) {
        this.editor = editor; this.text = text; this.position = position;
    }

    public void execute() { editor.insert(text, position); }
    public void undo()    { editor.delete(position, text.length()); }
}

class DeleteTextCommand implements Command {
    private Editor editor;
    private int position, length;
    private String backup;  // save deleted text for undo

    public void execute() {
        backup = editor.getText(position, length);
        editor.delete(position, length);
    }
    public void undo() { editor.insert(backup, position); }
}

// Invoker — history for undo/redo
class CommandHistory {
    private Deque<Command> history = new ArrayDeque<>();

    void execute(Command cmd) {
        cmd.execute();
        history.push(cmd);
    }

    void undo() {
        if (!history.isEmpty()) history.pop().undo();
    }
}
```

**Use cases:** Transaction log, job queue, macro recording, distributed commands.

---

## State

**Problem:** An object behaves differently depending on its internal state. Instead of big `switch` blocks, each state becomes a class.

```java
// State interface
interface OrderState {
    void next(OrderContext ctx);
    void cancel(OrderContext ctx);
    String getStatus();
}

class CreatedState implements OrderState {
    public void next(OrderContext ctx) {
        ctx.setState(new PaidState());  // created → paid
    }
    public void cancel(OrderContext ctx) {
        ctx.setState(new CancelledState());
    }
    public String getStatus() { return "CREATED"; }
}

class PaidState implements OrderState {
    public void next(OrderContext ctx) {
        ctx.setState(new ShippedState());  // paid → shipped
    }
    public void cancel(OrderContext ctx) {
        ctx.setState(new RefundedState());
    }
    public String getStatus() { return "PAID"; }
}

class ShippedState implements OrderState {
    public void next(OrderContext ctx) {
        ctx.setState(new DeliveredState());  // shipped → delivered
    }
    public void cancel(OrderContext ctx) {
        throw new IllegalStateException("Cannot cancel shipped order");
    }
    public String getStatus() { return "SHIPPED"; }
}

// Context delegates to current state
class OrderContext {
    private OrderState state = new CreatedState();

    void setState(OrderState s) { this.state = s; }
    void next()    { state.next(this); }
    void cancel()  { state.cancel(this); }
    String getStatus() { return state.getStatus(); }
}
```

```
State flow:
  Created → Paid → Shipped → Delivered
      ↓       ↓
  Cancelled Refunded
```

---

## Template Method

**Problem:** Multiple classes share the same algorithm skeleton but differ in specific steps. Define the skeleton once, let subclasses override the steps.

```java
abstract class DataProcessor {
    // Template method — final to prevent override
    public final void process() {
        readData();
        parseData();
        validate();
        save();
    }

    abstract void readData();     // subclasses define
    abstract void parseData();    // subclasses define

    void validate() { /* default: no-op, optional override */ }

    void save() {                 // common step
        System.out.println("Saving to database...");
    }
}

class CsvProcessor extends DataProcessor {
    void readData()   { System.out.println("Reading CSV file"); }
    void parseData()  { System.out.println("Parsing CSV rows"); }
}

class JsonProcessor extends DataProcessor {
    void readData()   { System.out.println("Reading JSON file"); }
    void parseData()  { System.out.println("Parsing JSON tree"); }
    void validate()   { System.out.println("JSON schema validation"); }
}
```

**Real-world:** JUnit's lifecycle (`@Before` → `@Test` → `@After`), Spring's `JdbcTemplate`, servlet's `doGet()`/`doPost()`.

---

## Iterator

**Problem:** Provide sequential access to a collection's elements without exposing its internal structure.

```java
// Java's built-in Iterator interface
public interface Iterator<E> {
    boolean hasNext();
    E next();
    default void remove() { throw new UnsupportedOperationException(); }
}

// Custom collection with its own iterator
class TreeNode<T> {
    T value;
    TreeNode<T> left, right;

    Iterator<T> inOrderIterator() {
        return new Iterator<>() {
            private Stack<TreeNode<T>> stack = new Stack<>();

            { pushLeft(this); }  // init block

            private void pushLeft(TreeNode<T> node) {
                while (node != null) { stack.push(node); node = node.left; }
            }

            public boolean hasNext() { return !stack.isEmpty(); }

            public T next() {
                TreeNode<T> node = stack.pop();
                pushLeft(node.right);
                return node.value;
            }
        };
    }
}
```

```typescript
// TypeScript: generators are natural iterators
function* inOrder<T>(node: TreeNode<T> | null): Generator<T> {
    if (!node) return;
    yield* inOrder(node.left);
    yield node.value;
    yield* inOrder(node.right);
}
```

**Java's `Iterable<T>`** interface enables `for-each` loops. Implement `iterator()` and your collection works with `for (T item : collection)`.

---

## Chain of Responsibility

**Problem:** Multiple objects may handle a request. Don't hardcode the handler — pass the request along a chain until someone handles it.

```java
// Handler base
abstract class Handler {
    private Handler next;

    Handler linkWith(Handler next) { this.next = next; return next; }

    public boolean handle(HttpRequest request) {
        if (next == null) return true;
        return next.handle(request);
    }
}

// Concrete handlers
class AuthHandler extends Handler {
    public boolean handle(HttpRequest req) {
        if (!req.hasValidToken()) {
            throw new UnauthorizedException("Invalid token");
        }
        System.out.println("✅ Auth passed");
        return super.handle(req);  // pass to next
    }
}

class RateLimitHandler extends Handler {
    public boolean handle(HttpRequest req) {
        if (rateLimiter.isExceeded(req.getIp())) {
            throw new TooManyRequestsException();
        }
        System.out.println("✅ Rate limit passed");
        return super.handle(req);
    }
}

class LoggingHandler extends Handler {
    public boolean handle(HttpRequest req) {
        System.out.println("📝 " + req.getMethod() + " " + req.getPath());
        return super.handle(req);
    }
}

// Build the chain
Handler chain = new LoggingHandler();
chain.linkWith(new RateLimitHandler())
     .linkWith(new AuthHandler());

chain.handle(request);  // request flows through the chain
```

**Real-world:**
- **Servlet Filters:** `javax.servlet.Filter` chain
- **Express/Koa middleware:** `app.use(auth).use(logging).use(rateLimit)`
- **Spring Security:** `FilterChainProxy` with security filters
- **Approval workflows:** Manager → Director → VP (escalation chain)

---

## Decision Flow

```
Behavioral problem?
├─ Swap algorithms at runtime? → Strategy
├─ Notify many objects on change? → Observer
├─ Need undo/redo or queuing? → Command
├─ Behavior depends on state? → State
├─ Same skeleton, different steps? → Template Method
├─ Access collection elements? → Iterator
└─ Multiple possible handlers? → Chain of Responsibility
```

---

## Book Deep Dive

For full implementations with UML diagrams, participants, and trade-offs:
- [[strategy]]
- [[observer]]
- [[command]]
- [[state]]
- [[template-method]]
- [[iterator]]
- [[chain-of-responsibility]]

---

## Sources

- Gamma et al. — *Design Patterns* (1994), Chapters 5, 9-10
- Refactoring Guru — https://refactoring.guru/design-patterns/behavioral-patterns
- Spring Framework docs — Events, AOP, Security Filter Chain
