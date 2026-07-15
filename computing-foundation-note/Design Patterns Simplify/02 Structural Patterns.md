---
tags:
  - design-patterns
  - structural
  - programming
---

# 02 Structural Patterns

Structural patterns explain how to **assemble classes and objects into larger structures** while keeping them flexible and efficient. They focus on relationships — composition, delegation, wrapping, and hierarchy.

---

## Quick Reference

| Pattern | Intent | When to Use |
|---------|--------|-------------|
| **Adapter** | Convert one interface into another | Integrating legacy code or third-party libraries |
| **Decorator** | Add responsibilities dynamically | Stackable behavior (logging, caching, compression) |
| **Facade** | Simplify a complex subsystem | One entry point for many internal calls |
| **Proxy** | Control access to an object | Lazy loading, access control, caching, remote calls |
| **Composite** | Treat individual and composed objects uniformly | Tree structures (files, UI components, org charts) |
| **Bridge** | Decouple abstraction from implementation | Two independent dimensions of variation |

---

## Adapter

**Problem:** You have a class with the right functionality but the wrong interface. The Adapter wraps it and translates calls.

```java
// Legacy system returns XML — your code expects JSON
interface JsonDataSource {
    String getJson();
}

class LegacyXmlService {
    String getXml() { return "<user><name>John</name></user>"; }
}

// Adapter: wraps legacy, exposes expected interface
class XmlToJsonAdapter implements JsonDataSource {
    private final LegacyXmlService legacy;

    XmlToJsonAdapter(LegacyXmlService legacy) { this.legacy = legacy; }

    public String getJson() {
        return XmlConverter.toJson(legacy.getXml());  // translate
    }
}
```

```typescript
// TypeScript: same pattern
class XmlToJsonAdapter implements JsonDataSource {
    constructor(private legacy: LegacyXmlService) {}
    getJson(): string { return xmlToJson(this.legacy.getXml()); }
}
```

---

## Decorator

**Problem:** You want to add behavior to an object without modifying its class or using subclass explosion. Stack decorators like layers.

**Real-world:** Java I/O streams are the canonical example — `BufferedInputStream` decorates `FileInputStream`, adding buffering without changing the stream interface.

```java
// Base component
interface DataSource {
    String readData();
}

// Concrete component
class FileDataSource implements DataSource {
    private String filename;
    public String readData() { /* read file */ }
}

// Base decorator
abstract class DataSourceDecorator implements DataSource {
    protected DataSource wrappee;
    DataSourceDecorator(DataSource source) { this.wrappee = source; }
    public String readData() { return wrappee.readData(); }
}

// Concrete decorators — stackable
class CompressionDecorator extends DataSourceDecorator {
    CompressionDecorator(DataSource s) { super(s); }
    public String readData() {
        return decompress(wrappee.readData());
    }
}

class EncryptionDecorator extends DataSourceDecorator {
    EncryptionDecorator(DataSource s) { super(s); }
    public String readData() {
        return decrypt(wrappee.readData());
    }
}

// Usage — compose behavior by stacking
DataSource source = new EncryptionDecorator(
    new CompressionDecorator(
        new FileDataSource("data.txt")
    )
);
```

---

## Facade

**Problem:** A subsystem has many classes with complex interactions. Client code shouldn't know the internals.

```java
// ❌ Bad: client orchestrates 5 services
PlaceOrderResult placeOrder(Order order) {
    inventoryService.reserve(order.getItems());
    paymentService.charge(order.getPayment());
    shippingService.schedule(order.getAddress());
    notificationService.sendConfirmation(order.getCustomer());
    analyticsService.trackOrder(order);
    return new PlaceOrderResult(true);
}
```

```java
// ✅ Good: Facade hides the complexity
class OrderFacade {
    private InventoryService inventory;
    private PaymentService payment;
    private ShippingService shipping;
    private NotificationService notification;
    private AnalyticsService analytics;

    public PlaceOrderResult placeOrder(Order order) {
        inventory.reserve(order.getItems());
        payment.charge(order.getPayment());
        shipping.schedule(order.getAddress());
        notification.sendConfirmation(order.getCustomer());
        analytics.trackOrder(order);
        return new PlaceOrderResult(true);
    }
}

// Client sees one method
orderFacade.placeOrder(order);
```

A Facade doesn't add new functionality — it's a convenience wrapper. Subsystems remain accessible for advanced use.

---

## Proxy

**Problem:** You need to control access to an object — for lazy loading, caching, access control, or remote communication.

| Proxy Type | Purpose | Example |
|-----------|---------|---------|
| **Virtual** | Lazy initialization | Load image from disk only when displayed |
| **Protection** | Access control | Check permissions before method call |
| **Caching** | Store results | Cache expensive DB queries |
| **Remote** | Network calls | gRPC stub wraps remote service |

```java
// Virtual proxy: expensive object created only on first use
class HeavyImageProxy implements Image {
    private HeavyImage realImage;
    private String filename;

    HeavyImageProxy(String filename) { this.filename = filename; }

    public void display() {
        if (realImage == null) {
            realImage = new HeavyImage(filename);  // lazy!
        }
        realImage.display();
    }
}
```

**Spring AOP** uses proxies extensively — `@Transactional`, `@Cacheable`, `@Async` all work by wrapping your bean in a proxy that intercepts method calls.

---

## Composite

**Problem:** You have a tree structure where individual objects and groups of objects should be treated uniformly.

**Real-world:** File system — files and folders both have names, sizes, and can be displayed. Folders contain files (and other folders).

```java
// Component — common interface for leaves and composites
interface FileSystemItem {
    String getName();
    long getSize();
    void display(String indent);
}

// Leaf
class File implements FileSystemItem {
    private String name;
    private long size;

    public void display(String indent) {
        System.out.println(indent + "📄 " + name + " (" + size + " bytes)");
    }
}

// Composite — contains children
class Folder implements FileSystemItem {
    private String name;
    private List<FileSystemItem> children = new ArrayList<>();

    public void addChild(FileSystemItem item) { children.add(item); }

    public long getSize() {
        return children.stream().mapToLong(FileSystemItem::getSize).sum();
    }

    public void display(String indent) {
        System.out.println(indent + "📁 " + name);
        children.forEach(c -> c.display(indent + "  "));
    }
}
```

---

## Bridge

**Problem:** You have two independent dimensions of variation (e.g., shape × color, platform × abstraction). Instead of a class explosion (`RedCircle`, `BlueCircle`, `RedSquare`…), separate them.

```java
// Implementation hierarchy
interface Renderer {
    void renderCircle(double radius);
    void renderSquare(double side);
}

class VectorRenderer implements Renderer {
    public void renderCircle(double r) { System.out.println("Drawing circle as vectors"); }
    public void renderSquare(double s) { System.out.println("Drawing square as vectors"); }
}

class RasterRenderer implements Renderer {
    public void renderCircle(double r) { System.out.println("Drawing circle as pixels"); }
    public void renderSquare(double s) { System.out.println("Drawing square as pixels"); }
}

// Abstraction — holds reference to implementation
abstract class Shape {
    protected Renderer renderer;
    Shape(Renderer renderer) { this.renderer = renderer; }
    abstract void draw();
}

class Circle extends Shape {
    private double radius;
    Circle(Renderer r, double radius) { super(r); this.radius = radius; }
    void draw() { renderer.renderCircle(radius); }
}
```

Bridge lets you combine any shape with any renderer without creating a subclass for each combination.

---

## Decision Flow

```
Structural problem?
├─ Incompatible interface? → Adapter
├─ Add behavior without modifying class? → Decorator
├─ Simplify complex subsystem? → Facade
├─ Control access / lazy init? → Proxy
├─ Tree structure with uniform interface? → Composite
└─ Two independent variation axes? → Bridge
```

---

## Book Deep Dive

For full implementations with UML diagrams, participants, and trade-offs:
- [[adapter]]
- [[decorator]]
- [[facade]]
- [[proxy]]
- [[composite]]
- [[bridge]]

---

## Sources

- Gamma et al. — *Design Patterns* (1994), Chapters 4, 6-8
- Refactoring Guru — https://refactoring.guru/design-patterns/structural-patterns
- Java I/O streams — `java.io` package (Decorator in practice)
