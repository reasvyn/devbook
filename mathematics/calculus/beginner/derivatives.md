# Derivatives

## Description

A derivative measures how a function changes as its input changes — the instantaneous rate of change. Derivatives are the foundation of gradient descent (machine learning), physics simulations, and optimization.

## Prerequisites

- [What Is Calculus](../intro/what-is-calculus.md)

## Table of Contents

- [Definition](#definition)
- [Basic Rules](#basic-rules)
- [Partial Derivatives](#partial-derivatives)
- [Gradient Descent](#gradient-descent)

## Content / Material

### Definition

The derivative of $f$ at $x$ is the limit of the slope between $x$ and $x + h$ as $h$ approaches zero:

$$ f'(x) = \lim_{h \to 0} \frac{f(x + h) - f(x)}{h} $$

Geometrically: the slope of the tangent line at point $x$.

### Basic Rules

| Rule | Formula |
|------|---------|
| Constant | $\frac{d}{dx} c = 0$ |
| Power | $\frac{d}{dx} x^n = n x^{n-1}$ |
| Sum | $(f + g)' = f' + g'$ |
| Product | $(f \cdot g)' = f' \cdot g + f \cdot g'$ |
| Chain | $(f \circ g)' = f'(g(x)) \cdot g'(x)$ |

```python
import sympy as sp
x = sp.Symbol('x')
sp.diff(x**3 + 2*x**2 - 5*x + 1, x)  # 3*x**2 + 4*x - 5
```

### Partial Derivatives

For functions with multiple inputs, a partial derivative measures change in one variable while holding others constant:

$$ f(x, y) = x^2 y + y^3 $$

$$ \frac{\partial f}{\partial x} = 2xy, \quad \frac{\partial f}{\partial y} = x^2 + 3y^2 $$

### Gradient Descent

The **gradient** $\nabla f$ is a vector of all partial derivatives, pointing in the direction of steepest ascent. Gradient descent moves opposite to the gradient to minimize a function:

```python
def gradient_descent(f_grad, start, lr=0.01, steps=100):
    x = start
    for _ in range(steps):
        x -= lr * f_grad(x)
    return x
```

This is how neural networks learn — backpropagation computes gradients, and gradient descent updates weights.

## Glossary

| Term | Definition |
|------|------------|
| Derivative | Instantaneous rate of change of a function |
| Gradient | A vector of partial derivatives |
| Gradient descent | Optimization algorithm using gradient to minimize a function |
| Chain rule | Rule for differentiating composed functions |

## Next Steps

- [Integrals](integrals.md) — accumulation and area under curves
