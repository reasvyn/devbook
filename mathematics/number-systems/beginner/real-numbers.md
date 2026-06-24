# Real Numbers

## Description

Real numbers ($\mathbb{R}$) include every rational and irrational number — every point on the continuous number line. They are the default "numbers" in most mathematical modeling. In computing, reals are approximated by floating-point numbers, with all the precision trade-offs that entails.

## Prerequisites

- [Irrational Numbers](irrational-numbers.md)

## Table of Contents

- [Definition](#definition)
- [Properties](#properties)
- [The Real Line](#the-real-line)
- [Reals in Code: Floating Point](#reals-in-code-floating-point)

## Content / Material

### Definition

$\mathbb{R}$ is the set of all numbers that can be represented as infinite decimals:

$$ \mathbb{R} = \\{ n \cdot 10^k + d_1 \cdot 10^{k-1} + ... + d_0 + d_{-1} \cdot 10^{-1} + ... \\} $$

Equivalently: the set of all points on the continuous number line.

There is no simple "construction" of reals — they are defined by **completeness**: every non-empty set of reals with an upper bound has a least upper bound (supremum).

### Properties

| Property | Description |
|----------|-------------|
| **Complete** | No gaps — every Cauchy sequence converges |
| **Uncountable** | Cannot be paired one-to-one with naturals (Cantor's diagonal argument) |
| **Ordered** | Any two reals can be compared ($a < b$, $a = b$, or $a > b$) |
| **Archimedean** | For any real $x$, there exists a natural $n$ such that $n > x$ |

### The Real Line

```
    -∞                    0                    +∞
─────┼─────────────────────┼─────────────────────┼─────
     ...   -√2   -1   0   ½   √2   e   π   4   ...
```

Every point on this line is a real number. There are infinitely many points between any two reals — the reals are a *continuum*.

### Reals in Code: Floating Point

Computers approximate reals with floating-point numbers (IEEE 754):

```python
import sys

# Machine epsilon: smallest difference between two floats
eps = sys.float_info.epsilon  # 2.22e-16

# Never compare floats directly
a = 0.1 + 0.2
b = 0.3
print(a == b)          # False — floating point error
print(abs(a - b) < 1e-15)  # True — approximate comparison
```

| Characteristic | Real Numbers | IEEE 754 Floats |
|----------------|-------------|-----------------|
| Continuity | Continuous | Discrete (≈ 2³² values) |
| Precision | Infinite | ≈ 7-16 decimal digits |
| Range | Unlimited | ≈ 10⁻³⁰⁸ to 10³⁰⁸ |
| Associativity | $a + (b + c) = (a + b) + c$ | Not guaranteed |

## Glossary

| Term | Definition |
|------|------------|
| Real number | Any number on the continuous number line |
| Completeness | Property that the real line has no gaps |
| IEEE 754 | The standard for floating-point arithmetic in computers |
| Machine epsilon | The maximum relative error of rounding to the nearest float |

## Next Steps

- [Complex Numbers](complex-numbers.md) — adding the imaginary dimension
