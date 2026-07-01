# Real Numbers

## Description

Real numbers ($\mathbb{R}$) unify rational and irrational numbers into a continuous, complete ordered field that underpins calculus, numerical computing, and all continuous mathematics. Every floating-point computation, physics simulation, and statistical model approximates operations on real numbers. Understanding their properties — completeness, the supremum, the Archimedean property, and cardinality — reveals why computers struggle with exact real arithmetic, why interval arithmetic exists, and how numerical error is inherent.

## Prerequisites

- [Rational Numbers](rational-numbers.md) — fractions, decimal expansions, density in $\mathbb{R}$
- [Irrational Numbers](irrational-numbers.md) — numbers that cannot be written as a ratio of integers

## Table of Contents

- [Definition and the Real Number Line](#definition-and-the-real-number-line)
- [Construction of the Reals](#construction-of-the-reals)
- [Order and the Completeness Property](#order-and-the-completeness-property)
- [Supremum and Infimum](#supremum-and-infimum)
- [The Archimedean Property](#the-archimedean-property)
- [Intervals](#intervals)
- [Absolute Value and Distance](#absolute-value-and-distance)
- [Neighborhoods and the Epsilon-Delta Foundation](#neighborhoods-and-the-epsilon-delta-foundation)
- [Cardinality of the Reals](#cardinality-of-the-reals)
- [The Continuum Hypothesis](#the-continuum-hypothesis)
- [Real Numbers in Computing](#real-numbers-in-computing)
- [IEEE 754 and the Finite Representation Problem](#ieee-754-and-the-finite-representation-problem)
- [Interval Arithmetic](#interval-arithmetic)
- [Applications of Real Numbers](#applications-of-real-numbers)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Definition and the Real Number Line

The real numbers $\mathbb{R}$ are the union of the rational numbers $\mathbb{Q}$ and the irrational numbers $\mathbb{R} \setminus \mathbb{Q}$. Every real number corresponds to a unique point on the **real number line** — an infinite one-dimensional continuum extending from $-\infty$ to $+\infty$.

$$
\mathbb{R} = \mathbb{Q} \cup (\mathbb{R} \setminus \mathbb{Q})
$$

The real number line is complete: there are no gaps. Between any two distinct reals there exist infinitely many others. The reals form an **ordered field**:

| Property | Description |
|----------|-------------|
| Field | Closed under $+$, $\times$, with identities $0$, $1$, additive inverses, multiplicative inverses for non-zero elements |
| Ordered | For $a,b \in \mathbb{R}$, exactly one of $a < b$, $a = b$, $a > b$ holds. Order is transitive and compatible with $+$ and $\times$ |
| Complete | Every non-empty subset bounded above has a supremum in $\mathbb{R}$ |

The completeness axiom distinguishes $\mathbb{R}$ from $\mathbb{Q}$. The set $\{x \in \mathbb{Q} : x^2 < 2\}$ is bounded above in $\mathbb{Q}$ but has no supremum in $\mathbb{Q}$ (the supremum $\sqrt{2}$ is irrational). In $\mathbb{R}$, every bounded set has a supremum.

### Construction of the Reals

Two standard constructions define $\mathbb{R}$ from $\mathbb{Q}$.

**Dedekind cuts (1872):** A Dedekind cut partitions $\mathbb{Q}$ into $A$ and $B$ where every element of $A$ is less than every element of $B$, $A$ has no greatest element, and $A \cup B = \mathbb{Q}$. Rational numbers correspond to cuts where $B$ has a least element; irrationals correspond to cuts where $B$ has no least element. For $\sqrt{2}$:

$$
A = \{x \in \mathbb{Q} : x < 0 \text{ or } x^2 < 2\}, \quad B = \{x \in \mathbb{Q} : x > 0 \text{ and } x^2 > 2\}
$$

**Cauchy sequences (Cantor, 1872):** A Cauchy sequence of rationals $(x_n)$ satisfies that for any $\epsilon > 0$, there exists $N$ with $|x_m - x_n| < \epsilon$ for all $m,n \ge N$. Two Cauchy sequences are equivalent if their difference tends to zero. $\mathbb{R}$ is the set of equivalence classes.

Both constructions produce the same $\mathbb{R}$ (up to isomorphism). The key takeaway: real numbers are defined as limits of rational approximations, which directly mirrors floating-point computation.

### Order and the Completeness Property

**Completeness axiom:** Every non-empty subset $S \subseteq \mathbb{R}$ that is bounded above has a least upper bound (supremum) in $\mathbb{R}$.

Formally, if $S \neq \emptyset$ and $\exists M \in \mathbb{R}$ with $s \le M$ for all $s \in S$, then $\exists \sup S \in \mathbb{R}$ such that:
- $s \le \sup S$ for all $s \in S$
- If $M$ is any upper bound, then $\sup S \le M$

This axiom is equivalent to:
- Every non-empty subset bounded below has an infimum
- The nested interval property: a nested sequence of closed intervals with lengths $\to 0$ has non-empty intersection
- Every Cauchy sequence converges
- The intermediate value property for continuous functions

```python
def is_upper_bound(S: list[float], M: float) -> bool:
    return all(s <= M for s in S)
```

### Supremum and Infimum

**Supremum (least upper bound):** $\sup S$ is the smallest number $\ge$ every element of $S$.

**Infimum (greatest lower bound):** $\inf S$ is the largest number $\le$ every element of $S$.

| Concept | Notation | Property |
|---------|----------|----------|
| Supremum | $\sup S$ | Least upper bound |
| Infimum | $\inf S$ | Greatest lower bound |
| Maximum | $\max S$ | Supremum that belongs to $S$ |
| Minimum | $\min S$ | Infimum that belongs to $S$ |

**Examples:**
- $S = (0, 1)$: $\sup S = 1$, $\inf S = 0$. Neither belongs to $S$, so $\max$ and $\min$ do not exist.
- $S = \{1 - 1/n : n \in \mathbb{N}^+\}$: $\sup S = 1$ (not attained), $\inf S = 0$ (attained at $n=1$).
- $S = \{x \in \mathbb{Q} : x^2 < 2\}$: $\sup S = \sqrt{2}$ (irrational, not in $S$).

**Approximation property:** For any $\epsilon > 0$, there exists $s \in S$ such that $\sup S - \epsilon < s \le \sup S$. This is the basis for numerical approximation — we can get arbitrarily close to the supremum.

```python
def gradient_stop_criterion(gradients: list[float], tol: float = 1e-6) -> bool:
    """Stop gradient descent when supremum of gradient components is small."""
    return max(abs(g) for g in gradients) < tol
```

### The Archimedean Property

**Statement:** For any real $x$, there exists $n \in \mathbb{N}$ such that $n > x$.

Equivalently:
- $\mathbb{N}$ is unbounded in $\mathbb{R}$
- For any $x > 0$, $\exists n \in \mathbb{N}$ with $1/n < x$
- For any $a, b > 0$, $\exists n \in \mathbb{N}$ with $na > b$

```python
import math

def archimedean_n(x: float) -> int:
    """Archimedean property: there exists integer n > x."""
    return math.floor(x) + 1
```

**Consequences:** Rationals are dense in $\mathbb{R}$ (any real can be approximated by a rational to arbitrary precision). There is no infinitesimal real. Every real has an integer ceiling, which makes floor/ceil functions well-defined.

### Intervals

An interval is a connected subset of $\mathbb{R}$, the building block of domains in calculus and numerical methods.

| Type | Notation | Example |
|------|----------|---------|
| Closed | $[a, b]$ | $[0, 1]$ |
| Open | $(a, b)$ | $(0, \pi)$ |
| Half-open | $[a, b)$ | $[0, \infty)$ |
| Half-open | $(a, b]$ | $(-\infty, 0]$ |
| Unbounded | $(a, \infty)$, $(-\infty, b)$ | $(0, \infty)$ |

```python
class Interval:
    """Closed interval [a, b]."""
    def __init__(self, a: float, b: float): self.a, self.b = a, b
    def contains(self, x: float) -> bool: return self.a <= x <= self.b
    def length(self) -> float: return self.b - self.a
    def intersects(self, o: 'Interval') -> bool: return self.a <= o.b and o.a <= self.b

# Nested interval property in bisection
def bisection_root(f, a: float, b: float, tol: float = 1e-12) -> float:
    """Find root of f in [a, b] where f(a)f(b) < 0."""
    fa, fb = f(a), f(b)
    while (b - a) / 2 > tol:
        m = (a + b) / 2
        fm = f(m)
        if abs(fm) < tol: return m
        if fa * fm < 0: b, fb = m, fm
        else: a, fa = m, fm
    return (a + b) / 2
```

### Absolute Value and Distance

$$
|x| = \begin{cases} x & x \ge 0 \\ -x & x < 0 \end{cases}
$$

**Properties:**

| Property | Formula |
|----------|---------|
| Non-negativity | $|x| \ge 0$, equality iff $x = 0$ |
| Triangle inequality | $|x + y| \le |x| + |y|$ |
| Reverse triangle | $||x| - |y|| \le |x - y|$ |
| Multiplication | $|xy| = |x||y|$ |

The **distance** between reals $x$ and $y$ is $d(x,y) = |x - y|$, the standard Euclidean metric on $\mathbb{R}$.

```python
def distance(x: float, y: float) -> float: return abs(x - y)
def clamp(x: float, lo: float, hi: float) -> float: return max(lo, min(x, hi))
```

**Applications:** error measurement $\epsilon = |x_{\text{approx}} - x_{\text{exact}}|$, relative error $\delta = \epsilon / |x_{\text{exact}}|$, convergence ($x_n \to L$ iff $|x_n - L| \to 0$).

### Neighborhoods and the Epsilon-Delta Foundation

An **epsilon neighborhood** (open ball) centered at $c$ with radius $\epsilon > 0$:

$$
B_\epsilon(c) = \{x \in \mathbb{R} : |x - c| < \epsilon\} = (c - \epsilon, c + \epsilon)
$$

The **epsilon-delta definition of limit:** $\lim_{x \to c} f(x) = L$ means:

For every $\epsilon > 0$, there exists $\delta > 0$ such that $0 < |x - c| < \delta$ implies $|f(x) - L| < \epsilon$.

```python
def check_limit(f, c: float, L: float, eps: float = 1e-6, delta: float = 1e-6) -> bool:
    """Numerically test epsilon-delta condition at c."""
    import random
    for _ in range(1000):
        x = c + random.uniform(-delta, delta)
        if x != c and abs(f(x) - L) >= eps: return False
    return True
```

**Continuity:** $f$ is continuous at $c$ if $\lim_{x \to c} f(x) = f(c)$. In epsilon-delta: for every $\epsilon > 0$, $\exists \delta > 0$ such that $|x - c| < \delta$ implies $|f(x) - f(c)| < \epsilon$.

The epsilon-delta framework is the foundation for:
- Numerical differentiation: $\frac{df}{dx} \approx \frac{f(x+h) - f(x)}{h}$ as $h \to 0$
- Root finding: bisection, Newton converge within epsilon tolerance
- Integration: Riemann sums approximate the integral with error controlled by partition size

### Cardinality of the Reals

$|\mathbb{R}|$ is strictly larger than $|\mathbb{N}|$. $\mathbb{R}$ is **uncountable**, proven by Cantor's diagonal argument (1891).

**Cantor's diagonal argument (sketch):**
1. Suppose $[0,1]$ is countable: list $r_1, r_2, r_3, \ldots$
2. Write each $r_i = 0.d_{i1}d_{i2}d_{i3}\ldots$ in decimal
3. Construct $x = 0.x_1x_2x_3\ldots$ where $x_i \neq d_{ii}$
4. $x$ differs from every $r_i$, so it is not in the list — contradiction

**Cardinality hierarchy:**

| Set | Cardinality |
|-----|-------------|
| $\mathbb{N}, \mathbb{Z}, \mathbb{Q}$ | $\aleph_0$ (countable) |
| Algebraic numbers | $\aleph_0$ |
| $\mathbb{R}$ | $2^{\aleph_0} = \mathfrak{c}$ (continuum) |
| $\mathcal{P}(\mathbb{R})$ | $2^{\mathfrak{c}}$ |

$$|\mathbb{R}| = |\mathcal{P}(\mathbb{N})| = 2^{\aleph_0}$$

**Implications for computing:** There are uncountably many reals but only countably many programs. Most real numbers are **non-computable** — no algorithm can output their digits. All numbers manipulated by computers are rational approximations.

### The Continuum Hypothesis

Posed by Cantor (1878): **There is no set whose cardinality is strictly between $\aleph_0$ and $2^{\aleph_0}$.**

In other words, $2^{\aleph_0} = \aleph_1$.

**Status:** Gödel (1940) proved CH is consistent with ZFC. Cohen (1963) proved it is independent — CH cannot be proved or disproved within standard set theory.

For developers: no direct computational consequences, but it highlights that $\mathbb{R}$ contains far more elements than our symbolic systems can describe.

### Real Numbers in Computing

Computers cannot represent arbitrary reals. Every stored number is a rational with a power-of-two denominator.

$$
|\text{Float64}| = 2^{64} \text{ values}, \quad |\mathbb{R}| = 2^{\aleph_0} \gg 2^{64}
$$

```python
import sys

# Machine epsilon
eps = sys.float_info.epsilon
print(f"Machine epsilon: {eps}")  # 2.22e-16

# The 0.1 problem
print(f"{0.1:.20f}")  # 0.10000000000000000555
print(0.1 + 0.2 == 0.3)  # False
print(0.1 + 0.2)         # 0.30000000000000004
```

**Strategies for inexact reals:**

| Strategy | Description | Use case |
|----------|-------------|----------|
| Tolerances | `abs(a - b) < tol` | General equality |
| Relative tolerance | `abs(a - b) / max(abs(a), abs(b), 1) < tol` | Mixed magnitudes |
| Decimal | `decimal.Decimal` | Financial |
| Rational | `fractions.Fraction` | Exact symbolic |
| Interval | Enclose value in $[a,b]$ | Verified computing |

```python
import math
print(math.isclose(0.1 + 0.2, 0.3, rel_tol=1e-15))  # True
```

### IEEE 754 and the Finite Representation Problem

IEEE 754 is the standard for floating-point arithmetic on virtually all CPUs. It specifies:

- **Formats:** binary32 (float), binary64 (double), binary128 (quad)
- **Special values:** $\pm\infty$, NaN, $\pm 0$
- **Rounding modes:** nearest (ties to even), toward zero, toward $\pm\infty$
- **Exceptions:** overflow, underflow, division by zero, invalid, inexact

**Float64 layout:** 1 sign bit, 11 exponent bits (biased 1023), 52 mantissa bits.

$$
\text{value} = (-1)^s \times (1 + m / 2^{52}) \times 2^{e - 1023}
$$

**Why 0.1 is not exact:** In binary, $1/10 = 1/(2 \cdot 5)$ has prime factor 5 in the denominator, so its binary representation repeats:

$$
0.1_{10} = 0.00011001100110011\ldots_2
$$

With only 52 mantissa bits, truncation introduces an error of $\approx 10^{-17}$.

```python
import struct

def float_components(x: float) -> dict:
    packed = struct.pack('>d', x)
    bits = format(int.from_bytes(packed, 'big'), '064b')
    return {'sign': bits[0], 'exponent': bits[1:12], 'mantissa': bits[12:]}

# NaN property: NaN != NaN
nan = float('nan')
print(nan == nan)  # False — must use math.isnan()
```

**Rounding error accumulation:**

```python
def naive_sum(values: list[float]) -> float:
    s = 0.0
    for v in values: s += v
    return s

def kahan_sum(values: list[float]) -> float:
    """Kahan compensated summation — error O(epsilon) instead of O(n epsilon)."""
    s, c = 0.0, 0.0
    for v in values:
        y = v - c
        t = s + y
        c = (t - s) - y
        s = t
    return s

n = 10_000_000
values = [0.1] * n
print(f"Naive: {naive_sum(values) - n*0.1:.3f}")
print(f"Kahan: {kahan_sum(values) - n*0.1:.3e}")
```

### Interval Arithmetic

Interval arithmetic represents a real as $[a, b]$ that is guaranteed to contain the true value. Let $X = [x_l, x_u]$, $Y = [y_l, y_u]$:

| Operation | Formula |
|-----------|---------|
| $X + Y$ | $[x_l + y_l, x_u + y_u]$ |
| $X - Y$ | $[x_l - y_u, x_u - y_l]$ |
| $X \cdot Y$ | $[\min(x_l y_l, x_l y_u, x_u y_l, x_u y_u), \max(\ldots)]$ |
| $X / Y$ | $[\min(x_l/y_l, \ldots), \max(\ldots)]$ if $0 \notin Y$ |

```python
class I:
    def __init__(self, lo: float, hi: float):
        assert lo <= hi; self.lo, self.hi = lo, hi
    def __add__(self, o): return I(self.lo + o.lo, self.hi + o.hi)
    def __sub__(self, o): return I(self.lo - o.hi, self.hi - o.lo)
    def __mul__(self, o):
        p = [self.lo*o.lo, self.lo*o.hi, self.hi*o.lo, self.hi*o.hi]
        return I(min(p), max(p))
    def contains(self, x): return self.lo <= x <= self.hi
    def __repr__(self): return f"[{self.lo}, {self.hi}]"

# Bounding f(x) = x^2 over an interval
x = I(0.5, 0.7)
print(x * x)  # [0.25, 0.49]
```

**Dependency problem:** Naive interval arithmetic treats each occurrence of a variable as independent, so $X - X$ with $X = [0,1]$ gives $[-1, 1]$ instead of $[0,0]$.

**Use cases:** verified computing, global optimization (branch-and-bound), ray tracing (bounding volume hierarchies), root finding (interval Newton method).

### Applications of Real Numbers

**Continuous simulation:** Physics engines solve ODEs over real-valued state:

```python
def spring_simulation(x0: float, v0: float, k: float, m: float, dt: float, steps: int):
    x, v = x0, v0
    traj = [(x, v)]
    for _ in range(steps):
        a = -k * x / m
        v += a * dt
        x += v * dt
        traj.append((x, v))
    return traj
```

**Graphics and rendering:** Transformations use real matrices. Bézier curves, anti-aliasing, ray-surface intersection all depend on real arithmetic.

```python
def lerp(a: float, b: float, t: float) -> float:
    """Linear interpolation, fundamental in graphics."""
    return a + t * (b - a)

def smoothstep(t: float) -> float:
    """Cubic Hermite interpolation: 3t^2 - 2t^3."""
    return t * t * (3 - 2 * t)
```

**Numerical methods:**

| Method | Real concept | Error |
|--------|-------------|-------|
| Newton's method | Derivative at real point | $O(h^2)$ per iteration |
| Gaussian quadrature | Integral of polynomial approx | $O(h^{2n})$ |
| Runge-Kutta | Taylor series truncation | $O(h^4)$ |

```python
def newton(f, fp, x0: float, tol: float = 1e-14, max_iter: int = 100) -> float:
    x = x0
    for _ in range(max_iter):
        fx = f(x)
        if abs(fx) < tol: return x
        x -= fx / fp(x)
    raise ValueError("Did not converge")

sqrt2 = newton(lambda x: x**2 - 2, lambda x: 2*x, 1.5)
print(f"sqrt(2) via Newton: {sqrt2:.15f}")
```

**Machine learning:** Gradient descent navigates the real-valued loss landscape:

```python
def gradient_descent(fp, x0: float, lr: float = 0.01, n: int = 1000) -> float:
    x = x0
    for _ in range(n): x -= lr * fp(x)
    return x

# Minimize f(x) = x^2 (minimum at 0)
print(gradient_descent(lambda x: 2*x, 5.0, lr=0.1, n=100))
```

## Glossary

| Term | Definition |
|------|------------|
| Real numbers | The complete ordered field $\mathbb{R}$, union of rationals and irrationals |
| Completeness axiom | Every non-empty subset bounded above has a supremum in $\mathbb{R}$ |
| Supremum | Least upper bound of a set |
| Infimum | Greatest lower bound of a set |
| Archimedean property | For any $x \in \mathbb{R}$, $\exists n \in \mathbb{N}$ with $n > x$ |
| Dedekind cut | Partition of $\mathbb{Q}$ defining a real number |
| Cauchy sequence | Sequence whose terms become arbitrarily close to each other |
| Interval | Connected subset of $\mathbb{R}$, e.g., $[a,b]$ or $(a,b)$ |
| Absolute value | $|x| = x$ for $x \ge 0$, $-x$ for $x < 0$ |
| Epsilon neighborhood | $(c-\epsilon, c+\epsilon)$, points within $\epsilon$ of $c$ |
| Continuum | Cardinality $2^{\aleph_0}$ of $\mathbb{R}$ |
| Uncountable | Cardinality strictly greater than $\aleph_0$ |
| Cantor diagonal argument | Proof that $\mathbb{R}$ is uncountable |
| Continuum hypothesis | $2^{\aleph_0} = \aleph_1$, independent of ZFC |
| Machine epsilon | Smallest $\epsilon$ with $1.0 + \epsilon \neq 1.0$ in floating-point |
| Catastrophic cancellation | Precision loss when subtracting nearly equal numbers |
| Interval arithmetic | Representing a real as $[a,b]$ guaranteed to contain the true value |
| IEEE 754 | Standard for floating-point arithmetic |
| NaN | Not a Number, IEEE 754 value for undefined results |
| ULP | Unit in the Last Place, spacing between adjacent floats |
| Kahan summation | Compensated algorithm reducing rounding error to $O(\epsilon)$ |

## Quick References

- [What Every Computer Scientist Should Know About Floating-Point Arithmetic](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html) — David Goldberg, 1991
- [IEEE 754 Standard](https://standards.ieee.org/ieee/754/6210/) — official standard
- [Real Numbers on Wikipedia](https://en.wikipedia.org/wiki/Real_number) — comprehensive reference
- [Principles of Mathematical Analysis](https://www.mheducation.com/highered/product/principles-mathematical-analysis-rudin/M9780070542358.html) — Walter Rudin

## Next Steps

- [Complex Numbers](complex-numbers.md) — extending the real line into the complex plane
- [Floating-Point Numbers](floating-point-numbers.md) — IEEE 754 in depth
