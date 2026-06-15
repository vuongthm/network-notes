---
title: "DNS Resolution in 6 Steps"
description: "How a hostname becomes an IP address, and where caches, recursion, and TTLs fit into the path."
tags: ["dns", "resolution", "protocols"]
date: "2025-03-12"
lang: "en"
---

# DNS Resolution in 6 Steps

DNS turns names into addresses, but the real value is that it does so predictably, at scale, and with enough caching to stay fast.

![Traffic flow diagram](/media/notes/protocols/network-traffic.svg)

## The lookup path

1. The browser checks its own cache.
2. The OS checks the system resolver.
3. The recursive resolver checks its cache.
4. The resolver asks the root servers.
5. The resolver asks the TLD servers.
6. The resolver asks the authoritative server.

### Recursive versus iterative

Recursive resolution means "do the work for me." Iterative resolution means "point me to the next hop."

#### Example command
dig +trace example.com

## TTL matters

| Field | Meaning | Why it matters |
|------|---------|----------------|
| A record | Maps a name to an IPv4 address | Common web traffic path |
| AAAA record | Maps a name to an IPv6 address | Dual-stack support |
| TTL | Time to live | Controls cache freshness |

### Common failure modes

- A low TTL creates extra lookup traffic.
- A stale cache can hide a DNS change.
- A typo in the authoritative zone can break everything downstream.

#### Takeaway

DNS looks simple because the hard work is spread across several caches and servers.