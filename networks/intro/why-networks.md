# Why Networks Matter

## Description

Every web request, API call, and cloud service depends on computer networks. Understanding how data travels from one machine to another helps developers debug issues, optimize performance, and build reliable distributed systems.

## Prerequisites

None.

## Table of Contents

- [What Is a Network](#what-is-a-network)
- [The OSI Model](#the-osi-model)
- [Key Concepts for Developers](#key-concepts-for-developers)
- [Why This Matters in Practice](#why-this-matters-in-practice)

## Content / Material

### What Is a Network

A network connects computers so they can exchange data. The simplest network is two machines with a cable. The largest is the internet — a global network of networks.

### The OSI Model

The Open Systems Interconnection (OSI) model breaks networking into 7 layers:

| Layer | Name | What Developers Care About |
|-------|------|--------------------------|
| 7 | Application | HTTP, WebSockets, DNS — the protocols your code uses directly |
| 4 | Transport | TCP (reliable) vs UDP (fast) — guarantees and trade-offs |
| 3 | Network | IP addresses, routing — how packets find their destination |
| 2 | Data Link | MAC addresses, Ethernet, Wi-Fi — local delivery |
| 1 | Physical | Cables, radio signals — the hardware |

### Key Concepts for Developers

- **Latency** — the time it takes for data to travel. Light in fiber is fast, but not instant.
- **Bandwidth** — how much data can travel per second.
- **TCP vs. UDP** — TCP guarantees delivery (web pages, email). UDP is faster but lossy (video calls, gaming).
- **DNS** — translates domain names to IP addresses. Every web request starts here.
- **HTTP** — the protocol your browser and API clients speak. Methods, status codes, headers.

### Why This Matters in Practice

```
Your app → HTTP → TCP → IP → Router → Internet → Server → Response
```

When a request fails, understanding the layers helps you isolate the problem: DNS misconfiguration? TLS handshake error? TCP timeout? Application bug?

## Glossary

| Term | Definition |
|------|------------|
| OSI model | A conceptual framework describing 7 layers of network communication |
| TCP | Transmission Control Protocol — reliable, connection-oriented transport |
| DNS | Domain Name System — translates human-readable names to IP addresses |
| Latency | The time delay between sending and receiving data |

## Next Steps

- [HTTP in Depth](../beginner/http-in-depth.md) — methods, status codes, headers, and versions
- [DNS Deep Dive](../beginner/dns-deep-dive.md) — resolution, caching, and common pitfalls
