# Abstraction as a Principle

## Description

Abstraction is the single most important idea in computer science. Every layer of computing — from transistors to the cloud — is an abstraction built on the layer below. This document explores what abstraction means, why it is powerful, and when it breaks down.

## Prerequisites

- [What Is Computer Science](what-is-cs.md)
- [Computational Thinking](computational-thinking.md)

## Table of Contents

- [The Abstraction Stack](#the-abstraction-stack)
- [Leaky Abstractions](#leaky-abstractions)
- [Good vs. Bad Abstraction](#good-vs-bad-abstraction)
- [Designing Abstractions](#designing-abstractions)

## Content / Material

### The Abstraction Stack

From bottom to top:

```
Applications
  ↑   Libraries / Frameworks
  ↑   Programming languages
  ↑   Operating system (syscalls, file systems, processes)
  ↑   Kernel / Drivers
  ↑   Instruction set architecture (x86, ARM)
  ↑   Microarchitecture
  ↑   Digital logic (gates, flip-flops)
  ↑   Transistors
  ↑   Physics (semiconductors)
```

Each layer hides the complexity of the layer below. A JavaScript developer writes `console.log()` never thinking about how electrons move through silicon.

### Leaky Abstractions

An abstraction is *leaky* when details of the underlying layer can't be completely hidden.

- "TCP provides a reliable stream" — until a timeout exposes packet loss.
- "SQL abstracts away storage" — until a bad query is slow because of a missing index.
- "HTTP is stateless" — until you need sessions, cookies, or connection pools.

Understanding the layer below helps you work with leaky abstractions effectively. This is why good developers know more than just their framework.

### Good vs. Bad Abstraction

| Good Abstraction | Bad Abstraction |
|-----------------|-----------------|
| Simple interface | Complex interface |
| Hides what should be hidden | Exposes implementation details |
| Consistent mental model | Surprising behavior |
| Composable with other abstractions | Requires special handling to combine |
| Makes common things easy | Makes everything equally hard |

### Designing Abstractions

To design a good abstraction:

1. **Identify the essence** — what must every implementation provide?
2. **Hide the accidents** — what is specific to one implementation?
3. **Name it well** — the name should convey the abstraction, not the implementation.
4. **Test the boundary** — does the abstraction make the common case simple and the complex case possible?

## Glossary

| Term | Definition |
|------|------------|
| Abstraction | Hiding complexity behind a simplified interface |
| Leaky abstraction | An abstraction that cannot fully hide underlying details |
| Instruction set | The interface between software and hardware |
| Mental model | A user's internal representation of how a system works |

## Next Steps

- [Data Structures](../beginner/data-structures.md) — abstract data types and their concrete implementations
