# Eigenvalues & Eigenvectors

## Description

Eigenvalues and eigenvectors reveal the fundamental structure of a linear transformation — the directions that are only stretched or compressed, never rotated. They are the key to diagonalization, stability analysis, principal component analysis (PCA), PageRank, Markov chains, vibration analysis, and the spectral theorem. Every developer working with data, networks, or dynamical systems needs a firm grasp of eigen-analysis.

## Prerequisites

- [Matrices & Matrix Operations](matrices-and-operations.md) — matrix multiplication, determinant, inverse, transpose
- [Linear Transformations](linear-transformations.md) — transformation matrices, kernel, image, change of basis

## Table of Contents

- [Definition](#definition)
- [The Characteristic Equation](#the-characteristic-equation)
- [Computing Eigenvalues and Eigenvectors](#computing-eigenvalues-and-eigenvectors)
- [Algebraic and Geometric Multiplicity](#algebraic-and-geometric-multiplicity)
- [Diagonalization](#diagonalization)
- [Spectral Theorem for Symmetric Matrices](#spectral-theorem-for-symmetric-matrices)
- [Applications](#applications)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Definition

For a square matrix $A \in \mathbb{R}^{n \times n}$, a scalar $\lambda$ is an **eigenvalue** and a non-zero vector $\mathbf{v}$ is an **eigenvector** if:

$$
A\mathbf{v} = \lambda \mathbf{v}
$$

In words: applying $A$ to $\mathbf{v}$ simply scales $\mathbf{v}$ by $\lambda$. The eigenvector's direction is preserved.

```python
import numpy as np

A = np.array([[2, 0],
              [0, 3]])
v = np.array([1, 0])

# A * [1, 0]^T = 2 * [1, 0]^T
result = A @ v
print(f"A * v = {result}")
print(f"lambda * v = {2 * v}")
print(f"Is eigenvector? {np.allclose(result, 2 * v)}")  # True
```

**Geometric meaning:** If $A$ represents a transformation, eigenvectors are the special directions that are only stretched (positive $\lambda$), compressed ($0 < \lambda < 1$), reversed ($\lambda < 0$), or zeroed out ($\lambda = 0$).

**Why eigenvectors matter:**

- They reveal the axes along which a transformation acts independently
- The set of all eigenvectors forms a basis in which $A$ becomes diagonal
- In data, eigenvectors of the covariance matrix are the principal components
- Stability of dynamical systems depends on eigenvalue magnitudes

### The Characteristic Equation

The equation $A\mathbf{v} = \lambda \mathbf{v}$ can be rewritten:

$$
(A - \lambda I) \mathbf{v} = \mathbf{0}
$$

For a non-trivial $\mathbf{v}$ to exist, the matrix $A - \lambda I$ must be singular:

$$
\det(A - \lambda I) = 0
$$

This is the **characteristic equation** — a polynomial of degree $n$ in $\lambda$.

```python
def characteristic_poly(A):
    """Compute coefficients of the characteristic polynomial det(A - λI)."""
    # Uses the fact that characteristic polynomial = poly(eigenvalues)
    evals = np.linalg.eigvals(A)
    return np.poly(evals)

A = np.array([[2, 1],
              [1, 2]])
coeffs = characteristic_poly(A)
print(f"Coefficients: {coeffs}")
# λ^2 - 4λ + 3 = 0 => λ = 3, λ = 1

# Verify with np.poly
print(f"From eigenvalues: {np.poly(np.linalg.eigvals(A))}")
```

**Example: $2 \times 2$ characteristic equation:**

$$
A = \begin{pmatrix} a & b \\ c & d \end{pmatrix}
$$

$$
\det(A - \lambda I) = \det\begin{pmatrix} a - \lambda & b \\ c & d - \lambda \end{pmatrix}
= (a - \lambda)(d - \lambda) - bc
= \lambda^2 - (a + d)\lambda + (ad - bc)
$$

So the characteristic equation is:

$$
\lambda^2 - \text{tr}(A) \lambda + \det(A) = 0
$$

**Properties:**

- The characteristic polynomial has degree $n$
- $\sum_{i=1}^n \lambda_i = \text{tr}(A)$ (sum of eigenvalues equals trace)
- $\prod_{i=1}^n \lambda_i = \det(A)$ (product of eigenvalues equals determinant)
- Real matrices can have complex eigenvalues (they appear in conjugate pairs)
- The coefficient of $\lambda^{n-1}$ is $-\text{tr}(A)$
- The constant term is $(-1)^n \det(A)$

```python
# Verify trace and determinant properties
A = np.random.randn(5, 5)
evals = np.linalg.eigvals(A)
print(f"Sum of eigenvalues: {np.sum(evals):.6f}")
print(f"Trace of A: {np.trace(A):.6f}")
print(f"Product of eigenvalues: {np.prod(evals):.6f}")
print(f"Det of A: {np.linalg.det(A):.6f}")
```

### Computing Eigenvalues and Eigenvectors

**By hand (for $2 \times 2$):**

```python
def eigen_2x2(A):
    """Compute eigenvalues and eigenvectors of a 2x2 matrix analytically."""
    a, b, c, d = A[0, 0], A[0, 1], A[1, 0], A[1, 1]
    trace = a + d
    det = a * d - b * c
    disc = trace**2 - 4 * det

    if disc < 0:
        print("Complex eigenvalues (not supported in this simple version)")
        return None, None

    sqrt_disc = np.sqrt(disc)
    lambda1 = (trace + sqrt_disc) / 2
    lambda2 = (trace - sqrt_disc) / 2

    # Eigenvectors (assuming c != 0)
    v1 = np.array([b, lambda1 - a])
    v2 = np.array([b, lambda2 - a])

    return np.array([lambda1, lambda2]), np.column_stack([v1, v2])

A = np.array([[2, 1],
              [1, 2]])
evals, evecs = eigen_2x2(A)
print(f"Eigenvalues: {evals}")
print(f"Eigenvectors:\n{evecs}")

# Verify
for i in range(2):
    print(f"A * v{i} = {A @ evecs[:, i]}")
    print(f"lambda{i} * v{i} = {evals[i] * evecs[:, i]}")
```

**Using NumPy (general case):**

```python
A = np.array([[2, 1],
              [1, 2]])

evals, evecs = np.linalg.eig(A)
print(f"Eigenvalues: {evals}")
print(f"Eigenvectors (columns):\n{evecs}")

# Verify: A @ v = λ * v for each eigenvector
for i in range(len(evals)):
    v = evecs[:, i]
    lhs = A @ v
    rhs = evals[i] * v
    print(f"λ{i}={evals[i]:.4f}: error={np.linalg.norm(lhs - rhs):.2e}")
```

**For symmetric matrices: use `eigh` (guarantees real eigenvalues, orthogonal eigenvectors):**

```python
A_sym = np.array([[2, -1, 0],
                  [-1, 2, -1],
                  [0, -1, 2]])

evals, evecs = np.linalg.eigh(A_sym)  # eigh for symmetric/Hermitian
print(f"Eigenvalues (sorted): {evals}")
print(f"Eigenvectors are orthogonal:\n{evecs.T @ evecs}")
# eigh returns eigenvalues in ascending order
```

**Spectral decomposition** (for diagonalizable matrices):

$$
A = \sum_{i=1}^{n} \lambda_i \mathbf{v}_i \mathbf{v}_i^T
$$

```python
def spectral_decomposition(A):
    """Decompose A into sum of λ_i * v_i * v_i^T."""
    evals, evecs = np.linalg.eigh(A)
    n = len(evals)
    components = []
    for i in range(n):
        component = evals[i] * np.outer(evecs[:, i], evecs[:, i])
        components.append(component)
    return components, evals, evecs

A = np.array([[3, 1], [1, 3]])
components, evals, evecs = spectral_decomposition(A)

# Reconstruct
A_reconstructed = sum(components)
print(f"Original:\n{A}")
print(f"Reconstructed:\n{A_reconstructed}")
print(f"Match: {np.allclose(A, A_reconstructed)}")
```

### Algebraic and Geometric Multiplicity

**Algebraic multiplicity:** The multiplicity of $\lambda$ as a root of the characteristic polynomial.

**Geometric multiplicity:** The dimension of the eigenspace for $\lambda$, i.e., $\dim(\ker(A - \lambda I))$.

Always: $1 \leq \text{geometric mult} \leq \text{algebraic mult}$.

A matrix is **defective** if geometric multiplicity < algebraic multiplicity for some eigenvalue. Defective matrices cannot be diagonalized.

```python
def analyze_eigenvalue(A, tol=1e-10):
    """Compute algebraic and geometric multiplicities for each eigenvalue."""
    evals = np.linalg.eigvals(A)
    unique_evals, counts = np.unique(np.round(evals, 10), return_counts=True)

    for i, lam in enumerate(unique_evals):
        alg_mult = counts[i]
        # Geometric multiplicity = dimension of null space of (A - λI)
        geo_mult = A.shape[0] - np.linalg.matrix_rank(A - lam * np.eye(A.shape[0]), tol=tol)
        defective = geo_mult < alg_mult
        print(f"λ = {lam:.4f}: algebraic={alg_mult}, geometric={geo_mult} {'[DEFECTIVE]' if defective else ''}")

A1 = np.array([[2, 0], [0, 2]])  # diagonalizable
A2 = np.array([[2, 1], [0, 2]])  # defective (Jordan block)
analyze_eigenvalue(A1)
analyze_eigenvalue(A2)
```

**Example of a defective matrix:**

$$
J = \begin{pmatrix} 2 & 1 \\ 0 & 2 \end{pmatrix}
$$

Characteristic polynomial: $(\lambda - 2)^2$, algebraic multiplicity 2.
Eigenspace: solve $(J - 2I)\mathbf{v} = \mathbf{0}$:

$$
\begin{pmatrix} 0 & 1 \\ 0 & 0 \end{pmatrix} \mathbf{v} = \mathbf{0}
$$

Only $\mathbf{v} = (1, 0)^T$ is an eigenvector — geometric multiplicity 1. The matrix is defective.

### Diagonalization

A matrix $A \in \mathbb{R}^{n \times n}$ is **diagonalizable** if there exists an invertible matrix $P$ and a diagonal matrix $D$ such that:

$$
A = P D P^{-1}
$$

The columns of $P$ are eigenvectors of $A$, and $D$ contains the corresponding eigenvalues on its diagonal.

```python
def diagonalize(A):
    """Diagonalize A: A = P @ D @ P^{-1}."""
    evals, evecs = np.linalg.eig(A)
    P = evecs
    D = np.diag(evals)

    # Verify: A = P D P^{-1}
    A_reconstructed = P @ D @ np.linalg.inv(P)
    success = np.allclose(A, A_reconstructed)

    return {
        "P": P,
        "D": D,
        "eigenvalues": evals,
        "success": success
    }

A = np.array([[5, 2],
              [2, 5]])
result = diagonalize(A)
print(f"Eigenvalues: {result['eigenvalues']}")
print(f"P (eigenvectors):\n{result['P']}")
print(f"D:\n{result['D']}")
print(f"A = PDP^-1: {result['success']}")
```

**When is a matrix diagonalizable?**

- $A$ has $n$ linearly independent eigenvectors
- All eigenvalues are distinct (sufficient but not necessary)
- $A$ is symmetric (always diagonalizable via orthogonal matrix)
- $A$ has no defective eigenvalues

**Power of diagonalization:**

If $A = PDP^{-1}$, then:

$$
A^k = P D^k P^{-1}
$$

This makes computing high powers cheap:

```python
A = np.array([[2, 1],
              [1, 2]])

# Direct computation of A^10
A_10_direct = np.linalg.matrix_power(A, 10)

# Via diagonalization
evals, evecs = np.linalg.eig(A)
A_10_diag = evecs @ np.diag(evals**10) @ np.linalg.inv(evecs)

print(f"Direct:\n{A_10_direct}")
print(f"Via diag:\n{A_10_diag}")
print(f"Match: {np.allclose(A_10_direct, A_10_diag)}")
```

**Matrix exponential:**

$$
e^A = P e^D P^{-1} = P \begin{bmatrix} e^{\lambda_1} & & \\ & \ddots & \\ & & e^{\lambda_n} \end{bmatrix} P^{-1}
$$

```python
from scipy.linalg import expm

# Matrix exponential via diagonalization
evals, evecs = np.linalg.eig(A)
expA_diag = evecs @ np.diag(np.exp(evals)) @ np.linalg.inv(evecs)
expA_direct = expm(A)

print(f"exp(A) via diag:\n{expA_diag}")
print(f"exp(A) direct:\n{expA_direct}")
print(f"Match: {np.allclose(expA_diag, expA_direct)}")
```

**Cayley-Hamilton theorem:** Every square matrix satisfies its own characteristic equation:

$$
p(A) = 0
$$

where $p$ is the characteristic polynomial.

```python
# Cayley-Hamilton for a 2x2: A^2 - tr(A)*A + det(A)*I = 0
A = np.array([[2, 1],
              [1, 2]])
trace = np.trace(A)
det = np.linalg.det(A)
result = A @ A - trace * A + det * np.eye(2)
print(f"C-H result:\n{np.round(result, 10)}")
# Should be zero matrix
```

### Spectral Theorem for Symmetric Matrices

The **spectral theorem** is one of the most important results in linear algebra:

> If $A$ is a real symmetric matrix ($A = A^T$), then $A$ is orthogonally diagonalizable. There exists an orthogonal matrix $Q$ ($Q^T Q = I$) and a diagonal matrix $D$ such that:
>
> $$
> A = Q D Q^T
> $$

**Consequences:**

- All eigenvalues are real
- Eigenvectors corresponding to distinct eigenvalues are orthogonal
- $A$ has a complete set of $n$ orthogonal eigenvectors
- $A$ can be decomposed as $A = \sum_{i=1}^n \lambda_i \mathbf{q}_i \mathbf{q}_i^T$

```python
# Symmetric matrix
A = np.array([[2, -1, 0],
              [-1, 3, -1],
              [0, -1, 2]])

# Orthogonal diagonalization (via eigh, which exploits symmetry)
evals, Q = np.linalg.eigh(A)
print(f"Eigenvalues: {evals}")
print(f"Q is orthogonal:\n{Q @ Q.T}")

# Decomposition: A = Q D Q^T
D = np.diag(evals)
A_reconstructed = Q @ D @ Q.T
print(f"Match: {np.allclose(A, A_reconstructed)}")

# Spectral decomposition
print("Spectral decomposition:")
for i in range(3):
    term = evals[i] * np.outer(Q[:, i], Q[:, i])
    print(f"  λ{i} * q{i} * q{i}^T:\n{np.round(term, 3)}\n")
```

**Positive definiteness from eigenvalues:**

- $A$ is positive definite iff all $\lambda_i > 0$
- $A$ is positive semidefinite iff all $\lambda_i \geq 0$
- $A$ is negative definite iff all $\lambda_i < 0$
- $A$ is indefinite iff it has both positive and negative eigenvalues

```python
def classify_matrix(A):
    """Classify a symmetric matrix by eigenvalue sign pattern."""
    evals = np.linalg.eigvalsh(A)
    print(f"Eigenvalues: {evals}")
    if np.all(evals > 0):
        return "Positive definite"
    elif np.all(evals >= 0):
        return "Positive semidefinite"
    elif np.all(evals < 0):
        return "Negative definite"
    elif np.all(evals <= 0):
        return "Negative semidefinite"
    else:
        return "Indefinite"

A1 = np.array([[2, 0], [0, 3]])
A2 = np.array([[1, 0], [0, 0]])
A3 = np.array([[-2, 0], [0, -3]])
A4 = np.array([[1, 0], [0, -1]])
for A in [A1, A2, A3, A4]:
    print(classify_matrix(A))
```

### Applications

**Principal Component Analysis (PCA):** PCA computes the eigenvectors of the covariance matrix to find directions of maximum variance.

```python
def pca(X, n_components):
    """Compute PCA via eigendecomposition of the covariance matrix."""
    X_centered = X - X.mean(axis=0)
    cov = (X_centered.T @ X_centered) / (X.shape[0] - 1)

    evals, evecs = np.linalg.eigh(cov)
    # Sort in descending order
    idx = np.argsort(evals)[::-1]
    evals = evals[idx]
    evecs = evecs[:, idx]

    components = evecs[:, :n_components]
    projected = X_centered @ components
    return projected, components, evals

# Generate 2D data with correlation
np.random.seed(42)
X = np.random.randn(100, 2)
X[:, 1] = 0.7 * X[:, 0] + 0.3 * X[:, 1]

projected, components, evals = pca(X, 1)
print(f"Explained variance ratio: {evals[0] / evals.sum():.3f}")
print(f"First principal component: {components[:, 0]}")
```

**PageRank:** Google's PageRank algorithm uses the eigenvector corresponding to eigenvalue 1 of the Google matrix.

```python
def pagerank(adjacency, damping=0.85, max_iter=100, tol=1e-10):
    """Compute PageRank via power iteration (finding dominant eigenvector)."""
    n = adjacency.shape[0]
    # Column-stochastic matrix
    out_degree = adjacency.sum(axis=0)
    P = adjacency / out_degree
    # Google matrix
    G = damping * P + (1 - damping) * np.ones((n, n)) / n

    # Power iteration for dominant eigenvector
    v = np.ones(n) / n
    for _ in range(max_iter):
        v_next = G @ v
        v_next = v_next / np.linalg.norm(v_next)
        if np.linalg.norm(v_next - v) < tol:
            break
        v = v_next

    return v / v.sum()

# Simple web graph
adj = np.array([[0, 1, 1],
                [1, 0, 0],
                [0, 1, 0]])
ranks = pagerank(adj)
print(f"PageRank scores: {ranks}")
```

**Markov chains:** The stationary distribution of a Markov chain is the eigenvector corresponding to $\lambda = 1$ of the transition matrix.

```python
# Weather Markov chain: [Sunny, Cloudy, Rainy]
P = np.array([[0.7, 0.3, 0.2],
              [0.2, 0.4, 0.3],
              [0.1, 0.3, 0.5]])

# Stationary distribution: π P = π, so π is left eigenvector of P for λ=1
evals, left_evecs = np.linalg.eig(P.T)
# Find index where eigenvalue ≈ 1
idx = np.argmin(np.abs(evals - 1.0))
pi = left_evecs[:, idx].real
pi = pi / pi.sum()
print(f"Stationary distribution: {np.round(pi, 4)}")

# Verify: π P = π
print(f"π P = {np.round(pi @ P, 4)}")
print(f"π   = {np.round(pi, 4)}")
```

**Vibration analysis:** Natural frequencies of mechanical systems are the square roots of eigenvalues of the stiffness matrix.

**Graph theory:** The eigenvalues of the graph Laplacian reveal connectivity (Fiedler value).

```python
# Graph Laplacian
adj = np.array([[0, 1, 0, 1],
                [1, 0, 1, 0],
                [0, 1, 0, 1],
                [1, 0, 1, 0]])
degrees = np.diag(adj.sum(axis=0))
L = degrees - adj

evals = np.linalg.eigvalsh(L)
print(f"Laplacian eigenvalues: {np.round(evals, 4)}")
# Number of zero eigenvalues = number of connected components
# Second smallest (Fiedler value) > 0 → graph is connected
print(f"Connected? {evals[1] > 1e-10}")
```

## Glossary

| Term | Definition |
|------|------------|
| Eigenvalue | A scalar $\lambda$ such that $A\mathbf{v} = \lambda \mathbf{v}$ for some non-zero $\mathbf{v}$ |
| Eigenvector | A non-zero vector $\mathbf{v}$ satisfying $A\mathbf{v} = \lambda \mathbf{v}$ |
| Eigenspace | The set of all eigenvectors for a given eigenvalue, plus $\mathbf{0}$ |
| Characteristic equation | $\det(A - \lambda I) = 0$, the polynomial whose roots are eigenvalues |
| Characteristic polynomial | $\det(A - \lambda I)$, an $n$th-degree polynomial |
| Algebraic multiplicity | The multiplicity of $\lambda$ as a root of the characteristic polynomial |
| Geometric multiplicity | The dimension of the eigenspace for $\lambda$ |
| Defective matrix | A matrix with geometric multiplicity < algebraic multiplicity for some eigenvalue |
| Diagonalizable | $A = PDP^{-1}$ where $D$ is diagonal and $P$ is invertible |
| Spectral theorem | A symmetric matrix is orthogonally diagonalizable with real eigenvalues |
| Spectral decomposition | $A = \sum \lambda_i \mathbf{v}_i \mathbf{v}_i^T$ |
| Spectrad | The set of all eigenvalues of a matrix |
| Power iteration | An iterative algorithm to find the dominant eigenvalue/eigenvector |
| Rayleigh quotient | $R(A, \mathbf{x}) = \frac{\mathbf{x}^T A \mathbf{x}}{\mathbf{x}^T \mathbf{x}}$; approximates an eigenvalue near $\mathbf{x}$ |
| Jordan block | A non-diagonalizable matrix block with repeated eigenvalues |
| Stationary distribution | The eigenvector of a Markov transition matrix corresponding to $\lambda = 1$ |

## Quick References

- [NumPy linalg.eig Documentation](https://numpy.org/doc/stable/reference/generated/numpy.linalg.eig.html) — standard eigendecomposition
- [NumPy linalg.eigh Documentation](https://numpy.org/doc/stable/reference/generated/numpy.linalg.eigh.html) — eigendecomposition for symmetric/Hermitian matrices
- [3Blue1Brown — Eigenvectors and Eigenvalues](https://www.youtube.com/watch?v=PFDu9oVAE-g) — geometric intuition video
- [MIT 18.06: Eigenvalues and Eigenvectors (Strang)](https://ocw.mit.edu/courses/mathematics/18-06-linear-algebra-spring-2010/video-lectures/lecture-21-eigenvalues-and-eigenvectors/) — lecture on the topic
- [Stanford CS229: PCA via Eigenvectors](https://cs229.stanford.edu/notes/cs229-notes10.pdf) — machine learning application notes

## Next Steps

- [Singular Value Decomposition](singular-value-decomposition.md) — generalizes eigendecomposition to non-square matrices
- [Orthogonality & Least Squares](orthogonality-and-least-squares.md) — orthogonal projections, the foundation for the spectral theorem
- [Systems of Linear Equations](systems-of-linear-equations.md) — solving $A\mathbf{x} = \mathbf{b}$, closely tied to eigenvalue analysis
