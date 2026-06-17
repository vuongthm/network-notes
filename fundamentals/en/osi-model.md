---
title: "The OSI Model Explained"
description: "A clear breakdown of the seven-layer OSI model — what each layer does, why the model exists, and how to remember it."
tags: ["networking", "osi", "fundamentals"]
date: "2024-10-01"
lang: "en"
locked: true
---

# The OSI Model Explained

The OSI (Open Systems Interconnection) model is a conceptual framework that standardizes the functions of a communication system into seven distinct layers.

![Layered networking stack](/media/notes/fundamentals/layered-stack.svg)

## Why It Exists

Before the OSI model, different vendors built incompatible networking equipment. A device from one manufacturer couldn't communicate with a device from another. The OSI model solved this by providing a common language and set of rules.

### The mental model

Think of the model as a contract between layers. Each layer accepts a task, adds what it needs, and passes the result downward.

## The Seven Layers

| Layer | Name | What it does |
|-------|------|------|
| 7 | Application | User-facing protocols (HTTP, FTP, DNS) |
| 6 | Presentation | Data formatting, encryption, compression |
| 5 | Session | Managing connections between applications |
| 4 | Transport | End-to-end delivery (TCP, UDP) |
| 3 | Network | Logical addressing and routing (IP) |
| 2 | Data Link | Physical addressing (MAC), error detection |
| 1 | Physical | Raw bits over a medium (cables, radio) |

### A quick packet walk
Application → Presentation → Session → Transport → Network → Data Link → Physical

#### Example

An HTTP request starts at Layer 7, becomes a TCP segment at Layer 4, and eventually travels as bits on a wire.

## How to Remember It

- **Top to bottom:**
  - *All People Seem To Need Data Processing*
- **Bottom to top:**
  - *Please Do Not Throw Sausage Pizza Away*

## The Practical Reality

In practice, most engineers work with TCP/IP's four-layer model rather than OSI's seven. But OSI remains useful for:

- **Troubleshooting**: "Is this a Layer 3 issue (routing) or a Layer 2 issue (switching)?"
- **Vendor communication**: A shared vocabulary across teams and companies
- **Education**: Building intuition for how networks actually work

## Layer 4 vs Layer 7

A common confusion is between "Layer 4 load balancing" and "Layer 7 load balancing":

- **Layer 4** routes traffic based on IP and TCP/UDP port — fast but dumb. It doesn't look at the content.
- **Layer 7** routes based on HTTP headers, URLs, or cookies — slower but much smarter. It can route `/api/` to one server and `/static/` to another.

### Why engineers still use OSI

The model is not a protocol stack. It is a way to describe failure domains.

#### Practical rule

If you can isolate the layer where the behavior changes, you can usually shorten the debugging session.