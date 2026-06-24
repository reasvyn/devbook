# Computational Thinking

## Description

Computational thinking is the cornerstone of computer science — a way of solving problems, designing systems, and understanding human behavior by drawing on the concepts fundamental to computing. It is not about writing code; it is about thinking like a computer scientist.

## Prerequisites

- [What Is Computer Science](what-is-cs.md)

## Table of Contents

- [The Four Pillars](#the-four-pillars)
- [Decomposition](#decomposition)
- [Pattern Recognition](#pattern-recognition)
- [Abstraction](#abstraction)
- [Algorithm Design](#algorithm-design)

## Content / Material

### The Four Pillars

Computational thinking rests on four pillars:

1. **Decomposition** — break a complex problem into smaller parts.
2. **Pattern recognition** — identify similarities and regularities.
3. **Abstraction** — focus on the essential, ignore the irrelevant.
4. **Algorithm design** — create step-by-step solutions.

These four skills apply whether you're writing a function, designing an API, or debugging a distributed system.

### Decomposition

A problem that seems overwhelming becomes manageable when split into pieces.

```
Problem: "Build a social media platform"
→ Authentication, Feed, Notifications, Messaging, Search
  → Each module can be broken further into sub-components
```

Good decomposition produces independent, loosely coupled pieces — the same principle as well-designed functions and microservices.

### Pattern Recognition

Recognizing patterns means you don't solve the same problem twice.

- "This looks like a cache miss" → apply caching strategies.
- "This is the same SQL query pattern" → use a repository or query builder.
- "This state machine matches a finite automaton" → apply state machine theory.

The more patterns you recognize, the faster you reach correct solutions.

### Abstraction

Abstraction removes irrelevant detail to reveal the essence of a problem.

```
Email: you type a message and hit send.
Abstraction: you don't think about SMTP, MX records, or TCP handshakes.
```

In code, abstraction is what lets you call `fetch(url)` without knowing how the OS opens a socket, negotiates TLS, or handles retransmission.

### Algorithm Design

An algorithm is a precise, unambiguous sequence of steps to solve a problem. Algorithmic thinking means:

- Defining inputs and outputs clearly.
- Ensuring each step is executable.
- Proving the algorithm terminates and is correct.
- Considering efficiency before writing code.

## Glossary

| Term | Definition |
|------|------------|
| Decomposition | Breaking a problem into smaller, manageable parts |
| Pattern recognition | Identifying commonalities across different problems |
| Algorithm | A finite sequence of well-defined steps to solve a problem |
| Computational thinking | A problem-solving approach based on CS fundamentals |

## Next Steps

- [Abstraction as a Principle](abstraction-as-a-principle.md) — the power and danger of hiding complexity
- [Data Structures](../beginner/data-structures.md) — fundamental containers for organizing data
