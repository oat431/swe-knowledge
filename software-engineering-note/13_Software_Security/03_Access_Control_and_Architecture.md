---
tags:
  - security
  - access-control
  - mls
  - distributed-systems
  - inference-control
  - software-security
source: "Anderson, Ross. Security Engineering, 3rd Edition. Chapters 6, 7, 9, 10."
created: 2026-07-21
---

# 03 — Access Control & Architecture

> *"Anything your computer can do for you it can potentially do for someone else."* — Alan Cox

A comprehensive synthesis of **Anderson's Security Engineering** covering operating-system access control (Ch 6), distributed systems fundamentals (Ch 7), multilevel security and IFC (Ch 9), and boundary/compartmentation policies (Ch 10). These chapters form the architectural backbone of security engineering — the mechanisms, policies, and failure modes that protect systems from the hardware up.

---

## 1. Operating System Access Control (Ch 6)

### 1.1 The Access Control Matrix

Access control can be modelled as a **matrix** of permissions:
- **Rows** = principals (users, processes)
- **Columns** = resources (files, ports, devices)
- **Cells** = permissions (`r`, `w`, `x`, `-`)

The matrix does not scale (50,000 staff × 300 apps = 15M entries). Two compression strategies exist:

| Strategy | Stores By | Examples |
|----------|-----------|----------|
| **ACLs** (Access Control Lists) | Columns (per resource) | Unix, Windows, macOS, iOS |
| **Capabilities** (tickets/permissions) | Rows (per principal) | Kerberos tickets, TLS certs, Android permissions |

### 1.2 Groups and Roles

| Concept | Definition |
|---------|------------|
| **Group** | A list of principals |
| **Role** | A fixed set of access permissions that principals assume for a period (e.g., "officer of the watch") |

Groups and roles combine: a bank manager might be in group `manager` while assuming role `acting manager of Cambridge branch`.

### 1.3 ACLs in Depth

ACLs are the dominant mechanism in mainstream OSes. They are **data-oriented** — suited when users manage their own file security.

#### Unix ACLs
- Three permission triples: `rwx` for **owner**, **group**, **world**
- `root` (uid 0) bypasses all checks — DAC model
- **suid** bit: program runs with owner's privileges, enabling (user, program, file) triples but introducing serious vulnerabilities
- **Limitations**: single user named, no program-level control, poor mutable state handling, hard to audit globally

#### Extended ACLs (POSIX)
- Allow arbitrary numbers of named users and groups per file
- In theory, ACL + suid can achieve desired effects; in practice, programmers over-privilege to "get the job done"

### 1.4 Capabilities

A capability is a row of the access matrix — it grants a principal specific rights to specific resources.

**Strengths**: efficient runtime checks, easy delegation (Bob can sign a certificate delegating read access to David for 4 hours)

**Weaknesses**: hard to find all who have access to a file; revocation is expensive

**Modern usage**: file descriptors (Unix), Kerberos tickets, TLS certificates, Android/iOS app permissions, CHERI CPU extensions

### 1.5 DAC vs MAC

| | DAC (Discretionary) | MAC (Mandatory) |
|---|---|---|
| **Control** | Machine owner / admin | System vendor or remote authority |
| **Model** | Users set policy on their files | Policy enforced regardless of user actions |
| **Examples** | Unix permissions, Windows file ACLs | SELinux, Windows integrity levels, iOS sandbox |
| **Origin** | Time-sharing systems (1960s) | Military MLS research (1970s), DRM (1990s) |

MAC emerged from military research, was adapted for DRM, then became mainstream defense against malware (Trusted Boot, TPM, integrity levels).

### 1.6 Platform-Specific Implementations

#### macOS
- FreeBSD Unix on Mach kernel, with **TrustedBSD** MAC extensions
- Root disabled by default; `wheel` group for admin
- DTE (Domain and Type Enforcement) protects core system components

#### iOS
- Mach + FreeBSD fusion; unique file pathnames (not inode-based)
- **App permissions = capabilities** (trust-on-first-use model)
- **Secure Enclave** (SE): separate processor for keys, biometrics, payments
- Closed ecosystem: only Apple-signed apps; 256-bit AES key in fusible links

#### Android
- Linux-based; each app = different UID
- Permissions (capabilities) requested in manifest; dangerous ones require user approval (trust-on-first-use since Android 6)
- **SELinux** (since Android 5): MAC via type enforcement; three-party consent (user, developer, platform)

#### Windows
- Evolved from no access control (DOS) → Unix-like NT → complex enterprise system
- Richer ACLs: `AccessDenied`, `AccessAllowed`, `SystemAudit` (parsed in that order)
- **Domains**, **Active Directory**, **Group Policy**, **UAC** (User Account Control)
- **Integrity levels** (Vista+): Low (browser) → Medium → High → System (Biba model: NoWriteUp)
- **Dynamic Access Control**, **RBAC/ABAC** (Win 8+), cloud identity (Microsoft accounts)

### 1.7 Middleware Access Control

#### Databases (Oracle, DB2, MySQL)
- Own access control mechanisms (ACL + capability hybrid)
- Often bypass OS controls; directly accessible from web → SQL injection risk
- Slammer worm (2003): stack overflow in SQL Server → massive traffic floods

#### Browsers
- **Same-origin policy**: JavaScript can only communicate with its origin IP
- Sandbox execution; Chrome runs each tab in separate OS process
- Vulnerabilities: CSRF, misconfigured CORS, drive-by downloads, tracker networks undermining origin isolation

### 1.8 Sandboxing and Virtualisation

| Mechanism | Description |
|-----------|-------------|
| **Sandbox** | Restricted environment (JVM, JavaScript); code has limited disk/network access; same-origin policy enforced by interpreter |
| **Virtual Machine (VM)** | Complete OS on hypervisor; hardware-assisted (Intel VT-x, 2006); isolation via VMM |
| **Container** | Isolated guest process sharing a kernel; namespaces, cgroups, syscall filtering; lighter than VMs |
| **Enclave** | Encrypted memory region (Intel SGX, Arm TrustZone); CPU-level isolation; vulnerable to Spectre |

**Key risk**: 194 of top 1000 Docker Hub containers (2019) had blank root passwords. Cloud IAM misconfiguration is a leading cause of data exposure.

### 1.9 Hardware Protection

#### Intel Processors
- **Rings** (0–3): kernel (0) to user (3)
- Modern: 9 rings total — ring 0–3, VMM root mode (0–3), SMM (BIOS)
- **VT-x** (2006): hardware VM support
- **SGX** (2015): encrypted enclaves; vulnerable to ROP from enclaves; MEE encrypts memory

#### Arm Processors
- Multiple supervisor modes; MPU/MMU variants
- **TrustZone** (2004): "two worlds" — normal OS + secure TEE
- **CHERI** (2018+): fine-grained capability support in CPU; sub-thread sandboxing; promises to prevent most zero-day exploits

### 1.10 Software Attacks and Defences

#### Attack Taxonomy
| Attack | Mechanism |
|--------|-----------|
| **Stack smashing** | Buffer overflow overwrites return address → attacker code executes (Morris worm, 1988) |
| **Use-after-free** | Freed memory still referenced → malicious chunk takes its place; #1 RCE cause in browsers |
| **SQL injection** | Unsanitised user input contains SQL → database compromise |
| **Race conditions (TOCTTOU)** | Time-of-check-to-time-of-use: state changes between permission check and action |
| **Return-oriented programming (ROP)** | Chain of "gadgets" (instruction sequences ending in return) → Turing-complete attack; bypasses DEP |
| **Meltdown/Spectre** (2018) | CPU pipeline side-channels: out-of-order/speculative execution leaks memory across processes |
| **Confused deputy** | Program with dual authority (its own + caller's) tricked into abusing higher privilege |

#### Defence Stack
1. **Stack canaries**: random value beside return address; overwrite detection
2. **DEP** (Data Execution Prevention): memory marked data or code (2003)
3. **ASLR** (Address Space Layout Randomisation): randomises memory layout per instance
4. **Control flow integrity**: validate indirect control-flow transfers at runtime
5. **Better languages**: Rust eliminates memory-safety bugs at compile time
6. **DevSecOps**: rapid patching, continuous security testing

#### Principles
- **Least privilege**: programs should have minimum necessary access
- **Safe defaults**: easiest path should be secure
- **Environmental creep**: Unix evolved from lab tool to global Internet OS — assumptions that held in 1970 fail today

---

## 2. Distributed Systems (Ch 7)

### 2.1 Concurrency Problems

| Problem | Description | Example |
|---------|-------------|---------|
| **TOCTTOU** | Time-of-check-to-time-of-use: state changes between auth and action | Unix `mkdir` two-phase attack; OS/360 file open race |
| **Replay attacks** | Out-of-date credentials reused | Stolen session tokens |
| **Old data** | Propagation delay in security state | Credit card hot lists (floor limits, stand-in processing) |
| **Inconsistent updates** | Two processes update shared state without coordination | Banking: credits before debits in overnight batch |
| **Deadlock** | Mutual wait cycles | Dining philosophers; "battle of the forms" in e-commerce |
| **Non-convergent state** | Systems never reach consistency | Payment gateways guessing mandatory/optional fields across jurisdictions |

#### ACID Properties
- **Atomic**: all-or-nothing
- **Consistent**: invariants preserved (e.g., debits = credits)
- **Isolated**: serialisable
- **Durable**: once done, cannot be undone

Reality: ACID is often too much, too little, or both. **Convergence** (eventual consistency with timestamps/versions) is often sufficient.

### 2.2 Secure Time

- **NTP**: moderate protection; vulnerable to jamming and DDoS amplification (NTP reflection attacks)
- **Lamport time**: only cares about happened-before ordering, not absolute dates
- **Cinderella attack**: wind clock forward to expire security licenses

### 2.3 Fault Tolerance

#### Byzantine Failure
Lamport, Shostak & Pease: `n` generals, `t` traitors — solution exists **iff** `n ≥ 3t + 1`.

With digital signatures (non-repudiation), the problem simplifies. Fail-stop processes (halt on inconsistency) are easier to build with than fail-arbitrary.

#### Redundancy Levels
| Level | Protects Against | Cannot Protect Against |
|-------|-----------------|----------------------|
| Hardware (RAID, Stratus) | Disk/CPU failure | Malicious software |
| Process groups (multiple servers) | Physical subversion | Authorised user attacks |
| Backup/checkpoints + journals | Logical attacks, ransomware | Slow corruption |
| Fallback systems (manual imprinters) | Total system failure | — |

**Key insight**: redundancy introduces its own failure modes. Multi-engine planes kill more novice pilots than single-engine. Netflix's "chaos monkeys" continuously test resilience.

#### Service Denial (DDoS)
- Botnets → flood targets (extortion of bookmakers, gamer "booter" services)
- Defence: replication (Akamai), arrest, or both
- **Fallback attack**: force victims to weaker mode (chip → magstripe on payment cards)

### 2.4 Naming

#### Needham's 10 Principles (1993)
1. Names exist to **facilitate sharing** of changeable data
2. Name resolution brings **all distributed system problems** (e.g., two-channel auth broken via SIM swap)
3. Don't assume few names needed (credit cards: 13 → 16 digits)
4. **Global names buy less than you think** (local-to-global-to-local resolution)
5. Names imply **commitments** — keep scheme flexible for org changes
6. Names may **double as passwords/capabilities** (SSN, Utrecht fraud, Norway ID number)
7. **Incorrect names should be obvious** (check digits, certificates)
8. **Consistency is hard** — often fudged (barcodes, replicated directories)
9. **Don't get too smart** — phone numbers more robust than computer addresses
10. **Avoid early binding** — use config files, DNS, external services

#### Zooko's Triangle
A naming system cannot simultaneously be **globally unique**, **decentralised**, and **human-meaningful**. Pick two.

#### Identity vs Naming
- **Identity**: two different names correspond to same principal (symbolic link)
- **Failures**: pre-issue passport fraud, postmortem applications, lost civil registers, cultural naming conflicts (mononyms, patronymics, name changes)
- **ID cards**: political and cultural — some cultures see them as oppressive, others as normal

---

## 3. Multilevel Security (Ch 9)

### 3.1 Security Policy Model

A proper **security policy model** is a succinct (≤1 page) statement of required protection properties — distinct from corporate platitudes about "need-to-know."

| Term | Definition |
|------|------------|
| **Security policy model** | Succinct protection properties (1 page) |
| **Security target** | Detailed description of a specific implementation's mechanisms |
| **Protection profile** | Implementation-independent requirement (Common Criteria) |
| **TCB** (Trusted Computing Base) | Components whose correct functioning is sufficient to enforce the policy |

### 3.2 Bell-LaPadula (BLP) Model (1973)

The foundational MLS confidentiality model:

| Property | Rule | Meaning |
|----------|------|---------|
| **Simple security** | **No read up** (NRU) | Process at Low cannot read High data |
| **\*-property** | **No write down** (NWD) | Process at High cannot write to Low (prevents Trojan leaks) |

The \*-property was the critical innovation — it prevents malware running at High from exfiltrating data to Low.

#### Refinements
- **Tranquility**: strong (labels never change) vs weak (labels change only per policy)
- **High water mark principle**: process label rises to highest data read; all subsequent writes are at that level
- **System Z critique** (McLean): BLP rules alone insufficient — need explicit constraints on label changes

#### Limitations
- Assumes information flow control is sufficient (trusted subjects needed for real apps)
- Silent on creation/deletion of subjects/objects
- High water mark leads to label creep (everything ends up at highest level) or fragmentation

### 3.3 Alternative MLS Models

| Model | Focus | Key Idea |
|-------|-------|----------|
| **Noninterference** (Goguen & Meseguer, 1982) | Confidentiality | High's actions have **no effect** on what Low can see |
| **Nondeducibility** (Sutherland, 1986) | Confidentiality | Low cannot deduce High input with certainty (weaker) |
| **Biba** (1975) | **Integrity** | Dual of BLP: **no read down**, **no write up**; low-water-mark principle |
| **Type Enforcement (TE)** / **DTE** | Both | Subjects in domains, objects in types; matrix of allowed interactions |
| **RBAC** (Ferraiolo & Kuhn, 1992) | Policy management | Permissions attached to roles, not users; supports delegation |
| **ABAC** | Context-aware | Adds context attributes (device, location, time) |

#### Biba in Practice
- Windows Vista+: integrity levels (Low → Medium → High → System); **NoWriteUp** enforced
- Used in safety-critical systems: signalling vs passenger info; power dispatch vs safety
- Limitation: cannot express assured pipelines; trusted subjects still needed

#### DTE/SELinux
- Subjects and objects in types; matrix of allowed type pairs
- Used in Android (since v5), Sidewinder firewall
- Supports **trusted pipelines**: each stage only talks to adjacent stages
- Combines with RBAC: users → roles → domains → types

### 3.4 Historical MLS Systems

| System | Era | Contribution |
|--------|-----|-------------|
| **ADEPT-50** (IBM S/360) | 1967 | First MAC system; triples of level, compartment, group |
| **Multics** | 1965+ | Honeywell product; template for "trusted systems"; Karger-Schell evaluation |
| **SCOMP** | 1983 | First A1-rated (Orange Book); formally verified; 32 compartments |
| **NRL Pump** | 1990s | One-way data diode with bounded backward leakage; enables system-high architecture |
| **CMW** (Compartmented Mode Workstation) | 1990s | Floating labels; analyst reads Top Secret in one window, writes Secret in another |

**Modern approach**: separate machines at different levels connected by data diodes and KVM switches — cheaper and simpler than MLS OSes.

---

## 4. Boundaries and Compartmentation (Ch 10)

### 4.1 The Lattice Model

While BLP controls **vertical** (hierarchical) information flow, **compartmentation** controls **lateral** flow across groups.

- **Codewords** (e.g., ULTRA, CRYPTO, NUCLEAR) restrict access beyond classification level
- Classification + codewords = **compartment** (up to 2^N compartments for N codewords)
- The set of all (level, {codewords}) forms a **lattice**: any two labels have a least upper bound and greatest lower bound

**Problem**: BLP + lattice = compartments sealed from each other. Real intelligence work needs to **combine** and **downgrade** data — which lattice models cannot model.

### 4.2 Compartmentation Failures

- **Aldrich Ames** (CIA): accumulated access to many compartments over long career → betrayed entire US agent network in Russia
- **Walker spy ring**: Navy tried to compartment ships but operational needs forced all 800 ships to share same cipher keys
- **Ed Snowden**: sysadmin access → bypassed discretionary controls; Vault 7 (CIA) revealed no compartmentation of cyberweapons, shared admin passwords, no activity monitoring

**Post-9/11 shift**: "collect it all" (NSA), make everything searchable → compartments became obstacles. Result: centralised search (XKeyscore) replaces compartmented access — at cost of insider threat.

### 4.3 The BMA Security Policy (Medical)

Commissioned by the British Medical Association (1995) in response to government push for centralised Electronic Patient Record.

**Nine Principles**:
1. **Access control**: each record has ACL naming who may read/append
2. **Record opening**: clinician opens with herself + patient on ACL
3. **Control**: one responsible clinician; only she may alter ACL (add healthcare professionals)
4. **Consent and notification**: patient must be notified of all ACL members; consent required
5. **Persistence**: no deletion until retention period expires
6. **Attribution**: all accesses logged with subject name, date, time
7. **Information flow**: data from record A may be appended to B only if B's ACL ⊆ A's ACL
8. **Aggregation control**: special notification if new ACL member already has access to many records
9. **Trusted computing base**: subsystem must enforce principles; subject to independent evaluation

**Key insight**: sensitivity is patient-determined, not government-determined. MLS (Secret/Confidential/Restricted) fails because a prescription for ARVs is simultaneously low (prescription) and high (reveals HIV status).

#### Modern Context
- **HIPAA** (US, 1996): Privacy Rule; covered entities must protect PHI; patient right to copies
- **Iv v Finland** (ECHR, 2010): "practical and effective protection to exclude any possibility of unauthorised access"
- Bulk threats: hospital record sales to drug companies; misconfigured cloud servers; centralisation amplifies both value and access

### 4.4 Privacy for Tigers (Wildlife Conservation)

Wildbook: ML-based image recognition links animal sightings across countries.

**Threat model**: 
- Level 3 (citizen scientists): includes poachers
- Level 2 (conservancy staff): includes careless/disloyal minority
- Level 1 (admins): hoped trustworthy

**Protection goals**: prevent actionable intelligence ("species A at location X at time T"); protect movement models (ML "lore" equivalent to decade of ranger experience)

**Strategy**: **two-dimensional transparency** — staff see who's been looking at records in their species group AND their location; peer accountability replaces central security bureaucracy.

### 4.5 Chinese Wall Model

The **Chinese Wall** policy (Brewer & Nash, 1989) prevents conflicts of interest in professional services:
- A consultant who has accessed Client A's data in sector X cannot access Client B's data in sector X
- Once a "wall" is crossed (access to one client in a conflict-of-interest class), all other clients in that class become inaccessible
- **Dynamic**: access rights change based on history of accesses

This model is used in law firms, accountancy, investment banking, and increasingly in cloud services where a provider must not use one customer's data to benefit a competitor.

---

## 5. Key Concepts Summary

### Access Control Primitives
| Primitive | Storage | Revocation | Delegation |
|-----------|---------|------------|------------|
| **ACL** | Per-object | Easy (change ACL) | Hard (need suid/capabilities) |
| **Capability** | Per-subject | Hard (find all holders) | Easy (sign delegation cert) |

### Security Models
| Model | Property | Domain |
|-------|----------|--------|
| **Bell-LaPadula** | No read up, no write down | Military confidentiality |
| **Biba** | No read down, no write up | Safety/Integrity |
| **Noninterference** | High actions invisible to Low | Formal MLS |
| **Type Enforcement** | Domain-type interaction matrix | OS hardening (SELinux) |
| **Chinese Wall** | Dynamic conflict-of-interest barriers | Professional services |
| **BMA** | Patient-consent-driven ACLs | Healthcare |

### Architectural Principles
1. **Least privilege**: grant minimum necessary access
2. **Safe defaults**: easiest path = secure path
3. **Defence in depth**: layers (hardware → OS → middleware → app)
4. **TCB minimality**: trusted code should be small enough to verify
5. **Fail-safe**: failures should deny access, not grant it
6. **Separation of mechanism and policy**: flexibly enforce different policies
7. **Complete mediation**: every access checked every time
8. **Open design**: security should not depend on secrecy of mechanism

### Common Failure Patterns
- Environmental creep (assumptions from original context fail at scale)
- Centralisation amplifying risk (more value + more access = more abuse)
- Incentive misalignment (who guards vs who suffers)
- Complexity as enemy of security (Windows, database, browser access control are too complex to configure correctly)
- Side channels circumventing logical controls (Meltdown/Spectre, power analysis)

---

## References

- Anderson, R. (2020). *Security Engineering*, 3rd Edition. Wiley.
- Bell, D. E., & LaPadula, L. J. (1973). *Secure Computer Systems*.
- Biba, K. J. (1975). *Integrity Considerations for Secure Computer Systems*.
- Needham, R. M. (1993). Naming. In *Distributed Systems* (ed. Mullender).
- Lamport, L., Shostak, R., & Pease, M. (1982). The Byzantine Generals Problem.
- Saltzer, J. H., & Schroeder, M. D. (1975). The Protection of Information in Computer Systems.
- Anderson, R. J. (1996). *A Security Policy Model for Clinical Information Systems* (BMA policy).
