# 🌐 Networking Fundamentals: Topologies, Addressing & Subnetting

A clear, structured set of notes covering the core building blocks of computer networking — from how devices physically connect, to how they find and talk to each other, to how large networks are broken down and routed.

📄 **[Download the full PDF version](./Networking_Fundamentals_Notes.pdf)**

---

## 📚 Table of Contents

1. [What Is a Network?](#-1-what-is-a-network)
2. [Network Topologies & Devices](#-2-network-topologies--devices)
3. [Addressing & Data Transfer Mechanisms](#-3-addressing--data-transfer-mechanisms)
4. [Subnetting & IP Addressing Structure](#-4-subnetting--ip-addressing-structure)
5. [Inter-Subnet Communication & Routers](#-5-inter-subnet-communication--routers)

---

## 🔹 1. What Is a Network?

A **network** is formed when two or more devices connect together to communicate and exchange data. Devices need a medium — wired or wireless — to communicate, and the structural arrangement of these connections is called a **network topology**.

---

## 🔹 2. Network Topologies & Devices

### A. Bus Topology
All devices connect directly along a single shared cable (the bus line).

**Drawbacks:**
- **Privacy/security risk** — data sent from Device A to Device C also passes through intermediate devices (e.g., Device B receives it too).
- **Scalability & traffic issues** — with 10 connected computers, a single message is broadcast to the other 9, causing heavy network congestion.

### B. Mesh Topology
Every device has a dedicated point-to-point connection to every other device in the network.

**Advantages:**
- Direct, private communication between any pair of devices, with no intermediary leakage.

**Drawbacks:**
- Requires an extreme amount of cabling.
- Very difficult and expensive to scale or manage as new devices are added.

### C. Star Topology & Switches
Devices connect to a central, common intermediary device called a **switch**.

- **Role of a switch:** receives data from a source device and forwards it specifically to the destination device, instead of broadcasting it everywhere.
- **OSI layer context:** a switch is a **Layer 2 (Data Link Layer)** device. Switches only understand **MAC addresses**, not IP addresses.

---

## 🔹 3. Addressing & Data Transfer Mechanisms

### A. IP Address (Internet Protocol)
A unique numerical identifier assigned to each device on a network, used to route data logically.

### B. MAC Address (Media Access Control)
- **Definition:** a permanent, physical address assigned to a Network Interface Card (NIC) by the device manufacturer.
- **Function:** used by Layer 2 devices (like switches) to deliver frames to the correct physical hardware interface.

### C. ARP (Address Resolution Protocol)
**Purpose:** maps a known IP address to an unknown MAC address.

**How ARP works:**
1. **ARP Request (Broadcast)** — when Device A wants to send data to an IP address, it sends an ARP request asking, "Who owns this IP address? Send me your MAC address."
2. **Broadcast nature** — the request is sent to every device on the network; devices that don't match the IP ignore it.
3. **ARP Reply (Unicast)** — the device owning that target IP responds directly with its MAC address.
4. **ARP Cache** — the requesting computer temporarily saves this IP-to-MAC mapping in local memory, so future transmissions don't require another ARP broadcast.

---

## 🔹 4. Subnetting & IP Addressing Structure

### A. Why Subnet?
In large organizations with hundreds of computers, broad broadcast traffic (like ARP) bogs down network performance. **Subnetting** breaks a single large network into smaller, logically isolated sub-networks (e.g., an HR subnet, a Finance subnet) to contain broadcast traffic locally.

### B. IPv4 Address Breakdown
An IPv4 address is 32 bits long, divided into 4 bytes (octets).

**Network ID vs. Host ID** — example: `192.168.1.X`
- The first 3 bytes (`192.168.1`) identify the **network**. Devices with matching network portions belong to the same local network.
- The last byte (`X`) identifies the individual **host** (device) within that network.

### C. Usable IP Addresses in a /24 Subnet

| Item | Value | Notes |
|---|---|---|
| Total combinations (1 byte, 0–255) | 256 addresses | Full range of the host octet |
| Network IP | `192.168.1.0` | Identifies the subnet itself — unusable for individual devices |
| Broadcast IP | `192.168.1.255` | Used to broadcast to all devices in the subnet — unusable for single devices |
| **Usable host addresses** | **254 (256 − 2)** | Range: `.1` to `.254` |

### D. CIDR (Classless Inter-Domain Routing)
CIDR solves the problem of IP address wastage. Instead of assigning a full standard block of 256 IPs when only a few are needed, CIDR allows custom-sized networks tailored to exact host requirements.

---

## 🔹 5. Inter-Subnet Communication & Routers

- **Switches** handle traffic within the same network/subnet, using MAC addresses.
- **Routers** are required to route traffic between different subnets or distinct networks (e.g., communicating from `192.168.1.0` to `192.168.2.0`).

---

## ✅ Quick Recap

- **Topology** = the layout of how devices connect (bus, mesh, star).
- **Switch** = Layer 2 device that forwards traffic using MAC addresses within a network.
- **Router** = connects different networks/subnets together.
- **ARP** = resolves an IP address to a MAC address.
- **Subnetting + CIDR** = contains broadcast traffic and avoids wasting IP addresses.

---

## 🗂️ Repo Contents

```
├── README.md                          # This file
└── Networking_Fundamentals_Notes.pdf  # Designed PDF version of these notes
```

---

*Notes compiled while studying computer networking fundamentals.*
