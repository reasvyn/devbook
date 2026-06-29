# Graph Theory

## Description

Graphs are the mathematical model of networks. Every connection between things — social relationships, network routes, web page links, dependency chains, state transitions — is a graph. Understanding graph theory gives you the language to reason about connectivity, optimization, and structure in any networked system.

## Prerequisites

- [Set Theory](set-theory.md) — sets, ordered pairs, Cartesian products
- [Functions & Relations](functions-and-relations.md) — relations, adjacency as a relation

## Table of Contents

- [Graphs and Digraphs](#graphs-and-digraphs)
- [Graph Representations](#graph-representations)
- [Paths and Cycles](#paths-and-cycles)
- [Trees and Forests](#trees-and-forests)
- [Spanning Trees](#spanning-trees)
- [Graph Coloring](#graph-coloring)
- [Bipartite Graphs](#bipartite-graphs)
- [Matchings](#matchings)
- [Planar Graphs](#planar-graphs)
- [Eulerian and Hamiltonian Paths](#eulerian-and-hamiltonian-paths)
- [Applications in Computing](#applications-in-computing)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Graphs and Digraphs

An **undirected graph** G = (V, E) consists of a set of vertices V and a set of edges E, where each edge is an unordered pair {u, v} with u, v in V.

A **directed graph** (digraph) uses ordered pairs (u, v) for edges, indicating direction from u to v.

**Terminology:**

- **Adjacent:** two vertices connected by an edge
- **Incident:** an edge is incident to its endpoints
- **Degree deg(v):** number of edges incident to v (for undirected)
- **In-degree / out-degree:** for directed graphs, count of incoming / outgoing edges
- **Isolated vertex:** degree 0
- **Loop:** an edge from a vertex to itself
- **Simple graph:** no loops, no multiple edges between same vertices
- **Multigraph:** allows multiple edges between same vertices
- **Complete graph Kn:** every pair of vertices is connected
- **Complement of G:** graph on same vertices with edges exactly where G has none

**Handshaking lemma:** The sum of degrees equals twice the number of edges:

$$\sum_{v \in V} \deg(v) = 2|E|$$

For directed graphs: sum of in-degrees = sum of out-degrees = |E|.

```python
class Graph:
    def __init__(self, vertices=None):
        self.vertices = set(vertices or [])
        self.edges = set()

    def add_edge(self, u, v):
        self.vertices.add(u)
        self.vertices.add(v)
        self.edges.add((u, v))

    def degree(self, v):
        return sum(1 for u, w in self.edges if u == v or w == v)

    def handshaking(self):
        total = sum(self.degree(v) for v in self.vertices)
        return total, total // 2  # (sum of degrees, number of edges)
```

### Graph Representations

**Adjacency matrix:** An n x n matrix A where A[i][j] = 1 if there is an edge from i to j, else 0.

$$A = \begin{bmatrix}
0 & 1 & 1 & 0 \\
1 & 0 & 0 & 1 \\
1 & 0 & 0 & 1 \\
0 & 1 & 1 & 0
\end{bmatrix}$$

**Properties:**

- Undirected graphs have symmetric adjacency matrices
- The number of walks of length k from i to j is (A^k)[i][j]
- Space: O(V^2) — good for dense graphs, bad for sparse

**Adjacency list:** For each vertex, store a list of its neighbors.

```python
class AdjacencyList:
    def __init__(self):
        self.adj = {}

    def add_edge(self, u, v):
        self.adj.setdefault(u, []).append(v)
        self.adj.setdefault(v, []).append(u)  # undirected

    def neighbors(self, v):
        return self.adj.get(v, [])
```

```python
# Comparison: matrix vs list
def matrix_vs_list(n, edge_density):
    """Compare memory usage for different representations."""
    import sys
    # Matrix: n^2 booleans
    matrix_size = n * n * 1  # 1 byte per entry
    # List: vertices + 2 * edges * references
    edges = int(n * (n-1) / 2 * edge_density)
    list_size = n * 8 + 2 * edges * 8  # rough estimate
    return matrix_size, list_size

for n in [100, 1000, 10000]:
    m, l = matrix_vs_list(n, 0.01)  # 1% density
    print(f"n={n}: matrix={m//1024}KB, list={l//1024}KB")
    # For sparse graphs (1% edges), adjacency list wins massively
```

**Edge list:** A simple list of (u, v) pairs.

```python
edges = [(1, 2), (1, 3), (2, 4), (3, 4)]
```

**Tradeoffs:**

| Operation | Adjacency Matrix | Adjacency List |
|-----------|-----------------|----------------|
| Edge lookup | O(1) | O(deg(v)) |
| Iterate neighbors | O(V) | O(deg(v)) |
| Space | O(V^2) | O(V + E) |
| Add edge | O(1) | O(1) |
| Remove edge | O(1) | O(deg(v)) |

### Paths and Cycles

A **path** is a sequence of vertices v1, v2, ..., vk such that each consecutive pair is an edge. A **simple path** has no repeated vertices.

A **cycle** is a path v1, ..., vk where v1 = vk and no other vertices repeat. A **simple cycle** has no repeated vertices.

**Path length:** number of edges in the path.

**Connectivity:**

- An undirected graph is **connected** if there is a path between every pair of vertices
- A digraph is **strongly connected** if there is a directed path from any vertex to any other
- A digraph is **weakly connected** if the underlying undirected graph is connected
- **Connected components:** maximal connected subgraphs

```python
def is_connected_bfs(graph, start):
    visited = set()
    queue = [start]
    while queue:
        v = queue.pop(0)
        if v in visited:
            continue
        visited.add(v)
        for neighbor in graph.neighbors(v):
            if neighbor not in visited:
                queue.append(neighbor)
    return len(visited) == len(graph.vertices)

def connected_components(graph):
    remaining = set(graph.vertices)
    components = []
    while remaining:
        start = remaining.pop()
        visited = set()
        queue = [start]
        while queue:
            v = queue.pop(0)
            if v in visited:
                continue
            visited.add(v)
            for neighbor in graph.neighbors(v):
                if neighbor not in visited:
                    queue.append(neighbor)
        components.append(visited)
        remaining -= visited
    return components
```

**Shortest paths — Dijkstra's algorithm:**

For weighted graphs with non-negative weights, Dijkstra finds the shortest path from a source to all vertices:

```python
import heapq
import math

def dijkstra(graph, start):
    distances = {v: math.inf for v in graph.vertices}
    distances[start] = 0
    pq = [(0, start)]

    while pq:
        dist, v = heapq.heappop(pq)
        if dist > distances[v]:
            continue
        for neighbor, weight in graph.neighbors(v):
            new_dist = dist + weight
            if new_dist < distances[neighbor]:
                distances[neighbor] = new_dist
                heapq.heappush(pq, (new_dist, neighbor))
    return distances
```

### Trees and Forests

A **tree** is a connected acyclic undirected graph. A **forest** is a disjoint union of trees.

**Properties of trees (all equivalent for a simple graph on n vertices):**

- Connected and acyclic
- Connected with n-1 edges
- Acyclic with n-1 edges
- Exactly one path between any two vertices
- Adding any edge creates exactly one cycle
- Removing any edge disconnects the graph

**Rooted trees:** Designate one vertex as the root. Then:

- **Parent:** the vertex closer to the root
- **Child:** a vertex further from the root
- **Leaf:** a vertex with no children
- **Depth:** distance from root
- **Height:** maximum depth
- **Subtree:** a vertex and all its descendants

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.children = []
        self.parent = None

    def add_child(self, child):
        self.children.append(child)
        child.parent = self

    def depth(self):
        d = 0
        node = self
        while node.parent:
            node = node.parent
            d += 1
        return d

    def height(self):
        if not self.children:
            return 0
        return 1 + max(child.height() for child in self.children)

    def traverse(self, order='pre'):
        if order == 'pre':
            yield self
        for child in self.children:
            yield from child.traverse(order)
        if order == 'post':
            yield self
```

**Binary trees:** Every node has at most 2 children (left and right). Applications:

- Binary search trees
- Heaps
- Expression trees
- Huffman coding trees
- Decision trees

**Properties of binary trees:**

- A full binary tree of height h has at most 2^(h+1) - 1 nodes
- A complete binary tree of n nodes has height floor(log2(n))

### Spanning Trees

A **spanning tree** of a connected graph is a subgraph that is a tree and includes all vertices.

**Minimum spanning tree (MST):** A spanning tree with minimum total edge weight.

**Kruskal's algorithm:** Sort edges by weight, add edges that don't create cycles:

```python
class DisjointSet:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        xr, yr = self.find(x), self.find(y)
        if xr == yr:
            return False
        if self.rank[xr] < self.rank[yr]:
            self.parent[xr] = yr
        elif self.rank[xr] > self.rank[yr]:
            self.parent[yr] = xr
        else:
            self.parent[yr] = xr
            self.rank[xr] += 1
        return True

def kruskal(vertices, edges):
    """Return MST. edges: list of (weight, u, v)"""
    ds = DisjointSet(len(vertices))
    mst = []
    for weight, u, v in sorted(edges):
        if ds.union(u, v):
            mst.append((weight, u, v))
    return mst
```

**Prim's algorithm:** Grow the MST from a starting vertex, always adding the cheapest edge connecting the tree to a non-tree vertex:

```python
import heapq

def prim(vertices, adjacency):
    """adjacency: dict of {v: [(weight, neighbor), ...]}"""
    start = next(iter(vertices))
    visited = {start}
    mst = []
    edges = [(w, start, n) for w, n in adjacency[start]]
    heapq.heapify(edges)

    while edges and len(visited) < len(vertices):
        w, u, v = heapq.heappop(edges)
        if v in visited:
            continue
        visited.add(v)
        mst.append((u, v, w))
        for w2, n2 in adjacency[v]:
            if n2 not in visited:
                heapq.heappush(edges, (w2, v, n2))
    return mst
```

**Applications of MST:**

- Network design (laying cable, connecting offices)
- Clustering (single-linkage clustering)
- Approximation algorithms (traveling salesman)

### Graph Coloring

A **proper coloring** assigns colors to vertices such that adjacent vertices have different colors.

**Chromatic number chi(G):** The minimum number of colors needed for a proper coloring.

**Bounds:**

- chi(G) <= Delta(G) + 1 (where Delta is max degree) — Brooks' theorem
- chi(G) >= omega(G) (clique number)
- Planar graphs: chi(G) <= 4 (Four Color Theorem)
- Trees: chi(G) = 2 (for any non-trivial tree)

```python
def greedy_coloring(graph):
    """Assign colors using a greedy algorithm."""
    colors = {}
    for v in graph.vertices:
        used = {colors.get(n) for n in graph.neighbors(v) if n in colors}
        color = 0
        while color in used:
            color += 1
        colors[v] = color
    return colors
```

**Graph coloring applications:**

- **Register allocation:** Graph coloring is used in compilers to assign variables to CPU registers. Each variable is a vertex, edges connect variables that are live at the same time. The chromatic number is the minimum number of registers needed.

- **Exam scheduling:** Courses are vertices, edges connect courses that share students. Colors represent exam time slots. The chromatic number is the minimum number of time slots.

- **Frequency assignment:** Radio towers are vertices, edges connect towers with overlapping coverage. Colors represent frequencies.

```python
# Register allocation example
variables = {'a', 'b', 'c', 'd', 'e'}
interference = {
    'a': {'b', 'c'},
    'b': {'a', 'd'},
    'c': {'a', 'd', 'e'},
    'd': {'b', 'c'},
    'e': {'c'}
}
colors = greedy_coloring(type('obj', (object,), {'vertices': variables, 'neighbors': lambda self, v: interference.get(v, [])})())
# If chi <= 3, we need at most 3 registers
print(f"Colors assigned: {colors}")
```

### Bipartite Graphs

A graph is **bipartite** if its vertices can be partitioned into two sets X and Y such that every edge connects a vertex in X to a vertex in Y.

**Equivalent conditions:**

- The graph contains no odd cycles
- The graph is 2-colorable
- All cycles are even length

**Checking bipartiteness (BFS):**

```python
def is_bipartite(graph):
    color = {}
    for start in graph.vertices:
        if start in color:
            continue
        queue = [start]
        color[start] = 0
        while queue:
            v = queue.pop(0)
            for n in graph.neighbors(v):
                if n not in color:
                    color[n] = 1 - color[v]
                    queue.append(n)
                elif color[n] == color[v]:
                    return False
    return True
```

**Complete bipartite graph K(m,n):** Every vertex in set X (size m) connects to every vertex in set Y (size n). Has m*n edges.

**Applications:**

- **Matching problems** — assigning tasks to workers, ads to slots
- **Recommender systems** — users and items form a bipartite graph
- **Network flow** — bipartite graphs model source-sink relationships
- **Boolean formulas** — 2-SAT can be modeled on implication graphs

### Matchings

A **matching** in a graph is a set of edges with no shared vertices. A **maximum matching** uses the most possible edges. A **perfect matching** covers all vertices.

**Hall's marriage theorem:** In a bipartite graph with parts X and Y, there exists a matching that covers X iff for every subset S of X, |N(S)| >= |S|, where N(S) is the set of neighbors of S.

```python
def maximum_bipartite_matching(adj_x_to_y):
    """adj_x_to_y: dict mapping X vertices to list of Y vertices.
    Returns dict mapping X to matched Y, or None."""
    match_y = {}  # Y -> X

    def dfs(x, visited):
        for y in adj_x_to_y[x]:
            if y not in visited:
                visited.add(y)
                if y not in match_y or dfs(match_y[y], visited):
                    match_y[y] = x
                    return True
        return False

    result = {}
    for x in adj_x_to_y:
        if dfs(x, set()):
            result[x] = match_y.get(x)
    return result  # only matched X vertices
```

**Applications:**

- **Job assignment:** Workers matched to jobs based on qualifications
- **Dating apps:** Users matched based on preferences
- **Network switches:** Packets matched to output ports
- **Chessboards:** Tiling with dominoes

### Planar Graphs

A graph is **planar** if it can be drawn in the plane without edge crossings.

**Euler's formula for connected planar graphs:**

$$V - E + F = 2$$

Where V = vertices, E = edges, F = faces (including the outer face).

**Consequences:**

- For a planar graph with V >= 3: E <= 3V - 6
- For a bipartite planar graph: E <= 2V - 4
- K5 and K(3,3) are non-planar (Kuratowski's theorem)
- Every planar graph has a vertex of degree <= 5

```python
def planar_edge_bound(V):
    """Maximum edges in a simple planar graph with V vertices."""
    return 3 * V - 6 if V >= 3 else V * (V - 1) // 2

for V in [4, 10, 100]:
    max_e = planar_edge_bound(V)
    complete_e = V * (V - 1) // 2
    print(f"V={V}: planar max E={max_e}, complete E={complete_e}")
```

**Applications of planarity:**

- **PCB design:** Circuit boards must avoid crossing traces
- **Map coloring:** The Four Color Theorem applies to planar maps
- **Graph visualization:** Planar drawings are more readable

### Eulerian and Hamiltonian Paths

An **Eulerian trail** is a trail that traverses every edge exactly once. An **Eulerian circuit** starts and ends at the same vertex.

**Euler's theorem:**

- A connected graph has an Eulerian circuit iff every vertex has even degree
- A connected graph has an Eulerian trail (not circuit) iff exactly two vertices have odd degree

```python
def has_eulerian_circuit(graph):
    if len(connected_components(graph)) > 1:
        return False
    return all(graph.degree(v) % 2 == 0 for v in graph.vertices)

def has_eulerian_trail(graph):
    if len(connected_components(graph)) > 1:
        return False
    odd = sum(1 for v in graph.vertices if graph.degree(v) % 2 == 1)
    return odd == 0 or odd == 2
```

**Fleury's algorithm** finds an Eulerian trail by avoiding bridges when possible.

A **Hamiltonian path** visits every vertex exactly once. A **Hamiltonian cycle** starts and ends at the same vertex, visiting all vertices.

**Conditions (sufficient but not necessary):**

- **Dirac's theorem:** If every vertex has degree >= V/2, the graph has a Hamiltonian cycle
- **Ore's theorem:** If for every non-adjacent pair (u,v), deg(u) + deg(v) >= V, there is a Hamiltonian cycle

```python
def dirac_condition(graph):
    n = len(graph.vertices)
    return all(graph.degree(v) >= n / 2 for v in graph.vertices)

def ore_condition(graph):
    n = len(graph.vertices)
    vertices = list(graph.vertices)
    for i in range(n):
        for j in range(i+1, n):
            u, v = vertices[i], vertices[j]
            if not has_edge(graph, u, v):
                if graph.degree(u) + graph.degree(v) < n:
                    return False
    return True
```

**The Hamiltonian path problem is NP-complete** — no known polynomial-time algorithm. The Traveling Salesman Problem (TSP) asks for the shortest Hamiltonian cycle in a weighted complete graph.

**Applications:**

- **Routing:** Delivery routes, circuit board drilling (TSP)
- **Genome assembly:** Hamiltonian paths in overlap graphs
- **Network analysis:** Finding Hamiltonian cycles in network topologies

## Applications in Computing

**Web crawling and PageRank:**

The web is a directed graph where pages are vertices and hyperlinks are edges. PageRank computes importance by simulating a random walk:

```python
import numpy as np

def pagerank(adj_matrix, damping=0.85, iterations=100):
    n = adj_matrix.shape[0]
    # Transition matrix from adjacency
    out_degrees = adj_matrix.sum(axis=1)
    transition = adj_matrix / out_degrees[:, np.newaxis]
    # Handle dangling nodes (pages with no outgoing links)
    transition = np.nan_to_num(transition)
    # Initialize uniform PageRank
    pr = np.ones(n) / n
    for _ in range(iterations):
        pr = damping * transition.T @ pr + (1 - damping) / n
    return pr
```

**Dependency resolution (npm, pip, apt):**

Package dependencies form a directed graph. The package manager must:

1. Detect cycles in the dependency graph
2. Compute a topological order
3. Find a consistent set of versions that satisfies all constraints

```python
def detect_cycle(graph):
    """Detect cycle using DFS coloring (0=white, 1=gray, 2=black)."""
    color = {v: 0 for v in graph.vertices}

    def dfs(v):
        color[v] = 1
        for n in graph.neighbors(v):
            if color.get(n) == 1:  # back edge
                return True
            if color.get(n) == 0 and dfs(n):
                return True
        color[v] = 2
        return False

    for v in graph.vertices:
        if color[v] == 0:
            if dfs(v):
                return True
    return False
```

**Social network analysis:**

Friendship graphs are analyzed for:

- **Connected components** — communities in the network
- **Clustering coefficient** — how tightly knit communities are
- **Betweenness centrality** — which users bridge different groups
- **PageRank** — influential users

```python
def clustering_coefficient(graph):
    """Average clustering coefficient for undirected graph."""
    total = 0
    for v in graph.vertices:
        neighbors = list(graph.neighbors(v))
        k = len(neighbors)
        if k < 2:
            continue
        # Count edges among neighbors
        edges = sum(1 for i in range(k) for j in range(i+1, k)
                    if has_edge(graph, neighbors[i], neighbors[j]))
        total += 2 * edges / (k * (k - 1))
    return total / len(graph.vertices)
```

**Shortest path in routing:**

The internet's routing protocols (OSPF, BGP) compute shortest paths:

```python
# OSPF uses Dijkstra's algorithm to compute the shortest path tree
# BGP uses path-vector routing (paths are sequences of AS numbers)
# Every router maintains a routing table:
#   (destination network, next hop, metric)
```

**Scheduling with topological order:**

Build systems like Make and Bazel use graph theory:

```python
# Build graph: targets -> dependencies
# Parallel build: schedule independent targets simultaneously
def parallel_build_schedule(dependency_graph):
    """Return list of levels: each level can be built in parallel."""
    in_degree = {}
    for v in dependency_graph.vertices:
        in_degree[v] = 0
    for u, v in dependency_graph.edges:
        in_degree[v] = in_degree.get(v, 0) + 1

    levels = []
    current = [v for v, d in in_degree.items() if d == 0]
    while current:
        levels.append(current)
        next_level = []
        for v in current:
            for n in dependency_graph.neighbors(v):
                in_degree[n] -= 1
                if in_degree[n] == 0:
                    next_level.append(n)
        current = next_level
    return levels
```

**State machines as graphs:**

Finite state machines are directed graphs where vertices are states and edges are transitions. Used in:

- Regular expressions (NFAs, DFAs)
- UI navigation
- Protocol specifications
- Compiler lexers

## Study Cases

### Case 1: Minimum Spanning Tree for Network Design

**Problem:** A company needs to connect 6 offices with fiber optic cable. The cost of connecting each pair is known. Find the cheapest way to connect all offices.

```python
offices = ['NYC', 'BOS', 'DC', 'CHI', 'SF', 'LA']
edges = [
    (100, 'NYC', 'BOS'), (200, 'NYC', 'DC'),
    (150, 'NYC', 'CHI'), (500, 'NYC', 'SF'),
    (180, 'BOS', 'DC'),  (250, 'BOS', 'CHI'),
    (220, 'DC', 'CHI'),  (600, 'DC', 'SF'),
    (300, 'CHI', 'SF'),  (400, 'CHI', 'LA'),
    (100, 'SF', 'LA'),
]

# Use Kruskal's algorithm
ds = DisjointSet(len(offices))
office_to_idx = {o: i for i, o in enumerate(offices)}
mst = []
for weight, u, v in sorted(edges):
    if ds.union(office_to_idx[u], office_to_idx[v]):
        mst.append((u, v, weight))
        print(f"Connect {u} - {v}: ${weight}")

total = sum(w for _, _, w in mst)
print(f"Total cost: ${total}")
# This creates a tree connecting all offices at minimum cost
```

The MST guarantees that adding any edge would create a cycle, and removing any edge would disconnect the network. This is optimal for physical network topology.

### Case 2: Shortest Path in a Navigation App

**Problem:** Find the fastest route from point A to point F in a road network with traffic delays.

```python
road_network = {
    'A': [(5, 'B'), (3, 'C')],
    'B': [(5, 'A'), (2, 'D'), (4, 'E')],
    'C': [(3, 'A'), (7, 'D'), (8, 'E')],
    'D': [(2, 'B'), (7, 'C'), (1, 'F')],
    'E': [(4, 'B'), (8, 'C'), (6, 'F')],
    'F': [(1, 'D'), (6, 'E')],
}

distances = dijkstra(type('obj', (object,), {
    'vertices': set(road_network.keys()),
    'neighbors': lambda self, v: road_network.get(v, [])
}), 'A')
print(distances)
# {'A': 0, 'B': 5, 'C': 3, 'D': 7, 'E': 9, 'F': 8}
# Shortest path: A -> C -> D -> F (total 8)
```

The navigation app uses Dijkstra's algorithm (or A* for larger graphs) to compute the optimal route in real time. Road networks are sparse (each intersection connects to a few others), making adjacency lists the natural representation.

### Case 3: Graph Coloring for Register Allocation

**Problem:** A compiler needs to allocate 4 variables to registers. The interference graph shows which variables cannot share a register.

```python
# Interference graph
interference = {
    'v1': {'v2', 'v3'},
    'v2': {'v1', 'v4'},
    'v3': {'v1', 'v4'},
    'v4': {'v2', 'v3'},
}

# Greedy coloring
graph_obj = type('obj', (object,), {
    'vertices': set(interference.keys()),
    'neighbors': lambda self, v: interference.get(v, set())
})()

colors = greedy_coloring(graph_obj)
print(f"Color assignment: {colors}")
# v1 -> 0, v2 -> 1, v3 -> 1, v4 -> 0
# Actually depends on ordering; chi = 2 colors needed

# If the graph is K4 (complete graph on 4 vertices),
# chi = 4, needing 4 registers
```

The compiler uses graph coloring to decide which variables to spill to memory when there aren't enough registers. If chi > available registers, some variables must be spilled.

## Examples

### Example 1: Complete Graph K5

```python
# K5: 5 vertices, each connected to every other
V = 5
edges = [(i, j) for i in range(V) for j in range(i+1, V)]
print(f"K5: {V} vertices, {len(edges)} edges")
# C(5, 2) = 10 edges

# Adjacency matrix
import numpy as np
matrix = np.zeros((V, V), dtype=int)
