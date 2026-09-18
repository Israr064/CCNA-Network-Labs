# Multi-Branch Network Topology

## Project Overview

This project demonstrates a basic multi-branch enterprise network
designed for three different branch offices.

Each branch has a different number of users and is assigned an
appropriate IPv4 network based on its host requirements.

The topology was designed and configured using Cisco Packet Tracer.

---

## Network Requirements

| Branch | Number of Users | Network Address | CIDR |
|--------|-----------------|-----------------|------|
| Branch 1 | 200 | 192.168.1.0 | /24 |
| Branch 2 | 100 | 172.16.1.0 | /25 |
| Branch 3 | 10 | 10.0.0.0 | /28 |

---

## IP Addressing

### Branch 1

- Network: `192.168.1.0/24`
- Subnet Mask: `255.255.255.0`
- Usable Host Range: `192.168.1.1 - 192.168.1.254`
- Broadcast: `192.168.1.255`
- Usable Hosts: 254
- enable = Branch1
- console = Israr
- Username = Branch-1  secret = Branch1
- Management vlan 99

### Branch 2

- Network: `172.16.1.0/25`
- Subnet Mask: `255.255.255.128`
- Usable Host Range: `172.16.1.1 - 172.16.1.126`
- Broadcast: `172.16.1.127`
- Usable Hosts: 126
- enable = Branch2
- console = Israr
- Username = Branch-2  secret = Branch2
- Management vlan 99

### Branch 3

- Network: `10.0.0.0/28`
- Subnet Mask: `255.255.255.240`
- Usable Host Range: `10.0.0.1 - 10.0.0.14`
- Broadcast: `10.0.0.15`
- Usable Hosts: 14
- - enable = Branch3
- console = Israr
- Username = Branch-3  secret = Branch3
- Management vlan 99

---

## Network Design

The network consists of three separate branch networks:

- Branch 1 — 200 users
- Branch 2 — 100 users
- Branch 3 — 10 users

Each branch uses a separate IPv4 network.

---

## ISP router

- enable secret = ISP
- console = Israr
- username = ISP-router pass ISP
## Technologies

- Cisco Packet Tracer
- IPv4 Addressing
- Subnetting
- Network Identification
- Broadcast Addressing
- Host Addressing

---

## Subnetting Considerations

The subnet size for each branch was selected according to
the number of required users.

Branch 1 requires at least 200 usable addresses, therefore `/24`
was selected.

Branch 2 requires at least 100 usable addresses, therefore `/25`
was selected.

Branch 3 requires at least 10 usable addresses, therefore `/28`
was selected.

---

## Project Objectives

The objectives of this project are:

1. Design a multi-branch network.
2. Assign suitable IPv4 networks to each branch.
3. Calculate subnet masks and usable host ranges.
4. Understand network and broadcast addresses.
5. Build the topology in Cisco Packet Tracer.
6. Document the network design.

---

## Verification

The following information was verified during the project:

- Network addresses
- Subnet masks
- Usable host ranges
- Broadcast addresses
- Number of available hosts

---

## Project Files

- `topology.png` — Network topology screenshot
- `Network-Topology.pkt` — Cisco Packet Tracer project
- `verification.png` — Verification screenshot

---

## Skills Demonstrated

- IPv4 Addressing
- Subnetting
- CIDR
- Network Planning
- Cisco Packet Tracer
- Network Documentation
