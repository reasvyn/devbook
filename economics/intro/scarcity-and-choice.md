# Scarcity & Choice

## Description

Scarcity is the bedrock of economics — the simple truth that you cannot have everything forces every decision to be a trade-off. Developers face scarcity every day in time, budget, compute, and attention.

## Prerequisites

- [Why Economics?](why-economics.md)

## Table of Contents

- [What Is Scarcity?](#what-is-scarcity)
- [Opportunity Cost](#opportunity-cost)
- [Trade-offs in Software Development](#trade-offs-in-software-development)
- [Scarcity in System Design](#scarcity-in-system-design)

## Content / Material

### What Is Scarcity?

Scarcity means resources are finite while human wants are infinite. The result: **choice is inevitable**. Every decision implicitly says "yes" to one thing and "no" to everything else.

Types of scarcity in software:

- **Time** — deadlines, sprint length, developer hours
- **Talent** — experienced engineers, domain experts
- **Compute** — CPU, memory, bandwidth, storage
- **Attention** — user focus, product surface area
- **Budget** — cloud costs, tooling, headcount

### Opportunity Cost

The opportunity cost of a choice is the value of the best alternative you gave up. It's not measured in money — it's measured in **what you could have done instead**.

```
Opportunity Cost = Value of Next Best Alternative
```

**Examples:**

- Spending 3 months rewriting the frontend means 3 months not building features
- Choosing AWS means not choosing GCP — lock-in, skill set, ecosystem all differ
- Attending a meeting means not writing code during that hour

### Trade-offs in Software Development

| Decision | Trade-off |
|----------|-----------|
| Monolith vs microservices | Simplicity vs flexibility |
| Static typing vs dynamic | Safety vs speed of iteration |
| SQL vs NoSQL | Consistency vs scalability |
| Build vs buy | Control vs velocity |
| In-house vs managed | Customization vs maintenance burden |

Every trade-off is an economic decision. **There is no free lunch** — a core economic insight that developers rediscover daily.

### Scarcity in System Design

System design is applied economics under scarcity:

- **Latency vs throughput** — optimize one at the expense of the other
- **Consistency vs availability** — CAP theorem is a scarcity constraint
- **Cache vs database** — speed vs freshness
- **Compute vs storage** — recompute vs cache

Engineers who internalize scarcity make better decisions because they explicitly acknowledge trade-offs rather than pretending they don't exist.

## Glossary

| Term | Definition |
|------|------------|
| Scarcity | Limited resources relative to unlimited wants |
| Opportunity cost | Value of the best alternative foregone |
| Trade-off | A situation where gaining one thing means losing another |
| CAP theorem | A system cannot simultaneously guarantee consistency, availability, and partition tolerance |

## Next Steps

- [Supply & Demand](../beginner/supply-and-demand.md) (future) — how prices emerge from scarcity
- [Why Finance?](../../financial/intro/why-finance.md) — scarcity applied to capital
