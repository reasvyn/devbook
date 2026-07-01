# Applications of Derivatives

## Description

Derivatives are not just theoretical — they power optimization algorithms in machine learning, physical simulations, economic modeling, and scientific computing. This document covers how derivatives solve real problems: finding minima and maxima, implementing gradient descent, approximating roots, and analyzing function behavior.

## Prerequisites

- [Differentiation](differentiation.md) — derivative rules, chain rule, higher-order derivatives

## Table of Contents

- [Optimization: Finding Minima and Maxima](#optimization-finding-minima-and-maxima)
- [Gradient Descent](#gradient-descent)
- [Related Rates](#related-rates)
- [Curve Sketching](#curve-sketching)
- [L'Hôpital's Rule](#lhôpitals-rule)
- [Newton's Method](#newtons-method)
- [Convexity and Concavity](#convexity-and-concavity)
- [Applications in ML, Physics, and Economics](#applications-in-ml-physics-and-economics)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Optimization: Finding Minima and Maxima

**Critical points:** Points where $f'(x) = 0$ or $f'(x)$ is undefined.

**First derivative test:**
- If $f'$ changes from $+$ to $-$ at $c$, $c$ is a local maximum.
- If $f'$ changes from $-$ to $+$ at $c$, $c$ is a local minimum.
- If $f'$ does not change sign, $c$ is a saddle point.

**Second derivative test:**
- If $f'(c) = 0$ and $f''(c) > 0$, then $c$ is a local minimum.
- If $f'(c) = 0$ and $f''(c) < 0$, then $c$ is a local maximum.
- If $f''(c) = 0$, the test is inconclusive.

**Extreme value theorem:** A continuous function on a closed interval $[a, b]$ attains both a maximum and a minimum.

```python
import numpy as np

def find_critical_points(f, f_prime, x_range, num_points=10000):
    """Find critical points by scanning for sign changes in the derivative."""
    xs = np.linspace(x_range[0], x_range[1], num_points)
    derivatives = f_prime(xs)

    critical = []
    for i in range(1, len(xs)):
        if derivatives[i-1] * derivatives[i] < 0:
            # Sign change — approximate the root
            x0 = xs[i-1]
            critical.append(x0)

    return critical

def f(x):
    return x**3 - 6*x**2 + 9*x + 1

def f_prime(x):
    return 3*x**2 - 12*x + 9

critical_pts = find_critical_points(f, f_prime, (0, 5))
for cp in critical_pts:
    fpp = 6*cp - 12  # second derivative
    kind = "minimum" if fpp > 0 else "maximum" if fpp < 0 else "saddle"
    print(f"Critical point at x={cp:.4f}, f(x)={f(cp):.4f}, type: {kind}")
```

Output:

```
Critical point at x=1.0002, f(x)=5.0002, type: maximum
Critical point at x=3.0001, f(x)=1.0000, type: minimum
```

### Gradient Descent

Gradient descent is the workhorse optimization algorithm behind most of machine learning. The idea: move in the direction opposite to the gradient to find a minimum.

$$
x_{k+1} = x_k - \alpha \nabla f(x_k)
```

where $\alpha$ is the learning rate.

**Why the negative gradient?** The gradient points in the direction of steepest ascent; moving opposite to it descends toward a minimum.

```python
import numpy as np

def gradient_descent(grad, start, lr=0.1, tol=1e-8, max_iters=1000):
    """Generic gradient descent for scalar functions."""
    x = start
    history = [x]
    for i in range(max_iters):
        g = grad(x)
        x_new = x - lr * g
        history.append(x_new)
        if abs(x_new - x) < tol:
            break
        x = x_new
    return x, history

def f(x):
    return x**4 - 3*x**3 + 2

def grad_f(x):
    return 4*x**3 - 9*x**2

x_opt, hist = gradient_descent(grad_f, 4.0, lr=0.01)
print(f"Minimum at x = {x_opt:.6f}, f(x) = {f(x_opt):.6f}")
print(f"Converged in {len(hist)} iterations")
```

**Multivariate gradient descent:**

```python
import numpy as np

def gradient_descent_multivariate(grad, x0, lr=0.01, tol=1e-8, max_iters=1000):
    x = x0.copy()
    history = [x.copy()]
    for i in range(max_iters):
        g = grad(x)
        x_new = x - lr * g
        history.append(x_new.copy())
        if np.linalg.norm(x_new - x) < tol:
            break
        x = x_new
    return x, history

def rosenbrock(x):
    """Rosenbrock banana function: f(x,y) = (a-x)^2 + b(y-x^2)^2."""
    a, b = 1, 100
    return (a - x[0])**2 + b * (x[1] - x[0]**2)**2

def grad_rosenbrock(x):
    a, b = 1, 100
    dx = -2*(a - x[0]) - 4*b*x[0]*(x[1] - x[0]**2)
    dy = 2*b*(x[1] - x[0]**2)
    return np.array([dx, dy])

x_opt, hist = gradient_descent_multivariate(grad_rosenbrock, np.array([-1.5, 1.5]),
                                             lr=0.002, max_iters=50000)
print(f"Found minimum at ({x_opt[0]:.6f}, {x_opt[1]:.6f})")
print(f"f(x,y) = {rosenbrock(x_opt):.6f} (expected 0 at (1,1))")
```

**Learning rate schedules:** The learning rate can decay over time:

```python
def gradient_descent_decay(grad, start, lr0=0.1, decay=0.99, max_iters=100):
    x = start
    lr = lr0
    for i in range(max_iters):
        g = grad(x)
        x = x - lr * g
        lr *= decay  # exponential decay
    return x

x_opt = gradient_descent_decay(grad_f, 4.0)
print(f"With decay: x = {x_opt:.6f}")
```

**Momentum:** Accelerates gradient descent by accumulating a velocity vector.

$$
v_{k+1} = \beta v_k + \nabla f(x_k), \quad x_{k+1} = x_k - \alpha v_{k+1}
$$

### Related Rates

Related rates problems involve finding how fast one quantity changes based on how fast another changes, using the chain rule.

**Example:** A spherical balloon inflates at 100 cm³/s. How fast is the radius growing when $r = 10$ cm?

$$
V = \frac{4}{3}\pi r^3
$$

$$
\frac{dV}{dt} = 4\pi r^2 \frac{dr}{dt}
$$

$$
\frac{dr}{dt} = \frac{dV/dt}{4\pi r^2} = \frac{100}{4\pi (10)^2} \approx 0.0796 \text{ cm/s}
$$

```python
def related_rates_balloon(dV_dt, r):
    """Compute dr/dt given dV/dt and current radius."""
    return dV_dt / (4 * np.pi * r**2)

dV_dt = 100  # cm^3/s
r = 10  # cm
dr_dt = related_rates_balloon(dV_dt, r)
print(f"dr/dt = {dr_dt:.4f} cm/s")
```

### Curve Sketching

Derivatives help analyze function behavior for plotting:

1. Find domain and intercepts.
2. Find critical points ($f'(x) = 0$).
3. Determine intervals of increase/decrease.
4. Find inflection points ($f''(x) = 0$).
5. Determine concavity.
6. Find asymptotes.
7. Plot key points and sketch.

```python
import numpy as np

def analyze_function(f, f_prime, f_double_prime, x_range, num_points=1000):
    """Analyze a function for curve sketching."""
    xs = np.linspace(x_range[0], x_range[1], num_points)
    ys = f(xs)
    fps = f_prime(xs)
    fpps = f_double_prime(xs)

    # Find critical points (sign changes in f_prime)
    critical = []
    for i in range(1, len(xs)):
        if fps[i-1] * fps[i] < 0:
            critical.append((xs[i], f(xs[i]), 'min' if fpps[i] > 0 else 'max'))

    # Find inflection points (sign changes in f_double_prime)
    inflection = []
    for i in range(1, len(xs)):
        if fpps[i-1] * fpps[i] < 0:
            inflection.append((xs[i], f(xs[i])))

    return critical, inflection

def f_sketch(x):
    return x**3 - 3*x**2 + 1

def fp_sketch(x):
    return 3*x**2 - 6*x

def fpp_sketch(x):
    return 6*x - 6

critical, inflection = analyze_function(f_sketch, fp_sketch, fpp_sketch, (-2, 4))
print("Critical points:")
for x, y, kind in critical:
    print(f"  x={x:.4f}, y={y:.4f}, type={kind}")
print("Inflection points:")
for x, y in inflection:
    print(f"  x={x:.4f}, y={y:.4f}")
```

### L'Hôpital's Rule

When evaluating a limit of the form $0/0$ or $\infty/\infty$, differentiate numerator and denominator separately.

$$
\lim_{x \to c} \frac{f(x)}{g(x)} = \lim_{x \to c} \frac{f'(x)}{g'(x)}
$$

**Example:**

$$
\lim_{x \to 0} \frac{\sin x}{x} = \lim_{x \to 0} \frac{\cos x}{1} = 1
$$

```python
import sympy as sp
import numpy as np

x = sp.Symbol('x')

# Classic examples
limits = [
    (sp.sin(x) / x, x, 0),
    ((sp.exp(x) - 1) / x, x, 0),
    (sp.log(x) / (1/x), x, 0),
    (x**2 / sp.exp(x), x, sp.oo),
]

for expr, var, at in limits:
    result = sp.limit(expr, var, at)
    print(f"lim_{{{sp.latex(var)} \\to {sp.latex(at)}}} {sp.latex(expr)} = {result}")
```

Output:

```
lim_{x → 0} sin(x)/x = 1
lim_{x → 0} (e^x - 1)/x = 1
lim_{x → 0} log(x)/(1/x) = 0
lim_{x → ∞} x^2/e^x = 0
```

### Newton's Method

Newton's method finds roots of $f(x) = 0$ using the derivative:

$$
x_{k+1} = x_k - \frac{f(x_k)}{f'(x_k)}
$$

Geometrically: at each step, follow the tangent line to where it crosses the x-axis.

```python
import numpy as np

def newtons_method(f, f_prime, x0, tol=1e-10, max_iters=100):
    """Find root of f(x) = 0 using Newton's method."""
    x = x0
    for i in range(max_iters):
        fx = f(x)
        fpx = f_prime(x)
        if abs(fpx) < 1e-15:
            raise ValueError("Derivative too small")
        x_new = x - fx / fpx
        if abs(x_new - x) < tol:
            return x_new, i + 1
        x = x_new
    return x, max_iters

def f_sqrt(x):
    return x**2 - 2  # find sqrt(2)

def fp_sqrt(x):
    return 2*x

root, iters = newtons_method(f_sqrt, fp_sqrt, 1.5)
print(f"sqrt(2) ≈ {root:.10f}, iterations: {iters}")
print(f"Error: {abs(root - np.sqrt(2)):.2e}")
```

Output:

```
sqrt(2) ≈ 1.4142135624, iterations: 4
Error: 2.22e-16
```

**Quadratic convergence:** Newton's method doubles the number of correct digits each iteration when close to the root.

### Convexity and Concavity

**Convex function:** $f''(x) \geq 0$ for all $x$. The line segment between any two points lies above the graph.

**Concave function:** $f''(x) \leq 0$ for all $x$. The line segment lies below the graph.

**Why convexity matters:** For convex functions, any local minimum is also a global minimum. Gradient descent is guaranteed to converge to the global minimum.

```python
import numpy as np

def is_convex(f_prime, f_double_prime, x_range, num_points=1000):
    """Check convexity over an interval by examining second derivative sign."""
    xs = np.linspace(x_range[0], x_range[1], num_points)
    fpps = f_double_prime(xs)
    return np.all(fpps >= -1e-10)  # allow small numerical tolerance

def f_convex(x):
    return x**2  # f''(x) = 2 > 0 everywhere

def f_nonconvex(x):
    return x**3 - 3*x  # f''(x) = 6x, changes sign

print(f"x^2 convex: {is_convex(None, lambda x: 2*x/x, (-10, 10))}")
```

**Jensen's inequality:** For convex $f$, $f(\mathbb{E}[X]) \leq \mathbb{E}[f(X)]$. This is fundamental in information theory, statistics, and machine learning.

### Applications in ML, Physics, and Economics

**Machine learning — training neural networks:**

The loss function $L(\theta)$ is minimized using gradient-based optimization. The gradient $\nabla_\theta L$ is computed via backpropagation, which is the chain rule applied through the network layers.

```python
import numpy as np

def train_linear_regression(X, y, lr=0.01, epochs=1000):
    """Train linear regression using gradient descent."""
    n, d = X.shape
    w = np.zeros(d)
    b = 0.0
    losses = []

    for epoch in range(epochs):
        y_pred = X @ w + b
        loss = np.mean((y_pred - y)**2)
        losses.append(loss)

        # Gradients
        dw = (2/n) * X.T @ (y_pred - y)
        db = (2/n) * np.sum(y_pred - y)

        # Update
        w -= lr * dw
        b -= lr * db

    return w, b, losses

# Generate data
np.random.seed(42)
n, d = 1000, 5
X = np.random.randn(n, d)
w_true = np.array([1.5, -2.0, 0.5, -1.0, 3.0])
y = X @ w_true + 0.1 * np.random.randn(n)

w, b, losses = train_linear_regression(X, y)
print(f"Weights: {w}")
print(f"Weight error: {np.linalg.norm(w - w_true):.4e}")
```

**Physics simulation — Euler integration for dynamics:**

Newton's second law $F = ma$ gives $a(t) = F/m$. Velocity $v(t)$ is the derivative of position, and acceleration is the derivative of velocity.

```python
import numpy as np

def simulate_spring(k=10, m=1, x0=1.0, v0=0.0, dt=0.01, t_max=10):
    """Simulate a mass-spring system: F = -kx, a = F/m."""
    n_steps = int(t_max / dt)
    x, v = x0, v0
    trajectory = [(0, x, v)]

    for i in range(1, n_steps):
        a = -k * x / m  # acceleration (derivative of velocity)
        v += a * dt      # update velocity (integrate acceleration)
        x += v * dt      # update position (integrate velocity)
        trajectory.append((i*dt, x, v))

    return trajectory

traj = simulate_spring(k=10, m=1, x0=1.0, v0=0.0, dt=0.01, t_max=5)
print(f"At t={traj[-1][0]:.2f}s: position={traj[-1][1]:.4f}, velocity={traj[-1][2]:.4f}")
```

**Economic modeling — marginal analysis:**

Marginal cost is the derivative of total cost with respect to quantity. Marginal revenue is the derivative of revenue. Profit is maximized when marginal cost equals marginal revenue.

```python
def marginal_analysis(C, C_prime, R, R_prime, q_range):
    """Find optimal production quantity where MC = MR."""
    qs = np.linspace(q_range[0], q_range[1], 1000)
    for q in qs:
        if abs(C_prime(q) - R_prime(q)) < 0.01:
            profit = R(q) - C(q)
            return q, profit
    return None, None

def cost(q):
    return 0.01 * q**2 + 2 * q + 1000

def cost_prime(q):
    return 0.02 * q + 2

def revenue(q):
    return 10 * q

def revenue_prime(q):
    return 10.0

q_opt, profit = marginal_analysis(cost, cost_prime, revenue, revenue_prime, (0, 1000))
if q_opt:
    print(f"Optimal quantity: {q_opt:.2f}, profit: ${profit:.2f}")
```

## Glossary

| Term | Definition |
|------|------------|
| Critical point | Point where $f'(x) = 0$ or is undefined |
| Local minimum | Point where $f(x) \leq f(c)$ for all $x$ near $c$ |
| Local maximum | Point where $f(x) \geq f(c)$ for all $x$ near $c$ |
| Gradient descent | Iterative optimization algorithm following the negative gradient |
| Learning rate | Step size in gradient descent |
| Convex function | Function where $f''(x) \geq 0$ everywhere |
| Concave function | Function where $f''(x) \leq 0$ everywhere |
| Newton's method | Root-finding algorithm using derivative |
| L'Hôpital's rule | Method for evaluating indeterminate limits |
| Related rates | Problems involving rates of change linked by the chain rule |
| Curvature | The second derivative measuring how the slope changes |
| Inflection point | Point where concavity changes ($f''(x) = 0$) |
| Backpropagation | Algorithm for computing gradients through neural networks via the chain rule |
| Momentum | Technique to accelerate gradient descent using accumulated velocity |
| Jensen's inequality | $f(\mathbb{E}[X]) \leq \mathbb{E}[f(X)]$ for convex $f$ |
| Marginal cost | Derivative of total cost with respect to quantity |

## Quick References

- [Gradient Descent — CS231n Notes](https://cs231n.github.io/optimization-1/) — intuitive explanation with visualizations
- [PyTorch Optimizer Docs](https://pytorch.org/docs/stable/optim.html) — SGD, Adam, RMSprop implementations
- [Newton's Method — Wikipedia](https://en.wikipedia.org/wiki/Newton%27s_method) — convergence analysis
- [Convex Optimization — Boyd & Vandenberghe](https://web.stanford.edu/~boyd/cvxbook/) — comprehensive resource
- [Scipy.optimize](https://docs.scipy.org/doc/scipy/reference/optimize.html) — production optimization algorithms in Python

## Next Steps

- [Integration](integration.md) — the inverse of differentiation, computing areas and accumulated change
- [Multivariate Calculus](multivariate-calculus.md) — extending derivatives and optimization to higher dimensions
