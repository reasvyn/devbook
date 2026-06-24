# Integrals

## Description

Integration is the inverse of differentiation. It measures accumulation — area under a curve, total distance traveled, total cost accumulated. For developers, integrals appear in probability (areas under PDFs), physics engines, signal processing, and optimization.

## Prerequisites

- [Derivatives](derivatives.md)

## Table of Contents

- [Definition](#definition)
- [Basic Rules](#basic-rules)
- [Definite vs. Indefinite](#definite-vs-indefinite)
- [The Fundamental Theorem of Calculus](#the-fundamental-theorem-of-calculus)
- [Numerical Integration](#numerical-integration)

## Content / Material

### Definition

The integral of $f(x)$ from $a$ to $b$ is the area under the curve:

$$ \int_a^b f(x) \, dx = \lim_{n \to \infty} \sum_{i=1}^n f(x_i^*) \Delta x $$

This is the limit of Riemann sums — approximating area with increasingly thin rectangles.

### Basic Rules

| Rule | Formula |
|------|---------|
| Power | $\int x^n dx = \frac{x^{n+1}}{n+1} + C$ |
| Constant | $\int c \cdot f(x) dx = c \int f(x) dx$ |
| Sum | $\int (f + g) dx = \int f dx + \int g dx$ |
| Exponential | $\int e^x dx = e^x + C$ |

### Definite vs. Indefinite

**Indefinite integral** — a family of functions (has $+ C$):

$$ \int 2x \, dx = x^2 + C $$

**Definite integral** — a number (area):

$$ \int_0^3 2x \, dx = [x^2]_0^3 = 9 - 0 = 9 $$

### The Fundamental Theorem of Calculus

Connects differentiation and integration:

1. $\frac{d}{dx} \int_a^x f(t) dt = f(x)$ — derivative of an integral gives the original function.
2. $\int_a^b f(x) dx = F(b) - F(a)$ where $F' = f$ — evaluate the antiderivative at endpoints.

This means integration and differentiation are inverse operations.

### Numerical Integration

Many functions have no closed-form antiderivative. Numerical methods approximate the integral:

```python
import numpy as np

def trapezoidal(f, a, b, n=1000):
    """Approximate ∫_a^b f(x) dx using trapezoidal rule."""
    x = np.linspace(a, b, n)
    y = f(x)
    return np.trapz(y, x)

# Example: ∫_0^π sin(x) dx should be 2
area = trapezoidal(np.sin, 0, np.pi)
print(area)  # ≈ 2.0
```

## Glossary

| Term | Definition |
|------|------------|
| Integral | The accumulation of a quantity over an interval |
| Antiderivative | A function whose derivative equals the original function |
| Riemann sum | Approximating area using rectangles |
| FTC | Fundamental Theorem of Calculus — connects derivatives and integrals |

## Next Steps

- [Basic Probability](../../probability-statistics/beginner/basic-probability.md) — integrals as probability
