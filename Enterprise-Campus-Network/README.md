# Redundant Enterprise Campus Network

## Project Overview

Designed and implemented a redundant enterprise campus network using Cisco Packet Tracer.

This project simulates a real-world collapsed core enterprise architecture focused on:

- High availability
- Network segmentation
- Secure management
- Dynamic routing
- Centralized services
- Troubleshooting methodology

The network was designed to provide redundancy, scalability, and reliable user connectivity.

---

# Network Topology

![Enterprise Campus Network](Topology/Enterprise_Topology.png)

## Architecture

The network consists of:

- ISR Router for WAN/ISP connectivity
- Edge Router for internet boundary routing, NAT/PAT, and ACL filtering
- Dual Layer 3 Core Switches for routing and redundancy
- Dual Layer 2 Access Switches for user connectivity
- Centralized DHCP Server

Design Features:

- Collapsed core architecture
- Redundant core-to-access connections
- HSRP gateway redundancy
- OSPF dynamic routing
- VLAN segmentation

---

# Technologies Implemented

## Switching

- VLAN segmentation
- Access and trunk ports
- 802.1Q trunking
- Rapid PVST+
- EtherChannel concepts
- Port Security

## Layer 3 Routing

- Inter-VLAN routing using SVIs
- Static routing
- Default routing
- OSPF Area 0
- Point-to-point OSPF links
- HSRP first-hop redundancy

## Network Services

- DHCP server configuration
- DHCP relay (`ip helper-address`)
- NAT/PAT
- DNS concepts

## Security

- ACL filtering
- SSH remote management
- Password encryption
- Switch hardening

---

# VLAN Design

| VLAN | Department |
|---|---|
|10|IT|
|20|HR|
|30|Finance|
|40|Sales|
|50|Engineering|
|60|Guest|
|90|Management|
|100|Servers|

---

# Verification & Testing

Network operation was verified using Layer 1-3 troubleshooting methodology.

Verified:

### Layer 1
- Physical connectivity
- Interface status

### Layer 2
- VLAN configuration
- Trunks
- STP operation
- Port security

### Layer 3
- SVI gateways
- Routing tables
- OSPF neighbors
- HSRP status

### Services

- DHCP address assignment
- NAT translations
- ACL operation
- End-to-end connectivity

Example verification commands:
show vlan brief
show interfaces trunk
show spanning-tree
show standby brief
show ip ospf neighbor
show ip route
show ip nat translations


---

# Troubleshooting & Lessons Learned

Documented troubleshooting scenarios include:

- Trunk configuration issues
- HSRP adjacency failures
- DHCP relay problems
- Incorrect static routes
- OSPF route installation issues
- NAT configuration issues
- Layer 3 addressing mistakes

Detailed troubleshooting documentation:

`Documentation/Troubleshooting.md`

---

# Project Skills Demonstrated

- Enterprise network design
- Cisco IOS configuration
- VLAN implementation
- Layer 2/Layer 3 switching
- Routing protocols
- High availability design
- Network security
- Infrastructure services
- Network verification and troubleshooting

---

# Future Improvements

- Firewall implementation
- AAA authentication
- Network monitoring
- Syslog/SNMP
- DHCP failover
- Enterprise wireless deployment
