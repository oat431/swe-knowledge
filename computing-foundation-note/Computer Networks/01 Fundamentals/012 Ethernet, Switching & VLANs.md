---
tags:
- networking
- programming
- protocols
- layer2
---

# 012 Ethernet, Switching & VLANs

Before IP routing works, frames must cross the local wire. Ethernet + switches + VLANs are Layer 2, the foundation every container network, office LAN and datacenter overlay stands on.

---

## The Ethernet Frame

```
| Preamble (7B) | SFD (1B) | Dst MAC (6B) | Src MAC (6B) | [802.1Q Tag 4B] | EtherType (2B) | Payload (46–1500B) | FCS (4B) |
```

| Field | Purpose |
|-------|---------|
| **Preamble + SFD** | Bit synchronization ;  `10101010...10101011` tells receivers "frame starts now" |
| **Dst/Src MAC** | 48-bit hardware addresses |
| **EtherType** | What's inside: `0x0800` IPv4, `0x86DD` IPv6, `0x0806` ARP, `0x8100` VLAN-tagged |
| **Payload** | 46–1500 bytes (padded if smaller) |
| **FCS** | CRC-32 checksum ;  corrupt frames are silently dropped, not retransmitted (L2 has no retry!) |

### MTU

| Type | Size | Where |
|------|------|-------|
| **Standard** | 1500 B | Internet default, every WAN path |
| **Jumbo** | 9000 B | Datacenter/storage backends ;  fewer frames, less CPU per byte |

Jumbo only helps if *every* hop supports it. Mixed MTU + no PMTUD awareness = silent blackholes. Tunnel overhead (VXLAN adds 50 B, IPsec more) is why overlay networks eat into your 1500.

---

## MAC Addresses

> 48 bits: **24-bit OUI** (vendor, e.g. `00:1A:2B` = Cisco) + 24-bit device serial. Universally unique, until someone runs a VM farm with cloned MACs.

| Address Type | Pattern | Example |
|--------------|---------|---------|
| **Unicast** | I/G bit = 0 | `00:1a:2b:3c:4d:5e` |
| **Multicast** | I/G bit = 1 | `01:00:5e:...` (IPv4 mcast) |
| **Broadcast** | All 1s | `ff:ff:ff:ff:ff:ff` ;  every device on the segment processes it |

ARP maps IP → MAC before any local delivery (recap in [[013 IP Addressing & Subnetting]]): broadcast "who has 10.0.0.5?", unicast reply "10.0.0.5 is at aa:bb:cc:dd:ee:ff".

---

## Hubs vs Switches vs Routers

| Device | Layer | Behavior | Domains |
|--------|-------|----------|---------|
| **Hub** | L1 (dumb) | Repeats every bit to every port | 1 collision domain, 1 broadcast domain |
| **Switch** | L2 | Learns MACs, forwards frame only to the right port | Per-port collision domains, 1 broadcast domain (per VLAN) |
| **Router** | L3 | Routes IP between networks, decrements TTL | Separate broadcast domains |

- **Collision domain:** set of devices competing for one wire. Switched Ethernet (full duplex) = collisions extinct.
- **Broadcast domain:** set of devices receiving each other's broadcasts. Routers (and VLANs) stop them.

---

## How a Switch Learns & Forwards

> **Learn from source, forward by destination, flood when clueless.**

1. **Learn:** every frame's *source MAC* + ingress port goes into the **MAC address table** (CAM/FDB)
2. **Forward:** known destination MAC → send only out that port
3. **Flood:** unknown unicast, multicast, broadcast → all ports except ingress
4. **Age:** entries expire (~300s default): stale entries get re-learned

```
Frame in (src AA, dst BB) on port 3:
  table[AA] = port 3                    ← learn
  if BB in table:  send to table[BB]    ← forward
  else:            send to ALL ports    ← flood (and hope BB replies so we learn it)
```

---

## STP / RSTP: Loop Prevention (brief)

> Redundant L2 links = switching loops = broadcast storms that melt the network in seconds. Spanning Tree blocks redundant paths, leaving one logical tree.

| Concept | Meaning |
|---------|---------|
| **Root bridge** | Elected switch (lowest bridge ID) ;  the tree's center |
| **Port states** | Blocking → Listening → Learning → Forwarding |
| **STP (802.1D)** | Original ;  up to 50s convergence. Painfully slow. |
| **RSTP (802.1w)** | Rapid version ;  converges in ~1–6s. Default on modern gear. |

If one VLAN's STP root is a closet switch and another's is the core, you have a design smell.

---

## VLANs: Virtual LANs

> One physical switch, multiple isolated broadcast domains. Traffic in VLAN 10 never sees VLAN 20 without a router (or L3 switch).

### 802.1Q Tagging

A 4-byte tag inserted after Src MAC carries a **12-bit VLAN ID (1–4094)** + priority bits (PCP, for QoS).

| Port Type | Behavior |
|-----------|----------|
| **Access port** | Belongs to ONE VLAN. Strips tag on egress (host sees plain Ethernet). Untagged. |
| **Trunk port** | Carries MANY VLANs between switches. Frames stay tagged. |
| **Native VLAN** | The one VLAN sent *untagged* over a trunk (default VLAN 1). Mismatched native VLANs between switches = classic silent leak/misforward. |

### Why Segment

| Reason | How VLANs Help |
|--------|---------------|
| **Broadcast control** | Broadcasts stay in-VLAN ;  a 500-host flat LAN drowns in ARP/NDP/mDNS |
| **Security** | Dev can't sniff Prod; guest WiFi can't reach servers |
| **Organization** | VLAN per function (10=servers, 20=office, 99=mgmt) maps cleanly to subnets |

Routing between VLANs needs a **router-on-a-stick** (subinterfaces) or, on modern gear, an **L3 switch** doing inter-VLAN routing at wire speed.

---

## VXLAN: VLANs at Cloud Scale (brief)

> VLANs cap at 4094 and don't cross L3 boundaries. **VXLAN** (RFC 7348) tunnels L2 Ethernet frames inside UDP (port 4789) over a normal IP fabric; **16M+ segments (VNI)**. This is how clouds give every tenant their own L2 network across racks.

- Overlay = VXLAN segments; underlay = plain IP routing (see [[015 Routing - BGP & OSPF]])
- VTEPs (VXLAN Tunnel Endpoints) encapsulate/de-encapsulate: on switches, hypervisors, or in-kernel on Linux (`ip link add vxlan0 type vxlan ...`)
- Same idea as VPN overlays: user-visible L2/L3 topology decoupled from physical wiring

---

## Link Aggregation: LACP / Bonding (brief)

> Bundle N physical links into one logical link: more bandwidth + failover. **LACP (802.3ad)** negotiates it between two switches; Linux **bonding** does it on hosts.

```bash
# Linux bond (mode 4 = 802.3ad/LACP)
ip link add bond0 type bond mode 802.3ad
ip link set eth1 master bond0
ip link set eth2 master bond0
cat /proc/net/bonding/bond0      # member state, LACP partner info
```

Gotcha: a single TCP flow hashes to ONE member link; aggregation adds total capacity, not per-flow speed.

---

## Practical Commands

```bash
ip link show                        # interfaces, MTU, state, MAC
ip -s link show eth0                # + RX/TX byte/packet/error counters
ip link set eth0 mtu 9000           # jumbo frames

bridge link show                    # bridge ports (Linux bridge)
bridge fdb show                     # MAC forwarding table (= switch's CAM)
bridge vlan show                    # VLAN membership per port

# Create a VLAN interface on iproute2
ip link add link eth0 name eth0.10 type vlan id 10
ip addr add 10.0.10.2/24 dev eth0.10

ip neigh show                       # ARP table
ip -s -d link show vxlan0           # VXLAN details
```

---

## Common L2 Failure Modes

| Failure | Symptom | Fix |
|---------|---------|-----|
| **Broadcast storm** | 100% switch CPU, everything slow, STP loop | Find the loop (STP misconfig / dual-homed host with bridged NICs). Storm control. |
| **ARP storm / flapping** | ARP cache churn, intermittent reachability | Duplicate IP! Check `ip neigh` for one IP ↔ two MACs. |
| **MAC table overflow** | Switch floods everything → perf cliff + sniffing exposure | MACsec/port-security limits; attacker with random MACs (macof) ;  enable sticky MAC / port security. |
| **Native VLAN mismatch** | Trunk up but cross-VLAN leaks / CDP warnings | Match native VLAN on both ends. Best practice: dedicated unused native VLAN. |
| **Asymmetric VLAN config** | Host reaches gateway but not peers; DHCP fails | VLAN allowed on trunk ≠ VLAN configured on access port ;  audit both ends. |
| **MTU mismatch** | SSH/ping works, large transfers stall | Ping with DF bit: `ping -M do -s 1472 <gw>`; align MTU end-to-end. |

---

## Sources

- IEEE 802.3: Ethernet
- IEEE 802.1Q: VLANs & Bridging
- IEEE 802.1w: RSTP
- RFC 7348: VXLAN
- Linux iproute2 man pages: `man bridge`, `man ip-link`
- *Ethernet: The Definitive Guide*: Spurgeon & Zimmermann, O'Reilly
