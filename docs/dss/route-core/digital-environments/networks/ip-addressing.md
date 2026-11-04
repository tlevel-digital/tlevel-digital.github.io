# IP Addressing

## Introduction

An IP (Internet Protocol) address is a unique number assigned to every device connected to the internet so it can be identified. IP addresses play a key role in allowing data packets to be sent between devices, whether over a local network or across the internet. If you think of an IP address like a physical address, it helps direct your “packet” to the correct destination. Just like posting a letter, the IP address ensures your data ends up in the right place.

## Dynamic vs Static Addressing

### Dynamic

When our computers, laptops, and smart devices connect to a network, they’re usually assigned an IP address automatically. This is known as a **dynamic IP address**, so called because it can change over time. For most client devices, this isn’t a problem—other users or devices don’t need to remember your device’s IP address, so the focus is on ease of setup rather than permanence.

To assign IP addresses automatically, the network needs a **DHCP (Dynamic Host Configuration Protocol) server**. This server provides all the key information a device needs to get online: an IP address, the IP address of the router (or gateway), and the address of the DNS server.

### Static

Unlike dynamic IP addresses, a **static IP address** does not change. It is manually assigned to a device and stays the same each time the device connects to the network. This is useful for devices that need to be consistently found by other devices—like servers, network printers, or CCTV systems. 

If the IP address kept changing, other devices wouldn’t know how to reach it. While static IPs take a bit more effort to set up, they provide reliability and are essential for services that need a fixed point of contact on the network.

## IP Configuration

While most devices use DHCP, knowing how to configure a **static IP address** is a very useful skill. The exact steps vary depending on your operating system, but regardless of platform, you’ll need the same key pieces of information:

- **IP Address** – The unique address you’re assigning to the device. It must not be in use by any other device on the network, or you’ll run into a connectivity issue known as an _IP conflict_.
- **Gateway** – The IP address of your network’s router, which acts as the route to other networks and the internet.
- **Subnet Mask** – This defines the size of your network. The most common value is `255.255.255.0` (also written as CIDR `/24`). [More about subnet masks.](https://digitalsupport.wiki/Route+Core+\(RC\)/Networks/Subnets+and+Subnet+Masks)
- **DNS Server** – The server responsible for translating domain names (like `google.com`) into the IP addresses that computers use to find each other.


### Windows

[Network Configuration in Microsoft Windows 10/11](https://support.microsoft.com/en-gb/windows/essential-network-settings-and-tasks-in-windows-f21a9bbc-c582-55cd-35e0-73431160a1b9)

### MacOS

[Network Configuration in MacOS Sequoia](https://support.apple.com/en-gb/guide/mac-help/mchlp2718/mac)

### Linux

[Network Configuration in Debian Linux](https://wiki.debian.org/NetworkConfiguration)

[Network Configuration in Ubuntu Linux](https://documentation.ubuntu.com/server/explanation/networking/configuring-networks/index.html)

[Network Configuration in Fedora Linux](https://docs.fedoraproject.org/en-US/fedora-coreos/sysconfig-network-configuration/#_configuring_a_static_ip)

---
## Practical Activity

Building on your knowledge of IP addresses, you’ll now convert a network from using DHCP to statically assigned IP addresses. For this task, you’ll need access to **Cisco Packet Tracer** to open the file provided below.

Guidance on how to access and install Packet Tracer can be found [here](https://digitalsupport.wiki/Route+Core+\(RC\)/Networks/Cisco+Packet+Tracer).

The following instructions are mirrored within activity file, the below is for reference when working with physical hardware.

---
## Scenario

You are a network technician setting up a small business network to serve 3 staff computers. Your objective is to:

- Set up DHCP so that devices receive IP addresses automatically
- Configure the desktop PCs to receive an IP address via DHCP

---
## Devices Required

- 1 x Cisco Router 
- 1 x Switch
- Client test machines
- 1 x Console Cable

---
## Step 1: Configure the Cisco 1941 Router

### Set a static IP for the LAN Interface

```shell
enable
configure terminal
interface GigabitEthernet0/0  
 ip address 192.168.1.1 255.255.255.0
 no shutdown  
exit
```

---
## Step 2: Configure DHCP Services on the Router

```shell
ip dhcp excluded-address 192.168.1.1 192.168.1.10   
​  
ip dhcp pool LAN
 network 192.168.1.0 255.255.255.0  
 default-router 192.168.1.1  
exit  ​
```

---
## Step 3: Write changes

Return to the main enable menu of your router, then enter the command "write memory" to save the current running configuration on your router.

```shell
Router# write memory
```

---
## Step 4: Verify the Setup

From each PC:

* Set the PC to use DHCP
- Use the `ipconfig` command to check IP address allocation
- Confirm the default gateway matches the router IP
- Use `ping` to:
    - Reach the default gateway
    - Communicate with another PC

From the router:

- Run `show ip interface brief` to confirm interfaces are up
- Run `show running-config` to review your setup
