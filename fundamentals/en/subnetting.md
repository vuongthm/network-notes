---
title: "Subnetting & IP Addressing"
description: "How IP addresses are structured, how subnets work, and how to calculate them — with Python examples."
tags: ["networking", "ip", "subnetting", "fundamentals"]
date: "2024-10-01"
lang: "en"
---

# Subnetting & IP Addressing

An IP address is a 32-bit number. Subnetting is the practice of dividing a network into smaller logical segments — controlling which hosts can reach which, and how.

## Why It Matters

Without subnetting, every device on a network competes for the same broadcast domain. Subnetting isolates traffic, improves performance, and enables access control.

## The Structure of an IPv4 Address

```
192      .  168      .  1        .  10
11000000    10101000    00000001    00001010
```

An IPv4 address is four octets (8 bits each), written in dotted-decimal notation.

### CIDR Notation

A subnet is expressed as `address/prefix`, where the prefix is the number of bits reserved for the **network** portion.

```
192.168.1.0/24
            ^--- 24 bits for network, 8 bits for hosts
```

| CIDR | Subnet Mask     | Hosts Available |
|------|-----------------|-----------------|
| /8   | 255.0.0.0       | 16,777,214      |
| /16  | 255.255.0.0     | 65,534          |
| /24  | 255.255.255.0   | 254             |
| /30  | 255.255.255.252 | 2               |

> **Note:** Hosts available = 2^(32−prefix) − 2. The two subtracted addresses are the network address and the broadcast address.

## Calculating Subnets in Python

The standard library's `ipaddress` module handles all of this cleanly.

```python
import ipaddress

net = ipaddress.ip_network("192.168.1.0/24")

print(net.network_address)   # 192.168.1.0
print(net.broadcast_address) # 192.168.1.255
print(net.netmask)           # 255.255.255.0
print(net.num_addresses)     # 256
print(list(net.hosts())[:3]) # [192.168.1.1, 192.168.1.2, 192.168.1.3]
```

### Splitting a Network into Subnets

```python
import ipaddress

parent = ipaddress.ip_network("10.0.0.0/8")

# Split into /10 subnets
subnets = list(parent.subnets(new_prefix=10))

for subnet in subnets[:4]:
    print(f"{subnet}  →  hosts: {subnet.num_addresses - 2}")

# Output:
# 10.0.0.0/10   →  hosts: 4194302
# 10.64.0.0/10  →  hosts: 4194302
# 10.128.0.0/10 →  hosts: 4194302
# 10.192.0.0/10 →  hosts: 4194302
```

### Checking if a Host Belongs to a Subnet

```python
import ipaddress

subnet = ipaddress.ip_network("192.168.10.0/24")
host   = ipaddress.ip_address("192.168.10.55")

if host in subnet:
    print(f"{host} is in {subnet}")
else:
    print(f"{host} is NOT in {subnet}")
```

## Private Address Ranges (RFC 1918)

These ranges are reserved and not routed on the public internet.

| Range              | CIDR           | Common Use          |
|--------------------|----------------|---------------------|
| 10.0.0.0–10.255.255.255 | 10.0.0.0/8 | Large enterprise |
| 172.16.0.0–172.31.255.255 | 172.16.0.0/12 | Mid-size networks |
| 192.168.0.0–192.168.255.255 | 192.168.0.0/16 | Home / small office |

```python
import ipaddress

addresses = ["10.5.1.1", "172.20.0.1", "8.8.8.8", "192.168.0.100"]

for addr in addresses:
    ip = ipaddress.ip_address(addr)
    print(f"{addr:20s} private={ip.is_private}")

# 10.5.1.1             private=True
# 172.20.0.1           private=True
# 8.8.8.8              private=False
# 192.168.0.100        private=True
```

## Supernetting (Route Summarization)

The reverse of subnetting — combining adjacent networks into a single, larger prefix for cleaner routing tables.

```python
import ipaddress

nets = [
    ipaddress.ip_network("192.168.0.0/24"),
    ipaddress.ip_network("192.168.1.0/24"),
    ipaddress.ip_network("192.168.2.0/24"),
    ipaddress.ip_network("192.168.3.0/24"),
]

supernet = ipaddress.collapse_addresses(nets)

for net in supernet:
    print(net)  # 192.168.0.0/22
```

## The Practical Reality

In production you rarely calculate by hand. But understanding the binary math matters when:

- **Debugging routing issues**: Why is traffic going to the wrong gateway?
- **Designing VPC/VLAN layouts**: Avoiding overlapping ranges between environments
- **Reading firewall rules**: `0.0.0.0/0` means all traffic; `/32` means exactly one host

### Quick mental check

If two hosts share the same network address after applying the subnet mask, they are on the same subnet — no router needed.

```
Host A:  192.168.1.10  &  255.255.255.0  =  192.168.1.0  ✓
Host B:  192.168.1.99  &  255.255.255.0  =  192.168.1.0  ✓  → same subnet
Host C:  192.168.2.5   &  255.255.255.0  =  192.168.2.0  ✗  → different subnet, needs router
```
