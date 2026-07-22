# Troubleshooting & Lessons Learned

## Issue 1 – Trunk Configuration Error

### Problem
VLAN traffic was not passing between Layer 3 switches even though the VLANs existed on each device and Trunk Encapsulation Not Specified

### Root Cause
A trunk link must be configured on both ends of the connection. Also many switch models support multiple trunk encapsulation methods so a encapsulations need to be selected.

### Resolution
Configure both sides as trunks and select a encapsulation method.

### Verification

```bash
interface <>
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk vlan allowed <vlan list>
```
---

## Issue 2 – HSRP Redundancy Failure

### Problem
HSRP failed to establish a proper Active/Standby relationship between the two core switches. In some VLANs, HSRP did not form at all, while in others both core switches became Active.

### Root Cause
The core-to-core connection was initially configured as a Layer 3 routed link. Since HSRP operates at Layer 2, the switches could not exchange HSRP hello packets.

After converting the link to a Layer 2 trunk, the required VLANs were still not allowed across the trunk. As a result, HSRP traffic for those VLANs was still unable to pass between the core switches.

### Resolution

- Converted the core-to-core connection from a routed Layer 3 link to a Layer 2 trunk.
- Allowed all required VLANs across the trunk.
- Verified that one switch became **Active** and the other became **Standby** for each configured HSRP group.

### Verification

```bash
show standby brief
show interfaces trunk

interface <interface>
 switchport mode trunk
 switchport trunk allowed vlan <vlan-list>
```

### Lesson Learned
HSRP requires Layer 2 connectivity because hello packets are exchanged within the same VLAN. A routed interface cannot carry HSRP traffic, and even after trunking is configured, the participating VLANs must be allowed across the trunk. Without both requirements, the core switches cannot synchronize gateway state, resulting in failed or split Active/Standby operation.

---

## Issue 3 – DHCP Clients Failed to Receive Addresses

### Problem
End devices could not obtain IP addresses automatically.

### Root Cause
The client NIC was manually configured and the DHCP relay (`ip helper-address`) did not reference the correct DHCP server.

### Resolution

- Changed client network adapters to DHCP mode.
- Verified the DHCP server had a static IP address.
- Corrected the `ip helper-address` configuration on each SVI.

### Verification

```bash
ipconfig
ping
show run | include ip helper-address
```

### Lesson Learned
DHCP relies on both proper client configuration and correct relay settings. The helper address must point to the actual DHCP server.

---

## Issue 4 – Incorrect Static Route

### Problem
The Edge router could reach the core router amd could not reach internal VLAN networks.

### Root Cause
The static route referenced an invalid next-hop IP address.

### Resolution
Removed the incorrect route and configured the neighboring router's actual interface IP as the next hop.

### Verification

```bash
show ip route
show ip ospf neighbor 
ping
```

### Lesson Learned
Static routes must point to a reachable neighboring router interface. An incorrect next-hop prevents packets from being forwarded correctly.

---

## Issue 5 – Overlapping IP Addressing

### Problem
A router rejected an interface IP configuration.

### Root Cause
Two Layer 3 interfaces were configured with addresses from the same subnet.

### Resolution
Assigned a unique subnet to each routed point-to-point link.

### Verification

```bash
show ip interface brief
show ip route
```

### Lesson Learned
Each routed Layer 3 interface requires its own subnet. Unlike VLANs, routed interfaces cannot share the same IP network.

---

## Issue 6 – OSPF Network Type

### Problem
Router-to-router Ethernet links were performing unnecessary DR/BDR elections.

### Root Cause
Ethernet interfaces use the broadcast OSPF network type by default.

### Resolution
Configured the routed links as point-to-point OSPF interfaces.

### Verification

```bash
interface <>
ip address <network> <mask>
ip ospf network point-to-point>
show ip ospf interface br
```

### Lesson Learned
Using the point-to-point network type on dedicated router links simplifies OSPF neighbor relationships and eliminates unnecessary DR/BDR elections.

---

## Issue 7 – NAT/PAT Configuration

### Problem
Internal devices could not access external networks.

### Root Cause
NAT inside/outside interfaces and translation rules were not fully configured.

### Resolution

Configured:

- NAT inside interfaces
- NAT outside interface
- Standard ACL identifying internal networks
- PAT overload

### Verification

```bash
ip nat inside
ip nat outside
access list <permit> <network ID> <wildcard>
ip nat inside source list <#> <interface> overload
show ip nat translations
show ip nat statistics
```

### Lesson Learned
PAT requires four components to function correctly:

- Inside interface
- Outside interface
- ACL identifying internal addresses
- NAT translation rule

---

# Final Validation

After resolving all issues, the network was successfully validated by verifying:

- Physical connectivity
- VLAN operation
- Trunk links
- STP convergence
- Inter-VLAN routing
- HSRP failover
- OSPF route exchange
- DHCP address assignment
- NAT translations
- ACL functionality
- End-to-end connectivity
- Internet connectivity
