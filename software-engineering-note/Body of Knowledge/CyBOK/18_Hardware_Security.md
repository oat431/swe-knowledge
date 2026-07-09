---
tags: [hardware-security, cyber-security, cybok]
---

# 18 — Hardware Security

> *Source: CyBOK v1.1 by NCSC, Chapter 19 — Hardware Security*

## Purpose

Hardware security addresses threats and defenses at the physical and hardware level — the foundation upon which all software security depends. If hardware is compromised, no software-level security can be trusted.

## Trusted Execution Environments
- **TPM** (Trusted Platform Module) — Secure key storage, platform integrity measurement, remote attestation
- **HSM** (Hardware Security Module) — Dedicated crypto processor for key management and operations
- **Secure Enclaves** — Intel SGX, AMD SEV, ARM TrustZone — isolated execution environments
- **Secure Boot** — Verifying firmware and OS integrity at boot time (UEFI Secure Boot)

## Side-Channel Attacks
Exploiting physical implementation characteristics rather than algorithmic weaknesses:
- **Timing attacks** — Measuring computation time to infer secrets
- **Power analysis** — SPA/DPA — correlating power consumption with data
- **Electromagnetic emanations** — TEMPEST, EM side-channels
- **Cache-based attacks** — Spectre, Meltdown — exploiting CPU speculative execution
- **Acoustic / thermal** — Sound or heat patterns revealing computation

## Hardware Trojans and Supply Chain
- Malicious modifications to ICs during design or fabrication
- Counterfeit components
- Supply chain integrity verification

## Physical Attacks
- **Invasive** — Decapsulation, microprobing, FIB (Focused Ion Beam) editing
- **Semi-invasive** — UV/optical attacks, fault injection (voltage, clock glitching)
- **Non-invasive** — JTAG/SWD debugging, side-channel analysis

## Mitigations
- Tamper-resistant and tamper-evident packaging
- Physically Unclonable Functions (PUFs)
- Constant-time implementations
- Masking and hiding countermeasures against side-channels
- Hardware security modules for key protection

## Related Chapters

- [[16_Applied_Cryptography]] — Cryptographic hardware and HSMs
- [[19_Cyber_Physical_Systems_Security]] — CPS/IoT hardware security
- [[20_Physical_Layer_and_Telecommunications_Security]] — Physical layer attacks
