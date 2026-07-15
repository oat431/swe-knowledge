---
tags:
- networking
- programming
- protocols
---

# 01 OSI & TCP/IP Models

The OSI model is a reference. The TCP/IP model is reality. Both describe how data moves from an application on one machine to an application on another.

---

## The OSI 7-Layer Model

```
┌──────────────────────────────────┐
│ 7. Application   │ HTTP, DNS, SMTP, FTP  │ What the user sees
├──────────────────────────────────┤
│ 6. Presentation  │ TLS/SSL, JPEG, ASCII  │ Translation, encryption
├──────────────────────────────────┤
│ 5. Session       │ NetBIOS, RPC, Sockets  │ Connection management
├──────────────────────────────────┤
│ 4. Transport     │ TCP, UDP              │ Reliable delivery (or not)
├──────────────────────────────────┤
│ 3. Network       │ IP, ICMP, OSPF        │ Routing, addressing
├──────────────────────────────────┤
│ 2. Data Link     │ Ethernet, MAC, ARP    │ Hop-to-hop delivery
├──────────────────────────────────┤
│ 1. Physical      │ Cables, radio, fiber  │ Bits on the wire
└──────────────────────────────────┘
```

### Mnemonic (Top to Bottom)
> **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing
> Application → Presentation → Session → Transport → Network → Data Link → Physical

---

## Encapsulation — How Data Flows

```mermaid
graph TD
    A["<b>Application</b><br><i>HTTP Request</i><br>GET /api/users HTTP/1.1"] -->|"+ TCP Header"| T["<b>Transport</b><br><i>TCP Segment</i><br>+ source/dest port, seq number"]
    T -->|"+ IP Header"| N["<b>Network</b><br><i>IP Packet</i><br>+ source/dest IP"]
    N -->|"+ Ethernet Frame"| D["<b>Data Link</b><br><i>Ethernet Frame</i><br>+ source/dest MAC"]
    D -->|"Bits on wire"| P["<b>Physical</b><br>101011100010..."]
```

> **Encapsulation:** Each layer wraps the data from the layer above in its own header. At the destination, each layer strips its header and passes the remainder up.

---

## TCP/IP Model — What Actually Runs the Internet

```mermaid
graph LR
    subgraph "TCP/IP Model"
        A["<b>Application</b><br>HTTP, DNS, SMTP, SSH"]
        T["<b>Transport</b><br>TCP, UDP"]
        I["<b>Internet</b><br>IP, ICMP"]
        N["<b>Network Access</b><br>Ethernet, Wi-Fi"]
    end
    subgraph "OSI Model"
        OA["OSI 5-7"]
        OT["OSI 4"]
        OI["OSI 3"]
        ON["OSI 1-2"]
    end
    A --- OA
    T --- OT
    I --- OI
    N --- ON
```

---

## What Happens at Each Layer — Backend Developer's View

| Layer | What You Debug | Tool |
|-------|---------------|------|
| **Application** | HTTP errors (4xx, 5xx), DNS resolution | `curl`, `dig`, browser DevTools |
| **Transport** | Connection refused, timeouts, port already in use | `netstat`, `ss`, `telnet` |
| **Network** | Routing issues, unreachable hosts | `ping`, `traceroute`, `ip route` |
| **Data Link** | ARP issues, MAC addresses | `arp -a`, `ip link` |
| **Physical** | Cable unplugged, Wi-Fi down | ...look at the cable |

---

## Key Mental Models

| Concept | What It Means |
|---------|--------------|
| **Hop-by-hop** (Layers 1-3) | Each router along the path makes independent forwarding decisions |
| **End-to-end** (Layer 4) | TCP connection is between the two endpoints. Routers don't track it. |
| **Connection-oriented vs connectionless** | TCP = connection (handshake, state). UDP = connectionless (fire and forget). |
| **Reliable vs unreliable** | TCP guarantees delivery. UDP doesn't. HTTP/3 uses QUIC (UDP-based, reliable via app layer). |

---

## Sources

- ISO/IEC 7498-1:1994 — OSI Basic Reference Model
- RFC 1122 — Requirements for Internet Hosts
- Kurose & Ross. *Computer Networking: A Top-Down Approach.*
