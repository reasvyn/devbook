# What Is Calculus

## Description

Calculus is the study of continuous change. It has two core branches — differential calculus (rates of change) and integral calculus (accumulation). For developers, calculus powers gradient descent (training neural networks), physics simulations, optimization algorithms, and any system involving continuous dynamics.

## Prerequisites

- [Why Math Matters](../../intro/why-math.md)

## Table of Contents

- [The Two Core Ideas](#the-two-core-ideas)
- [Why Calculus Matters for Developers](#why-calculus-matters-for-developers)
- [The Mindset: Local to Global](#the-mindset-local-to-global)

## Content / Material

### The Two Core Ideas

**Differential calculus** answers: how fast is something changing at a specific instant?

- The **derivative** $f'(x)$ gives the slope of $f$ at point $x$.
- Geometrically: the tangent line at a point.
- In code: "How much does the output change if I tweak this input by a tiny amount?"

**Integral calculus** answers: how much has accumulated over an interval?

- The **integral** $\int_a^b f(x) dx$ gives the area under $f$ from $a$ to $b$.
- Geometrically: the area under a curve.
- In code: "What's the total over time if I know the rate at each moment?"

The Fundamental Theorem of Calculus connects them: differentiation and integration are inverse operations.

### Why Calculus Matters for Developers

- **Machine learning**: gradient descent uses derivatives to minimize loss functions.
- **Physics engines**: velocity is the derivative of position. Acceleration is the derivative of velocity.
- **Computer graphics**: Bezier curves and splines are defined by polynomial functions.
- **Optimization**: finding maxima and minima of functions (cost, profit, error).
- **Probability**: probability density functions integrate to give probabilities.

### The Mindset: Local to Global

Calculus teaches a powerful mental model: understand local behavior (derivatives) to predict global behavior (integrals). This pattern appears everywhere:

- A profiler tells you where time is spent (local). Summing gives total runtime (global).
- A sensor measures velocity (local). Accumulating gives position (global).
- A gradient shows the steepest direction (local). Following it finds a minimum (global).

## Glossary

| Term | Definition |
|------|------------|
| Derivative | The instantaneous rate of change of a function |
| Integral | The accumulation of a quantity over an interval |
| Gradient | A vector of partial derivatives pointing in the direction of steepest ascent |
| Gradient descent | An optimization algorithm using gradients to minimize a function |

## Next Steps

- [Derivatives](../beginner/derivatives.md) — rules, interpretation, and computation
- [Integrals](../beginner/integrals.md) — accumulation, area, and the FTC
