# Systems of Linear Equations

## Description

Systems of linear equations are the practical interface between abstract linear algebra and real-world computation. Nearly every optimization problem, physics simulation, and machine learning model involves solving $A\mathbf{x} = \mathbf{b}$ at some level. This document covers how to represent, solve, and analyze linear systems using Gaussian elimination, LU decomposition, and their NumPy implementations.

## Prerequisites

- [Matrices & Matrix Operations](matrices-and-operations.md) — matrix multiplication, transpose, inverse, determinant
- [Vectors & Vector Spaces](vectors-and-vector-spaces.md) — vector notation, dot product, subspaces

## Table of Contents

- [The Linear System Problem](#the-linear-system-problem)
- [Representation as Ax = b](#representation-as-ax--b)
- [Gaussian Elimination](#gaussian-elimination)
- [Row Echelon Form and Reduced Row Echelon Form](#row-echelon-form-and-reduced-row-echelon-form)
- [LU Decomposition](#lu-decomposition)
- [Solving with NumPy](#solving-with-numpy)
- [Existence and Uniqueness](#existence-and-uniqueness)
- [Homogeneous Systems](#homogeneous-systems)
- [Overdetermined and Underdetermined Systems](#overdetermined-and-underdetermined-systems)
- [Applications in Computing](#applications-in-computing)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Linear System Problem

A **system of linear equations** is a set of $m$ equations in $n$ unknowns:

$$
\begin{aligned}
a_{11}x_1 + a_{12}x_2 + \dots + a_{1n}x_n &= b_1 \\
a_{21}x_1 + a_{22}x_2 + \dots + a_{2n}x_n &= b_2 \\
\vdots \quad\quad\quad\quad\quad& \\
a_{m1}x_1 + a_{m2}x_2 + \dots + a_{mn}x_n &= b_m
\end{aligned}
$$

The goal is to find values for $x_1, \dots, x_n$ that satisfy all equations simultaneously.

```python
# Example: 2 equations, 2 unknowns
# 3x + 2y = 7
# x - y = -1
A = np.array([[3, 2],
              [1, -1]])
b = np.array([7, -1])

x = np.linalg.solve(A, b)
print(x)  # [1, 2] meaning x=1, y=2
```

### Representation as Ax = b

The compact matrix form:

$$
A\mathbf{x} = \mathbf{b}
$$

where $A \in \mathbb{R}^{m \times n}$ is the **coefficient matrix**, $\mathbf{x} \in \mathbb{R}^n$ is the unknown vector, and $\mathbf{b} \in \mathbb{R}^m$ is the right-hand side.

**Augmented matrix:** A convenient notation for manipulation:

$$
[A \mid \mathbf{b}] = \left[\begin{array}{cccc|c}
a_{11} & a_{12} & \dots & a_{1n} & b_1 \\
a_{21} & a_{22} & \dots & a_{2n} & b_2 \\
\vdots & \vdots & \ddots & \vdots & \vdots \\
a_{m1} & a_{m2} & \dots & a_{mn} & b_m
\end{array}\right]
$$

```python
def augmented(A, b):
    """Create augmented matrix [A | b]."""
    b_col = b.reshape(-1, 1)
    return np.hstack([A, b_col])

A = np.array([[1, 2], [3, 4]])
b = np.array([5, 6])
print(augmented(A, b))
# [[1 2 5]
#  [3 4 6]]
```

**Geometric interpretation:** Each equation defines a hyperplane in $\mathbb{R}^n$. The solution set is the intersection of all $m$ hyperplanes.

- For $m = n$ (square), typically a single point (if $A$ is invertible)
- For $m < n$ (underdetermined), an infinite set (a line/plane through the solution space)
- For $m > n$ (overdetermined), usually no exact solution (but least squares gives an approximate one)

### Gaussian Elimination

**Gaussian elimination** is an algorithm to solve linear systems by transforming the augmented matrix into **row echelon form** using three elementary row operations:

1. **Swap** two rows
2. **Scale** a row by a non-zero constant
3. **Add** a multiple of one row to another

```python
def gaussian_elimination(A, b):
    """Solve Ax = b using Gaussian elimination (no pivoting for simplicity)."""
    n = len(A)
    aug = A.astype(float).copy()
    rhs = b.astype(float).copy()

    # Forward elimination
    for col in range(n - 1):
        for row in range(col + 1, n):
            factor = aug[row, col] / aug[col, col]
            aug[row, col:] -= factor * aug[col, col:]
            rhs[row] -= factor * rhs[col]

    # Back substitution
    x = np.zeros(n)
    for i in range(n - 1, -1, -1):
        x[i] = (rhs[i] - aug[i, i+1:] @ x[i+1:]) / aug[i, i]

    return x

A = np.array([[2, 1, -1],
              [-3, -1, 2],
              [-2, 1, 2]])
b = np.array([8, -11, -3])
print(gaussian_elimination(A, b))  # [2, 3, -1]
```

**The elimination process step-by-step:**

Starting with:

$$
\left[\begin{array}{ccc|c}
2 & 1 & -1 & 8 \\
-3 & -1 & 2 & -11 \\
-2 & 1 & 2 & -3
\end{array}\right]
$$

Step 1: Eliminate $x_1$ from rows 2 and 3 using row 1 as pivot.

$$
R_2 \leftarrow R_2 + \frac{3}{2}R_1,\quad
R_3 \leftarrow R_3 + R_1
$$

Step 2: Eliminate $x_2$ from row 3 using row 2 as pivot.

$$
R_3 \leftarrow R_3 - 3R_2
$$

Result in row echelon form, then back-substitute.

**Pivoting:** In practice, we use **partial pivoting** (swapping rows to place the largest absolute value in the pivot position) to avoid division by zero and reduce numerical errors.

```python
def gaussian_elimination_pivot(A, b):
    """Gaussian elimination with partial pivoting."""
    n = len(A)
    aug = A.astype(float).copy()
    rhs = b.astype(float).copy()

    for col in range(n - 1):
        # Partial pivoting: find row with max |value| in current column
        max_row = np.argmax(np.abs(aug[col:, col])) + col
        if max_row != col:
            aug[[col, max_row]] = aug[[max_row, col]]
            rhs[[col, max_row]] = rhs[[max_row, col]]

        for row in range(col + 1, n):
            factor = aug[row, col] / aug[col, col]
            aug[row, col:] -= factor * aug[col, col:]
            rhs[row] -= factor * rhs[col]

    x = np.zeros(n)
    for i in range(n - 1, -1, -1):
        x[i] = (rhs[i] - aug[i, i+1:] @ x[i+1:]) / aug[i, i]
    return x
```

**Computational cost:** Gaussian elimination requires $\frac{2}{3}n^3$ floating-point operations for an $n \times n$ system — $O(n^3)$.

### Row Echelon Form and Reduced Row Echelon Form

**Row echelon form (REF)** of a matrix satisfies:

1. All zero rows are at the bottom
2. The leading entry (pivot) of each non-zero row is to the right of the row above
3. All entries below a pivot are zero

**Reduced row echelon form (RREF)** adds:

4. Every pivot is 1
5. All entries above each pivot are also zero

```python
def rref(A):
    """Compute the reduced row echelon form of A using Gauss-Jordan."""
    M = A.astype(float).copy()
    m, n = M.shape
    row = 0

    for col in range(n):
        if row >= m:
            break

        # Find pivot
        pivot = np.argmax(np.abs(M[row:, col])) + row
        if np.abs(M[pivot, col]) < 1e-10:
            continue

        # Swap
        M[[row, pivot]] = M[[pivot, row]]

        # Scale pivot row
        M[row] = M[row] / M[row, col]

        # Eliminate all other rows
        for r in range(m):
            if r != row:
                factor = M[r, col]
                M[r] -= factor * M[row]

        row += 1

    return M

A = np.array([[1, 2, 0, 4],
              [2, 4, 1, 9],
              [3, 6, 2, 14]])
print(rref(A))
# Each column reveals pivot variables and free variables
```

**Rank from RREF:** The number of non-zero rows in RREF equals the rank of $A$. Columns with pivots correspond to **basic variables**; columns without pivots correspond to **free variables**.

### LU Decomposition

**LU decomposition** factors a matrix $A$ into the product of a lower triangular matrix $L$ and an upper triangular matrix $U$:

$$
A = LU
$$

If $A$ is invertible, we can also include row permutations: $PA = LU$.

```python
import scipy.linalg as la

A = np.array([[2, 1, -1],
              [-3, -1, 2],
              [-2, 1, 2]])

P, L, U = la.lu(A)
print("L:\n", L)
print("U:\n", U)
print("P:\n", P)
print("PA == LU?", np.allclose(P @ A, L @ U))
```

**Solving with LU decomposition:** To solve $A\mathbf{x} = \mathbf{b}$:

1. Factor $A = LU$ (or $PA = LU$)
2. Solve $L\mathbf{y} = P\mathbf{b}$ for $\mathbf{y}$ (forward substitution)
3. Solve $U\mathbf{x} = \mathbf{y}$ for $\mathbf{x}$ (back substitution)

```python
def solve_lu(A, b):
    """Solve Ax = b using LU decomposition."""
    P, L, U = la.lu(A)
    n = len(A)

    # Forward substitution: Ly = Pb
    Pb = P @ b
    y = np.zeros(n)
    for i in range(n):
        y[i] = Pb[i] - L[i, :i] @ y[:i]

    # Back substitution: Ux = y
    x = np.zeros(n)
    for i in range(n - 1, -1, -1):
        x[i] = (y[i] - U[i, i+1:] @ x[i+1:]) / U[i, i]

    return x

print(solve_lu(A, b))  # [2, 3, -1]
```

**Why LU is better than Gaussian elimination for multiple right-hand sides:**

Once we have $A = LU$, solving $A\mathbf{x} = \mathbf{b}_k$ for many $\mathbf{b}_k$ is efficient because the $O(n^3)$ factorization is done once, and each solve is only $O(n^2)$.

```python
A = np.random.randn(100, 100)
P, L, U = la.lu(A)
B = np.random.randn(100, 1000)
X = np.zeros((100, 1000))
for k in range(1000):
    y = la.solve_triangular(L, P @ B[:, k], lower=True)
    X[:, k] = la.solve_triangular(U, y)
print(np.allclose(A @ X, B))
```

**Cholesky decomposition:** For symmetric positive definite matrices, the Cholesky decomposition $A = LL^T$ is twice as efficient as LU (half the operations, no pivoting needed).

```python
# Positive definite matrix
A = np.array([[4, 2, -2],
              [2, 10, 4],
              [-2, 4, 8]])
L = np.linalg.cholesky(A)
print("L:\n", L)
print("LL^T == A?", np.allclose(L @ L.T, A))
```

### Solving with NumPy

NumPy and SciPy provide high-performance, production-ready solvers.

| Solver | Function | Use case |
|--------|----------|----------|
| General square | `np.linalg.solve` | $A$ is $n \times n$, invertible |
| General rectangular | `np.linalg.lstsq` | Overdetermined/underdetermined |
| Sparse (direct) | `scipy.sparse.linalg.spsolve` | Large sparse $A$ |
| Iterative | `scipy.sparse.linalg.cg` | Large sparse, symmetric PD |
| Cholesky | `scipy.linalg.cho_solve` | Symmetric positive definite |

```python
import numpy as np
from numpy.linalg import solve, lstsq

# Square system
A = np.array([[3, 1, -1],
              [1, 4, 2],
              [-1, 2, 5]])
b = np.array([5, 3, 1])
x = solve(A, b)
print(f"Square solution: {x}")
print(f"Residual: {np.linalg.norm(A @ x - b):.2e}")

# Overdetermined system (10 equations, 3 unknowns)
A_over = np.random.randn(10, 3)
b_over = np.random.randn(10)
x_lstsq, residuals, rank, sv = lstsq(A_over, b_over, rcond=None)
print(f"Least squares solution: {x_lstsq}")
print(f"Residuals: {residuals}")

# Underdetermined system (3 equations, 10 unknowns)
A_under = np.random.randn(3, 10)
b_under = np.random.randn(3)
x_under = lstsq(A_under, b_under, rcond=None)[0]
print(f"Underdetermined solution (min norm): {x_under}")
```

**Performance note:** `np.linalg.solve` uses LAPACK's optimized LU solver and is faster and more stable than computing an explicit inverse. For multiple RHS vectors, factor once with LU ($O(n^3)$) then solve each ($O(n^2)$).

**Numerical stability:** Never compute $A^{-1}$ explicitly to solve a system. Use `np.linalg.solve` which uses LAPACK's highly optimized LU solver with partial pivoting.

### Existence and Uniqueness

For $A\mathbf{x} = \mathbf{b}$, the solution behavior depends on $A$ and $\mathbf{b}$:

**Square systems ($m = n$):**

| Condition | Solutions |
|-----------|-----------|
| $A$ invertible ($\det A \neq 0$) | Unique solution $\mathbf{x} = A^{-1}\mathbf{b}$ |
| $A$ singular, $\mathbf{b} \in \text{Col}(A)$ | Infinitely many (null space dimension > 0) |
| $A$ singular, $\mathbf{b} \notin \text{Col}(A)$ | No solution |

```python
def classify_system(A, b):
    m, n = A.shape
    rank = np.linalg.matrix_rank(A)
    aug = np.hstack([A, b.reshape(-1, 1)])
    aug_rank = np.linalg.matrix_rank(aug)

    print(f"Rank(A) = {rank}, Rank([A|b]) = {aug_rank}")

    if rank == aug_rank:
        if rank == n:
            print("Unique solution exists")
        else:
            nullity = n - rank
            print(f"Infinite solutions (nullity = {nullity})")
    else:
        print("No solution (inconsistent)")

A1 = np.array([[1, 0], [0, 1]])
b1 = np.array([3, 4])
classify_system(A1, b1)  # Unique

A2 = np.array([[1, 2], [2, 4]])
b2 = np.array([3, 6])
classify_system(A2, b2)  # Infinite

A3 = np.array([[1, 2], [2, 4]])
b3 = np.array([3, 7])
classify_system(A3, b3)  # None
```

**The fundamental theorem of invertibility:** For a square $n \times n$ matrix $A$, the following are equivalent:

- $A$ is invertible
- $\det(A) \neq 0$
- $\text{rank}(A) = n$
- $\text{nullity}(A) = 0$
- The columns of $A$ are linearly independent
- The rows of $A$ are linearly independent
- $A\mathbf{x} = \mathbf{b}$ has a unique solution for every $\mathbf{b}$
- $A\mathbf{x} = \mathbf{0}$ implies $\mathbf{x} = \mathbf{0}$
- $0$ is not an eigenvalue of $A$

### Homogeneous Systems

A **homogeneous system** has $\mathbf{b} = \mathbf{0}$: $A\mathbf{x} = \mathbf{0}$.

- Always has the **trivial solution** $\mathbf{x} = \mathbf{0}$
- Has non-trivial solutions iff $\mathbf{0}$ is not the only solution, i.e., $A$ is singular (rank-deficient)
- The solution set is the **null space** of $A$

```python
# Homogeneous system
A = np.array([[1, 2, 3],
              [2, 4, 6],
              [1, 1, 1]])
b = np.zeros(3)

# Find non-trivial solutions via SVD
U, s, Vt = np.linalg.svd(A)
null_space = Vt[s < 1e-10]  # rows are basis for null space
print("Null space basis:\n", null_space)
# Any x in span of null_space satisfies Ax = 0

# Verify
for v in null_space:
    print(f"A * v = {A @ v}")  # ~ zero
```

**Structure:** The general solution to $A\mathbf{x} = \mathbf{b}$ is:

$$
\mathbf{x} = \mathbf{x}_p + \mathbf{x}_h
$$

where $\mathbf{x}_p$ is any particular solution and $\mathbf{x}_h$ is any solution to the homogeneous system (in the null space).

```python
# Particular + homogeneous solution
A = np.array([[1, 2, -1],
              [2, 4, 0]])
b = np.array([5, 10])

# Particular solution (minimum norm)
x_p = np.linalg.lstsq(A, b, rcond=None)[0]

# Homogeneous (null space)
U, s, Vt = np.linalg.svd(A)
null_basis = Vt[s < 1e-10]

print(f"Particular: {x_p}")
print(f"Null basis:\n{null_basis}")

# Verify: any x = x_p + t * null_basis[0] is a solution
for t in [0, 1, -2]:
    x = x_p + t * null_basis[0]
    print(f"t={t}: residual = {np.linalg.norm(A @ x - b):.2e}")
```

### Overdetermined and Underdetermined Systems

**Overdetermined systems** ($m > n$): More equations than unknowns. Usually no exact solution. Solved via **least squares**: minimize $\|A\mathbf{x} - \mathbf{b}\|^2$.

**Underdetermined systems** ($m < n$): More unknowns than equations. Infinitely many solutions. The minimum-norm solution is often preferred.

```python
# Overdetermined: find x that minimizes ||Ax - b||^2
A = np.array([[1, 1],
              [1, 2],
              [1, 3],
              [1, 4]])
b = np.array([2.1, 3.9, 6.1, 7.8])

x_ls, residuals, rank, sv = np.linalg.lstsq(A, b, rcond=None)
print(f"Least squares: {x_ls}")  # [~0.9, ~1.9] approximates y = 0.9 + 1.9x

# Underdetermined: find minimum-norm solution
A_ud = np.array([[1, 1, 1],
                 [1, -1, 0]])
b_ud = np.array([5, 1])

x_min_norm = np.linalg.lstsq(A_ud, b_ud, rcond=None)[0]
print(f"Min norm solution: {x_min_norm}, norm = {np.linalg.norm(x_min_norm):.3f}")

# Compare with a direct particular solution
# Choose free variable x3 = 0
x_particular = np.array([3, 2, 0])  # satisfies x1+x2+x3=5, x1-x2=1
print(f"Particular: {x_particular}, norm = {np.linalg.norm(x_particular):.3f}")
```

### Applications in Computing

**Circuit analysis:** Kirchhoff's laws produce linear systems for node voltages.

```python
# Simple resistor network
# Node equations: G v = i where G is conductance matrix
G = np.array([[2, -1, 0],
              [-1, 3, -1],
              [0, -1, 2]])
i = np.array([1, 0, 0])
v = np.linalg.solve(G, i)
print(f"Node voltages: {v}")
```

**Linear regression:** Solving the normal equations $(X^T X) \beta = X^T \mathbf{y}$ is exactly the least squares solution to an overdetermined system.

```python
# Simple linear regression
X = np.array([[1, 1],
              [1, 2],
              [1, 3],
              [1, 4]])
y = np.array([2.1, 3.9, 6.1, 7.8])

# Normal equations
beta = np.linalg.solve(X.T @ X, X.T @ y)
print(f"beta = {beta}")  # [intercept, slope]

# Predictions
y_pred = X @ beta
print(f"Predictions: {y_pred}")
```

**Finite element methods:** Structural engineering, fluid dynamics, and heat transfer discretize PDEs into enormous sparse linear systems, solved with direct or iterative methods.

**PageRank:** Google's PageRank algorithm solves $(\alpha S + (1-\alpha)\mathbf{1}\mathbf{1}^T/n)\mathbf{x} = \mathbf{x}$, an eigenvalue problem that reduces to solving a sparse linear system.

## Glossary

| Term | Definition |
|------|------------|
| Linear system | A set of equations of the form $A\mathbf{x} = \mathbf{b}$ |
| Coefficient matrix | The matrix $A$ containing the equation coefficients |
| Augmented matrix | $[A \mid \mathbf{b}]$ combining the coefficient matrix and RHS |
| Gaussian elimination | Algorithm to reduce a matrix to row echelon form via row operations |
| Elementary row operation | Row swap, scaling, or adding a multiple of one row to another |
| Pivot | The leading non-zero entry in a row during elimination |
| Partial pivoting | Swapping rows to place the largest magnitude element in pivot position |
| Row echelon form | Upper triangular form with leading entries shifted rightward |
| Reduced row echelon form | REF with unit pivots and zeros above each pivot |
| LU decomposition | Factorization $A = LU$ where $L$ is lower and $U$ is upper triangular |
| Cholesky decomposition | $A = LL^T$ for symmetric positive definite matrices |
| Forward substitution | Solving $L\mathbf{y} = \mathbf{b}$ for lower triangular $L$ |
| Back substitution | Solving $U\mathbf{x} = \mathbf{y}$ for upper triangular $U$ |
| Homogeneous system | $A\mathbf{x} = \mathbf{0}$ |
| Particular solution | One specific solution to $A\mathbf{x} = \mathbf{b}$ |
| Null space | The set of all solutions to $A\mathbf{x} = \mathbf{0}$ |
| Inconsistent system | A system with no solution ($\mathbf{b} \notin \text{Col}(A)$) |
| Underdetermined | Fewer equations than unknowns ($m < n$) |
| Overdetermined | More equations than unknowns ($m > n$) |
| Condition number | $\kappa(A) = \|A\|\|A^{-1}\|$, measures sensitivity to errors |
| Ill-conditioned | A system with high condition number, sensitive to perturbations |

## Quick References

- [NumPy linalg.solve Documentation](https://numpy.org/doc/stable/reference/generated/numpy.linalg.solve.html) — reference for direct solvers
- [SciPy linalg.lu Documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.linalg.lu.html) — LU decomposition reference
- [MIT 18.06: Solving Ax = b (Strang)](https://ocw.mit.edu/courses/mathematics/18-06-linear-algebra-spring-2010/video-lectures/lecture-8-solving-ax-b-row-reduced-form-r/) — lecture on solving linear systems

## Next Steps

- [Linear Transformations](linear-transformations.md) — understanding what $A\mathbf{x}$ means geometrically
- [Eigenvalues & Eigenvectors](eigenvalues-and-eigenvectors.md) — spectral analysis of linear systems
- [Singular Value Decomposition](singular-value-decomposition.md) — the fundamental factorization for linear systems
