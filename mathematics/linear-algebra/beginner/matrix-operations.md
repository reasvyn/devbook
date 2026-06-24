# Matrix Operations

## Description

Matrices are the workhorses of computation — they transform data, solve systems of equations, and power graphics and machine learning pipelines. This document covers the core operations every developer should know: transpose, determinant, inverse, and solving linear systems.

## Prerequisites

- [Vectors & Matrices](vectors-and-matrices.md)

## Table of Contents

- [Transpose](#transpose)
- [Determinant](#determinant)
- [Inverse](#inverse)
- [Solving Linear Systems](#solving-linear-systems)
- [Matrix Multiplication Review](#matrix-multiplication-review)

## Content / Material

### Transpose

The transpose flips rows and columns:

$$ A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \\ 5 & 6 \end{bmatrix}, \quad A^T = \begin{bmatrix} 1 & 3 & 5 \\ 2 & 4 & 6 \end{bmatrix} $$

```python
import numpy as np
A = np.array([[1, 2], [3, 4], [5, 6]])
print(A.T)  # [[1 3 5]
            #  [2 4 6]]
```

### Determinant

The determinant ($\det(A)$ or $|A|$) tells you how a transformation scales area/volume. A zero determinant means the matrix squishes space into a lower dimension (not invertible).

For a $2 \times 2$ matrix:

$$ \det\begin{pmatrix} a & b \\ c & d \end{pmatrix} = ad - bc $$

```python
A = np.array([[1, 2], [3, 4]])
det = np.linalg.det(A)  # -2.0
```

### Inverse

The inverse $A^{-1}$ undoes the transformation of $A$: $A \times A^{-1} = I$. Only square matrices with non-zero determinant have inverses.

```python
A = np.array([[1, 2], [3, 4]])
A_inv = np.linalg.inv(A)
print(A @ A_inv)  # ≈ [[1, 0], [0, 1]] (identity)
```

Geometrically: if $A$ rotates 90°, $A^{-1}$ rotates -90°.

### Solving Linear Systems

A system of equations $Ax = b$ can be solved with the inverse:

$$ x = A^{-1}b $$

```python
A = np.array([[2, 1], [1, 3]])
b = np.array([5, 6])
x = np.linalg.solve(A, b)  # [1.8, 1.4]
# Check: 2(1.8) + 1(1.4) = 5 ✓
```

**Never compute $A^{-1}$ explicitly** to solve systems — use `linalg.solve()` (faster and numerically stable).

### Matrix Multiplication Review

$$ (AB)_{ij} = \sum_k A_{ik} B_{kj} $$

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])
C = A @ B  # [[19, 22], [43, 50]]
```

- Not commutative: $AB \neq BA$ in general
- Associative: $A(BC) = (AB)C$
- Distributive: $A(B + C) = AB + AC$

## Glossary

| Term | Definition |
|------|------------|
| Transpose | Flipping a matrix over its diagonal |
| Determinant | A scalar measuring how a transformation scales area |
| Inverse | A matrix that undoes another matrix's transformation |
| Linear system | A set of equations of the form $Ax = b$ |

## Next Steps

- [Eigendecomposition](../intermediate/eigendecomposition.md) — eigenvalues and eigenvectors
