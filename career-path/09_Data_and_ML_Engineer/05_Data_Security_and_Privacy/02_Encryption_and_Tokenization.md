---
title: "Encryption and Tokenization"
note_type: capability-topic
capability_area: data-security-and-privacy
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
  - "[[01_Data_Classification_and_Sensitivity]]"
tags:
  - career-path
  - data-engineering
  - encryption
  - tokenization
  - key-management
---

# Encryption and Tokenization

> Transforming data so that unauthorized parties cannot read it, while authorized systems can process it efficiently.

## Why This Is a Senior Skill

Mid-level engineers enable encryption when a checkbox exists. Senior engineers choose between encryption and tokenization based on query patterns, design key rotation strategies that do not break running pipelines, and understand the performance cost of each approach at scale.

The senior question is not "should we encrypt?" but "at what granularity, with what key lifecycle, and what is the query-path cost?"

## Core Frameworks

### Encryption Scope Decision Matrix

| Scope | Use Case | Performance Impact | Query Support |
|-------|----------|-------------------|---------------|
| Full-disk | Storage-level protection | Minimal | Full: database unaware |
| Table-level | Bulk protection of sensitive tables | Low | Full |
| Column-level | Specific PII columns | Moderate | Limited: no indexing on ciphertext |
| Field-level with searchable encryption | Encrypted but searchable | High | Equality search only |
| Application-level | Data encrypted before reaching DB | Highest | Application manages decryption |

### Encryption vs Tokenization

| Dimension | Encryption | Tokenization |
|-----------|-----------|--------------|
| Reversibility | Reversible with key | Reversible via lookup table |
| Format preservation | No unless format-preserving encryption | Yes: token matches original format |
| Query support | Limited on ciphertext | Full: query the token |
| Key management | Cryptographic keys, rotation needed | Token vault, vault availability critical |
| Performance | CPU-intensive for strong algorithms | Lookup latency, usually lower CPU |
| Best for | Bulk data, data at rest | Payment cards, SSNs, high-query fields |
| Compliance fit | GDPR, HIPAA general requirement | PCI-DSS cardholder data |

### Key Management Lifecycle

| Phase | Decision | Risk if Wrong |
|-------|----------|---------------|
| Generation | HSM vs software, key length, algorithm | Weak keys, compliance failure |
| Distribution | Key wrapping, key encryption keys | Interception during transit |
| Storage | HSM, cloud KMS, envelope encryption | Key theft, single point of failure |
| Rotation | Automated schedule, dual-key period | Broken pipelines, data inaccessibility |
| Revocation | Immediate vs scheduled, re-encryption | Continued access by compromised key |
| Destruction | Cryptographic erasure, secure wipe | Data recovery from decommissioned media |

## In Practice

**Use envelope encryption for data lakes.** A data encryption key encrypts the data. A key encryption key from your KMS encrypts the DEK. This lets you rotate the KEK without re-encrypting terabytes of data.

**Tokenize high-query sensitive fields.** A credit card number appears in thousands of queries per second. Encrypting it means decrypting on every query. Tokenizing preserves the format and allows indexed lookups while the real value sits in the token vault.

**Plan key rotation from day one.** Key rotation breaks things: cached connections, materialized views, ETL jobs holding old keys. Design rotation as a dual-key window where both old and new keys are valid for a defined period.

**Measure the performance cost.** Column-level encryption on a 100-million-row table can add 2-5 seconds to full-table scans. Benchmark before committing to a scope. Sometimes the right answer is table-level encryption with strict access control rather than column-level encryption.

## Practical Exercise

Take a table with at least one PII column. Design a protection strategy:
1. Classify each column using the tier model from [[01_Data_Classification_and_Sensitivity]]
2. For each Restricted column, decide: encryption or tokenization? Justify based on query patterns
3. Sketch the key management flow: where keys live, how rotation works, what breaks during rotation
4. Estimate the performance impact: how many queries per second touch the protected column?
5. Write a one-page proposal for your team lead with your recommendation and trade-offs

## Knowledge Connections

- [[05_Data_Security]]: DMBOK encryption guidance
- [[CyBOK v1 - Overview]]: cryptography knowledge area
- [[01_Data_Classification_and_Sensitivity]]: classification drives encryption scope
- [[03_Access_Control_for_Data]]: encryption complements access control
- [[06_Secure_Data_Sharing]]: encryption for cross-organization data

## Common Pitfalls

- Encrypting at the wrong granularity: full-disk encryption does not protect against SQL injection
- Forgetting key rotation planning: rotating keys without a dual-key window breaks running pipelines
- Treating the token vault as an afterthought: if the vault goes down, no tokenized data can be resolved
- Ignoring format-preserving encryption: replacing a 16-digit card number with a 44-character ciphertext breaks downstream schemas

## Key Takeaways

- Encryption scope is a trade-off between security and query performance: full-disk is cheap but coarse, column-level is precise but costly
- Tokenization preserves format and query support, making it better for high-access sensitive fields
- Envelope encryption separates data keys from key-encryption keys, enabling rotation without re-encryption
- Key rotation must be designed as a dual-key window, not a cutover event
- Senior engineers benchmark encryption performance before committing to a scope
- The token vault is a critical dependency: its availability and security determine the safety of all tokenized data
