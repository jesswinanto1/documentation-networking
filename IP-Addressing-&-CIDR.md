# IP Addressing & CIDR — Study Notes

A concise reference guide covering IPv4 addressing fundamentals and Classless Inter-Domain Routing (CIDR). Useful for networking revision, technical interviews, and cloud/DevOps foundational knowledge.

## Table of Contents

- [1. Fundamentals of IPv4 Addressing](#1-fundamentals-of-ipv4-addressing)
- [2. Network ID vs. Host ID](#2-network-id-vs-host-id)
- [3. Special Reserved IP Addresses](#3-special-reserved-ip-addresses)
- [4. Classless Inter-Domain Routing (CIDR)](#4-classless-inter-domain-routing-cidr)
- [5. Formula to Calculate Total IPs](#5-formula-to-calculate-total-ips)
- [6. Real-World Example](#6-real-world-example)
- [7. Additional Concepts](#7-additional-concepts)
- [8. Interview Q&A](#8-interview-qa)

---

## 1. Fundamentals of IPv4 Addressing

An **IPv4 address** is a 32-bit unique numerical identifier assigned to every device (computers, mobile phones, routers) connected to a network.

### Address Structure & Anatomy

| Property | Description |
|---|---|
| Format | 4 numbers (octets) separated by periods — dotted-decimal format, e.g. `X.X.X.X` |
| Value range per octet | 0 to 255 (`256.0.0.0` is invalid) |
| Address space | `0.0.0.0` to `255.255.255.255` |
| Total possible addresses | 2³² ≈ 4.29 billion |

### How IPv4 Increments

IPv4 counts linearly starting from the rightmost octet:

```
0.0.0.0 → 0.0.0.1 → ... → 0.0.0.255   (256 addresses)
0.0.1.0 → 0.0.1.1 → ... → 0.0.1.255
...
255.255.255.255
```

---

## 2. Network ID vs. Host ID

Every IP address is split into two logical segments:

- **Network ID** — identifies the specific network/sub-domain a device belongs to.
- **Host ID** — identifies the unique device within that network.

> **Analogy:** The Network ID is like a street name or apartment complex; the Host ID is the individual flat/door number.

---

## 3. Special Reserved IP Addresses

In any usable IP block, the **first** and **last** addresses are reserved and cannot be assigned to end devices:

| Address | Position (for a /24 block) | Purpose |
|---|---|---|
| Network IP | Ends in `.0` | Identifies the network as a single collective entity |
| Broadcast IP | Ends in `.255` | Sends data to all devices on the network simultaneously |

**Usable Hosts Formula:**

```
Usable IPs = Total IPs in block − 2
```

---

## 4. Classless Inter-Domain Routing (CIDR)

### The Problem of IP Waste

A fixed subnet like `10.0.0.0` (256 total IPs) gives 254 usable IPs. If a company only needs 3 computers, 251 IPs go to waste and cannot be reallocated elsewhere.

### The Solution: CIDR Notation

CIDR allows dynamic allocation of variable-sized subnets based on exact requirements, eliminating waste.

- Notation: `IP_ADDRESS / PREFIX` (e.g., `10.0.0.0/24`)
- The number after the slash indicates how many bits belong to the Network ID.

### CIDR Prefix Breakdown

| CIDR Prefix | Network Portion | Host Portion | Formula | Total IPs | Usable IPs | Common Use Case |
|---|---|---|---|---|---|---|
| /8 | 1st Octet | Last 3 Octets | 2²⁴ | 16,777,216 | 16,777,214 | Very Large Enterprise / ISP |
| /16 | 1st & 2nd Octets | Last 2 Octets | 2¹⁶ | 65,536 | 65,534 | Mid-to-Large Organizations |
| /24 | 1st 3 Octets | Last Octet | 2⁸ | 256 | 254 | Small Office / Home Wi-Fi |
| /29 | Custom Bits | 3 Bits | 2³ | 8 | 6 | Small Subnet (3–5 devices) |
| /30 | Custom Bits | 2 Bits | 2² | 4 | 2 | Point-to-Point Router Links |

**Key Rule:** As the CIDR prefix number decreases, the number of available host IPs increases exponentially.

---

## 5. Formula to Calculate Total IPs

```
Total IP Count = 2^(32 - N)
```

Where `N` is the CIDR prefix number.

**Practical Examples:**

| Prefix | Calculation | Total IPs | Usable IPs |
|---|---|---|---|
| /24 | 2⁽³²⁻²⁴⁾ = 2⁸ | 256 | 254 |
| /16 | 2⁽³²⁻¹⁶⁾ = 2¹⁶ | 65,536 | 65,534 |
| /29 | 2⁽³²⁻²⁹⁾ = 2³ | 8 | 6 |

---

## 6. Real-World Example

**Requirement:** A company needs to connect exactly 3 computers.

**Using /30:**
- Total IPs = 2⁽³²⁻³⁰⁾ = 4
- Reserved (Network + Broadcast) = 2
- Usable IPs = 4 − 2 = **2** ❌ (not enough for 3 devices)

**Using /29:**
- Total IPs = 2⁽³²⁻²⁹⁾ = 8
- Usable IPs = 8 − 2 = **6** ✅ (sufficient, with room for expansion)

---

## 7. Additional Concepts

### A. Subnet Mask Representation

CIDR notation is shorthand for a subnet mask:

| CIDR | Subnet Mask |
|---|---|
| /8 | 255.0.0.0 |
| /16 | 255.255.0.0 |
| /24 | 255.255.255.0 |
| /30 | 255.255.255.252 |

### B. Public vs. Private IP Addresses

| Type | Description |
|---|---|
| Private IPs | Used inside local networks (LAN), e.g., `192.168.x.x`, `10.x.x.x`. Free and not routable on the public internet. |
| Public IPs | Globally unique IP assigned by an ISP to identify a network on the public internet. |

---

## 8. Interview Q&A

**Q1: What is the total size of an IPv4 address, and what is the valid range per octet?**
An IPv4 address is 32 bits long, divided into 4 octets. Each octet ranges from 0 to 255 (256 total values per octet).

**Q2: Why can't the first and last IP addresses of a subnet block be assigned to a host?**
The first IP is reserved as the Network ID (identifies the network sub-domain), and the last IP is reserved as the Broadcast IP (used to communicate with all hosts on that network simultaneously).

**Q3: What is CIDR, and why was it introduced?**
CIDR (Classless Inter-Domain Routing) replaced fixed IP classing (Class A, B, C) to allow flexible subnetting. It prevents IP address wastage by enabling administrators to allocate subnets matching exact host requirements.

**Q4: How many usable host IPs are available in a /28 CIDR block?**
```
Total IPs = 2^(32-28) = 2^4 = 16
Usable IPs = 16 - 2 = 14
```

**Q5: If a company requires IPs for 50 host devices, which CIDR prefix should be chosen?**
```
/27 → 2^5 = 32 IPs (30 usable) → Too small
/26 → 2^6 = 64 IPs (62 usable) → Correct choice
```

**Q6: What does the CIDR prefix /24 signify in `192.168.1.0/24`?**
The `/24` prefix means the first 24 bits (first 3 octets: `192.168.1`) represent the Network portion, while the remaining 8 bits (last octet) represent the Host portion.

---

## References

Compiled from personal study notes on IPv4 addressing and CIDR concepts.
