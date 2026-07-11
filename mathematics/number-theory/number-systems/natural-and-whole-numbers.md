# Natural & Whole Numbers

## Description

Natural numbers are the most fundamental number system — the counting numbers $1, 2, 3, \dots$ that arise directly from enumerating objects. Whole numbers extend this set by including $0$. Together they form the foundation for all other number systems and are deeply embedded in how computers index, count, recurse, and prove correctness.

## Prerequisites

None. This is the foundational number system.

If you are unfamiliar with mathematical proofs and induction, consider reviewing [Mathematical Thinking & Proofs](../../intro/mathematical-thinking.md) first.

## Table of Contents

- [What Are Natural Numbers?](#what-are-natural-numbers)
- [The Zero Debate](#the-zero-debate)
- [Whole Numbers](#whole-numbers)
- [Peano Axioms](#peano-axioms)
- [Properties of Natural Numbers](#properties-of-natural-numbers)
- [Ordering and the Well-Ordering Principle](#ordering-and-the-well-ordering-principle)
- [Mathematical Induction](#mathematical-induction)
- [Strong Induction](#strong-induction)
- [Structural Induction](#structural-induction)
- [Counting and Cardinality](#counting-and-cardinality)
- [Number Representations](#number-representations)
- [Natural Numbers in Computing](#natural-numbers-in-computing)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Are Natural Numbers?

Natural numbers, denoted by $\mathbb{N}$, are the numbers used for counting and ordering. They are the most intuitive number system — humans have been using them since prehistory.

The set of natural numbers has two common definitions:

$$
\mathbb{N} = \{1, 2, 3, 4, \dots\}
$$
$$
\mathbb{N}_0 = \{0, 1, 2, 3, \dots\}
$$

The first definition starts at $1$ and is common in most mathematics, computer science, and engineering contexts. The second includes $0$ and is used by ISO 80000-2 and some branches of logic and set theory.

The distinction matters in computing because the choice of starting index determines how algorithms are written. Array indexing in most languages starts at $0$ (C, Python, Java, Rust), but some languages like MATLAB and Lua start at $1$. Understanding the natural numbers means understanding both conventions.

### The Zero Debate

The question "Is $0$ a natural number?" has been debated for centuries. There is no universal answer — it depends on context.

**Arguments for $0 \in \mathbb{N}$:**

- The ISO 80000-2 standard defines $\mathbb{N} = \{0, 1, 2, \dots\}$.
- Set theory constructs natural numbers from the empty set: $0 = \emptyset$, $1 = \{\emptyset\}$, $2 = \{\emptyset, \{\emptyset\}\}$. Zero is the starting point.
- The Peano axioms (discussed below) explicitly include $0$ as a natural number.
- Many mathematical logic and combinatorics texts use $\mathbb{N}$ to include $0$.

**Arguments for $0 \notin \mathbb{N}$:**

- Historically, "natural" means "counting": you count $1, 2, 3, \dots$ objects. You do not count $0$ objects.
- Most computer science textbooks define $\mathbb{N} = \{1, 2, 3, \dots\}$ and use $\mathbb{N}_0$ when zero is needed.
- In number theory, natural numbers are often defined as positive integers, and $0$ requires special treatment in divisibility (division by zero is undefined).

**Resolution in practice:**

- In this document, $\mathbb{N} = \{1, 2, 3, \dots\}$ and $\mathbb{N}_0 = \mathbb{N} \cup \{0\}$.
- Always check the convention when reading a paper, textbook, or API documentation. The exponent $n > 0$ in a loop condition may reveal which convention is in use.

```python
# Some languages index from 0 (Python, C, Java, Rust)
arr = [10, 20, 30]
# arr[0] = 10, arr[1] = 20, arr[2] = 30
# This convention matches N_0 (zero-indexed)

# Other languages index from 1 (MATLAB, Lua, Julia)
# arr[1] = 10, arr[2] = 20, arr[3] = 30
# This convention matches N (one-indexed)
```

### Whole Numbers

Whole numbers, denoted $\mathbb{N}_0$ or $\mathbb{W}$, are the natural numbers plus zero:

$$
\mathbb{W} = \mathbb{N}_0 = \{0, 1, 2, 3, \dots\}
$$

The term "whole numbers" is primarily educational — it is used in elementary mathematics to distinguish between "counting numbers" (starting at $1$) and "counting numbers plus zero." In higher mathematics, $\mathbb{N}_0$ is preferred.

Why include $0$? Zero is the additive identity: $n + 0 = n$ for any $n$. It also enables subtraction when the result is not negative ($n - n = 0$). In computing, zero is essential as a base index, a sentinel value, and the identity element for addition.

```python
# Zero as additive identity
x = 42
assert x + 0 == x

# Zero as sentinel value for "not found"
def find_first(arr, target):
    """Returns 1-based index or 0 for not found."""
    for i, val in enumerate(arr, start=1):
        if val == target:
            return i
    return 0  # sentinel: not found
```

### Peano Axioms

The Peano axioms, formulated by Giuseppe Peano in 1889, provide a rigorous axiomatic foundation for the natural numbers. They define $\mathbb{N}$ using only a few primitive concepts: $0$, a successor function $S(n)$, and induction.

**Axiom 1: Zero is a natural number.**
$$
0 \in \mathbb{N}
$$

**Axiom 2: Every natural number has a successor.**
$$
n \in \mathbb{N} \implies S(n) \in \mathbb{N}
$$

The successor of $n$ is intuitively $n + 1$. So $S(0) = 1$, $S(1) = 2$, and so on.

**Axiom 3: Zero is not the successor of any natural number.**
$$
\forall n \in \mathbb{N}: S(n) \neq 0
$$

This ensures the number line starts at $0$ and does not loop back.

**Axiom 4: The successor function is injective.**
$$
S(m) = S(n) \implies m = n
$$

Two different numbers cannot have the same successor. This prevents branching in the number line.

**Axiom 5: Induction principle.**
If a property $P$ holds for $0$, and whenever $P(k)$ holds we also have $P(S(k))$, then $P$ holds for all natural numbers.

$$
P(0) \land (\forall k \in \mathbb{N}: P(k) \implies P(S(k))) \implies \forall n \in \mathbb{N}: P(n)
$$

From these five axioms, the entire structure of arithmetic can be derived. Addition, multiplication, and ordering are defined recursively using the successor function:

```python
def add(a, b):
    """Addition defined using successor (S)."""
    if b == 0:
        return a
    return S(add(a, b - 1))

def multiply(a, b):
    """Multiplication defined using addition."""
    if b == 0:
        return 0
    return add(a, multiply(a, b - 1))

def S(n):
    """Successor function."""
    return n + 1
```

**Why Peano axioms matter for developers:**

- They formalize the induction principle, which is the mathematical basis for recursion and loop invariants.
- They show that the entire arithmetic of natural numbers can be built from a tiny kernel of axioms — analogous to how programming languages are built from a small core of primitives.
- The successor function is the simplest possible iterator. Every `for` loop over a range is an application of the successor function.

### Properties of Natural Numbers

Natural numbers (and whole numbers) satisfy several algebraic properties under addition and multiplication.

**Closure.** The sum or product of two natural numbers is always a natural number:
$$
\forall a, b \in \mathbb{N}: a + b \in \mathbb{N}, \; a \times b \in \mathbb{N}
```

This seems trivial, but it fails for subtraction: $1 - 2$ is not a natural number. This limitation is what motivates the invention of integers.

**Associativity.** The grouping of operations does not matter:
$$
(a + b) + c = a + (b + c)
$$
$$
(a \times b) \times c = a \times (b \times c)
$$

**Commutativity.** The order of operands does not matter:
$$
a + b = b + a
$$
$$
a \times b = b \times a
$$

**Identity elements.** Zero is the additive identity, one is the multiplicative identity:
$$
a + 0 = a
$$
$$
a \times 1 = a
$$

**Distributivity.** Multiplication distributes over addition:
$$
a \times (b + c) = a \times b + a \times c
$$

**No additive inverses.** There is no natural number $x$ such that $a + x = 0$ (unless $a = 0$). This is the key limitation of $\mathbb{N}$.

**Subtraction is partial.** For $a, b \in \mathbb{N}$, $a - b$ is defined only when $a \geq b$. This partiality is why subtraction is not considered a proper operation on $\mathbb{N}$.

```python
# Closure holds for addition and multiplication
assert 3 + 5 in NATURALS  # 8 is natural
assert 3 * 5 in NATURALS  # 15 is natural

# Closure fails for subtraction
# 3 - 5 = -2 is NOT a natural number
try:
    result = 3 - 5
    assert result >= 0  # This would fail
except:
    pass
```

### Ordering and the Well-Ordering Principle

Natural numbers have a natural total order:
$$
a \leq b \iff \exists k \in \mathbb{N}_0: a + k = b
$$

This ordering is:

- **Reflexive:** $a \leq a$
- **Antisymmetric:** $a \leq b$ and $b \leq a$ implies $a = b$
- **Transitive:** $a \leq b$ and $b \leq c$ implies $a \leq c$
- **Total:** For any $a, b$, either $a \leq b$ or $b \leq a$

**Well-ordering principle.** Every non-empty set of natural numbers has a least element. This seemingly simple property is equivalent to the principle of mathematical induction. It is the basis for many proofs in number theory and for algorithms that find minima.

```python
def find_min(arr):
    """Well-ordering in code: every finite set has a minimum."""
    if not arr:
        raise ValueError("non-empty set required")
    min_val = arr[0]
    for x in arr:
        if x < min_val:
            min_val = x
    return min_val
```

The well-ordering principle also powers **infinite descent**, a proof technique where you assume a minimal counterexample exists and then derive a smaller one, leading to contradiction.

### Mathematical Induction

Mathematical induction is a proof technique for statements of the form "$P(n)$ holds for all natural numbers $n$." It is the single most important connection between natural numbers and programming.

**Principle of induction.** To prove $P(n)$ for all $n \in \mathbb{N}_0$:

1. **Base case:** Prove $P(0)$ (or $P(1)$, depending on convention).
2. **Inductive step:** Assume $P(k)$ holds for some $k \geq 0$, and prove $P(k+1)$.

The assumption $P(k)$ is called the **inductive hypothesis**.

**Example: Sum of the first $n$ natural numbers.**

Claim: $\displaystyle \sum_{i=1}^{n} i = \frac{n(n+1)}{2}$

Base case ($n = 1$): $1 = \frac{1(2)}{2} = 1$. Verified.

Inductive step: Assume true for $n = k$:
$$
1 + 2 + \cdots + k = \frac{k(k+1)}{2}
$$

Prove for $n = k + 1$:
$$
1 + 2 + \cdots + k + (k+1) = \frac{k(k+1)}{2} + (k+1) = \frac{k(k+1) + 2(k+1)}{2} = \frac{(k+1)(k+2)}{2}
$$

This is exactly the formula for $n = k+1$. By induction, the formula holds for all $n \geq 1$.

```python
def sum_natural(n):
    """Computes 1 + 2 + ... + n using the closed-form formula."""
    return n * (n + 1) // 2

# Proof by induction corresponds directly to recursion:
def sum_natural_recursive(n):
    """Recursive version mirrors the inductive proof."""
    if n == 1:
        return 1  # base case
    return n + sum_natural_recursive(n - 1)  # inductive step
```

**Induction meets loop invariants.** Every loop that iterates over a range can be proved correct using induction. The **loop invariant** is a property that holds before each iteration.

```python
def factorial(n):
    """Compute n! using a loop, proved correct by induction."""
    result = 1
    # Invariant: result == i! at the start of each iteration
    for i in range(1, n + 1):
        # Before: result == (i-1)!
        result *= i
        # After: result == i!
    # At termination: result == n!
    return result
```

The proof mirrors induction:

- **Base (i=1):** Before the first iteration, `result = 1 = 0!`. The invariant holds.
- **Inductive step:** Assume before iteration `i` we have `result = (i-1)!`. After `result *= i`, we have `result = (i-1)! * i = i!`. The invariant holds for the next iteration.
- **Termination:** When the loop ends, `i = n+1`, so `result = n!`. Correct.

### Strong Induction

Strong induction (also called complete induction) allows the inductive hypothesis to assume $P$ holds for all smaller values, not just $k$:

1. **Base case:** Prove $P(0)$.
2. **Strong inductive step:** Assume $P(0), P(1), \dots, P(k)$ all hold, and prove $P(k+1)$.

Strong induction is equivalent to ordinary induction but is more convenient when the proof for $P(k+1)$ depends on multiple smaller cases.

**Example: Every integer $n \geq 2$ can be factored into primes.**

Base case ($n = 2$): $2$ is prime, so it is trivially a product of primes.

Strong inductive step: Assume every integer from $2$ to $k$ can be factored into primes. Consider $n = k+1$. If $k+1$ is prime, we are done. If $k+1$ is composite, then $k+1 = a \times b$ where $2 \leq a, b \leq k$. By the strong inductive hypothesis, both $a$ and $b$ have prime factorizations. Multiplying them gives a prime factorization of $k+1$.

```python
def prime_factors(n):
    """Return list of prime factors of n."""
    factors = []
    d = 2
    while d * d <= n:
        while n % d == 0:
            factors.append(d)
            n //= d
        d += 1 if d == 2 else 2  # skip evens after 2
    if n > 1:
        factors.append(n)
    return factors

# The recursive decomposition mirrors strong induction:
# n = a * b, factor a and b independently
```

**When to use strong induction:**

- Algorithms that recurse on multiple subproblems (divide-and-conquer)
- Proofs involving the fundamental theorem of arithmetic
- Analysis of recursive data structures like trees

### Structural Induction

Structural induction extends induction from natural numbers to recursively-defined structures like lists, trees, and expressions.

The principle: to prove a property $P$ holds for all elements of a recursively-defined set:

1. **Base case:** Prove $P$ holds for all atomic (non-recursive) elements.
2. **Inductive step:** Assume $P$ holds for the substructures of a compound element, and prove $P$ holds for the compound element.

**Example: Prove that the length of a list is non-negative.**

```python
# Recursive definition of a list:
# A list is either:
#   - Empty (Nil)
#   - A head element followed by a tail list (Cons)

def length(lst):
    if lst == []:
        return 0
    return 1 + length(lst[1:])

# Induction on structure:
# Base: length([]) = 0 >= 0. Holds.
# Inductive step: Assume length(tail) >= 0.
# Then length(lst) = 1 + length(tail) >= 1 >= 0. Holds.
```

**Structural induction on binary trees:**

```python
class TreeNode:
    def __init__(self, value, left=None, right=None):
        self.value = value
        self.left = left
        self.right = right

def tree_size(node):
    """Return number of nodes in the tree."""
    if node is None:
        return 0
    return 1 + tree_size(node.left) + tree_size(node.right)

# Structural induction proof of size correctness:
# Base: None -> 0. Correct.
# Inductive: Assume size(left) and size(right) are correct.
# Then size(node) = 1 + size(left) + size(right). Correct.
```

Structural induction is the primary tool for proving correctness of recursive algorithms and data structure operations. Every recursive function has a structural induction proof implicitly embedded in its logic.

### Counting and Cardinality

**Cardinality** is the measure of the "number of elements" in a set. For finite sets, cardinality is a natural number: $|\{a, b, c\}| = 3$.

**Finite vs. infinite sets.** A set is finite if its cardinality is some natural number $n$. Otherwise it is infinite.

$$
\mathbb{N} \text{ is infinite}
$$

**Countability.** A set is **countably infinite** (or denumerable) if it can be put into a one-to-one correspondence with $\mathbb{N}$. Countable sets can be enumerated in a sequence.

- $\mathbb{N}$ is countably infinite (obviously — it is the template).
- $\mathbb{Z}$ (integers) is countably infinite: $0, 1, -1, 2, -2, 3, -3, \dots$
- $\mathbb{Q}$ (rationals) is countably infinite (Cantor's diagonal argument shows this).
- $\mathbb{R}$ (reals) is uncountably infinite — there are more reals than naturals.

**Why countability matters in computing:**

- Countable sets can be iterated through by a computer program (in principle, given infinite time and memory).
- Uncountable sets like $\mathbb{R}$ cannot — you can only approximate them.
- Many problems in computability theory turn on whether a set is countable.

```python
def enumerate_integers(n):
    """Return the first n integers in the standard enumeration."""
    result = [0]
    for i in range(1, (n + 1) // 2 + 1):
        result.append(i)
        result.append(-i)
    return result[:n]

# The enumeration proves Z is countable:
# 0 -> 0, 1 -> 1, 2 -> -1, 3 -> 2, 4 -> -2, ...
```

### Number Representations

**Unary (tally) representation.** The simplest way to represent a natural number is with tally marks. The number $n$ is represented by $n$ identical symbols:

$$
3 = |||
$$
$$
5 = |||||
$$

Unary is impractical for large numbers (representing $1000$ requires $1000$ symbols), but it is theoretically important.

**Church numerals.** In lambda calculus, natural numbers are represented as functions:

$$
0 = \lambda f.\lambda x. x
$$
$$
1 = \lambda f.\lambda x. f(x)
$$
$$
2 = \lambda f.\lambda x. f(f(x))
$$
$$
n = \lambda f.\lambda x. f^n(x)
$$

A Church numeral $n$ is a function that applies its first argument $f$ to its second argument $x$ exactly $n$ times.

```python
# Church numerals in Python
def church_zero(f):
    return lambda x: x

def church_succ(n):
    """Successor: apply f one more time."""
    return lambda f: lambda x: f(n(f)(x))

def church_to_int(n):
    """Convert Church numeral to integer."""
    return n(lambda x: x + 1)(0)

# Examples
one = church_succ(church_zero)
two = church_succ(one)
three = church_succ(two)

assert church_to_int(three) == 3
```

Church numerals show that natural numbers can be built from pure functions — no built-in numbers needed. This is the foundation of computation in pure lambda calculus and functional programming languages.

**Positional notation.** The decimal system represents numbers as sums of powers of $10$:

$$
342 = 3 \times 10^2 + 4 \times 10^1 + 2 \times 10^0
$$

Binary is the same but with base $2$:

$$
1011_2 = 1 \times 2^3 + 0 \times 2^2 + 1 \times 2^1 + 1 \times 2^0 = 11_{10}
$$

```python
def to_binary(n):
    """Convert a natural number to its binary string representation."""
    if n == 0:
        return "0"
    bits = []
    while n > 0:
        bits.append(str(n % 2))
        n //= 2
    return "".join(reversed(bits))

def from_binary(bits):
    """Convert binary string to natural number."""
    result = 0
    for bit in bits:
        result = result * 2 + int(bit)
    return result
```

### Natural Numbers in Computing

**Array indexing.** Every array access uses natural numbers (or whole numbers, if zero-indexed) as indices. The choice of $0$-based or $1$-based indexing corresponds to whether indices are natural numbers $\mathbb{N}_0$ or $\mathbb{N}$.

```python
# Zero-indexed (N_0): C, Python, Java, Rust
arr = [10, 20, 30]
for i in range(len(arr)):  # i in {0, 1, 2}
    print(arr[i])

# One-indexed (N): MATLAB, Julia, Lua
# for i = 1:length(arr)  # i in {1, 2, 3}
```

**Loop counters.** Every `for` loop that iterates from a lower bound to an upper bound is enumerating a subset of natural numbers.

```python
for i in range(100):  # 0, 1, 2, ..., 99
    pass

for i in range(1, 101):  # 1, 2, ..., 100
    pass
```

**Recursion depth.** The depth of a recursive call stack is a natural number. Recursion cannot exceed available stack space — a practical constraint modeled by the natural numbers.

```python
import sys

sys.setrecursionlimit(10000)  # max recursion depth

def factorial(n):
    if n == 0:
        return 1
    return n * factorial(n - 1)  # depth = n
```

**Loop invariants and induction.** Proving a loop correct means establishing:

1. **Initialization:** The invariant holds before the loop starts.
2. **Maintenance:** If it holds before an iteration, it holds after.
3. **Termination:** The loop terminates, and the invariant implies correctness.

This is mathematical induction applied to every iteration count $0, 1, 2, \dots, k$.

**Counting and enumeration.** Many algorithms reduce to counting: counting occurrences, counting paths, counting combinations. The result is always a natural number.

```python
from collections import Counter

def count_words(text):
    """Count word frequencies — result is a dict of natural numbers."""
    return Counter(text.lower().split())

# Result: {'the': 42, 'a': 15, ...}
```

**Size and cardinality in data structures.** The size of an array, the length of a string, the number of nodes in a tree — all are natural numbers.

```python
arr = [1, 2, 3, 4, 5]
assert len(arr) == 5  # natural number

string = "hello"
assert len(string) == 5  # natural number
```

**Church numerals in functional programming.** While most languages use machine integers, Church numerals appear in type-level programming in Haskell, Scala, and TypeScript:

```typescript
// TypeScript type-level natural numbers
type Zero = [];
type Succ<N extends any[]> = [...N, any];

type One = Succ<Zero>;
type Two = Succ<One>;
type Three = Succ<Two>;

// Length constraint using type-level naturals
type ArrayOfLength<N extends any[], T> = {
    [K in keyof N]: T;
};

type ThreeStrings = ArrayOfLength<Three, string>;  // [string, string, string]
```

## Glossary

| Term | Definition |
|------|------------|
| Additive identity | An element $e$ such that $a + e = a$ for all $a$; $0$ is the additive identity |
| Associativity | Property where grouping of operations does not affect the result: $(a + b) + c = a + (b + c)$ |
| Cardinality | The number of elements in a set |
| Church numeral | A representation of natural numbers as functions in lambda calculus |
| Closure | A set is closed under an operation if applying that operation to members of the set always produces a member of the set |
| Commutativity | Property where order of operands does not affect the result: $a + b = b + a$ |
| Countable | A set that can be put into one-to-one correspondence with $\mathbb{N}$ |
| Distributivity | Property linking addition and multiplication: $a \times (b + c) = a \times b + a \times c$ |
| Induction | A proof technique that proves a base case and an inductive step to establish a statement for all natural numbers |
| Inductive hypothesis | The assumption in an induction proof that the statement holds for smaller cases |
| Injective | A function where distinct inputs produce distinct outputs |
| Loop invariant | A condition that holds before each iteration of a loop, used to prove correctness |
| Natural numbers | The counting numbers $\{1, 2, 3, \dots\}$ or $\{0, 1, 2, \dots\}$ depending on convention |
| Peano axioms | Five axioms that define the natural numbers using zero and a successor function |
| Structural induction | Induction on recursively-defined data structures |
| Strong induction | Induction where the hypothesis assumes the statement holds for all smaller values, not just the immediate predecessor |
| Successor function | The function $S(n) = n + 1$, used in the Peano axioms |
| Total order | A binary relation that is reflexive, antisymmetric, transitive, and total (every pair is comparable) |
| Unary | A numeral system where $n$ is represented by $n$ repeated symbols (tally marks) |
| Well-ordering principle | Every non-empty set of natural numbers has a least element |
| Whole numbers | The set $\mathbb{N}_0 = \{0, 1, 2, 3, \dots\}$ |

## Quick References

- [Peano axioms (Wolfram)](https://mathworld.wolfram.com/PeanosAxioms.html) — reference for the axioms of natural numbers
- [Church numerals (Wikipedia)](https://en.wikipedia.org/wiki/Church_encoding) — Church's encoding of natural numbers in lambda calculus
- [Loop Invariants (MIT)](https://ocw.mit.edu/courses/6-042j-mathematics-for-computer-science-fall-2010/resources/lecture-4-induction/) — MIT OCW lecture on induction and loop invariants
- [ISO 80000-2](https://www.iso.org/standard/64973.html) — international standard for mathematical notation

## Next Steps

- [Integers](integers.md) — extending natural numbers with additive inverses (negative numbers)
- [Rational Numbers](rational-numbers.md) — fractions, ratios, and the need for division
- [Set Theory](../../discrete-mathematics/set-theory.md) — the formal foundation on which number systems are built
- [Functions & Relations](../../discrete-mathematics/functions-and-relations.md) — the mathematical structures used to define operations on numbers
- [Mathematical Thinking & Proofs](../../intro/mathematical-thinking.md) — deepen your understanding of induction and proof techniques
