# Vectors and Matrices

## Description

Vectors and matrices are the building blocks of linear algebra. A vector is an ordered list of numbers; a matrix is a 2D grid. Together they form the language of data, transformations, and machine learning.

## Prerequisites

- [What Is Linear Algebra](../intro/what-is-linear-algebra.md)

## Table of Contents

- [Vectors](#vectors)
- [Matrices](#matrices)
- [Matrix Multiplication](#matrix-multiplication)
- [Identity and Inverse](#identity-and-inverse)

## Content / Material

### Vectors

A vector is an arrow in space with magnitude and direction, represented as a column of numbers:

```python
import numpy as np
v = np.array([3, 4])
# magnitude: sqrt(3² + 4²) = 5
```

Operations:
- **Addition**: component-wise: $[1, 2] + [3, 4] = [4, 6]$
- **Scalar multiplication**: $3 \cdot [1, 2] = [3, 6]$
- **Dot product**: $[1, 2] \cdot [3, 4] = 1\cdot3 + 2\cdot4 = 11$

### Matrices

A matrix is a rectangular grid of numbers:

$$ A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix} $$

```python
A = np.array([[1, 2], [3, 4]])
```

A matrix transforms vectors. Every linear transformation (rotation, scaling, shearing) can be represented as a matrix.

### Matrix Multiplication

$C = A \times B$ where $C_{ij}$ is the dot product of row $i$ of $A$ and column $j$ of $B$:

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])
C = A @ B  # matrix multiplication
```

**Important**: matrix multiplication is not commutative ($AB \neq BA$).

### Identity and Inverse

- **Identity matrix** ($I$): ones on diagonal, zeros elsewhere. Multiplying by $I$ leaves a matrix unchanged.
- **Inverse** ($A^{-1}$): a matrix such that $A \times A^{-1} = I$. Not all matrices have inverses.

```python
A = np.array([[1, 2], [3, 4]])
inv_A = np.linalg.inv(A)
```

## Glossary

| Term | Definition |
|------|------------|
| Vector | An ordered list of numbers |
| Matrix | A 2D grid of numbers |
| Dot product | The sum of element-wise products of two vectors |
| Identity matrix | A square matrix with 1s on the diagonal and 0s elsewhere |

## Next Steps

- [Eigendecomposition](../intermediate/eigendecomposition.md) — eigenvalues and eigenvectors
