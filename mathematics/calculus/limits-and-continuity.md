# Limits and Continuity

## Description

Limits describe the behavior of a function as its input approaches a value — the foundation for derivatives, integrals, and analyzing algorithm convergence. Continuity ensures that small input changes produce small output changes, a property every developer relies on when iterating toward a solution.

## Prerequisites

- [Mathematical Thinking](../intro/mathematical-thinking.md) — functions, notation, and the concept of approximation

## Table of Contents

- [Intuitive Definition of a Limit](#intuitive-definition-of-a-limit)
- [One-Sided Limits](#one-sided-limits)
- [Limit Laws](#limit-laws)
- [Continuity](#continuity)
- [Types of Discontinuity](#types-of-discontinuity)
- [The Squeeze Theorem](#the-squeeze-theorem)
- [Infinite Limits and Asymptotes](#infinite-limits-and-asymptotes)
- [The Epsilon-Delta Definition](#the-epsilon-delta-definition)
- [Limits in Computing](#limits-in-computing)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Intuitive Definition of a Limit

A limit answers the question: what value does $f(x)$ approach as $x$ gets arbitrarily close to $c$?

$$
\lim_{x \to c} f(x) = L
$$

This means we can make $f(x)$ as close to $L$ as we want by picking $x$ sufficiently close to $c$ (but not necessarily equal to $c$).

**Example:** Consider $f(x) = \frac{x^2 - 1}{x - 1}$.

This function is undefined at $x = 1$ because the denominator is zero. But as $x$ approaches 1 from either side, the expression simplifies:

$$
\frac{x^2 - 1}{x - 1} = \frac{(x - 1)(x + 1)}{x - 1} = x + 1, \quad x \neq 1
$$

So $\lim_{x \to 1} f(x) = 2$, even though $f(1)$ is undefined.

```python
import numpy as np

def f(x):
    return (x**2 - 1) / (x - 1)

# Values approaching 1 from both sides
xs = np.array([0.9, 0.99, 0.999, 0.9999, 1.0001, 1.001, 1.01, 1.1])
ys = f(xs)
for x, y in zip(xs, ys):
    print(f"f({x:.6f}) = {y:.6f}")
```

Output:

```
f(0.900000) = 1.900000
f(0.990000) = 1.990000
f(0.999000) = 1.999000
f(0.999900) = 1.999900
f(1.000100) = 2.000100
f(1.001000) = 2.001000
f(1.010000) = 2.010000
f(1.100000) = 2.100000
```

The values converge to 2.

### One-Sided Limits

A two-sided limit exists only when both one-sided limits exist and are equal.

**Left-hand limit:**

$$
\lim_{x \to c^-} f(x) = L
$$

Value approaches as $x$ approaches $c$ from values less than $c$.

**Right-hand limit:**

$$
\lim_{x \to c^+} f(x) = L
$$

Value approaches as $x$ approaches $c$ from values greater than $c$.

**Existence condition:**

$$
\lim_{x \to c} f(x) = L \iff \lim_{x \to c^-} f(x) = \lim_{x \to c^+} f(x) = L
$$

**Example:** The floor function $f(x) = \lfloor x \rfloor$ has different one-sided limits at integer values.

```python
import numpy as np

def floor_limit_demo():
    x_left = 1.9999
    x_right = 2.0001
    print(f"Left of 2: floor({x_left}) = {np.floor(x_left)}")
    print(f"Right of 2: floor({x_right}) = {np.floor(x_right)}")

floor_limit_demo()
```

Output:

```
Left of 2: floor(1.9999) = 1.0
Right of 2: floor(2.0001) = 2.0
```

The left and right limits differ, so $\lim_{x \to 2} \lfloor x \rfloor$ does not exist.

### Limit Laws

If $\lim_{x \to c} f(x) = L$ and $\lim_{x \to c} g(x) = M$, then:

| Law | Formula |
|-----|---------|
| Sum | $\lim (f + g) = L + M$ |
| Difference | $\lim (f - g) = L - M$ |
| Constant multiple | $\lim (k f) = kL$ |
| Product | $\lim (f \cdot g) = L \cdot M$ |
| Quotient | $\lim (f / g) = L / M$, if $M \neq 0$ |
| Power | $\lim f^n = L^n$ |
| Root | $\lim \sqrt[n]{f} = \sqrt[n]{L}$, if $L \geq 0$ for even roots |

These laws let us decompose complex limits into simpler parts.

**Example:**

$$
\lim_{x \to 2} (3x^2 + 2x - 1) = 3(2)^2 + 2(2) - 1 = 12 + 4 - 1 = 15
$$

```python
import sympy as sp

x = sp.Symbol('x')
expr = 3*x**2 + 2*x - 1
limit_value = sp.limit(expr, x, 2)
print(f"lim_{x->2} (3x^2 + 2x - 1) = {limit_value}")
```

### Continuity

A function $f$ is continuous at $x = c$ if all three conditions hold:

1. $f(c)$ is defined
2. $\lim_{x \to c} f(x)$ exists
3. $\lim_{x \to c} f(x) = f(c)$

**Continuity on an interval:** A function is continuous on an interval if it is continuous at every point in the interval.

**Why continuity matters in computing:**

- Floating-point stability near singularities
- Convergence guarantees for iterative algorithms
- Ensuring that small perturbations in input produce small changes in output

```python
def is_continuous_numerically(f, c, epsilon=1e-6, delta=1e-6):
    """Check continuity of f at c numerically."""
    try:
        fc = f(c)
    except (ZeroDivisionError, ValueError):
        return False

    left = f(c - delta)
    right = f(c + delta)

    return abs(left - fc) < epsilon and abs(right - fc) < epsilon

def f(x):
    return x**2

def g(x):
    return 1 / (x - 1)

print(f"f(x)=x^2 continuous at 3: {is_continuous_numerically(f, 3)}")
print(f"g(x)=1/(x-1) continuous at 1: {is_continuous_numerically(g, 1)}")
```

Output:

```
f(x)=x^2 continuous at 3: True
g(x)=1/(x-1) continuous at 1: False
```

### Types of Discontinuity

**Removable Discontinuity:** The limit exists, but $f(c)$ is either undefined or does not match the limit.

$$
f(x) = \frac{x^2 - 1}{x - 1}, \quad x = 1
$$

The hole can be "filled" by defining $f(1) = 2$.

**Jump Discontinuity:** Left and right limits exist but are not equal.

$$
f(x) = \lfloor x \rfloor, \quad x \in \mathbb{Z}
$$

**Infinite Discontinuity:** The function approaches $\pm \infty$ near $c$.

$$
f(x) = \frac{1}{x - 1}, \quad x = 1
$$

**Oscillatory Discontinuity:** The function oscillates wildly near $c$ without settling.

$$
f(x) = \sin\left(\frac{1}{x}\right), \quad x = 0
$$

```python
import numpy as np
import matplotlib.pyplot as plt

def classify_discontinuity(f, c, h=1e-6):
    left_limit = f(c - h)
    right_limit = f(c + h)

    left_finite = np.isfinite(left_limit)
    right_finite = np.isfinite(right_limit)

    if not left_finite or not right_finite:
        return "infinite"

    if abs(left_limit - right_limit) > 1e-3:
        return "jump"

    try:
        fc = f(c)
        if abs(left_limit - fc) < 1e-6:
            return "continuous"
        return "removable"
    except (ZeroDivisionError, ValueError):
        return "removable"

def f_removable(x):
    return (x**2 - 1) / (x - 1) if x != 1 else np.nan

print(classify_discontinuity(f_removable, 1))  # removable
```

### The Squeeze Theorem

If $g(x) \leq f(x) \leq h(x)$ for all $x$ near $c$, and $\lim_{x \to c} g(x) = \lim_{x \to c} h(x) = L$, then $\lim_{x \to c} f(x) = L$.

**Classic application:**

$$
\lim_{x \to 0} x^2 \sin\left(\frac{1}{x}\right) = 0
$$

Since $-1 \leq \sin(1/x) \leq 1$, we have $-x^2 \leq x^2 \sin(1/x) \leq x^2$. Both $-x^2$ and $x^2$ approach 0 as $x \to 0$, so the limit is 0 by the squeeze theorem.

```python
import numpy as np

def squeeze_plot():
    xs = np.linspace(-0.1, 0.1, 1000)
    f = xs**2 * np.sin(1 / xs)
    lower = -xs**2
    upper = xs**2

    print("x        f(x)          lower       upper")
    print("-" * 45)
    for i in range(0, len(xs), 100):
        print(f"{xs[i]:+.4f}  {f[i]:+.8f}  {lower[i]:+.8f}  {upper[i]:+.8f}")

squeeze_plot()
```

Output:

```
x        f(x)          lower       upper
---------------------------------------------
-0.1000  -0.005065472  -0.010000  +0.010000
-0.0790  +0.005129935  -0.006241  +0.006241
-0.0580  +0.001500856  -0.003364  +0.003364
-0.0370  -0.000702627  -0.001369  +0.001369
-0.0160  -0.000144194  -0.000256  +0.000256
+0.0050  -0.000017777  -0.000025  +0.000025
+0.0260  +0.000611893  -0.000676  +0.000676
+0.0470  -0.001230330  -0.002209  +0.002209
+0.0680  -0.001835670  -0.004624  +0.004624
+0.0890  +0.003973098  -0.007921  +0.007921
```

The function is pinched between the two bounds.

### Infinite Limits and Asymptotes

**Vertical asymptotes** occur when $f(x) \to \pm\infty$ as $x \to c$.

$$
\lim_{x \to 0} \frac{1}{x^2} = +\infty, \quad \lim_{x \to 0^+} \frac{1}{x} = +\infty, \quad \lim_{x \to 0^-} \frac{1}{x} = -\infty
$$

**Horizontal asymptotes** describe behavior as $x \to \pm\infty$.

$$
\lim_{x \to \infty} \frac{1}{x} = 0, \quad \lim_{x \to \infty} \frac{3x^2 + 2x}{x^2 - 1} = 3
$$

**Finding asymptotes algorithmically:**

```python
import numpy as np

def find_horizontal_asymptote(f, x_range=1e6):
    """Estimate horizontal asymptote as x -> +infinity."""
    x_large = x_range
    return f(x_large)

def find_vertical_asymptotes(f, x_range, num_points=10000):
    """Detect potential vertical asymptotes by finding large function values."""
    xs = np.linspace(x_range[0], x_range[1], num_points)
    ys = np.array([f(x) for x in xs])

    asymptotes = []
    threshold = 1e6

    for i in range(1, len(xs)):
        if (abs(ys[i]) > threshold and abs(ys[i-1]) < threshold) or \
           (abs(ys[i]) < threshold and abs(ys[i-1]) > threshold):
            asymptotes.append((xs[i-1], xs[i]))

    return asymptotes

def g(x):
    return 1 / (x - 3) if x != 3 else np.nan

print(f"Horizontal asymptote: {find_horizontal_asymptote(g)}")
```

### The Epsilon-Delta Definition

Formally, $\lim_{x \to c} f(x) = L$ means:

For every $\epsilon > 0$, there exists $\delta > 0$ such that if $0 < |x - c| < \delta$, then $|f(x) - L| < \epsilon$.

**Intuition:** No matter how tight a tolerance $\epsilon$ we demand for the output, we can find an input tolerance $\delta$ that guarantees it.

```python
def epsilon_delta_check(f, c, L, epsilon, delta):
    """Check if f stays within epsilon of L when x is within delta of c."""
    xs = np.linspace(c - delta, c + delta, 10001)
    xs = xs[xs != c]  # exclude c itself
    ys = np.array([f(x) for x in xs])
    violations = np.where(np.abs(ys - L) >= epsilon)[0]
    if len(violations) == 0:
        return True
    else:
        first_violation = xs[violations[0]]
        return False, first_violation, ys[violations[0]]

def f_linear(x):
    return 2 * x + 1

# For limit L=5 at c=2, epsilon=0.1 => find delta=0.05
result = epsilon_delta_check(f_linear, 2, 5, 0.1, 0.05)
print(f"Delta=0.05 works for epsilon=0.1: {result}")
```

**Why developers care:** Epsilon-delta is the mathematical version of tolerance-based programming. When you specify a convergence tolerance for gradient descent, you are implicitly using the same idea — find parameters that bring the loss within $\epsilon$ of the minimum.

### Limits in Computing

**Algorithm convergence:** An iterative algorithm converges if the sequence of outputs approaches a limit.

$$
\lim_{k \to \infty} x_k = L
$$

**Stability:** A numerical algorithm is stable if small perturbations in input produce small changes in output — essentially a continuity requirement.

**Rate of convergence:** The speed at which a sequence approaches its limit.

- Linear: $|e_{k+1}| \leq C|e_k|$, $C < 1$
- Quadratic: $|e_{k+1}| \leq C|e_k|^2$

```python
import numpy as np

def bisection_method(f, a, b, tol=1e-10, max_iter=100):
    """Find root of f in [a,b] using bisection."""
    fa, fb = f(a), f(b)
    if fa * fb > 0:
        raise ValueError("f(a) and f(b) must have opposite signs")

    for k in range(max_iter):
        c = (a + b) / 2
        fc = f(c)
        if abs(fc) < tol or (b - a) / 2 < tol:
            return c, k
        if fa * fc < 0:
            b, fb = c, fc
        else:
            a, fa = c, fc
    return (a + b) / 2, max_iter

def f_root(x):
    return x**3 - x - 2

root, iters = bisection_method(f_root, 1, 2)
print(f"Root: {root:.10f}, iterations: {iters}")
```

**Limits in machine learning:**

- Gradient descent converges when the learning rate is chosen appropriately — the limit of the parameter sequence exists.
- Batch normalization stabilizes training by ensuring layer outputs have consistent mean and variance, improving continuity.
- Learning rate schedules are designed to ensure convergence in the limit.

**Numerical stability example:** The function $\log(1 + x)$ loses precision for small $x$ when computed directly. The stable approach uses `np.log1p(x)`, which computes the limit more accurately.

```python
x = 1e-15
direct = np.log(1 + x)
stable = np.log1p(x)
print(f"Direct: {direct}, Stable: {stable}")
```

Output:

```
Direct: 1.1102230246251565e-15, Stable: 1e-15
```

The direct computation loses half the precision due to floating-point cancellation — a continuity failure in floating-point arithmetic.

## Study Cases

### Case 1: Convergence of Gradient Descent

Gradient descent for a convex function $f$ produces a sequence $x_{k+1} = x_k - \alpha \nabla f(x_k)$. Under the right conditions, $\lim_{k \to \infty} x_k = x^*$, the global minimum.

```python
import numpy as np

def gradient_descent(grad, x0, alpha=0.01, tol=1e-8, max_iter=1000):
    x = x0
    history = [x]
    for k in range(max_iter):
        g = grad(x)
        x_new = x - alpha * g
        history.append(x_new)
        if abs(x_new - x) < tol:
            break
        x = x_new
    return x, len(history)

def grad_quadratic(x):
    return 2 * (x - 3)

x_opt, iterations = gradient_descent(grad_quadratic, 0.0)
print(f"Found minimum at x={x_opt:.6f} (expected x=3), took {iterations} iterations")
```

### Case 2: Floating-Point Cancellation

The quadratic formula $x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$ suffers from catastrophic cancellation when $b$ is large relative to $a$ and $c$.

```python
import numpy as np

def unstable_quadratic(a, b, c):
    disc = np.sqrt(b**2 - 4*a*c)
    x1 = (-b + disc) / (2*a)
    x2 = (-b - disc) / (2*a)
    return x1, x2

def stable_quadratic(a, b, c):
    disc = np.sqrt(b**2 - 4*a*c)
    if b >= 0:
        x1 = (-b - disc) / (2*a)
        x2 = (2*c) / (-b - disc)
    else:
        x1 = (2*c) / (-b + disc)
        x2 = (-b + disc) / (2*a)
    return x1, x2

# Test with problematic values
a, b, c = 1, 1000000, 1
u1, u2 = unstable_quadratic(a, b, c)
s1, s2 = stable_quadratic(a, b, c)
print(f"Unstable: {u1:.10f}, {u2:.10f}")
print(f"Stable:   {s1:.10f}, {s2:.10f}")
# The small root is more accurate with the stable formula
```

### Case 3: Algorithmic Stability as Continuity

A sorting algorithm should be continuous: small changes to the input (adding a small perturbation to values) should not completely rearrange the order unless crossing a comparison threshold.

```python
import numpy as np

def stability_test(algorithm, arr, noise_level=1e-10):
    """Test if sort is stable under tiny perturbations."""
    arr_noisy = arr + np.random.normal(0, noise_level, len(arr))
    sorted_clean = algorithm(arr.copy())
    sorted_noisy = algorithm(arr_noisy)
    return np.sum(sorted_clean != sorted_noisy) / len(arr)

arr = np.array([1.0, 2.0, 3.0, 4.0, 5.0])
# A stable sort should be unaffected by tiny noise
# An unstable sort might arbitrarily reorder equal elements
```

## Examples

### Example 1: Evaluating Limits with SymPy

```python
import sympy as sp

x = sp.Symbol('x')

# Basic limits
limits = [
    (sp.sin(x) / x, x, 0),
    ((sp.exp(x) - 1) / x, x, 0),
    ((x**2 - 4) / (x - 2), x, 2),
    (sp.log(1 + x) / x, x, 0),
    ((1 + 1/x)**x, x, sp.oo),
]

for expr, var, at in limits:
    result = sp.limit(expr, var, at)
    print(f"lim_{{{var}\\to {at}}} {sp.latex(expr)} = {result}")
```

### Example 2: Graphical Exploration of Limits

```python
import numpy as np

def explore_limit(f, c, directions=10):
    """Explore function values as x approaches c from both sides."""
    steps = [10**(-i) for i in range(1, directions + 1)]
    print(f"{'h':>12} {'f(c-h)':>16} {'f(c+h)':>16} {'diff':>16}")
    print("-" * 62)
    for h in steps:
        left = f(c - h) if (c - h) != c else None
        right = f(c + h) if (c + h) != c else None
        if left is not None and right is not None:
            diff = abs(left - right)
            print(f"{h:12.1e} {left:16.10f} {right:16.10f} {diff:16.10f}")

explore_limit(lambda x: np.sin(x)/x, 0)
```

### Example 3: Limits at Infinity in Algorithm Analysis

```python
import numpy as np

def growth_comparison(n):
    """Compare growth rates of common functions."""
    return {
        'constant': 1,
        'log': np.log2(n),
        'linear': n,
        'n_log_n': n * np.log2(n),
        'quadratic': n**2,
        'cubic': n**3,
        'exponential': 2**n if n < 50 else float('inf'),
    }

n = 100
rates = growth_comparison(n)
for name, val in rates.items():
    print(f"{name:>12}: {val:.2e}")

# As n -> infinity, exponential dominates all polynomials,
# and quadratic dominates linear.
# Limits help us reason about asymptotic complexity.
```

### Example 4: Proving a Limit with Epsilon-Delta

Find $\delta$ for $\lim_{x \to 3} (2x + 1) = 7$:

Given $\epsilon > 0$, we need $|(2x + 1) - 7| < \epsilon$ when $0 < |x - 3| < \delta$.

$|2x + 1 - 7| = |2x - 6| = 2|x - 3| < \epsilon$

So choose $\delta = \epsilon / 2$.

```python
def find_delta_linear(m, epsilon):
    """For limit of mx + b at x=c => L=mc+b, find delta for given epsilon."""
    return epsilon / abs(m)

# For f(x) = 2x + 1, epsilon = 0.01
delta = find_delta_linear(2, 0.01)
print(f"delta = {delta}")
```

### Example 5: Continuity of Neural Networks

```python
import numpy as np

def relu(x):
    return np.maximum(0, x)

def neural_network(x, weights, biases):
    """Simple 2-layer network as a composition of continuous functions."""
    h = relu(weights[0] * x + biases[0])
    return weights[1] * h + biases[1]

# Since ReLU, linear layers, and addition are all continuous,
# the entire network is a continuous function of its input.
# Small perturbations in input produce bounded changes in output.

w1, b1 = 2.0, -1.0
w2, b2 = 1.0, 0.5

x = 1.0
eps = 1e-6
output = neural_network(x, [w1, w2], [b1, b2])
output_perturbed = neural_network(x + eps, [w1, w2], [b1, b2])
print(f"Change in output for epsilon={eps}: {abs(output_perturbed - output):.2e}")
```

## Glossary

| Term | Definition |
|------|------------|
| Limit | The value a function approaches as the input approaches a given value |
| One-sided limit | The value a function approaches as the input approaches from only the left or right |
| Continuity | A property where small input changes produce small output changes |
| Removable discontinuity | A discontinuity that can be fixed by redefining the function at a point |
| Jump discontinuity | A discontinuity where left and right limits exist but differ |
| Infinite discontinuity | A discontinuity where the function approaches infinity |
| Squeeze theorem | A theorem that finds a limit by bounding a function between two others |
| Epsilon-delta | The formal definition of a limit using arbitrary tolerances |
| Asymptote | A line that a function approaches arbitrarily closely |
| Convergence | The property of a sequence approaching a finite limit |
| Cancellation | Loss of precision when subtracting nearly equal numbers |

## Quick References

- [3Blue1Brown — The Essence of Calculus](https://www.youtube.com/playlist?list=PLZHQObOWTQDMsr9K-rj53DwVRMYO3t5Yr) — visual intuition for limits
- [Paul's Online Math Notes — Limits](https://tutorial.math.lamar.edu/Classes/CalcI/LimitsIntro.aspx) — thorough reference
- [SymPy Documentation — Calculus](https://docs.sympy.org/latest/tutorials/intro-tutorial/calculus.html) — symbolic limits in Python
- [NumPy Floating Point Errors](https://numpy.org/doc/stable/reference/generated/numpy.errstate.html) — handling floating-point limits in code

## Next Steps

- [Differentiation](differentiation.md) — derivatives build directly on limits
- [Sequences & Series](sequences-and-series.md) — sequences are limits applied to discrete steps
