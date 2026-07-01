# Coordinate Geometry

## Description

Coordinate geometry (also called analytic geometry) bridges algebra and geometry by representing geometric objects as equations and points as coordinates. For developers, coordinate geometry is the foundation of computer graphics, game physics, GIS, and computer vision — anywhere you need to describe and manipulate positions in space.

## Prerequisites

- [Mathematical Thinking](../intro/mathematical-thinking.md) — abstraction, variables, and functions

## Table of Contents

- [The Cartesian Coordinate System](#the-cartesian-coordinate-system)
- [Distance and Midpoint](#distance-and-midpoint)
- [Slope and Equations of Lines](#slope-and-equations-of-lines)
- [Circles in the Coordinate Plane](#circles-in-the-coordinate-plane)
- [Conic Sections](#conic-sections)
- [Polar Coordinates](#polar-coordinates)
- [3D Coordinate Geometry](#3d-coordinate-geometry)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Cartesian Coordinate System

The Cartesian coordinate system, named after Rene Descartes, assigns each point in the plane an ordered pair $(x, y)$ of real numbers. Two perpendicular axes — the $x$-axis (horizontal) and $y$-axis (vertical) — intersect at the origin $(0, 0)$.

```
Quadrant II  |  Quadrant I
  (-, +)     |    (+, +)
-------------+-------------
Quadrant III |  Quadrant IV
  (-, -)     |    (+, -)
```

In computer graphics, the coordinate system is often flipped: the origin is at the top-left corner, the $y$-axis points downward, and coordinates are integer pixel values. This is called screen space.

```
Screen Space (2D graphics):
+-----> x (column)
|
v
y (row)
```

In 3D graphics, a third $z$-axis extends the system. By convention, OpenGL uses a right-handed coordinate system where $z$ points out of the screen, while DirectX uses a left-handed system where $z$ points into the screen.

**The coordinate plane partitions space into four quadrants.** A point $(x, y)$ belongs to:

- Quadrant I: $x > 0$, $y > 0$
- Quadrant II: $x < 0$, $y > 0$
- Quadrant III: $x < 0$, $y < 0$
- Quadrant IV: $x > 0$, $y < 0$

Points on the axes have at least one zero coordinate. The $x$-axis is defined by $y = 0$, and the $y$-axis by $x = 0$.

### Distance and Midpoint

**Distance formula.** Given two points $P_1 = (x_1, y_1)$ and $P_2 = (x_2, y_2)$, the Euclidean distance between them is:

$$
d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}
$$

This follows directly from the Pythagorean theorem. In 3D, with $P_1 = (x_1, y_1, z_1)$ and $P_2 = (x_2, y_2, z_2)$:

$$
d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2 + (z_2 - z_1)^2}
$$

When you write a distance check in game physics, you often compare squared distances to avoid the expensive square root:

```python
def distance_squared(p1, p2):
    dx = p2[0] - p1[0]
    dy = p2[1] - p1[1]
    return dx * dx + dy * dy

def is_collision(p1, p2, radius):
    return distance_squared(p1, p2) <= radius * radius
```

**Manhattan distance.** In grid-based systems (urban navigation, chip design), distance is the sum of absolute differences:

$$
d_{\text{Manhattan}} = |x_2 - x_1| + |y_2 - y_1|
$$

**Midpoint formula.** The midpoint $M$ of segment $P_1P_2$ is:

$$
M = \left(\frac{x_1 + x_2}{2}, \frac{y_1 + y_2}{2}\right)
$$

In 3D, just average all three coordinates. This is used in interpolation, collision detection, and path splitting:

```python
def midpoint(p1, p2):
    return ((p1[0] + p2[0]) / 2,
            (p1[1] + p2[1]) / 2,
            (p1[2] + p2[2]) / 2)
```

### Slope and Equations of Lines

**Slope** measures the steepness and direction of a line:

$$
m = \frac{y_2 - y_1}{x_2 - x_1} = \frac{\Delta y}{\Delta x}
$$

- $m > 0$: line rises left to right
- $m < 0$: line falls left to right
- $m = 0$: horizontal line
- $m$ undefined ($\Delta x = 0$): vertical line

**Slope-intercept form:**

$$
y = mx + b
$$

where $m$ is the slope and $b$ is the $y$-intercept (where the line crosses the $y$-axis). This is the most common form in programming because it directly gives $y$ for any $x$.

**Point-slope form:**

$$
y - y_1 = m(x - x_1)
$$

Useful when you know a point on the line and the slope.

**Standard form:**

$$
Ax + By + C = 0
$$

Useful for line intersection tests and determining side-of-line relationships.

**Parametric form:**

$$
\begin{aligned}
x &= x_0 + t \cdot d_x \\
y &= y_0 + t \cdot d_y
\end{aligned}
$$

where $(x_0, y_0)$ is a point on the line, $(d_x, d_y)$ is the direction vector, and $t$ is a real parameter. This is the form most commonly used in ray tracing and animation:

```python
def point_on_line(origin, direction, t):
    return (origin[0] + t * direction[0],
            origin[1] + t * direction[1])
```

**Intersection of two lines.** Given two lines $L_1$: $y = m_1 x + b_1$ and $L_2$: $y = m_2 x + b_2$, solve for $x$:

$$
m_1 x + b_1 = m_2 x + b_2 \implies x = \frac{b_2 - b_1}{m_1 - m_2}
$$

If $m_1 = m_2$, the lines are parallel (or coincident if $b_1 = b_2$).

**Perpendicular and parallel lines:**

- Parallel: $m_1 = m_2$
- Perpendicular: $m_1 \cdot m_2 = -1$
- For vertical/horizontal: a vertical line is perpendicular to a horizontal line

**Angle between two lines:**

$$
\tan \theta = \left|\frac{m_2 - m_1}{1 + m_1 m_2}\right|
$$

This is useful in computer graphics for computing the angle between edges or the direction of reflected rays.

### Circles in the Coordinate Plane

**Standard equation of a circle** with center $(h, k)$ and radius $r$:

$$
(x - h)^2 + (y - k)^2 = r^2
$$

The general form expands to:

$$
x^2 + y^2 + Dx + Ey + F = 0
$$

Completing the square converts general form to standard form.

**Intersection of a line and a circle.** Substitute the line equation into the circle equation and solve the resulting quadratic. The discriminant determines:

- $\Delta > 0$: two intersection points (secant)
- $\Delta = 0$: one intersection point (tangent)
- $\Delta < 0$: no intersection

```python
def line_circle_intersection(line_origin, line_dir, circle_center, radius):
    ox, oy = line_origin
    dx, dy = line_dir
    cx, cy = circle_center

    a = dx * dx + dy * dy
    b = 2 * (dx * (ox - cx) + dy * (oy - cy))
    c = (ox - cx) ** 2 + (oy - cy) ** 2 - radius * radius

    disc = b * b - 4 * a * c
    if disc < 0:
        return []

    t1 = (-b + math.sqrt(disc)) / (2 * a)
    t2 = (-b - math.sqrt(disc)) / (2 * a)
    return [(ox + t1 * dx, oy + t1 * dy),
            (ox + t2 * dx, oy + t2 * dy)]
```

**Circle-circle intersection.** Two circles intersect when the distance $d$ between their centers satisfies:

$$
|r_1 - r_2| \leq d \leq r_1 + r_2
$$

### Conic Sections

Conic sections are curves formed by intersecting a plane with a double cone. They include parabolas, ellipses, and hyperbolas.

**Parabola.** A parabola is the set of points equidistant from a focus and a directrix.

Standard form (vertical axis):

$$
y = a(x - h)^2 + k
$$

The vertex is $(h, k)$, and $a$ determines the direction and width. If $a > 0$, the parabola opens upward; if $a < 0$, it opens downward.

Standard form (horizontal axis):

$$
x = a(y - k)^2 + h
```

Applications:
- Satellite dishes (parabolic reflectors focus signals at the focus)
- Projectile motion (gravity produces a parabolic trajectory)
- Bezier curves (degree-2 Bezier curves are parabolic segments)

```python
def projectile_position(initial_pos, initial_velocity, t, gravity=-9.81):
    x = initial_pos[0] + initial_velocity[0] * t
    y = initial_pos[1] + initial_velocity[1] * t + 0.5 * gravity * t * t
    return (x, y)
```

**Ellipse.** An ellipse is the set of points where the sum of distances to two foci is constant.

Standard form:

$$
\frac{(x - h)^2}{a^2} + \frac{(y - k)^2}{b^2} = 1
$$

where $a$ is the semi-major axis (half the longer diameter) and $b$ is the semi-minor axis (half the shorter diameter). The foci are at $(\pm c, 0)$ where $c^2 = a^2 - b^2$.

A circle is an ellipse with $a = b = r$.

Applications:
- Planetary orbits (Kepler's first law: orbits are ellipses with the Sun at one focus)
- SVG and vector graphics (elliptical arcs)
- Lens design (elliptical lenses focus light)

**Hyperbola.** A hyperbola is the set of points where the absolute difference of distances to two foci is constant.

Standard form (horizontal transverse axis):

$$
\frac{(x - h)^2}{a^2} - \frac{(y - k)^2}{b^2} = 1
$$

Standard form (vertical transverse axis):

$$
\frac{(y - k)^2}{a^2} - \frac{(x - h)^2}{b^2} = 1
$$

Asymptotes are lines the hyperbola approaches at infinity:

$$
y - k = \pm \frac{b}{a}(x - h)
$$

Applications:
- Loran navigation systems (hyperbolic positioning)
- Cooling tower design (hyperboloid structures are structurally efficient)
- Signal processing (hyperbolic functions in filter design)

**Conic discriminant.** The general second-degree equation:

$$
Ax^2 + Bxy + Cy^2 + Dx + Ey + F = 0
$$

The discriminant $B^2 - 4AC$ determines the conic type:

- $B^2 - 4AC < 0$: ellipse (or circle if $A = C$ and $B = 0$)
- $B^2 - 4AC = 0$: parabola
- $B^2 - 4AC > 0$: hyperbola

### Polar Coordinates

Polar coordinates represent points by distance from the origin $r$ and angle from the positive $x$-axis $\theta$.

**Conversion to Cartesian:**

$$
\begin{aligned}
x &= r \cos \theta \\
y &= r \sin \theta
\end{aligned}
$$

**Conversion from Cartesian:**

$$
\begin{aligned}
r &= \sqrt{x^2 + y^2} \\
\theta &= \arctan2(y, x)
\end{aligned}
$$

```python
import math

def polar_to_cartesian(r, theta):
    return (r * math.cos(theta), r * math.sin(theta))

def cartesian_to_polar(x, y):
    return (math.sqrt(x * x + y * y), math.atan2(y, x))
```

The $\arctan2$ function handles the quadrant correctly, returning values in $(-\pi, \pi]$.

**Polar equations of common curves:**

- Line through origin: $\theta = \alpha$
- Circle centered at origin: $r = R$
- Circle through origin, radius $R$: $r = 2R \cos\theta$
- Cardioid: $r = a(1 + \cos\theta)$
- Spiral: $r = a\theta$
- Rose: $r = a\cos(n\theta)$ (for integer $n$, $n$ petals if $n$ is odd, $2n$ petals if $n$ is even)

**Applications in computing:**

- **Map projections.** The Mercator projection maps latitude/longitude ($\phi, \lambda$) to Cartesian coordinates using polar-to-Cartesian concepts at the core of the projection math.
- **Radar/UI dials.** Radar displays use polar coordinates natively — distance from center is range, angle is azimuth.
- **Image processing.** The Hough transform for circle detection operates in a polar parameter space.
- **Rotation animations.** Storing angles and radii simplifies rotation compared to Cartesian coordinates.

### 3D Coordinate Geometry

**Right-hand rule.** In 3D, coordinate systems follow the right-hand rule: point your thumb in the $+x$ direction, index finger in $+y$, and middle finger points in $+z$.

**Distance in 3D:**

$$
d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2 + (z_2 - z_1)^2}
$$

**Equation of a sphere** centered at $(h, k, l)$ with radius $r$:

$$
(x - h)^2 + (y - k)^2 + (z - l)^2 = r^2
$$

**Parametric line in 3D:**

$$
\begin{aligned}
x &= x_0 + t \cdot a \\
y &= y_0 + t \cdot b \\
z &= z_0 + t \cdot c
\end{aligned}
$$

where $(a, b, c)$ is the direction vector.

**Coordinate systems in 3D graphics:**

- **World space.** The global coordinate system of the 3D scene.
- **View space (camera space).** The coordinate system with the camera at the origin. Points are transformed from world space to view space by the view matrix.
- **Clip space.** After projection, coordinates are in clip space, where the GPU clips geometry outside the view frustum.
- **Screen space.** Final pixel coordinates after the viewport transform.

```python
def world_to_screen(world_pos, view_matrix, proj_matrix, viewport):
    # Transform to clip space
    clip_pos = proj_matrix @ (view_matrix @ world_pos)
    # Perspective divide to get NDC (normalized device coordinates)
    ndc = (clip_pos.x / clip_pos.w, clip_pos.y / clip_pos.w)
    # Viewport transform to screen pixel coordinates
    screen_x = viewport.x + (ndc[0] + 1) * viewport.w / 2
    screen_y = viewport.y + (1 - ndc[1]) * viewport.h / 2
    return (screen_x, screen_y)
```

**Map projections.** Representing the spherical Earth on a flat screen involves converting between geographic coordinates (latitude $\phi$, longitude $\lambda$) and Cartesian pixel coordinates.

The Mercator projection:

$$
\begin{aligned}
x &= R \cdot \lambda \\
y &= R \cdot \ln\left(\tan\left(\frac{\pi}{4} + \frac{\phi}{2}\right)\right)
\end{aligned}
$$

where $R$ is the Earth's radius in the projection.

```python
def mercator_projection(lat, lon, zoom):
    # Convert latitude and longitude to Mercator tile coordinates
    x = (lon + 180.0) / 360.0 * (1 << zoom)
    lat_rad = math.radians(lat)
    y = (1.0 - math.log(math.tan(lat_rad) + 1.0 / math.cos(lat_rad))
         / math.pi) / 2.0 * (1 << zoom)
    return (int(x), int(y))
```

**Game physics coordinates.** Most game engines use a right-handed coordinate system with $y$ as the up axis (Unreal Engine, Unity) or $z$ as the up axis (some off-road driving games). Understanding coordinate conventions is essential for porting physics code between engines.

## Glossary

| Term | Definition |
|------|------------|
| Abscissa | The $x$-coordinate of a point in the Cartesian plane |
| Asymptote | A line that a curve approaches arbitrarily closely but never touches |
| Cartesian coordinates | A coordinate system using perpendicular axes to locate points in space |
| Conic section | A curve (parabola, ellipse, hyperbola) formed by intersecting a plane with a cone |
| Directrix | A fixed line used in the definition of a conic section |
| Discriminant | The expression $b^2 - 4ac$ in a quadratic equation that determines the number of solutions |
| Ellipse | A conic section where the sum of distances to two foci is constant |
| Focus | A fixed point used in the definition of a conic section |
| Hyperbola | A conic section where the absolute difference of distances to two foci is constant |
| Midpoint | The point exactly halfway between two given points |
| Ordinate | The $y$-coordinate of a point in the Cartesian plane |
| Origin | The point $(0, 0)$ where the coordinate axes intersect |
| Parabola | A conic section formed by points equidistant from a focus and a directrix |
| Parameter $t$ | An independent variable used in parametric equations to express coordinates |
| Polar coordinates | A coordinate system using distance from origin and angle from reference axis |
| Quadrant | One of four regions of the Cartesian plane divided by the axes |
| Slope | The ratio of vertical change to horizontal change between two points ($\Delta y / \Delta x$) |
| Sphere | The set of points at a fixed distance from a center in 3D space |

## Quick References

- [Desmos Graphing Calculator](https://www.desmos.com/calculator) — interactive tool for exploring coordinate geometry
- [3Blue1Brown: Linear Algebra & Coordinate Systems](https://www.3blue1brown.com/topics/linear-algebra) — visual explanations
- [Math Insight: Coordinate Geometry](https://mathinsight.org/coordinate_geometry) — interactive applets
- [Khan Academy: Analytic Geometry](https://www.khanacademy.org/math/geometry-home/analytic-geometry-topic) — practice problems
- [Spatial Reference](https://spatialreference.org/) — coordinate system definitions for GIS developers
- [GPU Gems: Coordinate Transformations](https://developer.nvidia.com/gpugems/gpugems/part-i-natural-effects) — coordinate spaces in real-time rendering

## Next Steps

- [Analytic Geometry](analytic-geometry.md) — parametric curves, planes, quadric surfaces, and splines
- [Vectors & Transformations](vectors-and-transformations.md) — geometric transformations and matrix operations
