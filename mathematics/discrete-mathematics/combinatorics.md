# Combinatorics

## Description

Combinatorics is the mathematics of counting. It answers questions like "how many passwords of length 8 exist?" and "how many ways can I choose 3 items from 10?" Every algorithm analysis, probability calculation, and security estimate depends on combinatorial reasoning. Understanding combinatorics is essential for analyzing algorithm complexity, designing experiments, estimating password strength, and reasoning about probability.

## Prerequisites

- [Set Theory](set-theory.md) — sets, cardinality, Cartesian products
- [Functions & Relations](functions-and-relations.md) — functions, injections, bijections

## Table of Contents

- [Fundamental Counting Principles](#fundamental-counting-principles)
- [Permutations](#permutations)
- [Combinations](#combinations)
- [Binomial Theorem](#binomial-theorem)
- [Pascal's Triangle](#pascals-triangle)
- [Inclusion-Exclusion Principle](#inclusion-exclusion-principle)
- [Pigeonhole Principle](#pigeonhole-principle)
- [Generating Functions](#generating-functions)
- [Applications in Computing](#applications-in-computing)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Fundamental Counting Principles

**Sum Rule (Addition Principle):** If task A can be done in m ways and task B can be done in n ways, and the tasks are mutually exclusive, then there are m + n ways to do A or B.

$$|A \cup B| = |A| + |B| \text{ if } A \cap B = \emptyset$$

**Example:** A menu has 8 appetizers and 6 desserts. Choosing one appetizer or one dessert: 8 + 6 = 14 ways.

**Product Rule (Multiplication Principle):** If task A can be done in m ways and after that task B can be done in n ways, then there are m * n ways to do A then B.

$$|A \times B| = |A| \cdot |B|$$

**Example:** Choosing an appetizer AND a dessert: 8 * 6 = 48 ways.

**Combined example — password counting:**

A password is 8 characters long, each character is a lowercase letter (26) or digit (10). How many passwords?

$$(26 + 10)^8 = 36^8 \approx 2.8 \times 10^{12}$$

**Subtraction Rule (Inclusion-Exclusion for two sets):**

$$|A \cup B| = |A| + |B| - |A \cap B|$$

**Division Rule:** If a task can be done in n ways, but each distinct outcome corresponds to d of those ways, then there are n/d distinct outcomes.

**Example:** Arranging 4 people in a circle. Linear arrangements: 4! = 24. But each circular arrangement corresponds to 4 linear arrangements (rotations). So: 24 / 4 = 6.

```python
# Product rule in action: counting password combinations
lowercase = 26
digits = 10
length = 8
total = (lowercase + digits) ** length
print(f"{total:,}")  # 2,821,109,907,456
```

### Permutations

A **permutation** is an ordered arrangement of distinct elements.

**Permutations without repetition (nPr):** The number of ways to choose and arrange r elements from an n-element set:

$$P(n, r) = n \times (n-1) \times \dots \times (n - r + 1) = \frac{n!}{(n-r)!}$$

For r = n: P(n, n) = n!

**Example:** How many ways to award gold, silver, bronze medals to 8 racers?

$$P(8, 3) = 8 \times 7 \times 6 = 336$$

**Permutations with repetition:** The number of ways to arrange r elements chosen from n types, allowing repetition:

$$n^r$$

**Example:** How many 4-digit PINs using digits 0-9?

$$10^4 = 10{,}000$$

**Permutations with indistinguishable objects:** The number of distinct arrangements of n objects where there are n1 of type 1, n2 of type 2, ..., nk of type k:

$$\frac{n!}{n_1! \cdot n_2! \cdot \dots \cdot n_k!}$$

**Example:** How many distinct arrangements of "MISSISSIPPI"?

Letters: M(1), I(4), S(4), P(2). Total: 11! / (1! * 4! * 4! * 2!) = 34,650

```python
from math import perm, factorial, prod

# nPr
print(perm(8, 3))  # 336

# n!
print(factorial(5))  # 120

# Permutations with repetition (n^r)
print(10 ** 4)  # 10000

# Permutations with indistinguishable objects
def perm_with_duplicates(counts):
    n = sum(counts)
    return factorial(n) // prod(factorial(c) for c in counts)

print(perm_with_duplicates([1, 4, 4, 2]))  # 34650
```

### Combinations

A **combination** is an unordered selection of elements.

**Combinations without repetition (nCr):** The number of ways to choose r elements from an n-element set (order doesn't matter):

$$\binom{n}{r} = \frac{n!}{r!(n-r)!} = \frac{P(n, r)}{r!}$$

**Properties:**

$$\binom{n}{r} = \binom{n}{n-r}$$

$$\binom{n}{0} = \binom{n}{n} = 1$$

$$\binom{n}{1} = n$$

**Example:** How many 5-card hands from a 52-card deck?

$$\binom{52}{5} = 2{,}598{,}960$$

**Combinations with repetition (stars and bars):** The number of ways to choose r elements from n types, allowing repetition:

$$\binom{n + r - 1}{r} = \binom{n + r - 1}{n - 1}$$

**Example:** How many ways to choose 3 scoops of ice cream from 5 flavors (allowing repeats)?

$$\binom{5 + 3 - 1}{3} = \binom{7}{3} = 35$$

**Stars and bars derivation:** Represent the selection as r stars (*) divided into n bins by n-1 bars (|). The number of arrangements of stars and bars is C(r + n - 1, r).

```python
from math import comb

# nCr
print(comb(52, 5))  # 2598960

# Combinations with repetition
def comb_with_repetition(n, r):
    return comb(n + r - 1, r)

print(comb_with_repetition(5, 3))  # 35
```

### Binomial Theorem

The **binomial theorem** describes the expansion of (x + y)^n:

$$(x + y)^n = \sum_{k=0}^n \binom{n}{k} x^{n-k} y^k$$

**Example:**

$$(x + y)^3 = \binom{3}{0}x^3 + \binom{3}{1}x^2 y + \binom{3}{2}x y^2 + \binom{3}{3}y^3$$

$$= x^3 + 3x^2 y + 3x y^2 + y^3$$

**Important identities:**

$$\sum_{k=0}^n \binom{n}{k} = 2^n$$

Setting x = 1, y = 1: (1 + 1)^n = 2^n.

$$\sum_{k=0}^n (-1)^k \binom{n}{k} = 0$$

Setting x = 1, y = -1: (1 - 1)^n = 0.

**Multinomial theorem:**

$$(x_1 + x_2 + \dots + x_m)^n = \sum_{k_1 + \dots + k_m = n} \binom{n}{k_1, k_2, \dots, k_m} x_1^{k_1} \dots x_m^{k_m}$$

Where the multinomial coefficient is:

$$\binom{n}{k_1, \dots, k_m} = \frac{n!}{k_1! \dots k_m!}$$

```python
def binomial_coeffs(n):
    return [comb(n, k) for k in range(n + 1)]

print(binomial_coeffs(5))
# [1, 5, 10, 10, 5, 1]
```

### Pascal's Triangle

Pascal's triangle is a triangular array of binomial coefficients. Row n contains C(n,0), C(n,1), ..., C(n,n).

```
n=0:        1
n=1:       1 1
n=2:      1 2 1
n=3:     1 3 3 1
n=4:    1 4 6 4 1
n=5:   1 5 10 10 5 1
n=6:  1 6 15 20 15 6 1
```

**Pascal's identity:**

$$\binom{n}{k} = \binom{n-1}{k-1} + \binom{n-1}{k}$$

Each entry is the sum of the two entries above it.

```python
def pascal_triangle(n):
    row = [1]
    for _ in range(n + 1):
        print(' '.join(str(x) for x in row).center(40))
        row = [1] + [row[i] + row[i+1] for i in range(len(row)-1)] + [1]

pascal_triangle(6)
```

**Properties visible in Pascal's triangle:**

- Row sums are 2^n
- The Fibonacci numbers appear as sums of specific diagonals
- Symmetry: C(n,k) = C(n,n-k)

### Inclusion-Exclusion Principle

The inclusion-exclusion principle computes the size of a union of overlapping sets.

**For two sets:**

$$|A \cup B| = |A| + |B| - |A \cap B|$$

**For three sets:**

$$|A \cup B \cup C| = |A| + |B| + |C| - |A \cap B| - |A \cap C| - |B \cap C| + |A \cap B \cap C|$$

**General form:**

$$\left|\bigcup_{i=1}^n A_i\right| = \sum_i |A_i| - \sum_{i<j} |A_i \cap A_j| + \sum_{i<j<k} |A_i \cap A_j \cap A_k| - \dots + (-1)^{n+1} |A_1 \cap \dots \cap A_n|$$

**Application — counting derangements:**

A **derangement** is a permutation where no element appears in its original position. The number of derangements of n items is:

$$!n = n! \sum_{i=0}^n \frac{(-1)^i}{i!}$$

Derived using inclusion-exclusion: let Ai be the set of permutations where element i is fixed.

```python
from math import factorial

def derangements(n):
    total = 0
    for i in range(n + 1):
        total += ((-1) ** i) * factorial(n) // factorial(i)
    return total

for n in range(1, 8):
    print(f"!{n} = {derangements(n)}")
# !1 = 0, !2 = 1, !3 = 2, !4 = 9, !5 = 44, !6 = 265, !7 = 1854
```

### Pigeonhole Principle

The **pigeonhole principle** states: if n items are placed into m boxes and n > m, then at least one box contains at least 2 items.

**Generalized pigeonhole principle:** If n items are placed into m boxes, then at least one box contains at least ceil(n/m) items.

**Examples:**

1. **Birthday problem:** Among 367 people, at least two share a birthday.

2. **Hash collisions:** If a hash function produces m distinct values and you hash m+1 inputs, at least one collision occurs.

3. **Data compression:** No lossless compression algorithm can compress all inputs of size n to smaller sizes. There are 2^n possible inputs but only 2^n - 1 possible compressed outputs of size < n.

```python
# Generalized pigeonhole: minimum collisions
def min_collisions(items, boxes):
    import math
    return math.ceil(items / boxes)

# Hash function: if we have 1000 buckets and hash 2500 items
print(min_collisions(2500, 1000))  # 3 (at least one bucket has 3+ items)
```

### Generating Functions

A **generating function** is a formal power series whose coefficients encode a sequence. For a sequence a0, a1, a2, ..., the ordinary generating function is:

$$G(x) = \sum_{n=0}^{\infty} a_n x^n = a_0 + a_1 x + a_2 x^2 + \dots$$

**Example — constant sequence:** a_n = 1 for all n:

$$G(x) = \sum_{n=0}^{\infty} x^n = \frac{1}{1 - x}$$

**Example — binomial coefficients:** a_n = C(m, n) for fixed m:

$$G(x) = \sum_{n=0}^{m} \binom{m}{n} x^n = (1 + x)^m$$

This is the binomial theorem in generating function form.

**Using generating functions to solve combinatorial problems:**

How many ways to select r items from 3 types with constraints?
- Type A: at most 2 items
- Type B: any number
- Type C: at least 1 item

The generating function is:

$$(1 + x + x^2)(1 + x + x^2 + \dots)(x + x^2 + \dots)$$

Multiply and extract the coefficient of x^r.

```python
# Simple generating function: polynomial multiplication
import numpy as np

def multiply_polys(p1, p2):
    """Multiply two polynomials represented as coefficient lists."""
    result = [0] * (len(p1) + len(p2) - 1)
    for i, c1 in enumerate(p1):
        for j, c2 in enumerate(p2):
            result[i + j] += c1 * c2
    return result

# Type A: at most 2 items (1 + x + x^2)
# Type B: unlimited (1 + x + x^2 + ...) truncated to 5
# Type C: at least 1 (x + x^2 + x^3 + ...) truncated
type_a = [1, 1, 1]  # coefficients for 0, 1, 2 items
type_b = [1, 1, 1, 1, 1]  # 0-4 items (truncated)
type_c = [0, 1, 1, 1, 1]  # 1-4 items (at least 1)

result = multiply_polys(type_a, type_b)
result = multiply_polys(result, type_c)

for r, coeff in enumerate(result):
    if coeff > 0:
        print(f"Ways to select {r} items: {coeff}")
```

Generating functions are a powerful bridge between combinatorics and algebra. They are used in:
- **Algorithm analysis** — analyzing the average case of quicksort
- **Probability** — moment generating functions
- **Recurrence solving** — converting recurrences to closed form

## Applications in Computing

**Algorithm complexity analysis:**

Combinatorics drives Big O analysis. A triple nested loop over an n-element array runs at most n^3 iterations — but the actual number depends on loop bounds:

```python
def analyze_loops(n):
    count = 0
    for i in range(n):
        for j in range(i+1, n):
            for k in range(j+1, n):
                count += 1
    return count  # C(n, 3)

for n in [5, 10, 20]:
    print(f"n={n}: {analyze_loops(n)} iterations (C({n},3) = {comb(n,3)})")
```

The number of distinct pairs (i, j) with i < j is C(n, 2) = n(n-1)/2. The number of distinct triples is C(n, 3). This is why combinatorial functions appear in complexity analysis.

**Password strength estimation:**

Password entropy is measured in bits. An n-character password from an alphabet of size A has A^n possibilities, or log2(A^n) = n * log2(A) bits of entropy:

```python
import math

def password_entropy(length, alphabet_size):
    return length * math.log2(alphabet_size)

# 8-character lowercase password
print(password_entropy(8, 26))  # ~37.6 bits

# 12-character mixed case + digits + symbols (alphabet size ~95)
print(password_entropy(12, 95))  # ~78.9 bits

# Time to brute force at 10 billion guesses/sec
def crack_time(entropy, guesses_per_sec=1e10):
    return (2 ** entropy) / guesses_per_sec / (365.25 * 24 * 3600)

print(f"37.6 bits: {crack_time(37.6):.2f} years")   # ~0.0002 years
print(f"78.9 bits: {crack_time(78.9):.2f} years")   # millions of years
```

**Network design — complete graphs:**

A fully connected network of n nodes requires C(n, 2) edges. This determines the cost and complexity of mesh topologies.

**Load balancing — combinations with repetition:**

Assigning r jobs to n servers (jobs are indistinguishable, servers are distinguishable) is C(n + r - 1, r). This counts the number of possible load distributions.

**Counting in automated testing:**

Combinatorial test design (pairwise testing) covers all pairs of parameter values, requiring far fewer test cases than exhaustive testing. For 5 parameters each with 4 values, exhaustive testing requires 4^5 = 1024 cases, but pairwise testing covers all C(5,2) = 10 pairs with ~20 cases.

## Glossary

| Term | Definition |
|------|------------|
| Sum rule | If events are mutually exclusive, total ways is the sum of individual ways |
| Product rule | If events are sequential, total ways is the product of individual ways |
| Permutation | An ordered arrangement of distinct elements |
| Combination | An unordered selection of elements |
| Binomial coefficient | C(n, r) = n! / (r!(n-r)!), the number of r-element subsets of an n-element set |
| Binomial theorem | (x + y)^n = sum C(n,k) x^(n-k) y^k |
| Pascal's identity | C(n,k) = C(n-1,k-1) + C(n-1,k) |
| Pascal's triangle | Triangular arrangement of binomial coefficients |
| Inclusion-exclusion | Principle for counting elements in overlapping unions |
| Pigeonhole principle | If n > m items go into m boxes, one box has at least 2 items |
| Derangement | A permutation where no element remains in its original position |
| Stars and bars | Technique for counting combinations with repetition |
| Generating function | A power series whose coefficients encode a sequence |
| Catalan number | (1/(n+1)) * C(2n, n), counts many combinatorial structures |
| Multinomial coefficient | n! / (k1! k2! ... km!), counts arrangements with multiple types |

## Quick References

- [OEIS](https://oeis.org/) — Encyclopedia of Integer Sequences, searchable database of combinatorial sequences
- [Discrete Mathematics and Its Applications, Rosen](https://www.mheducation.com/highered/product/discrete-mathematics-its-applications-rosen/M9780073383095.html) — chapters on counting and advanced combinatorics
- [Python math.comb documentation](https://docs.python.org/3/library/math.html#math.comb) — built-in binomial coefficient function
- [Twelvefold Way](https://en.wikipedia.org/wiki/Twelvefold_way) — unified framework for counting functions between finite sets

## Next Steps

- [Graph Theory](graph-theory.md) — counting paths, cycles, and graph colorings
- [Recurrence Relations](recurrence-relations.md) — using recurrences to analyze combinatorial algorithms
