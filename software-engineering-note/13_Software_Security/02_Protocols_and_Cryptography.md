---
tags:
  - security
  - protocols
  - cryptography
  - tls
  - authentication
  - software-security
source: "Anderson Security Engineering, Chapters 4–5"
created: 2026-07-21
---

# Protocols and Cryptography

*From Ross Anderson's *Security Engineering*, Chapters 4 (Protocols) and 5 (Cryptography)*

---

## Part I: Protocols (Chapter 4)

### 4.1 What Are Security Protocols?

Protocols specify the steps that principals use to establish trust relationships in a system — authenticating identity claims, demonstrating ownership of credentials, or establishing claims on resources. They range from simple (swiping a badge) to complex (cryptographic key distribution). Protocols may be human-mediated (like the ritual of serving wine in a restaurant, which provides privacy, integrity, and non-repudiation) or fully technical.

Evaluating a protocol involves two questions:
1. Is the threat model realistic?
2. Does the protocol deal with it?

> **Key insight:** Protocols that evolve over decades often meet both social expectations and technical threats better than rapidly deployed technical solutions.

### 4.2 Password Eavesdropping Risks

Early remote key entry systems (garage doors, 1990s car locks) simply broadcast a serial number. They were broken by **grabbers** — devices that record and replay codes.  

**Countermeasure evolution:**
- Separate codes for lock/unlock → thief records unlock in morning, replays at night
- Longer passwords (16→32 bits) → grabbers still work; only 1–2 codes per car
- Serial numbers as passwords → visible on orders, invoices, dealer records

**The real lesson:** Static passwords for online systems are hazardous. You need cryptographic authentication protocols for anything worth stealing.

### 4.3 Simple Authentication: Challenge-Response

#### Basic Token Authentication
```
T → G : T, {T, N}_KT
```
The token sends its serial number `T` and encrypts it together with a **nonce** `N` (number used once). The garage decrypts, verifies the nonce is fresh, and checks the plaintext matches `T`.

**Nonce options:**

| Type | Pros | Cons |
|------|------|------|
| Random number | Simple | Must remember past codes (memory cost) |
| Counter | No memory needed | Synchronization problems |
| Timestamp | Natural ordering | Requires clocks (cost, drift) |

**Key diversification:** `KT = {T}_KM` — derive each device key from a global master key. Compromising one token only lets you clone that subscriber, not the whole system.

**Common failure:** Short serial numbers. A 128-bit key derived from a 16-bit device number gives only 2^16 possible keys.

#### Challenge-Response for Cars
```
E → T : N
T → E : T, {T, N}_K
```
The engine controller sends a random challenge; the key transponder encrypts it. Sound in theory, but **every major implementation failed (2005–2015):**

| System | Weakness |
|--------|----------|
| TI DST (40-bit) | Brute-forced from 2 responses |
| DST80 | Side channels + 24-bit key entropy (Hyundai) |
| Keeloq | Key diversification via XOR: `KT = T ⊕ KM` |
| Hitag2 (48-bit) | Weak cipher, key recovery in minutes |
| Megamos Crypto (96-bit) | Effective strength only 49 bits; global master keys |

**Passive Keyless Entry (PKES) disaster:** Relay attacks let thieves amplify the key fob signal from inside a house to unlock a car parked outside. UK car thefts surged 56% in 2017.  

**Fix:** Ultra-wideband (UWB) with intrinsic ranging (802.15.4z), measuring distance to ±10cm up to 150m.

#### Two-Factor Authentication (Challenge-Response Password Generators)
```
S → U : N
U → P : N, PIN
P → U : {N, PIN}_K
U → S : {N, PIN}_K
```
User enters server nonce + PIN into device; device returns encrypted result. Effective — DoD cut intrusions 46% after deploying Common Access Card.

**But vulnerable to real-time man-in-the-middle (MITM) attacks** where the phisher relays challenges between the bank and the victim.

### 4.4 The MIG-in-the-Middle Attack (Classic Relay Attack)

During the Angolan war, Soviet MIGs allegedly relayed IFF challenges from South African air defenses to Angolan ground batteries, which forwarded them to SAAF bombers. The bombers' correct responses were relayed back, letting the MIGs pass as friendly aircraft.

This illustrates the **relay/man-in-the-middle attack** — the fundamental flaw in challenge-response when the channel isn't physically bound to the principal.

> John Conway's analogy: "It's easy to get at least a draw against a grandmaster at postal chess — just play two grandmasters simultaneously, relaying moves between them."

### 4.5 Reflection Attacks

When two principals need mutual authentication, an attacker can **reflect** a challenge:
```
F → B : N
B → F' : N          (reflects to wingman)
F' → B : {N}_K
B → F : {N}_K       (forwards response)
```

**Fix:** Include both parties' names in the response: `{B, N}_K` instead of just `{N}_K`.

### 4.6 Chosen Protocol Attacks

The **Mafia-in-the-middle attack:** A porn site relays a coin dealer's transaction authorization to a visiting customer. The customer thinks they're proving their age; they're actually authorizing a gold coin purchase.

**Principle:** Using crypto keys in multiple applications is dangerous. Never let others bootstrap their security off yours.

### 4.7 Key Management Protocols

#### Trust-on-First-Use (TOFU) / Resurrecting Duckling
On first power-up after factory reset, the device trusts the first crypto key it sees. Used in:
- Digital tachographs (encrypted gearbox sensor pulse train)
- Homeplug AV (powerline encryption)
- Medical device pairing

If false imprinting occurs, the reset button "kills" the device and resurrects it to a newborn state.

#### Simple Key Distribution
```
A → S : A, B
S → A : {A, B, K_AB, T}_K_AS, {A, B, K_AB, T}_K_BS
A → B : {A, B, K_AB, T}_K_BS, {M}_K_AB
```
Sam (trusted third party) creates a session key encrypted to both Alice and Bob.

#### Needham-Schroeder Protocol (1978)
```
1. A → S : A, B, N_A
2. S → A : {N_A, B, K_AB, {K_AB, A}_K_BS}_K_AS
3. A → B : {K_AB, A}_K_BS
4. B → A : {N_B}_K_AB
5. A → B : {N_B - 1}_K_AB
```

**Subtle problem:** Bob assumes `K_AB` is fresh, but Alice could have waited a year. If Alice's key is compromised, all past session keys are vulnerable. **Revocation is hard** — Sam would need complete logs forever.

The protocol is sound under the **original** assumptions (all principals behave, attacks come from outsiders). It fails under modern assumptions (the enemy is among the users).

#### Kerberos
The practical derivative of Needham-Schroeder. Uses **two** trusted third parties:
- **Authentication Server** (AS) — users log on with passwords
- **Ticket Granting Server** (TGS) — issues tickets for specific resources

```
A → S : A, B
S → A : {T_S, L, K_AB, B, {T_S, L, K_AB, A}_K_BS}_K_AS
A → B : {T_S, L, K_AB, A}_K_BS, {A, T_A}_K_AB
B → A : {T_A + 1}_K_AB
```

**Fixes Needham-Schroeder's revocation problem** by using timestamps (`T_S`, `L`) instead of nonces. But introduces **clock synchronization vulnerability**.

Kerberos is a **trusted third-party (TTP) protocol** — Sam can be compelled (by warrant) to turn over keys.

#### OAuth / OpenID Connect
OAuth is a delegation framework (not designed for authentication). **OpenID Connect** is a "profile" of OAuth for authentication (what you use when logging into a newspaper with Google/Facebook). OAuth provides short-lived access tokens for scalability; OpenID Connect ties down the details for pure authentication.

### 4.8 Design Assurance

**Formal methods** (BAN logic, CSP, Isabelle theorem prover):
- Force designers to make everything explicit
- Find bugs — but only in the part verified
- Larry Paulson verified SSL/TLS with Isabelle in 1998; ~1 security bug found every year since — not in the basic design, but in added features and implementation issues (timing attacks)

**Robustness principles:**
- Interpretation should depend only on content, not context
- Everything important (names, roles) stated explicitly in messages
- Unambiguous message formats — no way to interpret data in multiple ways
- Randomness helps robustness at all layers

> **Golden rule:** Don't design your own protocols. Get a specialist, and publish for peer review. Even specialists get v1 wrong.

---

## Part II: Cryptography (Chapter 5)

### 5.1 Introduction

Cryptography is where security engineering meets mathematics. Three levels of approach:
1. **Underlying intuitions** — what every security engineer needs
2. **Mathematics** — for proofs and precise constructions
3. **Cryptographic engineering** — tools, experience of failure modes

Many tools offer **unsafe defaults** (e.g., Microsoft's CAPI nudges engineers toward ECB mode).

### 5.2 Historical Background

| Era | Cipher | Key Idea |
|-----|--------|----------|
| Roman | Caesar cipher | Fixed shift (key = 'D') |
| Medieval Arab | Monoalphabetic substitution | Keyword permutes alphabet |
| 16th c. | Vigenère | Polyalphabetic: repeating keyword, modular addition |
| 1854 | Playfair | First block cipher (digraphs in 5×5 grid) |
| WWI | One-time pad (Vernam) | Key as long as plaintext, never reused |
| WWII | Rotor machines (Enigma, Typex) | Mechanical pseudorandom keystream |
| 19th c. banking | Test keys (codebooks) | One-way authentication via compressed addition |

**One-time pad:**
- Perfect secrecy proven by Shannon (1948): cipher has perfect secrecy iff there are as many possible keys as plaintexts, each equally likely
- **Fails completely** to protect message integrity — attacker can flip bits
- Key reuse is catastrophic: `C1 - C2 = M1 - M2` (Venona project exploited Soviet key reuse)

### 5.3 Security Models

| Model | Description |
|-------|-------------|
| **Perfect secrecy** | All plaintexts equally likely given ciphertext (one-time pad) |
| **Concrete security** | `(t, ε)`-secure: adversary working time `t` succeeds with probability ≤ ε |
| **Indistinguishability (standard model)** | No efficient discriminator for plaintexts of equal length; semantic security |
| **Random oracle model** | Primitive is pseudorandom — indistinguishable from a random function. Enables more efficient constructions |

**Key sizes and work factors:**
- Bitcoin miners (~Denmark's electricity) solve 68-bit puzzle in ~10 minutes
- 80-bit key: ~1 month for them; 128-bit key: `2^48` times harder
- **128-bit keys are the modern default**

### 5.4 Cryptographic Primitives

#### Hash Functions (Random Functions)
- Accept any-length input → fixed-length output (n bits)
- Properties: **one-wayness**, **collision resistance**, **pseudorandomness**

**Birthday Theorem:** To find a collision in an n-bit hash, you need ~2^(n/2) attempts. A class of 23 pupils has >50% chance of shared birthday (from 365 possibilities = ~√365).

| Hash | Output | Collision Work Factor | Status |
|------|--------|----------------------|--------|
| MD4 | 128-bit | 2^64 | Broken 1998 |
| MD5 | 128-bit | 2^64 | Broken 2004; collisions now trivial |
| SHA-1 | 160-bit | 2^80 | Broken 2017 (collision); chosen-prefix 2020 |
| SHA-2 (256/512) | 256/512-bit | 2^128/2^256 | **Currently secure** |
| SHA-3 (Keccak) | Variable | Strong | **Standard since 2015** |

#### Stream Ciphers (Random Generators)
Short input (key + seed/IV) → long pseudorandom keystream. Data encrypted via XOR.
- **Critical rule:** Never reuse the same keystream (Venona lesson)
- Modern practice: use key + initialization vector (IV) to start keystream at different positions

#### Block Ciphers (Random Permutations)
Fixed-size input → fixed-size output, keyed, invertible family of permutations.
```
C = {M}_K
```

**Attack types:**

| Attack | Attacker capability |
|--------|-------------------|
| Known-plaintext | Given random input/output pairs |
| Chosen-plaintext | Can query oracle with chosen inputs |
| Chosen-ciphertext | Can query with chosen ciphertexts |
| Related-key | Queries answered with related keys (K+1, K+2) |

**Certificational vs. practical attacks:** Differential cryptanalysis on DES needed 2^47 chosen plaintexts — scientifically huge, practically irrelevant (no system exposes that much text).

#### Public Key Encryption (Trapdoor One-Way Permutations)
- Key generation: `(K_R, K_R^-1)` — public encryption key, private decryption key
- `C = {M}_K_R` (anyone can encrypt)
- `M = {C}_K_R^-1` (only key owner can decrypt)
- In practice, randomized encryption for semantic security

#### Digital Signatures
- Private signing key `σ_R` → creates signatures
- Public verification key `V_R` → anyone can verify
- Usually: hash message first, then sign the hash (requires collision-resistant hash)
- **Message recovery** variant: signature itself contains the message (used for postal indicia/machine-printed stamps)

### 5.5 Symmetric Crypto Algorithms

#### SP-Networks (Substitution-Permutation)
Shannon's principles: **confusion** (mixing in unknown key values) + **diffusion** (spreading plaintext information through ciphertext).

Built from iterated rounds of:
- S-boxes (substitution, e.g., 4-bit→4-bit lookup tables)
- Linear transformation (permutation / mixing of bits)

**Design requirements:**
1. Wide enough blocks (64–128+ bits to prevent dictionary attacks)
2. Enough rounds (more than strictly needed, for safety margin)
3. Well-chosen S-boxes (tight differential and linear bounds)

#### Linear Cryptanalysis
Finds linear approximations across the cipher (e.g., "bit 2+5 of S-box input = bit 1+8 of output with probability 13/16"). If overall bias is `p = 0.5 + 1/M`, key recovery needs ~M² known texts.

#### Differential Cryptanalysis
Studies how input differences propagate to output differences. If an input change `δ_i` produces output change `δ_o` with unusual probability, key bits can be deduced. Need enough chosen plaintexts.

#### AES (Rijndael)
- **Block size:** 128 bits
- **Key sizes:** 128, 192, 256 bits (10, 12, 14 rounds)
- **Structure:** SP-network with single 8-bit S-box (defined over GF(2^8))
- **Linear transform:** Byte shuffle (ShiftRows) + column mix (MixColumns) — avalanche after just 2 rounds
- Best known attack: biclique cryptanalysis, complexity 2^126 (vs. 2^127 brute force) — **certificational only**
- NSA approved AES-128 for SECRET, AES-192/256 for TOP SECRET
- **Implementation is where real attacks live:** timing analysis (cache misses), power analysis (current draw)

#### Feistel Ciphers
Ladder structure: split block, apply round function to one half, XOR with other half, swap. **Decryption uses round functions in reverse order** — round functions don't need to be invertible.

**Luby-Rackoff result (1988):** With random round functions, 4-round Feistel is a pseudorandom permutation.

#### DES (Data Encryption Standard)
- **Block:** 64 bits | **Key:** 56 bits
- **Structure:** 16-round Feistel
- **Round function:** 32→48 bit expansion, XOR with round key, 8×6→4 bit S-boxes, permutation
- **Key length is now inadequate** — brute-forced in 3 days (EFF Deep Crack, 1998, $250K), 7 days on $10K FPGAs (2006), hours on a modern botnet
- Best shortcut: linear attack using 2^42 known texts (practically irrelevant)
- **Triple-DES:** `3DES(k0,k1,k2; M) = DES(k2; DES^-1(k1; DES(k0; M)))` — still serviceable for banking
- **DESX:** Whitening variant: `DESX(k0,k1,k2; M) = DES(k0; M⊕k1) ⊕ k2`

### 5.6 Modes of Operation

| Mode | Use | Pros | Cons |
|------|-----|------|------|
| **ECB** (Electronic Code Book) | Single-block only | Simple | Patterns visible; cut-and-splice attacks; **NEVER use for multi-block** |
| **CBC** (Cipher Block Chaining) | Legacy bulk encryption | Hides patterns; each block depends on all previous | Not parallelizable; padding oracle attacks; needs separate MAC |
| **Counter (CTR)** | Modern stream-like encryption | Parallelizable; guaranteed 2^n cycle length | No integrity protection; key+IV reuse is catastrophic |
| **OFB** (Output Feedback) | Legacy stream | — | Cycle length ~2^(n/2); not parallelizable |
| **CFB** (Cipher Feedback) | Legacy radio/jamming-resistant | Self-synchronizing | 1 block cipher op per bit; bad error amplification |
| **GCM** (Galois Counter Mode) | **Modern default** for authenticated encryption | Parallelizable; incremental; provides both encryption + authentication in one pass | Expands ciphertext (adds IV + tag) |
| **XTS** | Disk encryption | Length-preserving; whitened by sector number | Needs higher-layer integrity checks |
| **CBC-MAC / CMAC** | Message authentication only | Proves block cipher strength | CMAC fixes variable-length attack |

**ECB warning:** Encrypting redundant data with ECB leaves patterns fully visible. A corporate email system using DES-ECB had the null block as the most common ciphertext — trivially dictionary-attacked.

**CBC padding oracle attack (Vaudenay, 2002):** If server signals invalid padding, attacker tweaks ciphertext bytes one at a time, watches error messages, decrypts entire messages. Exploited against SSL, IPsec, TLS as late as 2016.

**GCM:** Encrypts in counter mode variant + computes polynomial-based authenticator tag. Uses universal hash function (provably secure if keys never reused). **The sensible default for authenticated encryption of bulk content.**

### 5.7 Hash Functions in Detail

#### Davies-Meyer Construction
Build a hash from a block cipher: feed message blocks as key input, XOR previous hash with output (feedforward makes it non-invertible).  
A 128-bit block cipher gives only 2^64 collision resistance — borderline. Use 256+ bit hashes.

#### HMAC (Hash-based Message Authentication Code)
```
HMAC_k(M) = h(k ⊕ B, h(k ⊕ A, M))
```
Two nested hashings with key variants (XOR'd with different constants). Prevents length-extension attacks on naive `h(k, M)`.

#### Hash Function Applications
- **Password storage:** One-way encryption (hash the password)
- **File integrity / forensics:** Checksums to verify evidence wasn't tampered with
- **Digital signatures:** Hash-then-sign (avoids signing large messages; needs collision resistance)
- **Commitment schemes:** Publish `h(document)` to establish priority date without revealing content (modern Hooke's anagram)
- **Timestamps:** Submit hash to blockchain for proof of existence

> **Legal reality:** In 2005, a Sydney motorist was acquitted of speeding because the Roads Authority couldn't find an expert to testify that MD5 was secure. Even non-exploitable vulnerabilities can have certificational consequences.

---

## Key Takeaways

### Protocols
1. **Don't design your own** — get a specialist and publish for peer review
2. Challenge-response is only as strong as the channel binding — **relay attacks are real**
3. Key diversification limits breach impact; global master keys are catastrophic
4. Timestamps (Kerberos) fix revocation but introduce clock sync risks
5. Formal verification helps, but won't catch implementation bugs or feature creep
6. Environmental changes (chip+PIN, keyless entry) can invalidate fundamental assumptions

### Cryptography
1. **Never use ECB mode** for multi-block encryption
2. **Never reuse keystream** with stream ciphers or counter mode (same key + same IV)
3. Use **authenticated encryption** (GCM) — encryption without integrity is broken
4. **128-bit symmetric keys** are the minimum; 256-bit preferred for long-term secrets
5. Use **SHA-2 (256-bit)** or **SHA-3** — MD5 and SHA-1 are broken for collision resistance
6. **AES is the standard** — trusted, well-analyzed, hardware-accelerated; don't use alternative ciphers "just in case"
7. Implementation attacks (timing, power analysis, padding oracles) matter more than mathematical breaks
8. Cryptographic defaults in libraries are often unsafe — understand what you're choosing
