# What Is JavaScript

## Description

JavaScript is the only programming language natively supported by web browsers. It powers interactive web pages, server-side applications (Node.js), mobile apps, and desktop tools. TypeScript extends JavaScript with static typing for larger-scale projects.

## Prerequisites

- [What Is Programming](../intro/what-is-programming.md)

## Table of Contents

- [A Brief History](#a-brief-history)
- [JavaScript Today](#javascript-today)
- [TypeScript](#typescript)
- [Key Concepts](#key-concepts)

## Content / Material

### A Brief History

Created in 1995 by Brendan Eich in 10 days. Initially a simple scripting language for Netscape, it evolved through ES3 (1999), ES5 (2009), and ES6 (2015) — the modern version. TypeScript, created by Microsoft in 2012, adds optional static types.

### JavaScript Today

- **Browser** — the only native client-side language
- **Server** — Node.js, Deno, Bun
- **Mobile** — React Native, Expo
- **Desktop** — Electron, Tauri
- **Full stack** — Next.js, Nuxt, SvelteKit

### TypeScript

TypeScript is a superset of JavaScript that adds:

- Static type checking at compile time
- Interfaces, generics, enums
- Better IDE support (autocomplete, refactoring)
- Catches entire classes of bugs before runtime

```typescript
function greet(name: string): string {
  return `Hello, ${name}!`;
}
```

### Key Concepts

| Concept | Description |
|---------|-------------|
| **Event loop** | Single-threaded async model — non-blocking I/O |
| **Prototypes** | Inheritance via prototype chain (not classes) |
| **Closures** | Functions that remember their lexical scope |
| **Promises / async-await** | Modern async control flow |

## Glossary

| Term | Definition |
|------|------------|
| ECMAScript | The standardized specification that JavaScript implements |
| TypeScript | A statically typed superset of JavaScript developed by Microsoft |
| Event loop | JavaScript's mechanism for handling asynchronous operations |
| Closure | A function that retains access to its outer scope even after the outer function returns |

## Next Steps

- [Variables & Types](../beginner/variables-and-types.md) — let, const, var, and type annotations
- [Functions & Scope](../beginner/functions-and-scope.md) — arrow functions, closures, hoisting
