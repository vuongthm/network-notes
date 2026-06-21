---
title: "TCP/IP Model Explained"
description: "A practical walkthrough of the TCP/IP stack and how each layer maps to real network behavior."
tags: ["networking", "tcp-ip", "fundamentals"]
date: "2025-01-14"
lang: "en"
locked: true
---

# TCP/IP Model Explained

The TCP/IP model is the practical stack most engineers use every day. It is shorter than OSI, but it maps more directly to how traffic actually moves.

![TCP/IP layered stack](/media/notes/fundamentals/layered-stack.svg)

## The Four Layers

| Layer | Job | Common examples |
|-------|-----|-----------------|
| Application | User-facing protocols and content negotiation | HTTP, DNS, SMTP |
| Transport | End-to-end delivery between processes | TCP, UDP |
| Internet | Logical addressing and routing | IP, ICMP |
| Network access | Frames, bits, and physical media | Ethernet, Wi-Fi |

### How a request moves

An application asks for data, transport chooses ports, internet adds source and destination addresses, and the network access layer gets the frame onto the wire.

#### Example
curl https://example.com
## Why It Matters

TCP/IP gives you a cleaner debugging path than talking about every implementation detail at once.

### A useful shortcut

If the problem disappears when you change the port or protocol, you are probably in the transport layer.

## Layer Comparison

| Question | TCP/IP answer |
|----------|----------------|
| Who owns the message format? | The application layer |
| Who owns delivery between hosts? | The transport layer |
| Who owns routing? | The internet layer |
| Who owns the physical medium? | The network access layer |

#### Takeaway

OSI is great for explanation. TCP/IP is great for execution.