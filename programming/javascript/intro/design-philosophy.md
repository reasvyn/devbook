# Design Philosophy of JavaScript

## Description

JavaScript is one of the most widely used and misunderstood languages in the world. Its design was shaped by constraints: ten days of initial development, a mandate to "look like Java," and the need to run safely in a browser. Understanding its philosophy explains both its quirks and its power.

## Prerequisites

- [What Is JavaScript](what-is-javascript.md)

## Table of Contents

- [Born in a Hurry](#born-in-a-hurry)
- [Prototypal Inheritance](#prototypal-inheritance)
- [Functions as First-Class Citizens](#functions-as-first-class-citizens)
- [The Event-Driven Soul](#the-event-driven-soul)
- [Backward Compatibility as a Constraint](#backward-compatibility-as-a-constraint)

## Content / Material

### Born in a Hurry

Brendan Eich created JavaScript in ten days in May 1995. It was designed to be a "glue language" for the browser — simple enough for designers, powerful enough for engineers. This rushed birth explains many of JavaScript's idiosyncrasies: `typeof null === "object"`, automatic semicolon insertion, and loose equality (`==`).

### Prototypal Inheritance

Unlike class-based languages (Java, C++), JavaScript uses prototypal inheritance:

- Objects inherit directly from other objects.
- There are no classes in the classical sense (even `class` in ES6 is syntactic sugar over prototypes).
- Properties can be added, modified, or deleted at runtime on any object.

This is more flexible but requires understanding the prototype chain to use correctly.

### Functions as First-Class Citizens

In JavaScript, functions are values. They can be:

- Assigned to variables
- Passed as arguments
- Returned from other functions
- Stored in data structures

This enables functional programming patterns: callbacks, closures, higher-order functions, and composition.

### The Event-Driven Soul

JavaScript's runtime is fundamentally event-driven:

- A single thread runs an event loop.
- I/O operations (network, file, timers) are non-blocking.
- When an async operation completes, a callback is queued on the event loop.
- The event loop processes one callback at a time.

This model, while simple, requires understanding the event loop and microtask queue to avoid common pitfalls.

### Backward Compatibility as a Constraint

The web has no version numbers. JavaScript cannot break old code. This means:

- Every mistake ever shipped must be supported forever.
- New features must not break existing behavior.
- The language evolves through additions, not corrections.

This is why `"use strict"` is opt-in, why `undefined` can be reassigned (in older code), and why there are so many ways to do the same thing.

## Glossary

| Term | Definition |
|------|------------|
| Prototype | The object from which another object inherits properties |
| Event loop | JavaScript's mechanism for handling asynchronous callbacks |
| First-class function | A function that can be treated as a value |
| Backward compatibility | The ability to run code written for older versions without changes |

## Next Steps

- [Variables & Types](../beginner/variables-and-types.md) — let, const, var, and type coercion
