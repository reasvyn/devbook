# Graph Theory & Topological Applications

## Description

Graph theory and topology converge in the study of planar graphs, the Euler characteristic, knot theory, graph embeddings on surfaces, and topological data analysis. For developers, these ideas drive printed circuit board (PCB) routing, 3D mesh topology, molecular modeling, sensor network coverage analysis, and the extraction of shape from point cloud data.

## Prerequisites

- [Topology Basics](topology-basics.md) — open sets, continuity, homeomorphism, topological invariants
- [Graph Theory](../discrete-mathematics/graph-theory.md) — graph definitions, connectivity, paths, trees

## Table of Contents

- [Planar Graphs](#planar-graphs)
- [Euler Characteristic for Graphs and Surfaces](#euler-characteristic-for-graphs-and-surfaces)
- [Graph Embedding](#graph-embedding)
- [Knot Theory Basics](#knot-theory-basics)
- [Topological Data Analysis](#topological-data-analysis)
- [Persistent Homology](#persistent-homology)
- [The Mapper Algorithm](#the-mapper-algorithm)
- [Applications in Computing](#applications-in-computing)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Planar Graphs

A graph is **planar** if it can be drawn in the plane without edge crossings. Planar graphs are the foundation of PCB design, VLSI circuit layout, and map coloring.

**Kuratowski's theorem.** A graph is planar if and only if it does not contain a subdivision of $K_5$ (complete graph on 5 vertices) or $K_{3,3}$ (complete bipartite graph with 3+3 vertices) as a subgraph.

```
K_5:                        K_{3,3}:
  0---1---2                 0---3
 / \ / \ /                 /|\ /|\
3---4---5                 1---4
                           \|/ \|/
                           2---5
```

**Euler's formula for planar graphs.** For a connected planar graph drawn on the plane:

$$
V - E + F = 2
$$

where $V$ is vertices, $E$ is edges, and $F$ is faces (the regions the graph divides the plane into, including the unbounded outer face).

```python
def euler_formula_planar(V, E, F):
    """Verify Euler's formula for a connected planar graph."""
    return V - E + F == 2

def faces_from_euler(V, E):
    """Compute the number of faces from V and E for a connected planar graph."""
    return E - V + 2
```

**Consequences of Euler's formula:**

- For a connected planar graph with $V \geq 3$: $E \leq 3V - 6$
- For a bipartite planar graph: $E \leq 2V - 4$
- Every planar graph has at least one vertex of degree at most 5

```python
def max_edges_planar(V):
    """Maximum number of edges in a simple planar graph with V vertices."""
    return 3 * V - 6 if V >= 3 else V * (V - 1) // 2
```

**Planar graph coloring.** The Four Color Theorem states that every planar graph is 4-colorable (the vertices can be colored with at most 4 colors such that adjacent vertices have different colors).

**Applications:**

- **PCB routing:** Circuit traces must not cross on the same layer
- **Map coloring:** Political maps need at most 4 colors
- **VLSI design:** Minimizing crossings in chip layouts
- **Graph visualization:** Drawing graphs without edge crossings

**Testing planarity in code:**

```python
def is_planary_by_ear_decomposition(graph):
    """Check if a graph is planar using edge addition (simplified)."""
    # Full planarity testing uses the Hopcroft-Tarjan algorithm (O(V))
    # This is a placeholder showing the concept
    from planarity import is_planar  # External library
    return is_planar(graph)
```

### Euler Characteristic for Graphs and Surfaces

The Euler characteristic $\chi$ is a topological invariant for graphs and surfaces.

**For a graph** (considered as a 1-dimensional cell complex):

$$
\chi = V - E
$$

For a connected graph, $\chi = 1$ if the graph is a tree, and $\chi < 1$ if the graph has cycles.

**For a surface** triangulated by a graph:

$$
\chi = V - E + F
$$

**Euler characteristic of common surfaces:**

| Surface | $\chi$ | Genus | Description |
|---------|--------|-------|-------------|
| Sphere $S^2$ | 2 | 0 | No holes |
| Torus $T^2$ | 0 | 1 | One hole |
| Double torus | -2 | 2 | Two holes |
| Projective plane $\mathbb{RP}^2$ | 1 | N/A | Non-orientable |
| Klein bottle | 0 | N/A | Non-orientable |

```python
def genus_of_triangulated_mesh(V, E, F):
    """Compute genus of an orientable surface from a triangulation."""
    chi = V - E + F
    return (2 - chi) // 2

def euler_characteristic_of_mesh(V, E, F):
    """Euler characteristic of a triangle mesh."""
    return V - E + F
```

**Relationship between planar graphs and sphere.** A graph is planar if and only if it can be drawn on a sphere without crossings. The Euler formula for planar graphs ($V - E + F = 2$) is the Euler characteristic of the sphere.

**Polygonal mesh validation.** The Euler characteristic can detect errors in 3D mesh data:

```python
def validate_manifold_mesh(vertices, faces):
    """Check if a triangle mesh is a valid 2-manifold using Euler formula."""
    # Build edge list from faces
    edges = set()
    for f in faces:
        for i in range(3):
            e = tuple(sorted((f[i], f[(i+1)%3])))
            edges.add(e)
    V = len(vertices)
    E = len(edges)
    F = len(faces)

    chi = V - E + F
    print(f"V={V}, E={E}, F={F}, chi={chi}")

    # For a valid closed triangle mesh, chi should be even
    # and each edge should appear in exactly 2 faces (manifold)
    edge_count = {}
    for f in faces:
        for i in range(3):
            e = tuple(sorted((f[i], f[(i+1)%3])))
            edge_count[e] = edge_count.get(e, 0) + 1

    non_manifold = [e for e, c in edge_count.items() if c != 2]
    if non_manifold:
        print(f"Warning: {len(non_manifold)} non-manifold edges")

    return len(non_manifold) == 0
```

### Graph Embedding

A graph embedding is a drawing of a graph on a surface such that edges only intersect at vertices.

**Embedding on surfaces of higher genus.** A graph that is not planar may be embeddable on a surface with handles (higher genus). The minimum genus of a surface on which a graph can be embedded is the graph's genus.

- $K_5$ has genus 1 (embeds on a torus)
- $K_{3,3}$ has genus 1 (embeds on a torus)
- $K_n$ has genus $\lceil (n-3)(n-4)/12 \rceil$

```python
def complete_graph_genus(n):
    """Genus of the complete graph K_n."""
    if n <= 4:
        return 0
    return math.ceil((n - 3) * (n - 4) / 12)
```

**Graph embedding in 3D.** Any graph can be embedded in $\mathbb{R}^3$ without edge crossings (just place vertices on a curve). This is used in 3D graph visualization and molecular structure modeling.

**Embedding for visualization (force-directed placement):**

```python
def force_directed_layout(adjacency, iterations=100, repulsion=1000, attraction=0.01):
    """Compute a 2D embedding of a graph using force-directed placement."""
    import random
    n = len(adjacency)
    positions = [(random.random() * 100, random.random() * 100) for _ in range(n)]
    velocities = [(0, 0) for _ in range(n)]

    for _ in range(iterations):
        forces = [(0, 0) for _ in range(n)]

        # Repulsive forces (all pairs)
        for i in range(n):
            for j in range(i + 1, n):
                dx = positions[i][0] - positions[j][0]
                dy = positions[i][1] - positions[j][1]
                dist = math.sqrt(dx*dx + dy*dy) + 1e-6
                force = repulsion / (dist * dist)
                fx = force * dx / dist
                fy = force * dy / dist
                forces[i] = (forces[i][0] + fx, forces[i][1] + fy)
                forces[j] = (forces[j][0] - fx, forces[j][1] - fy)

        # Attractive forces (edges)
        for i in range(n):
            for j in adjacency[i]:
                if i < j:
                    dx = positions[i][0] - positions[j][0]
                    dy = positions[i][1] - positions[j][1]
                    dist = math.sqrt(dx*dx + dy*dy) + 1e-6
                    force = attraction * dist
                    fx = force * dx / dist
                    fy = force * dy / dist
                    forces[i] = (forces[i][0] - fx, forces[i][1] - fy)
                    forces[j] = (forces[j][0] + fx, forces[j][1] + fy)

        # Update positions
        for i in range(n):
            velocities[i] = (velocities[i][0] * 0.9 + forces[i][0] * 0.1,
                             velocities[i][1] * 0.9 + forces[i][1] * 0.1)
            positions[i] = (positions[i][0] + velocities[i][0],
                            positions[i][1] + velocities[i][1])

    return positions
```

### Knot Theory Basics

Knot theory studies embeddings of circles in $\mathbb{R}^3$. A knot is a closed, non-self-intersecting curve in 3D space.

**Knot types (up to ambient isotopy):**

- **Unknot (trivial knot):** A simple loop that can be deformed to a circle
- **Trefoil knot ($3_1$):** The simplest non-trivial knot, a loop with three crossings
- **Figure-eight knot ($4_1$):** Has four crossings
- **Granny knot:** Connected sum of two trefoils
- **Reef (square) knot:** Connected sum of trefoil and its mirror image

**Knot diagrams.** A knot is drawn on the plane with over/under crossing information:

```
Trefoil knot (3_1):
   ___     ___
  /   \   /   \
 /     \_/     \
 \             /
  \    ___    /
   \  /   \  /
    \/     \/
```

**Knot invariants.** To distinguish knots, we compute invariants:

- **Crossing number:** Minimum number of crossings across all diagrams
- **Linking number:** For links (multiple loops), counts how many times one component winds around another
- **Tricolorability:** Whether a knot can be colored with 3 colors following rules at crossings
- **Jones polynomial:** A polynomial invariant discovered in 1984

**Reidemeister moves.** Three local moves that transform a knot diagram without cutting the knot:

1. Twist/untwist (Type I)
2. Move one strand over another (Type II)
3. Move a strand over/under a crossing (Type III)

Two diagrams represent the same knot if and only if they are connected by a sequence of Reidemeister moves.

**Knot theory in computing:**

- **DNA topology:** DNA strands form knots; topoisomerase enzymes change knot type
- **Quantum computing:** Anyons in topological quantum computers braid like strands
- **Molecule structure:** Knots in proteins (e.g., human ubiquitin) affect function
- **Graphics:** Knot visualization for educational and artistic purposes

```python
def trefoil_knot(t):
    """Parametric trefoil knot."""
    x = math.sin(t) + 2 * math.sin(2 * t)
    y = math.cos(t) - 2 * math.cos(2 * t)
    z = -math.sin(3 * t)
    return (x, y, z)

def torus_knot(p, q, t):
    """Parametric (p,q) torus knot."""
    r = math.cos(q * t) + 2
    x = r * math.cos(p * t)
    y = r * math.sin(p * t)
    z = -math.sin(q * t)
    return (x, y, z)
```

### Topological Data Analysis

Topological Data Analysis (TDA) applies topology to extract shape information from data. It is particularly useful for high-dimensional data where traditional visualization fails.

**The core idea:** Data points sampled from an unknown space $X$ carry topological information about $X$. By building simplicial complexes at varying scales and tracking how homology changes, we infer the persistent topological features of $X$.

**Simplicial complexes from data:**

- **Vietoris-Rips complex:** $k$ points form a simplex if all pairwise distances are less than $\varepsilon$
- **Alpha (Cech) complex:** $k$ points form a simplex if the intersection of their $\varepsilon$-balls is non-empty
- **Witness complex:** A subset of points are witnesses for simplices formed by landmarks

```python
def rips_complex(points, epsilon):
    """Build the Vietoris-Rips simplicial complex at scale epsilon."""
    n = len(points)
    simplices = {0: [(i,) for i in range(n)]}  # 0-simplices

    # 1-simplices
    edges = []
    for i in range(n):
        for j in range(i + 1, n):
            if euclidean_dist(points[i], points[j]) < epsilon:
                edges.append((i, j))
    simplices[1] = edges

    # 2-simplices
    triangles = []
    edge_set = set(tuple(sorted(e)) for e in edges)
    for i in range(n):
        for j in range(i + 1, n):
            for k in range(j + 1, n):
                if ((i, j) in edge_set and (i, k) in edge_set and
                    (j, k) in edge_set):
                    triangles.append((i, j, k))
    simplices[2] = triangles

    return simplices
```

**Challenges in TDA:**

- **Scale selection:** What is the right $\varepsilon$? (Persistent homology solves this)
- **Computational cost:** Vietoris-Rips is $O(n^2)$ in edges alone
- **Interpretation:** What do topological features mean for the application?

### Persistent Homology

Persistent homology tracks how homology groups change as a scale parameter $\varepsilon$ increases. Features that persist over a wide range of $\varepsilon$ are considered "true" topological features.

**The pipeline:**

1. Build simplicial complexes at increasing $\varepsilon$ values
2. Compute homology groups $H_k$ at each scale
3. Track when each $k$-dimensional hole appears (birth) and disappears (death)
4. Visualize as a persistence diagram or barcode

**Persistence diagram.** A scatter plot where each point $(b, d)$ represents a topological feature born at scale $b$ and dying at scale $d$.

- Points near the diagonal are likely noise (short-lived)
- Points far from the diagonal are persistent features
- Points with $d = \infty$ represent essential features of the space

```
Persistence Diagram (conceptual):
d
^
|  *  persistent H1 (loop)
|     *  persistent H1 (loop)
|        *  *  * noise near diagonal
+-----------------> b
```

**Persistence barcode.** A set of horizontal lines, each representing a topological feature from its birth to its death.

```python
def compute_persistence_h0(distances, epsilons):
    """Compute 0-dimensional persistent homology (connected components).
    Uses single-linkage clustering.
    """
    n = len(distances)
    # Union-Find data structure
    parent = list(range(n))

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(x, y):
        px, py = find(x), find(y)
        if px != py:
            parent[px] = py
            return True
        return False

    # Sort edges by distance
    edges = [(distances[i][j], i, j)
             for i in range(n) for j in range(i + 1, n)]
    edges.sort(key=lambda e: e[0])

    components = n
    birth = {i: (0, i) for i in range(n)}  # all born at 0
    persistence = []

    for dist, i, j in edges:
        if union(i, j):
            components -= 1
            if components == 1:
                # Last component death
                persistence.append((0, dist))  # (birth, death)

    return persistence
```

**Applications of persistent homology:**

- **Shape recognition:** Classify shapes by their persistence signatures
- **Sensor networks:** Determine coverage holes using homology
- **Neuroscience:** Analyze neural activity patterns
- **Material science:** Characterize pore structures in materials

### The Mapper Algorithm

Mapper constructs a topological summary of high-dimensional data as a graph (the "Mapper graph") that captures the data's shape.

**Algorithm:**

1. Project data onto a low-dimensional space using a filter function $f$
2. Cover the range of $f$ with overlapping intervals
3. For each interval, cluster the points in that interval
4. Create a node for each cluster
5. Connect nodes whose clusters share points

```python
def mapper_algorithm(data, filter_function, n_intervals=10, overlap=0.5):
    """Simplified Mapper algorithm."""
    # Compute filter values
    filter_values = [filter_function(p) for p in data]
    f_min, f_max = min(filter_values), max(filter_values)

    # Create overlapping intervals
    step = (f_max - f_min) / (n_intervals * (1 - overlap))
    intervals = []
    for i in range(n_intervals):
        low = f_min + i * step * (1 - overlap)
        high = low + step + overlap * step / (1 - overlap)
        intervals.append((low, high))

    # Cluster within each interval (using DBSCAN, k-means, etc.)
    clusters = []
    for low, high in intervals:
        in_interval = [data[i] for i in range(len(data))
                      if low <= filter_values[i] <= high]
        if len(in_interval) >= 2:
            # In practice, use a clustering algorithm
            # Here we just treat each point as its own cluster for illustration
            clusters.extend(Clustering().fit_predict(in_interval))

    # Build graph: nodes are clusters, edges if clusters share points
    return build_mapper_graph(clusters)
```

**Filter functions for Mapper:**

- **Distance to a point:** $f(x) = \|x - p\|$
- **Eccentricity:** $f(x) = \sum \|x - x_i\|$ or $f(x) = \max \|x - x_i\|$
- **PCA coordinates:** $f(x) = \text{PCA}_1(x)$ (first principal component)
- **Density estimators:** $f(x) = \text{k-NN density estimate}$

**Applications of Mapper:**

- **Exploratory data analysis:** Find structure in high-dimensional data
- **Patient stratification:** Discover disease subtypes from genomic data
- **Anomaly detection:** Outliers appear as disconnected components
- **Feature engineering:** Mapper graph features improve ML models

### Applications in Computing

**PCB design.** Printed circuit board routing is a planar graph problem. Traces on a single layer must not cross. Multi-layer boards use vias to move traces between layers:

```python
def route_pcb(netlist, layers):
    """Route a PCB netlist (simplified)."""
    # Each net is a set of pins that must be connected
    # Traces on the same layer must not cross (planar constraint)
    # Vias connect traces on different layers
    # This is NP-hard in general; commercial tools use heuristics
    pass
```

**3D mesh topology.** A 3D mesh's topology determines how vertices connect:

```python
def mesh_statistics(vertices, faces):
    """Compute topological statistics of a triangle mesh."""
    V = len(vertices)
    F = len(faces)
    edges = set()
    for f in faces:
        for i in range(3):
            edges.add(tuple(sorted((f[i], f[(i+1)%3]))))
    E = len(edges)

    chi = V - E + F
    genus = (2 - chi) // 2

    # Vertex valence histogram
    valence = {i: 0 for i in range(V)}
    for e in edges:
        valence[e[0]] += 1
        valence[e[1]] += 1

    avg_valence = sum(valence.values()) / V

    return {
        "vertices": V,
        "edges": E,
        "faces": F,
        "euler_characteristic": chi,
        "genus": genus,
        "avg_valence": avg_valence,
        "manifold": all(v == 2 for v in valence.values())
    }
```

**Molecular modeling.** Molecules can be modeled as graphs (atoms are vertices, bonds are edges). Topological features like ring structures and molecular shape affect chemical properties:

```python
def find_rings_in_molecule(molecule_graph):
    """Find ring structures in a molecular graph (e.g., benzene rings)."""
    # Find cycles in the molecular graph
    # Benzene has 6-cycles
    # Ring structures affect molecular stability and reactivity
    pass
```

**Sensor network coverage.** Homology detects coverage holes in wireless sensor networks:

```python
def detect_coverage_holes(sensor_positions, sensing_radius):
    """Detect coverage holes using persistent homology."""
    # Build Vietoris-Rips complex from sensor positions
    # Coverage hole exists if there's a 1-cycle in H1
    # that persists across scales
    pass
```

**Node-link diagrams for graph visualization.** The topology of node-link diagrams affects readability. Planar embeddings with minimal crossings are preferred:

```python
def minimize_crossings(layers, edges):
    """Barycenter heuristic for crossing minimization in layered graphs."""
    # Assign y-positions within each layer to minimize edge crossings
    # This is NP-hard; heuristic methods (barycenter, median) are used
    pass
```

## Glossary

| Term | Definition |
|------|------------|
| Betti number | The $k$th Betti number $b_k$ counts $k$-dimensional holes |
| Crosscap number | The number of crosscaps needed for a non-orientable surface embedding |
| Crossing number | The minimum number of crossings in a knot diagram |
| Euler characteristic | $V - E + F$ for a cell complex, a topological invariant |
| Genus | The number of handles on an orientable surface |
| Graph embedding | A drawing of a graph on a surface such that edges meet only at vertices |
| Homeomorphism | A continuous bijection with continuous inverse |
| Jones polynomial | A polynomial invariant of knots |
| Knot | An embedding of a circle in $\mathbb{R}^3$ |
| Kuratowski's theorem | A graph is planar iff it contains no $K_5$ or $K_{3,3}$ subdivision |
| Linking number | A numerical invariant for links (multiple interlocked loops) |
| Mapper | An algorithm that constructs a topological summary of data as a graph |
| Non-orientable | A surface with only one side (Mobius strip, Klein bottle, projective plane) |
| Persistent homology | Homology that tracks features across multiple scales |
| Persistence diagram | A plot of birth vs. death times for topological features |
| Planar graph | A graph that can be drawn in the plane without edge crossings |
| Reidemeister moves | Three local moves that relate knot diagrams of the same knot |
| Simplicial complex | A collection of simplices (points, edges, triangles, etc.) |
| Topological data analysis | The use of topology to analyze the shape of data |
| Tricolorability | A knot invariant based on coloring with three colors |
| Unknot | The trivial knot (a simple loop) |
| Vietoris-Rips complex | A simplicial complex built from points within distance $\varepsilon$ |

## Quick References

- [KnotInfo](https://knotinfo.math.indiana.edu/) — comprehensive knot data and invariants
- [Gudhi Library](https://gudhi.inria.fr/) — TDA software with Python bindings
- [scikit-tda](https://scikit-tda.org/) — Python TDA toolkit
- [Planar Graph Coloring (Four Color Theorem)](https://www.maths.ed.ac.uk/~v1ranick/papers/robertsond.pdf) — proof and implications
- [PCB Routing and Planarity](https://www.cadence.com/en_US/home/tools/pcb-design-and-analysis.html) — industry PCB design tools
- [Red Blob Games: Graph Visualization](https://www.redblobgames.com/pathfinding/a-star/introduction.html) — interactive graph algorithms
- [3Blue1Brown: Euler Characteristic](https://www.youtube.com/watch?v=7utHf4P4lXk) — visual introduction

## Next Steps

- This is the final document in the Geometry & Topology module. Review the [index](index.md) for a full list of available topics.
