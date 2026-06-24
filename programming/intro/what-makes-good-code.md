# What Makes Good Code

## Description

Good code is not an aesthetic preference — it's a set of measurable qualities that make software easier to understand, safer to change, and cheaper to maintain. This document explores the universal principles of code quality that transcend language, paradigm, and framework.

## Prerequisites

- [What Is Programming](what-is-programming.md)
- [Paradigms as Philosophy](paradigms-as-philosophy.md)

## Table of Contents

- [Readability](#readability)
- [Simplicity](#simplicity)
- [Correctness](#correctness)
- [Testability](#testability)
- [Maintainability](#maintainability)

## Content / Material

### Readability

Code is read far more often than it is written. Readable code:

- Uses descriptive names — `calculateTotal()` not `calc()`
- Follows consistent formatting and conventions
- Avoids unnecessary cleverness and magic numbers
- Expresses intent, not just mechanics

```
// Hard to read
if (u.s & 2) { d(); }

// Readable
if (user.hasPermission(Permission.Delete)) {
    confirmAndDelete(user);
}
```

### Simplicity

The hardest thing in programming is not writing complex code — it's writing simple code that solves a complex problem.

Simple code:
- Does one thing well (single responsibility).
- Has no dead code, no premature optimization, no speculative features.
- Is easy to change because there are fewer dependencies.

### Correctness

Correct code does what it claims to do. This means:

- It handles edge cases (empty input, null, overflow).
- Its invariants hold before and after every operation.
- It fails predictably when something goes wrong.

Correctness is not the same as "it runs without errors" — it means the output matches the specification for all valid inputs.

### Testability

Code that is hard to test has problems:

- Tight coupling makes it impossible to test components in isolation.
- Hidden state makes tests unpredictable.
- Side effects make tests slow and flaky.

Writing testable code forces you to design better abstractions.

### Maintainability

Software spends 80% of its life in maintenance. Maintainable code:

- Has clear dependencies and well-defined interfaces.
- Is modular — each piece can be understood, tested, and changed independently.
- Has documentation that explains *why*, not *what* (the code shows what).
- Can be safely changed by someone other than the original author.

## Glossary

| Term | Definition |
|------|------------|
| Single responsibility | A principle where a module should have exactly one reason to change |
| Invariant | A condition that is always true at a specific point in program execution |
| Magic number | A literal value in code with no explanation of its meaning |
| Tight coupling | A situation where components depend heavily on each other's internals |

## Next Steps

- [Control Flow](../beginner/control-flow.md) — conditionals, loops, and branching basics
