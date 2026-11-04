# Subnets and Subnet Masks

## Introduction

In the world of networking, understanding subnets and subnet masks is crucial for effective network design, management, and security. This article aims to provide a clear understanding of these concepts, their importance, and how they are applied in real-world scenarios.

## What is a Subnet?

A **subnet** (short for subnetwork) is a logically visible subdivision of an IP network. Subnetting allows network administrators to divide a larger network into smaller, more manageable segments. This segmentation can improve performance, enhance security, and simplify network management.

### Why Use Subnets?

1. **Improved Performance**: By dividing a large network into smaller subnets, broadcast traffic is reduced, leading to improved overall network performance.
2. **Enhanced Security**: Subnets can isolate sensitive data and systems, making it harder for unauthorised users to access critical resources.
3. **Efficient IP Address Management**: Subnetting allows for better utilisation of IP addresses, reducing waste and ensuring that each segment of the network has the necessary addresses.

## What is a Subnet Mask?

A **subnet mask** is a 32-bit number that divides an IP address into the network and host portions. It helps devices determine which part of the IP address refers to the network and which part refers to the individual device (host).

### Structure of a Subnet Mask

A subnet mask consists of two parts:

- **Network bits**: The portion of the mask that identifies the network.
- **Host bits**: The portion of the mask that identifies the individual devices within that network.

For example, in the subnet mask `255.255.255.0`, the first three octets (255.255.255) represent the network portion, while the last octet (0) represents the host portion.

### CIDR Notation

Subnet masks can also be represented in **CIDR (Classless Inter-Domain Routing)** notation, which uses a slash (/) followed by the number of bits in the network portion. For example:

- `255.255.255.0` is represented as `/24` in CIDR notation.

## Common Subnet Masks

Here are some common subnet masks and their corresponding CIDR notations:

| Subnet Mask         | CIDR Notation | Usable Addresses |
|---------------------|---------------|------------------|
| 255.0.0.0           | /8            | 16,777,214       |
| 255.255.0.0         | /16           | 65,534           |
| 255.255.255.0       | /24           | 254              |
| 255.255.255.128     | /25           | 126              |
| 255.255.255.192     | /26           | 62               |
| 255.255.255.224     | /27           | 30               |
| 255.255.255.240     | /28           | 14               |
| 255.255.255.248     | /29           | 6                |
| 255.255.255.252     | /30           | 2                |

## How to Calculate Subnets

To calculate the number of subnets and usable addresses, you can use the following formulas:

1. **Number of Subnets**: $2^n$ (where $n$ is the number of bits borrowed for subnetting).
2. **Usable Addresses**: $2^h - 2$ (where $h$ is the number of host bits, subtracting 2 for the network and broadcast addresses).

### Example

Suppose you have a Class C network with the address `192.168.1.0` and you want to create 4 subnets. 

1. **Determine the number of bits needed**: To create 4 subnets, you need to borrow 2 bits (since $2^2 = 4$).
2. **New Subnet Mask**: The original subnet mask is `255.255.255.0` (/24). Borrowing 2 bits gives you a new subnet mask of `255.255.255.192` (/26).
3. **Usable Addresses**: With 6 host bits remaining, the number of usable addresses is $2^6 - 2 = 62$.
