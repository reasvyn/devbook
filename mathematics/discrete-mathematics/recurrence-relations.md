# Recurrence Relations

## Description

Recurrence relations define sequences where each term depends on previous terms. They are the mathematical model of recursive algorithms — divide-and-conquer, dynamic programming, and backtracking all produce recurrences. Solving recurrences is how we determine the time complexity of recursive algorithms like merge sort, binary search, and tree traversals.

## Prerequisites

- [Combinatorics](combinatorics.md) — binomial coefficients, sums, and generating functions

## Table of Contents

- [Recurrence Definitions](#recurrence-definitions)
- [Classic Recurrences](#classic-recurrences)
- [Solving by Iteration](#solving-by-iteration)
- [Solving by Characteristic Equation](#solving-by-characteristic-equation)
- [Master Theorem](#master-theorem)
- [Recursion Trees](#recursion-trees)
- [Generating Functions for Recurrences](#generating-functions-for-recurrences)
- [Applications in Computing](#applications-in-computing)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Recurrence Definitions

A **recurrence relation** is an equation that defines a sequence where each term is expressed as a function of previous terms, together with base cases.

**General form:**

$$a_n = f(a_{n-1}, a_{n-2}, \dots, a_{n-k})$$

With **initial conditions** (base cases) for a_0, a_1, ..., a_{k-1}.

**Order:** A recurrence of order k expresses a_n in terms of the k preceding terms.

**Linear recurrences:**

$$a_n = c_1 a_{n-1} + c_2 a_{n-2} + \dots + c_k a_{n-k} + f(n)$$

- **Homogeneous:** f(n) = 0
- **Non-homogeneous:** f(n) != 0
- **Constant coefficients:** c_i are constants (not functions of n)

**Non-linear recurrences:**

$$a_n = a_{n-1} \cdot a_{n-2}$$

$$T(n) = T(n/2) + T(n/4) + n$$

These are harder or impossible to solve in closed form.

```python
# Generating terms from a recurrence
def fibonacci(n):
    """Return first n+1 Fibonacci numbers."""
    a = [0, 1]
    for i in range(2, n + 1):
        a.append(a[i-1] + a[i-2])
    return a

print(fibonacci(10))
# [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```

### Classic Recurrences

**Fibonacci numbers:**

$$F_0 = 0, \quad F_1 = 1$$

$$F_n = F_{n-1} + F_{n-2}$$

Closed form (Binet's formula):

$$F_n = \frac{\phi^n - \psi^n}{\sqrt{5}}$$

Where phi = (1 + sqrt(5)) / 2 (golden ratio) and psi = (1 - sqrt(5)) / 2.

**Factorial:**

$$0! = 1$$

$$n! = n \cdot (n-1)!$$

**Binomial coefficients:**

$$\binom{n}{0} = \binom{n}{n} = 1$$

$$\binom{n}{k} = \binom{n-1}{k-1} + \binom{n-1}{k}$$

**Divide-and-conquer recurrences:**

$$T(1) = c$$

$$T(n) = a T(n/b) + f(n)$$

Where a is the number of subproblems, n/b is the size of each subproblem, and f(n) is the cost of dividing and combining.

**Specific examples:**

| Algorithm | Recurrence | Complexity |
|-----------|-----------|------------|
| Binary search | T(n) = T(n/2) + 1 | O(log n) |
| Merge sort | T(n) = 2T(n/2) + n | O(n log n) |
| Quickselect | T(n) = T(n/2) + n | O(n) |
| Strassen's matrix | T(n) = 7T(n/2) + O(n^2) | O(n^log2 7) |
| Karatsuba multiplication | T(n) = 3T(n/2) + O(n) | O(n^log2 3) |

### Solving by Iteration

The **iteration method** (also called the substitution method) repeatedly expands the recurrence until a pattern is found.

**Example 1: Factorial**

$$T(n) = T(n-1) + 1, \quad T(0) = 0$$

$$T(n) = T(n-1) + 1$$
$$= (T(n-2) + 1) + 1 = T(n-2) + 2$$
$$= T(n-3) + 3$$
$$\dots$$
$$= T(0) + n = n$$

So T(n) = n.

**Example 2: Binary search**

$$T(n) = T(n/2) + 1, \quad T(1) = 0$$

Assume n = 2^k:

$$T(2^k) = T(2^{k-1}) + 1$$
$$= (T(2^{k-2}) + 1) + 1 = T(2^{k-2}) + 2$$
$$\dots$$
$$= T(2^0) + k = k$$

Since k = log2 n, T(n) = O(log n).

**Example 3: Merge sort**

$$T(n) = 2T(n/2) + n, \quad T(1) = 0$$

Assume n = 2^k:

$$T(2^k) = 2T(2^{k-1}) + 2^k$$
$$= 2(2T(2^{k-2}) + 2^{k-1}) + 2^k = 4T(2^{k-2}) + 2 \cdot 2^k$$
$$= 8T(2^{k-3}) + 3 \cdot 2^k$$
$$\dots$$
$$= 2^k T(2^0) + k \cdot 2^k = 0 + n \log_2 n$$

So T(n) = O(n log n).

```python
def solve_merge_sort(n, depth=0):
    """Demonstrate the recurrence expansion."""
    if n <= 1:
        return 0
    # T(n) = 2 * T(n/2) + n
    sub = solve_merge_sort(n // 2, depth + 1)
    result = 2 * sub + n
    print(f"{'  ' * depth}T({n}) = 2 * T({n//2}) + {n} = {result}")
    return result

solve_merge_sort(8)
```

### Solving by Characteristic Equation

For linear homogeneous recurrences with constant coefficients:

$$a_n = c_1 a_{n-1} + c_2 a_{n-2} + \dots + c_k a_{n-k}$$

Assume a_n = r^n:

$$r^n = c_1 r^{n-1} + c_2 r^{n-2} + \dots + c_k r^{n-k}$$

Divide by r^{n-k}:

$$r^k - c_1 r^{k-1} - c_2 r^{k-2} - \dots - c_k = 0$$

This is the **characteristic equation**. Its roots determine the general solution.

**Case 1: Distinct roots r1, r2, ..., rk**

$$a_n = \alpha_1 r_1^n + \alpha_2 r_2^n + \dots + \alpha_k r_k^n$$

Constants alpha_i are determined by initial conditions.

**Case 2: Repeated roots**

If a root r has multiplicity m, the terms are:

$$(\alpha_1 + \alpha_2 n + \dots + \alpha_m n^{m-1}) r^n$$

**Example — Fibonacci:**

$$F_n = F_{n-1} + F_{n-2}$$

Characteristic equation: r^2 - r - 1 = 0

Roots: r = (1 +/- sqrt(5)) / 2 = phi, psi

General solution: F_n = alpha * phi^n + beta * psi^n

Using F_0 = 0, F_1 = 1:

$$F_n = \frac{1}{\sqrt{5}} \phi^n - \frac{1}{\sqrt{5}} \psi^n$$

```python
import math

phi = (1 + math.sqrt(5)) / 2
psi = (1 - math.sqrt(5)) / 2

def fibo_closed(n):
    return round((phi**n - psi**n) / math.sqrt(5))

for n in range(10):
    print(f"F_{n} = {fibo_closed(n)}")
# 0, 1, 1, 2, 3, 5, 8, 13, 21, 34
```

**Example — second order with repeated root:**

$$a_n = 4a_{n-1} - 4a_{n-2}, \quad a_0 = 1, a_1 = 4$$

Characteristic equation: r^2 - 4r + 4 = 0 => (r - 2)^2 = 0

Root r = 2 with multiplicity 2.

General solution: a_n = (alpha + beta * n) * 2^n

From a_0 = 1: alpha = 1
From a_1 = 4: (1 + beta) * 2 = 4 => beta = 1

So a_n = (1 + n) * 2^n.

### Master Theorem

The **Master theorem** provides closed-form solutions for recurrences of the form:

$$T(n) = a T(n/b) + f(n)$$

Where a >= 1, b > 1, and f(n) is asymptotically positive.

**Three cases:**

Compare f(n) with n^{log_b a}:

**Case 1:** f(n) = O(n^{log_b a - epsilon}) for some epsilon > 0

$$T(n) = \Theta(n^{\log_b a})$$

The cost is dominated by the leaves of the recursion tree.

**Case 2:** f(n) = Theta(n^{log_b a})

$$T(n) = \Theta(n^{\log_b a} \log n)$$

The cost is evenly distributed across levels.

**Case 3:** f(n) = Omega(n^{log_b a + epsilon}) for some epsilon > 0, and a f(n/b) <= c f(n) for some c < 1 and large n (regularity condition)

$$T(n) = \Theta(f(n))$$

The cost is dominated by the root.

**Examples:**

| Recurrence | a | b | log_b a | f(n) | Case | Result |
|-----------|---|---|---------|------|------|--------|
| T(n) = T(n/2) + 1 | 1 | 2 | 0 | 1 = Theta(n^0) | 2 | Theta(log n) |
| T(n) = 2T(n/2) + n | 2 | 2 | 1 | n = Theta(n^1) | 2 | Theta(n log n) |
| T(n) = 2T(n/2) + 1 | 2 | 2 | 1 | 1 = O(n^{1-eps}) | 1 | Theta(n) |
| T(n) = 4T(n/2) + n | 4 | 2 | 2 | n = O(n^{2-eps}) | 1 | Theta(n^2) |
| T(n) = 2T(n/2) + n^2 | 2 | 2 | 1 | n^2 = Omega(n^{1+eps}) | 3 | Theta(n^2) |

```python
import math

def master_theorem(a, b, f_power, f_log_factor=0):
    """
    Classify Master theorem case.
    f(n) = n^f_power * (log n)^f_log_factor
    Returns (case, complexity).
    """
    p = math.log(a, b)  # log_b a

    if f_power < p:
        return (1, f"Theta(n^{p})")
    elif f_power == p:
        log_power = f_log_factor + 1
        return (2, f"Theta(n^{p} log^{log_power} n)")
    else:
        # Check regularity condition (simplified)
        return (3, f"Theta(n^{f_power})")

recurrences = [
    ("T(n) = 2T(n/2) + n^0.5", 2, 2, 0.5, 0),
    ("T(n) = 2T(n/2) + n", 2, 2, 1, 0),
    ("T(n) = 4T(n/2) + n^2", 4, 2, 2, 0),
    ("T(n) = 8T(n/2) + n^2", 8, 2, 2, 0),
]

for desc, a, b, fp, flf in recurrences:
    case, comp = master_theorem(a, b, fp, flf)
    print(f"{desc}: Case {case}: {comp}")
```

**Limitations of the Master theorem:**

- Requires b > 1 and a >= 1 (integer)
- f(n) must be polynomially smaller/larger than n^{log_b a}
- Does not work for T(n) = 2T(n/2) + n / log n (the gap is not polynomial)
- Does not work when a is not constant or b varies
- For irregular recurrences, use recursion trees or the Akra-Bazzi method

### Recursion Trees

A **recursion tree** visualizes the recursive calls as a tree. Each node represents the cost of a subproblem, and the total is the sum across all nodes.

**Example: Merge sort T(n) = 2T(n/2) + n**

```
Level 0:              [n]                    cost = n
                     /    \
Level 1:          [n/2]   [n/2]             cost = 2 * (n/2) = n
                 /   \    /    \
Level 2:      [n/4] [n/4] [n/4] [n/4]       cost = 4 * (n/4) = n
               ...   ...   ...   ...
Level k:        1    1     1     1           cost = n * 1 = n
```

Each of the log2(n) levels costs n. Total: n * log2(n).

**Example: T(n) = 3T(n/4) + n^2**

```
Level 0:              [n^2]                  cost = n^2
                     /   |   \
Level 1:     [(n/4)^2] [(n/4)^2] [(n/4)^2]   cost = 3 * (n/4)^2 = (3/16)n^2
              / | \    / | \    / | \
Level 2:  9 nodes each (n/16)^2              cost = 9 * (n/16)^2 = (9/256)n^2
```

Level i cost: (3/16)^i * n^2

Total: n^2 * (1 + (3/16) + (3/16)^2 + ...) = n^2 * (1 / (1 - 3/16)) = (16/13) * n^2 = Theta(n^2)

Number of levels: log_4(n). The cost is dominated by the root (Case 3 of Master theorem).

```python
def recursion_tree_cost(a, b, f_n, n, levels=None):
    """Compute cost at each level of a recursion tree.
    a = branching factor, b = division factor,
    f_n(n) = cost at a node of size n."""
    if levels is None:
        levels = int(math.log(n, b)) + 1

    print(f"Level 0: 1 node of size {n}, cost {f_n(n)}")
    level_cost = f_n(n)
    total = level_cost
    nodes = 1
    size = n

    for level in range(1, levels + 1):
        nodes *= a
        size //= b
        if size < 1:
            break
        level_cost = nodes * f_n(size)
        total += level_cost
        print(f"Level {level}: {nodes} nodes of size {size}, cost {level_cost}")

    print(f"Total: {total}")
    return total

recursion_tree_cost(2, 2, lambda n: n, 16)
```

### Generating Functions for Recurrences

**Generating functions** convert a recurrence into an algebraic equation. If a_n satisfies a recurrence, its generating function G(x) = sum(a_n x^n) satisfies a functional equation that can be solved for a closed form.

**Example: Fibonacci**

G(x) = sum_{n >= 0} F_n x^n

From F_n = F_{n-1} + F_{n-2} (for n >= 2), multiply by x^n and sum:

G(x) - F_0 - F_1 x = x(G(x) - F_0) + x^2 G(x)

G(x) - x = x G(x) + x^2 G(x)

G(x)(1 - x - x^2) = x

G(x) = x / (1 - x - x^2)

Partial fraction decomposition gives the Binet formula.

**Example: Solving T(n) = 2T(n-1) + 1, T(0) = 0**

The generating function approach:

G(x) - 0 = 2x G(x) + 1/(1-x) (with adjustments for the +1 term)

G(x) = 1 / ((1-x)(1-2x))

Partial fractions:

G(x) = 2/(1-2x) - 1/(1-x)

So T_n = 2 * 2^n - 1 = 2^{n+1} - 1.

```python
# Verification
def solve_t(n):
    if n == 0:
        return 0
    return 2 * solve_t(n-1) + 1

def closed_t(n):
    return 2 ** (n + 1) - 1

for n in range(10):
    print(f"T({n}) = {solve_t(n)}, closed: {closed_t(n)}")
```

### Applications in Computing

**Algorithm analysis — merge sort:**

Merge sort's recurrence T(n) = 2T(n/2) + n gives Theta(n log n), which is optimal for comparison-based sorting.

```python
def merge_sort_complexity(n):
    """Return number of comparisons for merge sort."""
    if n <= 1:
        return 0
    mid = n // 2
    left = merge_sort_complexity(mid)
    right = merge_sort_complexity(n - mid)
    merge_cost = n  # at most n comparisons during merge
    return left + right + merge_cost

for n in [8, 16, 32, 64, 128, 256, 512, 1024]:
    comps = merge_sort_complexity(n)
    bound = n * math.log2(n)
    print(f"n={n}: {comps} comparisons, n*log2(n) = {bound:.0f}")
    # Mergesort comparisons are close to n*log2(n)
```

**Binary search:**

T(n) = T(n/2) + 1 gives Theta(log n). This is optimal for searching in a sorted array.

```python
def binary_search_steps(n):
    """Worst-case number of steps for binary search."""
    return math.ceil(math.log2(n + 1))

for n in [8, 64, 1024, 10**6, 10**9]:
    print(f"n={n}: {binary_search_steps(n)} steps")
```

**Dynamic programming:**

Many DP algorithms are defined by recurrences:

- **Knapsack:** V[i][w] = max(V[i-1][w], V[i-1][w-w_i] + v_i)
- **Longest common subsequence:** LCS(i,j) = max(LCS(i-1,j), LCS(i,j-1)) + match(i,j)
- **Matrix chain multiplication:** M[i][j] = min(M[i][k] + M[k+1][j] + d_{i-1}*d_k*d_j)

```python
def lcs_length(X, Y):
    """Longest Common Subsequence via DP recurrence."""
    m, n = len(X), len(Y)
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if X[i-1] == Y[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    return dp[m][n]

print(lcs_length("ABCBDAB", "BDCAB"))  # 4 ("BCAB" or "BDAB")
```

**Recurrences in probability:**

The expected time of randomized quicksort satisfies:

$$E[T(n)] = \frac{2}{n} \sum_{k=0}^{n-1} E[T(k)] + n$$

Which solves to E[T(n)] = Theta(n log n).

## Study Cases

### Case 1: Analyzing Karatsuba Multiplication

**Problem:** Traditional multiplication of two n-digit numbers takes O(n^2) time. Karatsuba's algorithm reduces this by splitting each number into two halves and using 3 multiplications instead of 4.

**Algorithm:** To multiply x and y (each n digits):

1. Split: x = a * 10^{n/2} + b, y = c * 10^{n/2} + d
2. Compute recursively:
   - p1 = a * c
   - p2 = b * d
   - p3 = (a + b) * (c + d)
3. Result: p1 * 10^n + (p3 - p1 - p2) * 10^{n/2} + p2

**Recurrence:** T(n) = 3T(n/2) + O(n)

**Solving with Master theorem:** a = 3, b = 2, log_2 3 ≈ 1.585, f(n) = n = O(n^{1.585 - epsilon}) — Case 1.

$$T(n) = \Theta(n^{\log_2 3}) \approx \Theta(n^{1.585})$$

```python
def karatsuba(x, y):
    """Karatsuba multiplication implementation."""
    if x < 10 or y < 10:
        return x * y

    n = max(len(str(x)), len(str(y)))
    m = n // 2
    power = 10 ** m

    a = x // power
    b = x % power
    c = y // power
    d = y % power

    ac = karatsuba(a, c)
    bd = karatsuba(b, d)
    ad_plus_bc = karatsuba(a + b, c + d) - ac - bd

    return ac * (10 ** (2 * m)) + ad_plus_bc * (10 ** m) + bd

# Verify
x, y = 1234, 5678
print(f"{x} * {y} = {karatsuba(x, y)} (expected {x * y})")
```

### Case 2: Counting Binary Strings Without Consecutive 1s

**Problem:** Find the number of binary strings of length n with no consecutive 1s.

**Recurrence:** Let a_n be the count. For a string of length n:

- If it ends in 0, the first n-1 positions can be any valid string: a_{n-1} ways
- If it ends in 1, the (n-1)th position must be 0, and the first n-2 can be any valid string: a_{n-2} ways

$$a_n = a_{n-1} + a_{n-2}$$

With a_0 = 1 (empty string), a_1 = 2 ("0", "1").

This is the Fibonacci sequence shifted: a_n = F_{n+2}.

```python
def count_no_consecutive_ones(n):
    """Count binary strings of length n with no consecutive 1s."""
    a, b = 1, 2  # a_0 = 1, a_1 = 2
    for _ in range(n):
        a, b = b, a + b
    return a

for n in range(1, 11):
    print(f"n={n}: {count_no_consecutive_ones(n)} strings")
# n=1: 2, n=2: 3, n=3: 5, n=4: 8, n=5: 13, ...
```

**Verification by enumeration:**

```python
def brute_force(n):
    count = 0
    for i in range(2 ** n):
        s = bin(i)[2:].zfill(n)
        if '11' not in s:
            count += 1
    return count

for n in range(1, 8):
    print(f"n={n}: DP={count_no_consecutive_ones(n)}, brute={brute_force(n)}")
```

### Case 3: Analyzing Divide-and-Conquer Matrix Multiplication

**Problem:** Strassen's matrix multiplication multiplies two n x n matrices using 7 multiplications of n/2 x n/2 submatrices instead of 8.

**Recurrence:** T(n) = 7T(n/2) + O(n^2)

**Master theorem:** a = 7, b = 2, log_2 7 ≈ 2.807, f(n) = n^2 = O(n^{2.807 - epsilon}) — Case 1.

$$T(n) = \Theta(n^{\log_2 7}) \approx \Theta(n^{2.807})$$

This beats the standard O(n^3) algorithm. Subsequent improvements (Coppersmith-Winograd) achieve approximately O(n^{2.376}), though with large constants.

```python
import math

a, b, f_power = 7, 2, 2
p = math.log(a, b)
print(f"log_{b}({a}) = {p:.3f}")
print(f"f(n) = n^{f_power}")
print(f"Case 1: T(n) = Theta(n^{p:.3f})")
```

## Examples

### Example 1: Solving T(n) = T(n-1) + n

This recurrence models the sum 1 + 2 + 3 + ... + n.

$$T(n) = T(n-1) + n, \quad T(0) = 0$$

Iteration:

$$T(n) = T(n-2) + (n-1) + n$$
$$= T(n-3) + (n-2) + (n-1) + n$$
$$\dots$$
$$= T(0) + 1 + 2 + \dots + n = n(n+1)/2$$

So T(n) = Theta(n^2).

```python
def t_n(n):
    if n == 0:
        return 0
    return t_n(n-1) + n

def closed(n):
    return n * (n + 1) // 2

for n in [5, 10, 20]:
    print(f"T({n}) = {t_n(n)}, closed: {closed(n)}")
```

### Example 2: Solving T(n) = 2T(n/2) + n (Merge Sort)

Using the recursion tree:

```
Level 0: cost = n
Level 1: 2 nodes, cost = 2 * n/2 = n
Level 2: 4 nodes, cost = 4 * n/4 = n
...
Level log2(n): n nodes, cost = n * 1 = n
```

There are log2(n) + 1 levels, each costing n. Total: n(log2(n) + 1) = Theta(n log n).

### Example 3: Characteristic Equation for Fibonacci

```python
import sympy as sp

n = sp.symbols('n')
r = sp.symbols('r')
# Characteristic equation: r^2 - r - 1 = 0
poly = r**2 - r - 1
roots = sp.solve(poly, r)
print(f"Roots: {roots}")
# [phi, psi] = [(1+sqrt(5))/2, (1-sqrt(5))/2]

# General solution: F_n = alpha * r1^n + beta * r2^n
# Using F_0 = 0, F_1 = 1
# alpha + beta = 0 => beta = -alpha
# alpha * r1 + beta * r2 = 1
# alpha * (r1 - r2) = 1
# alpha = 1 / sqrt(5)
```

### Example 4: Tower of Hanoi

$$T(n) = 2T(n-1) + 1, \quad T(0) = 0$$

Iteration:

$$T(n) = 2(2T(n-2) + 1) + 1 = 4T(n-2) + 2 + 1$$
$$= 8T(n-3) + 4 + 2 + 1$$
$$\dots$$
$$= 2^n T(0) + (2^{n-1} + 2^{n-2} + \dots + 1)$$
$$= 2^n - 1$$

So T(n) = Theta(2^n).

```python
def hanoi_moves(n):
    return 2 ** n - 1

def hanoi_solve(n, source, target, auxiliary, moves=None):
    if moves is None:
        moves = []
    if n == 1:
        moves.append(f"Move disk 1 from {source} to {target}")
        return moves
    hanoi_solve(n-1, source, auxiliary, target, moves)
    moves.append(f"Move disk {n} from {source} to {target}")
    hanoi_solve(n-1, auxiliary, target, source, moves)
    return moves

n = 3
print(f"Total moves for {n} disks: {hanoi_moves(n)}")
for step in hanoi_solve(n, 'A', 'C', 'B'):
    print(step)
# Total moves: 7
# Move disk 1 from A to C
# Move disk 2 from A to B
# ...
```

### Example 5: Master Theorem Edge Cases

```python
def master_case_check(a, b, p_f, p_log=0):
    """Check if master theorem applies and return case."""
    log_ba = math.log(a, b)

    if p_f < log_ba - 0.001:  # polynomial gap
        return 1
    elif abs(p_f - log_ba) < 0.001:
        return 2
    elif p_f > log_ba + 0.001:
        return 3
    else:
        return None  # polynomial gap too small

# Case that fails: T(n) = 2T(n/2) + n/log(n)
# Here f(n) = n/log(n) = n*log^{-1}(n)
# f(n) is not polynomially smaller/larger than n^{log_b a} = n
# The Master theorem does not apply.
```

### Example 6: Akra-Bazzi Method (Generalized Master Theorem)

For recurrences T(n) = sum a_i T(b_i n) + f(n) where the subproblem sizes are not equal fractions:

$$T(n) = \sum_{i=1}^k a_i T(b_i n) + f(n)$$

Find p such that sum a_i b_i^p = 1. Then:

$$T(n) = \Theta\left(n^p \left(1 + \int_1^n \frac{f(u)}{u^{p+1}} du\right)\right)$$

```python
def akra_bazzi(coefficients, f, n):
    """
    coefficients: list of (a_i, b_i) pairs where T(n) = sum a_i T(b_i n) + f(n)
    f: function f(n)
    n: input size
    Returns approximate T(n).
    """
    # Find p such that sum a_i * b_i^p = 1
    from scipy.optimize import fsolve  # would need scipy
    # For manual computation:
    # sum a_i * b_i^p = 1
    pass
```

## Glossary

| Term | Definition |
|------|------------|
| Recurrence relation | An equation defining a sequence in terms of previous terms |
| Order | The number of preceding terms a recurrence depends on |
| Initial condition | Base values that seed a recurrence |
| Linear recurrence | Each term is a linear combination of previous terms |
| Homogeneous | Recurrence with no external term f(n) |
| Characteristic equation | Polynomial equation whose roots determine the solution form |
| Closed form | An explicit formula for the n-th term (not recursive) |
| Master theorem | Theorem providing closed-form solutions for divide-and-conquer recurrences |
| Recursion tree | Visual representation of recursive call costs |
| Generating function | Power series encoding a sequence |
| Akra-Bazzi method | Generalization of the Master theorem for non-uniform subproblems |
| Dynamic programming | Optimization technique using recurrences |
| Golden ratio | phi = (1+sqrt(5))/2, appears in Fibonacci closed form |
| Binet's formula | Closed-form expression for Fibonacci numbers |

## Quick References

- [Master theorem (Wikipedia)](https://en.wikipedia.org/wiki/Master_theorem_(analysis_of_algorithms)) — detailed explanation with examples
- [OEIS: Recurrence sequences](https://oeis.org/) — searchable database of integer sequences
- [CLRS Introduction to Algorithms](https://mitpress.mit.edu/books/introduction-algorithms-fourth-edition) — chapters on recurrences and divide-and-conquer
- [generatingfunctionology, Wilf](https://www.math.upenn.edu/~wilf/gfologyLinked2.pdf) — free textbook on generating functions

## Next Steps

- [Boolean Algebra](boolean-algebra.md) — algebraic structures with applications to logic and circuit design
