# Computational Geometry

## Description

Computational geometry studies algorithms and data structures for solving geometric problems. It deals with convex hulls, Voronoi diagrams, triangulations, point location, and collision detection. For developers, computational geometry powers GIS systems, game physics engines, robotics path planners, mesh generation tools, and computer-aided design software.

## Prerequisites

- [Analytic Geometry](analytic-geometry.md) — parametric equations, distances, intersections

## Table of Contents

- [Geometric Primitives and Predicates](#geometric-primitives-and-predicates)
- [Convex Hull](#convex-hull)
- [Voronoi Diagrams](#voronoi-diagrams)
- [Delaunay Triangulation](#delaunay-triangulation)
- [Point-in-Polygon](#point-in-polygon)
- [Line Intersection Detection](#line-intersection-detection)
- [Closest Pair of Points](#closest-pair-of-points)
- [Collision Detection](#collision-detection)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Geometric Primitives and Predicates

Before building algorithms, define the fundamental building blocks:

```python
from dataclasses import dataclass
from typing import List, Tuple, Optional
import math

@dataclass
class Point:
    x: float
    y: float

@dataclass
class Segment:
    p1: Point
    p2: Point

@dataclass
class Polygon:
    vertices: List[Point]
```

**Orientation test.** The orientation of three points $(p, q, r)$ tells us whether they make a left turn, right turn, or are collinear. Compute the cross product of vectors $\mathbf{qp}$ and $\mathbf{rp}$:

$$
\text{orient}(p, q, r) = (q_x - p_x)(r_y - p_y) - (q_y - p_y)(r_x - p_x)
$$

- $> 0$: counter-clockwise (left turn)
- $< 0$: clockwise (right turn)
- $= 0$: collinear

```python
def orient(p, q, r):
    """Cross product of (q-p) and (r-p)."""
    return (q.x - p.x) * (r.y - p.y) - (q.y - p.y) * (r.x - p.x)
```

This is the most fundamental predicate in computational geometry. Almost every algorithm depends on it.

**Using floating-point epsilon.** Due to floating-point errors, never test for exact zero:

```python
def orient_with_eps(p, q, r, eps=1e-10):
    val = orient(p, q, r)
    if abs(val) < eps:
        return 0   # Collinear
    return 1 if val > 0 else -1  # CCW or CW
```

### Convex Hull

The convex hull of a set of points is the smallest convex polygon that contains all points. It generalizes the notion of a "boundary" for a point set.

**Graham scan.** A classic $O(n \log n)$ algorithm:

1. Find the point with the lowest $y$ (leftmost if tie)
2. Sort all other points by polar angle relative to this point
3. Iterate through sorted points, maintaining a stack
4. For each point, pop from the stack while the last three points make a non-left turn

```python
def graham_scan(points: List[Point]) -> List[Point]:
    if len(points) < 3:
        return points

    # Find lowest point (and leftmost if tie)
    p0 = min(points, key=lambda p: (p.y, p.x))

    # Sort by polar angle relative to p0
    sorted_pts = sorted(
        [p for p in points if p != p0],
        key=lambda p: math.atan2(p.y - p0.y, p.x - p0.x)
    )

    # Build hull
    hull = [p0, sorted_pts[0]]
    for p in sorted_pts[1:]:
        while len(hull) >= 2 and orient(hull[-2], hull[-1], p) <= 0:
            hull.pop()
        hull.append(p)
    return hull
```

**Jarvis march (gift wrapping).** An $O(nh)$ algorithm where $h$ is the number of hull points:

```python
def jarvis_march(points: List[Point]) -> List[Point]:
    if len(points) < 3:
        return points

    # Find leftmost point
    p0 = min(points, key=lambda p: p.x)
    hull = [p0]

    while True:
        next_point = None
        for p in points:
            if p == hull[-1]:
                continue
            if next_point is None:
                next_point = p
                continue
            o = orient(hull[-1], next_point, p)
            if o < 0 or (o == 0 and
                         dist_sq(hull[-1], p) > dist_sq(hull[-1], next_point)):
                next_point = p
        if next_point == p0:
            break
        hull.append(next_point)
    return hull
```

**Properties of convex hulls:**

- The convex hull of $n$ points has at most $n$ vertices
- A point is on the hull if it is an extreme point in some direction
- The hull is the intersection of all half-planes containing the point set
- Convex hull distance is used in support vector machines (SVMs) to find the maximum margin hyperplane

**Quickhull.** A divide-and-conquer algorithm analogous to Quicksort, with average $O(n \log n)$ complexity:

```python
def quickhull(points: List[Point]) -> List[Point]:
    if len(points) < 3:
        return points

    # Find extreme x points
    min_x = min(points, key=lambda p: p.x)
    max_x = max(points, key=lambda p: p.x)

    # Partition and recursively compute hull
    def find_hull(pts, p1, p2):
        if not pts:
            return []
        # Find farthest point from line p1-p2
        farthest = max(pts, key=lambda p: abs(orient(p1, p2, p)))
        # Partition into left and right of farthest
        left_set = [p for p in pts if orient(p1, farthest, p) > 0]
        right_set = [p for p in pts if orient(farthest, p2, p) > 0]
        return find_hull(left_set, p1, farthest) + [farthest] + find_hull(right_set, farthest, p2)

    left = [p for p in points if orient(min_x, max_x, p) > 0]
    right = [p for p in points if orient(max_x, min_x, p) > 0]
    return [min_x] + find_hull(left, min_x, max_x) + [max_x] + find_hull(right, max_x, min_x)
```

### Voronoi Diagrams

A Voronoi diagram partitions space into regions based on distance to a set of seed points. Each region contains all points closer to that seed than to any other.

**Formal definition.** Given seeds $S = \{s_1, s_2, \ldots, s_n\}$, the Voronoi cell for seed $s_i$ is:

$$
V(s_i) = \left\{ p \mid \text{dist}(p, s_i) \leq \text{dist}(p, s_j) \ \forall j \neq i \right\}
$$

**Properties:**

- Voronoi cells are convex polygons
- Edges are perpendicular bisectors between neighboring seeds
- Voronoi vertices are centers of circles through three seeds with no other seeds inside
- The number of edges is at most $3n - 6$ for $n$ seeds

```python
def voronoi_cell_area(seeds, bounds, cell_index, samples=100):
    """Approximate Voronoi cell area by sampling (for illustration)."""
    min_x, max_x, min_y, max_y = bounds
    count = 0
    total = samples * samples
    for i in range(samples):
        for j in range(samples):
            px = min_x + (max_x - min_x) * i / samples
            py = min_y + (max_y - min_y) * j / samples
            # Find closest seed
            closest = min(range(len(seeds)),
                         key=lambda k: dist_sq(Point(px, py), seeds[k]))
            if closest == cell_index:
                count += 1
    return (count / total) * (max_x - min_x) * (max_y - min_y)
```

**Fortune's algorithm** computes the Voronoi diagram in $O(n \log n)$ using a sweep line. The algorithm maintains a beach line (a set of parabolas) and processes site and circle events.

**Applications in computing:**

- **GIS:** Nearest facility search (find the closest hospital)
- **Path planning:** Compute paths that maximize clearance from obstacles
- **Wireless networks:** Cell tower placement and coverage analysis
- **Machine learning:** k-nearest neighbors classification
- **Computer graphics:** Procedural texture generation (Voronoi noise)

### Delaunay Triangulation

A Delaunay triangulation of a set of points is a triangulation where no point lies inside the circumcircle of any triangle. It maximizes the minimum angle of all triangles.

**Empty circumcircle criterion.** A triangle $\triangle ABC$ is Delaunay if its circumcircle contains no other points from the set:

```python
def circumcircle_contains(p, a, b, c):
    """Check if point p lies inside the circumcircle of triangle abc."""
    # Compute circumcenter and radius
    d = 2 * (a.x * (b.y - c.y) + b.x * (c.y - a.y) + c.x * (a.y - b.y))
    if abs(d) < 1e-10:
        return False  # Degenerate
    ux = ((a.x*a.x + a.y*a.y) * (b.y - c.y) +
          (b.x*b.x + b.y*b.y) * (c.y - a.y) +
          (c.x*c.x + c.y*c.y) * (a.y - b.y)) / d
    uy = ((a.x*a.x + a.y*a.y) * (c.x - b.x) +
          (b.x*b.x + b.y*b.y) * (a.x - c.x) +
          (c.x*c.x + c.y*c.y) * (b.x - a.x)) / d
    r2 = (ux - a.x)**2 + (uy - a.y)**2
    return (p.x - ux)**2 + (p.y - uy)**2 <= r2 + 1e-10
```

**Properties:**

- The Delaunay triangulation is the dual of the Voronoi diagram: every Delaunay edge corresponds to a Voronoi edge
- It contains $O(n)$ triangles and can be computed in $O(n \log n)$
- The boundary of a Delaunay triangulation is the convex hull of the point set
- It maximizes the minimum angle among all triangulations

**Bowyer-Watson algorithm.** An incremental $O(n^2)$ algorithm:

```python
def bowyer_watson(points: List[Point]) -> List[Tuple[Point, Point, Point]]:
    # Start with a super-triangle covering all points
    min_x = min(p.x for p in points)
    max_x = max(p.x for p in points)
    min_y = min(p.y for p in points)
    max_y = max(p.y for p in points)

    dx = (max_x - min_x) * 10
    dy = (max_y - min_y) * 10
    super_tri = (
        Point(min_x - dx, min_y - dy * 2),
        Point(max_x + dx, min_y - dy * 2),
        Point((min_x + max_x) / 2, max_y + dy * 2)
    )

    triangles = [super_tri]

    for p in points:
        # Find bad triangles (circumcircle contains p)
        bad_triangles = []
        for tri in triangles:
            if circumcircle_contains(p, tri[0], tri[1], tri[2]):
                bad_triangles.append(tri)

        # Find the boundary of the polygonal hole
        edges = {}
        for tri in bad_triangles:
            for i in range(3):
                edge = (tri[i], tri[(i + 1) % 3])
                if edge in edges:
                    del edges[edge]
                elif (edge[1], edge[0]) in edges:
                    del edges[(edge[1], edge[0])]
                else:
                    edges[edge] = True

        # Remove bad triangles
        for tri in bad_triangles:
            triangles.remove(tri)

        # Retriangulate the hole
        for edge in edges:
            triangles.append((edge[0], edge[1], p))

    # Remove triangles with super-triangle vertices
    result = []
    for tri in triangles:
        if (tri[0] in super_tri or tri[1] in super_tri or
            tri[2] in super_tri):
            continue
        result.append(tri)
    return result
```

**Delaunay triangulation in 3D (tetrahedralization).** The 3D generalization creates a tetrahedral mesh. A set of points in 3D does not always have a valid Delaunay tetrahedralization.

### Point-in-Polygon

Determining if a point lies inside a polygon is a fundamental operation in GIS, selection tools, and collision detection.

**Ray casting algorithm.** Cast a ray from the point and count edge crossings:

```python
def point_in_polygon(point: Point, polygon: List[Point]) -> bool:
    """Ray casting algorithm. Returns True if point is inside."""
    inside = False
    n = len(polygon)
    j = n - 1
    for i in range(n):
        if ((polygon[i].y > point.y) != (polygon[j].y > point.y) and
            point.x < (polygon[j].x - polygon[i].x) *
            (point.y - polygon[i].y) / (polygon[j].y - polygon[i].y) + polygon[i].x):
            inside = not inside
        j = i
    return inside
```

**Winding number algorithm.** Computes the winding number (total angle change around the point). A non-zero winding number means the point is inside:

```python
def winding_number(point: Point, polygon: List[Point]) -> int:
    wn = 0
    n = len(polygon)
    for i in range(n):
        j = (i + 1) % n
        if polygon[i].y <= point.y:
            if polygon[j].y > point.y and orient(point, polygon[i], polygon[j]) > 0:
                wn += 1
        else:
            if polygon[j].y <= point.y and orient(point, polygon[i], polygon[j]) < 0:
                wn -= 1
    return wn
```

**Point in convex polygon.** For convex polygons, test that the point is on the same side of all edges:

```python
def point_in_convex_polygon(point: Point, polygon: List[Point]) -> bool:
    n = len(polygon)
    for i in range(n):
        j = (i + 1) % n
        if orient(polygon[i], polygon[j], point) < 0:
            return False
    return True
```

### Line Intersection Detection

**Segment intersection.** Two line segments $(p_1, p_2)$ and $(p_3, p_4)$ intersect if the orientations of $(p_1, p_2, p_3)$ and $(p_1, p_2, p_4)$ have opposite signs, and vice versa.

```python
def segments_intersect(p1, p2, p3, p4) -> bool:
    """Check if two segments intersect (including endpoints)."""
    o1 = orient(p1, p2, p3)
    o2 = orient(p1, p2, p4)
    o3 = orient(p3, p4, p1)
    o4 = orient(p3, p4, p2)

    # General case
    if (o1 > 0 and o2 < 0) or (o1 < 0 and o2 > 0):
        if (o3 > 0 and o4 < 0) or (o3 < 0 and o4 > 0):
            return True

    # Collinear cases
    if o1 == 0 and on_segment(p1, p2, p3): return True
    if o2 == 0 and on_segment(p1, p2, p4): return True
    if o3 == 0 and on_segment(p3, p4, p1): return True
    if o4 == 0 and on_segment(p3, p4, p2): return True

    return False

def on_segment(p1, p2, p):
    """Check if collinear point p is on segment p1-p2."""
    return (min(p1.x, p2.x) <= p.x <= max(p1.x, p2.x) and
            min(p1.y, p2.y) <= p.y <= max(p1.y, p2.y))
```

**Sweep line algorithm for $n$ segments.** Testing all pairs is $O(n^2)$. The Bentley-Ottmann sweep line algorithm finds all intersections in $O((n + k) \log n)$ where $k$ is the number of intersections.

**Line segment intersection for GIS.** In GIS, polygon overlay operations (union, intersection, difference) rely on line intersection as their core operation.

### Closest Pair of Points

Given $n$ points, find the pair with the smallest Euclidean distance.

**Divide and conquer algorithm ($O(n \log n)$):**

1. Sort points by $x$
2. Divide into left and right halves
3. Recursively find closest pair in each half
4. Consider pairs that straddle the dividing line, limited to a strip of width $2 \times \text{min\_dist}$

```python
def closest_pair(points: List[Point]) -> Tuple[Point, Point, float]:
    pts_by_x = sorted(points, key=lambda p: p.x)
    pts_by_y = sorted(points, key=lambda p: p.y)

    def closest_pair_rec(px, py):
        n = len(px)
        if n <= 3:
            # Brute force for small n
            best = float('inf')
            best_pair = None
            for i in range(n):
                for j in range(i + 1, n):
                    d = dist(px[i], px[j])
                    if d < best:
                        best = d
                        best_pair = (px[i], px[j])
            return best_pair[0], best_pair[1], best

        mid = n // 2
        mid_x = px[mid].x

        # Partition y by x
        ly = [p for p in py if p.x <= mid_x]
        ry = [p for p in py if p.x > mid_x]

        (p1l, p2l, dl) = closest_pair_rec(px[:mid], ly)
        (p1r, p2r, dr) = closest_pair_rec(px[mid:], ry)

        if dl < dr:
            best, best_pair = dl, (p1l, p2l)
        else:
            best, best_pair = dr, (p1r, p2r)

        # Strip
        strip = [p for p in py if abs(p.x - mid_x) < best]
        for i in range(len(strip)):
            for j in range(i + 1, min(i + 7, len(strip))):
                d = dist(strip[i], strip[j])
                if d < best:
                    best = d
                    best_pair = (strip[i], strip[j])

        return best_pair[0], best_pair[1], best

    return closest_pair_rec(pts_by_x, pts_by_y)
```

### Collision Detection

Collision detection is essential for game physics, robotics, and CAD.

**Axis-Aligned Bounding Box (AABB).** The simplest bounding volume:

```python
@dataclass
class AABB:
    min_x: float
    min_y: float
    max_x: float
    max_y: float

    def intersects(self, other: 'AABB') -> bool:
        return (self.min_x <= other.max_x and
                self.max_x >= other.min_x and
                self.min_y <= other.max_y and
                self.max_y >= other.min_y)

    def contains(self, p: Point) -> bool:
        return (self.min_x <= p.x <= self.max_x and
                self.min_y <= p.y <= self.max_y)
```

**Oriented Bounding Box (OBB).** An arbitrarily oriented rectangle. Uses the separating axis theorem (SAT) for collision testing.

**Separating Axis Theorem (SAT).** Two convex polygons do not intersect if and only if there exists a line (separating axis) onto which their projections do not overlap:

```python
def obb_obb_collision(obb1, obb2):
    """Check collision between two OBBs using SAT."""
    axes = obb1.get_axes() + obb2.get_axes()

    for axis in axes:
        proj1 = obb1.project(axis)
        proj2 = obb2.project(axis)
        if proj1[1] < proj2[0] or proj2[1] < proj1[0]:
            return False  # Separating axis found
    return True  # No separating axis: collision
```

**Circle collision.** Two circles intersect if the distance between centers is less than the sum of radii:

```python
def circle_circle_collision(c1, r1, c2, r2):
    dx = c1.x - c2.x
    dy = c1.y - c2.y
    return dx*dx + dy*dy <= (r1 + r2) * (r1 + r2)
```

**Broad phase vs. narrow phase.** Modern physics engines divide collision detection into two phases:

1. **Broad phase:** Use spatial partitioning (grid, quadtree, BVH) to find candidate pairs
2. **Narrow phase:** Exact collision test on candidate pairs

```python
class Grid:
    """Simple spatial grid for broad-phase collision detection."""
    def __init__(self, cell_size):
        self.cell_size = cell_size
        self.cells = {}

    def _cell_key(self, p):
        return (int(p.x / self.cell_size), int(p.y / self.cell_size))

    def insert(self, obj):
        key = self._cell_key(obj.position)
        if key not in self.cells:
            self.cells[key] = []
        self.cells[key].append(obj)

    def get_nearby(self, obj):
        key = self._cell_key(obj.position)
        nearby = []
        for dx in (-1, 0, 1):
            for dy in (-1, 0, 1):
                neighbor = (key[0] + dx, key[1] + dy)
                if neighbor in self.cells:
                    nearby.extend(self.cells[neighbor])
        return nearby
```

**GJK (Gilbert-Johnson-Keerthi) algorithm.** An efficient algorithm for computing the distance between convex shapes. It uses the Minkowski difference and a support function.

```python
def support(shape, direction):
    """Find the farthest point in the given direction on the shape."""
    return max(shape, key=lambda p: p.x * direction.x + p.y * direction.y)

def gjk(shape1, shape2):
    """GJK collision detection between two convex shapes."""
    # Start with any point
    d = Point(1, 0)
    simplex = [support(shape1, d) - support(shape2, Point(-d.x, -d.y))]
    d = Point(-simplex[0].x, -simplex[0].y)

    while True:
        p = support(shape1, d) - support(shape2, Point(-d.x, -d.y))
        if dot(p, d) < 0:
            return False  # No collision
        simplex.append(p)
        # Check if simplex contains origin
        if contains_origin(simplex, d):
            return True
```

## Study Cases

### Case 1: GIS Point-of-Interest Nearest Neighbor

A GIS application needs to find the nearest hospital to a user's location among 10,000 hospitals. Use the Voronoi diagram for $O(1)$ lookup after preprocessing:

```python
def build_nearest_neighbor_index(pois):
    """Precompute Voronoi diagram for nearest neighbor queries."""
    # In practice, use a library like scipy.spatial.Voronoi
    from scipy.spatial import Voronoi
    vor = Voronoi([(p.x, p.y) for p in pois])

    # Build a point location structure (e.g., trapezoidal map)
    # For each query point, find which Voronoi cell contains it
    return vor

def find_nearest(query_point, vor, pois):
    """Find the nearest POI using the Voronoi diagram."""
    # Find the Voronoi region containing query_point
    # Return the corresponding POI
    pass
```

### Case 2: Physics Engine Collision Pipeline

A 2D physics engine with 1000 objects uses spatial partitioning:

```python
def physics_step(objects, dt):
    # Broad phase: spatial grid
    grid = Grid(cell_size=10.0)
    for obj in objects:
        grid.insert(obj)

    # Narrow phase: exact collision tests
    collisions = []
    for obj in objects:
        nearby = grid.get_nearby(obj)
        for other in nearby:
            if obj.id < other.id:  # Avoid duplicate pairs
                if circle_circle_collision(obj.pos, obj.radius,
                                          other.pos, other.radius):
                    collisions.append((obj, other))

    # Resolve collisions
    for a, b in collisions:
        resolve_collision(a, b)
```

### Case 3: Robot Path Planning with Clearance

A robot must navigate from start to goal while maintaining a minimum distance from obstacles. The Voronoi diagram of obstacle points provides a path that maximizes clearance:

```python
def voronoi_path(obstacles, start, goal):
    """Find a path along Voronoi edges from start to goal region."""
    from scipy.spatial import Voronoi
    vor = Voronoi(obstacles)

    # Build graph from Voronoi edges
    graph = {}
    for vpair in vor.ridge_vertices:
        if -1 not in vpair:  # Skip infinite ridges
            p1 = tuple(vor.vertices[vpair[0]])
            p2 = tuple(vor.vertices[vpair[1]])
            graph.setdefault(p1, []).append(p2)
            graph.setdefault(p2, []).append(p1)

    # Connect start and goal to nearest Voronoi vertices
    # Run A* on the graph
    # Return the path
    return a_star(graph, start, goal)
```

## Examples

### Example 1: Convex Hull Visualization Helper

```python
def hull_to_polygon(points):
    hull = graham_scan(points)
    # Close the polygon
    return hull + [hull[0]]
```

### Example 2: Point-in-Triangle Using Barycentric Coordinates

```python
def point_in_triangle(p, a, b, c):
    """Check if point p is inside triangle abc using barycentric coordinates."""
    denom = ((b.y - c.y) * (a.x - c.x) + (c.x - b.x) * (a.y - c.y))
    if abs(denom) < 1e-10:
        return False
    alpha = ((b.y - c.y) * (p.x - c.x) + (c.x - b.x) * (p.y - c.y)) / denom
    beta = ((c.y - a.y) * (p.x - c.x) + (a.x - c.x) * (p.y - c.y)) / denom
    gamma = 1 - alpha - beta
    return 0 <= alpha <= 1 and 0 <= beta <= 1 and 0 <= gamma <= 1
```

### Example 3: Simple Polygon Area

```python
def polygon_area(polygon):
    """Shoelace formula."""
    area = 0
    n = len(polygon)
    for i in range(n):
        j = (i + 1) % n
        area += polygon[i].x * polygon[j].y
        area -= polygon[j].x * polygon[i].y
    return abs(area) / 2.0
```

### Example 4: Point in Circle

```python
def point_in_circle(p, center, radius):
    dx = p.x - center.x
    dy = p.y - center.y
    return dx*dx + dy*dy <= radius*radius
```

### Example 5: Circle Bounding Box

```python
def circle_bounds(center, radius):
    return (center.x - radius, center.y - radius,
            center.x + radius, center.y + radius)
```

### Example 6: Minimum Bounding Rectangle

```python
def min_bounding_rect(points):
    min_x = min(p.x for p in points)
    max_x = max(p.x for p in points)
    min_y = min(p.y for p in points)
    max_y = max(p.y for p in points)
    return (min_x, min_y, max_x, max_y)
```

### Example 7: Convex Polygon Minkowski Sum

```python
def minkowski_sum(poly1, poly2):
    """Minkowski sum of two convex polygons."""
    result = []
    for v1 in poly1:
        for v2 in poly2:
            result.append(Point(v1.x + v2.x, v1.y + v2.y))
    return graham_scan(result)
```

### Example 8: Line Segment Closest Points

```python
def closest_points_on_segments(s1p1, s1p2, s2p1, s2p2):
    """Find the closest points between two line segments."""
    d1 = (s1p2.x - s1p1.x, s1p2.y - s1p1.y)
    d2 = (s2p2.x - s2p1.x, s2p2.y - s2p1.y)
    r = (s1p1.x - s2p1.x, s1p1.y - s2p1.y)

    a = d1[0]*d1[0] + d1[1]*d1[1]
    b = d1[0]*d2[0] + d1[1]*d2[1]
    c = d2[0]*d2[0] + d2[1]*d2[1]
    d = d1[0]*r[0] + d1[1]*r[1]
    e = d2[0]*r[0] + d2[1]*r[1]

    denom = a*c - b*b
    if abs(denom) < 1e-10:
        return None  # Parallel
    s = (b*e - c*d) / denom
    t = (a*e - b*d) / denom
    s = max(0, min(1, s))
    t = max(0, min(1, t))

    p1 = Point(s1p1.x + s*d1[0], s1p1.y + s*d1[1])
    p2 = Point(s2p1.x + t*d2[0], s2p1.y + t*d2[1])
    return (p1, p2)
```

## Glossary

| Term | Definition |
|------|------------|
| AABB | Axis-Aligned Bounding Box, a rectangular bounding volume aligned to the coordinate axes |
| Bentley-Ottmann | A sweep line algorithm for finding all intersections in a set of line segments |
| Bowyer-Watson | An incremental algorithm for computing Delaunay triangulation |
| Broad phase | The first stage of collision detection that finds candidate pairs using spatial partitioning |
| Circumcircle | The circle passing through all three vertices of a triangle |
| Convex hull | The smallest convex set containing a given set of points |
| Delaunay triangulation | A triangulation that maximizes the minimum angle and has no point inside any circumcircle |
| Fortune's algorithm | A sweep line algorithm for computing Voronoi diagrams in $O(n \log n)$ |
| GJK (Gilbert-Johnson-Keerthi) | An algorithm for computing distance between convex shapes using support functions |
| Graham scan | An $O(n \log n)$ algorithm for computing the convex hull |
| Jarvis march | An $O(nh)$ algorithm for computing the convex hull (gift wrapping) |
| Minkowski sum | The set of all pairwise sums of points from two convex sets |
| Narrow phase | The second stage of collision detection that performs exact tests on candidate pairs |
| OBB | Oriented Bounding Box, a rectangular bounding volume that can rotate |
| Orientation test | A predicate that determines whether three points form a left turn, right turn, or are collinear |
| Quickhull | A divide-and-conquer convex hull algorithm analogous to Quicksort |
| SAT | Separating Axis Theorem, used for collision detection between convex polygons |
| Sweep line | A computational geometry technique that processes events in sorted order of a coordinate |
| Voronoi diagram | A partition of space into regions closest to each seed point |

## Quick References

- [Computational Geometry: Algorithms and Applications (de Berg et al.)](https://link.springer.com/book/10.1007/978-3-540-77974-2) — the standard textbook
- [Real-Time Collision Detection (Christer Ericson)](https://realtimecollisiondetection.net/) — practical collision detection reference
- [scipy.spatial](https://docs.scipy.org/doc/scipy/reference/spatial.html) — Python implementations of convex hull, Voronoi, Delaunay
- [CGAL (Computational Geometry Algorithms Library)](https://www.cgal.org/) — production-grade C++ library
- [Red Blob Games: Voronoi Maps](https://www.redblobgames.com/maps/mapgen2/) — interactive Voronoi explanation
- [3Blue1Brown: Voronoi Polyhedra](https://www.youtube.com/watch?v=bhD6m5g-0Oo) — visual introduction

## Next Steps

- [Topology Basics](topology-basics.md) — continuity, homeomorphism, connectedness, and metric spaces
