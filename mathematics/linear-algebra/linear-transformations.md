# Linear Transformations

## Description

Linear transformations are functions between vector spaces that preserve the structure of addition and scalar multiplication. Every linear transformation corresponds to a matrix, and every matrix defines a linear transformation. Understanding this correspondence is the heart of linear algebra — it connects geometric intuition (rotations, scaling, reflections) to algebraic computation (matrix multiplication) and powers 3D graphics, computer vision, and neural network layers.

## Prerequisites

- [Vectors & Vector Spaces](vectors-and-vector-spaces.md) — vector spaces, basis, span, linear combinations
- [Matrices & Matrix Operations](matrices-and-operations.md) — matrix multiplication, identity, inverse

## Table of Contents

- [Definition of a Linear Transformation](#definition-of-a-linear-transformation)
- [The Matrix of a Linear Transformation](#the-matrix-of-a-linear-transformation)
- [Composition of Transformations](#composition-of-transformations)
- [Geometric Transformations](#geometric-transformations)
- [Change of Basis](#change-of-basis)
- [Kernel and Image](#kernel-and-image)
- [Rank-Nullity Theorem](#rank-nullity-theorem)
- [Invertible Linear Transformations](#invertible-linear-transformations)
- [Applications](#applications)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Definition of a Linear Transformation

A **linear transformation** $T: V \to W$ (where $V$ and $W$ are vector spaces) satisfies two properties:

1. **Additivity:** $T(\mathbf{u} + \mathbf{v}) = T(\mathbf{u}) + T(\mathbf{v})$ for all $\mathbf{u}, \mathbf{v} \in V$
2. **Homogeneity:** $T(c\mathbf{v}) = c T(\mathbf{v})$ for all $c \in \mathbb{R}$, $\mathbf{v} \in V$

These two properties can be combined:

$$
T(c_1\mathbf{v}_1 + c_2\mathbf{v}_2) = c_1 T(\mathbf{v}_1) + c_2 T(\mathbf{v}_2)
$$

```python
import numpy as np

def is_linear(T, test_vectors, tol=1e-10):
    """Test if transformation T is approximately linear."""
    for u, v in zip(test_vectors[:-1], test_vectors[1:]):
        u, v = np.array(u), np.array(v)
        c, d = 2.5, -1.3

        # Test linearity: T(cu + dv) == c T(u) + d T(v)
        lhs = T(c * u + d * v)
        rhs = c * T(u) + d * T(v)
        if not np.allclose(lhs, rhs, atol=tol):
            return False
    return True

# Linear: matrix multiplication
T_linear = lambda x: np.array([[2, 0], [0, 3]]) @ x
print(is_linear(T_linear, [[1, 2], [3, 4], [5, 6]]))  # True

# Non-linear: squaring components
T_nonlinear = lambda x: x ** 2
print(is_linear(T_nonlinear, [[1, 2], [3, 4], [5, 6]]))  # False

# Non-linear: adding a constant
T_affine = lambda x: x + 1
print(is_linear(T_affine, [[1, 2], [3, 4], [5, 6]]))  # False
```

**Examples of linear transformations:**

- **Scaling:** $T(\mathbf{v}) = c\mathbf{v}$
- **Rotation:** $T(\mathbf{v}) = R_\theta \mathbf{v}$ in $\mathbb{R}^2$
- **Projection:** $T(\mathbf{v}) = \text{proj}_U(\mathbf{v})$ onto a subspace $U$
- **Differentiation:** $T(f) = f'$ on polynomial spaces
- **Integration:** $T(f) = \int_a^x f(t) dt$
- **Matrix multiplication:** $T(\mathbf{x}) = A\mathbf{x}$

**Non-examples:**

- $T(\mathbf{x}) = \mathbf{x} + \mathbf{b}$ (translation is affine but not linear — it is linear only if $\mathbf{b} = \mathbf{0}$)
- $T(\mathbf{x}) = \|\mathbf{x}\|$ (norm is not linear; $\|c\mathbf{x}\| = |c| \|\mathbf{x}\| \neq c \|\mathbf{x}\|$ for $c < 0$)
- $T(\mathbf{x}) = \mathbf{x}^2$ (component-wise squaring)

### The Matrix of a Linear Transformation

Every linear transformation between finite-dimensional vector spaces can be represented as a matrix. If $T: \mathbb{R}^n \to \mathbb{R}^m$, there exists an $m \times n$ matrix $A$ such that:

$$
T(\mathbf{x}) = A\mathbf{x}
$$

The columns of $A$ are the images of the standard basis vectors:

$$
A = \begin{bmatrix} T(\mathbf{e}_1) & T(\mathbf{e}_2) & \dots & T(\mathbf{e}_n) \end{bmatrix}
$$

```python
def transformation_matrix(T, n):
    """Compute the matrix of linear transformation T: R^n -> R^m."""
    # Apply T to each standard basis vector
    m_output = T(np.eye(n)[:, 0]).shape[0]
    A = np.zeros((m_output, n))
    for i in range(n):
        e_i = np.eye(n)[:, i]
        A[:, i] = T(e_i)
    return A

# Example: T(x,y) = (2x + y, x - 3y)
T = lambda v: np.array([2*v[0] + v[1], v[0] - 3*v[1]])
A = transformation_matrix(T, 2)
print("Matrix:\n", A)
# [[2, 1],
#  [1, -3]]

# Verify: T(v) == A @ v for any v
v = np.array([4, 7])
print("T(v):", T(v))
print("A @ v:", A @ v)
```

**General case ($T: V \to W$ with arbitrary bases):** If $\mathcal{B} = \{\mathbf{b}_1, \dots, \mathbf{b}_n\}$ is a basis for $V$ and $\mathcal{C} = \{\mathbf{c}_1, \dots, \mathbf{c}_m\}$ is a basis for $W$, the matrix $[T]_{\mathcal{C} \leftarrow \mathcal{B}}$ has entries:

$$
[T]_{\mathcal{C} \leftarrow \mathcal{B}} = \begin{bmatrix} [T(\mathbf{b}_1)]_\mathcal{C} & [T(\mathbf{b}_2)]_\mathcal{C} & \dots & [T(\mathbf{b}_n)]_\mathcal{C} \end{bmatrix}
$$

where $[T(\mathbf{b}_j)]_\mathcal{C}$ are the coordinates of $T(\mathbf{b}_j)$ in the $\mathcal{C}$ basis.

### Composition of Transformations

If $T: U \to V$ and $S: V \to W$ are linear, their composition $S \circ T: U \to W$ is also linear. The matrix of the composition is the product of the individual matrices:

$$
[S \circ T] = [S][T]
$$

```python
# Composition of rotation and scaling
def rotation_2d(theta):
    c, s = np.cos(theta), np.sin(theta)
    return np.array([[c, -s], [s, c]])

def scaling_2d(sx, sy):
    return np.array([[sx, 0], [0, sy]])

# Rotate by 30 degrees, then scale x by 2
R = rotation_2d(np.radians(30))
S = scaling_2d(2, 1)

# Compose: scale after rotation
composed = S @ R

# Apply to a point
p = np.array([1, 0])
print("Original:", p)
print("Rotated:", R @ p)
print("Scaled after rotation:", composed @ p)

# Order matters! Rotation after scaling:
composed_rev = R @ S
print("Rotation after scale:", composed_rev @ p)
```

**Why composition is matrix multiplication:** The entries of $AB$ are precisely the coefficients of the composed transformation. This is why matrix multiplication is defined the way it is.

```python
# Verify: (S ∘ R)(v) = S(R(v)) = S @ (R @ v) = (S @ R) @ v
v = np.array([3, 4])
direct = S(R(v))
matmul = (S @ R) @ v
print(np.allclose(direct, matmul))  # True
```

### Geometric Transformations

**Rotation in 2D** by angle $\theta$ counterclockwise:

$$
R_\theta = \begin{bmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{bmatrix}
$$

```python
def rotate_2d(v, degrees):
    theta = np.radians(degrees)
    R = np.array([[np.cos(theta), -np.sin(theta)],
                  [np.sin(theta),  np.cos(theta)]])
    return R @ v

v = np.array([1, 0])
v_rot = rotate_2d(v, 90)
print(v_rot)  # [~0, 1]
```

**Scaling** by factors $s_x$ and $s_y$:

$$
S = \begin{bmatrix} s_x & 0 \\ 0 & s_y \end{bmatrix}
```

**Reflection** across the $x$-axis:

$$
F_x = \begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}
$$

Reflection across the line $y = x$:

$$
F_{y=x} = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}
$$

**Shear** in the $x$-direction:

$$
H_x = \begin{bmatrix} 1 & k \\ 0 & 1 \end{bmatrix}
$$

```python
def shear_x(v, k):
    H = np.array([[1, k], [0, 1]])
    return H @ v

# Square corners
square = np.array([[0, 0], [1, 0], [1, 1], [0, 1]]).T
sheared = shear_x(square, 0.5)
```

**Projection** onto a line through the origin with unit direction $\mathbf{u}$:

$$
P = \mathbf{u} \mathbf{u}^T = \begin{bmatrix} u_1^2 & u_1 u_2 \\ u_1 u_2 & u_2^2 \end{bmatrix}
$$

```python
def projection_matrix(u):
    """Projection matrix onto the span of unit vector u."""
    u = u / np.linalg.norm(u)  # ensure unit
    return np.outer(u, u)

u = np.array([1, 1]) / np.sqrt(2)
P = projection_matrix(u)
v = np.array([3, 4])
proj = P @ v
print(f"Projection: {proj}")
# proj is the component of v in the direction of u
```

**Combining transformations via homogeneous coordinates:**

In 3D graphics, affine transformations (linear + translation) are represented as $4 \times 4$ matrices using homogeneous coordinates:

$$
\begin{bmatrix} \mathbf{v}' \\ 1 \end{bmatrix} = \begin{bmatrix} A & \mathbf{t} \\ \mathbf{0}^T & 1 \end{bmatrix} \begin{bmatrix} \mathbf{v} \\ 1 \end{bmatrix}
$$

```python
def translation_matrix(tx, ty):
    """2D translation as a 3x3 homogeneous matrix."""
    return np.array([[1, 0, tx],
                     [0, 1, ty],
                     [0, 0, 1]])

def homogeneous_transform(A, t):
    """Combine linear transform A and translation t into 3x3."""
    H = np.eye(3)
    H[:2, :2] = A
    H[:2, 2] = t
    return H

# Rotation + translation
R = rotation_2d(np.radians(45))
t = np.array([2, 3])
H = homogeneous_transform(R, t)

# Apply to point
p = np.array([1, 0, 1])  # homogeneous
p_transformed = H @ p
print(f"Transformed: {p_transformed[:2]}")  # [2.707, 3.707]
```

### Change of Basis

A linear transformation's matrix representation depends on the basis chosen for the domain and codomain. **Change of basis** converts between representations.

If $\mathcal{B}$ and $\mathcal{B}'$ are two bases for $V$, the change-of-basis matrix $P_{\mathcal{B}' \leftarrow \mathcal{B}}$ satisfies:

$$
[\mathbf{v}]_{\mathcal{B}'} = P_{\mathcal{B}' \leftarrow \mathcal{B}} [\mathbf{v}]_{\mathcal{B}}
$$

The columns of $P$ are the $\mathcal{B}'$-coordinates of the $\mathcal{B}$-basis vectors.

```python
# Standard basis and a rotated basis
theta = np.radians(45)
B = np.array([[np.cos(theta), -np.sin(theta)],
              [np.sin(theta),  np.cos(theta)]])  # rotated basis

# Change-of-basis: standard -> rotated
P = B.T  # since B is orthogonal, B^-1 = B^T

v_standard = np.array([3, 2])
v_rotated = P @ v_standard
print(f"v in rotated coordinates: {v_rotated}")

# Convert back
v_back = B @ v_rotated
print(f"v back in standard: {v_back}")
```

**Similarity transformation:** If $A$ is the matrix of $T$ in standard basis and $A'$ is the matrix in basis $\mathcal{B}$, then:

$$
A' = P^{-1} A P
$$

where $P$'s columns are the $\mathcal{B}$ basis vectors expressed in the standard basis.

```python
# T represented by A in standard basis
A = np.array([[2, 1], [1, 2]])

# Change to eigenvector basis (which diagonalizes A)
evals, evecs = np.linalg.eig(A)
P = evecs  # columns are eigenvectors

# A in eigenvector basis = P^{-1} A P
A_diag = np.linalg.inv(P) @ A @ P
print(f"Diagonalized: {np.round(A_diag, 10)}")
# Should be diag(evals) = [[3, 0], [0, 1]]
```

### Kernel and Image

The **kernel** (or null space) of $T: V \to W$ is the set of vectors that map to zero:

$$
\ker(T) = \{\mathbf{v} \in V \mid T(\mathbf{v}) = \mathbf{0} \}
$$

The **image** (or range) of $T$ is the set of all outputs:

$$
\text{Im}(T) = \{ T(\mathbf{v}) \mid \mathbf{v} \in V \}
$$

```python
def kernel_image(T, n, m, tol=1e-10):
    """Compute basis for kernel and image of T: R^n -> R^m."""
    A = transformation_matrix(T, n)

    # Kernel = null space of A
    U, s, Vt = np.linalg.svd(A)
    kernel_basis = Vt[s < tol]  # rows of Vt with singular value ~0

    # Image = column space of A
    rank = np.sum(s > tol)
    image_basis = U[:, :rank]

    return kernel_basis, image_basis

# T(x, y, z) = (x + y, y + z)
T = lambda v: np.array([v[0] + v[1], v[1] + v[2]])
ker, im = kernel_image(T, 3, 2)
print(f"Kernel dimension: {len(ker)}")
print(f"Image dimension: {im.shape[1]}")
```

**Properties:**

- $\ker(T)$ is a subspace of $V$
- $\text{Im}(T)$ is a subspace of $W$
- $T$ is **injective** (one-to-one) iff $\ker(T) = \{\mathbf{0}\}$
- $T$ is **surjective** (onto) iff $\text{Im}(T) = W$
- $T$ is **bijective** (invertible) iff it is both injective and surjective

### Rank-Nullity Theorem

The **rank-nullity theorem** connects the dimensions of the kernel and image:

$$
\dim(\ker(T)) + \dim(\text{Im}(T)) = \dim(V)
$$

In matrix terms:

$$
\text{nullity}(A) + \text{rank}(A) = n
$$

where $A$ is $m \times n$.

```python
def rank_nullity(A):
    n = A.shape[1]
    rank = np.linalg.matrix_rank(A)
    nullity = n - rank
    print(f"Rank: {rank}, Nullity: {nullity}")
    print(f"Rank + Nullity = {rank + nullity} = n = {n}")
    return rank, nullity

A = np.array([[1, 2, 1],
              [2, 4, 2],
              [1, 0, 0]])
rank_nullity(A)  # Rank: 2, Nullity: 1, Sum: 3

# Another example: projection onto xy-plane
P = np.array([[1, 0, 0],
              [0, 1, 0],
              [0, 0, 0]])
rank_nullity(P)  # Rank: 2, Nullity: 1
# Kernel = z-axis, Image = xy-plane
```

**Consequences:**
- For $T: \mathbb{R}^n \to \mathbb{R}^m$ with $n > m$, the kernel is always non-trivial ($\dim \ker \geq n - m > 0$), so $T$ cannot be injective
- For $n < m$, the image cannot be all of $\mathbb{R}^m$, so $T$ cannot be surjective
- For $n = m$, injective $\iff$ surjective $\iff$ invertible

### Invertible Linear Transformations

A linear transformation $T: V \to W$ is **invertible** if there exists $T^{-1}: W \to V$ such that:

$$
T^{-1}(T(\mathbf{v})) = \mathbf{v} \quad \text{and} \quad T(T^{-1}(\mathbf{w})) = \mathbf{w}
$$

For matrix $A$, this means $A^{-1}$ exists and:

$$
A^{-1} A = I = A A^{-1}
$$

```python
# Invertible: rotation
R = rotation_2d(np.radians(30))
R_inv = np.linalg.inv(R)
print("R @ R_inv:\n", R @ R_inv)  # identity

# Non-invertible: projection
P = projection_matrix(np.array([1, 0]))
try:
    np.linalg.inv(P)
except np.linalg.LinAlgError:
    print("Projection is not invertible (singular)")
```

**Geometric interpretation of invertibility:**
- Injective: no two vectors map to the same output (distinct inputs map to distinct outputs)
- Surjective: every output vector is reachable
- Invertible: the transformation is a bijection (one-to-one and onto) that can be undone

Rotations, reflections, and non-zero scalings are invertible. Projections and shears with zero determinant are not.

## Glossary

| Term | Definition |
|------|------------|
| Linear transformation | A function $T$ satisfying $T(\mathbf{u}+\mathbf{v}) = T(\mathbf{u})+T(\mathbf{v})$ and $T(c\mathbf{v}) = cT(\mathbf{v})$ |
| Linear operator | A linear transformation from a space to itself ($T: V \to V$) |
| Transformation matrix | The matrix whose columns are the images of basis vectors |
| Composition | Applying one transformation after another: $S \circ T$ |
| Kernel (null space) | $\{\mathbf{v} \mid T(\mathbf{v}) = \mathbf{0}\}$ |
| Image (range) | $\{T(\mathbf{v}) \mid \mathbf{v} \in V\}$ |
| Rank | $\dim(\text{Im}(T))$, the dimension of the output space |
| Nullity | $\dim(\ker(T))$, the dimension of the kernel |
| Rank-Nullity theorem | $\text{rank}(T) + \text{nullity}(T) = \dim(V)$ |
| Injective (one-to-one) | $\mathbf{u} \neq \mathbf{v} \implies T(\mathbf{u}) \neq T(\mathbf{v})$ |
| Surjective (onto) | Every $\mathbf{w} \in W$ has some $\mathbf{v}$ with $T(\mathbf{v}) = \mathbf{w}$ |
| Isomorphism | An invertible linear transformation between vector spaces |
| Similarity transformation | $A' = P^{-1}AP$; represents the same linear map in a different basis |
| Homogeneous coordinates | Adding an extra coordinate to represent affine transformations as linear |
| Orthogonal transformation | $Q^TQ = I$, preserves lengths and angles |
| Projection | $P^2 = P$ (idempotent); maps onto a subspace |
| Reflection | $R^2 = I$; mirrors across a line or plane |

## Quick References

- [3Blue1Brown — Linear Transformations and Matrices](https://www.youtube.com/watch?v=kBw4z_cirTk) — geometric intuition for the matrix-vector product
- [MIT 18.06: Linear Transformations (Strang)](https://ocw.mit.edu/courses/mathematics/18-06-linear-algebra-spring-2010/video-lectures/lecture-30-linear-transformations-and-their-matrices/) — lecture on the matrix of a transformation
- [Scratchapixel — The Perspective and Orthographic Projection Matrix](https://www.scratchapixel.com/lessons/3d-basic-rendering/perspective-and-orthographic-projection-matrix) — graphics applications
- [Computer Graphics: Transformations (Song Ho Ahn)](https://www.songho.ca/opengl/gl_transform.html) — OpenGL transformation pipeline

## Next Steps

- [Eigenvalues & Eigenvectors](eigenvalues-and-eigenvectors.md) — vectors that are only scaled by a transformation, not rotated
- [Singular Value Decomposition](singular-value-decomposition.md) — the fundamental factorization revealing a transformation's geometry
- [Systems of Linear Equations](systems-of-linear-equations.md) — solving $A\mathbf{x} = \mathbf{b}$ for the input that produces a given output
