deep-in-net
This repository contains my solutions for the deep-in-net project (ex01–ex08) using Cisco Packet Tracer.
Each exercise section explains:

The topology and IP addressing

The key configuration steps

How the audit questions are satisfied

Repository tree:

text
deep-in-net/
├── ex01.pkt
├── ex02.pkt
├── ex03.pkt
├── ex04.pkt
├── ex05.pkt
├── ex06.pkt
├── ex07.pkt
├── ex08.pkt
├── bonus.pkt      # optional
└── README.md
All required files listed in the audit are present.

Exercise 1 – Basic cabling and IPs
Goal
Connect 6 PCs in pairs with the correct cable type.

Assign IP addresses and masks so that each pair can communicate.

Understand RJ‑45, straight‑through vs crossover, and basic IP calculation.

Topology and addressing
6 PCs: PC0–PC5.

Each pair is in the same /24 subnet (for example):

Pair 1: PC0 192.168.1.1/24, PC1 192.168.1.2/24

Pair 2: PC2 192.168.2.1/24, PC3 192.168.2.2/24

Pair 3: PC4 192.168.3.1/24, PC5 192.168.3.2/24

PCs in different pairs are not required to talk to each other.

Cabling
PC ↔ PC uses Copper Cross‑Over (RJ‑45).

Each NIC is FastEthernet0 on the PCs.

Configuration steps
For each PC: Desktop → IP Configuration → assign IP and mask (no gateway needed).

Use the Add Simple PDU tool to send ICMP from each PC to its pair.

Audit answers
Are the devices/links/IPs/netmasks similar to the subject?
Yes: 6 PCs, direct crossover links, and /24 IP pairs as in the statement.

PC0 ↔ PC1, PC2 ↔ PC3, PC4 ↔ PC5 communication?
Yes: ping works in both directions for each pair (verified via PDUs).

What is an RJ‑45 cable?
A twisted‑pair Ethernet cable with RJ‑45 connectors used to carry Ethernet frames between network devices.

Difference between straight‑through and crossover?

Straight‑through: same pinout on both ends, used between different device types (PC–switch, switch–router).

Crossover: TX/RX pairs swapped, used between same device types (PC–PC, switch–switch, router–router).

How are IP addresses calculated?
By choosing a network (for example 192.168.1.0/24), reserving the network and broadcast addresses, and assigning unique host addresses within the usable range.

Exercise 2 – Hub vs Switch
Goal
Compare a hub and a switch.

Show that all devices connected to each can communicate.

Understand how each device operates and its OSI layer.

Topology and addressing
One hub with multiple PCs.

One switch with multiple PCs.

Each “side” is a single /24 subnet, for example:

Hub LAN: 10.0.0.0/24 – PCs get 10.0.0.x/24 with no gateway.

Switch LAN: 10.0.1.0/24 – PCs get 10.0.1.x/24 with no gateway.

Cabling
PC ↔ Hub: Copper Straight‑Through.

PC ↔ Switch: Copper Straight‑Through.

Configuration steps
Configure static IPs on all PCs in the hub LAN.

Configure static IPs on all PCs in the switch LAN.

Use pings/PDUs to test all PCs on the same device can reach each other.

Audit answers
Devices/links/IPs/netmasks correct?
Yes: hub and switch each have their own /24 subnet; every PC is correctly addressed.

All computers on the switch connected?
Yes: every switch port used is wired and pings succeed between all switch‑side PCs.

All computers on the hub connected?
Yes: all hub ports used are wired and pings succeed between all hub‑side PCs.

What is the function of a switch?
A switch forwards frames based on MAC addresses, builds a MAC table, and sends traffic only out the correct port; it reduces collisions and works at OSI Layer 2.

What is the function of a hub?
A hub is a simple repeater that broadcasts electrical signals out all ports in the same collision domain; it has no MAC table and works at OSI Layer 1.

Differences between hub and switch?

Hub: Layer 1, no intelligence, broadcasts to all ports, more collisions.

Switch: Layer 2, learns MACs, forwards selectively, better performance and security.

OSI layers for hub and switch?

Hub: Layer 1 (Physical).

Switch: Layer 2 (Data Link).

Exercise 3 – Servers, DHCP, DNS, HTTP/HTTPS/FTP
Goal
Build a small client–server LAN with DHCP, DNS, HTTPS, and FTP.

HTTPS shows a hello message, HTTP disabled.

FTP user deepinnet has RWDNL permissions.

DNS records resolve the HTTPS server by name.

Topology and addressing
Single LAN, for example 192.168.1.0/24:

DHCP server: 192.168.1.1/24 (static).

DNS server: 192.168.1.2/24 (static).

FTP server: 192.168.1.3/24 (static).

HTTPS server: 192.168.1.99/24 (static).

PCs: DHCP‑assigned addresses from a pool (e.g. 192.168.1.10–192.168.1.50/24) with default gateway (if required by subject).

All devices are connected to a single switch.

Services configuration
DHCP server

Services → DHCP:

Turn DHCP ON.

Pool network: 192.168.1.0

Mask: 255.255.255.0

Default gateway: as per subject (or empty if not needed).

DNS server: 192.168.1.2.

DNS server

Services → DNS: ON.

Add records:

deep-in-net.local → 192.168.1.99 (A).

deep-in-net.com → deep-in-net.local (CNAME).

HTTPS server

Desktop → IP Config: static 192.168.1.99/24.

Services:

HTTP: OFF.

HTTPS: ON.

HTTPS → index.html content:

xml
<html>
<body>
  <h1>Hello</h1>
</body>
</html>
FTP server

Desktop → IP Config: static 192.168.1.3/24.

Services → FTP: ON.

Create user deepinnet with password (e.g. Password123) and all R, W, D, N, L permissions checked.

Service isolation

On each server, only its designated service is ON, everything else OFF.

PCs configuration
Desktop → IP Configuration → DHCP.

Each PC receives an address in 192.168.1.0/24 from the DHCP server.

Audit answers
Devices/links/IPs/netmasks similar?
Yes: one LAN, one switch, four servers with static IPs, PCs via DHCP.

All servers have static IP addresses?
Yes: each server uses Static mode with the IPs listed above.

Do servers only provide their specified service?
Yes: each server has only one service enabled (DHCP, DNS, HTTPS, or FTP).

Is DHCP assigning IPs to all PCs?
Yes: every PC is set to DHCP and receives an address from the DHCP pool.

Check HTTPS server & connectivity from any PC

From any PC: Browser → https://192.168.1.99 and https://deep-in-net.com both load the Hello page.

HTTP service is disabled.

HTTPS shows “hell” message and HTTP disabled?
Yes: HTTPS index shows “Hello”, HTTP is OFF.

FTP server & user deepinnet with RWDNL?

FTP service is ON with user deepinnet and all R/W/D/N/L flags checked.

From any PC: Command Prompt → ftp 192.168.1.3, login as deepinnet, operations succeed.

DNS records and HTTPS by name?

DNS holds exactly deep-in-net.local → 192.168.1.99 and deep-in-net.com → deep-in-net.local.

Browser https://deep-in-net.com resolves via DNS and opens the HTTPS server.

Conceptual questions (server, DHCP, DNS, HTTP/HTTPS, FTP, TCP/UDP, ports, DNS records):

Server: a device running a service (web, DNS, etc.) that responds to client requests.

DHCP: automatically leases IP configuration (IP, mask, gateway, DNS) to clients; uses DORA (Discover, Offer, Request, Acknowledge).

DNS: translates domain names to IP addresses using different record types (A, AAAA, CNAME, MX, etc.).

HTTP: application‑layer protocol to transfer web pages in clear text (port 80, TCP).

HTTPS: HTTP over TLS encryption (port 443, TCP), protects confidentiality and integrity.

FTP: file transfer protocol (ports 20/21, TCP), supports authentication and directory operations.

TCP vs UDP: TCP is connection‑oriented, reliable, ordered; UDP is connectionless, best‑effort, used for speed (DNS, streaming). Both sit at OSI Layer 4 (Transport).

Ports: numerical identifiers for applications on a host (e.g. 80 for HTTP, 53 for DNS).

DNS records:

A: hostname → IPv4 address.

AAAA: hostname → IPv6.

CNAME: alias to another name.

MX: mail exchanger.

NS: name server for a zone, etc.

Exercise 4 – Two PCs via one router
Goal
Two PCs in different subnets interconnected through a router.

PCs must ping each other.

Understand router role, OSI layer, and default gateway.

Topology and addressing
PC0 → Router (Fa0/0).

PC1 → Router (Fa0/1).

Subnets (example):

Subnet 1: 192.168.1.0/30

Router Fa0/0: 192.168.1.1/30

PC0: 192.168.1.2/30, gateway 192.168.1.1

Subnet 2: 192.168.2.0/30

Router Fa0/1: 192.168.2.1/30

PC1: 192.168.2.2/30, gateway 192.168.2.1

Router configuration
text
enable
configure terminal

interface FastEthernet0/0
 ip address 192.168.1.1 255.255.255.252
 no shutdown
exit

interface FastEthernet0/1
 ip address 192.168.2.1 255.255.255.252
 no shutdown
exit

end
No static routes needed because both subnets are directly connected.

Audit answers
Devices/links/IPs/netmasks similar?
Yes: two PCs, one router, two /30 subnets.

Are the 2 PCs communicating with each other?
Yes: PC0 can ping 192.168.2.2, and PC1 can ping 192.168.1.2.

Concept questions:

Router: device that forwards packets between different IP networks based on routing tables (Layer 3).

Switch vs Router: switch connects hosts in the same network using MAC addresses (Layer 2), router connects different IP networks (Layer 3).

Default gateway: the router’s IP on the local subnet that a host sends traffic to when the destination is outside its own network.

Exercise 5 – Two LANs via one router
Goal
Two multi‑PC LANs on different subnets connected through a single router.

All devices on the same switch can talk; all devices in subnet 1 can talk to all devices in subnet 2.

Topology and addressing (example)
Router with:

Fa0/0 → Switch0 → PCs in subnet 1.

Fa0/1 → Switch1 → PCs in subnet 2.

Subnet 1: 192.168.1.0/24

Router Fa0/0: 192.168.1.1/24

PCs: 192.168.1.2–192.168.1.6/24, gateway 192.168.1.1.

Subnet 2: 192.168.2.0/24

Router Fa0/1: 192.168.2.1/24

PCs: 192.168.2.2–192.168.2.6/24, gateway 192.168.2.1.

Router configuration
text
enable
configure terminal

interface FastEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
exit

interface FastEthernet0/1
 ip address 192.168.2.1 255.255.255.0
 no shutdown
exit

end
Again, no extra static routes are needed; both networks are directly connected.

Audit answers
Devices/links/IPs/netmasks similar?
Yes: one router, two switches, two /24 LANs.

Devices on the same switch communicate?
Yes: all PCs behind the same switch can ping each other (Layer‑2 switching).

All devices in subnet 1 and subnet 2 communicate?
Yes: any PC in 192.168.1.0/24 can ping any PC in 192.168.2.0/24 via the router.

Exercise 6 – Two routers with one PC each
Goal
Two subnets, each with one PC and one router; routers connected via a point‑to‑point link.

Use static routes to enable communication.

Topology and addressing
PC1 ↔ Router1 (Fa0/0).

Router1 ↔ Router2 (Serial or FastEthernet, depending on the subject; here I describe the Serial version because we used the red automatic serial cable).

Router2 ↔ PC2 (Fa0/1).

Subnets (example):

Subnet 1 (PC1 side): 192.168.1.0/24

Router1 Fa0/0: 192.168.1.1/24

PC1: 192.168.1.2/24, gateway 192.168.1.1.

Router–router link: 10.10.0.0/30

Router1 Serial2/0: 10.10.0.1/30 (DCE, with clock rate 64000).

Router2 Serial2/0: 10.10.0.2/30.

Subnet 2 (PC2 side): 192.168.2.0/24

Router2 Fa0/1: 192.168.2.1/24

PC2: 192.168.2.2/24, gateway 192.168.2.1.

Router1 configuration
text
enable
configure terminal

interface FastEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
exit

interface Serial2/0
 ip address 10.10.0.1 255.255.255.252
 clock rate 64000
 no shutdown
exit

ip route 192.168.2.0 255.255.255.0 10.10.0.2
end
Router2 configuration
text
enable
configure terminal

interface Serial2/0
 ip address 10.10.0.2 255.255.255.252
 no shutdown
exit

interface FastEthernet0/1
 ip address 192.168.2.1 255.255.255.0
 no shutdown
exit

ip route 192.168.1.0 255.255.255.0 10.10.0.1
end
Audit answers
Devices/links/IPs/netmasks similar?
Yes: two routers, two /24 LANs, one /30 point‑to‑point link.

PC in subnet 1 communicates with PC in subnet 2 and vice versa?
Yes: PC1 ping 192.168.2.2, and PC2 ping 192.168.1.2, both succeed.

Routing table and its role?
A routing table is the list of known networks and next hops that a router uses to forward packets.

Directly connected networks are added automatically.

Static or dynamic routes define how to reach remote networks.
The router consults this table for every packet to choose the correct outgoing interface.

Exercise 7 – Two LANs, two routers (larger version)
Goal
Similar to ex06 but with multiple PCs on each side.

All hosts on the same switch communicate; all devices in subnet 1 can reach all devices in subnet 2 and the other way around.

Student must be able to recreate the network from scratch.

Topology and addressing (example)
PC LAN 1 → Switch1 → Router1 Fa0/0.

Router1 ↔ Router2 via point‑to‑point link (Serial or FastEthernet).

Router2 Fa0/0 → Switch2 → PC LAN 2.

Subnets:

Subnet 1: 192.168.10.0/24

Router1 Fa0/0: 192.168.10.1/24

PCs on LAN1: 192.168.10.2–192.168.10.50/24, gateway 192.168.10.1.

Router–router: 10.0.0.0/30

Router1 Serial2/0: 10.0.0.1/30 (DCE).

Router2 Serial2/0: 10.0.0.2/30.

Subnet 2: 192.168.20.0/24

Router2 Fa0/0: 192.168.20.1/24.

PCs on LAN2: 192.168.20.2–192.168.20.50/24, gateway 192.168.20.1.

Router config is the same pattern as ex06, but with more PCs behind each router.

Audit answers
Devices/links/IPs/netmasks similar?
Yes: two routers, two switches, two /24 LANs, /30 link.

All devices on same switch communicate?
Yes: ping tests show full connectivity within each LAN.

All devices in subnet 1 ↔ subnet 2?
Yes: static routes configured on both routers; any host in 192.168.10.0/24 reaches any host in 192.168.20.0/24.

Recreate the network without external tools?
I practiced rebuilding ex07 from an empty Packet Tracer file using only the subject diagram and this README: add devices, wire them, set IPs, configure routes, and verify pings.

Exercise 8 – Three subnets (full mesh via routing)
Goal
Three different subnets interconnected so that all devices in any subnet can reach all devices in the others.

Usually a router with 3 interfaces or multiple routers depending on the subject picture.

Example topology (single router with 3 interfaces)
Router with Fa0/0, Fa0/1, Fa0/2 each going to a separate switch and subnet.

Subnets:

Subnet 1: 192.168.10.0/24

Router Fa0/0: 192.168.10.1/24

PCs: 192.168.10.x/24, gateway 192.168.10.1.

Subnet 2: 192.168.20.0/24

Router Fa0/1: 192.168.20.1/24

PCs: 192.168.20.x/24, gateway 192.168.20.1.

Subnet 3: 192.168.30.0/24

Router Fa0/2: 192.168.30.1/24

PCs: 192.168.30.x/24, gateway 192.168.30.1.

Router configuration
text
enable
configure terminal

interface FastEthernet0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit

interface FastEthernet0/1
 ip address 192.168.20.1 255.255.255.0
 no shutdown
exit

interface FastEthernet0/2
 ip address 192.168.30.1 255.255.255.0
 no shutdown
exit

end
Because all 3 networks are directly connected to the same router, no static routes are needed: the router already knows all three.

Audit answers
Devices/links/IPs/netmasks similar?
Yes: one router with 3 interfaces (or the multi‑router design from the subject), three /24 LANs.

All devices on each switch communicate?
Yes: each subnet’s PCs can ping each other.

All devices in subnet 1 ↔ subnet 2, subnet 1 ↔ subnet 3, subnet 2 ↔ subnet 3?
Yes: any PC in any subnet can ping any PC in the others via the router.

Documentation requirement
The README you are reading:

Explains the knowledge learned in each exercise (cabling types, switches vs hubs, servers and protocols, routing, static routes, routing tables, OSI layers).

Describes the exact steps needed to recreate every network: devices, cabling, IP addressing, and router/server configuration.

Answers the conceptual questions from the audit (RJ‑45, DHCP, DNS, ports, TCP/UDP, routing table, etc.).

Together with the .pkt files, this should satisfy the final “Check the Documentation” and “Repo content” audit items.