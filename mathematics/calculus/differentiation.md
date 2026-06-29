# Differentiation

## Description

The derivative measures how a function changes as its input changes — the instantaneous rate of change. Every optimization algorithm, physics simulation, and neural network training loop depends on differentiation. Understanding derivatives lets you compute gradients, optimize loss functions, and model dynamic systems.

## Prerequisites

- [Limits & Continuity](limits-and-continuity.md) — the derivative is defined as a limit of difference quotients

## Table of Contents

- [The Derivative as Slope and Rate of Change](#the-derivative-as-slope-and-rate-of-change)
- [Basic Derivative Rules](#basic-derivative-rules)
- [Derivatives of Common Functions](#derivatives-of-common-functions)
- [The Product Rule](#the-product-rule)
- [The Quotient Rule](#the-quotient-rule)
- [The Chain Rule](#the-chain-rule)
- [Higher-Order Derivatives](#higher-order-derivatives)
- [Implicit Differentiation](#implicit-differentiation)
- [The Derivative in Code](#the-derivative-in-code)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Derivative as Slope and Rate of Change

The derivative of $f$ at $x$ is defined as the limit of the difference quotient:

$$
f'(x) = \lim_{h \to 0} \frac{f(x + h) - f(x)}{h}
$$

**Interpretations:**

- **Slope:** The slope of the tangent line to the graph of $f$ at point $(x, f(x))$.
- **Rate of change:** How fast $f(x)$ changes per unit change in $x$.
- **Sensitivity:** How much the output changes for a small change in input.

```python
import numpy as np
import matplotlib.pyplot as plt

def derivative_numerical(f, x, h=1e-7):
    """Approximate the derivative using the forward difference."""
    return (f(x + h) - f(x)) / h

def f(x):
    return x**2

x = 2.0
print(f"f'({x}) ≈ {derivative_numerical(f, x):.6f} (exact: {2*x})")
```

### Basic Derivative Rules

**Constant rule:**

$$
\frac{d}{dx}[c] = 0
$$

**Power rule:**

$$
\frac{d}{dx}[x^n] = n x^{n-1}
$$

**Constant multiple rule:**

$$
\frac{d}{dx}[c f(x)] = c f'(x)
$$

**Sum rule:**

$$
\frac{d}{dx}[f(x) + g(x)] = f'(x) + g'(x)
$$

```python
import sympy as sp

x = sp.Symbol('x')

# Power rule examples
expressions = [x**5, x**(-2), x**(sp.Rational(1,2)), 3*x**4 + 2*x**3 - x + 7]
for expr in expressions:
    deriv = sp.diff(expr, x)
    print(f"d/dx({sp.latex(expr)}) = {sp.latex(deriv)}")
```

### Derivatives of Common Functions

| Function | Derivative |
|----------|-----------|
| $c$ (constant) | $0$ |
| $x^n$ | $n x^{n-1}$ |
| $e^x$ | $e^x$ |
| $\ln x$ | $1/x$ |
| $\sin x$ | $\cos x$ |
| $\cos x$ | $-\sin x$ |
| $\tan x$ | $\sec^2 x$ |
| $\arcsin x$ | $1/\sqrt{1-x^2}$ |
| $\arctan x$ | $1/(1+x^2)$ |

```python
import sympy as sp
import numpy as np

x = sp.Symbol('x')
functions = {
    'exp': sp.exp(x),
    'log': sp.log(x),
    'sin': sp.sin(x),
    'cos': sp.cos(x),
    'tan': sp.tan(x),
}

for name, func in functions.items():
    deriv = sp.diff(func, x)
    print(f"d/dx({name}(x)) = {sp.latex(deriv)}")
```

### The Product Rule

$$
\frac{d}{dx}[f(x)g(x)] = f'(x)g(x) + f(x)g'(x)
$$

**Intuition:** If you have two quantities that both change, the total change is the sum of each changing while the other stays fixed.

**Example:**

$$
\frac{d}{dx}[x^2 \sin x] = 2x \sin x + x^2 \cos x
$$

```python
def product_rule_numerical(f, g, x, h=1e-7):
    """Verify product rule numerically."""
    fg = lambda x: f(x) * g(x)
    lhs = derivative_numerical(fg, x, h)

    fp = derivative_numerical(f, x, h)
    gp = derivative_numerical(g, x, h)
    rhs = fp * g(x) + f(x) * gp

    return lhs, rhs

f = lambda x: x**2
g = lambda x: np.sin(x)
l, r = product_rule_numerical(f, g, 1.5)
print(f"LHS: {l:.8f}, RHS: {r:.8f}, diff: {abs(l-r):.2e}")
```

### The Quotient Rule

$$
\frac{d}{dx}\left[\frac{f(x)}{g(x)}\right] = \frac{f'(x)g(x) - f(x)g'(x)}{[g(x)]^2}
$$

**Example:**

$$
\frac{d}{dx}\left[\frac{x}{\ln x}\right] = \frac{1 \cdot \ln x - x \cdot (1/x)}{(\ln x)^2} = \frac{\ln x - 1}{(\ln x)^2}
$$

```python
import sympy as sp

x = sp.Symbol('x')
f = x
g = sp.log(x)
quotient = f / g
deriv = sp.diff(quotient, x)
sp.simplify(deriv)
```

### The Chain Rule

The chain rule composes derivatives — it is the most important rule for machine learning because neural networks are compositions of functions.

$$
\frac{d}{dx}[f(g(x))] = f'(g(x)) \cdot g'(x)
$$

**In Leibniz notation:**

$$
\frac{dy}{dx} = \frac{dy}{du} \cdot \frac{du}{dx}
$$

**Example:**

$$
\frac{d}{dx}[\sin(x^2)] = \cos(x^2) \cdot 2x
$$

```python
import sympy as sp

x = sp.Symbol('x')
composed = sp.sin(x**2)
deriv = sp.diff(composed, x)
print(f"d/dx sin(x^2) = {sp.latex(deriv)}")

# Deeper composition
deeper = sp.exp(sp.sin(x**2))
deriv_deep = sp.diff(deeper, x)
print(f"d/dx exp(sin(x^2)) = {sp.latex(deriv_deep)}")
```

**The chain rule in backpropagation:** A neural network computes $f(x) = f_L(f_{L-1}(\dots f_1(x)\dots))$. The gradient with respect to each layer's parameters is computed by multiplying Jacobians via the chain rule — this is backpropagation.

```python
import numpy as np

def forward_and_gradient(x, w, b):
    """Simple neural network: sigmoid(w*x + b).
    Compute both output and gradient dy/dw using chain rule."""
    z = w * x + b
    y = 1 / (1 + np.exp(-z))  # sigmoid
    # dy/dw = dy/dz * dz/dw = sigmoid'(z) * x
    sigmoid_prime = y * (1 - y)  # derivative of sigmoid
    dy_dw = sigmoid_prime * x
    return y, dy_dw

x, w, b = 2.0, 0.5, -1.0
y, grad = forward_and_gradient(x, w, b)
print(f"Output: {y:.4f}, Gradient dy/dw: {grad:.4f}")
```

### Higher-Order Derivatives

The second derivative measures the rate of change of the first derivative — it tells us about curvature and acceleration.

$$
f''(x) = \frac{d}{dx}[f'(x)]
$$

**Notation:**

- $f''(x)$, $f'''(x)$, $f^{(4)}(x)$, etc.
- $\frac{d^2y}{dx^2}$, $\frac{d^3y}{dx^3}$

**Physical interpretation:** If $s(t)$ is position, $s'(t)$ is velocity, and $s''(t)$ is acceleration.

**Convexity:** A function is convex if $f''(x) \geq 0$ for all $x$, concave if $f''(x) \leq 0$.

```python
import sympy as sp

x = sp.Symbol('x')
f = x**4 - 3*x**3 + 2*x**2 - x + 1

first = sp.diff(f, x)
second = sp.diff(f, x, 2)
third = sp.diff(f, x, 3)

print(f"f(x)    = {sp.latex(f)}")
print(f"f'(x)   = {sp.latex(first)}")
print(f"f''(x)  = {sp.latex(second)}")
print(f"f'''(x) = {sp.latex(third)}")
```

```python
import numpy as np

def hessian_diagonal_numerical(f, x, h=1e-5):
    """Approximate the second derivative (diagonal of Hessian) for scalar f."""
    return (f(x + h) - 2*f(x) + f(x - h)) / (h**2)

f = lambda x: x**3 - 2*x**2 + x
for x_val in [-2, -1, 0, 1, 2]:
    print(f"f''({x_val:2d}) = {hessian_diagonal_numerical(f, x_val):.4f}")
```

### Implicit Differentiation

When a function is defined implicitly (e.g., $x^2 + y^2 = 1$), differentiate both sides with respect to $x$ and solve for $dy/dx$.

**Example:** Find $dy/dx$ for $x^2 + y^2 = 1$.

$$
\frac{d}{dx}[x^2] + \frac{d}{dx}[y^2] = 0
$$

$$
2x + 2y \cdot \frac{dy}{dx} = 0
$$

$$
\frac{dy}{dx} = -\frac{x}{y}
$$

```python
import sympy as sp

x, y = sp.symbols('x y')
# Implicit equation: x**2 + y**2 = 1
# Differentiate and solve for dy/dx
expr = x**2 + y**2 - 1
dy_dx = sp.idiff(expr, y, x)
print(f"dy/dx for x^2 + y^2 = 1: {sp.latex(dy_dx)}")
```

### The Derivative in Code

**Numerical differentiation:** Finite difference approximations.

Forward difference: $f'(x) \approx \frac{f(x+h) - f(x)}{h}$

Central difference: $f'(x) \approx \frac{f(x+h) - f(x-h)}{2h}$

```python
import numpy as np

def forward_diff(f, x, h=1e-7):
    return (f(x + h) - f(x)) / h

def central_diff(f, x, h=1e-7):
    return (f(x + h) - f(x - h)) / (2 * h)

def complex_step(f, x, h=1e-20):
    """Complex-step derivative — extremely accurate for analytic functions."""
    return np.imag(f(x + 1j * h)) / h

def f(x):
    return np.sin(x**2) * np.exp(-0.1 * x)

x = 1.5
print(f"Forward diff:   {forward_diff(f, x):.12f}")
print(f"Central diff:   {central_diff(f, x):.12f}")
print(f"Complex step:   {complex_step(f, x):.12f}")
```

**Automatic differentiation (autodiff):** The method used by PyTorch, TensorFlow, and JAX. It applies the chain rule to elementary operations, building a computation graph and propagating derivatives.

```python
import numpy as np

class Value:
    """Minimal autodiff implementation for scalar operations."""
    def __init__(self, data, children=(), op=''):
        self.data = data
        self.grad = 0.0
        self._backward = lambda: None
        self._prev = set(children)
        self._op = op

    def __add__(self, other):
        other = other if isinstance(other, Value) else Value(other)
        out = Value(self.data + other.data, (self, other), '+')
        def _backward():
            self.grad += out.grad
            other.grad += out.grad
        out._backward = _backward
        return out

    def __mul__(self, other):
        other = other if isinstance(other, Value) else Value(other)
        out = Value(self.data * other.data, (self, other), '*')
        def _backward():
            self.grad += other.data * out.grad
            other.grad += self.data * out.grad
        out._backward = _backward
        return out

    def __pow__(self, other):
        assert isinstance(other, (int, float))
        out = Value(self.data ** other, (self,), f'**{other}')
        def _backward():
            self.grad += other * (self.data ** (other - 1)) * out.grad
        out._backward = _backward
        return out

    def backward(self):
        topo = []
        visited = set()
        def build_topo(v):
            if v not in visited:
                visited.add(v)
                for child in v._prev:
                    build_topo(child)
                topo.append(v)
        build_topo(self)
        self.grad = 1.0
        for v in reversed(topo):
            v._backward()

    def __repr__(self):
        return f"Value(data={self.data:.4f}, grad={self.grad:.4f})"

# Example: f(x) = 3*x**2 + 2*x + 1, compute f'(2)
x = Value(2.0)
f = 3 * x**2 + 2 * x + 1
f.backward()
print(f"f(2) = {f.data}")
print(f"f'(2) = {x.grad} (exact: 3*2*2 + 2 = 14)")
```

**Symbolic differentiation:** Using SymPy to derive exact expressions.

```python
import sympy as sp

x = sp.Symbol('x')
f = 3 * x**2 + 2 * x + 1
deriv = sp.diff(f, x)
print(f"f(x) = {f}")
print(f"f'(x) = {deriv}")
print(f"f'(2) = {deriv.subs(x, 2)}")
```

## Study Cases

### Case 1: Gradient Descent on a Quadratic

Minimize $f(x) = x^2 - 4x + 5$ using its derivative $f'(x) = 2x - 4$.

```python
import numpy as np

def quadratic(x):
    return x**2 - 4*x + 5

def grad_quadratic(x):
    return 2*x - 4

def gradient_descent(grad, start, lr=0.1, steps=20):
    x = start
    trajectory = [x]
    for _ in range(steps):
        x = x - lr * grad(x)
        trajectory.append(x)
    return x, trajectory

x_opt, traj = gradient_descent(grad_quadratic, 0.0, lr=0.1, steps=20)
print(f"Optimal x: {x_opt:.6f} (expected: 2.0)")
print(f"Function value: {quadratic(x_opt):.6f}")
```

### Case 2: Physics Engine — Velocity and Acceleration

In a game physics engine, position $s(t)$, velocity $v(t) = s'(t)$, and acceleration $a(t) = v'(t) = s''(t)$.

```python
import numpy as np

def simulate_projectile(v0, theta, dt=0.01, g=9.81):
    """Simulate projectile motion using derivatives."""
    vx = v0 * np.cos(theta)
    vy = v0 * np.sin(theta)
    x, y = 0.0, 0.0

    trajectory = [(x, y)]
    while y >= 0:
        # s'(t) = v(t), v'(t) = a(t)
        x += vx * dt
        y += vy * dt
        vy -= g * dt  # acceleration due to gravity
        trajectory.append((x, y))
    return trajectory

traj = simulate_projectile(50, np.pi/4)
print(f"Final position: ({traj[-1][0]:.2f}, {traj[-1][1]:.2f})")
print(f"Max height: {max(y for _, y in traj):.2f}")
```

### Case 3: Sensitivity Analysis in Finance

Delta of an option is the derivative of the option price with respect to the underlying asset price — how much the option price changes per dollar change in the stock.

```python
import numpy as np
from scipy.stats import norm

def black_scholes_call(S, K, T, r, sigma):
    """Black-Scholes call option price."""
    d1 = (np.log(S/K) + (r + 0.5*sigma**2)*T) / (sigma * np.sqrt(T))
    d2 = d1 - sigma * np.sqrt(T)
    return S * norm.cdf(d1) - K * np.exp(-r*T) * norm.cdf(d2)

def delta_numerical(S, K, T, r, sigma, h=1e-6):
    """Compute delta (dPrice/dS) numerically."""
    return (black_scholes_call(S + h, K, T, r, sigma) -
            black_scholes_call(S - h, K, T, r, sigma)) / (2*h)

S, K, T, r, sigma = 100, 100, 1, 0.05, 0.2
print(f"Option price: {black_scholes_call(S, K, T, r, sigma):.4f}")
print(f"Delta (sensitivity): {delta_numerical(S, K, T, r, sigma):.4f}")
```

## Examples

### Example 1: Computing Derivatives Using Multiple Methods

```python
import numpy as np
import sympy as sp

x_val = 0.5
h = 1e-7

def f(x):
    return np.sin(3*x) * np.exp(-x**2)

# Numerical
fd = (f(x_val + h) - f(x_val)) / h
cd = (f(x_val + h) - f(x_val - h)) / (2*h)

# Symbolic
x_sym = sp.Symbol('x')
f_sym = sp.sin(3*x_sym) * sp.exp(-x_sym**2)
deriv_sym = sp.diff(f_sym, x_sym)
exact = float(deriv_sym.subs(x_sym, x_val))

print(f"Forward diff:    {fd:.12f}")
print(f"Central diff:    {cd:.12f}")
print(f"Exact (sympy):   {exact:.12f}")
print(f"Central error:   {abs(cd - exact):.2e}")
```

### Example 2: Second Derivative and Curvature

```python
import numpy as np

def f(x):
    return x**4 - 8*x**2 + 16

def first_derivative(f, x, h=1e-6):
    return (f(x + h) - f(x - h)) / (2*h)

def second_derivative(f, x, h=1e-6):
    return (f(x + h) - 2*f(x) + f(x - h)) / (h**2)

xs = np.linspace(-3, 3, 7)
for x in xs:
    fp = first_derivative(f, x)
    fpp = second_derivative(f, x)
    print(f"x={x:+.1f}  f'(x)={fp:+.4f}  f''(x)={fpp:+.4f}  convex={fpp > 0}")
```

### Example 3: Chain Rule Visualization

```python
import numpy as np

def chain_rule_debug(x):
    """Compute f(x) = exp(sin(x^2)) and its derivative step by step."""
    # Forward pass
    a = x**2
    b = np.sin(a)
    c = np.exp(b)

    # Backward pass (chain rule)
    dc_db = c  # derivative of exp
    db_da = np.cos(a)  # derivative of sin
    da_dx = 2 * x  # derivative of x^2

    dc_dx = dc_db * db_da * da_dx

    print(f"x={x}")
    print(f"  a = x^2 = {a:.4f}")
    print(f"  b = sin(a) = {b:.4f}")
    print(f"  c = exp(b) = {c:.4f}")
    print(f"  dc/dx = dc/db * db/da * da/dx = {dc_dx:.4f}")

    return c, dc_dx

chain_rule_debug(1.0)
```

### Example 4: Derivative of Loss Function for Linear Regression

```python
import numpy as np

def mse_loss(w, X, y):
    """Mean squared error for linear regression y = Xw."""
    n = len(y)
    return np.sum((X @ w - y)**2) / n

def grad_mse(w, X, y):
    """Gradient of MSE: (2/n) * X^T (Xw - y)."""
    n = len(y)
    return (2/n) * X.T @ (X @ w - y)

# Generate synthetic data
np.random.seed(42)
n, d = 100, 3
X = np.random.randn(n, d)
w_true = np.array([2.0, -1.0, 0.5])
y = X @ w_true + 0.1 * np.random.randn(n)

# Verify gradient numerically
w0 = np.zeros(d)
eps = 1e-6
numerical_grad = np.zeros(d)
for i in range(d):
    w_plus = w0.copy()
    w_minus = w0.copy()
    w_plus[i] += eps
    w_minus[i] -= eps
    numerical_grad[i] = (mse_loss(w_plus, X, y) - mse_loss(w_minus, X, y)) / (2*eps)

analytical_grad = grad_mse(w0, X, y)
print(f"Numerical grad:   {numerical_grad}")
print(f"Analytical grad:  {analytical_grad}")
print(f"Max diff:         {np.max(np.abs(numerical_grad - analytical_grad)):.2e}")
```

### Example 5: Using Autograd from JAX

```python
import jax
import jax.numpy as jnp

def f(x):
    return 3 * x**3 - 2 * x**2 + 5 * x - 1

grad_f = jax.grad(f)
print(f"f'(2) = {grad_f(2.0)}")  # 3*3*4 - 2*2*2 + 5 = 36 - 8 + 5 = 33

# Higher-order gradients
f_prime = jax.grad(f)
f_double_prime = jax.grad(f_prime)
print(f"f''(2) = {f_double_prime(2.0)}")  # 18*2 - 4 = 32

# Gradient of a multivariate function
def g(x):
    return jnp.sum(x**2)

grad_g = jax.grad(g)
x = jnp.array([1.0, 2.0, 3.0])
print(f"grad_g([1,2,3]) = {grad_g(x)}")  # [2, 4, 6]
```

## Glossary

| Term | Definition |
|------|------------|
| Derivative | The instantaneous rate of change of a function |
| Difference quotient | The ratio $(f(x+h)-f(x))/h$ used to define the derivative |
| Product rule | Rule for differentiating products of functions |
| Quotient rule | Rule for differentiating quotients of functions |
| Chain rule | Rule for differentiating compositions of functions |
| Higher-order derivative | Repeated differentiation (second, third derivatives, etc.) |
| Implicit differentiation | Differentiating equations where variables are not separated |
| Automatic differentiation | Computing derivatives by accumulating chain rule through a computation graph |
| Finite difference | Numerical approximation of derivatives using small differences |
| Gradient | Vector of partial derivatives (for multivariate functions) |
| Convexity | Property where $f''(x) \geq 0$ everywhere |

## Quick References

- [3Blue1Brown — Derivatives](https://www.youtube.com/watch?v=WUvTyaaNkzM) — geometric intuition
- [SymPy Calculus Documentation](https://docs.sympy.org/latest/tutorials/intro-tutorial/calculus.html) — symbolic differentiation
- [JAX Autodiff Cookbook](https://jax.readthedocs.io/en/latest/notebooks/autodiff_cookbook.html) — automatic differentiation with JAX
- [PyTorch Autograd](https://pytorch.org/tutorials/beginner/blitz/autograd_tutorial.html) — autograd in deep learning frameworks

## Next Steps

- [Applications of Derivatives](applications-of-derivatives.md) — optimization, gradient descent, Newton's method
- [Integration](integration.md) — the inverse operation of differentiation
