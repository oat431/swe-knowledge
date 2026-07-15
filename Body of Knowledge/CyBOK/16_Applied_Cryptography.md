---
tags: [cryptography, cyber-security, cybok]
---

# 16 — Applied Cryptography

> *Source: CyBOK v1.1 by NCSC, Chapter 17 — Applied Cryptography*

## Purpose

Cryptography provides the mathematical foundations for confidentiality, integrity, authentication, and non-repudiation. This KA covers the practical application of cryptographic primitives and protocols in security systems.

## Symmetric Cryptography
- **Block Ciphers** — AES, DES/3DES. Modes: ECB, CBC, CTR, GCM (authenticated encryption)
- **Stream Ciphers** — ChaCha20, RC4 (deprecated)
- **Message Authentication Codes (MAC)** — HMAC, CMAC, Poly1305

## Asymmetric (Public-Key) Cryptography
- **Encryption** — RSA, ElGamal, Elliptic Curve (ECIES)
- **Digital Signatures** — RSA-PSS, ECDSA, EdDSA (Ed25519)
- **Key Exchange** — Diffie-Hellman (DH), ECDH
- **Post-Quantum Cryptography** — Lattice-based, code-based, multivariate

## Hash Functions
- SHA-2 family (SHA-256, SHA-512), SHA-3
- Properties: collision resistance, preimage resistance, second preimage resistance
- Applications: password hashing (bcrypt, Argon2), Merkle trees, commitment schemes

## Public Key Infrastructure (PKI)
- X.509 certificates, Certificate Authorities, trust models
- Certificate validation (CRL, OCSP), certificate pinning
- Web PKI (TLS certificates via ACME / Let's Encrypt)

## Cryptographic Protocols
- **TLS/SSL** — Transport layer security, handshake, cipher suites, forward secrecy
- **SSH** — Secure remote access
- **IPsec** — Network layer security
- **Signal Protocol** — End-to-end encrypted messaging (Double Ratchet)

## Key Management
Key generation, distribution, storage, rotation, and destruction. Hardware Security Modules (HSMs), key derivation functions (KDFs).

## Related Chapters

- [[17_Network_Security]] — TLS, IPsec, SSH in network context
- [[13_Authentication_Authorisation_Accountability]] — Cryptography in authentication
- [[12_Formal_Methods_for_Security]] — Proving cryptographic protocol security
