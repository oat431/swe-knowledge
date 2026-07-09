---
tags: [physical-security, telecommunications, cyber-security, cybok]
---

# 20 — Physical Layer and Telecommunications Security

> *Source: CyBOK v1.1 by NCSC, Chapter 21 — Physical Layer & Telecommunications Security*

## Purpose

Physical layer security addresses threats that operate below the traditional network stack — at the level of electromagnetic signals, physical media, and telecommunications infrastructure. It covers the intersection of physical security, signal processing, and communications engineering.

## Physical Layer Attacks
- **Jamming** — Intentional interference disrupting wireless communications
- **Eavesdropping** — Intercepting electromagnetic emanations (TEMPEST)
- **Traffic Analysis** — Inferring information from communication patterns even when content is encrypted
- **Signal Injection** — Injecting malicious signals into physical media
- **Relay/Replay Attacks** — Capturing and retransmitting signals (car key fob attacks)

## Telecommunications Security
- **SS7 Vulnerabilities** — Exploiting legacy signaling protocols for location tracking, call interception
- **Diameter Protocol** — Successor to SS7 in 4G/LTE with improved but imperfect security
- **5G Security** — New architecture with improved authentication, encryption, and network slicing isolation
- **VoIP Security** — SIP vulnerabilities, toll fraud, eavesdropping
- **Satellite Communications** — Uplink/downlink security, ground station vulnerabilities

## Physical Defenses
- Spread spectrum techniques (frequency hopping, direct sequence)
- Beamforming and directional antennas
- Physical unclonable functions (PUFs) for device authentication
- Electromagnetic shielding (Faraday cages, TEMPEST standards)
- Physical layer authentication (channel-based, RF fingerprinting)

## Related Chapters

- [[17_Network_Security]] — Upper-layer network security
- [[18_Hardware_Security]] — Physical and side-channel attacks
- [[19_Cyber_Physical_Systems_Security]] — Wireless sensor and industrial wireless security
