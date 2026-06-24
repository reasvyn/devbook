# Integers

## Description

Integers ($\mathbb{Z}$) extend the natural numbers with negative values: $\\{..., -3, -2, -1, 0, 1, 2, 3, ...\\}$. They are the smallest number system closed under subtraction. In programming, integers are the default numeric type in most languages.

## Prerequisites

- [Natural Numbers](natural-numbers.md)

## Table of Contents

- [Definition](#definition)
- [Properties](#properties)
- [Integer Overflow](#integer-overflow)
- [Integers in Code](#integers-in-code)

## Content / Material

### Definition

$\mathbb{Z} = \\{..., -3, -2, -1, 0, 1, 2, 3, ...\\}$

The Z stands for *Zahlen* (German for "numbers").

Integers can be constructed from naturals as equivalence classes of pairs:

$$ (a, b) \sim (c, d) \iff a + d = b + c $$

Intuitively: the pair $(a, b)$ represents $a - b$.

### Properties

| Property | Description |
|----------|-------------|
| **Closed under +, −, ×** | Adding, subtracting, or multiplying integers always yields an integer |
| **Not closed under ÷** | $1 \div 2$ is not an integer |
| **Ordered** | Integers are totally ordered ($... < -2 < -1 < 0 < 1 < 2 < ...$) |
| **Countable** | Same cardinality as naturals (they can be paired one-to-one) |
| **Discrete** | No integers exist between $n$ and $n + 1$ |

### Integer Overflow

Programming integers are bounded by fixed bit widths:

```c
// 8-bit signed integer: range -128 to 127
int8_t x = 127;
x = x + 1;  // overflow! x becomes -128
```

| Type | Size | Range |
|------|------|-------|
| `int8` / `sbyte` | 8 bits | -128 to 127 |
| `int16` / `short` | 16 bits | -32,768 to 32,767 |
| `int32` / `int` | 32 bits | ≈ -2.1B to 2.1B |
| `int64` / `long` | 64 bits | ≈ -9.2 × 10¹⁸ to 9.2 × 10¹⁸ |

### Integers in Code

```python
# Python integers are arbitrary precision
x = 42
y = -7
z = x ** 100  # no overflow in Python

# Type matters in statically typed languages
let a: i32 = 2_147_483_647;
let b: i64 = 9_223_372_036_854_775_807;
```

## Glossary

| Term | Definition |
|------|------------|
| Integer | A whole number, possibly negative |
| Overflow | When a computation exceeds the representable range |
| Signed | Integers that can be negative (uses one bit for sign) |
| Unsigned | Integers that are always non-negative |

## Next Steps

- [Rational Numbers](rational-numbers.md) — adding fractions to the number line
