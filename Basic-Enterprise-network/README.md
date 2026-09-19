# Multi-Branch Network Subnetting

## Project Overview

This project demonstrates the design of a basic multi-branch network using IPv4 subnetting.

The network consists of three branches with different user requirements.

## Network Addressing

| Branch | Users | Network | CIDR | Subnet Mask |
|--------|-------|---------|------|-------------|
| Branch 1 | 200 | 192.168.1.0 | /24 | 255.255.255.0 |
| Branch 2 | 100 | 172.16.1.0 | /25 | 255.255.255.128 |
| Branch 3 | 10 | 10.0.0.0 | /28 | 255.255.255.240 |

## Branch 1

- Network: 192.168.1.0/24
- Usable Hosts: 254
- Host Range: 192.168.1.1 - 192.168.1.254
- Broadcast: 192.168.1.255
- Enable = Branch1
- Console = Israr
- username = Branch-1 & pass = Branch1

## Branch 2

- Network: 172.16.1.0/25
- Usable Hosts: 126
- Host Range: 172.16.1.1 - 172.16.1.126
- Broadcast: 172.16.1.127
- - Enable = Branch2
- Console = Israr
- username = Branch-2 & pass = Branch2

## Branch 3

- Network: 10.0.0.0/28
- Usable Hosts: 14
- Host Range: 10.0.0.1 - 10.0.0.14
- Broadcast: 10.0.0.15
- Enable = Branch3
- Console = Israr
- username = Branch-3 & pass = Branch3

## Router
- Enable = ISP
- Console = Israr
- username = ISP-router & pass = ISP


## Tools

- Cisco Packet Tracer
- IPv4 Addressing
- Subnetting

## Project Files

- Basic network.pkt — Cisco Packet Tracer project
- topology.png — Network topology
- ping-test.png — Connectivity test

## Skills Demonstrated

- IPv4 addressing
- Subnetting
- CIDR notation
- Network and broadcast addresses
- Host calculation
- Basic network design
