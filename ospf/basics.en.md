---
title: "Basic OSPF Dynamic Routing Configuration Lab"
description: "Step-by-step tutorial on establishing single-area OSPF dynamic routing on Cisco IOS."
date: 2024-01-15
lang: en
canonical: /notes/ospf/basics
tags: [ospf, routing, lab]
prev: /notes
prev_title: "TOC Homepage"
next: "#"
next_title: "Basic BGP Configuration"
category: "Routing Protocols"
author: "Tran Hoang Minh Vuong"
---
Welcome to my network laboratory documentation. In this guide, we will walk through the steps to establish and configure single-area **OSPF (Open Shortest Path First)** dynamic routing in Area 0 on Cisco IOS devices.

OSPF is a Link-State routing protocol based on the Dijkstra shortest-path-first algorithm, widely used in modern enterprise network infrastructures.

## 1. Laboratory Topology

Our network lab consists of two Cisco Routers (R1 and R2) directly connected via FastEthernet interfaces:

*   **Point-to-point transit link:** `10.0.0.0/30`
*   **LAN segment behind R1:** `192.168.10.0/24` (simulated via Loopback 0)
*   **LAN segment behind R2:** `192.168.20.0/24` (simulated via Loopback 0)

> **[TIP]** In simulation environments such as PNETLab or GNS3, it is recommended to use Loopback interfaces to simulate local area networks to save computer system resources.

## 2. Configuration Procedures

### Step 2.1: Configure IP Addresses on R1

First, we assign IP addresses to the transit interface and the simulated LAN Loopback interface on Router R1:

```bash
Router> enable
Router# configure terminal
Router(config)# hostname R1
R1(config)# interface Loopback0
R1(config-if)# ip address 192.168.10.1 255.255.255.0
R1(config-if)# exit
R1(config)# interface FastEthernet0/0
R1(config-if)# ip address 10.0.0.1 255.255.255.252
R1(config-if)# no shutdown
```

### Step 2.2: Enable OSPF Routing Protocol on R1

We enable OSPF routing with Process ID `1` and advertise the directly connected networks into the backbone **Area 0**:

```bash
R1(config)# router ospf 1
R1(config-router)# router-id 1.1.1.1
R1(config-router)# network 10.0.0.0 0.0.0.3 area 0
R1(config-router)# network 192.168.10.0 0.0.0.255 area 0
```

> **[WARNING]** When advertising networks in OSPF, you must specify the **Wildcard Mask** instead of the traditional subnet mask. An incorrect wildcard mask will prevent neighbor adjacencies from forming.

### Step 2.3: Configure R2

Apply corresponding IP addresses and OSPF configurations on Router R2:

```bash
R2(config)# router ospf 1
R2(config-router)# router-id 2.2.2.2
R2(config-router)# network 10.0.0.0 0.0.0.3 area 0
R2(config-router)# network 192.168.20.0 0.0.0.255 area 0
```

## 3. Verification

Once configurations are applied, wait approximately 30 seconds for the routers to exchange link-state database information. Run the following verification command:

```bash
R1# show ip ospf neighbor
```

If the **State** column shows `FULL/DR` or `FULL/BDR`, the neighbor adjacency has successfully formed.
