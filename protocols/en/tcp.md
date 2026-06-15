---
title: "TCP Three-Way Handshake Explained"
description: "A clear walkthrough of how TCP establishes connections — SYN, SYN-ACK, ACK — and why this mechanism matters for reliability."
tags: ["networking", "tcp", "protocols"]
date: "2024-10-20"
lang: "en"
---

# TCP Three-Way Handshake Explained

TCP (Transmission Control Protocol) is the backbone of reliable internet communication. Before any data transfers, TCP performs a three-way handshake to establish a connection.

![Traffic flow diagram](/media/notes/protocols/network-traffic.svg)

## Step 1: SYN

The client sends a SYN (synchronize) packet to the server. This packet contains a randomly chosen Initial Sequence Number (ISN).
Client → Server: SYN (seq=x)
## Step 2: SYN-ACK

The server responds with a SYN-ACK packet. This acknowledges the client's sequence number and sends the server's own ISN.
Server → Client: SYN-ACK (seq=y, ack=x+1)
## Step 3: ACK

The client completes the handshake by acknowledging the server's sequence number.
Client → Server: ACK (ack=y+1)
## Why Three Steps?

Two steps would confirm only a one-way channel. Three steps confirm that both sides can send and receive — establishing the full-duplex channel TCP requires.

## Connection Teardown

Closing a TCP connection uses a four-way handshake:

1. **FIN** — one side signals it has finished sending
2. **ACK** — other side acknowledges the FIN
3. **FIN** — other side also signals it has finished
4. **ACK** — first side acknowledges the second FIN

## TCP vs UDP

| Feature | TCP | UDP |
|---------|-----|-----|
| Connection | Connection-oriented | Connectionless |
| Reliability | Guaranteed delivery | Best-effort |
| Order | In-order delivery | No ordering |
| Speed | Slower (overhead) | Faster (no overhead) |
| Use cases | HTTP, email, files | Video streaming, gaming, DNS |

### When to choose TCP

Use TCP when ordering and delivery matter more than raw latency.

#### Common failure signal

If a service is reachable but the payload is corrupt or incomplete, the bug is often not in the handshake itself but in how the data is handled after the connection is established.