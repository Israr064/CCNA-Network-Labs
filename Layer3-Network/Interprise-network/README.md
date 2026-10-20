
# Redundant Campus Network: HSRP, Inter-VLAN Routing & DHCP (Cisco Packet Tracer)

A three-tier campus network lab built in Cisco Packet Tracer. Two Layer 3 distribution switches provide **first-hop redundancy with HSRP**, **inter-VLAN routing with SVIs**, and **DHCP**, while an edge router connects the campus to the upstream network.



| Layer | Devices |
|-------|---------|
| Edge | `EDGE-ROUTER` |
| Distribution (Layer 3) | `D-SW1`, `D-SW2` |
| Access (Layer 2) | `S-A`, `S-B`, `S-C` |
| End devices | `PC0` to `PC8` (3 per access switch) |

**Connectivity**

- `EDGE-ROUTER` connects to both `D-SW1` and `D-SW2` via routed point-to-point links.
- `D-SW1` and `D-SW2` are interconnected by two links (EtherChannel / trunk).
- Every access switch is dual-homed: one uplink to each distribution switch.
- Each access switch serves three PCs.

## Features

- **VLAN segmentation**: one VLAN per access switch
- **Inter-VLAN routing** using SVIs on the Layer 3 switches
- **HSRP v2** for default gateway redundancy, with per-VLAN load balancing
- **STP root alignment** with the HSRP active switch
- **DHCP** served from the distribution layer
- **Routed uplinks** to the edge router

## IP Addressing Plan

### VLANs and HSRP

| VLAN | Name | Hosts | Subnet | D-SW1 SVI | D-SW2 SVI | HSRP Virtual IP (Gateway) | HSRP Active |
|------|------|-------|--------|-----------|-----------|---------------------------|-------------|
| 10 | S-A | PC0, PC1, PC2 | 192.168.10.0/24 | 192.168.10.2 | 192.168.10.3 | 192.168.10.1 | D-SW1 |
| 20 | S-B | PC3, PC4, PC5 | 192.168.20.0/24 | 192.168.20.2 | 192.168.20.3 | 192.168.20.1 | D-SW2 |
| 30 | S-C | PC6, PC7, PC8 | 192.168.30.0/24 | 192.168.30.2 | 192.168.30.3 | 192.168.30.1 | D-SW1 |

### Routed Links to the Edge Router

| Link | Router IP | Distribution Switch IP | Subnet |
|------|-----------|------------------------|--------|
| EDGE-ROUTER to D-SW1 | 10.0.1.1 | 10.0.1.2 | 10.0.1.0/30 |
| EDGE-ROUTER to D-SW2 | 10.0.2.1 | 10.0.2.2 | 10.0.2.0/30 |

### DHCP Ranges (split scope for redundancy)

| VLAN | D-SW1 serves | D-SW2 serves |
|------|--------------|--------------|
| 10 | 192.168.10.100 - .150 | 192.168.10.151 - .200 |
| 20 | 192.168.20.100 - .150 | 192.168.20.151 - .200 |
| 30 | 192.168.30.100 - .150 | 192.168.30.151 - .200 |

The ranges do not overlap, so either switch can answer a DHCP request without handing out duplicate addresses.

## Configuration

> Interface names below are examples. Adjust them to match the ports used in your own topology.

### 1. VLANs (D-SW1, D-SW2 and all access switches)

```
vlan 10
 name S-A
vlan 20
 name S-B
vlan 30
 name S-C
```

### 2. Access Switch Ports (example for S-A)

```
interface range fastethernet 0/1 - 3
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast

interface range gigabitethernet 0/1 - 2
 switchport mode trunk
```

Repeat on S-B (VLAN 20) and S-C (VLAN 30). On Layer 3 switches that support it, add `switchport trunk encapsulation dot1q` before `switchport mode trunk`.

### 3. Inter-Switch Link between D-SW1 and D-SW2

```
interface range gigabitethernet 1/0/23 - 24
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode active
```

### 4. Spanning Tree (root aligned with HSRP active)

**D-SW1**
```
spanning-tree vlan 10,30 root primary
spanning-tree vlan 20 root secondary
```

**D-SW2**
```
spanning-tree vlan 20 root primary
spanning-tree vlan 10,30 root secondary
```

### 5. SVIs and HSRP

**D-SW1**
```
ip routing

interface vlan 10
 ip address 192.168.10.2 255.255.255.0
 standby version 2
 standby 10 ip 192.168.10.1
 standby 10 priority 110
 standby 10 preempt
 no shutdown

interface vlan 20
 ip address 192.168.20.2 255.255.255.0
 standby version 2
 standby 20 ip 192.168.20.1
 standby 20 priority 100
 standby 20 preempt
 no shutdown

interface vlan 30
 ip address 192.168.30.2 255.255.255.0
 standby version 2
 standby 30 ip 192.168.30.1
 standby 30 priority 110
 standby 30 preempt
 no shutdown
```

**D-SW2**
```
ip routing

interface vlan 10
 ip address 192.168.10.3 255.255.255.0
 standby version 2
 standby 10 ip 192.168.10.1
 standby 10 priority 100
 standby 10 preempt
 no shutdown

interface vlan 20
 ip address 192.168.20.3 255.255.255.0
 standby version 2
 standby 20 ip 192.168.20.1
 standby 20 priority 110
 standby 20 preempt
 no shutdown

interface vlan 30
 ip address 192.168.30.3 255.255.255.0
 standby version 2
 standby 30 ip 192.168.30.1
 standby 30 priority 100
 standby 30 preempt
 no shutdown
```

### 6. Routed Uplinks to EDGE-ROUTER

**D-SW1**
```
interface gigabitethernet 1/0/1
 no switchport
 ip address 10.0.1.2 255.255.255.252
 no shutdown
```

**D-SW2**
```
interface gigabitethernet 1/0/1
 no switchport
 ip address 10.0.2.2 255.255.255.252
 no shutdown
```

**EDGE-ROUTER**
```
interface gigabitethernet 0/0
 ip address 10.0.1.1 255.255.255.252
 no shutdown

interface gigabitethernet 0/1
 ip address 10.0.2.1 255.255.255.252
 no shutdown
```

### 7. Routing (OSPF, so the router learns the VLAN subnets)

**D-SW1 and D-SW2**
```
router ospf 1
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0
 network 192.168.30.0 0.0.0.255 area 0
 network 10.0.1.0 0.0.0.3 area 0     ! use 10.0.2.0 on D-SW2
 passive-interface vlan 10
 passive-interface vlan 20
 passive-interface vlan 30
```

**EDGE-ROUTER**
```
router ospf 1
 network 10.0.1.0 0.0.0.3 area 0
 network 10.0.2.0 0.0.0.3 area 0
```

Static routes work just as well if you prefer to avoid a routing protocol.

### 8. DHCP

**D-SW1**
```
ip dhcp excluded-address 192.168.10.1 192.168.10.99
ip dhcp excluded-address 192.168.10.151 192.168.10.254
ip dhcp excluded-address 192.168.20.1 192.168.20.99
ip dhcp excluded-address 192.168.20.151 192.168.20.254
ip dhcp excluded-address 192.168.30.1 192.168.30.99
ip dhcp excluded-address 192.168.30.151 192.168.30.254

ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
ip dhcp pool VLAN20
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.8.8
ip dhcp pool VLAN30
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 dns-server 8.8.8.8
```

**D-SW2**: same pools, but exclude `.1 - .150` and `.201 - .254` in each subnet so it only serves `.151 - .200`.

`default-router` always points to the **HSRP virtual IP**, never to a real SVI address.

## Verification

```
show standby brief
show ip interface brief
show vlan brief
show spanning-tree summary
show etherchannel summary
show ip route
show ip dhcp binding
```

**Test plan**

1. PCs obtain addresses via DHCP and can ping their gateway (`.1`).
2. A PC in VLAN 10 pings a PC in VLAN 20 and VLAN 30 (inter-VLAN routing).
3. Run `show standby brief` on both switches and confirm the expected Active/Standby roles per VLAN.
4. Shut down D-SW1 (or its uplinks). The standby role should take over and pings should continue with only a brief interruption.
5. Bring D-SW1 back up. Preemption returns the Active role to it.
6. Ping the edge router (`10.0.1.1`, `10.0.2.1`) from a PC to confirm upstream reachability.

## Design Notes

- **One VLAN = one SVI = one subnet = one HSRP group.** SVIs and HSRP are configured on **both** distribution switches.
- **Per-VLAN load balancing**: VLANs 10 and 30 are active on D-SW1, VLAN 20 on D-SW2, so both distribution switches carry traffic.
- **STP root matches HSRP active** per VLAN to avoid suboptimal traffic paths across the inter-switch link.
- **HSRP preempt** is enabled so the preferred switch regains the Active role after recovering.
- **Split DHCP scopes** avoid duplicate IPs while keeping DHCP available if one switch fails.

## Requirements

- Cisco Packet Tracer 8.x
- Layer 3 switches (e.g. Catalyst 3560-24PS or 3650) for D-SW1 and D-SW2
- Layer 2 switches (e.g. 2960) for S-A, S-B and S-C
- A router for EDGE-ROUTER (e.g. ISR 4331 / 2911)

## Repository Structure

```
.
├── README.md
├── topology.png
├── hsrp-lab.pkt
└── configs/
    ├── D-SW1.txt
    ├── D-SW2.txt
    ├── S-A.txt
    ├── S-B.txt
    ├── S-C.txt
    └── EDGE-ROUTER.txt
```

## Author

Your Name: [GitHub Profile](https://github.com/your-username)
