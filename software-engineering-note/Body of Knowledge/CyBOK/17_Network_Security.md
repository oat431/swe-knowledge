---
tags: [network-security, cyber-security, cybok]
---

# 17 — Network Security

> *Source: CyBOK v1.1 by NCSC, Chapter 18 — Network Security*

## Purpose

Network security protects the confidentiality, integrity, and availability of data in transit and the network infrastructure itself. This KA covers attacks, defenses, and protocols at all network layers.

## Network Attacks
- **Reconnaissance** — Port scanning, OS fingerprinting, network mapping
- **Eavesdropping** — Packet sniffing, man-in-the-middle (MITM)
- **Denial of Service (DoS/DDoS)** — Volumetric (SYN flood, UDP flood, amplification), protocol, application layer
- **Spoofing** — IP, ARP, DNS spoofing/poisoning
- **Session Hijacking** — TCP session hijacking, cookie theft

## Network Defenses

### Perimeter Security
- **Firewalls** — Packet filtering, stateful inspection, application-layer (next-gen), WAF
- **IDS/IPS** — Signature-based, anomaly-based, network-based vs host-based
- **Network Segmentation** — VLANs, DMZ, micro-segmentation

### Encryption at Network Layers
- **Link Layer** — WPA3 (Wi-Fi), MACsec
- **Network Layer** — IPsec (AH, ESP, tunnel/transport modes)
- **Transport Layer** — TLS/SSL, DTLS
- **Application Layer** — SSH, HTTPS, VPNs

### Zero Trust Networking
- Never trust, always verify — every access request is authenticated and authorized
- Micro-segmentation of network resources
- Software-Defined Perimeter (SDP)
- BeyondCorp / Zero Trust Architecture

## Related Chapters

- [[16_Applied_Cryptography]] — TLS, IPsec, VPN protocols
- [[07_Security_Operations_and_Incident_Management]] — Network monitoring and incident response
- [[11_Distributed_Systems_Security]] — Distributed denial of service
