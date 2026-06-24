# Systems and Models

## Description

Physics teaches us to see the world as interconnected systems with inputs, outputs, feedback loops, and emergent behavior. This systems-thinking mindset is directly applicable to software architecture, distributed systems, and understanding complex codebases.

## Prerequisites

- [Why Physics Matters](why-physics.md)
- [Scientific Thinking](scientific-thinking.md)

## Table of Contents

- [What Is a System](#what-is-a-system)
- [State and Dynamics](#state-and-dynamics)
- [Feedback Loops](#feedback-loops)
- [Emergence](#emergence)

## Content / Material

### What Is a System

A system is a set of interacting components that produce behavior not achievable by any component alone.

```
System:    [Input] → [Process] → [Output]
                  ↺ feedback ↻
```

In physics: a pendulum is a system of mass + gravity + friction.
In software: a microservice is a system of code + database + network + load balancer.

### State and Dynamics

Physics distinguishes between:

- **State** — the current condition of a system (position, velocity, temperature).
- **Dynamics** — how the state changes over time (equations of motion).

In software:

- **State** — the values of all variables, database rows, cache entries at a given moment.
- **Dynamics** — the code that transforms state: functions, transactions, state machines.

### Feedback Loops

| Type | Physics | Software |
|------|---------|----------|
| **Negative feedback** | A thermostat turns off heat when target is reached | Auto-scaling: add instances when CPU > 80% |
| **Positive feedback** | A microphone too close to a speaker creates a loop | Viral growth: more users → more content → more users |

Negative feedback stabilizes. Positive feedback amplifies. Both appear in system design.

### Emergence

Emergence is when a system exhibits behavior not present in any individual component:

- Ants → colony intelligence
- Neurons → consciousness
- Microservices → a working application

Understanding emergence explains why adding more services doesn't linearly add complexity — it changes the nature of the system.

## Glossary

| Term | Definition |
|------|------------|
| System | A set of interacting components producing collective behavior |
| State | The current condition of a system at a point in time |
| Feedback loop | A process where the output of a system influences its input |
| Emergence | Behavior that arises from interactions but isn't present in individual parts |

## Next Steps

- [Newtonian Mechanics for Devs](../beginner/newtonian-mechanics.md) — concrete physics models for simulation
