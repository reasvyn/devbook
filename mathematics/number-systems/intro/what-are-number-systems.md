# What Are Number Systems

## Description

Number systems are the sets of numbers that mathematicians and developers work with, each adding new capabilities. From natural numbers (for counting) to complex numbers (for signal processing and quantum mechanics), understanding these sets is foundational to choosing the right abstraction in code.

## Prerequisites

- [Why Math Matters](../../intro/why-math.md)

## Table of Contents

- [Why Different Kinds of Numbers](#why-different-kinds-of-numbers)
- [The Hierarchy](#the-hierarchy)
- [Number Systems in Programming](#number-systems-in-programming)

## Content / Material

### Why Different Kinds of Numbers

Each number system solves a limitation of the previous one:

```
Natural N     → counting, but no subtraction result for 3 - 5
Integers Z    → handles negatives, but no division result for 1 ÷ 3
Rationals Q   → handles fractions, but no √2
Reals R       → handles continuity, but no √(-1)
Complex C     → handles imaginary numbers, but no 1 ÷ 0
```

Each expansion preserves the properties of the previous set while adding new capabilities.

### The Hierarchy

$$ \mathbb{N} \subset \mathbb{Z} \subset \mathbb{Q} \subset \mathbb{R} \subset \mathbb{C} $$

- **N** — Natural numbers: 0, 1, 2, 3, ...
- **Z** — Integers: ..., -2, -1, 0, 1, 2, ...
- **Q** — Rationals: fractions of integers (p/q)
- **R** — Reals: rationals + irrationals
- **C** — Complex: real + imaginary

### Number Systems in Programming

| Type | Language Examples | Space |
|------|------------------|-------|
| Natural | `uint`, `unsigned` | Bounded by hardware |
| Integer | `int`, `i32`, `Int` | Usually 32 or 64-bit |
| Rational | Manual impl, or `fractions.Fraction` | Arbitrary precision |
| Real | `float`, `f64`, `Double` | IEEE 754, finite precision |
| Complex | `complex`, `Complex` | Two floats |

## Glossary

| Term | Definition |
|------|------------|
| Set | A collection of distinct elements |
| Subset | A set whose every element is also in a larger set |
| Closure | A set is closed under an operation if applying it to members always yields a member |

## Next Steps

- [Natural Numbers](beginner/natural-numbers.md) — counting, induction, and discrete math
