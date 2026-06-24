# What Is Systems Design

## Description

Systems design is the process of defining the architecture, components, modules, interfaces, and data flow of a system to satisfy specific requirements. It bridges the gap between a single server running your code and a globally distributed product serving millions of users.

## Prerequisites

- [What Is Computer Science](../../computer-science/intro/what-is-cs.md) — basic CS concepts
- [Why Networks Matter](../../networks/intro/why-networks.md) — networking fundamentals

## Table of Contents

- [Why Systems Design Matters](#why-systems-design-matters)
- [Key Properties of a Good System](#key-properties-of-a-good-system)
- [Common Architectural Patterns](#common-architectural-patterns)
- [Trade-offs Everywhere](#trade-offs-everywhere)

## Content / Material

### Why Systems Design Matters

A single developer can write a working web app. But making that app serve millions of users reliably, efficiently, and cost-effectively requires systems thinking. Systems design interviews at top tech companies test this ability — and real production systems demand it.

### Key Properties of a Good System

| Property | Definition |
|----------|------------|
| **Scalability** | Can handle growth in users, data, or traffic |
| **Availability** | Stays up and serving requests |
| **Reliability** | Produces correct results consistently |
| **Performance** | Responds within acceptable time |
| **Maintainability** | Easy to debug, update, and extend |
| **Cost-efficiency** | Does not waste compute or storage |

### Common Architectural Patterns

- **Monolith** — single deployable unit. Simple but hard to scale.
- **Microservices** — independent services. Flexible but operationally complex.
- **Event-driven** — components react to events via message queues. Loose coupling.
- **Layered / N-tier** — presentation, business logic, data access. Separation of concerns.
- **Peer-to-peer** — no central server. Used in file sharing and blockchains.

### Trade-offs Everywhere

Every architectural decision is a trade-off:

- Consistency vs. availability (CAP theorem)
- Latency vs. throughput
- Read performance vs. write performance
- Development speed vs. operational complexity

## Glossary

| Term | Definition |
|------|------------|
| Scalability | The ability of a system to handle increased load by adding resources |
| CAP theorem | A theorem stating a distributed system can only guarantee two of: Consistency, Availability, Partition tolerance |
| Microservices | An architectural style that structures an app as a collection of loosely coupled services |

## Next Steps

- [Scalability Basics](../beginner/scalability-basics.md) — horizontal vs. vertical scaling, load balancing
- [Caching Strategies](../beginner/caching-strategies.md) — CDN, in-memory, database caching
