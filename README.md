# Smart IoT & PC Network — Cisco Packet Tracer

## 1. Project Overview

This project implements a small network in Cisco Packet Tracer with two separate networks:

- An **IoT network** using `192.168.10.0/24`
- A **PC network** using `192.168.20.0/24`

A router connects the two networks. The IoT side contains an IoT Server and an RFID-based access-control system, while the PC side contains six PCs with DHCP enabled for future scalability.

---

## 2. Network Topology

```text
                              ROUTER
                             /      \
                            /        \
                   server_switch    pc_switch
                     /       \          |
                    /         \         ├── PC1
             IoT Server     iot_switch  ├── PC2
             (Server-PT)        |       ├── PC3
                                ├──     ├── PC4
                                ├──     ├── PC5
                                ├──     └── PC6
                                |
                         ┌──────┼──────┐
                         │      │      │
                       6 Doors 6 RFID 6 Lights
```

---

## 3. IP Addressing

### IoT Network

```text
Network: 192.168.10.0/24
DHCP: Enabled
Purpose: IoT devices and IoT Server
```

The IoT network contains:

- IoT Server (`Server-PT`)
- 6 RFID readers
- 6 doors
- 6 lights

### PC Network

```text
Network: 192.168.20.0/24
DHCP: Enabled
Purpose: PC devices
```

The PC network currently contains six PCs.

DHCP is used so that additional PCs can be added later without manually assigning an IP address to every device.

---

## 4. Router

The router is the central Layer-3 device connecting the two networks.

```text
IoT Network → Router → PC Network
192.168.10.0/24       192.168.20.0/24
```

The router is required because the IoT devices and PCs belong to different IP networks. The router allows traffic to be routed between the two networks.

---

## 5. Switches

### server_switch

`server_switch` connects the IoT Server and the `iot_switch` to the router.

```text
Router
  |
server_switch
  ├── IoT Server (Server-PT)
  └── iot_switch
```

### iot_switch

`iot_switch` is the main switch for the IoT devices.

Connected devices:

- 6 doors
- 6 RFID readers
- 6 lights

All of these devices use the IoT network.

### pc_switch

`pc_switch` connects the six PCs.

```text
Router
   |
pc_switch
   ├── PC1
   ├── PC2
   ├── PC3
   ├── PC4
   ├── PC5
   └── PC6
```

The PC network uses DHCP to provide automatic addressing and allow future expansion.

---

## 6. IoT Server

The IoT Server is a `Server-PT` device and acts as the central control point for the IoT system.

It contains the automation rules used to control the RFID readers, doors, and lights.

The IoT Server communicates with the IoT devices through:

```text
192.168.10.0/24
```

---

## 7. RFID Access-Control System

The system contains six RFID readers. Each reader has one valid Card ID and controls one corresponding door and light.

| RFID Reader | Valid Card ID | Door | Light |
|---|---:|---|---|
| reader1 | `1001` | Door1 | Light1 |
| reader2 | `1002` | Door2 | Light2 |
| reader3 | `1003` | Door3 | Light3 |
| reader4 | `1004` | Door4 | Light4 |
| reader5 | `1005` | Door5 | Light5 |
| reader6 | `1006` | Door6 | Light6 |

---

## 8. IoT Rules

Each RFID reader has two validation rules.

### RFID Validation

For each reader:

```text
Correct Card ID
    ↓
RFID Status = Valid

Incorrect Card ID
    ↓
RFID Status = Invalid
```

The configured valid IDs are:

```text
reader1 → 1001
reader2 → 1002
reader3 → 1003
reader4 → 1004
reader5 → 1005
reader6 → 1006
```

### Door and Light Control

When an RFID reader becomes **Valid**:

```text
Door → Unlock
Light → ON
```

When an RFID reader becomes **Invalid**:

```text
Door → Lock
Light → OFF
```

The complete relationship is:

| Reader | Valid Condition | Valid Action | Invalid Action |
|---|---|---|---|
| reader1 | Card ID = `1001` | Door1 unlock + Light1 ON | Door1 lock + Light1 OFF |
| reader2 | Card ID = `1002` | Door2 unlock + Light2 ON | Door2 lock + Light2 OFF |
| reader3 | Card ID = `1003` | Door3 unlock + Light3 ON | Door3 lock + Light3 OFF |
| reader4 | Card ID = `1004` | Door4 unlock + Light4 ON | Door4 lock + Light4 OFF |
| reader5 | Card ID = `1005` | Door5 unlock + Light5 ON | Door5 lock + Light5 OFF |
| reader6 | Card ID = `1006` | Door6 unlock + Light6 ON | Door6 lock + Light6 OFF |

---

## 9. Example of System Operation

### Valid RFID card

For example, if `reader1` receives Card ID `1001`:

```text
Card ID = 1001
       ↓
reader1 = Valid
       ↓
Door1 = Unlocked
Light1 = ON
```

### Invalid RFID card

If `reader1` receives a Card ID other than `1001`:

```text
Card ID ≠ 1001
       ↓
reader1 = Invalid
       ↓
Door1 = Locked
Light1 = OFF
```

The same logic applies independently to all six RFID readers.

---

## 10. DHCP

Two separate DHCP-based networks are used.

### IoT DHCP Network

```text
192.168.10.0/24
```

The IoT devices obtain their network configuration through DHCP.

### PC DHCP Network

```text
192.168.20.0/24
```

The six PCs use DHCP for automatic addressing.

Using DHCP makes the network easier to expand because new devices can obtain their network configuration automatically instead of requiring manual IP assignment.

---

## 11. Why the Devices Are Connected This Way

The network is divided into functional sections:

### Router
Connects the IoT and PC networks and routes traffic between them.

### server_switch
Connects the IoT Server and IoT device network to the router.

### IoT Server
Provides centralized control and contains the IoT automation rules.

### iot_switch
Connects the six RFID readers, six doors, and six lights.

### pc_switch
Connects the six PCs and provides access to the DHCP-based PC network.

This organization keeps the IoT and PC sections logically separated while still allowing communication through the router.

---

## 12. Network Summary

| Component | Network | Purpose |
|---|---|---|
| IoT Network | `192.168.10.0/24` | IoT devices + IoT Server |
| PC Network | `192.168.20.0/24` | PCs |
| server_switch | IoT network | Server and IoT switch connectivity |
| iot_switch | IoT network | IoT device connectivity |
| pc_switch | PC network | PC connectivity |
| Router | Both networks | Routing between networks |
| IoT Server | IoT network | Central IoT control |

---

## 13. Project Outcome

The project demonstrates a networked smart access-control system using Cisco Packet Tracer.

The router connects two separate IP networks, while switches provide local connectivity. The IoT Server centrally manages six RFID-based access-control systems. Each RFID reader validates a specific Card ID and controls its corresponding door and light.

A valid card unlocks the corresponding door and turns its light on. An invalid card keeps the door locked and the light off.

The PC network uses DHCP to simplify network configuration and provide scalability for adding additional devices in the future.
