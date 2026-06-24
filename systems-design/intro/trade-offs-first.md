# Trade-offs First

## Description

Systems design is the art of making trade-offs. There is no "best" architecture — only architectures that balance competing concerns for a specific context. This document explores the mindset of trade-off thinking and the fundamental tensions every architect must navigate.

## Prerequisites

- [What Is Systems Design](what-is-systems-design.md)

## Table of Contents

- [There Is No Free Lunch](#there-is-no-free-lunch)
- [The Fundamental Trade-offs](#the-fundamental-trade-offs)
- [Context Is Everything](#context-is-everything)
- [Making Decisions Explicit](#making-decisions-explicit)

## Content / Material

### There Is No Free Lunch

Every decision in systems design gives something up:

- Adding a cache improves read performance but adds complexity and stale data.
- Switching to microservices improves team autonomy but adds network overhead.
- Using a strongly consistent database is simpler to reason about but limits availability under partition.

The skill is not avoiding trade-offs — it's choosing which ones to make.

### The Fundamental Trade-offs

| Trade-off | Description |
|-----------|-------------|
| **Latency vs. throughput** | Fast responses for few requests vs. slower responses for many |
| **Consistency vs. availability** | Every node sees the same data vs. every request gets a response |
| **Read vs. write performance** | Optimizing reads (indexes) slows writes |
| **Development speed vs. operational complexity** | Monolith ships fast but monolith is hard to scale |
| **Cost vs. performance** | More (or better) hardware improves performance but costs more |

### Context Is Everything

A trade-off that makes sense for a startup may be wrong for a bank:

```
Startup:                Bank:
  Speed over safety       Safety over speed
  Monolith over micro     Microservices over monolith
  Optimistic over pess.   Pessimistic over optim.
```

There are no right answers — only right answers *for your context*.

### Making Decisions Explicit

Good engineers document trade-offs. An Architecture Decision Record (ADR) captures:

1. The decision made.
2. The alternatives considered.
3. Why this option was chosen.
4. What was sacrificed (the trade-off).
5. Under what conditions the decision might be revisited.

This turns design into an evidence-based conversation rather than an opinion contest.

## Glossary

| Term | Definition |
|------|------------|
| Trade-off | A situation where improving one property degrades another |
| CAP theorem | A distributed systems theorem about consistency, availability, and partition tolerance |
| ADR | Architecture Decision Record — a document capturing a design decision and its rationale |
| Strong consistency | Guaranteeing all nodes return the same data at the same time |

## Next Steps

- [Thinking in Components](thinking-in-components.md) — modularity, coupling, and cohesion
