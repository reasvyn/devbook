# Analytic Geometry in 2D and 3D

## Description

Analytic geometry uses algebraic equations to describe geometric objects and their relationships. This document covers parametric equations of lines and curves, equations of planes in 3D, distances, intersections, quadric surfaces, curve parametrization, and splines. For developers, these tools are essential for CAD software, 3D modeling, curve fitting, collision detection, and path planning.

## Prerequisites

- [Vectors & Transformations](vectors-and-transformations.md) — vector operations, dot/cross products, coordinate systems

## Table of Contents

- [Parametric Equations of Lines](#parametric-equations-of-lines)
- [Equations of Planes in 3D](#equations-of-planes-in-3d)
- [Distance Computations](#distance-computations)
- [Intersection of Lines and Planes](#intersection-of-lines-and-planes)
- [Quadric Surfaces](#quadric-surfaces)
- [Curve Parametrization](#curve-parametrization)
- [Splines and Bezier Curves](#splines-and-bezier-curves)
- [B-Splines and NURBS](#b-splines-and-nurbs)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Parametric Equations of Lines

A line in 2D or 3D can be expressed parametrically as:

$$
\mathbf{p}(t) = \mathbf{p}_0 + t \mathbf{d}
$$

where $\mathbf{p}_0$ is a point on the line, $\mathbf{d}$ is the direction vector, and $t \in \mathbb{R}$ is the parameter.

In 2D:

$$
\begin{aligned}
x(t) &= x_0 + t d_x \\
y(t) &= y_0 + t d_y
\end{aligned}
$$

In 3D:

$$
\begin{aligned}
x(t) &= x_0 + t a \\
y(t) &= y_0 + t b \\
z(t) &= z_0 + t c
\end{aligned}
$$

**Line segment.** For $t \in [0, 1]$, $\mathbf{p}(t)$ traces the segment from $\mathbf{p}_0$ to $\mathbf{p}_0 + \mathbf{d}$.

**Ray.** For $t \geq 0$, $\mathbf{p}(t)$ is a ray starting at $\mathbf{p}_0$ and extending in direction $\mathbf{d}$.

```python
def point_on_line(p0, direction, t):
    return (p0[0] + t * direction[0],
            p0[1] + t * direction[1],
            p0[2] + t * direction[2])
```

**Two-point form.** Given two points $\mathbf{p}_1$ and $\mathbf{p}_2$:

$$
\mathbf{p}(t) = \mathbf{p}_1 + t(\mathbf{p}_2 - \mathbf{p}_1)
$$

Here $\mathbf{p}(0) = \mathbf{p}_1$ and $\mathbf{p}(1) = \mathbf{p}_2$.

**Symmetric form.** Eliminating $t$ from the parametric equations gives:

$$
\frac{x - x_0}{a} = \frac{y - y_0}{b} = \frac{z - z_0}{c}
$$

This only works when all direction components are non-zero.

**Distance from a point to a line.** Given point $\mathbf{q}$ and line $\mathbf{p}(t) = \mathbf{p}_0 + t\mathbf{d}$:

$$
d = \frac{\|(\mathbf{q} - \mathbf{p}_0) \times \mathbf{d}\|}{\|\mathbf{d}\|}
$$

```python
def point_to_line_distance(q, p0, direction):
    v = (q[0] - p0[0], q[1] - p0[1], q[2] - p0[2])
    cross_vd = cross(v, direction)
    mag_cross = math.sqrt(cross_vd[0]**2 + cross_vd[1]**2 + cross_vd[2]**2)
    mag_d = math.sqrt(direction[0]**2 + direction[1]**2 + direction[2]**2)
    return mag_cross / mag_d
```

The 2D version uses the determinant formula:

$$
d = \frac{|(y_2 - y_1)x_0 - (x_2 - x_1)y_0 + x_2 y_1 - y_2 x_1|}{\sqrt{(y_2 - y_1)^2 + (x_2 - x_1)^2}}
$$

### Equations of Planes in 3D

A plane is a flat 2D surface in 3D space. It can be described in several ways.

**Point-normal form.** Given a point $\mathbf{p}_0$ on the plane and a normal vector $\mathbf{n} = (A, B, C)$ perpendicular to the plane:

$$
\mathbf{n} \cdot (\mathbf{p} - \mathbf{p}_0) = 0
$$

**General (scalar) form:**

$$
Ax + By + Cz + D = 0
$$

where $D = -\mathbf{n} \cdot \mathbf{p}_0$.

```python
def plane_from_point_normal(point, normal):
    A, B, C = normal
    D = -(A * point[0] + B * point[1] + C * point[2])
    return (A, B, C, D)
```

**Three-point form.** Given three non-collinear points $\mathbf{p}_1, \mathbf{p}_2, \mathbf{p}_3$:

$$
\mathbf{n} = (\mathbf{p}_2 - \mathbf{p}_1) \times (\mathbf{p}_3 - \mathbf{p}_1)
$$

Then use the point-normal form with $\mathbf{p}_0 = \mathbf{p}_1$.

```python
def plane_from_points(p1, p2, p3):
    u = (p2[0] - p1[0], p2[1] - p1[1], p2[2] - p1[2])
    v = (p3[0] - p1[0], p3[1] - p1[1], p3[2] - p1[2])
    normal = cross(u, v)
    return plane_from_point_normal(p1, normal)
```

**Parametric form of a plane:**

$$
\mathbf{p}(s, t) = \mathbf{p}_0 + s\mathbf{u} + t\mathbf{v}
$$

where $\mathbf{u}$ and $\mathbf{v}$ are two non-parallel vectors lying in the plane.

```python
def point_on_plane(p0, u, v, s, t):
    return (p0[0] + s * u[0] + t * v[0],
            p0[1] + s * u[1] + t * v[1],
            p0[2] + s * u[2] + t * v[2])
```

**Half-spaces.** A plane divides 3D space into two half-spaces:

- Positive half-space: $Ax + By + Cz + D > 0$
- Negative half-space: $Ax + By + Cz + D < 0$

```python
def side_of_plane(point, plane):
    A, B, C, D = plane
    return A * point[0] + B * point[1] + C * point[2] + D
```

This is used in frustum culling (testing if a point is inside the view frustum) and collision detection.

### Distance Computations

**Distance from a point to a plane.** Given point $\mathbf{q}$ and plane $Ax + By + Cz + D = 0$:

$$
d = \frac{|A q_x + B q_y + C q_z + D|}{\sqrt{A^2 + B^2 + C^2}}
$$

```python
def point_to_plane_distance(point, plane):
    A, B, C, D = plane
    numerator = abs(A * point[0] + B * point[1] + C * point[2] + D)
    denominator = math.sqrt(A*A + B*B + C*C)
    return numerator / denominator
```

**Distance from a point to a line segment.** Find the closest point on the segment $\mathbf{p}_1\mathbf{p}_2$ to $\mathbf{q}$:

$$
t = \frac{(\mathbf{q} - \mathbf{p}_1) \cdot (\mathbf{p}_2 - \mathbf{p}_1)}{\|\mathbf{p}_2 - \mathbf{p}_1\|^2}
$$

Clamp $t$ to $[0, 1]$. The closest point is $\mathbf{p}_1 + t(\mathbf{p}_2 - \mathbf{p}_1)$.

```python
def closest_point_on_segment(q, p1, p2):
    ab = (p2[0] - p1[0], p2[1] - p1[1], p2[2] - p1[2])
    aq = (q[0] - p1[0], q[1] - p1[1], q[2] - p1[2])
    t = dot(aq, ab) / dot(ab, ab)
    t = max(0, min(1, t))
    return (p1[0] + t * ab[0],
            p1[1] + t * ab[1],
            p1[2] + t * ab[2])
```

**Distance between two lines (skew lines).** Given lines $\mathbf{p}_1 + t\mathbf{d}_1$ and $\mathbf{p}_2 + s\mathbf{d}_2$:

$$
d = \frac{|(\mathbf{p}_2 - \mathbf{p}_1) \cdot (\mathbf{d}_1 \times \mathbf{d}_2)|}{\|\mathbf{d}_1 \times \mathbf{d}_2\|}
```

If $\mathbf{d}_1 \times \mathbf{d}_2 = \mathbf{0}$, the lines are parallel, and the distance is the distance from $\mathbf{p}_2$ to the first line.

**Distance between two planes.** Two parallel planes $A x + B y + C z + D_1 = 0$ and $A x + B y + C z + D_2 = 0$:

$$
d = \frac{|D_2 - D_1|}{\sqrt{A^2 + B^2 + C^2}}
$$

Non-parallel planes intersect along a line.

### Intersection of Lines and Planes

**Ray-plane intersection.** Given ray $\mathbf{p}(t) = \mathbf{p}_0 + t\mathbf{d}$ and plane $\mathbf{n} \cdot (\mathbf{p} - \mathbf{p}_{\text{plane}}) = 0$:

$$
t = \frac{\mathbf{n} \cdot (\mathbf{p}_{\text{plane}} - \mathbf{p}_0)}{\mathbf{n} \cdot \mathbf{d}}
$$

If $\mathbf{n} \cdot \mathbf{d} = 0$, the ray is parallel to the plane (no intersection or lies in the plane).

```python
def ray_plane_intersection(ray_origin, ray_dir, plane):
    A, B, C, D = plane
    denom = A * ray_dir[0] + B * ray_dir[1] + C * ray_dir[2]
    if abs(denom) < 1e-10:
        return None  # Parallel
    t = -(A * ray_origin[0] + B * ray_origin[1] + C * ray_origin[2] + D) / denom
    if t < 0:
        return None  # Behind ray origin
    return (ray_origin[0] + t * ray_dir[0],
            ray_origin[1] + t * ray_dir[1],
            ray_origin[2] + t * ray_dir[2])
```

**Line-plane intersection.** Same formula as ray-plane, but $t$ can be any real value.

**Intersection of two planes.** Two non-parallel planes intersect in a line. The line direction is $\mathbf{d} = \mathbf{n}_1 \times \mathbf{n}_2$. Find a point on the line by solving the system of two plane equations (set one coordinate to zero).

```python
def plane_plane_intersection(plane1, plane2):
    A1, B1, C1, D1 = plane1
    A2, B2, C2, D2 = plane2
    # Direction of intersection line
    d = cross((A1, B1, C1), (A2, B2, C2))
    if all(abs(x) < 1e-10 for x in d):
        return None  # Parallel planes
    # Find a point on the line (set z=0)
    det = A1 * B2 - A2 * B1
    if abs(det) > 1e-10:
        x0 = (B1 * D2 - B2 * D1) / det
        y0 = (A2 * D1 - A1 * D2) / det
        return ((x0, y0, 0), normalize(d))
    # Try setting x=0 or y=0
    det = B1 * C2 - B2 * C1
    if abs(det) > 1e-10:
        y0 = (C1 * D2 - C2 * D1) / det
        z0 = (B2 * D1 - B1 * D2) / det
        return ((0, y0, z0), normalize(d))
    return None
```

**Intersection of three planes.** Solving a $3 \times 3$ linear system gives the point where three planes meet. This is the core of solving for 3D trilateration (GPS positioning).

**Ray-triangle intersection.** The most common operation in 3D graphics. Using the Moller-Trumbore algorithm:

```python
def ray_triangle_intersection(ray_origin, ray_dir, v0, v1, v2):
    edge1 = (v1[0] - v0[0], v1[1] - v0[1], v1[2] - v0[2])
    edge2 = (v2[0] - v0[0], v2[1] - v0[1], v2[2] - v0[2])
    pvec = cross(ray_dir, edge2)
    det = dot(edge1, pvec)
    if abs(det) < 1e-10:
        return None
    inv_det = 1.0 / det
    tvec = (ray_origin[0] - v0[0], ray_origin[1] - v0[1], ray_origin[2] - v0[2])
    u = dot(tvec, pvec) * inv_det
    if u < 0 or u > 1:
        return None
    qvec = cross(tvec, edge1)
    v = dot(ray_dir, qvec) * inv_det
    if v < 0 or u + v > 1:
        return None
    t = dot(edge2, qvec) * inv_det
    if t < 0:
        return None
    return (ray_origin[0] + t * ray_dir[0],
            ray_origin[1] + t * ray_dir[1],
            ray_origin[2] + t * ray_dir[2],
            u, v)
```

### Quadric Surfaces

Quadric surfaces are the 3D generalization of conic sections. They are defined by the general second-degree equation in $x, y, z$:

$$
Ax^2 + By^2 + Cz^2 + Dxy + Exz + Fyz + Gx + Hy + Iz + J = 0
$$

**Ellipsoid:**

$$
\frac{x^2}{a^2} + \frac{y^2}{b^2} + \frac{z^2}{c^2} = 1
$$

A sphere is the special case $a = b = c = r$.

```python
def ellipsoid_contains(point, a, b, c):
    x, y, z = point
    return (x/a)**2 + (y/b)**2 + (z/c)**2 <= 1.0
```

**Hyperboloid of one sheet:**

$$
\frac{x^2}{a^2} + \frac{y^2}{b^2} - \frac{z^2}{c^2} = 1
$$

Used in cooling tower design and telescope mirrors.

**Hyperboloid of two sheets:**

$$
\frac{x^2}{a^2} - \frac{y^2}{b^2} - \frac{z^2}{c^2} = 1
$$

**Paraboloid (elliptic):**

$$
z = \frac{x^2}{a^2} + \frac{y^2}{b^2}
$$

Used in satellite dishes and parabolic microphones.

**Hyperbolic paraboloid (saddle):**

$$
z = \frac{x^2}{a^2} - \frac{y^2}{b^2}
$$

A saddle surface, appearing in architectural roofs and optimization landscapes in machine learning.

**Cone:**

$$
\frac{x^2}{a^2} + \frac{y^2}{b^2} - \frac{z^2}{c^2} = 0
$$

**Cylinder:**

$$
\frac{x^2}{a^2} + \frac{y^2}{b^2} = 1
$$

The generating line is parallel to the $z$-axis.

**Ray-quadric intersection.** Substitute the parametric ray equations into the quadric equation and solve the resulting quadratic in $t$:

```python
def ray_sphere_intersection(ray_origin, ray_dir, center, radius):
    oc = (ray_origin[0] - center[0],
          ray_origin[1] - center[1],
          ray_origin[2] - center[2])
    a = dot(ray_dir, ray_dir)
    b = 2 * dot(oc, ray_dir)
    c = dot(oc, oc) - radius * radius
    disc = b * b - 4 * a * c
    if disc < 0:
        return None
    t = (-b - math.sqrt(disc)) / (2 * a)
    if t < 0:
        t = (-b + math.sqrt(disc)) / (2 * a)
    if t < 0:
        return None
    return (ray_origin[0] + t * ray_dir[0],
            ray_origin[1] + t * ray_dir[1],
            ray_origin[2] + t * ray_dir[2])
```

### Curve Parametrization

A parametric curve in 2D or 3D is a function $\mathbf{C}(t) = (x(t), y(t), z(t))$ for $t \in [a, b]$.

**Arc length.** The length of a curve from $t=a$ to $t=b$:

$$
L = \int_a^b \|\mathbf{C}'(t)\| \, dt = \int_a^b \sqrt{x'(t)^2 + y'(t)^2 + z'(t)^2} \, dt
$$

```python
def arc_length(C, a, b, n=1000):
    """Approximate arc length using numerical integration."""
    dt = (b - a) / n
    length = 0.0
    for i in range(n):
        t = a + i * dt
        # Derivative at t (finite difference)
        eps = 1e-6
        p1 = C(t)
        p2 = C(t + eps)
        ds = math.sqrt((p2[0]-p1[0])**2 + (p2[1]-p1[1])**2 + (p2[2]-p1[2])**2)
        length += ds / eps * dt
    return length
```

**Reparametrization by arc length.** For uniform speed along a curve, reparametrize so the parameter equals arc length.

**Common parametric curves:**

- **Circle:** $x(t) = R\cos t,\ y(t) = R\sin t,\ t \in [0, 2\pi)$
- **Ellipse:** $x(t) = a\cos t,\ y(t) = b\sin t$
- **Helix:** $x(t) = R\cos t,\ y(t) = R\sin t,\ z(t) = ct$
- **Cycloid:** $x(t) = R(t - \sin t),\ y(t) = R(1 - \cos t)$
- **Lissajous curve:** $x(t) = A\sin(at + \delta),\ y(t) = B\sin(bt)$

**Tangent and normal vectors:**

- Tangent: $\mathbf{T}(t) = \frac{\mathbf{C}'(t)}{\|\mathbf{C}'(t)\|}$
- Normal: $\mathbf{N}(t) = \frac{\mathbf{T}'(t)}{\|\mathbf{T}'(t)\|}$
- Binormal: $\mathbf{B}(t) = \mathbf{T}(t) \times \mathbf{N}(t)$

The Frenet-Serret frame $(\mathbf{T}, \mathbf{N}, \mathbf{B})$ defines an orthonormal coordinate system along the curve, used in path following for robotics and animation.

```python
def frenet_frame(C, t):
    eps = 1e-6
    p0 = C(t - eps)
    p1 = C(t + eps)
    # First derivative (central difference)
    d1 = ((p1[0] - p0[0]) / (2*eps),
          (p1[1] - p0[1]) / (2*eps),
          (p1[2] - p0[2]) / (2*eps))
    T = normalize(d1)
    # Second derivative
    p2 = C(t + 2*eps)  # Approximate
    p_m1 = C(t - 2*eps)
    d2 = ((p2[0] - 2*p1[0] + p0[0]) / (eps*eps),
          (p2[1] - 2*p1[1] + p0[1]) / (eps*eps),
          (p2[2] - 2*p1[2] + p0[2]) / (eps*eps))
    N = normalize(d2 - dot(d2, T) * T)
    B = cross(T, N)
    return (T, N, B)
```

### Splines and Bezier Curves

Splines are piecewise polynomial curves used extensively in CAD, vector graphics, and animation.

**Linear interpolation (degree 1):**

$$
\mathbf{B}(t) = (1 - t)\mathbf{P}_0 + t\mathbf{P}_1
$$

**Quadratic Bezier (degree 2):**

$$
\mathbf{B}(t) = (1-t)^2 \mathbf{P}_0 + 2(1-t)t \mathbf{P}_1 + t^2 \mathbf{P}_2
$$

**Cubic Bezier (degree 3):**

$$
\mathbf{B}(t) = (1-t)^3 \mathbf{P}_0 + 3(1-t)^2 t \mathbf{P}_1 + 3(1-t) t^2 \mathbf{P}_2 + t^3 \mathbf{P}_3
$$

```python
def bezier_curve(points, t):
    """Evaluate a Bezier curve of any degree using de Casteljau's algorithm."""
    n = len(points)
    temp = [list(p) for p in points]
    for r in range(1, n):
        for i in range(n - r):
            temp[i] = ((1 - t) * temp[i][0] + t * temp[i+1][0],
                       (1 - t) * temp[i][1] + t * temp[i+1][1],
                       (1 - t) * temp[i][2] + t * temp[i+1][2])
    return temp[0]
```

**Properties of Bezier curves:**

- The curve passes through the first and last control points
- The tangent at the start is along $\mathbf{P}_1 - \mathbf{P}_0$
- The tangent at the end is along $\mathbf{P}_n - \mathbf{P}_{n-1}$
- The curve lies in the convex hull of its control points
- Bezier curves are affine invariant (transform the control points, not the curve)

**Piecewise Bezier curves.** Complex shapes are built from connected Bezier segments. Smooth joins require matching tangent directions at junction points.

**Catmull-Rom spline.** A cubic interpolating spline that passes through all control points:

$$
\mathbf{p}(t) = 0.5 \cdot ((2\mathbf{P}_1) + (-\mathbf{P}_0 + \mathbf{P}_2)t + (2\mathbf{P}_0 - 5\mathbf{P}_1 + 4\mathbf{P}_2 - \mathbf{P}_3)t^2 + (-\mathbf{P}_0 + 3\mathbf{P}_1 - 3\mathbf{P}_2 + \mathbf{P}_3)t^3)
$$

```python
def catmull_rom(p0, p1, p2, p3, t):
    t2 = t * t
    t3 = t2 * t
    x = 0.5 * ((2*p1[0]) + (-p0[0] + p2[0])*t + (2*p0[0] - 5*p1[0] + 4*p2[0] - p3[0])*t2 + (-p0[0] + 3*p1[0] - 3*p2[0] + p3[0])*t3)
    y = 0.5 * ((2*p1[1]) + (-p0[1] + p2[1])*t + (2*p0[1] - 5*p1[1] + 4*p2[1] - p3[1])*t2 + (-p0[1] + 3*p1[1] - 3*p2[1] + p3[1])*t3)
    return (x, y)
```

### B-Splines and NURBS

**Uniform B-spline (cubic).** A basis-spline that offers local control: moving one control point only affects nearby parts of the curve. B-splines are $C^2$ continuous (second derivative continuous).

The cubic B-spline basis functions are:

$$
\begin{aligned}
N_{0,3}(t) &= \frac{1}{6}(1 - t)^3 \\
N_{1,3}(t) &= \frac{1}{6}(3t^3 - 6t^2 + 4) \\
N_{2,3}(t) &= \frac{1}{6}(-3t^3 + 3t^2 + 3t + 1) \\
N_{3,3}(t) &= \frac{1}{6}t^3
\end{aligned}
$$

The curve segment is:

$$
\mathbf{C}(t) = N_{0,3}(t)\mathbf{P}_i + N_{1,3}(t)\mathbf{P}_{i+1} + N_{2,3}(t)\mathbf{P}_{i+2} + N_{3,3}(t)\mathbf{P}_{i+3}
$$

**Key differences from Bezier:**

| Property | Bezier | B-Spline |
|----------|--------|----------|
| Local control | No (moving one control point affects entire curve) | Yes |
| Degree | Fixed by number of control points | Independent of control points |
| Continuity | $C^n$ for degree $n$ at joins if structured | $C^2$ for cubic B-spline |
| Interpolation | Passes through first and last control points | Does not generally interpolate control points |

**NURBS (Non-Uniform Rational B-Splines).** NURBS extend B-splines by:
- Adding weights to control points (making them rational)
- Allowing non-uniform knot spacing

NURBS can exactly represent conic sections (circles, ellipses) which polynomial splines cannot.

```python
def nurbs_point(control_points, weights, knots, u, degree):
    """Evaluate a NURBS curve at parameter u (simplified)."""
    n = len(control_points)
    # Find knot span
    span = find_knot_span(knots, n, u)
    # Compute basis functions
    N = basis_functions(span, u, degree, knots)
    # Weighted control points
    w_points = [(w * cp[0], w * cp[1], w * cp[2], w)
                for cp, w in zip(control_points, weights)]
    # Sum
    x, y, z, w = 0, 0, 0, 0
    for i in range(degree + 1):
        idx = span - degree + i
        x += N[i] * w_points[idx][0]
        y += N[i] * w_points[idx][1]
        z += N[i] * w_points[idx][2]
        w += N[i] * w_points[idx][3]
    return (x / w, y / w, z / w)
```

NURBS are the standard for CAD data exchange (IGES, STEP formats) and are used in virtually all industrial 3D modeling software.

## Glossary

| Term | Definition |
|------|------------|
| Arc length | The distance along a curve measured from one endpoint to another |
| Basis function | A function used in spline representation that blends control points |
| Bezier curve | A parametric curve defined by control points using Bernstein polynomials |
| B-spline | A spline that offers local control and is built from basis functions |
| Catmull-Rom spline | An interpolating cubic spline that passes through all control points |
| Convex hull | The smallest convex set containing all control points |
| De Casteljau's algorithm | A recursive method for evaluating Bezier curves |
| Ellipsoid | A quadric surface that is a 3D generalization of an ellipse |
| Frenet-Serret frame | An orthonormal basis (tangent, normal, binormal) along a curve |
| Half-space | The set of points on one side of a plane |
| Hyperboloid | A quadric surface with a negative term, forming one or two sheets |
| Interpolation | A curve that passes exactly through its control points |
| NURBS | Non-Uniform Rational B-Splines, the industrial standard for curve/surface representation |
| Paraboloid | A quadric surface formed by rotating a parabola around its axis |
| Parametric equation | An equation where coordinates are expressed as functions of one or more parameters |
| Quadric surface | A surface defined by a second-degree polynomial equation in three variables |
| Ray | A half-line starting from a point and extending infinitely in one direction |
| Spline | A piecewise polynomial function used for curve fitting and design |
| Tangent vector | The direction of a curve at a point, given by the first derivative |

## Quick References

- [Scratchapixel: Ray-Sphere Intersection](https://www.scratchapixel.com/lessons/3d-basic-rendering/minimal-ray-tracer-rendering-simple-shapes/ray-sphere-intersection) — practical intersection tests
- [Pomax: A Primer on Bezier Curves](https://pomax.github.io/bezierinfo/) — comprehensive interactive guide
- [The NURBS Book (Piegl & Tiller)](https://link.springer.com/book/9783540615453) — definitive reference for NURBS
- [Freya Holmér: Splines](https://www.youtube.com/playlist?list=PLImQaTpSAdsDl1W1N98co1MGL76vRyF41) — visual spline explanations
- [Real-Time Collision Detection (Christer Ericson)](https://realtimecollisiondetection.net/) — intersection algorithms reference
- [Khan Academy: Parametric Equations](https://www.khanacademy.org/math/multivariable-calculus/thinking-about-multivariable-function/parametric-functions) — interactive lessons

## Next Steps

- [Computational Geometry](computational-geometry.md) — convex hull, Voronoi diagrams, collision detection algorithms
