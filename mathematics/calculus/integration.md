# Integration

## Description

Integration is the inverse of differentiation — it computes areas under curves, accumulated quantities, and antiderivatives. Every developer encounters integration in probability (cumulative distributions), physics engines (position from velocity), signal processing (Fourier transforms), and numerical computing.

## Prerequisites

- [Differentiation](differentiation.md) — the fundamental theorem connects derivatives and integrals

## Table of Contents

- [Indefinite Integrals](#indefinite-integrals)
- [Definite Integrals](#definite-integrals)
- [The Fundamental Theorem of Calculus](#the-fundamental-theorem-of-calculus)
- [Integration by Substitution](#integration-by-substitution)
- [Integration by Parts](#integration-by-parts)
- [Partial Fractions](#partial-fractions)
- [Trigonometric Substitution](#trigonometric-substitution)
- [Improper Integrals](#improper-integrals)
- [Numerical Integration](#numerical-integration)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Indefinite Integrals

An indefinite integral (antiderivative) of $f$ is a function $F$ such that $F'(x) = f(x)$.

$$
\int f(x) \, dx = F(x) + C
$$

The constant $C$ reminds us that antiderivatives differ by a constant.

**Basic integration rules:**

| Rule | Formula |
|------|---------|
| Power rule | $\int x^n \, dx = \frac{x^{n+1}}{n+1} + C$, $n \neq -1$ |
| Reciprocal | $\int \frac{1}{x} \, dx = \ln\|x\| + C$ |
| Exponential | $\int e^x \, dx = e^x + C$ |
| Sine | $\int \sin x \, dx = -\cos x + C$ |
| Cosine | $\int \cos x \, dx = \sin x + C$ |

```python
import sympy as sp

x = sp.Symbol('x')
expressions = [
    x**3,
    sp.sin(x),
    sp.exp(x),
    1/x,
    3*x**2 + 2*x + 1,
]

for expr in expressions:
    integral = sp.integrate(expr, x)
    print(f"∫ {sp.latex(expr)} dx = {sp.latex(integral)} + C")
```

### Definite Integrals

The definite integral $\int_a^b f(x) \, dx$ represents the signed area under $f$ from $x = a$ to $x = b$.

$$
\int_a^b f(x) \, dx = \lim_{n \to \infty} \sum_{i=1}^n f(x_i^*) \Delta x
$$

This limit of Riemann sums is how integration is defined formally.

**Properties:**

| Property | Formula |
|----------|---------|
| Reversal | $\int_a^b f = -\int_b^a f$ |
| Zero width | $\int_a^a f = 0$ |
| Constant multiple | $\int_a^b cf = c\int_a^b f$ |
| Sum | $\int_a^b (f+g) = \int_a^b f + \int_a^b g$ |
| Additivity | $\int_a^b f + \int_b^c f = \int_a^c f$ |
| Comparison | If $f \leq g$, then $\int_a^b f \leq \int_a^b g$ |

### The Fundamental Theorem of Calculus

**Part 1 (FTC1):** If $F'(x) = f(x)$, then:

$$
\int_a^b f(x) \, dx = F(b) - F(a)
$$

**Part 2 (FTC2):** If $F(x) = \int_a^x f(t) \, dt$, then $F'(x) = f(x)$.

The FTC bridges differentiation and integration — they are inverse operations.

```python
import numpy as np

def ftc_demo():
    """Demonstrate FTC by computing area under sin(x) from 0 to pi.
    ∫ sin(x) dx = -cos(x), so ∫_0^π sin(x) dx = -cos(π) - (-cos(0)) = 2
    """
    f = lambda x: np.sin(x)
    F = lambda x: -np.cos(x)  # antiderivative

    a, b = 0, np.pi
    exact = F(b) - F(a)
    print(f"Exact area: {exact:.10f} (expected 2)")

    # Numerical verification
    xs = np.linspace(a, b, 100000)
    dx = xs[1] - xs[0]
    riemann = np.sum(f(xs)) * dx
    print(f"Riemann sum: {riemann:.10f}")

ftc_demo()
```

### Integration by Substitution

The chain rule in reverse:

$$
\int f(g(x)) g'(x) \, dx = \int f(u) \, du, \quad u = g(x)
$$

**Example:**

$$
\int 2x \cos(x^2) \, dx
$$

Let $u = x^2$, $du = 2x \, dx$:

$$
\int \cos(u) \, du = \sin(u) + C = \sin(x^2) + C
$$

```python
import sympy as sp

x = sp.Symbol('x')
expr = 2*x * sp.cos(x**2)
integral = sp.integrate(expr, x)
print(f"∫ 2x cos(x^2) dx = {sp.latex(integral)} + C")

# Definite example
definite = sp.integrate(expr, (x, 0, sp.sqrt(sp.pi/2)))
print(f"∫_0^{{π/2}} 2x cos(x^2) dx = {definite}")
```

**In code — substitution for numerical integration:**

```python
import numpy as np

def integrate_by_substitution(f, a, b, subs):
    """Numerical integration after change of variables x = g(u)."""
    # ∫_a^b f(x) dx = ∫_{g^{-1}(a)}^{g^{-1}(b)} f(g(u)) g'(u) du
    pass  # see numerical methods below
```

### Integration by Parts

The product rule in reverse:

$$
\int u \, dv = uv - \int v \, du
$$

**Example:** $\int x e^x \, dx$

Let $u = x$, $dv = e^x dx$. Then $du = dx$, $v = e^x$.

$$
\int x e^x \, dx = x e^x - \int e^x \, dx = x e^x - e^x + C
$$

```python
import sympy as sp

x = sp.Symbol('x')

# ∫ x e^x dx
expr1 = x * sp.exp(x)
print(f"∫ x e^x dx = {sp.latex(sp.integrate(expr1, x))} + C")

# ∫ x^2 sin(x) dx
expr2 = x**2 * sp.sin(x)
print(f"∫ x^2 sin(x) dx = {sp.latex(sp.integrate(expr2, x))} + C")

# ∫ ln(x) dx
expr3 = sp.log(x)
print(f"∫ ln(x) dx = {sp.latex(sp.integrate(expr3, x))} + C")
```

**Tabular integration:** For repeated integration by parts, use a table:

```python
import sympy as sp

# ∫ x^3 e^x dx — requires integration by parts 3 times
x = sp.Symbol('x')
expr = x**3 * sp.exp(x)
result = sp.integrate(expr, x)
print(f"∫ x^3 e^x dx = {sp.latex(result)} + C")
```

### Partial Fractions

For rational functions $\frac{P(x)}{Q(x)}$, decompose into simpler fractions that integrate easily.

**Method:**

1. Factor the denominator.
2. Write the decomposition.
3. Solve for coefficients.
4. Integrate each term.

**Example:**

$$
\int \frac{x + 2}{x^2 - 3x + 2} \, dx = \int \frac{x + 2}{(x-1)(x-2)} \, dx
$$

Decompose:

$$
\frac{x + 2}{(x-1)(x-2)} = \frac{A}{x-1} + \frac{B}{x-2}
$$

Solve: $A = -3$, $B = 4$.

$$
\int \frac{x + 2}{x^2 - 3x + 2} \, dx = -3\ln|x-1| + 4\ln|x-2| + C
$$

```python
import sympy as sp

x = sp.Symbol('x')
expr = (x + 2) / (x**2 - 3*x + 2)
decomposed = sp.apart(expr, x)
print(f"Partial fraction decomposition: {sp.latex(decomposed)}")
result = sp.integrate(expr, x)
print(f"Integral: {sp.latex(result)} + C")
```

### Trigonometric Substitution

Used when integrating expressions with $\sqrt{a^2 - x^2}$, $\sqrt{a^2 + x^2}$, or $\sqrt{x^2 - a^2}$.

| Expression | Substitution | Result |
|-----------|-------------|--------|
| $\sqrt{a^2 - x^2}$ | $x = a\sin\theta$ | $dx = a\cos\theta\,d\theta$ |
| $\sqrt{a^2 + x^2}$ | $x = a\tan\theta$ | $dx = a\sec^2\theta\,d\theta$ |
| $\sqrt{x^2 - a^2}$ | $x = a\sec\theta$ | $dx = a\sec\theta\tan\theta\,d\theta$ |

**Example:**

$$
\int \frac{1}{\sqrt{1 - x^2}} \, dx
$$

Let $x = \sin\theta$, $dx = \cos\theta\,d\theta$:

$$
\int \frac{\cos\theta}{\sqrt{1 - \sin^2\theta}} \, d\theta = \int \frac{\cos\theta}{\cos\theta} \, d\theta = \int d\theta = \theta + C = \arcsin x + C
$$

```python
import sympy as sp

x = sp.Symbol('x')

# ∫ 1/sqrt(1-x^2) dx
expr1 = 1 / sp.sqrt(1 - x**2)
print(f"∫ 1/√(1-x^2) dx = {sp.latex(sp.integrate(expr1, x))} + C")

# ∫ 1/(1+x^2) dx
expr2 = 1 / (1 + x**2)
print(f"∫ 1/(1+x^2) dx = {sp.latex(sp.integrate(expr2, x))} + C")

# ∫ sqrt(1 - x^2) dx — area of a semicircle
expr3 = sp.sqrt(1 - x**2)
print(f"∫ √(1-x^2) dx = {sp.latex(sp.integrate(expr3, x))} + C")
```

### Improper Integrals

Integrals with infinite limits or integrands that blow up within the interval.

**Type 1: Infinite limits**

$$
\int_1^\infty \frac{1}{x^2} \, dx = \lim_{t \to \infty} \int_1^t \frac{1}{x^2} \, dx = \lim_{t \to \infty} \left[-\frac{1}{x}\right]_1^t = 1
$$

**Type 2: Discontinuous integrand**

$$
\int_0^1 \frac{1}{\sqrt{x}} \, dx = \lim_{t \to 0^+} \int_t^1 \frac{1}{\sqrt{x}} \, dx = \lim_{t \to 0^+} [2\sqrt{x}]_t^1 = 2
$$

```python
import sympy as sp
import numpy as np

x = sp.Symbol('x')

# ∫_1^∞ 1/x^2 dx
improper1 = sp.integrate(1/x**2, (x, 1, sp.oo))
print(f"∫_1^∞ 1/x^2 dx = {improper1}")

# ∫_0^1 1/√x dx
improper2 = sp.integrate(1/sp.sqrt(x), (x, 0, 1))
print(f"∫_0^1 1/√x dx = {improper2}")

# ∫_-∞^∞ e^{-x^2} dx = √π (Gaussian integral)
improper3 = sp.integrate(sp.exp(-x**2), (x, -sp.oo, sp.oo))
print(f"∫_-∞^∞ e^(-x^2) dx = {improper3}")

# Numerical check for convergence
def improper_check_numerical(f, a, b, tol=1e-10):
    """Check if an improper integral converges numerically."""
    # For infinite limits, integrate over increasingly large bounds
    if np.isinf(b):
        t = 10.0
        prev = 0.0
        while t < 1e10:
            xs = np.linspace(a, t, 100000)
            dx = xs[1] - xs[0]
            area = np.sum(f(xs)) * dx
            if abs(area - prev) < tol:
                return area, True
            prev = area
            t *= 2
        return prev, False
    return None, False

result_numeric, converged = improper_check_numerical(lambda x: np.exp(-x**2), 0, np.inf)
print(f"Numerical ∫_0^∞ e^(-x^2) dx ≈ {result_numeric:.6f} (expected √π/2 ≈ {np.sqrt(np.pi)/2:.6f})")
```

### Numerical Integration

When an antiderivative is impossible (most functions) or impractical, we integrate numerically.

**Riemann sums:** Simplest approach — sum of rectangles.

Left Riemann sum: $\sum_{i=0}^{n-1} f(x_i) \Delta x$

Right Riemann sum: $\sum_{i=1}^{n} f(x_i) \Delta x$

Midpoint rule: $\sum_{i=0}^{n-1} f\left(\frac{x_i + x_{i+1}}{2}\right) \Delta x$

```python
import numpy as np

def riemann_sum(f, a, b, n=1000, method='midpoint'):
    """Compute Riemann sum of f from a to b with n intervals."""
    xs = np.linspace(a, b, n + 1)
    dx = (b - a) / n

    if method == 'left':
        return np.sum(f(xs[:-1])) * dx
    elif method == 'right':
        return np.sum(f(xs[1:])) * dx
    elif method == 'midpoint':
        mids = (xs[:-1] + xs[1:]) / 2
        return np.sum(f(mids)) * dx
    else:
        raise ValueError(f"Unknown method: {method}")

# ∫_0^π sin(x) dx = 2
for method in ['left', 'right', 'midpoint']:
    approx = riemann_sum(np.sin, 0, np.pi, n=1000, method=method)
    print(f"{method:>8} Riemann sum: {approx:.8f} (error: {abs(approx - 2):.2e})")
```

**Trapezoidal rule:** Averages left and right Riemann sums — uses trapezoids.

$$
\int_a^b f(x) \, dx \approx \frac{\Delta x}{2} \left[ f(a) + 2\sum_{i=1}^{n-1} f(x_i) + f(b) \right]
$$

```python
def trapezoidal(f, a, b, n=1000):
    """Trapezoidal rule integration."""
    xs = np.linspace(a, b, n + 1)
    dx = (b - a) / n
    return dx * (0.5 * f(xs[0]) + 0.5 * f(xs[-1]) + np.sum(f(xs[1:-1])))

area_trap = trapezoidal(np.sin, 0, np.pi, n=100)
print(f"Trapezoidal: {area_trap:.8f} (error: {abs(area_trap - 2):.2e})")
```

**Simpson's rule:** Uses quadratic approximations over each pair of intervals. More accurate than trapezoidal for smooth functions.

$$
\int_a^b f(x) \, dx \approx \frac{\Delta x}{3} \left[ f(a) + 4\sum_{i=1,3,5,\dots} f(x_i) + 2\sum_{i=2,4,6,\dots} f(x_i) + f(b) \right]
$$

```python
def simpson(f, a, b, n=1000):
    """Simpson's rule integration. n must be even."""
    if n % 2 != 0:
        n += 1
    xs = np.linspace(a, b, n + 1)
    dx = (b - a) / n
    result = f(xs[0]) + f(xs[-1])
    result += 4 * np.sum(f(xs[1:-1:2]))  # odd indices
    result += 2 * np.sum(f(xs[2:-1:2]))  # even indices
    return result * dx / 3

area_simp = simpson(np.sin, 0, np.pi, n=100)
print(f"Simpson: {area_simp:.10f} (error: {abs(area_simp - 2):.2e})")
```

**Using scipy.integrate:**

```python
from scipy import integrate
import numpy as np

# Basic quadrature
result, error = integrate.quad(np.sin, 0, np.pi)
print(f"Scipy quad: {result:.15f}, error estimate: {error:.2e}")

# Multiple integration
def f_2d(x, y):
    return np.sin(x) * np.cos(y)

# Double integral ∫_0^π ∫_0^π sin(x) cos(y) dx dy
result_2d, err = integrate.nquad(lambda x, y: np.sin(x) * np.cos(y),
                                 [[0, np.pi], [0, np.pi]])
print(f"Double integral: {result_2d:.6f} (expected 2.0)")

# Integration with parameters
def integrand(x, a, b):
    return a * np.sin(x) + b * np.cos(x)

result_param, _ = integrate.quad(integrand, 0, np.pi, args=(2, 3))
print(f"Parametric integral: {result_param:.6f}")
```

**Convergence comparison:**

```python
import numpy as np

def convergence_demo(f, a, b, exact, ns=[10, 100, 1000, 10000]):
    """Compare convergence rates of numerical integration methods."""
    print(f"{'n':>6} {'Riemann':>12} {'Trapezoidal':>14} {'Simpson':>14}")
    print("-" * 50)
    for n in ns:
        riemann = riemann_sum(f, a, b, n, 'midpoint')
        trap = trapezoidal(f, a, b, n)
        simp = simpson(f, a, b, n)
        print(f"{n:>6} {abs(riemann - exact):>12.2e} {abs(trap - exact):>14.2e} {abs(simp - exact):>14.2e}")

# ∫_0^1 x^2 dx = 1/3
convergence_demo(lambda x: x**2, 0, 1, 1/3)
```

Output:

```
     n     Riemann    Trapezoidal       Simpson
--------------------------------------------------
    10    8.33e-05     8.33e-04     8.33e-06
   100    8.33e-07     8.33e-05     8.33e-08
  1000    8.33e-09     8.33e-06     8.33e-10
 10000    8.33e-11     8.33e-07     8.33e-12
```

Simpson's rule converges fastest for smooth functions.

## Glossary

| Term | Definition |
|------|------------|
| Antiderivative | A function $F$ such that $F'(x) = f(x)$ |
| Indefinite integral | The family of all antiderivatives of a function |
| Definite integral | The signed area under a curve between two points |
| Riemann sum | Approximation of an integral using rectangles |
| Fundamental theorem of calculus | Connects differentiation and integration |
| Integration by substitution | Reverse of the chain rule |
| Integration by parts | Reverse of the product rule |
| Partial fractions | Decomposition of rational functions for integration |
| Improper integral | Integral with infinite limits or unbounded integrand |
| Numerical integration | Approximating integrals numerically |
| Trapezoidal rule | Numerical integration using trapezoids |
| Simpson's rule | Numerical integration using quadratic approximations |
| Monte Carlo integration | Integration using random sampling |
| Quadrature | Another term for numerical integration |
| Adaptive quadrature | Numerical integration with adaptive step size |
| Arc length | Length of a curve, computed via integration |
| Laplace transform | Integral transform mapping time to frequency domain |

## Quick References

- [Scipy Integrate Documentation](https://docs.scipy.org/doc/scipy/reference/integrate.html) — numerical integration routines
- [SymPy Integration](https://docs.sympy.org/latest/tutorials/intro-tutorial/calculus.html) — symbolic integration
- [Paul's Online Math Notes — Integration Techniques](https://tutorial.math.lamar.edu/Classes/CalcII/IntegrationStrategy.aspx) — comprehensive reference
- [Monte Carlo Integration — Stanford CS](https://mathworld.wolfram.com/MonteCarloIntegration.html) — high-dimensional integration

## Next Steps

- [Multivariate Calculus](multivariate-calculus.md) — partial derivatives, multiple integrals, gradient, Hessian
- [Differential Equations](differential-equations.md) — equations involving derivatives and integrals
