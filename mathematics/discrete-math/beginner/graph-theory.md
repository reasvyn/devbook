# Graph Theory

## Description

A graph is a collection of nodes (vertices) connected by edges. Graphs model networks: social connections, web pages, road systems, dependency trees, and database relationships. Understanding graph theory is essential for backend engineers, ML engineers, and anyone building networked systems.

## Prerequisites

- [Set Theory](set-theory.md)

## Table of Contents

- [Definition and Terminology](#definition-and-terminology)
- [Types of Graphs](#types-of-graphs)
- [Graph Representation](#graph-representation)
- [Traversal: BFS and DFS](#traversal-bfs-and-dfs)
- [Shortest Paths](#shortest-paths)

## Content / Material

### Definition and Terminology

A graph $G = (V, E)$ consists of vertices $V$ and edges $E \subseteq V \times V$.

```
    A ─── B
    │     │
    C ─── D
```

- **Vertex** (or node): a single point in the graph
- **Edge**: a connection between two vertices
- **Degree**: number of edges connected to a vertex
- **Path**: a sequence of edges connecting vertices
- **Cycle**: a path that starts and ends at the same vertex

### Types of Graphs

| Type | Description | Example |
|------|-------------|---------|
| **Undirected** | Edges have no direction | Facebook friends |
| **Directed** | Edges have direction (digraph) | Twitter follows |
| **Weighted** | Edges have costs | Road distances |
| **Tree** | Acyclic connected graph | File system |
| **DAG** | Directed acyclic graph | Build dependencies |

### Graph Representation

```python
# Adjacency list (most common)
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D'],
    'C': ['A', 'D'],
    'D': ['B', 'C']
}

# Adjacency matrix
#     A  B  C  D
# A  [0, 1, 1, 0]
# B  [1, 0, 0, 1]
# C  [1, 0, 0, 1]
# D  [0, 1, 1, 0]
```

| Representation | Space | Edge Check | Iterate Neighbors |
|---------------|-------|------------|-------------------|
| Adjacency list | $O(V+E)$ | $O(\deg(v))$ | $O(\deg(v))$ |
| Adjacency matrix | $O(V^2)$ | $O(1)$ | $O(V)$ |

### Traversal: BFS and DFS

**BFS** (Breadth-First Search) — explores level by level. Finds shortest paths in unweighted graphs.

```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    while queue:
        vertex = queue.popleft()
        if vertex not in visited:
            visited.add(vertex)
            queue.extend(graph[vertex] - visited)
    return visited
```

**DFS** (Depth-First Search) — explores as far as possible before backtracking. Useful for detecting cycles, topological sort.

```python
def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()
    visited.add(start)
    for neighbor in graph[start]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)
    return visited
```

### Shortest Paths

**Dijkstra's algorithm** finds shortest paths in weighted graphs:

```python
import heapq

def dijkstra(graph, start):
    distances = {vertex: float('inf') for vertex in graph}
    distances[start] = 0
    pq = [(0, start)]
    
    while pq:
        dist, vertex = heapq.heappop(pq)
        if dist > distances[vertex]:
            continue
        for neighbor, weight in graph[vertex].items():
            new_dist = dist + weight
            if new_dist < distances[neighbor]:
                distances[neighbor] = new_dist
                heapq.heappush(pq, (new_dist, neighbor))
    return distances
```

## Glossary

| Term | Definition |
|------|------------|
| Vertex | A node in a graph |
| Edge | A connection between two vertices |
| BFS | Breadth-First Search — level-by-level traversal |
| DFS | Depth-First Search — go-deep traversal |
| Dijkstra | Algorithm for shortest paths in weighted graphs |

## Next Steps

- [Combinatorics](combinatorics.md) — counting paths, permutations, and combinations in graphs
