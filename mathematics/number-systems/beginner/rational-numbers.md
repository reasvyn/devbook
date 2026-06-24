# Rational Numbers

## Description

Rational numbers ($\mathbb{Q}$) are numbers that can be expressed as a fraction $\frac{p}{q}$ where $p$ and $q$ are integers and $q \neq 0$. They are the first number system that is dense — between any two rationals, there is another rational. In programming, rationals appear in financial calculations, symbolic math, and exact arithmetic.

## Prerequisites

- [Integers](integers.md)

## Table of Contents

- [Definition](#definition)
- [Properties](#properties)
- [Rationals vs. Floating Point](#rationals-vs-floating-point)
- [Rationals in Code](#rationals-in-code)

## Content / Material

### Definition

$$\mathbb{Q} = \\{ \frac{p}{q} \mid p, q \in \mathbb{Z}, q \neq 0 \\}$$

Two fractions $\frac{a}{b}$ and $\frac{c}{d}$ are equal iff $a \cdot d = b \cdot c$.

Every rational number has a **reduced form** where numerator and denominator share no common factors.

### Properties

| Property | Description |
|----------|-------------|
| **Closed under +, −, ×, ÷** | Adding, subtracting, multiplying, or dividing rationals (by non-zero) yields a rational |
| **Dense** | Between any two rationals there is another rational |
| **Countable** | Surprisingly, rationals are countable (same size as naturals) |
| **Not complete** | There are "gaps" — irrational numbers like $\sqrt{2}$ |

### Rationals vs. Floating Point

```python
# Floating point (imprecise)
0.1 + 0.2  # → 0.30000000000000004

# Rational (exact)
from fractions import Fraction
Fraction(1, 10) + Fraction(1, 5)  # → Fraction(3, 10)
```

| Aspect | Floating Point | Rational |
|--------|---------------|----------|
| **Precision** | Finite (≈15-17 decimal digits) | Exact |
| **Performance** | Hardware-accelerated | Slower (big integer ops) |
| **Memory** | 4-8 bytes per value | Grows with numerator/denominator |
| **Use case** | Graphics, ML, physics | Finance, symbolic math |

### Rationals in Code

```javascript
// JavaScript has no built-in rational — use libraries
// or represent as { num: number, den: number }

// Rust's `num` crate
use num::rational::Ratio;
let half = Ratio::new(1, 2);
let third = Ratio::new(1, 3);
println!("{}", half + third);  // 5/6
```

## Glossary

| Term | Definition |
|------|------------|
| Rational | A number expressible as a ratio of integers |
| Reduced form | A fraction with coprime numerator and denominator |
| Dense | Between any two elements there is another element of the same set |

## Next Steps

- [Irrational Numbers](irrational-numbers.md) — numbers that cannot be written as fractions
