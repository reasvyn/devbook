# Principles of Communication

## Description

All network communication — whether between two computers, two microservices, or two threads — follows the same fundamental principles. This document covers the universal concepts that underlie every protocol: encoding, addressing, reliability, and flow control.

## Prerequisites

- [Why Networks Matter](why-networks.md)

## Table of Contents

- [Encoding and Signals](#encoding-and-signals)
- [Addressing and Routing](#addressing-and-routing)
- [Reliability and Retransmission](#reliability-and-retransmission)
- [Flow Control](#flow-control)

## Content / Material

### Encoding and Signals

To communicate, you need a way to represent information as a signal.

- A **message** is the information you want to send (e.g., "Hello").
- An **encoding** is the mapping from message to signal (e.g., ASCII bits → voltage levels).
- The **medium** is what carries the signal (e.g., copper wire, radio wave, fiber optic).

In software: JSON, Protocol Buffers, and gRPC are encodings. HTTP/2 is a medium (a framing layer over TCP).

### Addressing and Routing

If only two parties communicate, addressing is trivial. In a network of millions, every message needs:

- **Source address** — where it came from.
- **Destination address** — where it's going.
- **Routing algorithm** — how to get there.

This is exactly how postal systems work, how the internet works (IP), and how service meshes work (service discovery + routing).

### Reliability and Retransmission

Communication channels lose messages. Handling this requires:

- **Acknowledgments** — "I got it."
- **Timeouts** — "I didn't get an ack, so I'll resend."
- **Sequence numbers** — "This is message #5, ignore it if you already received it."
- **Deduplication** — remove duplicates caused by retransmission.

TCP implements all of these. Many application protocols reinvent them badly.

### Flow Control

A sender can produce data faster than a receiver can consume it. Solutions:

- **Stop-and-wait** — send one, wait for ack, send next. Simple but slow.
- **Sliding window** — send N messages before stopping. Efficient but complex.
- **Backpressure** — the receiver tells the sender to slow down.

These concepts appear everywhere: TCP flow control, backpressure in streams (Node.js, RxJS), database connection pooling, and rate limiting.

## Glossary

| Term | Definition |
|------|------------|
| Encoding | Converting information into a transmittable signal |
| Routing | The process of selecting a path for data across a network |
| Acknowledgement | A signal confirming successful receipt of data |
| Backpressure | A mechanism that slows the sender when the receiver is overwhelmed |

## Next Steps

- [Layers and Protocols](layers-and-protocols.md) — how protocols compose to form the internet stack
