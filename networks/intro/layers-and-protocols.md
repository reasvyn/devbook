# Layers and Protocols

## Description

Network protocols are organized in layers, each building on the one below. This layered architecture is the internet's greatest design insight — it allows innovation at any layer without disrupting the others. This document explores the philosophy of layering and how it enables the modern web.

## Prerequisites

- [Why Networks Matter](why-networks.md)
- [Principles of Communication](principles-of-communication.md)

## Table of Contents

- [Why Layers](#why-layers)
- [The OSI and TCP/IP Models](#the-osi-and-tcpip-models)
- [Encapsulation](#encapsulation)
- [The Power of Standard Interfaces](#the-power-of-standard-interfaces)

## Content / Material

### Why Layers

Each layer solves a specific problem and presents a clean interface to the layer above:

- The **physical layer** does not care what data you're sending — it just moves bits.
- The **network layer** does not care if the connection is reliable — it just routes packets.
- The **application layer** does not care how packets traverse the network — it just exchanges messages.

Layering reduces complexity and enables independent evolution.

### The OSI and TCP/IP Models

| OSI Layer | TCP/IP Layer | Example Protocols |
|-----------|-------------|-------------------|
| Application | Application | HTTP, DNS, SMTP, WebSocket |
| Presentation | (merged into Application) | TLS, SSL |
| Session | (merged into Application) | gRPC, WebRTC |
| Transport | Transport | TCP, UDP |
| Network | Internet | IP, ICMP |
| Data Link | Link | Ethernet, Wi-Fi |
| Physical | Link | Cables, radio signals |

Most developers only interact with the top layers, but understanding the full stack helps debug problems at every level.

### Encapsulation

When data travels down the stack, each layer wraps (encapsulates) the data from the layer above:

```
[ HTTP Request ]
[ TCP Header | HTTP Request ]
[ IP Header | TCP Header | HTTP Request ]
[ Ethernet Frame | IP Header | TCP Header | HTTP Request ]
```

Each header contains metadata needed by the corresponding layer on the receiving end. This is the same principle as nested function calls or middleware chains.

### The Power of Standard Interfaces

Because each layer has a standard interface, you can mix and match:

- HTTP over TCP over IPv4 over Ethernet
- HTTP over TCP over IPv6 over Wi-Fi
- HTTP/3 over QUIC (UDP) over IPv4 over Fiber

The application layer (your code) doesn't change when the lower layers change. This is the ultimate goal of abstraction — and the internet stack is one of the most successful examples in engineering.

## Glossary

| Term | Definition |
|------|------------|
| Encapsulation | Wrapping data from a higher layer with a header from a lower layer |
| Protocol | A set of rules governing communication between peers |
| OSI model | A 7-layer conceptual framework for network communication |
| TCP/IP model | A 4-layer model representing the internet protocol suite |

## Next Steps

- [HTTP in Depth](../beginner/http-in-depth.md) — the protocol that powers the web
