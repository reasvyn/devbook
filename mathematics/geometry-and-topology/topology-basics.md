# Topology Basics

## Description

Topology is the study of properties that are preserved under continuous deformations — stretching, bending, twisting, but not tearing or gluing. For developers, topology provides the mathematical framework for understanding data shapes, network topologies, configuration spaces in robotics, and the continuous structure of space in computer graphics and physics simulations.

## Prerequisites

- [Analytic Geometry](analytic-geometry.md) — continuity, parametric curves, surfaces

## Table of Contents

- [What is Topology?](#what-is-topology)
- [Open and Closed Sets](#open-and-closed-sets)
- [Neighborhoods and Interior](#neighborhoods-and-interior)
- [Continuity in Topological Terms](#continuity-in-topological-terms)
- [Homeomorphism](#homeomorphism)
- [Topological Invariants](#topological-invariants)
- [Connectedness](#connectedness)
- [Compactness](#compactness)
- [Metric Spaces](#metric-spaces)
- [Product and Quotient Topology](#product-and-quotient-topology)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What is Topology?

Topology answers the question: "What properties of a shape remain when all notions of distance, angles, and straightness are discarded?" A coffee mug and a donut (torus) are topologically equivalent because one can be continuously deformed into the other.

Intuitively, topology studies the qualitative properties of space:

- **Connectedness:** Is the space all one piece?
- **Holes:** Does the space have holes? Of what dimension?
- **Boundary:** Does the space have edges?
- **Compactness:** Is the space "finite" in a topological sense?

For a developer, topological thinking appears in:
- **Data analysis:** Topological data analysis (TDA) finds shape in point cloud data
- **Network topology:** The arrangement of nodes and links (star, mesh, ring) is a topological property
- **Robotics:** Configuration spaces describe all possible robot poses as a topological space
- **Computer graphics:** Mesh topology determines how vertices connect into faces
- **Sensor networks:** Coverage and connectivity are topological problems

### Open and Closed Sets

A topology on a set $X$ is a collection $\mathcal{T}$ of subsets of $X$ (called open sets) satisfying:

1. $\emptyset$ and $X$ are in $\mathcal{T}$
2. Any union of sets in $\mathcal{T}$ is in $\mathcal{T}$
3. Any finite intersection of sets in $\mathcal{T}$ is in $\mathcal{T}$

**Standard topology on $\mathbb{R}$.** A set $U \subseteq \mathbb{R}$ is open if for every $x \in U$, there exists $\varepsilon > 0$ such that $(x - \varepsilon, x + \varepsilon) \subseteq U$. In other words, every point in an open set has some "wiggle room" around it that stays inside the set.

- Open intervals $(a, b)$ are open sets
- Closed intervals $[a, b]$ are not open (endpoints have no wiggle room)
- The union of open intervals $(n, n+1)$ for all integers $n$ is open
- A finite intersection of open intervals is open (or empty)

**Closed sets.** A set $C \subseteq X$ is closed if its complement $X \setminus C$ is open.

- Closed intervals $[a, b]$ are closed in $\mathbb{R}$
- Singletons $\{x\}$ are closed in $\mathbb{R}$
- A set can be both open and closed (clopen): $\emptyset$ and $X$
- A set can be neither open nor closed: $(0, 1]$ in $\mathbb{R}$

```python
def is_open_interval(a, b, x, eps=1e-6):
    """Check if point x is in open interval (a, b)."""
    return a < x < b

def has_neighborhood_in_set(S, x, eps=1e-6):
    """Check if there's an epsilon-neighborhood of x contained in S."""
    return all((x - eps < p < x + eps) for p in S)
```

**Open sets in $\mathbb{R}^n$.** Using the Euclidean metric, an open ball $B_r(x) = \{y \mid \|y - x\| < r\}$ is the basic open set. A set is open if every point has some open ball around it contained in the set.

**Discrete topology.** Every subset is open. This topology detects no structure — every function from a discrete space is continuous.

**Trivial (indiscrete) topology.** Only $\emptyset$ and $X$ are open. Everything is glued together.

**The importance of open sets.** Open sets encode the notion of "nearness" without requiring a distance metric. Two points are "near" if they are in many of the same open sets.

### Neighborhoods and Interior

**Neighborhood.** A set $N$ is a neighborhood of a point $x$ if there exists an open set $U$ with $x \in U \subseteq N$. Every open set containing $x$ is a neighborhood, but neighborhoods need not be open.

**Interior.** The interior of a set $S$, denoted $\text{int}(S)$, is the union of all open sets contained in $S$. It is the largest open set inside $S$.

- $\text{int}((0, 1]) = (0, 1)$
- $\text{int}([0, 1]) = (0, 1)$
- $\text{int}(\mathbb{Q}) = \emptyset$ (the rationals have empty interior in $\mathbb{R}$)

**Closure.** The closure of $S$, denoted $\overline{S}$, is the intersection of all closed sets containing $S$. It is the smallest closed set containing $S$.

- $\overline{(0, 1)} = [0, 1]$
- $\overline{(0, 1)} = [0, 1]$
- $\overline{\mathbb{Q}} = \mathbb{R}$ (rationals are dense)

**Boundary.** The boundary of $S$, denoted $\partial S$, is $\overline{S} \setminus \text{int}(S)$.

- $\partial((0, 1)) = \{0, 1\}$
- $\partial(\mathbb{Q}) = \mathbb{R}$

**Dense set.** $S$ is dense in $X$ if $\overline{S} = X$. Examples: $\mathbb{Q}$ in $\mathbb{R}$, continuous functions in the space of integrable functions.

```python
def interior_of_interval(a, b):
    """The interior of any interval (a,b), [a,b], (a,b], [a,b) is (a,b)."""
    return (a, b)

def boundary_of_interval(a, b):
    """The boundary of an interval."""
    return {a, b}
```

**Limit points (accumulation points).** A point $x$ is a limit point of $S$ if every neighborhood of $x$ contains a point of $S$ other than $x$. The closure $\overline{S}$ equals $S \cup \{\text{limit points of } S\}$.

### Continuity in Topological Terms

A function $f: X \to Y$ between topological spaces is **continuous** if for every open set $V \subseteq Y$, the preimage $f^{-1}(V)$ is open in $X$.

This generalizes the epsilon-delta definition: in metric spaces, the topological definition is equivalent to the standard calculus definition.

```python
def is_continuous_at_point(f, x0, eps=1e-6):
    """Check epsilon-delta continuity at a point (for real-valued functions)."""
    # For each epsilon, there should be a delta such that
    # |x - x0| < delta implies |f(x) - f(x0)| < eps
    # This is a finite approximation
    for delta in [0.1, 0.01, 0.001, 0.0001]:
        x1 = x0 - delta
        x2 = x0 + delta
        if abs(f(x1) - f(x0)) < eps and abs(f(x2) - f(x0)) < eps:
            return True
    return False
```

**Properties of continuous functions:**

- The composition of continuous functions is continuous
- Constant functions are continuous
- The identity function is continuous
- Continuous functions map limits to limits: if $x_n \to x$, then $f(x_n) \to f(x)$
- Continuous functions map connected sets to connected sets
- Continuous functions map compact sets to compact sets

**Homeomorphism.** A bijective function $f: X \to Y$ is a homeomorphism if both $f$ and $f^{-1}$ are continuous. Two spaces are homeomorphic (topologically equivalent) if there exists a homeomorphism between them.

```python
def homeomorphic_examples():
    """Examples of homeomorphic and non-homeomorphic spaces."""
    # Open interval (0,1) is homeomorphic to R (via tan function)
    # f: (0,1) -> R, f(x) = tan(pi*x - pi/2)

    # Circle S^1 is NOT homeomorphic to interval [0,1]
    # (removing a point from circle gives connected set;
    #  removing an interior point from [0,1] disconnects it)

    # Sphere S^2 is NOT homeomorphic to torus T^2
    # (different Euler characteristic: 2 vs 0)
    pass
```

### Homeomorphism

Two spaces $X$ and $Y$ are homeomorphic if there exists a continuous bijection $f: X \to Y$ with continuous inverse $f^{-1}$.

**How to prove two spaces are homeomorphic:** Construct an explicit homeomorphism.

- Open interval $(0, 1) \cong \mathbb{R}$: $f(x) = \tan(\pi x - \pi/2)$
- Sphere minus a point $\cong \mathbb{R}^2$: Stereographic projection
- Any two closed intervals $[a, b] \cong [c, d]$: Linear mapping

**How to prove two spaces are NOT homeomorphic:** Find a topological invariant that differs.

**Common homeomorphisms:**

| Space | Homeomorphic to | Homeomorphism |
|-------|-----------------|---------------|
| $(0,1)$ | $\mathbb{R}$ | $f(x) = \tan(\pi x - \pi/2)$ |
| $(-1,1)$ | $\mathbb{R}$ | $f(x) = x/(1-x^2)$ |
| Open disk | $\mathbb{R}^2$ | $f(x,y) = (x,y)/\sqrt{1-x^2-y^2}$ |
| $S^2 \setminus \{N\}$ | $\mathbb{R}^2$ | Stereographic projection |
| Torus $T^2$ | $S^1 \times S^1$ | Product of two circles |

### Topological Invariants

A topological invariant is a property preserved under homeomorphism. If two spaces have different invariants, they cannot be homeomorphic.

**Cardinality.** The number of points (if finite) or cardinality of the underlying set.

**Connectedness.** Whether the space is one piece or multiple.

**Compactness.** Whether the space is "finite" in a precise topological sense.

**Euler characteristic.** For a cell complex (like a polyhedron):

$$
\chi = V - E + F
$$

where $V$ is the number of vertices, $E$ edges, $F$ faces.

- Sphere: $\chi = 2$
- Torus: $\chi = 0$
- Double torus: $\chi = -2$
- Projective plane: $\chi = 1$

**Number of path components.** How many disconnected pieces.

**Fundamental group.** The group of loops up to continuous deformation. $\pi_1(X)$ captures the 1-dimensional hole structure.

- $\pi_1(S^1) = \mathbb{Z}$ (loops are classified by winding number)
- $\pi_1(S^2) = 0$ (every loop contracts to a point)
- $\pi_1(T^2) = \mathbb{Z} \times \mathbb{Z}$

**Higher homotopy groups.** $\pi_n(X)$ captures $n$-dimensional hole structure.

**Betti numbers.** $b_n$ counts the number of $n$-dimensional holes:

- $b_0$: number of connected components
- $b_1$: number of 1-dimensional holes (loops)
- $b_2$: number of 2-dimensional voids (cavities)

```python
def euler_characteristic_of_mesh(vertices, edges, faces):
    """Euler characteristic of a triangle mesh."""
    V = len(vertices)
    E = len(edges)
    F = len(faces)
    return V - E + F

def genus_from_euler(chi):
    """Genus (number of holes) of an orientable surface from Euler char."""
    return (2 - chi) // 2
```

### Connectedness

A topological space $X$ is **connected** if it cannot be written as the disjoint union of two non-empty open subsets.

Equivalently: $X$ is connected if the only clopen (both open and closed) subsets are $\emptyset$ and $X$.

**Path-connectedness.** A space is path-connected if for any two points $x, y \in X$, there exists a continuous path $\gamma: [0, 1] \to X$ with $\gamma(0) = x$ and $\gamma(1) = y$.

- Path-connected implies connected
- Connected does NOT necessarily imply path-connected (the topologist's sine curve is connected but not path-connected)
- In $\mathbb{R}^n$, open connected sets are path-connected

**Connected components.** Every space partitions into maximal connected subsets called connected components.

```python
def is_graph_connected(adjacency_list):
    """Check if a graph is connected using BFS."""
    if not adjacency_list:
        return True
    visited = set()
    stack = [next(iter(adjacency_list))]
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            stack.extend(n for n in adjacency_list[node] if n not in visited)
    return len(visited) == len(adjacency_list)
```

**Applications of connectedness:**

- **Image segmentation:** Connected components labeling finds blobs in binary images
- **Network reliability:** A network is disconnected if some nodes cannot communicate
- **Sensor networks:** Coverage connectivity ensures data can reach the base station

```python
def connected_components_labeling(binary_image):
    """Label connected components in a binary image (4-connectivity)."""
    h, w = len(binary_image), len(binary_image[0])
    labels = [[0] * w for _ in range(h)]
    current_label = 0

    for y in range(h):
        for x in range(w):
            if binary_image[y][x] == 1 and labels[y][x] == 0:
                current_label += 1
                stack = [(x, y)]
                while stack:
                    cx, cy = stack.pop()
                    if 0 <= cx < w and 0 <= cy < h and \
                       binary_image[cy][cx] == 1 and labels[cy][cx] == 0:
                        labels[cy][cx] = current_label
                        stack.extend([(cx-1, cy), (cx+1, cy),
                                      (cx, cy-1), (cx, cy+1)])
    return labels, current_label
```

### Compactness

A topological space $X$ is **compact** if every open cover of $X$ has a finite subcover.

An open cover is a collection of open sets whose union is $X$. Compactness means we can always pick finitely many of them that still cover $X$.

**Heine-Borel theorem.** In $\mathbb{R}^n$, a set is compact if and only if it is closed and bounded.

- $[0, 1]$ is compact
- $(0, 1)$ is not compact (the cover $\{(1/n, 1)\}_{n=2}^\infty$ has no finite subcover)
- $\mathbb{R}$ is not compact

**Properties of compactness:**

- Continuous image of a compact set is compact
- Closed subset of a compact set is compact
- Product of compact sets is compact (Tychonoff theorem)
- Compact sets in a metric space are sequentially compact (every sequence has a convergent subsequence)

**Applications of compactness:**

- **Optimization:** Continuous functions on compact sets attain their maximum and minimum (Extreme Value Theorem)
- **Physics simulation:** Compact domains ensure bounded computation
- **Computer graphics:** Compact bounding volumes speed up collision detection

```python
def extreme_values(f, domain_points):
    """Find min and max of f on a compact set (approximated by finite sample)."""
    values = [f(p) for p in domain_points]
    return min(values), max(values)
```

**Local compactness.** A space is locally compact if every point has a compact neighborhood. $\mathbb{R}^n$ is locally compact.

### Metric Spaces

A metric space is a set $X$ with a distance function $d: X \times X \to \mathbb{R}_{\geq 0}$ satisfying:

1. $d(x, y) = 0 \iff x = y$ (identity of indiscernibles)
2. $d(x, y) = d(y, x)$ (symmetry)
3. $d(x, z) \leq d(x, y) + d(y, z)$ (triangle inequality)

Every metric space has a natural topology: the metric topology, where open sets are unions of open balls $B_r(x) = \{y \mid d(x, y) < r\}$.

**Common metrics:**

- **Euclidean metric:** $d(x, y) = \sqrt{\sum (x_i - y_i)^2}$
- **Manhattan metric:** $d(x, y) = \sum |x_i - y_i|$
- **Chebyshev metric (max norm):** $d(x, y) = \max_i |x_i - y_i|$
- **Discrete metric:** $d(x, y) = 0$ if $x = y$, else $1$
- **Hamming distance:** Number of positions where strings differ

```python
def euclidean_dist(p, q):
    return math.sqrt(sum((a-b)**2 for a, b in zip(p, q)))

def manhattan_dist(p, q):
    return sum(abs(a-b) for a, b in zip(p, q))

def chebyshev_dist(p, q):
    return max(abs(a-b) for a, b in zip(p, q))

def discrete_metric(p, q):
    return 0 if p == q else 1
```

**Complete metric spaces.** A metric space is complete if every Cauchy sequence converges. $\mathbb{R}$ is complete; $\mathbb{Q}$ is not.

**Contraction mapping theorem.** If $f: X \to X$ is a contraction ($d(f(x), f(y)) \leq c \cdot d(x, y)$ for $c < 1$) on a complete metric space, then $f$ has a unique fixed point.

```python
def fixed_point_iteration(f, x0, tolerance=1e-10, max_iters=1000):
    """Find fixed point of a contraction mapping."""
    x = x0
    for _ in range(max_iters):
        x_next = f(x)
        if abs(x_next - x) < tolerance:  # Using Euclidean-like distance
            return x_next
        x = x_next
    return x
```

**Metrics in computing:**

- **k-d trees and ball trees:** Use metrics for nearest neighbor search
- **DBSCAN clustering:** Uses distance to define density-connected components
- **MDS (Multidimensional scaling):** Finds point configurations matching given distances
- **Locality-sensitive hashing (LSH):** Uses metrics to hash similar items to the same bucket

### Product and Quotient Topology

**Product topology.** Given topological spaces $X$ and $Y$, the product topology on $X \times Y$ has basis $\{U \times V \mid U \text{ open in } X, V \text{ open in } Y\}$.

The projection maps $\pi_X: X \times Y \to X$ and $\pi_Y: X \times Y \to Y$ are continuous.

- $\mathbb{R}^2$ with product topology is homeomorphic to $\mathbb{R}^2$ with Euclidean topology
- The torus $T^2 = S^1 \times S^1$

**Quotient topology.** Given a space $X$ and an equivalence relation $\sim$, the quotient space $X/\sim$ has the finest topology making the quotient map $q: X \to X/\sim$ continuous.

Intuitively, we glue points together:

- Gluing the ends of $[0, 1]$ gives $S^1$: $[0, 1] / \{0 \sim 1\}$
- Gluing opposite edges of a square gives a torus: $[0, 1]^2 / \{(0, y) \sim (1, y), (x, 0) \sim (x, 1)\}$
- Gluing opposite edges with a twist gives a Mobius strip

```python
def quotient_interval_to_circle(t):
    """Map t in [0,1] to a point on the circle S^1 (glue 0 and 1)."""
    theta = 2 * math.pi * t
    return (math.cos(theta), math.sin(theta))
```

**Applications of quotient topology:**

- **Texture mapping:** Wrapping a 2D texture onto a 3D surface involves quotient-like identifications
- **Robot configuration spaces:** Identifying joint angles modulo $2\pi$ creates torus-like spaces
- **Periodic boundary conditions:** In molecular dynamics, simulating a torus topology (wrap-around)

```python
def wrap_position(pos, bounds):
    """Apply periodic boundary conditions (toroidal topology)."""
    wrapped = []
    for p, (low, high) in zip(pos, bounds):
        wrapped.append(low + (p - low) % (high - low))
    return tuple(wrapped)
```

**Configuration space in robotics.** The configuration space (C-space) of a robot is the set of all possible poses. For a robot arm with $n$ revolute joints, the C-space is the $n$-torus $T^n = S^1 \times S^1 \times \cdots \times S^1$.

```python
def configuration_space_angle(theta):
    """Map a joint angle to the circle S^1, handling wrap-around."""
    return theta % (2 * math.pi)
```

## Glossary

| Term | Definition |
|------|------------|
| Betti number | The $n$th Betti number $b_n$ counts the number of $n$-dimensional holes |
| Boundary | The set of points in the closure of $S$ not in the interior of $S$ |
| Clopen | A set that is both open and closed |
| Closed set | A set whose complement is open |
| Compact | A space where every open cover has a finite subcover |
| Complete metric space | A metric space where every Cauchy sequence converges |
| Connected | A space that cannot be partitioned into two non-empty open subsets |
| Continuity | A function where the preimage of every open set is open |
| Contraction mapping | A function $f$ satisfying $d(f(x), f(y)) \leq c \cdot d(x, y)$ for $c < 1$ |
| Cover | A collection of sets whose union contains the space |
| Dense set | A set whose closure is the whole space |
| Euler characteristic | The alternating sum $V - E + F$ for a cell complex |
| Homeomorphism | A continuous bijection with continuous inverse |
| Homotopy | A continuous deformation between two functions or paths |
| Interior | The largest open set contained in a given set |
| Limit point | A point whose every neighborhood contains another point from the set |
| Metric | A distance function satisfying non-negativity, symmetry, and triangle inequality |
| Neighborhood | A set containing an open set around a point |
| Open set | A set where every point has an epsilon-neighborhood contained in it |
| Path-connected | A space where any two points can be joined by a continuous path |
| Quotient topology | The topology obtained by identifying (gluing) points in a space |
| Topological invariant | A property preserved under homeomorphism |
| Topology | The collection of open sets defining a space's structure |

## Quick References

- [Introduction to Topology (Munkres)](https://www.pearson.com/en-us/subject-catalog/p/introduction-to-topology/P200000010088) — the standard textbook
- [Applied Topology (Carlsson)](https://www.ams.org/journals/notices/201104/rtx110400454p.pdf) — topology for data analysis
- [Topological Data Analysis Guide](https://tda.guide/) — interactive introduction
- [Gudhi Library](https://gudhi.inria.fr/) — TDA software library
- [3Blue1Brown: Euler Characteristic](https://www.youtube.com/watch?v=7utHf4P4lXk) — visual intuition
- [Wikipedia: Point-Set Topology](https://en.wikipedia.org/wiki/Point-set_topology) — comprehensive reference

## Next Steps

- [Graph Theory & Topology](graph-theory-topology.md) — planar graphs, Euler characteristic, topological data analysis, knot theory
