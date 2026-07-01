# Applications in Computing

## Description

Discrete mathematics is not an abstract pursuit — it is the mathematical substrate of every piece of software. Set theory models database queries, graph theory models network routing, combinatorics analyzes algorithm complexity, recurrence relations analyze recursive programs, and Boolean algebra underpins every conditional judgment. This document synthesizes how all discrete math topics work together in real computing systems.

## Prerequisites

- [Set Theory](set-theory.md)
- [Functions & Relations](functions-and-relations.md)
- [Combinatorics](combinatorics.md)
- [Graph Theory](graph-theory.md)
- [Recurrence Relations](recurrence-relations.md)
- [Boolean Algebra](boolean-algebra.md)

## Table of Contents

- [Algorithm Analysis](#algorithm-analysis)
- [Data Structures](#data-structures)
- [Relational Databases](#relational-databases)
- [Computer Networking](#computer-networking)
- [Cryptography](#cryptography)
- [Compilers and Programming Languages](#compilers-and-programming-languages)
- [Scheduling and Planning](#scheduling-and-planning)
- [Error Detection and Correction](#error-detection-and-correction)
- [Artificial Intelligence](#artificial-intelligence)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Algorithm Analysis

**Combinatorics + Recurrence Relations + Graph Theory**

Algorithm analysis is the most direct application of discrete math in computing. Every algorithm's time and space complexity is derived using combinatorial counting and recurrence relations.

**Counting operations:**

A simple loop that iterates over all pairs (i, j) with i < j runs exactly C(n, 2) = n(n-1)/2 times:

```python
def analyze_nested_loop(n):
    operations = 0
    for i in range(n):
        for j in range(i + 1, n):
            operations += 1
    return operations  # C(n, 2)

for n in [10, 100, 1000]:
    actual = analyze_nested_loop(n)
    formula = n * (n - 1) // 2
    print(f"n={n}: actual={actual}, n(n-1)/2={formula}")
```

The combinatorial explosion means that O(n^2) algorithms become infeasible for large n. Understanding this helps in choosing the right algorithm.

**Recurrence analysis of recursive algorithms:**

Merge sort, quicksort, binary search, tree traversals — all are analyzed via recurrences:

| Algorithm | Recurrence | Complexity |
|-----------|-----------|------------|
| Binary search | T(n) = T(n/2) + 1 | O(log n) |
| Merge sort | T(n) = 2T(n/2) + n | O(n log n) |
| Tree traversal | T(n) = T(n/2) + T(n/2) + 1 (balanced) | O(n) |
| Strassen matrix | T(n) = 7T(n/2) + O(n^2) | O(n^2.807) |

**Average-case vs worst-case:**

Combinatorics helps compute average-case complexity. For quicksort, the average number of comparisons is approximately 2n ln n, derived from the expected depth of the recursion tree:

$$E[C(n)] = \frac{2}{n} \sum_{k=0}^{n-1} E[C(k)] + n$$

This recurrence solves to Theta(n log n), making quicksort practical despite its O(n^2) worst case.

**Bit complexity:**

Analyzing cryptographic algorithms requires counting the number of bit operations:

$$T(bit) = T(bit-1) + O(b)$$

Modular exponentiation with exponent of length k bits requires O(k) multiplications, each O(k^2) bit operations, totaling O(k^3).

```python
import math

def modular_exponentiation_complexity(bits):
    """Time to compute a^b mod n where b is `bits` long."""
    # Using repeated squaring: O(bits) multiplications
    # Each multiplication is O(bits^2) bit operations
    multiplications = bits
    bit_ops_per_mul = bits ** 2
    return multiplications * bit_ops_per_mul

for bits in [256, 1024, 2048, 4096]:
    ops = modular_exponentiation_complexity(bits)
    print(f"{bits}-bit exponent: ~{ops:,} bit operations")
```

### Data Structures

**Graph Theory + Set Theory + Boolean Algebra + Relations**

**Arrays and indexing:**

Array indexing is a bijective function from {0, ..., n-1} to memory addresses:

$$address(a[i]) = base + i \times element\_size$$

This is why array access is O(1) — it is a function evaluation.

**Binary search trees:**

BSTs are rooted binary trees where the in-order traversal yields sorted order. Operations depend on tree height, which for a balanced tree is O(log n).

A BST can be seen as a set with operations that correspond to set theory:

- Lookup: check membership
- Insert: add element to set
- Delete: remove element from set
- Range query: subset extraction

```python
class BST:
    """Binary search tree as a set implementation."""
    class Node:
        def __init__(self, value):
            self.value = value
            self.left = None
            self.right = None

    def __init__(self):
        self.root = None
        self.size = 0

    def insert(self, value):
        if not self.root:
            self.root = self.Node(value)
            self.size += 1
            return
        node = self.root
        while node:
            if value < node.value:
                if node.left:
                    node = node.left
                else:
                    node.left = self.Node(value)
                    self.size += 1
                    return
            elif value > node.value:
                if node.right:
                    node = node.right
                else:
                    node.right = self.Node(value)
                    self.size += 1
                    return
            else:
                return  # already in set

    def contains(self, value):
        node = self.root
        while node:
            if value < node.value:
                node = node.left
            elif value > node.value:
                node = node.right
            else:
                return True
        return False
```

**Heaps and priority queues:**

A binary heap is a complete binary tree satisfying the heap property (parent >= children for max-heap). It represents a partial order on elements.

The heap property defines a poset: the root is the maximal element, and every parent-greater-than-child relation is a cover relation.

**Hash tables:**

Hash tables use a hash function f: keys -> bucket indices. Ideally, f is a perfect hash (bijective), but in practice collisions occur. The pigeonhole principle guarantees collisions when more keys than buckets exist.

Chaining (each bucket is a linked list) turns collision resolution into a set operation: find the key in the set of keys at that bucket.

**Graph representations of data structures:**

- Linked lists: directed path graphs
- Trees: connected acyclic graphs with a distinguished root
- Graphs: arbitrary graphs (adjacency lists or matrices)

### Relational Databases

**Set Theory + Relations + Boolean Algebra**

Relational databases are a direct implementation of set theory and relational algebra.

**SQL operations and their discrete math counterparts:**

| SQL | Discrete Math |
|-----|---------------|
| SELECT * FROM A | Return set A |
| WHERE condition | Subset: {x in A | P(x)} |
| A UNION B | A union B |
| A INTERSECT B | A intersect B |
| A EXCEPT B | A set difference B |
| CROSS JOIN | Cartesian product A x B |
| INNER JOIN ON condition | Subset of Cartesian product |
| GROUP BY | Partition by equivalence relation |
| DISTINCT | Convert multiset to set |
| ORDER BY | Total order on result set |

**Functional dependencies and normalization:**

A functional dependency X -> Y means that tuples with the same X value have the same Y value. This is a function from X to Y.

Normal forms eliminate redundancies implied by functional dependencies:

- 1NF: table is a set of rows with atomic values (set theory basics)
- 2NF: no partial dependencies on a key subset
- 3NF: no transitive dependencies through non-key attributes
- BCNF: every functional dependency is a key constraint

```sql
-- Before normalization (redundant data)
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    customer_name VARCHAR(100),  -- depends on customer_id, not order_id
    product VARCHAR(100),
    quantity INT
);

-- After 3NF
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100)
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT REFERENCES customers(customer_id),
    product VARCHAR(100),
    quantity INT
);
```

**Query optimization with Boolean algebra:**

The query optimizer rewrites WHERE clauses using Boolean identities:

```sql
-- Before optimization
SELECT * FROM orders
WHERE (status = 'shipped' OR status = 'pending')
  AND (status = 'shipped' OR amount > 100);

-- After optimization (distributive law)
SELECT * FROM orders
WHERE status = 'shipped'
   OR (status = 'pending' AND amount > 100);
```

**Bitmap indexes:**

Boolean operations on bitmap indexes enable fast set operations:

```python
# Each bitmap represents which rows satisfy a condition
bitmap_status_shipped = 0b10011010
bitmap_status_pending = 0b01100101
bitmap_amount_gt_100  = 0b11110000

# Query: (status = shipped OR status = pending) AND amount > 100
result = (bitmap_status_shipped | bitmap_status_pending) & bitmap_amount_gt_100
print(f"Rows matching: {bin(result)}")
```

### Computer Networking

**Graph Theory + Set Theory + Boolean Algebra**

The internet is a graph: routers are vertices, physical links are edges.

**Routing protocols:**

- **OSPF** (Open Shortest Path First): Uses Dijkstra's algorithm to compute shortest paths. Each router maintains an adjacency list of neighbors and computes the shortest path tree.

- **BGP** (Border Gateway Protocol): Path-vector routing. Each route advertisement is a path through AS nodes (a sequence of vertices). The routing table is a set of (destination, path, metric) tuples.

```python
def ospf_shortest_paths(router, network_graph):
    """OSPF computes shortest paths from router to all destinations."""
    # network_graph: adjacency list with link costs
    # Uses Dijkstra's algorithm
    distances = {router: 0}
    unvisited = set(network_graph.keys())
    predecessors = {}

    while unvisited:
        current = min(unvisited, key=lambda v: distances.get(v, float('inf')))
        unvisited.remove(current)
        current_dist = distances.get(current, float('inf'))

        if current_dist == float('inf'):
            break

        for neighbor, cost in network_graph[current]:
            new_dist = current_dist + cost
            if new_dist < distances.get(neighbor, float('inf')):
                distances[neighbor] = new_dist
                predecessors[neighbor] = current

    return distances, predecessors
```

**Subnetting and CIDR:**

IP addresses are 32-bit integers, and subnet masks define set membership:

```python
def ip_in_subnet(ip, subnet, mask_bits):
    """Check if IP address belongs to subnet using bitmask."""
    ip_int = ip_as_int(ip)
    subnet_int = ip_as_int(subnet)
    mask = (~0 << (32 - mask_bits)) & 0xFFFFFFFF
    return (ip_int & mask) == (subnet_int & mask)

def ip_as_int(ip_str):
    parts = [int(p) for p in ip_str.split('.')]
    return (parts[0] << 24) | (parts[1] << 16) | (parts[2] << 8) | parts[3]

print(ip_in_subnet('192.168.1.50', '192.168.1.0', 24))  # True
print(ip_in_subnet('192.168.2.50', '192.168.1.0', 24))  # False
```

**Ethernet bridging and spanning trees:**

Ethernet networks use the Spanning Tree Protocol (STP) to prevent loops. Switches run a distributed algorithm to compute a spanning tree of the network graph, deactivating redundant links.

**Firewall rules as Boolean expressions:**

Firewall rules are Boolean expressions over packet fields:

```python
def firewall_check(packet, rules):
    """rules: list of (condition_fn, action) where action is ACCEPT or DENY.
    Returns first matching action, or default DENY."""
    for condition, action in rules:
        if condition(packet):
            return action
    return 'DENY'

# Rule: allow HTTP from 10.0.0.0/8 to anywhere
def rule1(p):
    return (p['src_ip'][0:1] == b'\x0a' and  # 10.0.0.0/8
            p['dst_port'] == 80)

# Rule set as SOP expression
rules = [
    (rule1, 'ACCEPT'),
    (lambda p: True, 'DENY'),  # default deny
]
```

### Cryptography

**Number Theory + Combinatorics + Boolean Algebra**

Cryptography depends heavily on discrete math.

**Symmetric encryption (XOR):**

XOR is the fundamental operation in symmetric ciphers because it is its own inverse:

$$(A \oplus K) \oplus K = A$$

```python
def xor_encrypt(plaintext, key):
    """Symmetric encryption using XOR."""
    key_bytes = key.encode()
    return bytes(plaintext[i] ^ key_bytes[i % len(key_bytes)]
                 for i in range(len(plaintext)))

def xor_decrypt(ciphertext, key):
    return xor_encrypt(ciphertext, key)  # Same operation

msg = b"Hello, World!"
key = "KEY"
cipher = xor_encrypt(msg, key)
decrypted = xor_decrypt(cipher, key)
print(f"Original: {msg}")
print(f"Decrypted: {decrypted}")
```

**One-time pad:**

A one-time pad uses a key as long as the message, used exactly once. Bits are combined with XOR. The pigeonhole principle shows why it is unbreakable: any plaintext of length n could produce any ciphertext of length n, depending on the key.

**Hash functions:**

A hash function f: {0,1}* -> {0,1}^k maps an infinite set to a finite set. By the pigeonhole principle, collisions exist. A cryptographic hash should make finding collisions computationally infeasible.

The birthday paradox (combinatorics) tells us that finding a collision requires only approximately sqrt(2^k) = 2^{k/2} hash evaluations.

```python
import math

def birthday_bound(bits):
    """Number of hashes needed for 50% collision probability."""
    return int(math.sqrt(2 ** bits * 2 * math.log(2)))

for bits in [128, 160, 256]:
    print(f"{bits}-bit hash: {birthday_bound(bits):.0f} evaluations for collision")
    print(f"  Security level: {bits/2} bits (birthday bound)")
```

**RSA and modular arithmetic:**

RSA encryption is based on the difficulty of factoring large integers. Key generation uses:

1. Choose two large primes p, q (number theory)
2. Compute n = p * q and phi(n) = (p-1)(q-1)
3. Choose e coprime to phi(n)
4. Find d such that e*d = 1 mod phi(n)

Encryption: c = m^e mod n
Decryption: m = c^d mod n

The security relies on the computational difficulty of factoring n (number theory) and the discrete logarithm problem.

### Compilers and Programming Languages

**Graph Theory + Set Theory + Relations + Boolean Algebra**

**Syntax analysis:**

A context-free grammar is a set of production rules. Parsing constructs a parse tree (a rooted tree) from a token stream. The language accepted by a grammar is a set of strings.

**Register allocation:**

Register allocation is graph coloring. Variables are vertices, and an edge connects two variables if they are live simultaneously (interfere). The chromatic number of the interference graph is the minimum number of registers needed.

```python
def register_count(llvm_ir):
    """Estimate register need using graph coloring bound."""
    # In practice LLVM's register allocator uses graph coloring
    # with heuristics for spilling
    pass

# For a basic block with n simultaneously live variables,
# the interference graph may form a clique (chi = n),
# requiring at least n registers
```

**Type systems:**

Types form a poset under the subtype relation. Type checking verifies that expressions have consistent types using set membership (type of value is in the set of types accepted by the function).

Variance: Function types are contravariant in input and covariant in output:

```typescript
type Animal = { name: string };
type Dog = Animal & { breed: string };

// Function type variance
type Handler<T> = (arg: T) => void;
// Handler<Animal> is assignable to Handler<Dog> (contravariance)
```

**Control flow analysis:**

Control flow graphs (CFGs) are directed graphs where vertices are basic blocks and edges are jumps. Dataflow analysis propagates information through the CFG using fixed-point iteration on a lattice (a poset with meet and join operations).

```python
def reaching_definitions(cfg):
    """Compute reaching definitions using dataflow analysis.
    The lattice is the power set of all definitions,
    ordered by subset inclusion."""
    # Initialize IN and OUT for each block
    in_sets = {block: set() for block in cfg}
    out_sets = {block: set() for block in cfg}

    # Iterate until fixed point
    changed = True
    while changed:
        changed = False
        for block in cfg:
            # IN = intersection of predecessors' OUTs (for "must" analysis)
            # or union (for "may" analysis)
            new_in = set()
            for pred in cfg.predecessors(block):
                new_in |= out_sets[pred]

            # OUT = GEN union (IN - KILL)
            new_out = (new_in - cfg.kill[block]) | cfg.gen[block]

            if new_in != in_sets[block]:
                in_sets[block] = new_in
                changed = True
            if new_out != out_sets[block]:
                out_sets[block] = new_out
                changed = True

    return in_sets, out_sets
```

### Scheduling and Planning

**Partial Orders + Graph Theory + Combinatorics**

**Task scheduling:**

Tasks and their dependencies form a partial order. A topological sort gives a valid execution order. The minimum makespan (total time with parallel resources) is computed using critical path analysis.

```python
def critical_path(tasks, dependencies, durations):
    """Find the critical path in a task graph.
    Returns (path, total_duration)."""
    # Topological sort first
    in_degree = {t: 0 for t in tasks}
    for deps in dependencies.values():
        for d in deps:
            in_degree[t] += 1

    queue = [t for t in tasks if in_degree[t] == 0]
    earliest_start = {t: 0 for t in tasks}

    while queue:
        current = queue.pop(0)
        for next_task in tasks:
            if current in dependencies.get(next_task, []):
                start = earliest_start[current] + durations[current]
                if start > earliest_start[next_task]:
                    earliest_start[next_task] = start
                in_degree[next_task] -= 1
                if in_degree[next_task] == 0:
                    queue.append(next_task)

    # The longest path is the critical path
    total = max(earliest_start[t] + durations[t] for t in tasks)
    return total

tasks = ['design', 'implement', 'test', 'deploy', 'document']
dependencies = {
    'implement': ['design'],
    'test': ['implement'],
    'deploy': ['test'],
    'document': ['implement'],
}
durations = {'design': 3, 'implement': 5, 'test': 2, 'deploy': 1, 'document': 2}

total = critical_path(tasks, dependencies, durations)
print(f"Minimum project duration: {total} days")
```

**Load balancing:**

Distributing n tasks to k servers is a combinatorial problem. The number of distributions of indistinguishable tasks to distinguishable servers is C(n + k - 1, k - 1) (stars and bars).

```python
def distribution_count(tasks, servers):
    """Number of ways to distribute tasks among servers (tasks indistinguishable)."""
    from math import comb
    return comb(tasks + servers - 1, servers - 1)

print(distribution_count(10, 3))  # 66 ways to distribute 10 tasks to 3 servers
```

**Resource allocation:**

Allocating resources to processes while avoiding deadlock is a graph problem. The resource allocation graph has vertices for processes and resources, edges for "requested by" and "held by." A cycle in this graph indicates a deadlock.

### Error Detection and Correction

**Combinatorics + Boolean Algebra + Number Theory**

**Hamming distance:**

The Hamming distance between two binary strings is the number of positions where they differ. Minimum Hamming distance d determines error detection/correction capability:

- Detect up to d-1 errors
- Correct up to floor((d-1)/2) errors

```python
def hamming_distance(a, b):
    """Number of positions where bits differ."""
    xor = a ^ b
    return xor.bit_count()  # Python 3.8+ or bin(xor).count('1')

# Parity bit: distance 2, detects 1 error
def parity_bit(data):
    return data.bit_count() % 2

# Hamming(7,4) code: distance 3, corrects 1 error
# 4 data bits + 3 parity bits = 7 transmitted bits
```

**Error-correcting codes:**

Linear codes use vector spaces over GF(2). Code words form a subspace. Syndrome decoding uses the parity check matrix to identify errors.

```python
class Hamming74:
    """Hamming (7,4) code: encodes 4 bits into 7 bits, corrects 1 error."""
    def encode(self, data):
        """data: 4-bit integer. Returns 7-bit codeword."""
        d = [(data >> i) & 1 for i in range(4)]
        # Parity bits
        p1 = d[0] ^ d[1] ^ d[2]
        p2 = d[1] ^ d[2] ^ d[3]
        p3 = d[0] ^ d[1] ^ d[3]
        # Codeword: p1, p2, d0, p3, d1, d2, d3
        return (p1 << 6) | (p2 << 5) | (d[0] << 4) | (p3 << 3) | (d[1] << 2) | (d[2] << 1) | d[3]

# Checksums and CRCs use polynomial arithmetic over GF(2)
# (Boolean algebra + finite field theory)
```

**Checksums:**

A checksum is a Boolean function of the data bits (typically XOR of chunks). TCP, IP, and UDP use 16-bit ones' complement checksums.

### Artificial Intelligence

**Graph Theory + Combinatorics + Boolean Algebra**

**Search algorithms:**

State space search is graph traversal. Each state is a vertex, each action is an edge. BFS, DFS, and A* search are graph algorithms.

```python
def a_star(start, goal, neighbors, heuristic):
    """A* search on a graph."""
    open_set = {start}
    g_score = {start: 0}
    f_score = {start: heuristic(start, goal)}

    while open_set:
        current = min(open_set, key=lambda v: f_score.get(v, float('inf')))
        if current == goal:
            return g_score[current]

        open_set.remove(current)
        for neighbor, cost in neighbors(current):
            tentative_g = g_score[current] + cost
            if tentative_g < g_score.get(neighbor, float('inf')):
                g_score[neighbor] = tentative_g
                f_score[neighbor] = tentative_g + heuristic(neighbor, goal)
                open_set.add(neighbor)

    return None  # No path
```

**Bayesian networks:**

Bayesian networks are directed acyclic graphs (DAGs) where vertices are random variables and edges represent conditional dependencies. The graph structure encodes independence relations.

**Decision trees:**

Decision trees are rooted trees where internal nodes test Boolean conditions and leaves represent decisions. The branching factor depends on the number of possible outcomes of each test.

**Constraint satisfaction:**

Solving a CSP (e.g., Sudoku, scheduling) uses graph theory for constraint graphs, combinatorics for counting solutions, and Boolean algebra for encoding constraints.

```python
def sudoku_constraints(grid):
    """A Sudoku puzzle as a constraint satisfaction problem.
    Each cell is a variable with domain {1,...,9}.
    Constraints: all cells in same row/column/box are pairwise different.
    This defines a graph coloring problem on 81 vertices
    where the chromatic number is 9."""
    pass  # Sudoku can be solved with backtracking + constraint propagation
```

**Boolean satisfiability (SAT):**

SAT is the canonical NP-complete problem. Given a Boolean formula in CNF, determine if there exists an assignment making it true. Modern SAT solvers use DPLL (Davis-Putnam-Logemann-Loveland) with clause learning, which depends on implication graphs (directed graphs from Boolean clauses).

```python
def dpll(clauses, assignment):
    """Simplified DPLL SAT solver."""
    # Unit propagation
    clauses = unit_propagate(clauses, assignment)
    if not clauses:
        return assignment  # satisfied
    if any(not clause for clause in clauses):
        return None  # unsatisfied

    # Choose unassigned variable
    var = choose_variable(clauses)
    # Try true
    result = dpll(clauses + [[var]], {**assignment, var: True})
    if result:
        return result
    # Try false
    return dpll(clauses + [[-var]], {**assignment, var: False})
```

