---
tags:
  - security
  - networks
  - attack-defence
  - ddos
  - firewalls
  - software-security
source: "Anderson Security Engineering, Chapter 21"
created: 2026-07-21
---

# Network Attack and Defence

> "Simplicity is the ultimate sophistication." — Leonardo da Vinci

## 1. Introduction

Network security is practical engineering — much of it is one bodge piled on top of another. While developers building apps on cloud platforms (AWS, Azure) can delegate much of the worry, organisations with legacy systems, industrial control systems (DNP3, Modbus), or sensitive internal networks must pay close attention.

### The Perimeter Debate

Two opposing strategic trends define modern network security:

| Model | Philosophy | Champion |
|-------|-----------|----------|
| **Re-perimeterization** | Put protection at the single connection to the outside world (e.g., substations, vehicles with CANBUS) | Utilities, industrial control |
| **Deperimeterisation (Zero Trust)** | Abandon the perimeter; authenticate every user, device, and service individually | Google (BeyondCorp) |

**BeyondCorp** shifts access controls from the network perimeter to individual users and devices. Internal networks are unprivileged; each service has an Internet-facing access proxy. Requires:
- Tight authentication and authorisation of users and devices
- A device inventory service and access control engine
- Excellent HR data to tie staff/contractors to devices and permitted services

The pandemic-driven surge in home working has accelerated zero-trust adoption.

---

## 2. Network Protocols and Service Denial

### 2.1 BGP Security

**Border Gateway Protocol (BGP)** is the core routing protocol of the Internet, gluing together Autonomous Systems (ASes). It has no built-in mechanism to validate routing information.

**Key risks:**
- **Route hijacking**: Attacker advertises false routes; traffic gets redirected
- **Accidental leaks**: Pakistan's attempted YouTube censorship (2008) propagated globally
- **Intelligence collection**: Traffic routed via adversary countries (e.g., Canada→Korea via China, 2016)
- **Criminal misuse**: IP address space theft by spammers; $10M+ ad fraud
- **Strategic risk**: Concentration of Tier-1 providers reduces resilience

**Defences:**
- **RPKI** (Resource Public Key Infrastructure): Certifies "AS X announces IP range Y" — catches fat-finger mistakes, not capable attackers
- **Peerlock**: ASes at interchanges share route policies
- **Route limits**: Routers accept only a limited number of routes per peer

### 2.2 DNS Security

DNS maps human-readable names to IP addresses through a hierarchical, massively distributed system.

**Attack vectors:**
- **DNS hijacking**: Redirecting DNS queries for censorship, ad injection, or phishing
- **Pharming / drive-by pharming**: Changing a victim's DNS server (e.g., via malicious JavaScript on home router)
- **DDoS amplification**: DNSSEC-signed records are large — attackers send small queries spoofing the victim's IP, DNS servers bombard the victim

**Defences:**
- **DNSSEC**: Digital signatures on DNS records. Patchy uptake — many firms avoid it (fragility concerns, zone-walking exposure)
- **DoH (DNS-over-HTTPS)**: Encrypts DNS queries. Controversial — improves privacy but breaks enterprise DNS monitoring, threat intelligence feeds, and content filtering

### 2.3 TCP, UDP, and Amplification Attacks

**SYN Flood**: Attacker sends many SYN packets without acknowledging replies; victim exhausts state tracking half-open connections. Mitigated by **SYN cookies** — encrypting connection state into the SYN-ACK rather than storing it.

**SYN Reflection**: Attacker sends SYN packets spoofing the victim's IP; the responder (reflector) sends up to 5 ACKs per SYN, amplifying the attack.

**Other amplifiers**: Protocols with asymmetric responses have been exploited:
- **Smurfing** (ICMP broadcast) → mitigated by disabling broadcast replies
- **NTP, DNS** → UDP-based amplification
- **Countermeasure**: ISPs filter UDP packets with forged source addresses; Microsoft hardened the Windows network stack against spoofing

### 2.4 DDoS — Distributed Denial of Service

As clever amplification attacks were closed, attackers turned to brute-force floods from botnets.

**First DDoS**: Morris worm (1980s, accidental); first deliberate: Panix ISP (1996).

**Modern landscape:**
- **IoT botnets** (Mirai, 2016+): CCTV cameras, routers with known default passwords, un-patchable firmware
- **DDoS-for-hire**: Underground market; schoolkids targeting gamers, blackmail of online bookmakers, political suppression
- **Law enforcement response**: Raiding and arresting DDoS-for-hire operators

### 2.5 Email Security

SMTP provides neither encryption nor authentication by default.

**Bulk interception countermeasures:**
- **STARTTLS**: Opportunistic encryption between mail servers (boosted post-Snowden)
- **MTA-STS**: Mail providers publish policies requiring authenticated TLS
- **Webmail dominance**: ~95% of personal email at big 5 providers; confidentiality via TLS + certificate pinning

**Anti-spam mechanisms:**
1. **DKIM**: Cryptographically ties email to the sending domain
2. **SPF**: Ties email to source IP address
3. **DMARC**: Domain owner's policy for handling authentication failures
4. **Machine learning**: Filters using user spam reports as ground truth

Modern spam gangs steal IP space via malicious BGP, register thousands of domains, and send a few hundred spams from each before filters adapt.

---

## 3. Malware — Trojans, Worms, and RATs

### 3.1 Taxonomy

| Type | Definition |
|------|------------|
| **Trojan** | Program that does something malicious when run by an unsuspecting user |
| **Worm** | Self-replicating program that spreads to other systems |
| **Virus** | Malware that hooks itself into other programs' code |
| **Remote Access Trojan (RAT)** | Software enabling remote party to access/control a device |
| **Rootkit** | Software installed as root that stealthily enables third-party control |
| **Potentially Unwanted Software (PUS)** | Installed openly/by deception; does something user doesn't want |

### 3.2 Historical Evolution

- **1960s-70s**: University pranks; Trojans to create privileged backdoor accounts
- **1984**: Ken Thompson's "Reflections on Trusting Trust" — compiler-based backdoor that survives source code inspection
- **1981**: First computer virus in the wild (Apple II, 9th-grader)
- **1987**: "Christma" virus (IBM mainframes, BITNET) — prank, user-executed
- **1988**: **Morris Internet Worm** — exploited password guessing, fingerd bug, trusted-host relationships. Bug caused overload; ~10% of 60,000 Arpanet machines affected. Lesson: sites that kept their nerve and stayed connected recovered faster
- **1990s**: PC viruses → anti-virus industry; macro viruses (Word) become main vector
- **2000**: **Love Bug** — social engineering worm ("I love you" subject); taught that perimeter filtering fails when users have personal webmail
- **2001-2003**: **Flash worms** (Code Red, Slammer) — scanned entire Internet, infected all vulnerable machines in hours/minutes
- **2004-2006**: **Professionalisation** — underground markets, crime forums; malware for profit replaces amateur pranks
- **2007+**: **Storm botnet** — peer-to-peer architecture; pump-and-dump, pharmacy scams
- **2016+**: **Mirai** — IoT worm exploiting default passwords on un-patchable CCTV cameras; 1000+ variants

### 3.3 How Malware Works

Malware has two components:
1. **Dropper / replication mechanism** — how it spreads (worm self-copy, virus macro, Trojan user-execution)
2. **Payload** — what it does:
   - Exfiltrate confidential data
   - Banking malware / spyware
   - **Ransomware** (encrypt data, demand payment)
   - Attack others (e.g., GCHQ's Operation Socialist)
   - Mine cryptocurrency
   - Install rootkit/RAT for persistent control

**Lateral movement**: Once inside a network, attackers:
- Install packet sniffers to harvest passwords → blocked by 2FA, Kerberos, SSH
- Exploit shared resources (NFS file handles, Windows file shares like EternalBlue)
- Abuse SSH key trust structures spanning thousands of machines

### 3.4 Countermeasures

**Antivirus evolution:**
- **Scanners**: Search for byte-string signatures (IoC). Countered by **polymorphism** — re-encrypting code with different keys, tweaking decryption stubs
- **Checksummers**: Whitelist of authorised executables. Countered by **stealth** — malware hides during OS calls
- **Modern**: Run code in VMs to observe unpacking; but malware detects VMs

**The effectiveness decline**: AV detected ~100% of exploits in early 2000s, ~30% by 2010, and by 2020 you expect to detect infection after the fact and clean up. Competent attackers "live off the land" — add their SSH key to authorised_keys, leaving no malware files for legacy AV.

---

## 4. Defence Against Network Attack

A modern large Windows shop typically deploys:
1. **Endpoint agents** — visibility of all software, push updates
2. **Vulnerability scanners** — continuous network probing
3. **Boundary control** — firewalls, web proxies, application proxies
4. **SSL gateway** — for remote workers
5. **BYOD manager** — control staff-owned devices
6. **DLP (Data Leakage Prevention)** — detect document/code exfiltration
7. **Threat intelligence platform** — feeds of IoCs (bad domains, IPs)
8. **Log analysis / SIEM** — trace compromises back to origin
9. **SOAR (Security Orchestration and Response)** — automate incident response

Integration is critical: without it, dozens of staff copy lists of bad domains and IPs between tools.

### 4.1 Filtering: Firewalls

Firewalls examine packet streams and filter/log. Three levels:

#### Packet Filtering
- Inspects IP addresses and port numbers
- Blocks IP spoofing, known-bad addresses
- Used in the Great Firewall of China at router hardware speed
- **Limitations**: Fragment overwriting attacks; fast-flux DNS defeats IP blacklisting

#### Circuit Gateways
- Reassembles and inspects all packets per TCP session
- Provides VPN functionality (IPsec)
- Can do DNS filtering; directs specific traffic to application filters

#### Application Proxies
- Understand specific services (mail filters, web proxies)
- Strip executables, active content, macros
- **Bottleneck risk**: As services adopt HTTPS and certificate transparency, proxying becomes harder; filtering shifts to endpoints

#### Ingress vs. Egress Filtering
- **Ingress**: Keep bad things out (traditional firewall)
- **Egress**: Stop bad things leaving (military mail guards, ISP source-address validation, DLP)
- Source address validation by ISPs has forced DDoS operators to rent servers in data centres instead of using botnets

#### Architecture Considerations

| Principle | Detail |
|-----------|--------|
| **Simplicity** | Firewalls do few things — reduce vulnerability surface |
| **Usability** | Overly restrictive firewalls → users install backdoors (cable modems bypassing perimeter) |
| **Deperimeterisation** | Phones, PDAs, outsourced functions, IoT make perimeter hard to define |
| **Underblocking vs. Overblocking** | ROC curve trade-off; censorship systems face legal risks either way |
| **Incentives** | Departmental sysadmin who knows 100 users is more motivated than corporate team managing thousands |

### 4.2 Intrusion Detection (IDS)

Systems that monitor for signs of attack in progress or compromised machines.

**Examples:**
- Machine contacting known-bad IP/DNS (C2 server)
- Packets with forged source addresses from inside a subnet
- Spam originating from internal machines
- Payment fraud detection, insider trading patterns

#### Types of IDS

**Misuse Detection**: Model of known attacker behaviour.
- Signature-based (e.g., malware byte strings, known C2 IPs)
- Machine learning classifiers on multiple signals (like card fraud systems)
- Requires low false-alarm rates at scale

**Anomaly Detection**: Look for anomalous behaviour without a clear attacker model. Harder problem; Google policy avoids systems that learn thresholds, preferring simple change-detection.

**Honeypots**: Emulated vulnerable devices to attract attackers, observe techniques, and gather IoCs.

#### Limitations of IDS

1. **Halting problem**: Cohen proved detecting whether a program will do something bad is equivalent to the halting problem — no complete solution
2. **Noisy Internet environment**: Random crud, software bugs, stale DNS → false alarms
3. **Base rate fallacy**: If 10 real attacks per million sessions, even 0.1% false alarm rate = 100 false alarms per real one
4. **Stateful vs. stateless attacks**: Data exfiltration leaves no incorrect state — harder to detect than theft
5. **Encryption**: As HTTPS and DoH become universal, content analysis becomes ineffective
6. **Stealthy attacks**: 1-2 packets/day to 100,000 hosts — need central packet counting

**Modern approach**: Integration and automation. SIEM + SOAR + metrics. Coordinating dozens of products across network and endpoints.

---

## 5. Cryptography at the Boundary

### 5.1 SSH

Secure Shell (1995) provides encrypted, authenticated links between hosts.

**Standard mode**: Each machine has a public-private keypair. User's private key protected by passphrase. Diffie-Hellman key exchange, keys sign transient public keys to prevent MITM.

**Problems:**
- Character-by-character typing leaks inter-arrival timing information
- **Most server-to-server SSH keys stored in the clear** (no passphrase) → one compromised server compromises all machines trusting its key
- IoT devices with default/weak SSH passwords → recruited into Mirai-style botnets

### 5.2 Wireless at the Periphery

#### WiFi (WPA2)
- WEP (original, 1997): Broken — weak export-grade ciphers, poor protocol design
- WPA2 (2004+): AES encryption; key on a card in the router
- **Usability design**: Key on card lets householder choose security level — pin on wall (open) or lock up (restricted)
- WiFi passwords are more about preventing bandwidth theft than eavesdropping

---

## 6. Key Takeaways

1. **Network security is a layered ecosystem** — no single defence suffices; BGP, DNS, TCP, and application layers each have distinct threats and defences.

2. **The perimeter is dissolving** — mobile, cloud, IoT, and home working make perimeters hard to define. Zero-trust architectures authenticate everything individually.

3. **Malware is now a professional industry** — underground markets, exploit kits, DDoS-for-hire, and ransomware-as-a-service. Signature-based AV alone is insufficient.

4. **DDoS has evolved** — from SYN floods to IoT botnets (Mirai). Amplification attacks closed off by ISP source-address validation and OS hardening, but brute-force floods from millions of IoT devices persist.

5. **Intrusion detection is hard** — false alarm rates, base rate fallacy, encryption, and stealthy attacks mean you must detect after the fact and clean up. Integration of SIEM/SOAR is critical.

6. **Cryptography is necessary but insufficient** — SSH, TLS, IPsec, WiFi encryption all help, but implementation weaknesses (cleartext SSH keys, default passwords, un-patchable IoT) undermine them.

7. **Usability and incentives matter** — overly restrictive defences cause workarounds; sysadmins with local accountability perform better than centralised teams; victim-blaming is maladaptive.
