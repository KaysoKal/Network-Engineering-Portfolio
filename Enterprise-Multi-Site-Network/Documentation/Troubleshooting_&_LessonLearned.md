# Troubleshooting & Lessons Learned

Most of the problems in this project came from WAN issues.

## Issue 1 – Missing Return Route

### Problem
Branches could ping some HQ router interfaces but not others. Traffic went out fine but the replies never came back.

### Root Cause
The HQ edge router had a static route sending the destination network back out toward the ISP instead of toward the HQ router. Because the static route had a lower administrative distance than OSPF, it silently overrode the correct path.

### Resolution
Removed the incorrect static route so OSPF's correct path was used.

### Verification

```bash
show ip route <network>
ping <destination>
```

### Lesson Learned
A working forward path proves nothing about the return path. When one destination fails while everything around it works, check for a static route silently overriding the correct dynamic route.

---

## Issue 2 – GRE Tunnel Up/Up But No Traffic Passing

### Problem
The tunnel interface showed up/up, but pings to the remote tunnel IP failed and OSPF would not form a neighbor across it.

### Root Cause
A GRE tunnel's line protocol comes up as long as the router has any route to the tunnel destination. It does not verify that the far end actually exists or is reachable, so up/up is a weak signal.

### Resolution
Verified the underlay first (physical interface up, route to the tunnel destination), then the tunnel IPs, then the tunnel itself.

### Verification

```bash
show ip interface brief
ping <ISP next-hop>
show interface Tunnel0
ping <remote tunnel IP>
```

### Lesson Learned
Up/up is not proof of connectivity. Trust the ping, not the interface status line.

---

## Issue 3 – Confusing Tunnel IPs with Physical WAN IPs

### Problem
Tunnels failed to come up correctly or pointed at the wrong peer.

### Root Cause
Tunnel source and destination must use the physical WAN IPs, not the tunnel IPs. These are two separate address spaces and are easy to mix up.

### Resolution
Set tunnel source/destination to the physical WAN interfaces, and kept the tunnel IPs as their own /30 pair (.1 and .2).

### Verification

```bash
show interface Tunnel0
show ip interface brief
```

### Lesson Learned
The tunnel destination is always the far end's physical WAN address, never its tunnel address. The tunnel IPs only exist inside the encapsulation.

---

## Issue 4 – OSPF Adjacency Not Forming Over the Tunnel

### Problem
The tunnel and tunnel IPs worked fine, but `show ip ospf neighbor` came back empty.

### Root Cause
The tunnel network was missing from OSPF's network statements, or the tunnel was left passive.

### Resolution
Added the tunnel network to OSPF and removed it from the passive-interface list.

### Verification

```bash
show ip ospf neighbor
show ip protocols
```

### Lesson Learned
A passive interface still advertises its subnet but never forms an adjacency. User VLANs should stay passive; tunnels and transit links must be non-passive.

---

## Issue 5 – Failover Not Working for Backup

### Problem
Simulating a primary ISP failure did not shift traffic to the backup path.

### Root Cause
The floating static route was missing the correct higher administrative distance, and/or the backup GRE tunnel had no working OSPF adjacency of its own.

### Resolution
Confirmed the floating static's administrative distance and verified the backup tunnel's OSPF neighbor independently of the primary.

### Verification

```bash
show ip route 0.0.0.0
show ip ospf neighbor
```

### Lesson Learned
Failover here is two separate mechanisms — a floating static default for internet access, and OSPF over dual tunnels for site-to-site traffic. Test both independently.

---

## Issue 6 – Two NAT Statements Won't Apply on HQ Edge

### Problem
When configuring PAT overload for both ISP1 and the ISP backup on the same router, only one would take effect.

### Root Cause
Packet Tracer does not support two overlapping NAT overload statements active on one router at the same time.

### Resolution
Configured PAT overload on the primary ISP interface only, and documented the limitation rather than leaving it unexplained.

### Verification

```bash
show ip nat translations
show running-config | include ip nat
```

### Lesson Learned
Not every limitation is a configuration mistake — some are platform limitations of the simulator. Real hardware would solve this with route-map-based NAT instead of a single interface overload statement.

---

## Final Validation

After resolving all issues, the WAN was validated by verifying:

- Physical WAN connectivity
- GRE tunnel status and tunnel IP reachability
- OSPF neighbor adjacency and route exchange
- NAT translations
- Primary and backup default route failover
- End-to-end connectivity across all three sites
