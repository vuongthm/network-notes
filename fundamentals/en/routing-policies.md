---
title: "Routing Policy Basics"
description: "How routing policy decides where packets go when more than one path is available."
tags: ["routing", "policy", "fundamentals"]
date: "2025-02-14"
lang: "en"
---

# Routing Policy Basics

Routing policy is what keeps a network from being a random collection of paths. It decides which path should win, which one should wait, and which one should never be used.

![Layered networking stack](/media/notes/fundamentals/layered-stack.svg)

## The decision tree

When multiple routes exist, the router compares them by preference, specificity, and cost.

### Longest prefix match

The most specific route usually wins.

#### Example
10.10.0.0/16
10.10.20.0/24

## Policy table

| Signal | What it affects | Typical result |
|-------|------------------|----------------|
| Prefix length | Specificity | Smaller networks win |
| Local preference | Outbound selection | Higher is preferred |
| MED | Neighbor hint | Lower can be preferred |
| AS path | Internet path quality | Shorter often wins |

### Operational rule

Policy should make the common path boring and the exceptional path obvious.

#### Debug shortcut

If traffic takes the wrong path, inspect the policy first and the packet second.