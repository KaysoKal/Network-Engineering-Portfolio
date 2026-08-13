# Enterprise Multi-Site Network

A Cisco enterprise network built in **Cisco Packet Tracer**, connecting a headquarters site to two branch offices over redundant dual-ISP WAN paths.

## Overview

**Sites:** HQ / Core, Branch 1, Branch 2, Primary ISP, Backup ISP

**Technologies:** VLAN segmentation & inter-VLAN routing · HSRP · OSPF · GRE tunnels · Primary/backup WAN with floating static routes · NAT/PAT · DHCP relay · centralized DNS/TFTP/Syslog/NTP · SSH management · standard & extended ACLs · port security · Rapid-PVST+ · EtherChannel/LACP · BPDU Guard & PortFast · management VLAN

## IP Addressing

| Site | VLAN | Network | Gateway |
|---|--:|---|---|
| HQ | 10 | 10.10.10.0/26 | 10.10.10.1 |
| HQ | 20 | 10.10.20.0/26 | 10.10.20.1 |
| HQ | 30 | 10.10.30.0/25 | 10.10.30.1 |
| HQ | 40 | 10.10.40.0/26 | 10.10.40.1 |
| HQ | 50 | 10.10.50.0/25 | 10.10.50.1 |
| HQ | 60 | 10.10.60.0/24 | 10.10.60.1 |
| HQ | 70 | 10.10.70.0/26 | 10.10.70.1 |
| HQ | 80 | 10.10.80.0/29 | 10.10.80.1 |
| HQ | 90 | 10.10.90.0/27 | 10.10.90.1 |
| Branch 1 | 10–90 | 10.20.x.0 | 10.20.x.1 |
| Branch 2 | 10–90 | 10.30.x.0 | 10.30.x.1 |

**Infrastructure links**

| Purpose | Network |
|---|---|
| HQ – ISP1 | 200.0.0.0/30 |
| Branch1 – ISP1 | 200.0.0.4/30 |
| Branch2 – ISP1 | 200.0.0.8/30 |
| Backup WAN | 192.0.0.0/30 and related /30 links |
| GRE tunnels | 172.16.x.0/30 |
| OSPF core link | 10.255.0.0/30 |

## Routing & Redundancy

- **OSPF Area 0** dynamically routes between the core and branch networks.
- **GRE tunnels** carry that routing across the WAN, connecting each branch router to HQ.
- **Primary + floating static default routes** fail over to the backup ISP if the primary path drops.
- **HSRP** provides redundant gateways for user VLANs — Core1 holds higher priority for selected VLANs at HQ, and each branch router serves as the HSRP gateway for its local VLANs.

## Security

SSH-only management, restricted by the `SSH-MGMT` ACL · `SERVER-PROTECTION` ACL guarding the centralized server · sticky-MAC port security · BPDU Guard & PortFast on access ports · unused ports parked in VLAN 999 and shut down · dedicated management VLAN (90) · centralized logging and NTP.

## Services

The centralized server at **10.10.80.4** (HQ server VLAN) provides DHCP, DNS, TFTP, Syslog, and NTP. VLAN interfaces relay DHCP requests to it:

```text
ip helper-address 10.10.80.4
```

## NAT

NAT/PAT at each site's WAN edge provides internet connectivity for internal networks, over both the primary and backup WAN paths.

## Verification

```text
show ip route
show ip ospf neighbor
show ip ospf interface
show standby
show ip nat translations
show access-lists
show port-security
show etherchannel summary
show interfaces trunk
show spanning-tree
```

## Topology

`/images/network-topology.png`

## Skills Demonstrated

Cisco IOS · OSPF · HSRP · GRE · VLANs · Inter-VLAN Routing · NAT/PAT · DHCP Relay · ACLs · SSH · Port Security · STP · EtherChannel · DNS · TFTP · Syslog · NTP · WAN Redundancy
