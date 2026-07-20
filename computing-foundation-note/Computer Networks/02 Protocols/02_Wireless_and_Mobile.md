---
tags:
  - wireless
  - mobile
  - networking
  - swebok
  - cellular
  - wifi
source: "Wireless Communications & Networks — Stallings; Wireless Communications: Principles and Practice — Rappaport"
---

# Wireless and Mobile Networks

> **Source:** *Wireless Communications & Networks* by Stallings; *Wireless Communications: Principles and Practice* by Rappaport

---

## 1. Wireless Network Types

| Type | Acronym | Range | Examples |
|------|---------|-------|----------|
| **Wireless Personal Area Network** | WPAN | ~10 m | Bluetooth, Zigbee, NFC |
| **Wireless Local Area Network** | WLAN | ~100 m | Wi-Fi (802.11) |
| **Wireless Metropolitan Area Network** | WMAN | ~5-50 km | WiMAX (802.16) |
| **Wireless Wide Area Network** | WWAN | ~100+ km | Cellular (3G/4G/5G) |

---

## 2. Wireless Standards

### 2.1 Wi-Fi (IEEE 802.11 Family)

| Standard | Name | Freq Band | Max Data Rate | Modulation | Channel Width | Year |
|----------|------|-----------|---------------|------------|---------------|------|
| **802.11** | -- | 2.4 GHz | 2 Mbps | FHSS/DSSS | 22 MHz | 1997 |
| **802.11a** | Wi-Fi 2 | 5 GHz | 54 Mbps | OFDM | 20 MHz | 1999 |
| **802.11b** | Wi-Fi 1 | 2.4 GHz | 11 Mbps | DSSS (CCK) | 22 MHz | 1999 |
| **802.11g** | Wi-Fi 3 | 2.4 GHz | 54 Mbps | OFDM | 20 MHz | 2003 |
| **802.11n** | Wi-Fi 4 | 2.4/5 GHz | 600 Mbps | OFDM + MIMO | 20/40 MHz | 2009 |
| **802.11ac** | Wi-Fi 5 | 5 GHz | 6.93 Gbps | OFDM + MU-MIMO | 20/40/80/160 MHz | 2013 |
| **802.11ax** | Wi-Fi 6/6E | 2.4/5/6 GHz | 9.6 Gbps | OFDMA + MU-MIMO | 20/40/80/160 MHz | 2020 |
| **802.11be** | Wi-Fi 7 | 2.4/5/6 GHz | 46 Gbps | OFDMA + 4096-QAM | Up to 320 MHz | 2024 |

**Key Wi-Fi Concepts:**
- **DSSS** (Direct Sequence Spread Spectrum): spreads signal over wider bandwidth using chipping code
- **OFDM** (Orthogonal Frequency Division Multiplexing): divides channel into many narrow subcarriers
- **MIMO** (Multiple Input Multiple Output): uses multiple antennas for spatial multiplexing
- **MU-MIMO**: allows simultaneous communication with multiple clients
- **OFDMA**: extends OFDM to allocate subsets of subcarriers to different users simultaneously
- **CSMA/CA** (Carrier Sense Multiple Access with Collision Avoidance): the MAC protocol for Wi-Fi

### 2.2 Bluetooth (IEEE 802.15.1)

| Version | Data Rate | Range | Year | Key Feature |
|---------|-----------|-------|------|-------------|
| 1.0 | 1 Mbps | ~10 m | 1999 | Initial standard |
| 2.0 + EDR | 3 Mbps | ~10 m | 2004 | Enhanced Data Rate |
| 4.0 (BLE) | 1 Mbps | ~100 m | 2010 | Bluetooth Low Energy |
| 5.0 | 2 Mbps | ~240 m | 2016 | Extended range, higher speed |
| 5.3 | 2 Mbps | ~240 m | 2021 | LE Audio, channel classification |

**Bluetooth Architecture:**
- **Piconet:** master + up to 7 active slaves (255 parked)
- **Scatternet:** multiple interconnected piconets
- **Frequency Hopping:** 79 channels (1 MHz each), 1600 hops/sec

### 2.3 Zigbee (IEEE 802.15.4)

| Feature | Specification |
|---------|--------------|
| Frequency | 868 MHz (EU), 915 MHz (US), 2.4 GHz (global) |
| Data Rate | 250 kbps (2.4 GHz) |
| Range | 10-100 m (typical ~30 m indoor) |
| Topology | Star, tree, mesh |
| Max nodes | ~65,000 per network |
| Power | Very low — battery life of years |

**Use Cases:** Home automation, industrial control, sensor networks, smart energy.

### 2.4 NFC (Near Field Communication)

| Feature | Specification |
|---------|--------------|
| Frequency | 13.56 MHz |
| Data Rate | 106, 212, 424 kbps |
| Range | ~4 cm (near contact) |
| Modes | Reader/Writer, Peer-to-Peer, Card Emulation |

**NFC vs Bluetooth:** NFC is slower, shorter range, but requires no pairing — instant "tap" connection.

---

## 3. Cellular Generations (1G-5G)

| Generation | Technology | Data Rate | Switching | Key Feature | Era |
|------------|-----------|-----------|-----------|-------------|-----|
| **1G** | AMPS, TACS | ~2.4 kbps | Analog circuit | Voice only, analog | 1980s |
| **2G** | GSM, IS-95 | 9.6-14.4 kbps | Digital circuit | SMS, encryption, digital | 1990s |
| **2.5G** | GPRS | 56-114 kbps | Packet | Always-on data | 2000 |
| **3G** | UMTS (W-CDMA) | 384 kbps - 2 Mbps | Packet | Video calling, mobile internet | 2001 |
| **3.5G** | HSPA/HSPA+ | 5.76-42 Mbps | Packet | High-speed packet access | 2006 |
| **4G** | LTE / LTE-Advanced | 100 Mbps - 1 Gbps | All-IP | VoIP, HD streaming | 2010 |
| **5G** | NR (New Radio) | 1-20 Gbps | All-IP + SBA | Ultra-low latency (<1ms), massive IoT | 2020 |

**Key Cellular Concepts:**
- **Cell:** basic geographic unit served by a base station
- **Frequency Reuse:** same frequencies used in non-adjacent cells
- **Handoff/Handover:** transferring active call/data session between cells
  - *Hard handoff:* break-before-make
  - *Soft handoff:* make-before-break (CDMA)
  - *Vertical handoff:* between different network types (Wi-Fi <-> cellular)

**5G Key Technologies:**
- **mmWave** (24-100 GHz): ultra-wide bandwidths, short range
- **Massive MIMO:** 64-256 antenna elements at base station
- **Beamforming:** focused energy transmission toward specific users
- **Network Slicing:** virtual networks on shared physical infrastructure
- **Edge Computing (MEC):** compute at network edge for low latency

---

## 4. Multiple Access Techniques

| Technique | Domain | How It Works | Used In |
|-----------|--------|-------------|---------|
| **FDMA** | Frequency | Divides spectrum into fixed frequency bands; each user gets one band | 1G (AMPS) |
| **TDMA** | Time | Divides time into slots; users take turns on same frequency | 2G (GSM) |
| **CDMA** | Code | All users share all frequencies and time; unique spreading codes separate signals | 3G (IS-95, UMTS) |
| **SDMA** | Space | Directional antennas or beamforming serve different users in different spatial directions | Satellite, MIMO |
| **OFDMA** | Freq + Time | Divides bandwidth into orthogonal subcarriers; groups assigned to users | 4G LTE, Wi-Fi 6 |

**CDMA Details:**
- Each user multiplied by a unique **spreading code** (PN sequence / Walsh code)
- **Processing gain** = chip rate / data rate
- Soft capacity limit — more users = more noise, graceful degradation
- Near-far problem requires **power control**

**OFDMA Details:**
- Extends OFDM: subcarriers grouped into **resource blocks** assigned to users
- Orthogonal subcarriers eliminate inter-carrier interference
- Used in 4G LTE (downlink), Wi-Fi 6, WiMAX

---

## 5. Mobile IP and Handoff

### Mobile IP (RFC 5944 / RFC 6275)

**Components:**
- **Mobile Node (MN):** the device that moves
- **Home Agent (HA):** router on the home network that tunnels packets to MN
- **Foreign Agent (FA):** router on the visited network
- **Care-of Address (CoA):** temporary address on the visited network

**Mobile IPv4 Process:**
1. MN moves to foreign network, discovers FA
2. MN registers CoA with HA
3. CN sends packets to MN's home address -> HA intercepts
4. HA tunnels packets (IP-in-IP) to FA at CoA
5. FA decapsulates and delivers to MN

**Triangle Problem:** CN -> HA -> FA -> MN (suboptimal route)

**Mobile IPv6 Improvements:**
- Route optimization via **Binding Updates** sent directly to CN
- No FA needed — MN generates its own CoA

---

## 6. Wireless Security

### Wi-Fi Security Protocols

| Protocol | Encryption | Key Mgmt | Status |
|----------|------------|----------|--------|
| **WEP** | RC4 (40/104-bit) | Static shared key | Broken — never use |
| **WPA** | RC4 + TKIP | 802.1X / PSK | Deprecated |
| **WPA2** | AES-CCMP (128-bit) | 802.1X / PSK | Vulnerable to KRACK |
| **WPA3** | AES-GCMP (192/256-bit) | SAE | Recommended |

**WPA3 Improvements:**
- **SAE** replaces PSK: resistant to offline dictionary attacks
- **Forward secrecy:** compromising one key doesn't expose past traffic
- **Protected Management Frames (PMF):** mandatory, prevents deauth attacks

### Wireless Attacks

| Attack | Description | Mitigation |
|--------|-------------|------------|
| **Evil Twin** | Rogue AP mimics legitimate SSID | WPA3-SAE, 802.1X, wireless IDS |
| **Wardriving** | Scanning for open/weak Wi-Fi | WPA3, disable SSID broadcast |
| **Jamming** | Flooding RF band with noise | Spread spectrum, frequency hopping |
| **Deauthentication** | Forcing clients to disconnect | PMF (802.11w), WPA3 |
| **Man-in-the-Middle** | Intercepting communication | Certificate validation, VPN |

---

## 7. Antenna Types

| Type | Pattern | Gain | Use Case |
|------|---------|------|----------|
| **Isotropic** | Omnidirectional (theoretical) | 0 dBi | Reference model |
| **Dipole** | Figure-8 | 2.15 dBi | Basic AP, handheld |
| **Patch** | Directional, low profile | 5-8 dBi | Wall/ceiling mount APs |
| **Yagi-Uda** | Highly directional | 10-17 dBi | Point-to-point links |
| **Parabolic dish** | Very narrow beam | 20-30 dBi | Satellite, long-range |
| **Sector** | Wide horizontal, narrow vertical | 10-15 dBi | Cellular base stations |
| **Phased array** | Electronically steerable | Variable | 5G beamforming, radar |

**MIMO Concepts:**
- **Spatial diversity:** multiple antennas reduce fading effects
- **Spatial multiplexing:** send independent data streams per antenna
- **Massive MIMO:** 64+ elements, 5G base stations

---

## 8. Radio Propagation

### Propagation Mechanisms

- **Free-space path loss:** signal weakens with distance squared
- **Reflection:** bouncing off large surfaces
- **Diffraction:** bending around obstacles
- **Scattering:** dispersing off rough surfaces
- **Absorption:** energy absorbed by materials

### Friis Free-Space Formula

$$P_r = P_t G_t G_r \left(\frac{\lambda}{4\pi d}\right)^2$$

### Path Loss Models

| Model | Environment | Path Loss Exponent (n) |
|-------|-------------|----------------------|
| Free space | Line of sight | 2 |
| Urban (Okumura-Hata) | Dense city | 3.5-4.5 |
| Suburban | Residential | 2.5-3.5 |
| Indoor | Office | 2.5-4.0 |

### Key Phenomena

- **Shadow fading:** large-scale variations from obstacles (log-normal)
- **Rayleigh fading:** no line-of-sight — deep fades
- **Rician fading:** line-of-sight + scattered paths
- **Doppler shift:** frequency change due to relative motion
- **Multipath:** signal arrives via multiple paths with different delays

---

## 9. Channel Capacity

### Shannon-Hartley Theorem

$$C = B \log_2\!\left(1 + \frac{S}{N}\right)$$

Where C = capacity (bits/sec), B = bandwidth (Hz), S/N = signal-to-noise ratio.

### Nyquist Theorem (Noiseless Channel)

$$C = 2B \log_2(M)$$

Where M = number of signal levels.

### Practical Factors

| Factor | Effect on Capacity |
|--------|-------------------|
| More bandwidth | Linear increase |
| Higher SNR | Logarithmic increase |
| MIMO (N antennas) | Up to N capacity |
| Coding gain | Approaches Shannon limit |

---

## Key Takeaways

1. **Wireless networks** span from WPAN (Bluetooth, Zigbee) to WWAN (cellular), each optimized for range and data rate
2. **Wi-Fi evolution** (802.11a/be) has increased speeds from 2 Mbps to 46 Gbps through OFDM, MIMO, and wider channels
3. **Cellular generations** (1G-5G) evolved from analog voice to all-IP with ultra-low latency and massive IoT support
4. **Multiple access techniques** (FDMA -> TDMA -> CDMA -> OFDMA) enable efficient spectrum sharing among many users
5. **Mobile IP** enables connectivity during movement; handoff types (hard/soft/vertical) manage transitions between cells
6. **Wireless security** has evolved from broken WEP to WPA3-SAE; defense-in-depth is essential
7. **Shannon-Hartley theorem** sets the fundamental limit on channel capacity based on bandwidth and SNR
8. **Propagation models** predict signal behavior; fading (Rayleigh/Rician) and multipath are key challenges

---

## Related

- [[Computer Networks Overview]] — OSI model, TCP/IP, HTTP
- [[01 OSI & TCP-IP Models]] — Network protocol fundamentals
- [[02 TCP & UDP]] — Transport layer protocols
- [[03 Network Security]] — Network security fundamentals

---

## Sources

- Stallings, W. *Wireless Communications & Networks*
- Rappaport, T.S. *Wireless Communications: Principles and Practice*
- IEEE 802.11 Standards (Wi-Fi Alliance)
- 3GPP TS 38 Series (5G NR)
- RFC 5944 (Mobile IPv4), RFC 6275 (Mobile IPv6)
- SWEBOK v4, Chapter 16 — Computing Foundations
