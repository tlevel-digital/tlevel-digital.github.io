# VLANs, DHCP, and Inter-VLAN Routing Using Packet Tracer

## Introduction

VLANs are a fundamental part of enterprise networking. While they can seem complex at first, they offer a highly effective way to optimise and segment a network once understood. In this introduction, we’ll complete a series of tasks using the Cisco command line interface, working with managed switches and integrated services routers to build practical experience.

For an interactive experience, please download the following Packet Tracer Activity:

[Introduction to VLANs in Packet Tracer](https://codeberg.org/RDMillen/ComputingEducation/raw/branch/main/PacketTracerActivities/IntroToVLANs.pka)

The following instructions are mirrored within activity file, the below is for reference when working with physical hardware.

## Scenario

You are a network technician setting up a small segmented office network. Your objective is to:

- Create two VLANs: Admin (VLAN 10) and Students (VLAN 20)
- Configure a router to perform inter-VLAN routing using subinterfaces
- Set up DHCP so that devices receive IP addresses automatically
- Verify that devices on different VLANs can communicate

---

## Devices Required

- 1 x Cisco Router 
- 1 x Cisco Managed Switch 
- Client test machines
- 1 x Console Cable

---

## IP Addressing and VLAN Scheme

This is a common example of an IP addressing scheme with basic VLAN usage.

|VLAN|Subnet|Gateway|DHCP Range|
|---|---|---|---|
|10|192.168.10.0/24|192.168.10.1|192.168.10.11 – .254|
|20|192.168.20.0/24|192.168.20.1|192.168.20.11 – .254|

---

## Step 1: Configure the Cisco 3650 Switch

### Create VLANs

```shell
enable  
configure terminal  
vlan 10  
 name Admin  
exit  
vlan 20  
 name Students  
exit
```


### Assign Ports to VLANs

Assume:

- GigabitEthernet1/0/1 and 1/0/2 are for VLAN 10
- GigabitEthernet1/0/3 and 1/0/4 are for VLAN 20

```shell
interface range GigabitEthernet1/0/1 - 2  
 switchport mode access  
 switchport access vlan 10  
exit  
​  
interface range GigabitEthernet1/0/3 - 4  
 switchport mode access  
 switchport access vlan 20  
exit
```


### Configure Trunk Port to Router

Assume GigabitEthernet1/0/24 connects to the router:

```shell
interface GigabitEthernet1/0/24  
 switchport mode trunk  
exit
```

---

## Step 2: Configure the Cisco 1941 Router

### Remove IP from the Physical Interface

```shell
enable
configure terminal
interface GigabitEthernet0/0  
 no ip address  
 no shutdown  
exit
```

### Create Subinterfaces for Each VLAN

```shell
interface GigabitEthernet0/0.10  
 encapsulation dot1Q 10  
 ip address 192.168.10.1 255.255.255.0  
exit  
​  
interface GigabitEthernet0/0.20  
 encapsulation dot1Q 20  
 ip address 192.168.20.1 255.255.255.0  
exit
```

---

## Step 3: Configure DHCP Services on the Router

```shell
ip dhcp excluded-address 192.168.10.1 192.168.10.10  
ip dhcp excluded-address 192.168.20.1 192.168.20.10  
​  
ip dhcp pool VLAN10  
 network 192.168.10.0 255.255.255.0  
 default-router 192.168.10.1  
exit  
​  
ip dhcp pool VLAN20  
 network 192.168.20.0 255.255.255.0  
 default-router 192.168.20.1  
exit
```

---

## Step 4: Verify the Setup

From each PC:

* Set the PC to use DHCP
- Use the `ipconfig` command to check IP address allocation
- Confirm the default gateway matches the VLAN's router IP
- Use `ping` to:
    - Reach the default gateway
    - Communicate with another PC on the same VLAN
    - Communicate with a PC on the other VLAN

From the router:

- Run `show ip interface brief` to confirm interfaces are up
- Run `show running-config` to review your setup
