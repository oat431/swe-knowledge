---
tags:
  - programming
  - fundamental
  - serialization
---

# Data Formats & Serialization

Choosing the right data format impacts performance, interoperability, and maintainability. JSON is the default for APIs, but binary formats like Protobuf and Avro dominate high-performance and big data scenarios. This note covers the main formats and when to reach for each.

---

## JSON

Human-readable, language-agnostic, the lingua franca of web APIs.

**When to use:** REST APIs, config files, message queues with small payloads, any human-facing interchange.

### Parsers

| Language | Library | Notes |
|----------|---------|-------|
| Java | Jackson | Industry standard, annotations-based, streaming API |
| Java | Gson | Google's library, simpler API, good for quick parsing |
| TypeScript | Built-in `JSON.parse()` / `JSON.stringify()` | No library needed |
| Python | `json` stdlib | Built-in, `json.dumps()` / `json.loads()` |

### ❌ JSON Date Problem

JSON has **no native date type**. Dates become strings, and every system formats them differently.

```json
// ❌ Ambiguous — is this MM-DD or DD-MM?
{ "createdAt": "01/02/2024" }

// ✅ ISO 8601 is the standard
{ "createdAt": "2024-01-02T14:30:00Z" }
```

```java
// ❌ Don't serialize dates as timestamps
@JsonFormat(shape = JsonFormat.Shape.NUMBER)
private LocalDateTime createdAt;  // 1704197400000 — unreadable

// ✅ Use ISO format
@JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd'T'HH:mm:ss'Z'")
private LocalDateTime createdAt;
```

---

## XML

Verbose but powerful — schemas, namespaces, transformation (XSLT), and validation built-in.

**When to use:** SOAP web services, enterprise integrations (banks, government), legacy systems, configuration (Maven, Spring).

### Parsers

| Language | Library | Notes |
|----------|---------|-------|
| Java | JAXB | Annotation-based, built into Jakarta EE |
| Java | DOM / SAX | Low-level, SAX is event-driven for large files |
| Python | `xml.etree.ElementTree` | Built-in, simple API |
| TypeScript | `fast-xml-parser` | npm package, fast and lightweight |

### XML vs JSON at a Glance

| Aspect | JSON | XML |
|--------|------|-----|
| Readability | ✅ Compact | ❌ Verbose with closing tags |
| Schema support | Weak (JSON Schema) | ✅ Strong (XSD, DTD) |
| Namespaces | ❌ No native support | ✅ Built-in |
| Transform | Manual | ✅ XSLT |

---

## Protocol Buffers (Protobuf)

Binary format by Google. Schema-driven, extremely compact, blazing fast serialization.

**When to use:** [[02 gRPC]], microservice-to-microservice communication, any high-throughput internal API, mobile apps (small payloads).

### Schema Definition (.proto)

```protobuf
syntax = "proto3";

message User {
  int32 id = 1;
  string name = 2;
  string email = 3;
  repeated string roles = 4;
  google.protobuf.Timestamp created_at = 5;
}

service UserService {
  rpc GetUser(GetUserRequest) returns (User);
  rpc ListUsers(ListUsersRequest) returns (stream User);
}
```

### Code Generation

```bash
# Generate Java classes
protoc --java_out=./src/main/java user.proto

# Generate TypeScript types
protoc --plugin=protoc-gen-ts_proto=./node_modules/.bin/protoc-gen-ts_proto \
  --ts_proto_out=./src/generated user.proto
```

### Key Characteristics

- **Binary format** — not human-readable, requires `.proto` schema to decode
- **Forward/backward compatible** — field numbers are the contract, not field names
- **Much smaller** than JSON (typically 3-10x)
- **Much faster** to serialize/deserialize (20-100x vs JSON)
- **Strongly typed** — schema is the single source of truth

---

## Apache Avro

Schema-based binary format with built-in schema evolution. The standard for big data ecosystems.

**When to use:** [[02 Event-Driven Architecture]] with Kafka, Hadoop/Spark pipelines, any system requiring schema evolution without downtime.

### Schema Definition (.avsc)

```json
{
  "type": "record",
  "name": "User",
  "fields": [
    {"name": "id", "type": "int"},
    {"name": "name", "type": "string"},
    {"name": "email", "type": ["null", "string"], "default": null}
  ]
}
```

### Schema Evolution Rules

| Change | Reader Compatibility |
|--------|---------------------|
| Add field with default | ✅ Safe — old data gets default |
| Remove field with default | ✅ Safe — reader ignores unknown fields |
| Add field without default | ❌ Breaks old readers |
| Change field type | ❌ Breaks unless union |

### Avro vs Protobuf

| Aspect | Avro | Protobuf |
|--------|------|----------|
| Schema location | Embedded in data | Separate `.proto` file |
| Schema evolution | ✅ Excellent (reader/writer schemas) | ✅ Good (field numbers) |
| Big data ecosystem | ✅ Native (Kafka, Spark) | ⚠️ Requires adapters |
| Code generation | Optional | Required |
| Human-readable | ❌ Binary | ❌ Binary |

---

## Comparison Table

| Feature | JSON | XML | Protobuf | Avro |
|---------|------|-----|----------|------|
| **Size** | Large | Largest | ✅ Small | ✅ Small |
| **Speed** | Slow | Slower | ✅ Fast | ✅ Fast |
| **Human-readable** | ✅ Yes | ✅ Yes | ❌ No | ❌ No |
| **Schema** | Optional | Optional | Required | Required |
| **Schema evolution** | ❌ Weak | ⚠️ XSD | ✅ Good | ✅ Excellent |
| **Language support** | All | All | Many | Many |
| **Best for** | APIs, config | SOAP, legacy | gRPC, internal | Kafka, big data |

---

## Java Serialization

Built-in Java object serialization — rarely recommended but important to understand.

```java
// ✅ Implement Serializable
public class User implements Serializable {
    private static final long serialVersionUID = 1L;  // version control

    private String name;
    private String email;

    // transient = excluded from serialization
    private transient String temporaryToken;
}
```

| Concept | Purpose |
|---------|---------|
| `Serializable` | Marker interface — enables serialization |
| `serialVersionUID` | Version control — mismatch causes `InvalidClassException` |
| `transient` | Skip field during serialization (passwords, tokens, caches) |

### ⚠️ Don't Use Java Serialization for New Projects

- **Security vulnerabilities** — deserialization attacks (RCE via gadget chains)
- **Not cross-language** — Java-only
- **Verbose binary format** — larger than JSON, slower than Protobuf
- **Better alternatives:** JSON (Jackson), Protobuf, or Avro

```java
// ❌ Native Java serialization
ObjectOutputStream out = new ObjectOutputStream(fileOut);
out.writeObject(user);

// ✅ Use Jackson instead
ObjectMapper mapper = new ObjectMapper();
mapper.writeValue(file, user);
```

---

## Decision Flowchart

```
Need human-readable? → Yes → Is it a web API? → Yes → JSON
                                         → No  → XML (if SOAP/legacy) or JSON
         → No  → Need schema evolution? → Yes → Avro (big data) or Protobuf (gRPC)
                             → No  → Is it gRPC/internal microservices? → Yes → Protobuf
                                                        → No  → JSON (simplicity wins)
```

---

**Sources:** Google Protocol Buffers docs; Apache Avro spec 1.11; Martin Kleppmann, *Designing Data-Intensive Applications* (2017); JSON.org
