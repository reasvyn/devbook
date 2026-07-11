# Multivariate Calculus

## Description

Multivariate calculus extends derivatives and integrals to functions of multiple variables. The gradient, Jacobian, and Hessian are the core tools for optimizing neural networks, simulating physical systems, and understanding high-dimensional data — everything from backpropagation to numerical optimization.

## Prerequisites

- [Differentiation](differentiation.md) — derivative rules, chain rule, partial derivatives foundation
- [Integration](integration.md) — single-variable integration as basis for multiple integrals

## Table of Contents

- [Functions of Multiple Variables](#functions-of-multiple-variables)
- [Partial Derivatives](#partial-derivatives)
- [The Gradient Vector](#the-gradient-vector)
- [Directional Derivatives](#directional-derivatives)
- [The Jacobian Matrix](#the-jacobian-matrix)
- [The Hessian Matrix](#the-hessian-matrix)
- [Multiple Integrals](#multiple-integrals)
- [Change of Variables](#change-of-variables)
- [Applications in Machine Learning](#applications-in-machine-learning)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Functions of Multiple Variables

A function $f: \mathbb{R}^n \to \mathbb{R}$ maps a vector input to a scalar output.

$$
f(x_1, x_2, \dots, x_n) = y
$$

**Examples:**

- $f(x, y) = x^2 + y^2$ — a paraboloid
- $f(x, y) = \sin(x)\cos(y)$ — a periodic surface
- Loss function $L(w_1, \dots, w_d) = \frac{1}{n}\sum_{i=1}^n(y_i - \hat{y}_i)^2$ — MSE in regression

```python
import numpy as np

def f_surface(x, y):
    return np.sin(np.sqrt(x**2 + y**2))

xs = np.linspace(-5, 5, 100)
ys = np.linspace(-5, 5, 100)
X, Y = np.meshgrid(xs, ys)
Z = f_surface(X, Y)
print(f"Surface shape: {Z.shape}")
print(f"f(0,0) = {f_surface(0, 0):.4f}")
```

### Partial Derivatives

The partial derivative measures how $f$ changes when only one variable changes, holding all others fixed.

$$
\frac{\partial f}{\partial x} = \lim_{h \to 0} \frac{f(x+h, y) - f(x, y)}{h}
$$

**Example:** $f(x, y) = x^2 y + \sin(xy)$

$$
\frac{\partial f}{\partial x} = 2xy + y\cos(xy), \quad
\frac{\partial f}{\partial y} = x^2 + x\cos(xy)
$$

```python
import sympy as sp

x, y = sp.symbols('x y')
f = x**2 * y + sp.sin(x * y)
partial_x = sp.diff(f, x)
partial_y = sp.diff(f, y)
print(f"df/dx = {partial_x}")
print(f"df/dy = {partial_y}")
```

**Numerical partial derivatives:**

```python
import numpy as np

def partial_derivative(f, x_vec, i, h=1e-7):
    x_plus = x_vec.copy(); x_plus[i] += h
    x_minus = x_vec.copy(); x_minus[i] -= h
    return (f(x_plus) - f(x_minus)) / (2 * h)

def f_example(x_vec):
    x, y = x_vec[0], x_vec[1]
    return x**2 * y + np.sin(x * y)

x0 = np.array([1.0, 2.0])
for i in range(2):
    print(f"df/dx{i+1} at {x0} = {partial_derivative(f_example, x0, i):.8f}")
```

**Higher-order partial derivatives:**

$$
\frac{\partial^2 f}{\partial x^2},\quad
\frac{\partial^2 f}{\partial y^2},\quad
\frac{\partial^2 f}{\partial x \partial y}
$$

**Clairaut's theorem:** If second partial derivatives are continuous, mixed partials are equal: $\frac{\partial^2 f}{\partial x \partial y} = \frac{\partial^2 f}{\partial y \partial x}$.

```python
f = x**3 * y**2 + x * sp.sin(y)
f_xx = sp.diff(f, x, 2)
f_yy = sp.diff(f, y, 2)
f_xy = sp.diff(f, x, y)
f_yx = sp.diff(f, y, x)
print(f"Mixed partials equal: {sp.simplify(f_xy - f_yx) == 0}")
```

### The Gradient Vector

The gradient is a vector of all partial derivatives — it points in the direction of steepest ascent.

$$
\nabla f = \left\langle \frac{\partial f}{\partial x_1}, \frac{\partial f}{\partial x_2}, \dots, \frac{\partial f}{\partial x_n} \right\rangle
$$

**Properties:**
- $\nabla f$ points in the direction of greatest increase.
- $-\nabla f$ points in the direction of greatest decrease (gradient descent).
- $\nabla f$ is orthogonal to level sets of $f$.

```python
def gradient_numerical(f, x_vec, h=1e-7):
    grad = np.zeros_like(x_vec)
    for i in range(len(x_vec)):
        x_plus = x_vec.copy(); x_plus[i] += h
        x_minus = x_vec.copy(); x_minus[i] -= h
        grad[i] = (f(x_plus) - f(x_minus)) / (2 * h)
    return grad

print(f"Gradient: {gradient_numerical(lambda x: 3*x[0]**2+2*x[0]*x[1]+x[1]**2, np.array([1.,2.]))}")
```

**Gradient descent update:** $x_{k+1} = x_k - \alpha \nabla f(x_k)$

```python
def gradient_descent(grad_func, x0, alpha=0.1, max_iters=1000, tol=1e-8):
    x = x0.copy()
    for i in range(max_iters):
        x_new = x - alpha * grad_func(x)
        if np.linalg.norm(x_new - x) < tol:
            return x_new, i + 1
        x = x_new
    return x, max_iters

x_opt, iters = gradient_descent(lambda x: np.array([6*x[0]+2*x[1], 2*x[0]+2*x[1]]),
                                 np.array([10., -10.]))
print(f"Minimum at {x_opt}, found in {iters} iterations")
```

### Directional Derivatives

The directional derivative measures the rate of change of $f$ in the direction of a unit vector $u$.

$$
D_u f(x) = \nabla f(x) \cdot u
$$

```python
def directional_derivative(f, x_vec, direction, h=1e-7):
    direction = direction / np.linalg.norm(direction)
    grad = gradient_numerical(f, x_vec, h)
    return np.dot(grad, direction)

def f_valley(x):
    return x[0]**2 + 10 * x[1]**2

x0 = np.array([2.0, 2.0])
d = np.array([1, 1])
print(f"Directional derivative along {d}: {directional_derivative(f_valley, x0, d):.4f}")
```

### The Jacobian Matrix

For a vector-valued function $f: \mathbb{R}^n \to \mathbb{R}^m$, the Jacobian is an $m \times n$ matrix of all first-order partial derivatives.

$$
J_f = \begin{bmatrix}
\frac{\partial f_1}{\partial x_1} & \dots & \frac{\partial f_1}{\partial x_n} \\
\vdots & \ddots & \vdots \\
\frac{\partial f_m}{\partial x_1} & \dots & \frac{\partial f_m}{\partial x_n}
\end{bmatrix}
$$

**In backpropagation:** Each layer of a neural network has a Jacobian. The chain rule multiplies them.

```python
import numpy as np

def jacobian_numerical(f, x_vec, h=1e-7):
    m = len(f(x_vec))
    n = len(x_vec)
    J = np.zeros((m, n))
    for j in range(n):
        x_plus = x_vec.copy(); x_plus[j] += h
        x_minus = x_vec.copy(); x_minus[j] -= h
        J[:, j] = (f(x_plus) - f(x_minus)) / (2 * h)
    return J

def f_vector(x):
    return np.array([
        x[0]**2 + x[1],
        np.sin(x[0] * x[1]),
        x[0] * np.exp(x[1])
    ])

J = jacobian_numerical(f_vector, np.array([0.5, 1.0]))
print(f"Jacobian (3x2):\n{J}")
```

### The Hessian Matrix

The Hessian is a square matrix of second-order partial derivatives. For $f: \mathbb{R}^n \to \mathbb{R}$:

$$
H_f = \begin{bmatrix}
\frac{\partial^2 f}{\partial x_1^2} & \frac{\partial^2 f}{\partial x_1 \partial x_2} & \dots \\
\frac{\partial^2 f}{\partial x_2 \partial x_1} & \frac{\partial^2 f}{\partial x_2^2} & \dots \\
\vdots & \vdots & \ddots
\end{bmatrix}
$$

**Properties:**
- By Clairaut's theorem, the Hessian is symmetric: $H_{ij} = H_{ji}$.
- If $H$ is positive definite at a critical point, it is a local minimum.
- If $H$ is negative definite, it is a local maximum.
- If $H$ has mixed signs, it is a saddle point.

```python
import numpy as np

def hessian_numerical(f, x_vec, h=1e-5):
    n = len(x_vec)
    H = np.zeros((n, n))
    for i in range(n):
        for j in range(i, n):
            x_pp = x_vec.copy(); x_pp[i] += h; x_pp[j] += h
            x_pm = x_vec.copy(); x_pm[i] += h; x_pm[j] -= h
            x_mp = x_vec.copy(); x_mp[i] -= h; x_mp[j] += h
            x_mm = x_vec.copy(); x_mm[i] -= h; x_mm[j] -= h
            H[i, j] = (f(x_pp) - f(x_pm) - f(x_mp) + f(x_mm)) / (4 * h**2)
            H[j, i] = H[i, j]
    return H

def f_rosenbrock(x):
    return (1 - x[0])**2 + 100 * (x[1] - x[0]**2)**2

H = hessian_numerical(f_rosenbrock, np.array([1.0, 1.0]))
print(f"Hessian at minimum:\n{H}")
print(f"Eigenvalues: {np.linalg.eigvals(H)}")
```

**Newton's method for optimization** uses the Hessian:

$$
x_{k+1} = x_k - H_f(x_k)^{-1} \nabla f(x_k)
$$

This converges faster than gradient descent near the optimum but requires computing and inverting the Hessian, which is $O(n^3)$.

```python
def newton_optimization(grad, hessian, x0, tol=1e-8, max_iters=50):
    x = x0.copy()
    for i in range(max_iters):
        g = grad(x)
        H = hessian(x)
        try:
            step = np.linalg.solve(H, g)
        except np.linalg.LinAlgError:
            break
        x_new = x - step
        if np.linalg.norm(step) < tol:
            return x_new, i + 1
        x = x_new
    return x, max_iters

def grad_rosenbrock(x):
    dx = -2*(1 - x[0]) - 400*x[0]*(x[1] - x[0]**2)
    dy = 200*(x[1] - x[0]**2)
    return np.array([dx, dy])

def hessian_rosenbrock(x):
    return np.array([
        [2 - 400*x[1] + 1200*x[0]**2, -400*x[0]],
        [-400*x[0], 200]
    ])

x_opt, iters = newton_optimization(grad_rosenbrock, hessian_rosenbrock,
                                    np.array([-1.2, 1.0]))
print(f"Newton: minimum at {x_opt}, {iters} iterations")
```

### Multiple Integrals

Double integrals compute volume under a surface $z = f(x, y)$:

$$
\iint_R f(x, y) \, dA = \int_a^b \int_c^d f(x, y) \, dy \, dx
$$

**Iterated integration:** Integrate with respect to one variable at a time.

```python
import sympy as sp
x, y = sp.symbols('x y')
inner = sp.integrate(x**2 * y, (y, 0, 3))
outer = sp.integrate(inner, (x, 0, 2))
print(f"Double integral of x^2*y: {outer}")
```

**Numerical double integration:**

```python
from scipy import integrate
import numpy as np

def f_2d(x, y):
    return x**2 * y

result, error = integrate.nquad(f_2d, [[0, 2], [0, 3]])
print(f"Numerical double integral: {result:.6f} (expected 18)")
```

**Triple integrals** extend to 3D volumes:

```python
def f_3d(x, y, z):
    return x * y * z

result_3d, _ = integrate.nquad(f_3d, [[0, 1], [0, 1], [0, 1]])
print(f"Triple integral of xyz: {result_3d:.6f} (expected 0.125)")
```

### Change of Variables

When integrating over non-rectangular regions, change variables using the Jacobian determinant.

For a transformation $T(u, v) = (x(u,v), y(u,v))$:

$$
\iint_R f(x, y) \, dx \, dy = \iint_S f(T(u,v)) \left| \det J_T \right| \, du \, dv
$$

**Example: Polar coordinates**

$$
x = r\cos\theta, \quad y = r\sin\theta, \quad \left| \det J \right| = r
$$

$$
\iint_R f(x,y) \, dx\,dy = \iint_S f(r\cos\theta, r\sin\theta) \, r \, dr\,d\theta
$$

```python
import numpy as np
from scipy import integrate

# Compute area of a circle of radius 2 using polar coordinates
# Area = ∫_0^{2π} ∫_0^2 r dr dθ

def integrand_polar(theta, r):
    return r  # f = 1 in polar, times Jacobian r

area, _ = integrate.nquad(integrand_polar, [[0, 2], [0, 2*np.pi]])
print(f"Area of circle (r=2): {area:.6f} (expected {4*np.pi:.6f})")
```

**Change of variables in probability:**

If $X$ has PDF $f_X$, then $Y = g(X)$ has PDF:

$$
f_Y(y) = f_X(g^{-1}(y)) \left| \frac{d}{dy} g^{-1}(y) \right|
$$

### Applications in Machine Learning

**Backpropagation** is the chain rule applied to neural networks. For a network with layers $f_1, f_2, \dots, f_L$:

$$
\frac{\partial L}{\partial w_{ij}^{(l)}} = \frac{\partial L}{\partial a_j^{(L)}} \cdot \frac{\partial a_j^{(L)}}{\partial a_k^{(L-1)}} \cdots \frac{\partial a_p^{(l+1)}}{\partial w_{ij}^{(l)}}
$$

Each term is a Jacobian matrix.

```python
import numpy as np

def mse_gradient(X, y, w):
    """Gradient of MSE loss for linear regression: (2/n) * X^T (Xw - y)."""
    n = len(y)
    return (2/n) * X.T @ (X @ w - y)

def gradient_check(f, grad, w, eps=1e-7):
    """Verify gradient computation using finite differences."""
    numerical = np.zeros_like(w)
    for i in range(len(w)):
        w_plus = w.copy(); w_plus[i] += eps
        w_minus = w.copy(); w_minus[i] -= eps
        numerical[i] = (f(w_plus) - f(w_minus)) / (2 * eps)
    return np.linalg.norm(numerical - grad) / (np.linalg.norm(numerical) + 1e-12)

np.random.seed(42)
n, d = 100, 5
X = np.random.randn(n, d)
w_true = np.random.randn(d)
y = X @ w_true + 0.1 * np.random.randn(n)

def loss(w):
    return np.mean((X @ w - y)**2)

w0 = np.zeros(d)
g = mse_gradient(X, y, w0)
diff = gradient_check(loss, g, w0)
print(f"Gradient check relative diff: {diff:.2e}")
```

**Training a neural network with backpropagation:**

```python
import numpy as np

def sigmoid(x):
    return 1 / (1 + np.exp(-np.clip(x, -100, 100)))

def sigmoid_prime(x):
    s = sigmoid(x)
    return s * (1 - s)

def train_nn(X, y, hidden_size=10, lr=0.1, epochs=1000):
    n, d = X.shape
    np.random.seed(42)
    W1 = np.random.randn(d, hidden_size) * 0.01
    b1 = np.zeros((1, hidden_size))
    W2 = np.random.randn(hidden_size, 1) * 0.01
    b2 = np.zeros((1, 1))

    for epoch in range(epochs):
        # Forward pass
        Z1 = X @ W1 + b1
        A1 = sigmoid(Z1)
        Z2 = A1 @ W2 + b2
        y_pred = Z2

        loss = np.mean((y_pred - y)**2)

        # Backward pass (chain rule)
        dZ2 = 2 * (y_pred - y) / n
        dW2 = A1.T @ dZ2
        db2 = np.sum(dZ2, axis=0, keepdims=True)
        dA1 = dZ2 @ W2.T
        dZ1 = dA1 * sigmoid_prime(Z1)
        dW1 = X.T @ dZ1
        db1 = np.sum(dZ1, axis=0, keepdims=True)

        # Gradient descent
        W1 -= lr * dW1; b1 -= lr * db1
        W2 -= lr * dW2; b2 -= lr * db2

        if epoch % 200 == 0:
            print(f"Epoch {epoch}: loss = {loss:.6f}")

    return W1, b1, W2, b2

# Test on synthetic data
np.random.seed(42)
n = 500
X = np.random.randn(n, 3)
y = (X[:, 0:1] + X[:, 1:2] - 2*X[:, 2:3]).reshape(-1, 1)

train_nn(X, y, hidden_size=8, lr=0.5, epochs=1000)
```

**Gradient of softmax with cross-entropy loss:**

$$
\frac{\partial L}{\partial z_i} = p_i - y_i
$$

where $p_i = \frac{e^{z_i}}{\sum_j e^{z_j}}$ is the softmax probability and $y_i$ is the one-hot label.

```python
def softmax_gradient(z, y_onehot):
    """Gradient of cross-entropy loss w.r.t. logits."""
    exp_z = np.exp(z - np.max(z, axis=1, keepdims=True))
    p = exp_z / np.sum(exp_z, axis=1, keepdims=True)
    return p - y_onehot
```

## Glossary

| Term | Definition |
|------|------------|
| Partial derivative | Derivative of a multivariable function with respect to one variable |
| Gradient | Vector of all partial derivatives, pointing in the direction of steepest ascent |
| Directional derivative | Rate of change in a specified direction |
| Jacobian | Matrix of all first-order partial derivatives of a vector-valued function |
| Hessian | Matrix of all second-order partial derivatives of a scalar function |
| Multiple integral | Integral over a region in 2D, 3D, or higher dimensions |
| Change of variables | Transforming an integral using a substitution with Jacobian determinant |
| Backpropagation | Algorithm for computing gradients through neural networks via chain rule |
| Positive definite | A symmetric matrix with all positive eigenvalues |
| Saddle point | A critical point where the Hessian has both positive and negative eigenvalues |
| BFGS | Quasi-Newton optimization algorithm that approximates the Hessian |
| Clairaut's theorem | Mixed partial derivatives are equal when continuous |
| Polar coordinates | Coordinate system using radius and angle: $x = r\cos\theta, y = r\sin\theta$ |

## Quick References

- [JAX Autodiff Cookbook](https://jax.readthedocs.io/en/latest/notebooks/autodiff_cookbook.html) — automatic differentiation
- [Scipy Optimize Documentation](https://docs.scipy.org/doc/scipy/reference/optimize.html) — optimization algorithms
- [CS231n Backpropagation Notes](https://cs231n.github.io/optimization-2/) — intuitive backprop explanation
- [Matrix Calculus](https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf) — The Matrix Cookbook

## Next Steps

- **Vector Calculus** — gradient, divergence, curl, line integrals, Green's theorem (planned)
- [Sequences and Series](sequences-and-series.md) — discrete sums and function approximation
