# deep-in-net

## Introduction
This project introduces core networking concepts using Cisco Packet Tracer.
All exercises were configured using CLI only, following static IP and subnet requirements.

---

## Networking Concepts

### RJ-45 Cables
- **Straight-through**: Connects different device types (PC→Switch, PC→Router). 
  TX pins on one end connect to RX on the other.
- **Crossover**: Connects same device types (Router→Router, PC→PC, Switch→Switch). 
  TX pins connect to TX, requiring a physical cross in the wiring.

### Hub vs Switch
| Feature | Hub | Switch |
|---------|-----|--------|
| OSI Layer | Layer 1 (Physical) | Layer 2 (Data Link) |
| Traffic handling | Broadcasts to all ports | Forwards to specific MAC |
| Collision domain | One shared domain | Per-port collision domain |
| Efficiency | Low | High |

### Router
- Operates at **OSI Layer 3 (Network)**
- Routes traffic between different networks using IP addresses
- Maintains a **routing table** to determine the best path for packets
- The **default gateway** is the router interface IP that hosts send traffic to when the destination is outside their subnet

### Routing Table
A routing table is a database stored on a router listing known networks, 
the next-hop address to reach them, and the interface to use. 
Static routes are manually configured; connected routes are added automatically.

### Protocols & Ports
| Protocol | Port | OSI Layer | Transport |
|----------|------|-----------|-----------|
| HTTP | 80 | Layer 7 (Application) | TCP |
| HTTPS | 443 | Layer 7 (Application) | TCP |
| FTP | 20/21 | Layer 7 (Application) | TCP |
| DNS | 53 | Layer 7 (Application) | UDP/TCP |
| DHCP | 67/68 | Layer 7 (Application) | UDP |

### TCP vs UDP
| Feature | TCP | UDP |
|---------|-----|-----|
| Connection | Connection-oriented | Connectionless |
| Reliability | Guaranteed delivery | No guarantee |
| Speed | Slower | Faster |
| Use case | HTTP, FTP, HTTPS | DNS, DHCP, streaming |

### OSI Model
| Layer | Name | Example |
|-------|------|---------|
| 7 | Application | HTTP, FTP, DNS |
| 6 | Presentation | SSL/TLS |
| 5 | Session | NetBIOS |
| 4 | Transport | TCP, UDP |
| 3 | Network | IP, Router |
| 2 | Data Link | Switch, MAC |
| 1 | Physical | Hub, cables |

### DNS Record Types
- **A record**: Maps hostname → IPv4 address (`deep-in-net.local → 192.168.1.99`)
- **CNAME record**: Maps hostname → another hostname (`deep-in-net.com → deep-in-net.local`)
- **MX record**: Mail server for a domain
- **NS record**: Authoritative name server for a domain

---

## Subnet Calculations (manual)

### How to calculate subnets
- `/24` → mask `255.255.255.0` → 254 usable hosts
- `/26` → mask `255.255.255.192` → 62 usable hosts (2⁶ - 2)
- `/28` → mask `255.255.255.240` → 14 usable hosts (2⁴ - 2)
- `/30` → mask `255.255.255.252` → 2 usable hosts (2² - 2) — ideal for point-to-point WAN links

---

## Exercises

### Exercise 1 — Direct PC Communication
**Topology**: 3 pairs of PCs connected directly  
**Cable**: Crossover (same device type)  
**Concept**: RJ-45 cable types, direct communication without network devices

### Exercise 2 — Hub and Switch
**Topology**: PCs connected via a Switch and a Hub  
**Concept**: Switch operates at Layer 2 (forwards by MAC), Hub at Layer 1 (broadcasts to all)

### Exercise 3 — Server Services
**Topology**: DHCP, DNS, HTTPS, FTP servers + PCs on a switch  
**Key configs**:
- DHCP assigns IPs automatically to all PCs
- DNS maps `deep-in-net.local → 192.168.1.99` and `deep-in-net.com → deep-in-net.local`
- HTTPS serves a hello message on port 443; HTTP disabled
- FTP has user `deepinnet` with RWDNL permissions

### Exercise 4 — Basic Routing
**Topology**: 2 PCs, 1 Router  
**Concept**: Default gateway, router as inter-network device (Layer 3)

### Exercise 5 — Two Subnets via Router + Switches
**Topology**: Router1 connecting Subnet1 and Subnet2, each with a switch and multiple PCs  
**WAN link**: `10.10.0.0/30`  
**Static routes** configured on Router1 for both directions

### Exercise 6 — Routing Table
**Topology**: 2 Routers, 2 PCs  
**Concept**: Routing table entries, static route configuration, next-hop addressing

### Exercise 7 — Extended Two-Subnet Network
**Topology**: 2 Routers, 2 Switches, multiple PCs and Laptop  
**Subnets**: `192.168.1.0/24`, `192.168.2.0/24`, WAN `10.10.0.0/30`

### Exercise 8 — Three-Subnet Network
**Topology**: 3 Routers, 3 Switches, 12 end devices  
**Subnets**:
- Subnet 1: `192.168.1.0/26` (Router1 Fa0/0: `192.168.1.1`)
- Subnet 2: `192.168.2.0/24` (Router2 Fa0/0: `192.168.2.1`)
- Subnet 3: `192.168.3.160/28` (Router3 Fa0/0: `192.168.3.161`)
- WAN R1↔R2: `10.10.0.0/30`
- WAN R2↔R3: `10.10.1.0/30`

**Static routes**: Each router has routes to all remote subnets via the appropriate next-hop.

---

## Files
- `ex01.pkt` - Exercise 1: Direct PC Communication
- `ex02.pkt` - Exercise 2: Hub and Switch
- `ex03.pkt` - Exercise 3: Server Services
- `ex-04.pkt` - Exercise 4: Basic Routing
- `ex-05.pkt` - Exercise 5: Two Subnets via Router + Switches
- `ex-06.pkt` - Exercise 6: Routing Table
- `ex-07.pkt` - Exercise 7: Extended Two-Subnet Network
- `ex-08.pkt` - Exercise 8: Three-Subnet Network

---

## Requirements
- **Cisco Packet Tracer** (for opening `.pkt` files)
- All configurations were done using **CLI only** (no GUI configuration)

---

## Learning Outcomes
- Understanding of OSI Model layers and their functions
- Practical experience with network device types and their roles
- Subnetting and IP addressing concepts
- Static routing configuration and routing tables
- Server services configuration (DHCP, DNS, HTTPS, FTP)
- Network topology design and implementation

---

## Author
Created as a hands-on learning project for networking fundamentals.
