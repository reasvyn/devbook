# Irrational Numbers

## Description

Irrational numbers are real numbers that cannot be expressed as a fraction of integers. They have non-repeating, non-terminating decimal expansions. Famous irrationals include $\pi$, $e$, $\sqrt{2}$, and the golden ratio $\phi$. In computing, irrationals appear everywhere — but must be approximated.

## Prerequisites

- [Rational Numbers](rational-numbers.md)

## Table of Contents

- [Definition](#definition)
- [Famous Irrationals](#famous-irrationals)
- [Proof by Contradiction: $\sqrt{2}$ Is Irrational](#proof-by-contradiction-sqrt2-is-irrational)
- [Irrationals in Computing](#irrationals-in-computing)

## Content / Material

### Definition

A real number is **irrational** if it cannot be written as $\frac{p}{q}$ where $p, q \in \mathbb{Z}$ and $q \neq 0$.

Equivalently: its decimal expansion never terminates and never repeats.

$$
\sqrt{2} = 1.4142135623730950488... \quad \text{(non-repeating, non-terminating)}
$$

### Famous Irrationals

| Number | Value (approx) | Where It Appears |
|--------|---------------|------------------|
| $\pi$ | 3.14159... | Circles, trigonometry, signal processing |
| $e$ | 2.71828... | Exponential growth, compound interest, probability |
| $\sqrt{2}$ | 1.41421... | Geometry, A4 paper ratio, diagonal of a square |
| $\phi$ | 1.61803... | Golden ratio, art, nature, optimization |
| $\sqrt{3}$ | 1.73205... | 2D/3D geometry, trigonometry |

### Proof by Contradiction: $\sqrt{2}$ Is Irrational

1. Assume $\sqrt{2} = \frac{p}{q}$ in reduced form.
2. Square both sides: $2 = \frac{p^2}{q^2} \implies p^2 = 2q^2$.
3. So $p^2$ is even, therefore $p$ is even. Write $p = 2k$.
4. Substituting: $(2k)^2 = 2q^2 \implies 4k^2 = 2q^2 \implies q^2 = 2k^2$.
5. So $q^2$ is even, therefore $q$ is even.
6. Both $p$ and $q$ are even — contradiction (fraction was in reduced form).
7. Therefore $\sqrt{2}$ cannot be rational.

### Irrationals in Computing

Computers cannot represent irrational numbers exactly. All irrationals in code are approximations:

```python
import math

# pi is approximated to ~15 decimal digits
print(math.pi)        # 3.141592653589793
print(math.pi == 3.141592653589793)  # True

# floating point precision limits
print(0.1 + 0.2)      # 0.30000000000000004
```

Libraries like `mpmath` (Python) or `BigDecimal` (Java) provide more precision but never exact representation of irrationals.

## Glossary

| Term | Definition |
|------|------------|
| Irrational | A real number that is not rational |
| Proof by contradiction | Assume the opposite, derive an impossibility |
| Approximation | A value close to the true value but not equal |

## Next Steps

- [Real Numbers](real-numbers.md) — the union of rationals and irrationals
