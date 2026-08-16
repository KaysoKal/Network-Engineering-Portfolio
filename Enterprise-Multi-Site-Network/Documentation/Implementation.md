# Network Infrastructure Project

## Overview

This project implements a secure, redundant, and scalable multi-site network connecting **HQ and two branch offices**. The design includes Layer 2 segmentation, Layer 3 routing, HSRP gateway redundancy, dual-ISP WAN connectivity, GRE tunnels, OSPF dynamic routing, centralized network services, NAT/PAT, and security controls.

The network was configured and verified incrementally, with each technology tested individually before completing end-to-end and failover testing.

---

## 1. Basic Device Configuration

Configured all network devices with a consistent baseline before implementing network services.

### Tasks Completed

* Assigned hostnames
* Disabled DNS lookup
* Configured secure passwords
* Enabled password encryption
* Configured SSH remote management
* Generated RSA keys
* Configured login banners
* Saved device configurations

---

## 2. Layer 2 Configuration

Configured the access layer to provide connectivity for end devices at HQ and both branch offices.

### Tasks Completed

* Created departmental VLANs
* Assigned access ports
* Configured 802.1Q trunk links with a defined native VLAN
* Enabled Rapid-PVST+
* Configured Port Security with sticky MAC addressing
* Configured EtherChannel using LACP between the HQ core switches
* Moved unused ports to VLAN 999 and shut them down

---

## 3. Layer 3 Configuration

Configured Layer 3 routing at HQ using dual core switches and at each branch using router-on-a-stick.

### Tasks Completed

* Enabled IP routing on the core switches
* Created SVIs for each HQ VLAN
* Configured router-on-a-stick subinterfaces at each branch
* Assigned gateway addresses
* Configured routed links toward the edge router

---

## 4. Gateway Redundancy

Configured HSRP between the two HQ core switches and per-VLAN HSRP at each branch router.

### Tasks Completed

* Created virtual gateway addresses
* Assigned Active and Standby roles
* Configured HSRP priorities
* Enabled preemption

---

## 5. WAN Connectivity

Configured physical WAN connectivity between each site and both ISPs, using static routing for Internet failover.

### Tasks Completed

* Assigned WAN IP addresses between each site and both ISPs
* Configured primary default routes toward ISP1
* Configured floating static default routes toward the backup ISP using a higher administrative distance
* Configured summarized static return routes on each ISP router

---

## 6. GRE Tunnel Overlay

Configured GRE tunnels between the HQ edge router and each branch router, providing a primary and backup tunnel per branch.

### Tasks Completed

* Configured GRE tunnel interfaces
* Set tunnel sources and destinations using the physical WAN addresses
* Assigned unique `/30` networks to each tunnel
* Configured primary tunnels over ISP1
* Configured backup tunnels over the secondary ISP

---

## 7. Dynamic Routing — OSPF over GRE

Configured single-area OSPF across the GRE tunnels and internal routed transit links.

### Tasks Completed

* Created OSPF Area 0
* Advertised internal, WAN, and tunnel networks
* Configured passive interfaces for user VLANs
* Configured point-to-point OSPF network types on routed transit links
* Enabled non-passive OSPF on GRE tunnel interfaces

---

## 8. Network Services

Configured centralized DHCP, DNS, NTP, and Syslog services.

### Tasks Completed

* Configured DHCP pools
* Assigned default gateways
* Configured DNS information
* Configured DHCP relay using `ip helper-address` on VLAN interfaces
* Configured NTP synchronization with the central server
* Configured centralized Syslog

---

## 9. NAT/PAT

Configured NAT/PAT at each site's WAN edge.

### Tasks Completed

* Defined inside and outside NAT interfaces
* Configured standard ACLs identifying internal networks
* Configured PAT using overload

---

## 10. Security Configuration

Applied security controls throughout the network to protect infrastructure and restrict unauthorized access.

### Tasks Completed

* Configured SSH-only remote management
* Implemented Port Security
* Applied standard and extended ACLs
* Encrypted passwords
* Secured console and VTY access
* Restricted management access to the management VLAN using the `SSH-MGMT` ACL

---

## 11. Verification and Testing

Each technology was validated individually after implementation before performing end-to-end and failover testing.

### Verified

* Interface status
* VLAN operation
* Trunk links
* Spanning Tree Protocol
* Inter-VLAN routing
* HSRP failover
* GRE tunnel status
* GRE tunnel IP reachability
* OSPF neighbor relationships
* DHCP operation
* NAT translations
* ACL functionality
* End-to-end connectivity across all three sites
* Primary-to-backup WAN failover

---

## Network Technologies

| Area              | Technologies                                     |
| ----------------- | ------------------------------------------------ |
| Device Management | SSH, RSA, Password Encryption                    |
| Layer 2           | VLANs, 802.1Q, Rapid-PVST+, Port Security        |
| Redundancy        | HSRP, EtherChannel/LACP                          |
| Layer 3           | SVIs, Router-on-a-Stick, IP Routing              |
| WAN               | Dual ISP, Static Routing, Floating Static Routes |
| Overlay           | GRE                                              |
| Dynamic Routing   | OSPF Area 0                                      |
| Network Services  | DHCP, DNS, NTP, Syslog                           |
| Internet Access   | NAT/PAT                                          |
| Security          | ACLs, SSH, Port Security                         |
| Testing           | Connectivity, Routing, Failover, Services        |

## Project Outcome

The completed design provides a **segmented, redundant, and dynamically routed three-site network**. HQ and both branch offices maintain Layer 2 and Layer 3 connectivity while GRE tunnels provide the private WAN overlay. OSPF dynamically manages routes across the overlay, HSRP provides gateway redundancy, and dual-ISP connectivity provides WAN failover.

Centralized network services, NAT/PAT, ACLs, and secure device management further improve the network's usability, security, and maintainability.
