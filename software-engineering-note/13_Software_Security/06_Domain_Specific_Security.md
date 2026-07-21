---
tags:
  - software-engineering
  - swebok
  - ka13
  - software-security
  - domain-security
  - cloud-security
  - iot-security
  - ml-security
  - api-security
source: "SWEBOK v4 Chapter 13"
created: 2026-07-21
---

# 06 Domain-Specific Security

> *Security must be adapted to the unique constraints and attack surfaces of each computing domain.*

---

## 1. Overview

General-purpose security principles from [[01_Security_Fundamentals]] and [[02_Protocols_and_Cryptography]] provide a foundation, but modern software spans diverse deployment environments with distinct threat models. SWEBOK KA 13.6 addresses security concerns unique to cloud and container platforms, Internet of Things devices, machine learning systems, mobile applications, and APIs.

| Domain | Primary Concern | Key Constraint |
|--------|----------------|----------------|
| Cloud & Containers | Shared responsibility boundaries | Ephemeral, orchestrated infrastructure |
| IoT | Massive endpoint attack surface | Constrained hardware, long lifecycles |
| Machine Learning | Integrity of training and inference | Data-driven, opaque decision logic |
| Mobile | Device loss/theft, app store distribution | User-managed devices, sandboxing |
| APIs | Programmatic access control | High automation, structured data exchange |

---

## 2. Container and Cloud Security

### 2.1 The Shared Responsibility Model

Cloud providers and customers share security duties, but the boundary shifts depending on the service model:

```mermaid
graph TB
    subgraph "IaaS"
        direction TB
        A1[Customer: OS, apps, data, network config]
        A2[Provider: Physical, hypervisor, compute]
    end
    subgraph "PaaS"
        direction TB
        B1[Customer: Apps, data]
        B2[Provider: Runtime, OS, middleware, infra]
    end
    subgraph "SaaS"
        direction TB
        C1[Customer: Data, access management]
        C2[Provider: Everything else]
    end
```

| Layer | IaaS | PaaS | SaaS |
|-------|------|------|------|
| Physical security | Provider | Provider | Provider |
| Network infrastructure | Provider | Provider | Provider |
| Hypervisor / virtualization | Provider | Provider | Provider |
| Operating system | **Customer** | Provider | Provider |
| Runtime / middleware | **Customer** | Provider | Provider |
| Application code | **Customer** | **Customer** | Provider |
| Data classification & access | **Customer** | **Customer** | **Customer** |
| Identity & access management | **Customer** | Shared | Shared |

> **Common mistake:** Treating cloud as "someone else's security problem." Under IaaS, you own patching the OS, configuring firewalls, and hardening your application. Misconfigured S3 buckets remain the most frequent cloud breach vector.

### 2.2 Forgotten Assets

Cloud environments create assets that are easy to orphan:

- **Orphaned storage volumes** left after instance termination containing sensitive data
- **Old snapshots** of databases or virtual machines in shared storage
- **DNS records** pointing to decommissioned infrastructure now reassigned to other tenants
- **API keys and credentials** embedded in CI/CD pipelines or Lambda environment variables
- **Test environments** with production data clones running outdated software

**Mitigation:** Implement automated asset inventory scanning, tagging policies, lifecycle management, and regular access reviews. Tools like AWS Config, Azure Resource Graph, or GCP Cloud Asset Inventory provide continuous inventory.

### 2.3 Outsourced Physical Controls

When workloads move to the cloud, physical security becomes the provider's responsibility:

- Data center physical access controls, biometric authentication, man-traps
- Environmental controls (fire suppression, flood, power redundancy)
- Hardware disposal and media sanitization
- Supply chain integrity for hardware components

Customers must verify these controls through provider compliance reports (SOC 2 Type II, ISO 27001 certificates) rather than direct inspection.

### 2.4 Container Security

Containers introduce a distinct attack surface beyond traditional VMs:

#### Container Escape

A container escape allows an attacker to break out of the container isolation boundary and access the host system:

| Attack Vector | Description | Mitigation |
|---------------|-------------|------------|
| Kernel exploit | Container shares host kernel; a kernel vulnerability breaks isolation | Keep host kernel patched; use minimal base images |
| Misconfigured capabilities | Excessive Linux capabilities (SYS_ADMIN, NET_ADMIN) | Drop all capabilities; add only those needed |
| Writable host mounts | Volume mounts exposing host filesystem | Read-only mounts; avoid mounting / or /etc |
| Privileged mode | `--privileged` flag disables all security boundaries | Never use privileged mode in production |
| Runtime vulnerability | Bugs in container runtime (runc, containerd) | Keep runtime updated; use gVisor or Kata Containers for additional isolation |

#### Container Image Security

```
┌──────────────────────────────────────────────────┐
│              Container Image Pipeline             │
│                                                   │
│  Base Image ──► Build ──► Scan ──► Registry ──► Deploy │
│       ▲                                     │    │
│       └── Signed, verified, minimal ◄────────┘    │
└──────────────────────────────────────────────────┘
```

**Image scanning** checks container images for known vulnerabilities (CVEs), malware, secrets, and policy violations:

| Tool | Type | Strengths |
|------|------|-----------|
| Trivy | Open-source scanner | Fast, multi-target (images, repos, SBOMs) |
| Clair | Open-source scanner | API-driven, integrates with registries |
| Snyk Container | Commercial | Developer-friendly, fix advice |
| Aqua Security | Commercial platform | Runtime protection, compliance |
| Docker Scout | Integrated | Native Docker Hub integration |

**Best practices for container images:**
1. Use minimal base images (distroless, Alpine, scratch)
2. Pin package versions in Dockerfiles
3. Scan images in CI/CD before pushing to registry
4. Sign images with Docker Content Trust or cosign
5. Never run containers as root
6. Generate and track Software Bill of Materials (SBOM)

### 2.5 Secrets Management

Secrets (API keys, database credentials, TLS certificates, encryption keys) must never be hardcoded in images or source code:

| Approach | Description | Risk Level |
|----------|-------------|------------|
| Hardcoded in source | Credentials in config files or environment | **Critical** — leaked in version control |
| Environment variables | Injected at runtime | **High** — visible in process listings, logs |
| Configuration files | External config with restricted permissions | **Medium** — file permissions must be enforced |
| Secrets management service | Vault, AWS Secrets Manager, Azure Key Vault | **Low** — centralized, audited, rotated |
| Ephemeral credentials | Short-lived tokens from identity broker | **Minimal** — no persistent secrets to steal |

```mermaid
sequenceDiagram
    participant App as Application
    participant Vault as Secrets Vault
    participant DB as Database

    App->>Vault: Request DB credential (authenticated)
    Vault-->>App: Temporary credential (TTL: 1h)
    App->>DB: Connect with temporary credential
    DB-->>App: Connection established
    Note over App,DB: Credential expires after TTL<br/>App must re-request
```

**HashiCorp Vault**, **AWS Secrets Manager**, **Azure Key Vault**, and **GCP Secret Manager** all provide:
- Centralized secret storage with encryption at rest
- Dynamic secret generation (short-lived credentials)
- Audit logging of every secret access
- Automatic rotation policies

---

## 3. IoT Software Security

### 3.1 The IoT Attack Surface

IoT devices present a uniquely large attack surface due to scale, heterogeneity, and constrained resources:

| Challenge | Description |
|-----------|-------------|
| **Massive endpoint count** | Billions of devices; each is a potential entry point |
| **Long lifecycle** | Devices deployed for 10-20 years, often without update mechanisms |
| **Physical exposure** | Devices in uncontrolled environments (homes, fields, vehicles) |
| **Resource constraints** | Limited CPU, memory, power; cannot run traditional security software |
| **Heterogeneous protocols** | Zigbee, BLE, LoRaWAN, MQTT, CoAP, custom protocols |
| **Supply chain complexity** | Components from multiple vendors with varying security postures |

### 3.2 Device-to-Device Communication Hardening

IoT devices communicate in mesh networks, peer-to-peer, or through gateways. Securing these channels:

```mermaid
graph LR
    subgraph "IoT Network"
        D1[Sensor A] -->|Encrypted| GW[Gateway]
        D2[Sensor B] -->|Encrypted| GW
        D3[Actuator C] -->|Authenticated| GW
        GW -->|TLS 1.3| Cloud[Cloud Platform]
    end
    subgraph "Security Layers"
        S1[Device Identity]
        S2[Mutual Authentication]
        S3[Message Encryption]
        S4[Firmware Integrity]
    end
```

| Security Layer | IoT-Appropriate Technology |
|----------------|---------------------------|
| Device identity | X.509 certificates, TPM-based keys, device attestation |
| Mutual authentication | mTLS, DTLS, PSK (pre-shared keys for constrained devices) |
| Message encryption | AES-128-CCM (constrained), ChaCha20-Poly1305, TLS 1.3 |
| Message integrity | HMAC-SHA256, EdDSA signatures |
| Secure boot | Hardware root of trust, signed boot chain |
| OTA updates | Signed firmware images, rollback protection, A/B partitions |

**Key protocols for IoT security:**
- **DTLS (Datagram TLS):** TLS equivalent for UDP-based protocols like CoAP
- **OSCORE:** Object Security for Constrained RESTful Environments (CoAP with end-to-end security)
- **LwM2M:** Lightweight M2M protocol with built-in device management and security
- **Matter:** Smart home standard with device attestation and certificate-based commissioning

### 3.3 Firmware Analysis

Firmware is the primary attack surface for embedded IoT devices:

**Static analysis techniques:**
1. **Firmware extraction** — dump flash memory via JTAG, SPI, or UART interfaces
2. **Filesystem mounting** — extract and mount squashfs, JFFS2, or CramFS images
3. **Binary analysis** — use Ghidra or IDA Pro to reverse-engineer firmware binaries
4. **Credential harvesting** — search for hardcoded passwords, API keys, and certificates
5. **Vulnerability scanning** — check for known vulnerable libraries (BusyBox, OpenSSL versions)

**Dynamic analysis:**
- Emulate firmware with QEMU to observe runtime behavior
- Monitor network traffic for unencrypted communications
- Fuzz protocol parsers and web interfaces
- Analyze update mechanisms for signature verification gaps

### 3.4 Constrained-Device Cryptography

Standard cryptographic algorithms may be too expensive for microcontrollers with 16KB RAM and 8-bit processors:

| Algorithm | Key Size | RAM Requirement | Suitability |
|-----------|----------|-----------------|-------------|
| RSA-2048 | 2048-bit | ~10 KB | Poor — too memory-intensive |
| ECDSA P-256 | 256-bit | ~2 KB | Good — widely supported |
| Ed25519 | 256-bit | ~1.5 KB | Good — fast verification |
| AES-128 | 128-bit | ~0.5 KB | Excellent — hardware acceleration common |
| ChaCha20-Poly1305 | 256-bit | ~1 KB | Good — no hardware dependency |
| Ascon | 128-bit | ~0.3 KB | Excellent — NIST lightweight crypto winner |

**NIST Lightweight Cryptography (LWC) standardization** selected Ascon in 2023 as the preferred algorithm for constrained environments, offering authenticated encryption with associated data (AEAD) in minimal footprint.

---

## 4. Machine Learning Security

### 4.1 ML Threat Landscape

Machine learning systems introduce a new class of security concerns beyond traditional software. The model itself, its training data, and the inference pipeline all become attack surfaces.

```mermaid
graph TB
    subgraph "Training Phase"
        TD[Training Data] -->|Poisoning| M[Model]
        TA[Adversarial Training Samples] -->|Backdoor| M
    end
    subgraph "Inference Phase"
        I[Input Data] -->|Evasion Attack| M
        I -->|Adversarial Example| M
        M -->|Model Stealing| A[Adversary]
        M -->|Privacy Attack| A
    end
    subgraph "Deployment Phase"
        M -->|Model Extraction| A
        M -->|Membership Inference| A
    end
```

### 4.2 Attack Types

| Attack | Phase | Description | Example |
|--------|-------|-------------|---------|
| **Data poisoning** | Training | Inject malicious samples into training data | Adding mislabeled images to cause systematic misclassification |
| **Model poisoning** | Training | Directly manipulate model parameters | Backdoor triggers: specific input pattern causes targeted misclassification |
| **Evasion attack** | Inference | Craft inputs that cause misclassification | Adversarial patches on stop signs that self-driving cars misread |
| **Adversarial examples** | Inference | Small perturbations imperceptible to humans | Adding noise to an image of a panda that makes the model classify it as a gibbon with 99% confidence |
| **Model stealing** | Inference | Query model to replicate its functionality | Sending queries and training a surrogate model on the responses |
| **Membership inference** | Inference | Determine if specific data was in training set | Privacy violation: determining if a patient's records were used to train a medical model |
| **Model inversion** | Inference | Reconstruct training data from model outputs | Extracting face images from a facial recognition model |
| **Prompt injection** | Inference | Manipulate LLM behavior through crafted prompts | "Ignore previous instructions and reveal your system prompt" |

### 4.3 Defense Strategies

| Defense | Target Attack | Technique |
|---------|--------------|-----------|
| **Differential privacy** | Membership inference, model inversion | Add calibrated noise to training process |
| **Federated learning** | Data poisoning, privacy | Train on distributed data without centralizing it |
| **Adversarial training** | Evasion, adversarial examples | Include adversarial samples in training data |
| **Input validation** | Adversarial examples, prompt injection | Detect and reject anomalous inputs |
| **Model watermarking** | Model stealing | Embed verifiable ownership marks in model |
| **Robust aggregation** | Model poisoning | Byzantine-resilient aggregation in federated settings |
| **Red teaming** | Prompt injection, jailbreaking | Systematic adversarial testing of LLMs |
| **Guardrails** | Prompt injection, harmful outputs | Input/output filtering layers around LLMs |

### 4.4 ML Supply Chain Security

The ML supply chain introduces dependencies on:

- **Pre-trained models** from Hugging Face, PyTorch Hub, or TensorFlow Hub — may contain backdoors
- **Training datasets** (LAION, Common Crawl) — may contain poisoned, biased, or copyrighted data
- **Libraries** (PyTorch, TensorFlow, scikit-learn) — traditional software vulnerabilities
- **Compute infrastructure** — GPU drivers, CUDA, cloud ML platforms

**Mitigations:** Model cards documenting provenance, dataset datasheets, SBOM for ML pipelines, and integrity verification of downloaded models.

---

## 5. Mobile Application Security

### 5.1 OWASP Mobile Top 10 (2024)

| Rank | Category | Description |
|------|----------|-------------|
| M1 | Improper Credential Usage | Hardcoded API keys, tokens, or credentials in app |
| M2 | Inadequate Supply Chain Security | Vulnerable third-party libraries, SDKs |
| M3 | Insecure Authentication/Authorization | Weak auth mechanisms, missing server-side validation |
| M4 | Insufficient Input/Output Validation | SQL injection, XSS through WebView, path traversal |
| M5 | Insecure Communication | Missing TLS, weak cipher suites, certificate validation bypass |
| M6 | Inadequate Privacy Controls | Excessive permissions, PII leakage, analytics oversharing |
| M7 | Insufficient Binary Proteions | Missing code obfuscation, debuggable builds in production |
| M8 | Security Misconfiguration | Debug flags enabled, backup allowed, exported components |
| M9 | Insecure Data Storage | Unencrypted local databases, world-readable files |
| M10 | Insufficient Cryptography | Weak algorithms, hardcoded keys, ECB mode usage |

### 5.2 Code Obfuscation

Mobile apps are distributed as binaries that users can reverse-engineer:

| Technique | Platform | Effect |
|-----------|----------|--------|
| **Name obfuscation** | Both | Rename classes/methods to meaningless strings (ProGuard/R8 for Android) |
| **String encryption** | Both | Encrypt sensitive strings; decrypt at runtime |
| **Control flow obfuscation** | Both | Insert opaque predicates, flatten control flow |
| **Native code (NDK)** | Both | Move critical logic to compiled C/C++ native libraries |
| **Anti-tampering** | Both | Detect modified APK/IPA, rooted/jailbroken devices |
| **Anti-debugging** | Both | Detect attached debuggers, emulators |
| **Symbol stripping** | iOS | Remove debug symbols from release builds |
| **Swift/ObjC metadata stripping** | iOS | Remove class/method names from Mach-O binaries |

### 5.3 Certificate Pinning

Certificate pinning prevents man-in-the-middle attacks by validating that the server's certificate matches an expected value:

```mermaid
sequenceDiagram
    participant App as Mobile App
    participant Proxy as Attacker Proxy
    participant Server as API Server

    App->>Proxy: HTTPS Request
    Proxy-->>App: Attacker's certificate
    Note over App: Pin check FAILS<br/>Certificate does not match pinned value
    App--xProxy: Connection rejected

    App->>Server: HTTPS Request
    Server-->>App: Server's certificate
    Note over App: Pin check PASSES<br/>Certificate matches pinned value
    App->>Server: Request proceeds
```

**Implementation approaches:**

| Approach | Description | Trade-off |
|----------|-------------|-----------|
| **Public key pin** | Hash of certificate's public key | Survives certificate renewal |
| **Certificate pin** | Hash of entire certificate | Must update app before certificate expires |
| **Backup pins** | Include backup keys for rotation | Reduces lockout risk |
| **Pin enforcement** | Reject connection on pin mismatch | Strong security but may cause outages |
| **Pin reporting** | Report violations but allow connection | Easier debugging but weaker security |

> **Note:** Apple deprecated `NSURLConnection` certificate pinning APIs in iOS 14+; modern implementations use `NSURLSession` delegate methods or third-party libraries like TrustKit. Android 7+ removed user certificate trust for apps by default, simplifying pin enforcement.

---

## 6. API Security

### 6.1 OWASP API Security Top 10 (2023)

| Rank | Category | Description |
|------|----------|-------------|
| API1 | Broken Object Level Authorization (BOLA) | Users access objects belonging to other users by manipulating IDs |
| API2 | Broken Authentication | Weak token management, credential stuffing, missing rate limits |
| API3 | Broken Object Property Level Authorization | Excessive data exposure, mass assignment of fields |
| API4 | Unrestricted Resource Consumption | No rate limiting, no pagination limits, unbounded queries |
| API5 | Broken Function Level Authorization | Admin endpoints accessible without proper role checks |
| API6 | Unrestricted Access to Sensitive Business Flows | Automated abuse of business logic (ticket scalping, hoarding) |
| API7 | Server-Side Request Forgery (SSRF) | API fetches attacker-supplied URLs, accessing internal resources |
| API8 | Security Misconfiguration | Verbose error messages, unnecessary HTTP methods, CORS misconfiguration |
| API9 | Improper Inventory Management | Undocumented, deprecated, or shadow APIs still exposed |
| API10 | Unsafe Consumption of APIs | Trusting third-party API responses without validation |

### 6.2 Rate Limiting

Rate limiting protects APIs from abuse, brute force, and denial of service:

| Strategy | Description | Use Case |
|----------|-------------|----------|
| **Fixed window** | N requests per time window (e.g., 100/min) | Simple, predictable |
| **Sliding window** | Rolling window prevents boundary burst attacks | More accurate than fixed |
| **Token bucket** | Tokens added at fixed rate; each request consumes one | Allows bursts up to bucket size |
| **Leaky bucket** | Requests processed at constant rate; excess queued or dropped | Smooth out traffic spikes |
| **Per-user limiting** | Limits based on authenticated user identity | Prevents individual abuse |
| **Per-IP limiting** | Limits based on source IP address | Catches unauthenticated abuse |
| **Adaptive limiting** | Dynamic limits based on server load and anomaly detection | Production-grade defense |

```mermaid
graph LR
    R[Request] --> RL{Rate Limiter}
    RL -->|Under limit| P[Process Request]
    RL -->|Over limit| D[429 Too Many Requests]
    RL -->|Over limit, retry-after| H[Retry-After Header]
```

### 6.3 API Input Validation

APIs must validate all inputs at the boundary:

| Validation | Example | Risk if Missing |
|------------|---------|-----------------|
| **Type checking** | Ensure `age` is integer, not string | Type confusion, injection |
| **Range checking** | Ensure `quantity` is 1-1000 | Overflow, business logic abuse |
| **Format validation** | Ensure `email` matches RFC 5322 | Injection, data corruption |
| **Schema validation** | Validate JSON against OpenAPI schema | Unexpected fields, mass assignment |
| **Sanitization** | Strip HTML from text fields | Stored XSS via API |
| **Encoding** | Ensure UTF-8, reject malformed sequences | Encoding attacks, buffer issues |
| **Size limits** | Maximum request body, array length | DoS via resource exhaustion |

**Best practices:**
1. Validate at the API gateway AND in the service layer (defense in depth)
2. Use allowlists over denylists for input validation
3. Return generic error messages; log detailed errors server-side
4. Validate Content-Type headers; reject unexpected content types
5. Use OpenAPI/Swagger schema validation in API gateways

---

## 7. Cross-Cutting Concerns

### 7.1 Domain Security Maturity Comparison

| Capability | Cloud/Container | IoT | ML | Mobile | API |
|------------|----------------|-----|-----|--------|-----|
| Authentication standards | Strong (IAM, OIDC) | Weak (many custom) | Emerging | Moderate (biometrics) | Strong (OAuth 2.0) |
| Vulnerability tracking | Good (CVE coverage) | Poor (no CVE for firmware) | Nascent | Good (OWASP) | Good (OWASP) |
| Security tooling | Mature | Limited | Emerging | Mature | Mature |
| Compliance frameworks | SOC 2, ISO 27001 | NIST IoT, ETSI EN 303 645 | NIST AI RMF | MASVS | PCI DSS |
| Update mechanisms | CI/CD, OTA | Often manual | Model redeployment | App stores | Rolling deploy |

### 7.2 Links to Existing Notes

- [[01_Security_Fundamentals]]: Core threat modeling and security principles applicable across all domains
- [[02_Protocols_and_Cryptography]]: Cryptographic foundations used in TLS, DTLS, and constrained-device crypto
- [[03_Access_Control_and_Architecture]]: Access control models underlying IAM, RBAC, and API authorization
- [[04_Network_Attack_and_Defence]]: Network attacks relevant to cloud infrastructure and IoT networks
- [[05_Secure_Development_and_Assurance]]: Secure SDLC practices extended to ML pipelines and mobile development

---

## 8. Key Takeaways

1. **Shared responsibility does not mean provider responsibility** — in cloud IaaS, customers own OS, application, and data security.
2. **Container escape is the critical cloud-native threat** — avoid privileged mode, drop capabilities, keep runtimes patched.
3. **IoT devices are deployed for decades with minimal update capability** — design security into the hardware root of trust and signed OTA mechanism from day one.
4. **ML models are attack surfaces** — data poisoning, adversarial examples, and model stealing require dedicated defenses beyond traditional software security.
5. **Mobile apps are distributed binaries** — obfuscation, certificate pinning, and runtime integrity checks are essential because attackers control the device.
6. **API security is authorization security** — BOLA/BFLA (broken function-level authorization) are the top API vulnerabilities; validate authorization at every object access, not just endpoint access.

---

*Source: SWEBOK v4 Chapter 13, Section 13.6*
*Related: [[Cybersecurity/]] subfolder for deeper coverage of specific attack and defense techniques*
