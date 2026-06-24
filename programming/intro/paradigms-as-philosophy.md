# Paradigms as Philosophy

## Description

A programming paradigm is more than a technical choice — it's a worldview. It shapes how you decompose problems, how you structure code, and how you think about state and behavior. Understanding paradigms at a philosophical level makes you a more flexible and deliberate programmer.

## Prerequisites

- [What Is Programming](what-is-programming.md)

## Table of Contents

- [What Is a Paradigm](#what-is-a-paradigm)
- [The Major Worldviews](#the-major-worldviews)
- [Paradigms Are Tools, Not Religions](#paradigms-are-tools-not-religions)
- [Multiparadigm Reality](#multiparadigm-reality)

## Content / Material

### What Is a Paradigm

A paradigm is a set of assumptions and principles that shape how you approach problems:

- What is the fundamental unit of code? (function? object? expression?)
- How does data flow? (mutations? streams? pure transformations?)
- How do you manage complexity? (encapsulation? composition? immutability?)

Your paradigm determines which solutions you see — and which you miss.

### The Major Worldviews

| Paradigm | Core Principle | Think of It As |
|----------|---------------|----------------|
| **Imperative** | Step-by-step instructions telling the computer *how* to do something | A recipe |
| **Declarative** | Describing *what* you want, not how to do it | A shopping list |
| **Functional** | Pure functions, immutable data, no side effects | Math equations |
| **Object-oriented** | Objects encapsulating state and behavior together | Small worlds with their own rules |
| **Procedural** | Grouping code into procedures/functions | A toolbox |
| **Logic** | Declaring facts and rules, letting the computer infer | A legal contract |

### Paradigms Are Tools, Not Religions

The best programmers are multilingual in paradigms. They use:

- OOP for modeling business entities.
- Functional for data transformations.
- Imperative for performance-critical hot paths.
- Declarative for configuration and UI.

A hammer is not better than a saw — they solve different problems.

### Multiparadigm Reality

Most modern languages support multiple paradigms:

```
JavaScript: objects + functional + imperative
Python:     OOP + functional + imperative
Scala:      pure OOP + pure functional (merged)
C#:         OOP + functional + declarative (LINQ)
Rust:       imperative + functional + traits (OOP-like)
```

The key skill is recognizing which paradigm fits the problem, not forcing the problem into your favorite paradigm.

## Glossary

| Term | Definition |
|------|------------|
| Paradigm | A fundamental style or worldview of programming |
| Pure function | A function whose output depends only on its inputs, with no side effects |
| Immutability | The principle that data cannot be changed after creation |
| Side effect | Any state change outside a function's local scope |

## Next Steps

- [What Makes Good Code](what-makes-good-code.md) — principles that transcend any paradigm
