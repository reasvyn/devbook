# Vectors & Vector Spaces

## Description

Vectors are the fundamental building blocks of linear algebra — ordered lists of numbers that represent points in space, features in a dataset, or directions in a transformation. Understanding vectors, vector spaces, linear combinations, span, basis, and dimension gives you the language to describe data, geometry, and transformations across machine learning, computer graphics, and scientific computing.

## Prerequisites

- [Mathematical Thinking](../intro/mathematical-thinking.md) — comfort with sets, functions, and abstract reasoning
- Basic Python programming

## Table of Contents

- [What Is a Vector?](#what-is-a-vector)
- [Vector Operations](#vector-operations)
- [Vector Spaces](#vector-spaces)
- [Linear Combinations](#linear-combinations)
- [Span](#span)
- [Linear Independence](#linear-independence)
- [Basis and Dimension](#basis-and-dimension)
- [Coordinate Systems](#coordinate-systems)
- [Subspaces](#subspaces)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is a Vector?

A **vector** is an ordered tuple of numbers that belongs to a vector space. In computing, vectors are the natural representation for:

- A data point with $n$ features
- A position in 2D or 3D space
- A direction and magnitude
- Weights in a neural network layer

**Notation:** Vectors are written as bold lowercase letters or column matrices:

$$
\mathbf{v} = \begin{bmatrix} v_1 \\ v_2 \\ \vdots \\ v_n \end{bmatrix}
$$

In Python, we represent vectors as NumPy arrays:

```python
import numpy as np

v = np.array([3.0, -1.0, 2.0])
print(v.shape)  # (3,)
```

**Geometric interpretation:** A vector in $\mathbb{R}^2$ or $\mathbb{R}^3$ can be visualized as an arrow from the origin to a point. The **magnitude** (length) is the Euclidean distance from the origin:

$$
\|\mathbf{v}\| = \sqrt{\sum_{i=1}^{n} v_i^2}
$$

```python
v = np.array([3, 4])
magnitude = np.linalg.norm(v)  # 5.0
```

**Row vs. column vectors:** By convention, vectors in linear algebra are column vectors. NumPy 1D arrays are agnostic, but we can reshape:

```python
col_v = v.reshape(-1, 1)  # (3, 1) column vector
row_v = v.reshape(1, -1)  # (1, 3) row vector
```

### Vector Operations

**Addition:** Vectors of the same dimension are added component-wise:

$$
\mathbf{u} + \mathbf{v} = \begin{bmatrix} u_1 + v_1 \\ u_2 + v_2 \\ \vdots \\ u_n + v_n \end{bmatrix}
$$

```python
u = np.array([1, 2, 3])
v = np.array([4, 5, 6])
w = u + v  # [5, 7, 9]
```

Geometrically, vector addition corresponds to placing the tail of $\mathbf{v}$ at the head of $\mathbf{u}$ (the parallelogram law).

**Scalar multiplication:** Multiplying a vector by a scalar scales its magnitude and, if the scalar is negative, reverses its direction:

$$
c\mathbf{v} = \begin{bmatrix} cv_1 \\ cv_2 \\ \vdots \\ cv_n \end{bmatrix}
$$

```python
v = np.array([1, -2, 3])
scaled = 2.5 * v  # [2.5, -5.0, 7.5]
```

**Properties of vector addition and scalar multiplication:**

| Property | Addition | Scalar multiplication |
|----------|----------|----------------------|
| Commutativity | $\mathbf{u} + \mathbf{v} = \mathbf{v} + \mathbf{u}$ | — |
| Associativity | $(\mathbf{u} + \mathbf{v}) + \mathbf{w} = \mathbf{u} + (\mathbf{v} + \mathbf{w})$ | $c(d\mathbf{v}) = (cd)\mathbf{v}$ |
| Identity | $\mathbf{v} + \mathbf{0} = \mathbf{v}$ | $1\mathbf{v} = \mathbf{v}$ |
| Inverse | $\mathbf{v} + (-\mathbf{v}) = \mathbf{0}$ | — |
| Distributivity | $c(\mathbf{u} + \mathbf{v}) = c\mathbf{u} + c\mathbf{v}$ | $(c + d)\mathbf{v} = c\mathbf{v} + d\mathbf{v}$ |

**Dot product (inner product):** The dot product of two vectors produces a scalar:

$$
\mathbf{u} \cdot \mathbf{v} = \sum_{i=1}^{n} u_i v_i
$$

```python
u = np.array([1, 2, 3])
v = np.array([4, 5, 6])
dot = np.dot(u, v)  # 1*4 + 2*5 + 3*6 = 32
dot = u @ v  # same, using @ operator
```

The dot product relates to the angle between vectors:

$$
\mathbf{u} \cdot \mathbf{v} = \|\mathbf{u}\| \|\mathbf{v}\| \cos \theta
$$

**Cross product (3D only):** Produces a vector orthogonal to both inputs:

$$
\mathbf{u} \times \mathbf{v} = \begin{bmatrix} u_2 v_3 - u_3 v_2 \\ u_3 v_1 - u_1 v_3 \\ u_1 v_2 - u_2 v_1 \end{bmatrix}
$$

```python
u = np.array([1, 0, 0])
v = np.array([0, 1, 0])
cross = np.cross(u, v)  # [0, 0, 1] (z-axis)
```

### Vector Spaces

A **vector space** $V$ over a field $\mathbb{F}$ (typically $\mathbb{R}$) is a set closed under addition and scalar multiplication, satisfying the eight axioms listed above. The most important vector space for developers is $\mathbb{R}^n$ — all $n$-tuples of real numbers.

**Examples of vector spaces:**

- $\mathbb{R}^n$ — all $n$-dimensional real vectors
- $\mathbb{C}^n$ — all $n$-dimensional complex vectors
- The set of all $m \times n$ matrices
- The set of all polynomials of degree $\leq n$
- The set of all functions $f: \mathbb{R} \to \mathbb{R}$

**Non-examples:**

- The set of all vectors with non-negative components (not closed under scalar multiplication by negatives)
- The set of all unit vectors (not closed under addition)

```python
# Check closure: sum of two vectors is still in R^n
v1 = np.array([1, 2, 3])
v2 = np.array([-5, 7, 0])
v_sum = v1 + v2  # [-4, 9, 3] — still in R^3, so closure holds

# Non-example: scaling a non-negative vector by -1 leaves the set
v_nonneg = np.array([1, 2, 3])
neg = -2 * v_nonneg  # [-2, -4, -6] — not non-negative, so not a vector space
```

### Linear Combinations

A **linear combination** of vectors $\mathbf{v}_1, \mathbf{v}_2, \dots, \mathbf{v}_k$ is any vector of the form:

$$
c_1 \mathbf{v}_1 + c_2 \mathbf{v}_2 + \dots + c_k \mathbf{v}_k
$$

where $c_i \in \mathbb{R}$ are scalars (coefficients).

```python
v1 = np.array([1, 0])
v2 = np.array([0, 1])
linear_combination = 3 * v1 + (-2) * v2  # [3, -2]
```

Linear combinations are everywhere in computing:

- **Pixel values:** An image is a linear combination of basis images
- **Features:** A prediction is a linear combination of input features (linear regression)
- **Neural networks:** Each neuron computes a linear combination of its inputs followed by a nonlinearity

**Affine combinations:** A linear combination where the coefficients sum to 1:

$$
c_1 + c_2 + \dots + c_k = 1
$$

Affine combinations produce points on the line through the vectors (or inside the convex hull when coefficients are also non-negative).

### Span

The **span** of a set of vectors is the set of all possible linear combinations:

$$
\text{Span}\{\mathbf{v}_1, \dots, \mathbf{v}_k\} = \left\{ \sum_{i=1}^{k} c_i \mathbf{v}_i \mid c_i \in \mathbb{R} \right\}
$$

```python
import numpy as np

# span of [1,0] and [0,1] is all of R^2
e1 = np.array([1.0, 0.0])
e2 = np.array([0.0, 1.0])
# any vector in R^2 can be formed: a*e1 + b*e2

# span of [1,0] alone is just the x-axis
e1 = np.array([1.0, 0.0])
# only vectors of form [a, 0] can be produced
```

**Span in 2D intuition:**

- One non-zero vector spans a line through the origin
- Two non-collinear vectors span the entire plane
- Two collinear vectors span a line (same as one of them)
- The zero vector alone spans $\{\mathbf{0}\}$ (the trivial subspace)

```python
# Check if a vector is in the span of others
v1 = np.array([1, 2])
v2 = np.array([3, 4])
target = np.array([5, 8])

# Solve c1*v1 + c2*v2 = target
A = np.column_stack([v1, v2])
coeffs, _, _, _ = np.linalg.lstsq(A, target, rcond=None)
print(coeffs)  # solution exists if residual is near zero
```

### Linear Independence

A set of vectors $\{\mathbf{v}_1, \dots, \mathbf{v}_k\}$ is **linearly independent** if the only solution to:

$$
c_1 \mathbf{v}_1 + c_2 \mathbf{v}_2 + \dots + c_k \mathbf{v}_k = \mathbf{0}
$$

is $c_1 = c_2 = \dots = c_k = 0$.

If there exists a non-trivial solution (some $c_i \neq 0$), the set is **linearly dependent**.

```python
# Independent vectors
v1 = np.array([1, 0])
v2 = np.array([0, 1])
# Only solution to c1*v1 + c2*v2 = 0 is c1=c2=0

# Dependent vectors
v3 = np.array([2, 4])  # v3 = 2 * v1 + 4 * v2
# Actually v3 = 2*v1 + 4*v2 means {v1, v2, v3} is dependent

# Check independence by building a matrix and computing rank
A = np.column_stack([v1, v2, v3])
rank = np.linalg.matrix_rank(A)
print(rank)  # 2, meaning only 2 independent vectors
```

**Testing linear independence:** The rank of the matrix formed by the vectors equals the number of vectors if and only if they are independent.

```python
def is_independent(vectors):
    A = np.column_stack(vectors)
    return np.linalg.matrix_rank(A) == len(vectors)

v1 = np.array([1, 2, 3])
v2 = np.array([4, 5, 6])
v3 = np.array([7, 8, 9])
print(is_independent([v1, v2, v3]))  # False

v3b = np.array([0, 0, 1])
print(is_independent([v1, v2, v3b]))  # True
```

**Intuition for independence:**

- Two vectors are independent if they are not collinear (one is not a scalar multiple of the other)
- Three vectors in $\mathbb{R}^3$ are independent if they are not coplanar
- You cannot have more than $n$ independent vectors in $\mathbb{R}^n$
- One vector is independent iff it is non-zero

### Basis and Dimension

A **basis** of a vector space $V$ is a set of vectors that is:

1. **Linearly independent** — no redundancy
2. **Spanning** — every vector in $V$ can be expressed as a linear combination

The **dimension** of $V$ is the number of vectors in any basis — a fundamental invariant.

**Standard basis for $\mathbb{R}^n$:**

$$
\mathbf{e}_1 = \begin{bmatrix}1\\0\\\vdots\\0\end{bmatrix},\;
\mathbf{e}_2 = \begin{bmatrix}0\\1\\\vdots\\0\end{bmatrix},\;\dots,\;
\mathbf{e}_n = \begin{bmatrix}0\\0\\\vdots\\1\end{bmatrix}
$$

```python
# Standard basis vectors in R^3
e1 = np.array([1, 0, 0])
e2 = np.array([0, 1, 0])
e3 = np.array([0, 0, 1])

# Every vector is a unique combination of basis vectors
v = np.array([3, -2, 5])
# v = 3*e1 + (-2)*e2 + 5*e3
reconstructed = 3*e1 + (-2)*e2 + 5*e3
print(np.allclose(v, reconstructed))  # True
```

**Non-standard bases are powerful:** Choosing the right basis makes problems easier. For example, the Fourier basis (sine/cosine waves) makes convolution trivial. Eigenvector bases diagonalize linear transformations.

```python
# Expressing a vector in a non-standard basis
basis = np.array([
    [1, 1],   # first basis vector
    [1, -1]   # second basis vector
]).T  # columns are basis vectors

v = np.array([5, 1])
# Solve basis * coords = v for coordinates
coords = np.linalg.solve(basis, v)
print(coords)  # [3, 2] meaning v = 3*b1 + 2*b2
```

**Dimension facts:**
- $\dim(\mathbb{R}^n) = n$
- $\dim(\text{space of } m \times n \text{ matrices}) = mn$
- $\dim(\text{polynomials of degree } \leq n) = n + 1$
- Any set of $n$ independent vectors in an $n$-dimensional space automatically spans (and vice versa)

### Coordinate Systems

A **coordinate system** assigns a unique tuple of numbers to each vector relative to a chosen basis. If $\mathcal{B} = \{\mathbf{b}_1, \dots, \mathbf{b}_n\}$ is a basis for $V$, then every $\mathbf{v} \in V$ has a unique representation:

$$
\mathbf{v} = [\mathbf{v}]_{\mathcal{B}} = \begin{bmatrix} c_1 \\ c_2 \\ \vdots \\ c_n \end{bmatrix}_\mathcal{B}
$$

where $\mathbf{v} = c_1 \mathbf{b}_1 + c_2 \mathbf{b}_2 + \dots + c_n \mathbf{b}_n$.

**Change of basis:** To convert coordinates from basis $\mathcal{B}$ to basis $\mathcal{C}$, we use a change-of-basis matrix:

$$
[\mathbf{v}]_{\mathcal{C}} = P_{\mathcal{C} \leftarrow \mathcal{B}} [\mathbf{v}]_{\mathcal{B}}
$$

```python
# Change of basis in R^2
B = np.array([[1, 1], [1, -1]])  # basis B columns
C = np.array([[1, 0], [0, 1]])   # standard basis

# Coordinates of v in basis B
v_B = np.array([2, 3])  # v = 2*b1 + 3*b2

# Convert to standard basis
v_standard = B @ v_B
print(v_standard)  # [5, -1]

# Convert from standard back to B
v_B_again = np.linalg.solve(B, v_standard)
print(v_B_again)  # [2, 3]
```

**Why coordinate systems matter in development:**

- **3D graphics:** Objects are defined in local coordinates, then transformed to world, view, and clip coordinates via basis changes
- **Machine learning:** Feature engineering is choosing a good basis for your data
- **Signal processing:** The Fourier transform is a change of basis from the time domain to the frequency domain
- **Quantum computing:** Qubit states are vectors in $\mathbb{C}^2$; quantum gates are basis changes

### Subspaces

A **subspace** $W$ of a vector space $V$ is a subset that is itself a vector space under the same operations. To verify a subspace, check:

1. $\mathbf{0} \in W$ (contains zero)
2. Closed under addition ($\mathbf{u}, \mathbf{v} \in W \implies \mathbf{u} + \mathbf{v} \in W$)
3. Closed under scalar multiplication ($\mathbf{v} \in W, c \in \mathbb{R} \implies c\mathbf{v} \in W$)

**Important subspaces of a matrix $A$:**

- **Column space (range):** $\text{Col}(A)$ — span of the columns of $A$. Contains all vectors $A\mathbf{x}$.
- **Null space (kernel):** $\text{Null}(A)$ — all $\mathbf{x}$ such that $A\mathbf{x} = \mathbf{0}$.
- **Row space:** $\text{Row}(A)$ — span of the rows of $A$.
- **Left null space:** $\text{Null}(A^T)$ — all $\mathbf{y}$ such that $\mathbf{y}^T A = \mathbf{0}$.

```python
A = np.array([[1, 2, 0],
              [2, 4, 1],
              [0, 0, 3]])

# Column space = span of columns
# Null space via SVD
U, s, Vt = np.linalg.svd(A)
null_space = Vt[s < 1e-10]
print(null_space)  # rows form basis for null space

# Rank = dimension of column space
rank = np.linalg.matrix_rank(A)
print(f"Column space dimension: {rank}")
```

**The four fundamental subspaces** (due to Strang) are central to understanding linear systems:

| Subspace | Notation | Dimension |
|----------|----------|-----------|
| Column space | $\text{Col}(A)$ | $r$ (rank) |
| Null space | $\text{Null}(A)$ | $n - r$ |
| Row space | $\text{Row}(A)$ | $r$ |
| Left null space | $\text{Null}(A^T)$ | $m - r$ |

where $A$ is $m \times n$ with rank $r$.

```python
def four_subspaces(A):
    m, n = A.shape
    r = np.linalg.matrix_rank(A)

    # Column space basis: first r columns of U from SVD
    U, s, Vt = np.linalg.svd(A)
    col_basis = U[:, :r]

    # Null space basis: last n-r rows of Vt (or columns of V)
    null_basis = Vt[r:, :].T

    # Row space basis: first r rows of Vt
    row_basis = Vt[:r, :]

    # Left null space: last m-r columns of U
    left_null_basis = U[:, r:]

    return col_basis, null_basis, row_basis, left_null_basis

A = np.array([[1, 2, 1],
              [2, 4, 2],
              [1, 0, 0]])
cb, nb, rb, lnb = four_subspaces(A)
```

**Dimension of sum and intersection:**

$$
\dim(U + W) = \dim U + \dim W - \dim(U \cap W)
$$

This is analogous to the inclusion-exclusion principle for sets.

**Direct sum:** If $U \cap W = \{\mathbf{0}\}$, then $U + W$ is called a **direct sum**, denoted $U \oplus W$, and every vector in the sum has a unique decomposition.

## Study Cases

### Study Case 1: Representing Image Data as Vectors

A grayscale image of size $28 \times 28$ pixels (like MNIST) is a vector in $\mathbb{R}^{784}$.

```python
import numpy as np

# Flatten a 28x28 image into a 784-dimensional vector
image = np.random.rand(28, 28)
vector = image.flatten()
print(f"Image vector shape: {vector.shape}")

# Each pixel value is a component of the vector
# The vector lives in a 784-dimensional vector space

# Classifying digits: each digit class spans a subspace
# A classifier finds which subspace the vector is closest to

# The span of all digit images is lower-dimensional due to correlations
from numpy.linalg import matrix_rank
images = np.random.rand(100, 784)  # 100 digit images
print(f"Rank of image set: {matrix_rank(images)}")
# Typically rank < min(100, 784) due to structure
```

### Study Case 2: Feature Vectors in Machine Learning

A house prediction dataset with 10 features (bedrooms, square footage, year built, etc.) produces vectors in $\mathbb{R}^{10}$.

```python
# Feature vector for a house
house = np.array([3, 1500, 2005, 2, 1, 0, 1, 30000, 5, 0])
# components: [bedrooms, sqft, year, baths, garage, pool, ac, lot_size, rooms, foreclosure]

# Linear regression finds coefficients w such that:
# price ≈ w · house + bias

# The training data spans a subspace of R^10
# Linear regression projects the target vector onto this subspace

# Feature engineering = changing the basis
# Example: add polynomial features -> higher-dimensional space
def polynomial_features(x, degree=2):
    """Expand x into polynomial basis: [1, x, x^2, ..., x^degree]"""
    return np.array([x**i for i in range(degree+1)]).T

x = np.array([1, 2, 3])
print(polynomial_features(x, 2))
# [[1, 1, 1],
#  [1, 2, 4],
#  [1, 3, 9]]
```

### Study Case 3: 3D Graphics Coordinate Systems

In computer graphics, every object has local coordinates that are transformed through a pipeline of basis changes.

```python
# Object defined in local coordinates
local_vertices = np.array([
    [0, 0, 0],  # vertex 1
    [1, 0, 0],  # vertex 2
    [0, 1, 0],  # vertex 3
    [0, 0, 1]   # vertex 4
])

# Model matrix: local -> world (rotation + translation)
theta = np.radians(45)
c, s = np.cos(theta), np.sin(theta)
rotation = np.array([[c, -s, 0],
                     [s,  c, 0],
                     [0,  0, 1]])
translation = np.array([5, 3, 0])

world_vertices = local_vertices @ rotation.T + translation
print("World coordinates:\n", world_vertices)

# View matrix: world -> camera coordinates (change of basis)
# Projection matrix: camera -> clip coordinates
# Each step is a linear transformation / change of basis
```

## Examples

### Example 1: Vector Operations in 2D

```python
import numpy as np
import matplotlib.pyplot as plt

# Define vectors
u = np.array([2, 3])
v = np.array([5, 1])

# Addition
sum_vec = u + v

# Scalar multiplication
scaled = 2 * u

# Dot product
dot = np.dot(u, v)

print(f"u = {u}")
print(f"v = {v}")
print(f"u + v = {sum_vec}")
print(f"2 * u = {scaled}")
print(f"u · v = {dot}")

# Angle between vectors
cos_theta = dot / (np.linalg.norm(u) * np.linalg.norm(v))
angle = np.degrees(np.arccos(cos_theta))
print(f"Angle: {angle:.2f} degrees")
```

### Example 2: Checking Linear Independence

```python
def check_independence(vectors):
    A = np.column_stack(vectors)
    r = np.linalg.matrix_rank(A)
    n = len(vectors)
    print(f"Rank: {r}, Vectors: {n}")
    return r == n

# Independent pair
v1 = np.array([1, 0, 0])
v2 = np.array([0, 1, 0])
print("v1, v2 independent:", check_independence([v1, v2]))

# Dependent pair
v3 = np.array([2, 3, 0])
print("v1, v3 independent:", check_independence([v1, np.array([2, 3, 0])]))

# Three independent in R^3
v4 = np.array([0, 0, 1])
print("v1, v2, v4 independent:", check_independence([v1, v2, v4]))

# Four vectors in R^3 are always dependent
v5 = np.array([1, 1, 1])
print("v1, v2, v4, v5:", check_independence([v1, v2, v4, v5]))
```

### Example 3: Basis and Coordinate Representation

```python
# Two different bases for R^2
standard = np.array([[1, 0], [0, 1]])
custom = np.array([[1, 1], [1, -1]])

# Same vector in different bases
v_standard = np.array([4, 3])

# Coordinates in custom basis
coords_custom = np.linalg.solve(custom, v_standard)
print(f"Standard coords: {v_standard}")
print(f"Custom basis coords: {coords_custom}")

# Verify
reconstructed = custom @ coords_custom
print(f"Reconstructed: {reconstructed}")

# Express a vector defined in custom basis in standard basis
w_custom = np.array([1, 2])
w_standard = custom @ w_custom
print(f"Vector [1,2]_B in standard basis: {w_standard}")
```

### Example 4: Computing the Four Fundamental Subspaces

```python
A = np.array([[1, 2, 0, -1],
              [0, 1, 3,  2],
              [2, 4, 0, -2]])

m, n = A.shape
r = np.linalg.matrix_rank(A)
print(f"A is {m}x{n}, rank = {r}")

# Using SVD for all four subspaces
U, s, Vt = np.linalg.svd(A)

# Column space: first r columns of U
col_space = U[:, :r]
print(f"Column space basis (dim={r}):\n{col_space}")

# Null space: last n-r rows of Vt (transposed)
null_space = Vt[r:, :].T
print(f"Null space basis (dim={n-r}):\n{null_space}")

# Row space: first r rows of Vt
row_space = Vt[:r, :]
print(f"Row space basis (dim={r}):\n{row_space.T}")

# Left null space: last m-r columns of U
left_null = U[:, r:]
print(f"Left null space basis (dim={m-r}):\n{left_null}")

# Verify: A * (null space vector) should be zero
for i in range(n-r):
    residual = A @ null_space[:, i]
    print(f"A * n{i} residual norm: {np.linalg.norm(residual):.2e}")
```

### Example 5: Projection onto a Subspace

```python
# Project vector v onto the span of basis vectors
def project_onto_subspace(v, basis):
    """Project v onto the column space of basis matrix."""
    # P = A(A^T A)^{-1} A^T
    A = np.column_stack(basis)
    P = A @ np.linalg.inv(A.T @ A) @ A.T
    return P @ v

# Project v = [3, 4] onto the line y = x
basis = [np.array([1, 1])]
v = np.array([3, 4])
proj = project_onto_subspace(v, basis)
print(f"Original v: {v}")
print(f"Projection onto y=x: {proj}")
print(f"Residual: {v - proj}")

# Project onto the plane z = 0 in R^3
basis_xy = [np.array([1, 0, 0]), np.array([0, 1, 0])]
v3 = np.array([2, 3, 7])
proj_xy = project_onto_subspace(v3, basis_xy)
print(f"Projection onto xy-plane: {proj_xy}")
```

## Glossary

| Term | Definition |
|------|------------|
| Vector | An ordered tuple of numbers, representing a point or direction in space |
| Scalar | A single real number used to scale vectors |
| Vector space | A set closed under addition and scalar multiplication, satisfying eight axioms |
| Linear combination | A sum of scaled vectors: $c_1\mathbf{v}_1 + \dots + c_k\mathbf{v}_k$ |
| Span | The set of all linear combinations of a given set of vectors |
| Linear independence | A set where no vector is a linear combination of the others |
| Basis | A linearly independent spanning set for a vector space |
| Dimension | The number of vectors in any basis of a vector space |
| Coordinate system | The representation of a vector relative to a specific basis |
| Subspace | A subset of a vector space that is itself a vector space |
| Column space | The span of the columns of a matrix |
| Null space | The set of all $\mathbf{x}$ such that $A\mathbf{x} = \mathbf{0}$ |
| Direct sum | A sum of subspaces where intersection is trivial, giving unique decomposition |
| Dot product | The scalar product $\sum u_i v_i$, relating to angle between vectors |
| Cross product | A vector orthogonal to two input vectors in $\mathbb{R}^3$ |
| Affine combination | A linear combination whose coefficients sum to 1 |
| Rank | The dimension of the column (or row) space |

## Quick References

- [3Blue1Brown — Essence of Linear Algebra](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab) — geometric intuition for vectors and vector spaces
- [Gilbert Strang — Linear Algebra Course (MIT 18.06)](https://ocw.mit.edu/courses/mathematics/18-06-linear-algebra-spring-2010/) — the definitive lecture series
- [NumPy Documentation: Linear Algebra](https://numpy.org/doc/stable/reference/routines.linalg.html) — reference for all linear algebra operations in NumPy
- [Immersive Linear Algebra (J. Strom, K. Strom)](https://immersivemath.com/ila/) — interactive visual textbook
- [Linear Algebra Done Right (Axler)](https://linear.axler.net/) — rigorous but accessible coverage

## Next Steps

- [Matrices & Matrix Operations](matrices-and-operations.md) — extend vectors to 2D arrays and their algebra
- [Linear Transformations](linear-transformations.md) — maps between vector spaces and their matrix representations
- [Systems of Linear Equations](systems-of-linear-equations.md) — solving $A\mathbf{x} = \mathbf{b}$ using Gaussian elimination
- [Orthogonality & Least Squares](orthogonality-and-least-squares.md) — dot products, angles, and orthogonal projections
