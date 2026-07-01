# Matrices & Matrix Operations

## Description

Matrices are the workhorses of linear algebra — rectangular arrays of numbers that represent linear transformations, datasets, and systems of equations. Matrix operations (multiplication, inversion, decomposition) form the foundation of graphics pipelines, neural network layers, optimization algorithms, and scientific computing. This document covers matrix types, arithmetic, and key properties every developer needs.

## Prerequisites

- [Vectors & Vector Spaces](vectors-and-vector-spaces.md) — familiarity with vectors, linear combinations, and dot products
- Basic Python programming with NumPy

## Table of Contents

- [What Is a Matrix?](#what-is-a-matrix)
- [Matrix Types](#matrix-types)
- [Matrix Addition and Scalar Multiplication](#matrix-addition-and-scalar-multiplication)
- [Matrix Multiplication](#matrix-multiplication)
- [Transpose](#transpose)
- [Matrix Inverse](#matrix-inverse)
- [Determinant](#determinant)
- [Trace](#trace)
- [Rank](#rank)
- [Matrix Norms](#matrix-norms)
- [Special Matrices](#special-matrices)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is a Matrix?

A **matrix** is a rectangular array of numbers arranged in $m$ rows and $n$ columns, written as $A \in \mathbb{R}^{m \times n}$:

$$
A = \begin{bmatrix}
a_{11} & a_{12} & \dots & a_{1n} \\
a_{21} & a_{22} & \dots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{m1} & a_{m2} & \dots & a_{mn}
\end{bmatrix}
$$

The entry at row $i$, column $j$ is denoted $A_{ij}$ or $a_{ij}$. In many programming contexts, this is accessed as `A[i, j]` (0-indexed).

```python
import numpy as np

A = np.array([[1, 2, 3],
              [4, 5, 6]])
print(A.shape)  # (2, 3)
print(A[0, 1])  # 2 (row 0, column 1)
```

**Notation conventions:**
- Capital letters for matrices ($A$, $B$, $M$)
- Bold lowercase for vectors ($\mathbf{v}$, $\mathbf{w}$)
- $A_{ij}$ or $a_{ij}$ for individual entries
- $A_{i:}$ for row $i$, $A_{:j}$ for column $j$

**Matrix as a linear map:** An $m \times n$ matrix defines a function $T: \mathbb{R}^n \to \mathbb{R}^m$ via $T(\mathbf{x}) = A\mathbf{x}$.

**Matrix as data:** An $m \times n$ matrix can represent $m$ data points each with $n$ features.

```python
# Matrix as a dataset: 100 samples, 5 features
dataset = np.random.randn(100, 5)
print(dataset.shape)  # (100, 5)
```

### Matrix Types

**Square matrix:** $m = n$, same number of rows and columns.

$$
A = \begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{bmatrix}
$$

```python
A = np.array([[1, 2, 3],
              [4, 5, 6],
              [7, 8, 9]])  # 3x3 square matrix
```

**Rectangular matrix:** $m \neq n$. Tall matrices ($m > n$) arise from overdetermined systems. Wide matrices ($m < n$) arise from underdetermined systems.

**Diagonal matrix:** All off-diagonal entries are zero: $D_{ij} = 0$ if $i \neq j$.

$$
D = \begin{bmatrix}
d_1 & 0 & 0 \\
0 & d_2 & 0 \\
0 & 0 & d_3
\end{bmatrix}
$$

```python
D = np.diag([2, 5, -1])
print(D)
# [[ 2  0  0]
#  [ 0  5  0]
#  [ 0  0 -1]]
```

**Identity matrix:** A diagonal matrix with 1s on the diagonal. Denoted $I_n$ for $n \times n$. Acts as the multiplicative identity: $AI = A$ and $IA = A$.

```python
I = np.eye(3)
print(I)
# [[1. 0. 0.]
#  [0. 1. 0.]
#  [0. 0. 1.]]
```

**Symmetric matrix:** $A = A^T$, meaning $A_{ij} = A_{ji}$. Symmetric matrices are fundamental in optimization (Hessians), statistics (covariance matrices), and graph theory (adjacency matrices).

```python
A = np.array([[1, 2, 3],
              [2, 4, 5],
              [3, 5, 6]])
print(np.allclose(A, A.T))  # True
```

**Upper triangular matrix:** All entries below the main diagonal are zero: $A_{ij} = 0$ for $i > j$.

$$
U = \begin{bmatrix}
1 & 2 & 3 \\
0 & 4 & 5 \\
0 & 0 & 6
\end{bmatrix}
$$

**Lower triangular matrix:** All entries above the main diagonal are zero: $A_{ij} = 0$ for $i < j$.

```python
U = np.triu(np.ones((3, 3)))
L = np.tril(np.ones((3, 3)))
print("Upper:\n", U)
print("Lower:\n", L)
```

**Sparse matrix:** Most entries are zero. Common in NLP (bag-of-words), graph analysis (adjacency), and scientific computing. Stored in compressed formats (CSR, CSC, COO) for efficiency.

```python
from scipy.sparse import csr_matrix

data = np.array([1, 2, 3, 4, 5, 6])
row = np.array([0, 0, 1, 2, 2, 2])
col = np.array([0, 2, 2, 0, 1, 2])
sparse = csr_matrix((data, (row, col)), shape=(3, 3))
print(sparse.toarray())
# [[1 0 2]
#  [0 0 3]
#  [4 5 6]]
```

### Matrix Addition and Scalar Multiplication

**Addition:** Two matrices of the same shape are added component-wise:

$$
(A + B)_{ij} = A_{ij} + B_{ij}
$$

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])
C = A + B  # [[6, 8], [10, 12]]
```

**Scalar multiplication:** Every entry is multiplied by the scalar:

$$
(cA)_{ij} = c \cdot A_{ij}
$$

```python
scaled = 2 * A  # [[2, 4], [6, 8]]
```

**Properties:** Addition is commutative and associative. Scalar multiplication distributes over addition.

### Matrix Multiplication

Matrix multiplication is the defining operation of linear algebra. The product $C = AB$ is defined only when $A$ is $m \times n$ and $B$ is $n \times p$. The result is $m \times p$:

$$
C_{ij} = \sum_{k=1}^{n} A_{ik} B_{kj}
$$

This is the **dot product** of row $i$ of $A$ with column $j$ of $B$.

```python
A = np.array([[1, 2],
              [3, 4]])  # 2x2
B = np.array([[5, 6, 7],
              [8, 9, 10]])  # 2x3

C = A @ B  # 2x3
# [[1*5+2*8, 1*6+2*9, 1*7+2*10],
#  [3*5+4*8, 3*6+4*9, 3*7+4*10]]
# = [[21, 24, 27],
#    [47, 54, 61]]
```

**Key properties of matrix multiplication:**

- **Not commutative:** $AB \neq BA$ in general (and one product may not even be defined)
- **Associative:** $(AB)C = A(BC)$
- **Distributive:** $A(B + C) = AB + AC$ and $(A + B)C = AC + BC$
- **No cancellation:** $AB = AC$ does not imply $B = C$ (even if $A \neq 0$)

```python
A = np.array([[1, 0], [0, 0]])
B = np.array([[0, 0], [1, 0]])
C = np.array([[0, 0], [0, 1]])
print(A @ B)  # [[0, 0], [0, 0]]
print(A @ C)  # [[0, 0], [0, 0]] -- AB = AC but B != C
```

**Block matrix multiplication:** Partition matrices into blocks and multiply block-wise:

$$
\begin{bmatrix} A_{11} & A_{12} \\ A_{21} & A_{22} \end{bmatrix}
\begin{bmatrix} B_{11} \\ B_{21} \end{bmatrix}
= \begin{bmatrix} A_{11}B_{11} + A_{12}B_{21} \\ A_{21}B_{11} + A_{22}B_{21} \end{bmatrix}
```

**Computational complexity:** Multiplying two $n \times n$ matrices by the definition is $O(n^3)$. Strassen's algorithm achieves $O(n^{2.807})$, and the best known algorithms approach $O(n^{2.373})$.

```python
# Naive triple loop (O(n^3))
def naive_multiply(A, B):
    m, n = A.shape
    n2, p = B.shape
    assert n == n2
    C = np.zeros((m, p))
    for i in range(m):
        for j in range(p):
            for k in range(n):
                C[i, j] += A[i, k] * B[k, j]
    return C
```

**Element-wise (Hadamard) product:** Also called the Schur product, denoted $A \odot B$:

$$
(A \odot B)_{ij} = A_{ij} B_{ij}
$$

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])
hadamard = A * B  # [[5, 12], [21, 32]] -- element-wise
```

### Transpose

The **transpose** of a matrix $A \in \mathbb{R}^{m \times n}$ is $A^T \in \mathbb{R}^{n \times m}$, formed by flipping rows and columns:

$$
(A^T)_{ij} = A_{ji}
$$

```python
A = np.array([[1, 2, 3],
              [4, 5, 6]])
print(A.T)
# [[1, 4],
#  [2, 5],
#  [3, 6]]
```

**Properties:**

- $(A^T)^T = A$
- $(A + B)^T = A^T + B^T$
- $(cA)^T = c A^T$
- $(AB)^T = B^T A^T$ (note the reversal)
- $(A^{-1})^T = (A^T)^{-1}$ if $A$ is invertible

**Conjugate transpose (Hermitian):** For complex matrices, $A^H = \overline{A^T}$.

### Matrix Inverse

For a square matrix $A \in \mathbb{R}^{n \times n}$, the **inverse** $A^{-1}$ satisfies:

$$
A A^{-1} = A^{-1} A = I_n
$$

Not all matrices have inverses. A matrix that has an inverse is **invertible** (or **non-singular**). Otherwise it is **singular**.

**Conditions for invertibility (equivalent statements):**

- $\det(A) \neq 0$
- $\text{rank}(A) = n$
- Columns of $A$ are linearly independent
- Null space of $A$ is $\{\mathbf{0}\}$
- $A\mathbf{x} = \mathbf{b}$ has a unique solution for every $\mathbf{b}$
- All eigenvalues are non-zero

```python
A = np.array([[1, 2],
              [3, 4]])
A_inv = np.linalg.inv(A)
print(A_inv)
# [[-2. ,  1. ],
#  [ 1.5, -0.5]]

# Verify: A @ A_inv should be identity
print(A @ A_inv)
# [[1., 0.],
#  [0., 1.]]

# Singular matrix (determinant = 0)
singular = np.array([[1, 2],
                     [2, 4]])
try:
    np.linalg.inv(singular)
except np.linalg.LinAlgError:
    print("Matrix is singular -- cannot invert")
```

**Properties:**

- $(A^{-1})^{-1} = A$
- $(AB)^{-1} = B^{-1} A^{-1}$
- $(A^T)^{-1} = (A^{-1})^T$
- $I^{-1} = I$
- $(cA)^{-1} = \frac{1}{c} A^{-1}$ for $c \neq 0$

**Computational considerations:** You almost never need to compute $A^{-1}$ explicitly. Instead, solve $A\mathbf{x} = \mathbf{b}$ using `np.linalg.solve`, which is faster and numerically more stable.

```python
# Don't do this:
x = np.linalg.inv(A) @ b

# Do this:
x = np.linalg.solve(A, b)
```

**Pseudoinverse:** For non-square or singular matrices, the **Moore-Penrose pseudoinverse** $A^+$ generalizes the inverse:

```python
A = np.array([[1, 2, 3],
              [4, 5, 6]])
A_pinv = np.linalg.pinv(A)
# A_pinv is 3x2
print(A_pinv)
```

### Determinant

The **determinant** $\det(A)$ (or $|A|$) is a scalar that captures the volume scaling factor of the linear transformation represented by $A$. For $2 \times 2$:

$$
\det\begin{pmatrix} a & b \\ c & d \end{pmatrix} = ad - bc
$$

For $3 \times 3$:

$$
\det\begin{pmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{pmatrix} = a(ei - fh) - b(di - fg) + c(dh - eg)
$$

```python
A = np.array([[1, 2],
              [3, 4]])
det = np.linalg.det(A)  # -2.0
```

**Geometric meaning:** $|\det(A)|$ is the factor by which $A$ scales volumes. If $\det(A) = 2$, the transformation doubles volumes. If $\det(A) = 0$, the transformation collapses space into a lower dimension (hence singular).

**Properties:**

- $\det(I) = 1$
- $\det(A^T) = \det(A)$
- $\det(AB) = \det(A)\det(B)$
- $\det(cA) = c^n \det(A)$ for $n \times n$ matrix
- $\det(A^{-1}) = 1 / \det(A)$
- $\det(A) = 0$ implies $A$ is singular
- $\det(A) = \prod_i \lambda_i$ where $\lambda_i$ are eigenvalues

### Trace

The **trace** of a square matrix is the sum of diagonal entries:

$$
\text{tr}(A) = \sum_{i=1}^{n} A_{ii}
$$

```python
A = np.array([[1, 2, 3],
              [4, 5, 6],
              [7, 8, 9]])
trace = np.trace(A)  # 1 + 5 + 9 = 15
```

**Properties:**

- $\text{tr}(A + B) = \text{tr}(A) + \text{tr}(B)$
- $\text{tr}(cA) = c \cdot \text{tr}(A)$
- $\text{tr}(A^T) = \text{tr}(A)$
- $\text{tr}(AB) = \text{tr}(BA)$ (cyclic property)
- $\text{tr}(ABC) = \text{tr}(BCA) = \text{tr}(CAB)$
- $\text{tr}(A) = \sum_i \lambda_i$ (sum of eigenvalues)

The trace appears in:

- **Frobenius norm:** $\|A\|_F = \sqrt{\text{tr}(A^T A)}$
- **Loss functions:** Matrix factorization with trace norm regularization
- **Statistical mechanics:** Partition functions

### Rank

The **rank** of a matrix $A$ is the dimension of its column space (or row space). It represents the number of linearly independent rows or columns.

```python
A = np.array([[1, 2, 3],
              [2, 4, 6],
              [3, 6, 9]])  # rows are multiples
rank = np.linalg.matrix_rank(A)
print(rank)  # 1

B = np.array([[1, 0, 0],
              [0, 1, 0],
              [0, 0, 1]])
print(np.linalg.matrix_rank(B))  # 3
```

**Rank properties:**

- $\text{rank}(A) \leq \min(m, n)$ for $m \times n$ matrix
- $\text{rank}(A) = \text{rank}(A^T) = \text{rank}(A^T A)$
- $\text{rank}(AB) \leq \min(\text{rank}(A), \text{rank}(B))$
- $\text{rank}(A + B) \leq \text{rank}(A) + \text{rank}(B)$
- A matrix is **full rank** if $\text{rank}(A) = \min(m, n)$

**Full rank vs. rank-deficient:**

- **Full column rank:** $\text{rank}(A) = n$. Columns are independent. $A^T A$ is invertible.
- **Full row rank:** $\text{rank}(A) = m$. Rows are independent. $A A^T$ is invertible.
- **Rank-deficient:** $\text{rank}(A) < \min(m, n)$. There is linear dependence among columns or rows.

```python
def describe_rank(A):
    m, n = A.shape
    r = np.linalg.matrix_rank(A)
    print(f"A is {m}x{n}, rank = {r}")
    if r == min(m, n):
        print("Full rank")
    else:
        print(f"Rank-deficient (nullity = {n-r})")
    return r

A = np.array([[1, 0],
              [0, 1],
              [1, 1]])
describe_rank(A)  # 3x2, rank=2, full column rank

B = np.array([[1, 2],
              [2, 4],
              [3, 6]])
describe_rank(B)  # 3x2, rank=1, rank-deficient
```

### Matrix Norms

A **matrix norm** measures the size or magnitude of a matrix. Different norms capture different aspects.

**Frobenius norm** (element-wise):

$$
\|A\|_F = \sqrt{\sum_{i=1}^{m} \sum_{j=1}^{n} A_{ij}^2} = \sqrt{\text{tr}(A^T A)}
$$

```python
A = np.array([[1, 2], [3, 4]])
frob = np.linalg.norm(A, 'fro')
# sqrt(1^2 + 2^2 + 3^2 + 4^2) = sqrt(30) ≈ 5.477
```

**Spectral norm** (induced $L_2$ norm):

$$
\|A\|_2 = \max_{\|\mathbf{x}\| = 1} \|A\mathbf{x}\| = \sigma_{\max}(A)
$$

where $\sigma_{\max}$ is the largest singular value.

```python
spec = np.linalg.norm(A, 2)  # spectral norm
```

**Nuclear norm** (trace norm):

$$
\|A\|_* = \sum_i \sigma_i(A)
$$

sum of singular values. Used in matrix completion (recommendation systems).

```python
U, s, Vt = np.linalg.svd(A)
nuclear = np.sum(s)
```

**Common norms comparison:**

| Norm | Definition | Use case |
|------|------------|----------|
| Frobenius | $\sqrt{\sum A_{ij}^2}$ | General-purpose, easy to compute |
| Spectral | $\sigma_{\max}(A)$ | Sensitivity analysis, condition number |
| Nuclear | $\sum \sigma_i(A)$ | Low-rank optimization, regularization |
| Max | $\max \|A_{ij}\|$ | Element-wise bounds |

**Condition number:** $\kappa(A) = \|A\| \cdot \|A^{-1}\|$ measures how sensitive a linear system is to input errors.

```python
A = np.array([[1, 2],
              [2, 4.0001]])
cond = np.linalg.cond(A)
print(f"Condition number: {cond:.2f}")
# High condition number => ill-conditioned (near-singular)
```

A high condition number means small changes in $\mathbf{b}$ cause large changes in the solution $\mathbf{x}$.

### Special Matrices

**Orthogonal matrix:** $Q^T Q = Q Q^T = I$, meaning $Q^{-1} = Q^T$. Columns are orthonormal (unit length and mutually orthogonal). Orthogonal matrices preserve lengths and angles (rotations/reflections).

```python
# Rotation matrix (orthogonal)
theta = np.radians(45)
Q = np.array([[np.cos(theta), -np.sin(theta)],
              [np.sin(theta),  np.cos(theta)]])
print(Q.T @ Q)  # identity (within rounding)

# Verify: Q^-1 = Q^T
print(np.allclose(np.linalg.inv(Q), Q.T))  # True
```

**Positive definite matrix:** A symmetric matrix $A$ such that $\mathbf{x}^T A \mathbf{x} > 0$ for all non-zero $\mathbf{x}$. All eigenvalues are positive. Governs convex quadratic forms and appears in covariance matrices, Hessians, and energy functions.

```python
# Positive definite: all eigenvalues > 0
A = np.array([[2, -1],
              [-1, 2]])
evals = np.linalg.eigvalsh(A)  # [1, 3]
print(evals)  # both positive => positive definite

# Test positive definiteness
try:
    L = np.linalg.cholesky(A)
    print("Positive definite")
except np.linalg.LinAlgError:
    print("Not positive definite")
```

**Matrix exponentials:** Defined as $e^A = \sum_{k=0}^{\infty} \frac{A^k}{k!}$. Used in differential equations, control theory, and Lie groups.

```python
from scipy.linalg import expm

A = np.array([[0, -1],
              [1, 0]])
expA = expm(A)  # rotation matrix for angle 1 radian
print(expA)
```

## Glossary

| Term | Definition |
|------|------------|
| Matrix | A rectangular array of numbers with $m$ rows and $n$ columns |
| Square matrix | A matrix with equal number of rows and columns |
| Diagonal matrix | A square matrix with zeros off the main diagonal |
| Identity matrix | A diagonal matrix with 1s on the diagonal; the multiplicative identity |
| Transpose | The matrix formed by swapping rows and columns; $A^T$ |
| Symmetric matrix | A matrix equal to its transpose: $A = A^T$ |
| Inverse | $A^{-1}$ such that $AA^{-1} = A^{-1}A = I$ |
| Singular matrix | A non-invertible square matrix (determinant zero) |
| Determinant | A scalar measuring volume scaling; $\det(A)$ |
| Trace | The sum of diagonal entries; $\text{tr}(A)$ |
| Rank | The dimension of the column (or row) space |
| Full rank | Rank equal to the smaller dimension |
| Orthogonal matrix | $Q^T Q = QQ^T = I$; columns are orthonormal |
| Positive definite | $\mathbf{x}^T A \mathbf{x} > 0$ for all $\mathbf{x} \neq \mathbf{0}$ |
| Frobenius norm | The element-wise $L_2$ norm: $\sqrt{\sum A_{ij}^2}$ |
| Condition number | $\kappa(A) = \|A\|\|A^{-1}\|$; measures sensitivity to errors |
| Hadamard product | Element-wise multiplication: $(A \odot B)_{ij} = A_{ij}B_{ij}$ |
| Pseudoinverse | $A^+$; generalizes inverse to non-square/singular matrices |
| Sparse matrix | A matrix with mostly zero entries |
| Block matrix | A matrix partitioned into sub-matrices |

## Quick References

- [NumPy Linear Algebra Guide](https://numpy.org/doc/stable/reference/routines.linalg.html) — complete reference for NumPy matrix operations
- [Matrix Calculus for Machine Learning](https://arxiv.org/abs/1802.01528) — derivatives of matrix expressions
- [MIT 18.06: Linear Algebra (Strang)](https://ocw.mit.edu/courses/mathematics/18-06-linear-algebra-spring-2010/) — lectures 1-6 cover matrix fundamentals
- [The Matrix Cookbook](https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf) — exhaustive reference of matrix identities
- [Wolfram Alpha Matrix Calculator](https://www.wolframalpha.com/examples/mathematics/algebra/matrices) — interactive matrix computation

## Next Steps

- [Systems of Linear Equations](systems-of-linear-equations.md) — applying matrices to solve $A\mathbf{x} = \mathbf{b}$
- [Eigenvalues & Eigenvectors](eigenvalues-and-eigenvectors.md) — spectral properties of matrices
- [Linear Transformations](linear-transformations.md) — matrices as maps between vector spaces
- [Singular Value Decomposition](singular-value-decomposition.md) — the fundamental matrix factorization
