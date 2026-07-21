# Enterprise Campus Network

## Overview

Designed and implemented a redundant two-tier enterprise campus network using Cisco Packet Tracer.

The design simulates a small-to-medium enterprise environment with:

- Access layer switches
- Redundant Layer 3 core switches
- Edge router
- ISP router
- Dedicated DHCP server

---

## Topology

![Topology](diagrams/topology.png)

---

## Architecture

The network uses a collapsed core design where the Layer 3 core switches provide both core and distribution functionality.

Core responsibilities:

- Inter-VLAN routing
- HSRP gateway redundancy
- OSPF routing
- DHCP relay
- Default routing

Access responsibilities:

- User connectivity
- VLAN assignment
- Port security
- SSH management

Edge responsibilities:

- NAT/PAT
- ACL filtering
- Internet boundary routing

---

## Technologies Implemented

### VLAN Segmentation

Created departmental VLANs:

| VLAN | Department |
|-|-|
|10|IT|
|20|HR|
|30|Finance|
|40|Sales|
|50|Engineering|
|60|Guest|
|90|Management|
|100|Servers|

---

### Routing

Implemented:

- OSPF Area 0
- Default route propagation
- Layer 3 routed links

Verification:

