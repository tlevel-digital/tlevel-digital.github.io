# Static Routing

## Overview

This covers the complete static routing configuration for a chain of 8 Cisco routers, connecting a T-Lab OPNsense gateway through to a test PC on a dedicated LAN segment at the far end.

## Physical Layout

```
[OPNsense - Dell R210 II]
        192.168.1.1
             |
   [Cisco Catalyst 4849]
      (Layer 2 — no config needed)
             |
            R1  ← Cisco 2900
       WAN: 192.168.1.107 (static, from OPNsense LAN)
             |
        192.168.10.0/30
             |
            R2
             |
        192.168.20.0/30
             |
            R3
             |
        192.168.30.0/30
             |
            R4
             |
        192.168.40.0/30
             |
            R5
             |
        192.168.50.0/30
             |
            R6
             |
        192.168.60.0/30
             |
            R7
             |
        192.168.70.0/30
             |
            R8
       LAN: 192.168.80.1
             |
       192.168.80.0/24
             |
         [Test PC]
       (DHCP from R8)
```

---

## Network Summary

|Link|Left Side|IP|Right Side|IP|Mask|
|---|---|---|---|---|---|
|Link 1|R1|`192.168.10.1`|R2|`192.168.10.2`|`/30`|
|Link 2|R2|`192.168.20.1`|R3|`192.168.20.2`|`/30`|
|Link 3|R3|`192.168.30.1`|R4|`192.168.30.2`|`/30`|
|Link 4|R4|`192.168.40.1`|R5|`192.168.40.2`|`/30`|
|Link 5|R5|`192.168.50.1`|R6|`192.168.50.2`|`/30`|
|Link 6|R6|`192.168.60.1`|R7|`192.168.60.2`|`/30`|
|Link 7|R7|`192.168.70.1`|R8|`192.168.70.2`|`/30`|
|R8 LAN|R8|`192.168.80.1`|Test PC|DHCP|`/24`|

> All point-to-point links use `255.255.255.252` (`/30`), providing exactly 2 usable host addresses per link.

## Routing Logic

Every router must have a static route for **every network it is not directly connected to.**

- Traffic heading **toward the test PC** → next-hop is always the **right-hand neighbour**
- Traffic heading **toward OPNsense** → next-hop is always the **left-hand neighbour**

---

## Router Configurations

### R1 — Cisco 2900 (T-Lab Entry Point)

R1's WAN interface should be set as a **static IP** to prevent the OPNsense return route from breaking on DHCP renewal.

```
Router1> enable
Router1# configure terminal

! Set static WAN IP
Router1(config)# interface GigabitEthernet0/0
Router1(config-if)# ip address 192.168.1.107 255.255.255.0
Router1(config-if)# no shutdown
Router1(config-if)# exit

! Default route toward OPNsense
Router1(config)# ip route 0.0.0.0 0.0.0.0 192.168.1.1

! Route to test PC LAN
Router1(config)# ip route 192.168.80.0 255.255.255.0 192.168.10.2

! Routes to all downstream link subnets
Router1(config)# ip route 192.168.20.0 255.255.255.252 192.168.10.2
Router1(config)# ip route 192.168.30.0 255.255.255.252 192.168.10.2
Router1(config)# ip route 192.168.40.0 255.255.255.252 192.168.10.2
Router1(config)# ip route 192.168.50.0 255.255.255.252 192.168.10.2
Router1(config)# ip route 192.168.60.0 255.255.255.252 192.168.10.2
Router1(config)# ip route 192.168.70.0 255.255.255.252 192.168.10.2
```

---

### R2

```
Router2> enable
Router2# configure terminal

! Routes toward test PC (rightward)
Router2(config)# ip route 192.168.80.0 255.255.255.0 192.168.20.2
Router2(config)# ip route 192.168.30.0 255.255.255.252 192.168.20.2
Router2(config)# ip route 192.168.40.0 255.255.255.252 192.168.20.2
Router2(config)# ip route 192.168.50.0 255.255.255.252 192.168.20.2
Router2(config)# ip route 192.168.60.0 255.255.255.252 192.168.20.2
Router2(config)# ip route 192.168.70.0 255.255.255.252 192.168.20.2

! Route toward OPNsense (leftward)
Router2(config)# ip route 192.168.1.0 255.255.255.0 192.168.10.1
```

---

### R3

```
Router3> enable
Router3# configure terminal

! Routes toward test PC (rightward)
Router3(config)# ip route 192.168.80.0 255.255.255.0 192.168.30.2
Router3(config)# ip route 192.168.40.0 255.255.255.252 192.168.30.2
Router3(config)# ip route 192.168.50.0 255.255.255.252 192.168.30.2
Router3(config)# ip route 192.168.60.0 255.255.255.252 192.168.30.2
Router3(config)# ip route 192.168.70.0 255.255.255.252 192.168.30.2

! Routes toward OPNsense (leftward)
Router3(config)# ip route 192.168.1.0 255.255.255.0 192.168.20.1
Router3(config)# ip route 192.168.10.0 255.255.255.252 192.168.20.1
```

---

### R4

```
Router4> enable
Router4# configure terminal

! Routes toward test PC (rightward)
Router4(config)# ip route 192.168.80.0 255.255.255.0 192.168.40.2
Router4(config)# ip route 192.168.50.0 255.255.255.252 192.168.40.2
Router4(config)# ip route 192.168.60.0 255.255.255.252 192.168.40.2
Router4(config)# ip route 192.168.70.0 255.255.255.252 192.168.40.2

! Routes toward OPNsense (leftward)
Router4(config)# ip route 192.168.1.0 255.255.255.0 192.168.30.1
Router4(config)# ip route 192.168.10.0 255.255.255.252 192.168.30.1
Router4(config)# ip route 192.168.20.0 255.255.255.252 192.168.30.1
```

---

### R5 (Middle)

```
Router5> enable
Router5# configure terminal

! Routes toward test PC (rightward)
Router5(config)# ip route 192.168.80.0 255.255.255.0 192.168.50.2
Router5(config)# ip route 192.168.60.0 255.255.255.252 192.168.50.2
Router5(config)# ip route 192.168.70.0 255.255.255.252 192.168.50.2

! Routes toward OPNsense (leftward)
Router5(config)# ip route 192.168.1.0 255.255.255.0 192.168.40.1
Router5(config)# ip route 192.168.10.0 255.255.255.252 192.168.40.1
Router5(config)# ip route 192.168.20.0 255.255.255.252 192.168.40.1
Router5(config)# ip route 192.168.30.0 255.255.255.252 192.168.40.1
```

---

### R6

```
Router6> enable
Router6# configure terminal

! Routes toward test PC (rightward)
Router6(config)# ip route 192.168.80.0 255.255.255.0 192.168.60.2
Router6(config)# ip route 192.168.70.0 255.255.255.252 192.168.60.2

! Routes toward OPNsense (leftward)
Router6(config)# ip route 192.168.1.0 255.255.255.0 192.168.50.1
Router6(config)# ip route 192.168.10.0 255.255.255.252 192.168.50.1
Router6(config)# ip route 192.168.20.0 255.255.255.252 192.168.50.1
Router6(config)# ip route 192.168.30.0 255.255.255.252 192.168.50.1
Router6(config)# ip route 192.168.40.0 255.255.255.252 192.168.50.1
```

---

### R7

```
Router7> enable
Router7# configure terminal

! Route toward test PC (rightward)
Router7(config)# ip route 192.168.80.0 255.255.255.0 192.168.70.2

! Routes toward OPNsense (leftward)
Router7(config)# ip route 192.168.1.0 255.255.255.0 192.168.60.1
Router7(config)# ip route 192.168.10.0 255.255.255.252 192.168.60.1
Router7(config)# ip route 192.168.20.0 255.255.255.252 192.168.60.1
Router7(config)# ip route 192.168.30.0 255.255.255.252 192.168.60.1
Router7(config)# ip route 192.168.40.0 255.255.255.252 192.168.60.1
Router7(config)# ip route 192.168.50.0 255.255.255.252 192.168.60.1
```

---

### R8 (Far End — Test PC Gateway)

```
Router8> enable
Router8# configure terminal

! Static WAN address
Router8(config)# interface [WAN-interface]
Router8(config-if)# ip address 192.168.70.2 255.255.255.252
Router8(config-if)# no shutdown
Router8(config-if)# exit

! LAN interface for test PC
Router8(config)# interface [LAN-interface]
Router8(config-if)# ip address 192.168.80.1 255.255.255.0
Router8(config-if)# no shutdown
Router8(config-if)# exit

! Default route — sends all unknown traffic back up the chain
Router8(config)# ip route 0.0.0.0 0.0.0.0 192.168.70.1
```

> Use `show ip interface brief` to identify the correct interface names before applying.

## Verification

### Check Routing Tables

Run on every router — look for `S` (static) entries:

```
show ip route
```

### Check Interface Status

Both `Status` and `Protocol` columns must show `up`:

```
show ip interface brief
```

### Test End-to-End Ping

From R1, ping the test PC gateway:

```
ping 192.168.80.1
```

From R1, ping with source interface to verify return path:

```
ping 192.168.80.1 source GigabitEthernet0/0
```

### Trace the Full Path

```
traceroute 192.168.80.1
```

Expected output: **R1 → R2 → R3 → R4 → R5 → R6 → R7 → R8**

## Troubleshooting

|Symptom|Likely Cause|Fix|
|---|---|---|
|Ping fails at a specific hop|Missing static route on that router|Run `show ip route` — look for missing `S` entry|
|Traffic goes out but no reply|Missing OPNsense return route, or missing leftward route on a router|Check OPNsense routes and verify leftward routes on each router|
|Interface down|Interface not enabled|Run `show ip interface brief`, apply `no shutdown`|
|Wrong next-hop|Typo in `ip route` command|Next-hop must be the **neighbour's** IP, not your own|
|R1 route breaks after reboot|R1 WAN is still on DHCP|Set R1 WAN to static `192.168.1.107` as shown above|

## Key Concepts

**`/30` point-to-point links** — Using `255.255.255.252` gives exactly 2 usable host addresses per link, which is all that is needed between two routers. This conserves address space.

**Longest prefix match** — When a router has multiple routes that could match a destination, it always uses the most specific one (the longest prefix). This is why OPNsense's local `/24` route takes priority over any `/16` summary.

**Default route** (`0.0.0.0/0`) — A catch-all used on R1 (toward OPNsense) and R8 (toward the chain). Matches any destination not covered by a more specific route.

**Static vs dynamic routing** — This guide uses fully manual static routing. In larger networks, protocols like **OSPF** or **EIGRP** would build these tables automatically. Static routing is excellent for learning because it requires you to understand every path explicitly.
