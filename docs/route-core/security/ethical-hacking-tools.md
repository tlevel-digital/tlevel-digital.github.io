# Ethical Hacking Tools

!!! warning "Important"
    Only scan machines that you have explicit permission to scan.

## Network Reconnaissance Tools

In this practical we will use three tools to investigate our infrastructure. 

* `ping`
* `traceroute`
* `nmap`

### ping

The ping tool checks whether a device is reachable on the work, it sends a small message known as an *ICPM echo request* to the target device and waits for a reply. If the device responds, it is online.

The following command will ping the IP address of the T-Lab router three times.

```bash
ping -c 3 192.168.1.1
```

This command will return data that looks something like this:

```bash
64 bytes from 192.168.1.10: icmp_seq=1 ttl=128 time=0.54 ms
```

#### What does this mean?

- **64 bytes:** Size of the reply
- **icmp_seq:** Packet number
- **ttl:** Time to Live (packet lifespan)
- **time:** How long it took (latency)

The ping command can be used to check latency to websites as well as IP addresses:

```bash
ping -c 3 dovercollege.ac.uk
```

### traceroute

Traceroute shows the path that packets take to reach a destination. 

Each step in the path is called a *hop*.

```bash
traceroute 192.168.1.50
```

This command will return data that looks something like this:

```bash
1 192.168.1.1 1.123 ms  
2 192.168.1.50 0.543 ms
```

#### What does this show?

- Each numbered line = a router or device
- The IP address = the hop
- The time = how long it took between hops

### nmap

Nmap is a network mapping software that scans a device or local network to discover:

* Connected devices
* Open ports
* Running services
* Sometimes, version information
* Known vulnerabilities

The follow command will initial a full scan of the local network:

```bash
sudo nmap 192.168.1.0/24
```

To scan a specific device, and check service versions:

```bash
sudo nmap -sV 192.168.1.11
```


This command will return data that looks something like this:

```bash
PORT STATE SERVICE  
135/tcp open msrpc  
445/tcp open microsoft-ds  
3389/tcp open ms-wbt-server
```
#### What does this mean?

- **PORT:** Port number
- **STATE:** Open / Closed / Filtered
- **SERVICE:** What is running

|State|Meaning|
|---|---|
|open|Service is accepting connections|
|closed|Port is accessible but no service|
|filtered|Blocked by firewall|
