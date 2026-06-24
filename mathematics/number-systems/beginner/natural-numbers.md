# Natural Numbers

## Description

Natural numbers ($\mathbb{N}$) are the numbers we count with: 0, 1, 2, 3, ... They are the first number system humans invented and the foundation of all discrete mathematics. In programming, natural numbers appear as array indices, loop counters, and unsigned integers.

## Prerequisites

- [What Are Number Systems](../intro/what-are-number-systems.md)

## Table of Contents

- [Definition](#definition)
- [Properties](#properties)
- [Mathematical Induction](#mathematical-induction)
- [Natural Numbers in Code](#natural-numbers-in-code)

## Content / Material

### Definition

There are two conventions:

- $\mathbb{N} = \\{0, 1, 2, 3, ...\\}$ (includes zero — common in computer science)
- $\mathbb{N} = \\{1, 2, 3, ...\\}$ (excludes zero — traditional mathematics)

Computer scientists typically include zero because array indices start at 0.

### Properties

| Property | Description |
|----------|-------------|
| **Well-ordered** | Every non-empty subset has a smallest element |
| **Discrete** | Each number has a unique successor |
| **Countable** | There are infinitely many, but you can enumerate them |
| **Closed under + and ×** | Adding or multiplying naturals always yields a natural |

### Mathematical Induction

Induction is a proof technique that works because naturals are well-ordered:

1. **Base case**: prove $P(0)$ is true.
2. **Inductive step**: prove if $P(k)$ is true, then $P(k+1)$ is true.
3. **Conclusion**: $P(n)$ is true for all $n \in \mathbb{N}$.

```python
# Induction in code: recursive functions
def sum_to_n(n: int) -> int:
    if n == 0:           # base case
        return 0
    return n + sum_to_n(n - 1)  # inductive step
```

### Natural Numbers in Code

```javascript
// Loop counter (natural)
for (let i = 0; i < arr.length; i++) {
    // i is a natural number
}

// Unsigned integer (bounded natural)
let index: u32 = 42;

// Recursion (inductive structure)
function factorial(n: number): number {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```

## Glossary

| Term | Definition |
|------|------------|
| Natural numbers | Counting numbers, optionally including zero |
| Induction | A proof technique proving a statement for all naturals |
| Well-ordered | Every subset has a least element |
| Countable | Can be put in one-to-one correspondence with naturals |

## Next Steps

- [Integers](integers.md) — adding negative numbers to the mix
