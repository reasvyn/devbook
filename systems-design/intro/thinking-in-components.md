# Thinking in Components

## Description

Every system — whether a monolith or a mesh of microservices — is composed of components. How you divide responsibility between components, how they communicate, and how they depend on each other determines the quality of the system. This document explores the principles of modular design that scale from a single function to a global architecture.

## Prerequisites

- [What Is Systems Design](what-is-systems-design.md)
- [Trade-offs First](trade-offs-first.md)

## Table of Contents

- [What Is a Component](#what-is-a-component)
- [Cohesion and Coupling](#cohesion-and-coupling)
- [Contracts and Boundaries](#contracts-and-boundaries)
- [The Size Question](#the-size-question)

## Content / Material

### What Is a Component

A component is a self-contained unit with a clear responsibility and a well-defined interface. It can be:

- A **function** — `calculateTotal(items)`
- A **class** — `UserRepository`
- A **service** — `PaymentService`
- A **microservice** — `inventory-service`
- A **library** — `lodash`

At every scale, the same principles apply: a component hides its internals and communicates through an interface.

### Cohesion and Coupling

| Principle | Good | Bad |
|-----------|------|-----|
| **High cohesion** | Things that belong together are in the same component | A `User` class that also sends emails |
| **Low coupling** | Components can change independently | Changing the database forces changes in every service |

High cohesion + low coupling = maintainable system.

### Contracts and Boundaries

The boundary of a component is its **contract** — the promises it makes to the outside world:

- **Preconditions** — what must be true before calling.
- **Postconditions** — what will be true after calling.
- **Invariants** — what is always true.

A contract can be:
- Explicit: a type signature, an API schema, a protobuf definition.
- Implicit: "this function expects a non-null argument" (enforced by convention).

Strong contracts allow components to be developed, tested, and deployed independently.

### The Size Question

How big should a component be? There is no universal answer, but here are useful heuristics:

| Size | When to Use | Risk |
|------|-------------|------|
| **Fine-grained** | Clear boundaries, independent change cycles | Too many interfaces, orchestration overhead |
| **Coarse-grained** | Tightly related logic, fast development | Monolith, hard to scale team effort |

Start coarse, then split when you feel the pain of coupling. Premature splitting creates more problems than it solves.

## Glossary

| Term | Definition |
|------|------------|
| Component | A self-contained unit with a defined interface |
| Cohesion | The degree to which elements inside a component belong together |
| Coupling | The degree to which components depend on each other |
| Contract | The set of promises a component makes to its callers |

## Next Steps

- [Scalability Basics](../beginner/scalability-basics.md) — horizontal vs. vertical scaling, load balancing
