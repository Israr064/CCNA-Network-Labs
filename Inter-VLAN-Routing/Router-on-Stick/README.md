# VLAN, Trunking & Inter-VLAN Routing Project

## Project Overview

This project demonstrates a small enterprise network designed using **VLAN segmentation, trunk links, and Router-on-a-Stick inter-VLAN routing** in Cisco Packet Tracer.

The network is divided into separate VLANs for **HR, Admin, and IT departments**. VLANs logically separate users even when they are connected to the same physical switching infrastructure. Trunk links are configured between switches to carry traffic from multiple VLANs across a single physical link.

A router is connected to the switch through a trunk link and performs **Inter-VLAN Routing** using subinterfaces. This allows devices in different VLANs to communicate with each other while maintaining logical network separation.

## Network Design

| VLAN    | Department | Ports         | Network         |
| ------- | ---------- | ------------- | --------------- |
| VLAN 10 | HR         | F0/1 - F0/4   | 192.168.10.0/24 |
| VLAN 20 | Admin      | F0/5 - F0/9   | 192.168.20.0/24 |
| VLAN 30 | IT         | F0/10 - F0/15 | 192.168.30.0/24 |
| VLAN 99 | Management | Management    | 192.168.99.0/24 |

## PC Addressing

| Switch | PC  |            VLAN | IP Address    |
| ------ | --- | --------------: | ------------- |
| SW1    | PC1 |    VLAN 10 - HR | 192.168.10.10 |
| SW1    | PC2 | VLAN 20 - Admin | 192.168.20.10 |
| SW1    | PC3 |    VLAN 30 - IT | 192.168.30.10 |
| SW2    | PC4 |    VLAN 10 - HR | 192.168.10.11 |
| SW2    | PC5 | VLAN 20 - Admin | 192.168.20.11 |
| SW2    | PC6 |    VLAN 30 - IT | 192.168.30.11 |

## Management VLAN

**VLAN 99** is configured as the Management VLAN for managing the switches remotely.

| Switch | Management VLAN | SVI IP       |
| ------ | --------------: | ------------ |
| SW0    |         VLAN 99 | 192.168.99.1 |
| SW1    |         VLAN 99 | 192.168.99.3 |
| SW2    |         VLAN 99 | 192.168.99.2 |

## How the Network Works

### 1. VLAN Creation

Separate VLANs are created for each department:

* **VLAN 10 → HR**
* **VLAN 20 → Admin**
* **VLAN 30 → IT**
* **VLAN 99 → Management**

Access ports are assigned to the appropriate VLAN according to the department of the connected PC.

For example, a PC connected to an access port in VLAN 10 becomes part of the HR network.

### 2. Trunking

Trunk ports are configured between the switches and between the switch and router.

A trunk link can carry traffic from **multiple VLANs over one physical connection**. VLAN tags are used to identify which VLAN each frame belongs to.

For example, traffic from VLAN 10, VLAN 20, and VLAN 30 can travel through the same trunk link while remaining logically separated.

### 3. Router-on-a-Stick

The router performs Inter-VLAN Routing using the **Router-on-a-Stick** method.

Instead of using a separate physical router interface for every VLAN, a single physical router interface is configured with multiple **subinterfaces**.

Conceptually:

```text
Router
   |
   | Trunk
   |
Switch
 ├── VLAN 10 (HR)
 ├── VLAN 20 (Admin)
 ├── VLAN 30 (IT)
 └── VLAN 99 (Management)
```

Each VLAN has a corresponding router subinterface that acts as its default gateway.

For example:

```text
VLAN 10 → 192.168.10.1
VLAN 20 → 192.168.20.1
VLAN 30 → 192.168.30.1
```

Therefore:

* HR PCs use `192.168.10.1` as their gateway.
* Admin PCs use `192.168.20.1` as their gateway.
* IT PCs use `192.168.30.1` as their gateway.

### 4. Inter-VLAN Communication

By default, VLANs are separate Layer 2 broadcast domains. Therefore, a PC in VLAN 10 cannot directly communicate with a PC in VLAN 20 at Layer 2.

When PC1 in VLAN 10 wants to communicate with PC2 in VLAN 20:

```text
PC1
 ↓
Access Port
 ↓
VLAN 10
 ↓
Switch
 ↓
Trunk Link
 ↓
Router
 ↓
VLAN 20 Subinterface
 ↓
Trunk Link
 ↓
Switch
 ↓
VLAN 20
 ↓
PC2
```

The router receives the traffic from VLAN 10, performs Layer 3 routing, and forwards it toward VLAN 20.

This is how **Inter-VLAN Routing** allows devices from different VLANs to communicate.

## Technologies Used

* Cisco Packet Tracer
* VLAN
* Access Ports
* Trunk Ports
* 802.1Q VLAN Tagging
* Router-on-a-Stick
* Inter-VLAN Routing
* Switch Virtual Interface (SVI)
* Management VLAN
* Basic Switch Security

## Security Configuration

Basic switch security was also configured using:

* Enable Secret
* Line/Console Password
* Management VLAN

This provides basic protection for administrative access to the network devices.

## Project Objective

The main objective of this project is to understand how a real-world departmental network can be designed using **VLAN segmentation** and how communication between different VLANs can be achieved through **Router-on-a-Stick Inter-VLAN Routing**.

The project demonstrates the complete flow of traffic from an end device through an access port and trunk link to the router, where Layer 3 routing takes place before the traffic is forwarded to the destination VLAN.
