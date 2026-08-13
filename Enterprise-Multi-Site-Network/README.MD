# Enterprise Multi-Site Network

A Cisco enterprise network designed in **Cisco Packet Tracer**, connecting a headquarters site with two branch offices through redundant ISP paths.

## Network Overview

**Sites**

* HQ / Core Network
* Branch 1
* Branch 2
* Primary ISP
* Backup ISP

### Technologies Implemented

* VLAN segmentation
* Inter-VLAN routing
* HSRP gateway redundancy
* OSPF dynamic routing
* GRE tunnels
* Primary/backup WAN connectivity
* NAT/PAT
* Centralized TFTP, SYSLOG, DNS, DHCP with DHCP relay
* SSH management
* Extended and standard ACLs
* Port security
* Rapid-PVST
* EtherChannel/LACP
* BPDU Guard & PortFast
* NTP and centralized Syslog
* Management VLAN

## IP Addressing Plan

| Site     |  VLAN | Network       | Gateway    |
| -------- | ----: | ------------- | ---------- |
| HQ       |    10 | 10.10.10.0/26 | 10.10.10.1 |
| HQ       |    20 | 10.10.20.0/26 | 10.10.20.1 |
| HQ       |    30 | 10.10.30.0/25 | 10.10.30.1 |
| HQ       |    40 | 10.10.40.0/26 | 10.10.40.1 |
| HQ       |    50 | 10.10.50.0/25 | 10.10.50.1 |
| HQ       |    60 | 10.10.60.0/24 | 10.10.60.1 |
| HQ       |    70 | 10.10.70.0/26 | 10.10.70.1 |
| HQ       |    80 | 10.10.80.0/29 | 10.10.80.1 |
| HQ       |    90 | 10.10.90.0/27 | 10.10.90.1 |
| Branch 1 | 10–90 | 10.20.x.0     | 10.20.x.1  |
| Branch 2 | 10–90 | 10.30.x.0     | 10.30.x.1  |

**Infrastructure networks**

| Purpose        | Network                            |
| -------------- | ---------------------------------- |
| HQ–ISP1        | 200.0.0.0/30                       |
| Branch 1–ISP1  | 200.0.0.4/30                       |
| Branch 2–ISP1  | 200.0.0.8/30                       |
| Backup WAN     | 192.0.0.0/30 and related /30 links |
| GRE tunnels    | 172.16.x.0/30                      |
| OSPF Core Link | 10.255.0.0/30                      |

## Routing

OSPF Area 0 provides dynamic routing between the core and branch networks.

GRE tunnels provide logical WAN connectivity between the branch routers and HQ/core routing infrastructure.

Primary and floating static default routes provide WAN failover through the backup ISP.

## Redundancy

HSRP provides redundant default gateways for user VLANs.

* Core 1 uses higher HSRP priority for selected VLANs.
* Branch routers provide HSRP gateways for their local VLANs.
* Primary ISP paths are preferred.
* Backup paths provide failover.

## Security

The network includes:

* SSH-only remote management
* Restricted management access using `SSH-MGMT`
* `SERVER-PROTECTION` ACL protecting the centralized server
* Port security with sticky MAC addresses
* BPDU Guard
* PortFast
* Unused ports placed into VLAN 999 and shutdown
* Management VLAN 90
* Centralized logging and NTP

## DHCP & Services

The centralized server at **10.10.80.4** provides network services.

DHCP relay is configured on the Layer 3 VLAN interfaces using:

```text
ip helper-address 10.10.80.4
```

The server is located in the HQ server/services VLAN.

## NAT

NAT/PAT is configured at the WAN edge to provide Internet connectivity for internal networks.

Primary and backup WAN paths are used to provide connectivity and failover.

## Verification

Useful verification commands:

```text
show ip route
show ip ospf neighbor
show ip ospf interface
show standby
show ip nat translations
show ip nat statistics
show access-lists
show port-security
show etherchannel summary
show interfaces trunk
show spanning-tree
```

## Project Goals

This project demonstrates practical enterprise networking concepts including **routing, redundancy, segmentation, WAN connectivity, NAT, security, and network management** in a multi-site environment.

## Topology

Add a screenshot of the final Packet Tracer topology here:

`/images/network-topology.png`

---

### Skills Demonstrated

**Cisco IOS • DNS • TFTP • OSPF • HSRP • GRE • VLANs • Inter-VLAN Routing • NAT/PAT • DHCP Relay • ACLs • SSH • Port Security • STP • EtherChannel • Syslog • NTP • WAN Redundancy**

