---
tags:
- networking
- overview
- programming
- protocols
---

# 000 Computer Networks — Overview

Networks are the backbone of every distributed system. Every backend developer needs to understand what happens between "the request left the browser" and "the database returned the result." This vault covers the essentials.

---

## Topics

> **File naming:** `[chapter][order]` — the first digit is the folder/chapter, the next two are the reading order inside it. `000` = this overview. Read top to bottom.

### 01 Fundamentals — the stack, bottom to top

> [[011 OSI & TCP-IP Models]] — 7 layers, encapsulation, TCP/IP stack, what happens at each layer *(start here — the map for everything else)*
> [[012 Ethernet, Switching & VLANs]] — Frames, MAC learning, switches vs routers, 802.1Q, VXLAN, LACP *(Layer 2)*
> [[013 IP Addressing & Subnetting]] — IPv4, IPv6, CIDR, subnet masks, NAT, private/public addresses *(Layer 3 addressing)*
> [[014 DNS Deep Dive]] — Resolution, record types, TTL, DNSSEC, troubleshooting *(naming on top of IP)*
> [[015 Routing - BGP & OSPF]] — RIB/FIB, link-state vs path-vector, OSPF areas, BGP attributes, hijacks & RPKI *(how packets actually find the path)*

### 02 Protocols — what runs on the stack

> [[021 TCP & UDP]] — 3-way handshake, flow control, congestion, when to use UDP *(transport layer first)*
> [[022 HTTP & HTTPS]] — Methods, status codes, headers, TLS handshake, HTTP/2, HTTP/3
> [[023 TLS & PKI Deep Dive]] — TLS 1.2/1.3 handshakes, X.509, chain of trust, ACME/Let's Encrypt, mTLS, revocation *(the security under HTTPS)*
> [[024 Supporting Protocols]] — SMTP, POP3, IMAP, FTP/SFTP, DHCP, SNMP
> [[025 Wireless & Mobile]] — WiFi 802.11, Bluetooth, cellular 1G–5G, Mobile IP, WPA3 *(access-layer special case)*

### 03 Infrastructure — building and defending real networks

> [[031 Network Programming & Sockets]] — Socket API, epoll, connection pooling, TIME_WAIT/CLOSE_WAIT at scale, SO_REUSEPORT *(code against the stack)*
> [[032 Load Balancing & Proxies]] — L4 vs L7, NGINX, HAProxy, reverse proxy, CDN
> [[033 Realtime Protocols - WebSockets, SSE & gRPC]] — Server push options compared, gRPC streaming modes, NAT idle timeouts
> [[034 VPN & Overlay Networks]] — IPSec, WireGuard, Tailscale, NAT traversal, SD-WAN, VXLAN overlays
> [[035 Network Security]] — Firewalls, WAF, DDoS, VPN, mTLS, zero-trust *(needs the VPN/PKI context above)*
> [[036 Troubleshooting Tools]] — ping, traceroute, netstat, tcpdump, Wireshark, curl, dig *(read last — debugs everything above)*

---

## The Request Journey

```
Browser → DNS lookup → TCP handshake → TLS handshake → HTTP request
  → Load Balancer → Reverse Proxy → App Server → Database
  → Response flows back through each layer
```

Every step in this journey has a protocol, a port, and potential failure modes. Understanding each one makes you a better backend engineer.

---

## Quick Reference

| Protocol | Port | Layer | Purpose |
|----------|:----:|:-----:|---------|
| HTTP | 80 | Application | Web traffic |
| HTTPS | 443 | Application | Secure web traffic |
| DNS | 53 | Application | Name resolution |
| SSH | 22 | Application | Secure shell |
| SMTP | 25 | Application | Email sending |
| FTP | 20/21 | Application | File transfer |
| MySQL | 3306 | Application* | Database |
| PostgreSQL | 5432 | Application* | Database |
| Redis | 6379 | Application* | Cache |

---

## Sources

- SWEBOK v4 — Section 7: Computer Networks and Communications
- Kurose & Ross. *Computer Networking: A Top-Down Approach.*
