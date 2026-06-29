# Vectors & Transformations in Geometry

## Description

Vectors are the fundamental building blocks for describing positions, directions, velocities, and forces in space. Geometric transformations — translation, rotation, scaling, reflection, and shear — are matrix operations that move and deform objects. For developers, these concepts are the engine behind 3D rendering, animation, robot kinematics, image processing, and game physics.

## Prerequisites

- [Coordinate Geometry](coordinate-geometry.md) — Cartesian coordinates and basic geometric concepts

## Table of Contents

- [2D and 3D Vectors](#2d-and-3d-vectors)
- [Vector Operations](#vector-operations)
- [Linear Transformations](#linear-transformations)
- [Translation and Homogeneous Coordinates](#translation-and-homogeneous-coordinates)
- [Transformation Matrices in 2D](#transformation-matrices-in-2d)
- [Transformation Matrices in 3D](#transformation-matrices-in-3d)
- [Composing Transformations](#composing-transformations)
- [Inverse Transformations](#inverse-transformations)
- [Coordinate Systems and Change of Basis](#coordinate-systems-and-change-of-basis)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### 2D and 3D Vectors

A vector is a quantity with both magnitude and direction. In coordinate geometry, a vector $\mathbf{v}$ in 2D is represented as an ordered pair $(v_x, v_y)$, and in 3D as $(v_x, v_y, v_z)$.

A vector can represent:
- A **position** (a point in space, relative to the origin)
- A **direction** (velocity, normal, ray direction)
- A **displacement** (the difference between two points)

```python
# Representing vectors as tuples
v2d = (3.0, 4.0)
v3d = (1.0, 0.0, 0.0)  # Unit vector along x-axis

# Or using NumPy for efficient operations
import numpy as np
v = np.array([3.0, 4.0])
```

**Magnitude (length, norm)** of a vector:

$$
\|\mathbf{v}\| = \sqrt{v_x^2 + v_y^2 + v_z^2}
$$

**Unit vector** (direction only, magnitude = 1):

$$
\hat{\mathbf{v}} = \frac{\mathbf{v}}{\|\mathbf{v}\|}
$$

```python
def normalize(v):
    mag = math.sqrt(v[0]**2 + v[1]**2 + v[2]**2)
    if mag == 0:
        return (0, 0, 0)
    return (v[0] / mag, v[1] / mag, v[2] / mag)
```

**Zero vector** $(0, 0, 0)$ has magnitude 0 and no direction.

### Vector Operations

**Addition.** Add component-wise:

$$
\mathbf{u} + \mathbf{v} = (u_x + v_x, u_y + v_y, u_z + v_z)
$$

Geometrically, this is the diagonal of the parallelogram formed by $\mathbf{u}$ and $\mathbf{v}$.

**Subtraction.** $\mathbf{u} - \mathbf{v}$ gives the vector from $\mathbf{v}$ to $\mathbf{u}$.

**Scalar multiplication.** Multiply each component by a scalar:

$$
c \cdot \mathbf{v} = (c v_x, c v_y, c v_z)
$$

This scales the vector's magnitude by $|c|$ and reverses direction if $c < 0$.

**Dot product (scalar product):**

$$
\mathbf{u} \cdot \mathbf{v} = u_x v_x + u_y v_y + u_z v_z = \|\mathbf{u}\| \|\mathbf{v}\| \cos\theta
$$

```python
def dot(u, v):
    return u[0] * v[0] + u[1] * v[1] + u[2] * v[2]
```

The dot product is used for:
- Computing the angle between vectors: $\cos\theta = \frac{\mathbf{u} \cdot \mathbf{v}}{\|\mathbf{u}\| \|\mathbf{v}\|}$
- Projecting one vector onto another: $\text{proj}_{\mathbf{v}} \mathbf{u} = \frac{\mathbf{u} \cdot \mathbf{v}}{\|\mathbf{v}\|^2} \mathbf{v}$
- Checking orthogonality: $\mathbf{u} \cdot \mathbf{v} = 0$ means perpendicular vectors
- Lighting calculations (the dot product between surface normal and light direction)

```python
def project(u, v):
    """Project u onto v."""
    v_norm_sq = v[0]**2 + v[1]**2 + v[2]**2
    if v_norm_sq == 0:
        return (0, 0, 0)
    scalar = dot(u, v) / v_norm_sq
    return (scalar * v[0], scalar * v[1], scalar * v[2])
```

**Cross product (vector product)** — only defined in 3D:

$$
\mathbf{u} \times \mathbf{v} = (u_y v_z - u_z v_y,\ u_z v_x - u_x v_z,\ u_x v_y - u_y v_x)
$$

```python
def cross(u, v):
    return (u[1] * v[2] - u[2] * v[1],
            u[2] * v[0] - u[0] * v[2],
            u[0] * v[1] - u[1] * v[0])
```

Properties:
- Result is perpendicular to both $\mathbf{u}$ and $\mathbf{v}$
- $\|\mathbf{u} \times \mathbf{v}\| = \|\mathbf{u}\| \|\mathbf{v}\| \sin\theta$
- Direction follows the right-hand rule
- Used for computing normals, torque, and rotation axes

```python
def triangle_normal(a, b, c):
    """Compute the normal vector of a triangle."""
    u = (b[0] - a[0], b[1] - a[1], b[2] - a[2])
    v = (c[0] - a[0], c[1] - a[1], c[2] - a[2])
    return normalize(cross(u, v))
```

### Linear Transformations

A transformation $T$ is linear if it satisfies:

$$
T(\mathbf{u} + \mathbf{v}) = T(\mathbf{u}) + T(\mathbf{v})
$$

$$
T(c\mathbf{v}) = c T(\mathbf{v})
$$

Every linear transformation in $\mathbb{R}^n$ can be represented as matrix multiplication:

$$
T(\mathbf{v}) = M \mathbf{v}
$$

**Basic linear transformations:**

- **Scaling:** Stretches or shrinks along axes
- **Rotation:** Rotates around the origin
- **Reflection:** Mirrors across a line (2D) or plane (3D)
- **Shear:** Slants the object along an axis
- **Projection:** Flattens onto a lower-dimensional subspace

**Non-linear transformations** include translation (which we address with homogeneous coordinates) and perspective projection.

### Translation and Homogeneous Coordinates

Translation $\mathbf{v} \to \mathbf{v} + \mathbf{t}$ is not a linear transformation because $T(\mathbf{0}) \neq \mathbf{0}$. To represent all transformations uniformly, we use **homogeneous coordinates**.

A 2D point $(x, y)$ becomes $(x, y, 1)$ in homogeneous coordinates. A 3D point $(x, y, z)$ becomes $(x, y, z, 1)$.

The transformation becomes a single $3 \times 3$ matrix in 2D or $4 \times 4$ in 3D:

$$
\begin{bmatrix}
x' \\
y' \\
w'
\end{bmatrix}
=
\begin{bmatrix}
a & b & t_x \\
c & d & t_y \\
0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
1
\end{bmatrix}
$$

The resulting Cartesian coordinates are $(x'/w', y'/w')$. When $w' \neq 1$, this enables perspective projection.

```python
def apply_transform(matrix, point):
    """Apply a 4x4 transformation matrix to a 3D point."""
    x, y, z = point
    # Homogeneous coordinates
    v = np.array([x, y, z, 1.0])
    result = matrix @ v
    # Perspective divide
    return (result[0] / result[3],
            result[1] / result[3],
            result[2] / result[3])
```

### Transformation Matrices in 2D

**Translation:**

$$
T(t_x, t_y) =
\begin{bmatrix}
1 & 0 & t_x \\
0 & 1 & t_y \\
0 & 0 & 1
\end{bmatrix}
$$

**Scaling about origin:**

$$
S(s_x, s_y) =
\begin{bmatrix}
s_x & 0 & 0 \\
0 & s_y & 0 \\
0 & 0 & 1
\end{bmatrix}
$$

**Rotation about origin (counterclockwise by $\theta$):**

$$
R(\theta) =
\begin{bmatrix}
\cos\theta & -\sin\theta & 0 \\
\sin\theta & \cos\theta & 0 \\
0 & 0 & 1
\end{bmatrix}
$$

```python
def rotation_matrix_2d(theta):
    c = math.cos(theta)
    s = math.sin(theta)
    return np.array([
        [c, -s, 0],
        [s,  c, 0],
        [0,  0, 1]
    ])
```

**Reflection across the $x$-axis:**

$$
\begin{bmatrix}
1 & 0 & 0 \\
0 & -1 & 0 \\
0 & 0 & 1
\end{bmatrix}
$$

**Shear (horizontal):**

$$
H(h) =
\begin{bmatrix}
1 & h & 0 \\
0 & 1 & 0 \\
0 & 0 & 1
\end{bmatrix}
$$

### Transformation Matrices in 3D

**Translation in 3D:**

$$
T(t_x, t_y, t_z) =
\begin{bmatrix}
1 & 0 & 0 & t_x \\
0 & 1 & 0 & t_y \\
0 & 0 & 1 & t_z \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

**Scaling in 3D:**

$$
S(s_x, s_y, s_z) =
\begin{bmatrix}
s_x & 0 & 0 & 0 \\
0 & s_y & 0 & 0 \\
0 & 0 & s_z & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

**Rotation around $x$-axis (roll):**

$$
R_x(\theta) =
\begin{bmatrix}
1 & 0 & 0 & 0 \\
0 & \cos\theta & -\sin\theta & 0 \\
0 & \sin\theta & \cos\theta & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

**Rotation around $y$-axis (yaw):**

$$
R_y(\theta) =
\begin{bmatrix}
\cos\theta & 0 & \sin\theta & 0 \\
0 & 1 & 0 & 0 \\
-\sin\theta & 0 & \cos\theta & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

**Rotation around $z$-axis (pitch):**

$$
R_z(\theta) =
\begin{bmatrix}
\cos\theta & -\sin\theta & 0 & 0 \\
\sin\theta & \cos\theta & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

```python
def rotation_matrix_3d(axis, angle):
    """Rodrigues' rotation formula."""
    c = math.cos(angle)
    s = math.sin(angle)
    t = 1 - c
    x, y, z = normalize(axis)
    return np.array([
        [t*x*x + c,   t*x*y - s*z, t*x*z + s*y, 0],
        [t*x*y + s*z, t*y*y + c,   t*y*z - s*x, 0],
        [t*x*z - s*y, t*y*z + s*x, t*z*z + c,   0],
        [0,           0,           0,           1]
    ])
```

### Composing Transformations

Transformations are composed by multiplying their matrices. The order matters: matrix multiplication is not commutative.

If you want to scale an object by $S$, then rotate by $R$, then translate by $T$, the combined matrix is:

$$
M = T \cdot R \cdot S
$$

Apply this to a point $\mathbf{v}$: $\mathbf{v}' = M \mathbf{v} = T(R(S(\mathbf{v})))$.

The transformations are applied from right to left: first scale, then rotate, then translate.

```python
def compose_transforms(*matrices):
    """Multiply matrices in order: last * ... * first."""
    result = np.eye(4)
    for m in reversed(matrices):
        result = result @ m
    return result
```

**Pivot point rotation.** To rotate around an arbitrary point $\mathbf{p}$ (not the origin):

$$
M = T(\mathbf{p}) \cdot R(\theta) \cdot T(-\mathbf{p})
$$

First translate so $\mathbf{p}$ is at the origin, rotate, then translate back.

```python
def rotate_around_point(angle, pivot):
    """2D rotation matrix for rotation around an arbitrary point."""
    T_to_origin = np.array([
        [1, 0, -pivot[0]],
        [0, 1, -pivot[1]],
        [0, 0, 1]
    ])
    R = rotation_matrix_2d(angle)
    T_back = np.array([
        [1, 0, pivot[0]],
        [0, 1, pivot[1]],
        [0, 0, 1]
    ])
    return T_back @ R @ T_to_origin
```

**Transformation order in graphics pipelines:**

In a typical 3D engine, the model matrix is:

$$
M_{\text{model}} = T \cdot R \cdot S
$$

The view matrix (camera) typically inverts this: $M_{\text{view}} \approx (T_{\text{camera}} \cdot R_{\text{camera}})^{-1}$.

The full transformation pipeline is:

$$
\mathbf{v}_{\text{screen}} = M_{\text{viewport}} \cdot M_{\text{projection}} \cdot M_{\text{view}} \cdot M_{\text{model}} \cdot \mathbf{v}_{\text{local}}
$$

### Inverse Transformations

The inverse of a transformation undoes it. If $T(\mathbf{v}) = M\mathbf{v}$, then $T^{-1}(\mathbf{v}) = M^{-1}\mathbf{v}$.

**Inverse of translation:**

$$
T^{-1}(t_x, t_y, t_z) = T(-t_x, -t_y, -t_z)
$$

**Inverse of scaling:**

$$
S^{-1}(s_x, s_y, s_z) = S(1/s_x, 1/s_y, 1/s_z)
$$

**Inverse of rotation:**

$$
R^{-1}(\theta) = R(-\theta) = R(\theta)^T
$$

The inverse of a rotation matrix is its transpose (rotation matrices are orthogonal).

**Inverse of a composition:**

$$
(T \cdot R \cdot S)^{-1} = S^{-1} \cdot R^{-1} \cdot T^{-1}
$$

```python
def inverse_transform(M):
    """Compute the inverse of a 4x4 transformation matrix."""
    # For affine transforms, the inverse has a closed form
    # Extract rotation/scale and translation
    R = M[:3, :3]
    t = M[:3, 3]

    R_inv = np.linalg.inv(R)
    t_inv = -R_inv @ t

    result = np.eye(4)
    result[:3, :3] = R_inv
    result[:3, 3] = t_inv
    return result
```

**Uses of inverse transformations:**

- **Camera view matrix:** Transform world points into camera space (inverse of camera's transform)
- **Normal transformation:** Transform normals using the inverse transpose of the model matrix: $N = (M^{-1})^T$
- **Unprojection:** Convert screen coordinates back to world space
- **IK (inverse kinematics):** Compute joint angles from end-effector position

```python
def transform_normal(M, normal):
    """Transform a normal vector using the inverse transpose."""
    R = M[:3, :3]
    R_inv_T = np.linalg.inv(R).T
    return normalize(R_inv_T @ normal)
```

### Coordinate Systems and Change of Basis

A coordinate system is defined by an origin and a set of basis vectors. Changing basis means expressing a vector in a new coordinate system.

Given a vector $\mathbf{v}$ expressed in basis $B = \{\mathbf{b}_1, \mathbf{b}_2, \mathbf{b}_3\}$, to express it in the standard basis $E$:

$$
\mathbf{v}_E = B \cdot \mathbf{v}_B
$$

where $B$ is the matrix whose columns are the basis vectors $\mathbf{b}_1, \mathbf{b}_2, \mathbf{b}_3$.

To convert from standard basis to basis $B$:

$$
\mathbf{v}_B = B^{-1} \cdot \mathbf{v}_E
$$

```python
def to_basis(v, basis):
    """Convert vector v from standard basis to given basis."""
    basis_matrix = np.column_stack(basis)  # basis vectors as columns
    return np.linalg.solve(basis_matrix, v)

def from_basis(v, basis):
    """Convert vector v from given basis to standard basis."""
    basis_matrix = np.column_stack(basis)
    return basis_matrix @ v
```

**World space to local space.** In 3D graphics, each object has its own local coordinate system. To convert a point from local to world space, multiply by the object's model matrix. To go from world to local, multiply by the inverse.

**Tangent space (normal mapping).** Normal maps store normals in tangent space, a coordinate system aligned to the surface. Converting to world space requires the TBN (tangent, bitangent, normal) matrix:

```python
def tbn_matrix(tangent, bitangent, normal):
    """Build a TBN matrix for normal mapping."""
    return np.column_stack([tangent, bitangent, normal])
```

**Coordinate systems in robotics.** A robot arm consists of links connected by joints. Each link has its own coordinate frame. The transformation from the end-effector to the base is the product of all joint transforms:

$$
T_{\text{base}}^{\text{end}} = T_1 \cdot T_2 \cdot \cdots \cdot T_n
$$

```python
def forward_kinematics(joint_angles, link_lengths):
    """Compute end-effector position from joint angles (2D arm)."""
    x, y = 0.0, 0.0
    angle = 0.0
    for theta, length in zip(joint_angles, link_lengths):
        angle += theta
        x += length * math.cos(angle)
        y += length * math.sin(angle)
    return (x, y)
```

**Image processing coordinate transforms:**

- **Flip (reflection):** Mirrors an image horizontally or vertically
- **Rotate:** Rotates the image by a given angle
- **Affine transform:** A $2 \times 3$ matrix combining linear transformation and translation
- **Perspective transform:** A $3 \times 3$ matrix for perspective warping

```python
import numpy as np

def affine_transform(image, matrix):
    """Apply an affine transformation to an image.
    matrix is a 2x3 transformation matrix.
    """
    h, w = image.shape[:2]
    # Create the transformation
    transform_matrix = np.vstack([matrix, [0, 0, 1]])
    # Apply inverse mapping
    inv_matrix = np.linalg.inv(transform_matrix)[:2]
    # Use the transformation (in practice, cv2.warpAffine or PIL)
    return inv_matrix
```

## Study Cases

### Case 1: 3D Camera Orbit (Arcball)

A camera orbits around a target point $(0, 0, 0)$ at distance $d$. The user moves the mouse to change the camera's spherical coordinates $(\theta, \phi)$.

Given the orbiting angles, compute the view matrix:

```python
def orbit_view_matrix(theta, phi, distance, target):
    # Camera position in spherical coordinates
    x = distance * math.sin(phi) * math.cos(theta)
    y = distance * math.cos(phi)
    z = distance * math.sin(phi) * math.sin(theta)

    camera_pos = np.array([x, y, z])
    up = np.array([0, 1, 0])

    # Compute the view matrix using look-at
    forward = normalize(target - camera_pos)
    right = normalize(cross(forward, up))
    up = cross(right, forward)

    view = np.eye(4)
    view[:3, :3] = np.vstack([right, up, -forward]).T
    view[:3, 3] = -view[:3, :3] @ camera_pos
    return view
```

This is the standard orbit camera used in 3D editors, CAD tools, and game engines.

### Case 2: 2D Platformer Camera Following a Player

The camera follows the player with a smooth interpolation (lerp). The player is at position $(p_x, p_y)$.

```python
def smooth_follow(player_pos, camera_pos, follow_speed, dt):
    target_x, target_y = player_pos
    cam_x, cam_y = camera_pos
    # Lerp camera toward player
    new_x = cam_x + (target_x - cam_x) * follow_speed * dt
    new_y = cam_y + (target_y - cam_y) * follow_speed * dt
    return (new_x, new_y)
```

The view matrix translates everything by the negative camera position:

$$
M_{\text{view}} =
\begin{bmatrix}
1 & 0 & -c_x + w/2 \\
0 & 1 & -c_y + h/2 \\
0 & 0 & 1
\end{bmatrix}
$$

### Case 3: Robot Arm Forward Kinematics

A 3-link planar robot arm has link lengths $l_1 = 3$, $l_2 = 2$, $l_3 = 1$. Joint angles are $\theta_1 = 30^\circ$, $\theta_2 = 45^\circ$, $\theta_3 = -60^\circ$.

```python
import math

def planar_arm_fk(theta1, theta2, theta3, l1, l2, l3):
    # Base to joint 1
    T0 = np.eye(4)
    T0[:3, :3] = rotation_matrix_3d((0, 0, 1), theta1)

    # Joint 1 to joint 2
    T1 = np.eye(4)
    T1[0, 3] = l1  # Translate along x
    T1[:3, :3] = rotation_matrix_3d((0, 0, 1), theta2)

    # Joint 2 to joint 3
    T2 = np.eye(4)
    T2[0, 3] = l2
    T2[:3, :3] = rotation_matrix_3d((0, 0, 1), theta3)

    # End-effector
    T3 = np.eye(4)
    T3[0, 3] = l3

    # Full chain
    T = T0 @ T1 @ T2 @ T3
    return T[:3, 3]  # Position of end-effector

end_effector = planar_arm_fk(
    math.radians(30), math.radians(45),
    math.radians(-60), 3, 2, 1
)
print(f"End-effector position: {end_effector}")
```

## Examples

### Example 1: Vector Math Utility Library

```python
import math

class Vector3:
    def __init__(self, x, y, z):
        self.x, self.y, self.z = x, y, z

    def __add__(self, other):
        return Vector3(self.x + other.x, self.y + other.y, self.z + other.z)

    def __sub__(self, other):
        return Vector3(self.x - other.x, self.y - other.y, self.z - other.z)

    def __mul__(self, scalar):
        return Vector3(self.x * scalar, self.y * scalar, self.z * scalar)

    def dot(self, other):
        return self.x * other.x + self.y * other.y + self.z * other.z

    def cross(self, other):
        return Vector3(
            self.y * other.z - self.z * other.y,
            self.z * other.x - self.x * other.z,
            self.x * other.y - self.y * other.x
        )

    def magnitude(self):
        return math.sqrt(self.x**2 + self.y**2 + self.z**2)

    def normalize(self):
        m = self.magnitude()
        return Vector3(self.x/m, self.y/m, self.z/m) if m > 0 else Vector3(0, 0, 0)
```

### Example 2: Reflection Vector (Billiard Ball Bounce)

When a ball hits a wall with normal $\mathbf{n}$, the reflected velocity is:

$$
\mathbf{v}_{\text{reflect}} = \mathbf{v} - 2(\mathbf{v} \cdot \mathbf{n})\mathbf{n}
$$

```python
def reflect(v, normal):
    n = normalize(normal)
    dot_vn = v[0]*n[0] + v[1]*n[1] + v[2]*n[2]
    return (v[0] - 2*dot_vn*n[0],
            v[1] - 2*dot_vn*n[1],
            v[2] - 2*dot_vn*n[2])
```

### Example 3: Rotating a 2D Point Around the Origin

```python
def rotate_2d(point, angle):
    x, y = point
    c = math.cos(angle)
    s = math.sin(angle)
    return (x * c - y * s, x * s + y * c)
```

### Example 4: Image Rotation Using Affine Transform

```python
def image_rotate_points(points, angle, center):
    """Rotate a list of (x, y) points around center by angle degrees."""
    theta = math.radians(angle)
    c, s = math.cos(theta), math.sin(theta)
    cx, cy = center

    result = []
    for x, y in points:
        # Translate to origin, rotate, translate back
        dx = x - cx
        dy = y - cy
        rx = dx * c - dy * s + cx
        ry = dx * s + dy * c + cy
        result.append((rx, ry))
    return result
```

### Example 5: Gram-Schmidt Orthonormalization

```python
def gram_schmidt(vectors):
    """Orthonormalize a set of vectors using Gram-Schmidt process."""
    basis = []
    for v in vectors:
        w = list(v)
        for b in basis:
            # Subtract projection onto b
            proj = dot(v, b)
            for i in range(len(w)):
                w[i] -= proj * b[i]
        mag = math.sqrt(sum(x*x for x in w))
        if mag > 1e-10:
            basis.append(tuple(x/mag for x in w))
    return basis
```

## Glossary

| Term | Definition |
|------|------------|
| Affine transformation | A transformation that preserves points, straight lines, and planes (linear + translation) |
| Basis | A set of linearly independent vectors that span a vector space |
| Cross product | A binary operation on two vectors in 3D producing a vector perpendicular to both |
| Determinant | A scalar value that represents the scaling factor of a linear transformation |
| Dot product | A scalar operation on two vectors yielding $\|\mathbf{u}\|\|\mathbf{v}\|\cos\theta$ |
| Homogeneous coordinates | Coordinates with an extra dimension enabling translation as matrix multiplication |
| Linear transformation | A mapping $T$ satisfying $T(\mathbf{u}+\mathbf{v}) = T(\mathbf{u})+T(\mathbf{v})$ and $T(c\mathbf{v}) = cT(\mathbf{v})$ |
| Normal vector | A vector perpendicular to a surface |
| Orthogonal matrix | A matrix whose rows/columns are orthonormal vectors; $Q^T = Q^{-1}$ |
| Perspective divide | Division by the $w$ component to convert from homogeneous to Cartesian coordinates |
| Quaternion | A 4D complex number used for rotation that avoids gimbal lock |
| Right-hand rule | Convention for determining the direction of the cross product and coordinate axes |
| Rodrigues' formula | Formula for rotating a vector around an arbitrary axis |
| Singular value decomposition (SVD) | Factorization of a matrix into rotation, scaling, and rotation components |
| Tangent space | A coordinate system local to a surface point, used in normal mapping |
| Transformation matrix | A matrix that represents a geometric transformation when multiplied with coordinates |
| Vector | A quantity with magnitude and direction, represented as an ordered tuple |
| View matrix | The matrix that transforms world coordinates to camera-relative coordinates |

## Quick References

- [3Blue1Brown: Linear Algebra](https://www.3blue1brown.com/topics/linear-algebra) — visual intuition for vectors and transformations
- [LearnOpenGL: Coordinate Systems](https://learnopengl.com/Getting-started/Coordinate-Systems) — practical graphics pipeline
- [Scratchapixel: Geometry](https://www.scratchapixel.com/lessons/mathematics-physics-for-computer-graphics/geometry) — geometry for computer graphics
- [Real-Time Rendering: Transformations](https://www.realtimerendering.com/) — comprehensive reference
- [NumPy Linear Algebra Guide](https://numpy.org/doc/stable/reference/routines.linalg.html) — Python implementation reference
- [Khan Academy: Linear Transformations](https://www.khanacademy.org/math/linear-algebra/matrix-transformations) — interactive lessons

## Next Steps

- [Analytic Geometry](analytic-geometry.md) — parametric curves, planes, quadric surfaces, and splines
- [Computational Geometry](computational-geometry.md) — algorithms for geometric problems
