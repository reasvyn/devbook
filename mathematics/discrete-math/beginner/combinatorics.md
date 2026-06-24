# Combinatorics

## Description

Combinatorics is the mathematics of counting. It answers: how many ways can I arrange these items? How many subsets exist? How many paths through this graph? Combinatorics is essential for algorithm analysis, probability, cryptography, and understanding the complexity of brute-force solutions.

## Prerequisites

- [Set Theory](set-theory.md)

## Table of Contents

- [The Fundamental Counting Principle](#the-fundamental-counting-principle)
- [Permutations](#permutations)
- [Combinations](#combinations)
- [Pigeonhole Principle](#pigeonhole-principle)
- [Applications in Code](#applications-in-code)

## Content / Material

### The Fundamental Counting Principle

If task A can be done in $m$ ways and task B in $n$ ways, then A followed by B can be done in $m \times n$ ways.

**Example**: How many possible 4-digit PINs?

$$ 10 \times 10 \times 10 \times 10 = 10^4 = 10,000 $$

### Permutations

Permutations count **ordered** arrangements.

**Without repetition** (choose and arrange $k$ items from $n$):

$$ P(n, k) = \frac{n!}{(n-k)!} $$

```python
import math
# How many ways to pick 3 runners for gold/silver/bronze from 10?
p = math.perm(10, 3)  # 720
```

**With repetition** (arrange with replacement):

$$ n^k $$

```python
# How many 4-letter codes from A-Z?
codes = 26 ** 4  # 456,976
```

### Combinations

Combinations count **unordered** selections.

**Without repetition** (choose $k$ items from $n$, order doesn't matter):

$$ C(n, k) = \binom{n}{k} = \frac{n!}{k!(n-k)!} $$

```python
# How many 5-card hands from a 52-card deck?
from math import comb
hands = comb(52, 5)  # 2,598,960
```

**With repetition** (combinations where you can pick the same item multiple times):

$$ C(n + k - 1, k) $$

```python
# How many ways to choose 3 scoops from 5 ice cream flavors?
ways = comb(5 + 3 - 1, 3)  # 35
```

### Pigeonhole Principle

If $n$ items are placed into $m$ boxes and $n > m$, at least one box must contain more than one item.

**Example**: In a group of 367 people, at least two share a birthday (366 possible birthdays). This principle is used in hash table collision analysis.

### Applications in Code

```python
# Itertools for permutations and combinations
from itertools import permutations, combinations

items = ['A', 'B', 'C']

# All orderings (permutations)
list(permutations(items, 2))  # [('A','B'), ('A','C'), ('B','A'), ('B','C'), ('C','A'), ('C','B')]

# All subsets of size 2 (combinations)
list(combinations(items, 2))  # [('A','B'), ('A','C'), ('B','C')]
```

## Glossary

| Term | Definition |
|------|------------|
| Factorial | $n! = n \times (n-1) \times ... \times 1$ |
| Permutation | An ordered arrangement of items |
| Combination | An unordered selection of items |
| Pigeonhole principle | If $n > m$, putting $n$ items into $m$ boxes forces at least one box to have multiple items |

## Next Steps

- [Modular Arithmetic](modular-arithmetic.md) — arithmetic with remainders, the math behind cryptography
