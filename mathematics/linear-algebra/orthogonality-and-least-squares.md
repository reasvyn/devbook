# Orthogonality & Least Squares

## Description

Orthogonality is the geometric foundation of many linear algebra applications — projections, least squares regression, Gram-Schmidt orthogonalization, and QR decomposition. When exact solutions are impossible (as in overdetermined systems), the least squares solution provides the best approximation by minimizing the squared error. This is the mathematics behind linear regression, data fitting, and countless optimization problems in machine learning and engineering.

## Prerequisites

- [Vectors & Vector Spaces](vectors-and-vector-spaces.md) — vectors, dot product, span, subspaces
- [Matrices & Matrix Operations](matrices-and-operations.md) — matrix multiplication, transpose, inverse

## Table of Contents

- [Dot Product and Length](#dot-product-and-length)
- [Angle Between Vectors](#angle-between-vectors)
- [Orthogonal Vectors and Orthogonal Complements](#orthogonal-vectors-and-orthogonal-complements)
- [Orthogonal Projection](#orthogonal-projection)
- [Gram-Schmidt Process](#gram-schmidt-process)
- [QR Decomposition](#qr-decomposition)
- [Least Squares](#least-squares)
- [Normal Equations](#normal-equations)
- [Applications](#applications)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Dot Product and Length

The **dot product** (inner product) of two vectors $\mathbf{u}, \mathbf{v} \in \mathbb{R}^n$:

$$
\mathbf{u} \cdot \mathbf{v} = \sum_{i=1}^{n} u_i v_i = \mathbf{u}^T \mathbf{v}
$$

```python
import numpy as np

u = np.array([1, 2, 3])
v = np.array([4, 5, 6])
dot = np.dot(u, v)      # 32
dot = u @ v             # same, using @ operator
dot = np.sum(u * v)     # also same
```

**Length (norm):**

$$
\|\mathbf{v}\| = \sqrt{\mathbf{v} \cdot \mathbf{v}} = \sqrt{\sum_{i=1}^{n} v_i^2}
$$

```python
v = np.array([3, 4])
norm = np.linalg.norm(v)  # 5.0
norm_manual = np.sqrt(v @ v)  # also 5.0
```

**Properties of the dot product:**

- **Symmetric:** $\mathbf{u} \cdot \mathbf{v} = \mathbf{v} \cdot \mathbf{u}$
- **Bilinear:** $(c\mathbf{u} + \mathbf{w}) \cdot \mathbf{v} = c(\mathbf{u} \cdot \mathbf{v}) + \mathbf{w} \cdot \mathbf{v}$
- **Positive definite:** $\mathbf{v} \cdot \mathbf{v} \geq 0$ and equals $0$ only if $\mathbf{v} = \mathbf{0}$
- **Cauchy-Schwarz inequality:** $|\mathbf{u} \cdot \mathbf{v}| \leq \|\mathbf{u}\| \|\mathbf{v}\|$
- **Triangle inequality:** $\|\mathbf{u} + \mathbf{v}\| \leq \|\mathbf{u}\| + \|\mathbf{v}\|$

```python
# Verify Cauchy-Schwarz
u = np.array([1, 2, 3])
v = np.array([4, 5, 6])
lhs = np.abs(u @ v)
rhs = np.linalg.norm(u) * np.linalg.norm(v)
print(f"|u·v| = {lhs:.4f} <= {rhs:.4f} (CS bound)")
print(f"Equality iff ||u·v|| = ||u|| ||v|| (collinear vectors)")

# Triangle inequality
print(f"||u+v|| = {np.linalg.norm(u+v):.4f}")
print(f"||u|| + ||v|| = {np.linalg.norm(u) + np.linalg.norm(v):.4f}")
```

### Angle Between Vectors

The dot product relates to the angle $\theta$ between vectors:

$$
\cos \theta = \frac{\mathbf{u} \cdot \mathbf{v}}{\|\mathbf{u}\| \|\mathbf{v}\|}
$$

```python
def angle_between(u, v, degrees=True):
    """Compute the angle between two vectors."""
    cos_theta = (u @ v) / (np.linalg.norm(u) * np.linalg.norm(v))
    # Clamp to handle floating point
    cos_theta = np.clip(cos_theta, -1.0, 1.0)
    theta = np.arccos(cos_theta)
    return np.degrees(theta) if degrees else theta

u = np.array([1, 0])
v = np.array([0, 1])
print(f"Angle between (1,0) and (0,1): {angle_between(u, v)} degrees")

u2 = np.array([1, 0])
v2 = np.array([1, 1])
print(f"Angle between (1,0) and (1,1): {angle_between(u2, v2)} degrees")

# Collinear vectors (angle = 0)
u3 = np.array([1, 2])
v3 = np.array([3, 6])
print(f"Angle between collinear: {angle_between(u3, v3)} degrees")
```

**Special cases:**

- $\mathbf{u} \cdot \mathbf{v} > 0$: acute angle ($\theta < 90^\circ$)
- $\mathbf{u} \cdot \mathbf{v} = 0$: right angle ($\theta = 90^\circ$)
- $\mathbf{u} \cdot \mathbf{v} < 0$: obtuse angle ($\theta > 90^\circ$)

### Orthogonal Vectors and Orthogonal Complements

Two vectors are **orthogonal** if their dot product is zero: $\mathbf{u} \cdot \mathbf{v} = 0$.

A set of vectors is **orthonormal** if every pair is orthogonal and each has unit length:

$$
\mathbf{q}_i \cdot \mathbf{q}_j = \begin{cases} 1 & i = j \\ 0 & i \neq j \end{cases}
$$

```python
# Orthonormal basis for R^2
q1 = np.array([1, 0])
q2 = np.array([0, 1])
print(f"q1·q2 = {q1 @ q2}")  # 0 (orthogonal)
print(f"||q1|| = {np.linalg.norm(q1)}")  # 1 (unit)
print(f"||q2|| = {np.linalg.norm(q2)}")  # 1 (unit)

# Non-standard orthonormal basis
theta = np.radians(30)
Q = np.array([[np.cos(theta), -np.sin(theta)],
              [np.sin(theta),  np.cos(theta)]])
print(f"Columns are orthonormal:\n{Q.T @ Q}")
# Q^T Q = I (orthogonal matrix property)
```

**Orthogonal complement:** For a subspace $W \subseteq V$, the orthogonal complement $W^\perp$ is:

$$
W^\perp = \{\mathbf{v} \in V \mid \mathbf{v} \cdot \mathbf{w} = 0 \text{ for all } \mathbf{w} \in W\}
$$

```python
# W = span{[1, 1]} (line y=x)
w = np.array([1, 1])
# W_perp = span{[1, -1]} (line y=-x, perpendicular)
w_perp = np.array([1, -1])
print(f"w · w_perp = {w @ w_perp}")  # 0

# Every vector in R^2 can be decomposed uniquely as w_comp + w_perp_comp
v = np.array([3, 5])
w_comp = ((v @ w) / (w @ w)) * w  # projection onto w
w_perp_comp = v - w_comp            # component in W_perp
print(f"v = {v}")
print(f"w_comp = {w_comp}, w_perp_comp = {w_perp_comp}")
print(f"w_comp + w_perp_comp = {w_comp + w_perp_comp}")
```

**Fundamental theorem:** For any subspace $W \subseteq \mathbb{R}^n$:

$$
\dim(W) + \dim(W^\perp) = n
$$

The four fundamental subspaces are related by orthogonality:

- $\text{Row}(A)^\perp = \text{Null}(A)$
- $\text{Col}(A)^\perp = \text{Null}(A^T)$

```python
def check_orthogonal_subspaces(A):
    """Verify row space ⟂ null space and col space ⟂ left null space."""
    m, n = A.shape
    U, s, Vt = np.linalg.svd(A)
    r = np.sum(s > 1e-10)

    row_basis = Vt[:r]       # rows of Vt
    null_basis = Vt[r:]       # null space basis

    col_basis = U[:, :r]      # column space
    left_null = U[:, r:]      # left null space

    print(f"Row space ⟂ Null space? ", end="")
    orthogonal = all(np.allclose(rb @ nb.T, 0) for rb in row_basis for nb in null_basis)
    print(orthogonal)

    print(f"Col space ⟂ Left null? ", end="")
    orthogonal = all(np.allclose(cb @ ln.T, 0) for cb in col_basis.T for ln in left_null.T)
    print(orthogonal)

A = np.array([[1, 2, 1],
              [2, 4, 2],
              [1, 0, 0]])
check_orthogonal_subspaces(A)
```

### Orthogonal Projection

The **orthogonal projection** of $\mathbf{v}$ onto a subspace $W$ is the closest point in $W$ to $\mathbf{v}$. It is the unique vector $\mathbf{w} \in W$ such that $\mathbf{v} - \mathbf{w} \in W^\perp$.

**Projection onto a line through origin (direction $\mathbf{a}$):**

$$
\text{proj}_{\mathbf{a}}(\mathbf{v}) = \frac{\mathbf{v} \cdot \mathbf{a}}{\mathbf{a} \cdot \mathbf{a}} \mathbf{a}
$$

```python
def project_onto_line(v, a):
    """Project vector v onto the line spanned by a."""
    return ((v @ a) / (a @ a)) * a

v = np.array([3, 4])
a = np.array([1, 1])
proj = project_onto_line(v, a)
print(f"Projection of {v} onto line y=x: {proj}")
print(f"Residual (perpendicular): {v - proj}")
print(f"Residual ⟂ line? {(v - proj) @ a}")  # ≈ 0
```

**Projection matrix onto the column space of $A$:**

$$
P = A (A^T A)^{-1} A^T
$$

The matrix $P$ satisfies $P^2 = P$ (idempotent) and $P^T = P$ (symmetric).

```python
def projection_matrix(A):
    """Return the projection matrix onto Col(A)."""
    return A @ np.linalg.inv(A.T @ A) @ A.T

A = np.array([[1, 0],
              [0, 1],
              [0, 0]])  # xy-plane in R^3
P = projection_matrix(A)
print(f"Projection matrix:\n{np.round(P, 2)}")
print(f"P^2 = P? {np.allclose(P @ P, P)}")

# Project a point onto the xy-plane
v = np.array([3, 4, 5])
proj = P @ v
print(f"Projection of {v} onto xy-plane: {proj}")
# Result: [3, 4, 0]
```

**Properties of orthogonal projection:**

- $P\mathbf{v}$ is the vector in $\text{Col}(A)$ closest to $\mathbf{v}$
- $I - P$ projects onto $\text{Col}(A)^\perp$
- $P$ is uniquely determined by the subspace, not the basis

```python
# Complementary projection
P_perp = np.eye(3) - P
proj_perp = P_perp @ v
print(f"Projection onto orthogonal complement: {proj_perp}")
print(f"Sum = original? {np.allclose(proj + proj_perp, v)}")
```

### Gram-Schmidt Process

The **Gram-Schmidt process** takes a set of linearly independent vectors and produces an orthonormal basis for the same subspace.

Given $\{\mathbf{a}_1, \dots, \mathbf{a}_k\}$, compute:

1. $\mathbf{u}_1 = \mathbf{a}_1 / \|\mathbf{a}_1\|$
2. For $i = 2, \dots, k$:
   - $\mathbf{v}_i = \mathbf{a}_i - \sum_{j=1}^{i-1} (\mathbf{a}_i \cdot \mathbf{u}_j) \mathbf{u}_j$
   - $\mathbf{u}_i = \mathbf{v}_i / \|\mathbf{v}_i\|$

```python
def gram_schmidt(vectors):
    """Gram-Schmidt orthonormalization.

    Args:
        vectors: list of numpy arrays (linearly independent)
    Returns:
        Q: matrix with orthonormal columns
    """
    k = len(vectors)
    n = len(vectors[0])
    Q = np.zeros((n, k))

    for i in range(k):
        v = vectors[i].astype(float).copy()
        # Subtract projections onto previous vectors
        for j in range(i):
            v -= (vectors[i] @ Q[:, j]) * Q[:, j]
        # Normalize
        Q[:, i] = v / np.linalg.norm(v)

    return Q

vectors = [np.array([1, 1, 0]),
           np.array([1, 0, 1]),
           np.array([0, 1, 1])]

Q = gram_schmidt(vectors)
print("Orthonormal basis:\n", np.round(Q, 4))
print("Q^T Q:\n", np.round(Q.T @ Q, 4))
# Should be identity (within rounding)

# Verify: original vectors are in span of Q
for i, v in enumerate(vectors):
    coords = Q.T @ v
    reconstructed = Q @ coords
    print(f"a{i} in span? {np.allclose(v, reconstructed)}")
```

**Modified Gram-Schmidt** is numerically more stable:

```python
def modified_gram_schmidt(vectors):
    """More numerically stable version of Gram-Schmidt."""
    k = len(vectors)
    n = len(vectors[0])
    Q = np.zeros((n, k))
    R = np.zeros((k, k))

    for i in range(k):
        Q[:, i] = vectors[i]
        for j in range(i):
            R[j, i] = Q[:, j] @ Q[:, i]
            Q[:, i] -= R[j, i] * Q[:, j]
        R[i, i] = np.linalg.norm(Q[:, i])
        Q[:, i] /= R[i, i]

    return Q, R

Q, R = modified_gram_schmidt(vectors)
print("Via modified GS:\n", np.round(Q, 4))
print("R factor:\n", np.round(R, 4))
```

### QR Decomposition

The **QR decomposition** factors a matrix $A \in \mathbb{R}^{m \times n}$ into:

$$
A = QR
$$

where $Q \in \mathbb{R}^{m \times n}$ has orthonormal columns and $R \in \mathbb{R}^{n \times n}$ is upper triangular.

```python
A = np.array([[1, 1, 0],
              [1, 0, 1],
              [0, 1, 1]])

# NumPy's QR (uses Householder reflections, more stable)
Q, R = np.linalg.qr(A)
print("Q:\n", np.round(Q, 4))
print("R:\n", np.round(R, 4))
print("Q^T Q:\n", np.round(Q.T @ Q, 4))
print("QR == A?", np.allclose(Q @ R, A))

# Reduced vs. full QR
A_rect = np.array([[1, 1],
                   [1, 0],
                   [0, 1]])
Q_full, R_full = np.linalg.qr(A_rect, mode='reduced')
print(f"Reduced Q: {Q_full.shape}, R: {R_full.shape}")

Q_econ, R_econ = np.linalg.qr(A_rect, mode='reduced')
print(f"Economic Q: {Q_econ.shape}, R: {R_econ.shape}")
```

**Solving least squares with QR:**

The QR approach to least squares is numerically more stable than the normal equations:

$$
A\mathbf{x} \approx \mathbf{b} \quad \Rightarrow \quad R\mathbf{x} = Q^T \mathbf{b}
$$

```python
def least_squares_qr(A, b):
    """Solve min ||Ax - b||^2 using QR decomposition."""
    Q, R = np.linalg.qr(A)
    return np.linalg.solve(R, Q.T @ b)

# Overdetermined system
A = np.array([[1, 1],
              [1, 2],
              [1, 3],
              [1, 4]])
b = np.array([2.1, 3.9, 6.1, 7.8])

x_qr = least_squares_qr(A, b)
x_lstsq = np.linalg.lstsq(A, b, rcond=None)[0]
print(f"QR solution: {x_qr}")
print(f"lstsq solution: {x_lstsq}")
print(f"Match: {np.allclose(x_qr, x_lstsq)}")
```

**Why QR is preferred over normal equations:**

- $A^T A$ squares the condition number: $\kappa(A^T A) = \kappa(A)^2$
- QR has the same condition number as $A$
- For ill-conditioned problems, QR is significantly more accurate

```python
# Compare stability on ill-conditioned matrix
A_ill = np.array([[1, 1],
                  [1, 1.0001],
                  [1, 1.0002]])
b_ill = np.array([2, 3, 4])

# Normal equations (bad)
x_ne = np.linalg.solve(A_ill.T @ A_ill, A_ill.T @ b_ill)

# QR (good)
x_qr = least_squares_qr(A_ill, b_ill)

# Expected: large first coeff, x1 + x2 ≈ average
print(f"Normal eq: {x_ne}")
print(f"QR: {x_qr}")
print(f"Residual NE: {np.linalg.norm(A_ill @ x_ne - b_ill):.2e}")
print(f"Residual QR: {np.linalg.norm(A_ill @ x_qr - b_ill):.2e}")
```

### Least Squares

The **least squares problem**: find $\mathbf{x}$ minimizing $\|A\mathbf{x} - \mathbf{b}\|^2$ for $A \in \mathbb{R}^{m \times n}$ with $m > n$ (overdetermined).

$$
\mathbf{x}^* = \arg\min_{\mathbf{x}} \|A\mathbf{x} - \mathbf{b}\|^2
$$

```python
# Data: points (x_i, y_i)
x_data = np.array([0, 1, 2, 3, 4, 5])
y_data = np.array([0.1, 1.9, 3.8, 6.2, 7.9, 10.1])

# Fit model: y = β0 + β1*x
# Design matrix: [[1, x0], [1, x1], ...]
A = np.column_stack([np.ones_like(x_data), x_data])

# Least squares
beta, residuals, rank, sv = np.linalg.lstsq(A, y_data, rcond=None)
print(f"β0 (intercept) = {beta[0]:.4f}")
print(f"β1 (slope) = {beta[1]:.4f}")

# Predictions
y_pred = A @ beta
print(f"RSS: {np.sum((y_data - y_pred)**2):.4f}")

# R-squared
ss_total = np.sum((y_data - y_data.mean())**2)
ss_residual = np.sum((y_data - y_pred)**2)
r_squared = 1 - ss_residual / ss_total
print(f"R² = {r_squared:.4f}")
```

### Normal Equations

The least squares solution satisfies the **normal equations**:

$$
A^T A \mathbf{x} = A^T \mathbf{b}
$$

```python
def least_squares_ne(A, b):
    """Solve least squares using normal equations."""
    return np.linalg.solve(A.T @ A, A.T @ b)

A = np.array([[1, 1],
              [1, 2],
              [1, 3],
              [1, 4]])
b = np.array([2.1, 3.9, 6.1, 7.8])

x_ne = least_squares_ne(A, b)
print(f"Normal eq solution: {x_ne}")

# Verify: A^T A x = A^T b
lhs = A.T @ A @ x_ne
rhs = A.T @ b
print(f"Satisfies normal eq? {np.allclose(lhs, rhs)}")
```

**Derivation of normal equations:**

Set the gradient of $f(\mathbf{x}) = \|A\mathbf{x} - \mathbf{b}\|^2 = (A\mathbf{x} - \mathbf{b})^T (A\mathbf{x} - \mathbf{b})$ to zero:

$$
\nabla f = 2A^T(A\mathbf{x} - \mathbf{b}) = 0
$$

Which gives $A^T A \mathbf{x} = A^T \mathbf{b}$.

**When normal equations work well:**

- $A$ is well-conditioned (condition number not too large)
- $m$ (number of equations) is moderate
- Precision requirements are modest

**When to avoid normal equations:**

- Near-rank-deficient $A$ (high condition number)
- $n$ is large (solving $n \times n$ system is expensive)
- When numerical stability is paramount (use QR or SVD instead)

```python
def compare_methods(A, b):
    """Compare least squares solution methods."""
    x_ne = least_squares_ne(A, b)
    x_qr = least_squares_qr(A, b)
    x_svd = np.linalg.lstsq(A, b, rcond=None)[0]

    residuals = {
        "Normal eq": np.linalg.norm(A @ x_ne - b),
        "QR": np.linalg.norm(A @ x_qr - b),
        "SVD (lstsq)": np.linalg.norm(A @ x_svd - b)
    }
    return residuals

# Well-conditioned
A1 = np.array([[1, 1], [1, 2], [1, 3]])
b1 = np.array([2, 3, 4])
print("Well-conditioned:", compare_methods(A1, b1))

# Ill-conditioned
A2 = np.array([[1, 1], [1, 1.0001], [1, 1.0002]])
b2 = np.array([2, 3, 4])
print("Ill-conditioned:", compare_methods(A2, b2))
```

### Applications

**Linear regression:** The most common application of least squares.

```python
# Multiple linear regression
np.random.seed(42)
n = 100
p = 5

X = np.random.randn(n, p)
true_beta = np.array([2.0, -1.5, 0.5, 0.0, 3.0])
y = X @ true_beta + np.random.randn(n) * 0.5

# Add intercept column
X_with_intercept = np.column_stack([np.ones(n), X])

# Fit
beta_hat, _, _, _ = np.linalg.lstsq(X_with_intercept, y, rcond=None)
print(f"Estimated coefficients: {beta_hat}")
print(f"True coefficients:     [0, {true_beta}]")  # first is intercept
```

**Polynomial fitting:**

```python
x = np.array([0, 1, 2, 3, 4, 5])
y = np.array([0.5, 0.8, 2.5, 5.0, 8.2, 13.1])

# Quadratic fit: y = β0 + β1*x + β2*x^2
degree = 2
A = np.vander(x, degree + 1, increasing=True)
beta = np.linalg.lstsq(A, y, rcond=None)[0]
print(f"Coefficients: {beta}")

# Predict
x_new = 6
A_new = np.vander([x_new], degree + 1, increasing=True)
y_pred = A_new @ beta
print(f"Prediction at x=6: {y_pred:.4f}")
```

**Image reconstruction from projections:** CT scanners solve least squares problems to reconstruct images from X-ray measurements.

## Study Cases

### Study Case 1: Fitting a Plane to 3D Points

Given noisy 3D points $(x_i, y_i, z_i)$, fit a plane $z = \beta_0 + \beta_1 x + \beta_2 y$.

```python
np.random.seed(42)
n = 50
x = np.random.uniform(-5, 5, n)
y = np.random.uniform(-5, 5, n)
# True plane: z = 2 + 0.5*x - 0.3*y + noise
z_true = 2 + 0.5 * x - 0.3 * y
z = z_true + np.random.randn(n) * 0.5

# Design matrix: [1, x, y]
A = np.column_stack([np.ones(n), x, y])
beta = np.linalg.lstsq(A, z, rcond=None)[0]
print(f"Fitted plane: z = {beta[0]:.4f} + {beta[1]:.4f}*x + {beta[2]:.4f}*y")
print(f"True plane:   z = 2.0000 + 0.5000*x - 0.3000*y")

# Residual analysis
residuals = z - A @ beta
print(f"RMS residual: {np.sqrt(np.mean(residuals**2)):.4f}")
print(f"Max residual: {np.max(np.abs(residuals)):.4f}")
```

### Study Case 2: Least Squares for Sensor Calibration

Calibrating a temperature sensor: known reference temperatures vs. sensor readings.

```python
# Sensor readings vs. actual temperature (linear response)
sensor_readings = np.array([15.2, 25.1, 34.8, 45.3, 55.0, 64.9])
actual_temps = np.array([15.0, 25.0, 35.0, 45.0, 55.0, 65.0])

# Model: actual = offset + gain * sensor
A = np.column_stack([np.ones(len(sensor_readings)), sensor_readings])
beta = np.linalg.lstsq(A, actual_temps, rcond=None)[0]

print(f"Offset: {beta[0]:.4f}, Gain: {beta[1]:.4f}")
print(f"Calibration: actual = {beta[0]:.4f} + {beta[1]:.4f} * reading")

# Use calibration
new_reading = 72.3
predicted_actual = beta[0] + beta[1] * new_reading
print(f"Sensor reading {new_reading} -> actual {predicted_actual:.2f}°C")
```

### Study Case 3: Financial Data — Removing Market Trend

```python
np.random.seed(42)
n_days = 252
market_returns = np.random.randn(n_days) * 0.01
stock_returns = 0.8 * market_returns + np.random.randn(n_days) * 0.005

beta = (stock_returns @ market_returns) / (market_returns @ market_returns)
market_component = beta * market_returns
idiosyncratic = stock_returns - market_component
print(f"Beta: {beta:.4f}, Idio vol: {np.std(idiosyncratic):.4f}")
print(f"Orthogonal? {np.abs(market_component @ idiosyncratic) < 1e-10}")
```

## Examples

### Example 1: Dot Product, Length, Angle

```python
v1 = np.array([1, 2, 3])
v2 = np.array([-4, 5, 6])

print(f"v1 · v2 = {v1 @ v2}")
print(f"||v1|| = {np.linalg.norm(v1):.4f}")
print(f"||v2|| = {np.linalg.norm(v2):.4f}")

cos_theta = (v1 @ v2) / (np.linalg.norm(v1) * np.linalg.norm(v2))
angle = np.degrees(np.arccos(np.clip(cos_theta, -1, 1)))
print(f"Angle = {angle:.2f}°")

# Orthogonal
v3 = np.array([1, 0, 0])
v4 = np.array([0, 1, 0])
print(f"v3 · v4 = {v3 @ v4} (orthogonal)")
```

### Example 2: Gram-Schmidt Step by Step

```python
vectors = [np.array([1, 1, 1]),
           np.array([1, 0, 2]),
           np.array([1, 2, 0])]

Q = np.zeros((3, 3))

v0 = vectors[0]
Q[:, 0] = v0 / np.linalg.norm(v0)

v1 = vectors[1]
proj = (v1 @ Q[:, 0]) * Q[:, 0]
u1 = v1 - proj
Q[:, 1] = u1 / np.linalg.norm(u1)

v2 = vectors[2]
proj1 = (v2 @ Q[:, 0]) * Q[:, 0]
proj2 = (v2 @ Q[:, 1]) * Q[:, 1]
u2 = v2 - proj1 - proj2
Q[:, 2] = u2 / np.linalg.norm(u2)

print("Q:\n", np.round(Q, 4))
print("Q^T Q:\n", np.round(Q.T @ Q, 4))
```

### Example 3: QR for Least Squares

```python
# Noisy data
np.random.seed(42)
x_pts = np.array([0, 1, 2, 3, 4, 5])
y_pts = 0.5 * x_pts + 1.0 + np.random.randn(6) * 0.3

# Fit linear model via QR
A = np.column_stack([np.ones_like(x_pts), x_pts])
Q, R = np.linalg.qr(A)
beta = np.linalg.solve(R, Q.T @ y_pts)
print(f"QR fit: y = {beta[0]:.4f} + {beta[1]:.4f}x")

# Compare with polyfit
beta_poly = np.polyfit(x_pts, y_pts, 1)
print(f"Polyfit: y = {beta_poly[1]:.4f} + {beta_poly[0]:.4f}x")

# Confidence via covariance of beta
sigma2 = np.sum((y_pts - A @ beta)**2) / (len(y_pts) - 2)
cov_beta = sigma2 * np.linalg.inv(R.T @ R)
print(f"Std err of slope: {np.sqrt(cov_beta[1, 1]):.4f}")
```

### Example 4: Orthogonal Matrices Preserve Length

```python
# Rotation matrix (orthogonal)
theta = np.radians(45)
Q = np.array([[np.cos(theta), -np.sin(theta)],
              [np.sin(theta),  np.cos(theta)]])

v = np.array([3, 4])
v_rotated = Q @ v

print(f"||v|| = {np.linalg.norm(v):.4f}")
print(f"||Qv|| = {np.linalg.norm(v_rotated):.4f}")
print(f"Length preserved? {np.isclose(np.linalg.norm(v), np.linalg.norm(v_rotated))}")

# Dot product preserved
u = np.array([1, 2])
print(f"u·v = {u @ v:.4f}")
print(f"Qu·Qv = {(Q @ u) @ (Q @ v):.4f}")
```

### Example 5: Projection onto a Subspace

```python
# Project onto the subspace spanned by two vectors in R^4
A = np.array([[1, 0],
              [0, 1],
              [1, 1],
              [0, 0]])

v = np.array([2, 3, 4, 5])

# Projection matrix
P = A @ np.linalg.inv(A.T @ A) @ A.T
proj = P @ v

print(f"Original: {v}")
print(f"Projected: {np.round(proj, 4)}")
print(f"Residual: {np.round(v - proj, 4)}")

# Verify residual is orthogonal to all columns of A
for i in range(A.shape[1]):
    print(f"residual ⟂ col{i}? {np.isclose((v - proj) @ A[:, i], 0)}")
```

## Glossary

| Term | Definition |
|------|------------|
| Dot product | $\mathbf{u} \cdot \mathbf{v} = \sum u_i v_i$, the standard inner product |
| Norm | $\|\mathbf{v}\| = \sqrt{\mathbf{v} \cdot \mathbf{v}}$, the length of a vector |
| Orthogonal | Two vectors are orthogonal if their dot product is zero |
| Orthonormal | A set of mutually orthogonal unit vectors |
| Orthogonal complement | $W^\perp$, the set of vectors orthogonal to every vector in $W$ |
| Orthogonal projection | The closest point in a subspace to a given vector |
| Projection matrix | $P = A(A^T A)^{-1} A^T$, maps vectors onto $\text{Col}(A)$ |
| Idempotent | $P^2 = P$, a property of projection matrices |
| Gram-Schmidt | Process to produce an orthonormal basis from a set of vectors |
| QR decomposition | $A = QR$ with $Q$ orthonormal columns and $R$ upper triangular |
| Least squares | Minimizing $\|A\mathbf{x} - \mathbf{b}\|^2$ for overdetermined systems |
| Normal equations | $A^T A \mathbf{x} = A^T \mathbf{b}$, the first-order condition for least squares |
| Residual | $\mathbf{r} = \mathbf{b} - A\mathbf{x}$, the error vector |
| Cauchy-Schwarz | $|\mathbf{u} \cdot \mathbf{v}| \leq \|\mathbf{u}\| \|\mathbf{v}\|$ |
| $R^2$ | Coefficient of determination; proportion of variance explained |
| Design matrix | The matrix $A$ (or $X$) in a regression problem |
| Householder reflection | A stable orthogonal transformation used in QR algorithms |

## Quick References

- [NumPy linalg.qr Documentation](https://numpy.org/doc/stable/reference/generated/numpy.linalg.qr.html) — QR decomposition reference
- [NumPy linalg.lstsq Documentation](https://numpy.org/doc/stable/reference/generated/numpy.linalg.lstsq.html) — least squares solver
- [MIT 18.06: Orthogonality (Strang)](https://ocw.mit.edu/courses/mathematics/18-06-linear-algebra-spring-2010/video-lectures/lecture-14-orthogonal-vectors-and-subspaces/) — lecture on orthogonal subspaces
- [MIT 18.06: Least Squares (Strang)](https://ocw.mit.edu/courses/mathematics/18-06-linear-algebra-spring-2010/video-lectures/lecture-16-projection-matrices-and-least-squares/) — lecture on projections and least squares
- [Stanford CS229: Linear Regression](https://cs229.stanford.edu/notes/cs229-notes1.pdf) — machine learning application of least squares

## Next Steps

- [Singular Value Decomposition](singular-value-decomposition.md) — the ultimate decomposition revealing orthogonal structure in any matrix
- [Eigenvalues & Eigenvectors](eigenvalues-and-eigenvectors.md) — spectral analysis connects to orthogonal decomposition via the spectral theorem
- [Systems of Linear Equations](systems-of-linear-equations.md) — least squares as the generalization of solving $A\mathbf{x} = \mathbf{b}$
