---
tags: [cps-security, iot, scada, cyber-security, cybok]
---

# 19 — Cyber-Physical Systems Security

> *Source: CyBOK v1.1 by NCSC, Chapter 20 — Cyber-Physical Systems Security*

## Purpose

Cyber-Physical Systems (CPS) integrate computation, networking, and physical processes. Their security is uniquely challenging because attacks can cause physical harm — damaging equipment, disrupting critical infrastructure, or endangering human life.

## CPS Domains
- **Industrial Control Systems (ICS)** — SCADA, DCS, PLCs
- **IoT (Internet of Things)** — Smart homes, wearables, connected vehicles
- **Critical Infrastructure** — Power grid, water systems, transportation
- **Medical Devices** — Implantable devices, infusion pumps, imaging systems
- **Autonomous Vehicles** — Sensors, control systems, V2X communication

## CPS Threat Landscape
- **Safety-Security Interaction** — Attacks that manipulate physical processes to cause unsafe conditions
- **Real-Time Constraints** — Security mechanisms must not violate hard real-time deadlines
- **Resource Constraints** — Embedded devices with limited CPU, memory, power
- **Long Lifecycles** — Systems deployed for decades; must resist future attacks
- **Physical Access** — Devices often deployed in physically accessible locations

## Notable Incidents
- **Stuxnet (2010)** — Targeted attack on Iranian uranium centrifuges via PLC manipulation
- **Ukraine Power Grid (2015)** — Coordinated attack causing blackouts for 225,000 customers
- **TRITON/TRISIS (2017)** — Attack on safety instrumented systems at petrochemical plant
- **Colonial Pipeline (2021)** — Ransomware disrupting fuel distribution

## Security Approaches
- Defense-in-depth for CPS (IT/OT convergence)
- Network segmentation (Purdue model)
- Secure protocols for industrial environments (OPC UA, DNP3 Secure)
- Anomaly detection for physical processes
- Safety-integrated security (IEC 61508, IEC 62443)
- Secure firmware updates and supply chain integrity

## Related Chapters

- [[18_Hardware_Security]] — Embedded hardware security, side-channel attacks
- [[17_Network_Security]] — Network segmentation for OT/IT
- [[05_Malware_and_Attack_Technologies]] — ICS-specific malware
