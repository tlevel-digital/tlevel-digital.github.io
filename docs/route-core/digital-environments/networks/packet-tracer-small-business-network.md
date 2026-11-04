# Packet Tracer - Small Business Network

## Scenario

You are a network engineer at a small company.  

The company has moved into a two-floor office and needs a working network that connects PCs, phones, and a shared server.  

You will start by setting up a **basic single network** and then extend it over time to introduce services and segmentation.

## 1. Core Network Setup

### Office Layout

- **Ground Floor:** 4 × PCs, 4 × IP phones, 1 × switch  
- **First Floor:** 4 × PCs, 4 × IP phones, 1 × switch  
- **Router:** connects both switches to the same network  
- **Server:** provides shared web, file, and name services  
- **Wireless Access Point:** connects laptops and mobiles  

Focus first on **physical layout**, **addressing**, and **connectivity** before moving on to VLANs or routing.

### IP Addressing (Flat Network)

Use one simple subnet to start, additional guidance can be found here: [IP Addressing](ip-addressing.md)

| Device | Example IP Address | Notes |
|---------|-------------------|-------|
| Router (Gateway) | 192.168.10.1 | Default gateway |
| Server | 192.168.10.10 | Hosts intranet/DNS/FTP |
| WAP | 192.168.10.50 | Static address |
| PCs | 192.168.10.100–199 | Static IPs |
| DNS Server | 192.168.10.10 | Same as main server |

**Steps:**

1. Assign static IPs to each device.  
2. Test connectivity using `ping`.  
3. Confirm that all PCs can reach the server and router.

### Wireless Access Point

| Setting    | Value         |
| ---------- | ------------- |
| SSID       | TechEase-WiFi |
| Security   | WPA2 PSK      |
| Passphrase | Packet123     |
| IP Address | 192.168.10.50 |
|            |               |

### Testing Checklist

- [ ] All devices have valid IP addresses  
- [ ] PCs can ping the server and router  
- [ ] Wireless device connects and accesses intranet  

## 2. Intermediate Challenge: Network Services

1. Move IP allocation from static to **DHCP** (on the server).  
    - Scope: `192.168.10.100 – 199`  
    - Gateway: `192.168.10.1`  
    - DNS: `192.168.10.10`
2. Extend DNS records:  
    - `ftp.techease.local → 192.168.10.10`  
    - `wap.techease.local → 192.168.10.50`
3. Add a second test file or folder for shared resources.

## 3. Advanced Challenge: VLANs and Segmentation

Attempt once the single-network design works reliably.

The following resource will be useful here: [Virtual LANs](virtual-lans.md)

1. Configure VLANs on both switches:  
    - VLAN 10 – Data (PCs + Server)  
    - VLAN 20 – Voice (IP Phones)  
    - VLAN 99 – Management (optional)  
2. Assign ports appropriately and create a trunk link between switches.  
3. Convert the router to a **router-on-a-stick** (one sub-interface per VLAN).  
4. Verify:
    - Devices within a VLAN can ping each other  
    - VLANs are isolated  
    - Inter-VLAN routing allows controlled access if enabled  

## 4. Expert Challenge: Multi-Network Router

> Optional for stretch students or after VLANs are secure.

1. Replace the router with one that has **3 or more Ethernet ports**.  
2. Introduce additional subnets:  
    - Office Network – `192.168.10.0/24`  
    - Server LAN – `192.168.30.0/24`  
    - Guest Wi-Fi – `192.168.40.0/24`
3. Configure **basic inter-network routing** so that PCs can reach the server but guest Wi-Fi remains separate.  
4. Test connectivity and DNS resolution across networks.

## 5. Documentation and Reflection

Submit:

- **Packet Tracer (.pkt)** file with labels and configs  
- **Network diagram** showing topology and addressing  
- **Test evidence:** screenshots of pings, DNS lookups, webpage access  
- **Short reflection (½ page):**  
> “How did I structure my network, and what would I change if the company expanded to three floors?”  
