---
title: "HTTP Caching That Actually Helps"
description: "A practical guide to cache headers, freshness, and when to serve from the edge versus the origin."
tags: ["http", "caching", "protocols"]
date: "2025-04-08"
lang: "en"
---

# HTTP Caching That Actually Helps

HTTP caching is not just about speed. It is about reducing repeated work in exactly the places where repetition hurts the most.

![Traffic flow diagram](/media/notes/protocols/network-traffic.svg)

## The Cache Story

The browser asks once, the edge serves many times, and the origin only steps in when the cached copy is stale.

### Cache-Control in practice
Cache-Control: public, max-age=300, stale-while-revalidate=60

#### What each part means

- `public` allows shared caches to store the response.
- `max-age=300` keeps it fresh for five minutes.
- `stale-while-revalidate=60` serves a slightly stale copy while a new one is fetched.

## Freshness vs validation

| Strategy | What it does | Tradeoff |
|----------|---------------|----------|
| Freshness | Reuse a response until it expires | Very fast, may be slightly stale |
| Validation | Ask the server if the cached copy is still valid | Slower, but safer |

### ETag example
If-None-Match: "abc123"

The server can reply with `304 Not Modified` when the content has not changed.

## When caching fails

Caching fails when different users get the same response but should not.

#### Rule of thumb

If personalization matters, make the cache key explicit or skip caching entirely.

## Practical checklist

| Question | Default answer |
|----------|----------------|
| Is the response public? | Cache it |
| Is it user-specific? | Do not share it |
| Does it change often? | Use validation |
| Is it static? | Make it long-lived |