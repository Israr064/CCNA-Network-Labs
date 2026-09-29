# DHCP Configuration with Two LANs – Cisco Packet Tracer

## 📌 Project Overview

This project demonstrates the configuration of **DHCP (Dynamic Host Configuration Protocol)** on a Cisco router using Cisco Packet Tracer.

The topology contains two separate LANs connected through a router. The router is configured with two DHCP pools, **LAN-01** and **LAN-02**, to automatically assign IP addresses and other network information to the PCs in each network.

## 🌐 Network Topology

The network consists of:

- 1 Cisco Router
- 2 Cisco Switches
- 6 PCs
- 2 Different LAN Networks
- 2 DHCP Pools


### Router 0   (To check the configuration)
- enable = Cisco123
- line console = Cisco 123
- username = Cisco    secret = Cisco123 


### LAN-01

| Configuration | Value |
|---|---|
| Network ID | `192.168.1.0/24` |
| Subnet Mask | `255.255.255.0` |
| DHCP Pool | `LAN-01` |
| Devices | PC0, PC1, PC2 |

### LAN-02

| Configuration | Value |
|---|---|
| Network ID | `172.16.10.0/24` |
| Subnet Mask | `255.255.255.0` |
| DHCP Pool | `LAN-02` |
| Devices | PC3, PC4, PC5 |

## ⚙️ DHCP Configuration

Two DHCP pools are configured on the router.

### DHCP Pool for LAN-01 and LAN-02

```cisco
Router(config)# ip dhcp pool LAN-01
Router(dhcp-config)# network 192.168.1.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.1.1

```cisco
Router(config)# ip dhcp pool LAN-02
Router(dhcp-config)# network 172.16.1.0 255.255.255.0
Router(dhcp-config)# default-router 172.16.1.1
