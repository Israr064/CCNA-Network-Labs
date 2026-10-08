
# 🖧 Three-Tier Campus Network — Cisco Packet Tracer

## 📌 Project Overview

This project is a **three-tier campus network topology** designed in **Cisco Packet Tracer**. It combines the networking concepts studied so far into one practical topology.

The topology is divided into:

* **Core Layer** — Router R1
* **Distribution Layer** — Switch SW1
* **Access Layer** — Switches SW2 and SW3

The project demonstrates **Basic Device Configuration, VLANs, VTP, DTP, Trunking, and Inter-VLAN Routing using Router-on-a-Stick**.
Username Cisco and all password = cisco

---

## 🏗️ Network Topology

```text
                         CORE LAYER
                            R1
                     Router-on-a-Stick
                            │
                         TRUNK
                            │
                    DISTRIBUTION LAYER
                           SW1
                       VTP Server
                       /         \
                   TRUNK         TRUNK
                    /               \
                   /                 \
             ACCESS LAYER       ACCESS LAYER
                 SW2                 SW3
             VTP Client           VTP Client
             /   |   \             /   |   \
           PC1  PC2  PC3         PC4  PC5  PC6
           V10  V20  V30         V10  V20  V30
```

---

## 🏢 Network Layers

### 🔵 Core Layer

**Device:** R1

Responsibilities:

* Router-on-a-Stick
* Inter-VLAN Routing
* Default Gateway for VLANs
* Routing between different VLANs

R1 uses subinterfaces for the VLANs:

| Interface | VLAN | IP Address   |
| --------- | ---: | ------------ |
| G0/0.10   |   10 | 192.168.10.1 |
| G0/0.20   |   20 | 192.168.20.1 |
| G0/0.30   |   30 | 192.168.30.1 |
| G0/0.99   |   99 | 192.168.99.1 |

---

### 🟢 Distribution Layer

**Device:** SW1

SW1 acts as the central distribution switch.

Responsibilities:

* VTP Server
* VLAN creation
* Trunking
* DTP
* Connecting the Access Layer to the Router

SW1 is configured as:

```text
VTP Mode: Server
VTP Domain: COMPANY
```

---

### 🟣 Access Layer

**Devices:** SW2 and SW3

Both switches act as **VTP Clients**.

Responsibilities:

* Connecting end devices
* Assigning access ports to VLANs
* Receiving VLAN information through VTP
* Connecting to the Distribution Layer through trunk links

---

## 🏷️ VLAN Configuration

| VLAN ID | Name       | Purpose                |
| ------: | ---------- | ---------------------- |
|      10 | HR         | Human Resources        |
|      20 | ADMIN      | Administration         |
|      30 | IT         | Information Technology |
|      99 | MANAGEMENT | Switch Management      |

---

## 💻 PC Addressing

### VLAN 10 — HR

| Device | IP Address    | Gateway      |
| ------ | ------------- | ------------ |
| PC1    | 192.168.10.10 | 192.168.10.1 |
| PC4    | 192.168.10.11 | 192.168.10.1 |

### VLAN 20 — ADMIN

| Device | IP Address    | Gateway      |
| ------ | ------------- | ------------ |
| PC2    | 192.168.20.10 | 192.168.20.1 |
| PC5    | 192.168.20.11 | 192.168.20.1 |

### VLAN 30 — IT

| Device | IP Address    | Gateway      |
| ------ | ------------- | ------------ |
| PC3    | 192.168.30.10 | 192.168.30.1 |
| PC6    | 192.168.30.11 | 192.168.30.1 |

Subnet mask for all VLANs:

```text
255.255.255.0
```

---

## 🔗 Trunk Links

The following links carry multiple VLANs:

```text
R1 ↔ SW1
SW1 ↔ SW2
SW1 ↔ SW3
```

The trunk links carry:

```text
VLAN 10
VLAN 20
VLAN 30
VLAN 99
```

---

## 🔄 VTP Configuration

The topology uses **VTP** to distribute VLAN information.

### SW1

```text
VTP Mode: Server
VTP Domain: COMPANY
```

### SW2

```text
VTP Mode: Client
VTP Domain: COMPANY
```

### SW3

```text
VTP Mode: Client
VTP Domain: COMPANY
```

VLANs are created on SW1 and propagated to the VTP clients.

---

## 🔀 DTP Configuration

DTP is used to dynamically negotiate trunk links between switches.

Example:

```text
SW1 ↔ SW2
SW1 ↔ SW3
```

The trunk links can be configured using DTP modes such as:

```text
switchport mode dynamic desirable
```

and:

```text
switchport mode dynamic auto
```

---

## 🌐 Router-on-a-Stick

Router-on-a-Stick is used to provide **Inter-VLAN Routing**.

R1 uses subinterfaces:

```text
G0/0.10 → VLAN 10
G0/0.20 → VLAN 20
G0/0.30 → VLAN 30
G0/0.99 → VLAN 99
```

Each subinterface acts as the default gateway for its VLAN.

For example:

```text
PC1
192.168.10.10
       │
       ↓
Gateway: 192.168.10.1
       │
       ↓
R1 G0/0.10
```

This allows devices in different VLANs to communicate.

---

## 🧪 Connectivity Testing

The following tests should be successful:

### Same VLAN

```text
PC1 → PC4
```

Both devices belong to VLAN 10.

### Inter-VLAN

```text
PC1 → PC2
PC2 → PC3
PC1 → PC3
```

These tests verify Router-on-a-Stick Inter-VLAN Routing.

### Gateway Testing

```text
PC1 → 192.168.10.1
PC2 → 192.168.20.1
PC3 → 192.168.30.1
```

---

## 🔍 Verification Commands

### Check VLANs

```text
show vlan brief
```

### Check Trunking

```text
show interfaces trunk
```

### Check VTP

```text
show vtp status
```

### Check Interface Configuration

```text
show interfaces switchport
```

### Check Router Interfaces

```text
show ip interface brief
```

### Test Connectivity

```text
ping <destination-ip>
```

---

## 🎯 Concepts Demonstrated

This project demonstrates the following CCNA concepts:

* ✅ Basic Cisco device configuration
* ✅ VLAN creation
* ✅ VLAN assignment
* ✅ Access ports
* ✅ Trunk ports
* ✅ DTP
* ✅ VTP
* ✅ VTP Server and Client
* ✅ Router-on-a-Stick
* ✅ Inter-VLAN Routing
* ✅ Default Gateways
* ✅ IP Addressing
* ✅ Network troubleshooting
* ✅ `ping` and Cisco verification commands

---

## 📚 Learning Objective

The main objective of this project is to understand how multiple Cisco networking concepts work together in a practical campus network.

Instead of configuring each technology separately, this topology combines **VLANs, VTP, DTP, trunking, and Router-on-a-Stick** into a single network.

---

## 🛠️ Software Used

**Cisco Packet Tracer**

---

## 👨‍💻 Project Structure

```text
Three-Tier-Campus-Network/
│
├── README.md
│
└── Campus_Network.pkt
```

---

## 🚀 Future Improvements

The topology can later be expanded by adding:

* DHCP
* SSH
* Port Security
* EtherChannel
* STP
* Static Routing
* OSPF
* AAA
* ACLs
* Redundant Distribution/Core switches

---

## 📌 Conclusion

This project provides a practical implementation of a **three-tier campus network** using Cisco Packet Tracer.

The network demonstrates how the **Core, Distribution, and Access layers** work together while implementing the major CCNA topics studied in this project.
