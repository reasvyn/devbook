# Irrational Numbers

## Description

Irrational numbers are the real numbers that cannot be written as a ratio of two integers — they fill the gaps between rationals on the number line. Every developer encounters irrationals when computing with $\pi$, $e$, or square roots. Understanding irrationality is essential for grasping floating-point limitations, symbolic computation, numerical approximation, and the very nature of the real numbers used in every scientific and graphics application.

## Prerequisites

- [Rational Numbers](rational-numbers.md) — the definition of $\mathbb{Q}$, fraction arithmetic, and decimal representation

## Table of Contents

- [Definition: R minus Q](#definition-r-minus-q)
- [Classic Proof: sqrt2 is Irrational](#classic-proof-sqrt2-is-irrational)
- [Other Irrational Numbers](#other-irrational-numbers)
- [Decimal Representation](#decimal-representation)
- [Algebraic vs Transcendental Numbers](#algebraic-vs-transcendental-numbers)
- [Algebraic Numbers](#algebraic-numbers)
- [Transcendental Numbers](#transcendental-numbers)
- [Liouville Numbers and Existential Proof](#liouville-numbers-and-existential-proof)
- [Cantor: Almost All Reals Are Transcendental](#cantor-almost-all-reals-are-transcendental)
- [Irrationality of pi and e](#irrationality-of-pi-and-e)
- [Irrationality of e](#irrationality-of-e)
- [Irrationality of pi](#irrationality-of-pi)
- [Density of Irrationals](#density-of-irrationals)
- [Uncountability of R](#uncountability-of-r)
- [Irrationals in Computing](#irrationals-in-computing)
- [Approximating Irrationals](#approximating-irrationals)
- [Computing pi to Arbitrary Precision](#computing-pi-to-arbitrary-precision)
- [Machin-like Formulas](#machin-like-formulas)
- [The Chudnovsky Algorithm](#the-chudnovsky-algorithm)
- [Symbolic Computation vs Numeric Approximation](#symbolic-computation-vs-numeric-approximation)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Definition: R minus Q

An **irrational number** is a real number that cannot be expressed as a ratio of two integers:

$$
\mathbb{R} \setminus \mathbb{Q} = \{ x \in \mathbb{R} \mid x \notin \mathbb{Q} \}
$$

Equivalently, $x$ is irrational if there exist no integers $p, q$ with $q \neq 0$ such that $x = p/q$. Irrational numbers have non-terminating, non-repeating decimal expansions. They arise naturally from square roots, logarithms, trigonometric functions, and many geometric constants.

### Classic Proof: sqrt2 is Irrational

The proof that $\sqrt{2}$ is irrational is a classic example of proof by contradiction, attributed to the Pythagorean school.

**Theorem:** $\sqrt{2}$ is irrational.

**Proof:** Assume $\sqrt{2} = p/q$ where $p, q \in \mathbb{Z}$, $q \neq 0$, and the fraction is in lowest terms (so $\gcd(p, q) = 1$).

Squaring both sides: $2 = p^2 / q^2$, so $p^2 = 2 q^2$.

This means $p^2$ is even, so $p$ must be even (the square of an odd number is odd). Write $p = 2k$.

Substituting: $(2k)^2 = 2 q^2$ simplifies to $4k^2 = 2 q^2$, or $q^2 = 2 k^2$.

Thus $q^2$ is even, so $q$ is even. But if both $p$ and $q$ are even, then $\gcd(p, q) \ge 2$, contradicting the assumption that $p/q$ is in lowest terms.

Therefore no such representation exists — $\sqrt{2}$ is irrational. $\square$

```python
def is_sqrt_irrational_approximate(n: int, tol: float = 1e-15) -> bool:
    """Check if sqrt(n) is irrational by verifying it is not an integer
    and that its square does not converge to exact integer root."""
    r = n ** 0.5
    return abs(r - round(r)) > tol

# Only perfect squares have rational square roots
for n in [2, 3, 4, 5, 9, 16]:
    print(f"sqrt({n}) irrational: {is_sqrt_irrational_approximate(n)}")
```

### Other Irrational Numbers

Many important mathematical constants are irrational:

- $\pi = 3.1415926535\dots$ — the ratio of a circle's circumference to its diameter
- $e = 2.7182818284\dots$ — the base of natural logarithms
- $\phi = \frac{1 + \sqrt{5}}{2} = 1.6180339887\dots$ — the golden ratio
- $\sqrt{3}, \sqrt{5}, \sqrt{7}$ — square roots of non-square integers
- $\sqrt{p}$ for any prime $p$ (generalization of the $\sqrt{2}$ proof)
- $\log_2 3$ — logarithms of coprime integers are irrational
- $\sin(1), \cos(1)$ — most trigonometric values at rational arguments are irrational (Lindemann-Weierstrass)

```python
import math

irrationals = {
    "pi": math.pi,
    "e": math.e,
    "phi": (1 + math.sqrt(5)) / 2,
    "sqrt(3)": math.sqrt(3),
    "sqrt(7)": math.sqrt(7),
}

for name, val in irrationals.items():
    print(f"{name} = {val:.15f}")
```

### Decimal Representation

The decimal expansion of an irrational number is **non-terminating** and **non-repeating**. Unlike rational numbers, there is no finite block of digits that cycles forever. The digits appear to be random, though proving randomness is an open problem for constants like $\pi$ (it is not known whether $\pi$ is normal — whether every digit sequence appears with equal frequency).

Current known digits:

- $\pi$: over 100 trillion digits computed (2022)
- $e$: over 1 trillion digits computed
- $\sqrt{2}$: over 10 trillion digits computed

Despite this, we will never write the full decimal expansion of any irrational number — doing so would require infinite storage.

```python
# Computing digits of pi using the built-in math.pi (only ~15 digits)
import math
print(f"{math.pi:.15f}")

# For more digits, we use arbitrary-precision libraries
# Or series expansions like the Chudnovsky algorithm (see later section)
```

### Algebraic vs Transcendental Numbers

Irrational numbers are classified into two categories based on whether they can be roots of polynomial equations with integer coefficients.

### Algebraic Numbers

An **algebraic number** is a number that is a root of a non-zero polynomial with integer coefficients:

$$
a_0 x^n + a_1 x^{n-1} + \cdots + a_n = 0, \quad a_i \in \mathbb{Z}
$$

Examples:

- $\sqrt{2}$ is algebraic: it satisfies $x^2 - 2 = 0$
- $\phi = (1 + \sqrt{5})/2$ is algebraic: it satisfies $x^2 - x - 1 = 0$
- $\sqrt[3]{7}$ is algebraic: it satisfies $x^3 - 7 = 0$
- Every rational number $p/q$ is algebraic: it satisfies $q x - p = 0$

The set of algebraic numbers is denoted $\overline{\mathbb{Q}}$ and is countable (every polynomial has finitely many roots, and there are countably many polynomials with integer coefficients).

```python
import sympy as sp

# Verify that sqrt(2) is a root of x^2 - 2
x = sp.Symbol('x')
poly = x**2 - 2
print(sp.factor(poly))             # x**2 - 2
print(poly.subs(x, sp.sqrt(2)))    # 0

# The golden ratio satisfies x^2 - x - 1 = 0
phi = (1 + sp.sqrt(5)) / 2
print(sp.simplify(phi**2 - phi - 1))  # 0
```

### Transcendental Numbers

A **transcendental number** is a real number that is not algebraic — it satisfies no polynomial equation with integer coefficients. Proving transcendence is much harder than proving irrationality.

Well-known transcendental numbers:

- $\pi$ — proven transcendental by Lindemann in 1882
- $e$ — proven transcendental by Hermite in 1873
- $e^\pi$ (Gelfond's constant) — proven transcendental by Gelfond in 1934
- $2^{\sqrt{2}}$ — proven transcendental by Gelfond-Schneider theorem (1934)
- $\ln(2)$ — proven transcendental by Lindemann-Weierstrass
- $\sin(1), \cos(1), \tan(1)$ — Lindemann-Weierstrass theorem implies these are transcendental

```python
# Check if sympy considers numbers algebraic
import sympy as sp

print(sp.sqrt(2).is_algebraic)     # True
print(sp.pi.is_algebraic)          # None (unknown to sympy, but known to be transcendental)
print(sp.E.is_algebraic)           # None
```

### Liouville Numbers and Existential Proof

Before Hermite and Lindemann, Liouville (1844) constructed the first explicit transcendental numbers. A **Liouville number** is a real number $x$ such that for every integer $n > 0$, there exist integers $p, q$ with $q > 1$ satisfying:

$$
0 < \left| x - \frac{p}{q} \right| < \frac{1}{q^n}
$$

Liouville numbers are "extremely well approximable" by rationals. The classic example is:

$$
L = \sum_{k=1}^\infty 10^{-k!} = 0.110001000000000000000001\dots
$$

where the digit $1$ appears at positions $1!, 2!, 3!, \dots$ and zeros elsewhere. Liouville proved such numbers are transcendental by showing that if an algebraic number of degree $d$ satisfies the approximation inequality for $n > d$, it leads to a contradiction.

```python
def liouville_digit(pos: int) -> int:
    """Return the digit at position pos (1-indexed) of the Liouville constant."""
    import math
    for k in range(1, 10):
        if math.factorial(k) == pos:
            return 1
    return 0

# First 30 digits
digits = [str(liouville_digit(i)) for i in range(1, 31)]
print("0." + "".join(digits))  # 0.110001000000000000000001000...
```

### Cantor: Almost All Reals Are Transcendental

Cantor's 1874 diagonal argument showed that $\mathbb{R}$ is uncountable. Since the algebraic numbers $\overline{\mathbb{Q}}$ are countable (there are countably many polynomials with integer coefficients, each with finitely many roots), it follows that:

- There are uncountably many transcendental numbers.
- **Almost every** real number (in the sense of Lebesgue measure) is transcendental.
- Explicitly constructing transcendental numbers is hard, but they vastly outnumber algebraic ones.

This is a classic example of a non-constructive existence proof: Cantor proved transcendental numbers exist without producing a single one.

### Irrationality of pi and e

The proofs that $\pi$ and $e$ are irrational are accessible while being historically significant.

### Irrationality of e

Hermite's proof (1873) that $e$ is irrational uses a simple infinite series argument.

**Theorem:** $e$ is irrational.

**Proof sketch:** Assume $e = a/b$ with $a, b \in \mathbb{Z}^+$. Write:

$$
e = \sum_{n=0}^\infty \frac{1}{n!}
$$

Let $b! e = b! \sum_{n=0}^\infty \frac{1}{n!}$. The sum splits into an integer part (terms $n \le b$) and a remainder:

$$
b! e = \underbrace{\sum_{n=0}^b \frac{b!}{n!}}_{\text{integer}} + \underbrace{\sum_{n=b+1}^\infty \frac{b!}{n!}}_{R}
$$

The remainder $R$ satisfies:

$$
0 < R = \frac{1}{b+1} + \frac{1}{(b+1)(b+2)} + \cdots < \frac{1}{b+1} + \frac{1}{(b+1)^2} + \cdots = \frac{1}{b}
$$

Since $b! e$ is an integer (by the assumption $e = a/b$), $R$ would be an integer minus an integer — so $R$ would be an integer. But $0 < R < 1/b \le 1$, contradiction. $\square$

### Irrationality of pi

Lambert (1761) proved $\pi$ is irrational using continued fractions. He showed that $\tan(x)$ is irrational for every non-zero rational $x$. Since $\tan(\pi/4) = 1$ is rational, $\pi/4$ cannot be rational, so $\pi$ is irrational.

A more elementary proof was given by Niven (1947):

**Theorem:** $\pi$ is irrational.

**Proof sketch (Niven):** Assume $\pi = a/b$ with $a, b \in \mathbb{Z}^+$. Define:

$$
f(x) = \frac{x^n (a - bx)^n}{n!}
$$

and consider $F(x) = f(x) - f''(x) + f^{(4)}(x) - \cdots + (-1)^n f^{(2n)}(x)$. One shows that $F(0) + F(\pi)$ is an integer (by construction) but $0 < F(0) + F(\pi) < 1$ for sufficiently large $n$, a contradiction. $\square$

```python
# Demonstrating that pi is irrational: 
# we cannot find integers p, q such that pi = p/q exactly
from fractions import Fraction
import math

def best_rational_approx(x: float, max_denom: int) -> Fraction:
    """Find best rational approximation within denominator limit."""
    return Fraction(x).limit_denominator(max_denom)

for d in [7, 113, 1000, 100000]:
    approx = best_rational_approx(math.pi, d)
    error = abs(math.pi - float(approx))
    print(f"pi ≈ {approx} (error {error:.2e})")
# pi ≈ 22/7 (error 1.26e-03)
# pi ≈ 355/113 (error 2.67e-07)
# pi ≈ 355/113 (error 2.67e-07)
# pi ≈ 312689/99532 (error 3.28e-10)
```

### Density of Irrationals

The irrational numbers are **dense** in $\mathbb{R}$: between any two distinct real numbers $a < b$, there exists an irrational number. In fact, between any two reals there are infinitely many irrationals (and also infinitely many rationals).

One constructive proof: given $a < b$, let $n$ be a positive integer such that $n > \sqrt{2} / (b - a)$. Then $a \sqrt{2} / n$ is between translations — more directly, pick a rational $r$ between $a/\sqrt{2}$ and $b/\sqrt{2}$; then $r\sqrt{2}$ is irrational and lies between $a$ and $b$.

```python
import math

def irrational_between(a: float, b: float) -> float:
    """Return an irrational number between a and b."""
    if a >= b:
        raise ValueError("Require a < b")
    # Scale by sqrt(2) which is irrational
    factor = (b - a) / a if a != 0 else 1.0
    # Simple approach: pick midpoint and add a small irrational
    mid = (a + b) / 2
    # Adaptively scale sqrt(2) to fit within (a, b)
    epsilon = (b - a) * 0.01
    candidate = mid + epsilon * math.sqrt(2)
    if a < candidate < b:
        return candidate
    return mid - epsilon * math.sqrt(2)

r = irrational_between(0, 1)
print(f"{r:.15f}")  # ~0.5071067811865475
```

### Uncountability of R

Cantor's second diagonal argument (1874) proved that $\mathbb{R}$ is **uncountable** — there is no bijection between $\mathbb{N}$ and $\mathbb{R}$.

**Proof sketch:** Assume there exists a bijection $f: \mathbb{N} \to [0, 1]$, listing all real numbers in $[0, 1]$ as decimal expansions:

```
f(1) = 0.d_11 d_12 d_13 d_14 ...
f(2) = 0.d_21 d_22 d_23 d_24 ...
f(3) = 0.d_31 d_32 d_33 d_34 ...
```

Construct a new number $x = 0.x_1 x_2 x_3 \dots$ where $x_i = 5$ if $d_{ii} \neq 5$, and $x_i = 6$ if $d_{ii} = 5$. This $x$ differs from every $f(i)$ in at least the $i$-th decimal place, so it is not in the list — contradiction. Therefore no such bijection exists.

Since $\mathbb{Q}$ is countable and $\mathbb{R}$ is uncountable, $\mathbb{R} \setminus \mathbb{Q}$ (the irrationals) must be uncountable. There are vastly more irrationals than rationals.

```python
# The rationals are countable, but reals are not.
# This function demonstrates listing rationals — no such function exists for reals.
def cantor_diagonal_example(digits: list[list[int]]) -> list[int]:
    """Construct a number not in the given list using Cantor's diagonal method."""
    n = len(digits)
    result = []
    for i in range(n):
        if i < len(digits[i]):
            result.append(5 if digits[i][i] != 5 else 6)
        else:
            result.append(5)
    return result

sample = [
    [1, 4, 1, 5, 9],  # pi-ish
    [7, 1, 8, 2, 8],  # e-ish
    [2, 3, 6, 0, 6],  # sqrt(5)-ish
]
new = cantor_diagonal_example(sample)
print(f"Constructed number: 0.{''.join(map(str, new))}")  # 0.555
```

### Irrationals in Computing

Computers cannot represent irrational numbers exactly. Every irrational stored in memory is actually a rational approximation, whether using floating-point or arbitrary-precision arithmetic:

```python
import math

# math.pi is not pi — it's the closest IEEE 754 double
print(math.pi)
print(Fraction(math.pi).limit_denominator(10**15))
# 3.141592653589793
# 884279719003555/281474976710656

# The error is detectable
true_pi = "3.14159265358979323846264338327950288419716939937510"
approx_pi = f"{math.pi:.15f}"
print(f"Error: {float(true_pi[:20]) - math.pi:.2e}")
```

The key consequences for developers:

1. **Equality checks fail**: `math.sqrt(2) ** 2 == 2` is `False`
2. **Accumulating errors**: trigonometric identities drift after many operations
3. **Chaotic systems**: small initial errors amplify exponentially
4. **Numerical stability**: algorithm choice matters (e.g., using `atan2` vs computing inverse tangents)

```python
print(math.sqrt(2) ** 2 == 2)           # False
print(abs(math.sqrt(2) ** 2 - 2))       # 4.44e-16

# Use tolerance-based comparison
def approx_equal(a: float, b: float, eps: float = 1e-15) -> bool:
    return abs(a - b) < eps

print(approx_equal(math.sqrt(2) ** 2, 2))  # True
```

### Approximating Irrationals

The best rational approximations to irrational numbers come from continued fractions. The convergents of the continued fraction provide the best possible approximations given a denominator bound:

```python
from fractions import Fraction
import math

# Continued fraction for sqrt(2): [1; 2, 2, 2, 2, ...]
def sqrt2_convergents(n: int) -> list[Fraction]:
    """First n convergents of sqrt(2) = [1; 2, 2, 2, ...]"""
    result = []
    h_prev, k_prev = 1, 0
    h_curr, k_curr = 1, 1
    result.append(Fraction(h_curr, k_curr))
    for i in range(1, n):
        a = 1 if i == 1 else 2  # Actually for sqrt(2), a0=1, then all 2
        # More accurately:
        pass
    return result

# Simpler: use Fraction.limit_denominator
for max_den in [10, 100, 1000, 10000, 100000]:
    approx = Fraction(math.sqrt(2)).limit_denominator(max_den)
    error = abs(math.sqrt(2) - float(approx))
    print(f"sqrt(2) ≈ {approx} (error {error:.2e})")
# sqrt(2) ≈ 17/12 (error 2.45e-04)
# sqrt(2) ≈ 1393/985 (error 8.20e-08)
# sqrt(2) ≈ 1393/985 (error 8.20e-08)
# sqrt(2) ≈ 21371/15125 (error 7.74e-11)
# sqrt(2) ≈ 114243/80782 (error 6.96e-12)
```

### Computing pi to Arbitrary Precision

Several families of formulas can compute $\pi$ to millions of digits.

### Machin-like Formulas

Machin's formula (1706) uses the arctangent identity:

$$
\frac{\pi}{4} = 4 \arctan\left(\frac{1}{5}\right) - \arctan\left(\frac{1}{239}\right)
$$

Combined with the Taylor series for $\arctan$:

$$
\arctan(x) = x - \frac{x^3}{3} + \frac{x^5}{5} - \frac{x^7}{7} + \cdots
$$

```python
def arctan_taylor(x: float, terms: int) -> float:
    """Approximate arctan(x) using Taylor series."""
    result = 0.0
    x_pow = x
    for n in range(terms):
        term = x_pow / (2 * n + 1)
        if n % 2 == 0:
            result += term
        else:
            result -= term
        x_pow *= x * x
    return result

def pi_machin(terms: int) -> float:
    return 4 * (4 * arctan_taylor(1/5, terms) - arctan_taylor(1/239, terms))

for t in [5, 10, 20, 50]:
    p = pi_machin(t)
    error = abs(math.pi - p)
    print(f"Machin({t} terms): pi ≈ {p:.15f}, error {error:.2e}")
# Machin(50 terms): pi ≈ 3.141592653589794, error ~ 1.78e-15
```

### The Chudnovsky Algorithm

The Chudnovsky brothers' algorithm (1989) is the fastest known method for computing $\pi$ to arbitrary precision. It is based on a Ramanujan-inspired rapidly converging series:

$$
\frac{1}{\pi} = 12 \sum_{k=0}^\infty \frac{(-1)^k (6k)! (13591409 + 545140134k)}{(3k)! (k!)^3 640320^{3k + 3/2}}
$$

Each term adds approximately 14 digits of $\pi$. This algorithm is used by most modern $\pi$-computing record holders.

```python
import decimal
from decimal import Decimal, getcontext

def pi_chudnovsky(precision: int) -> Decimal:
    """Compute pi using the Chudnovsky algorithm to `precision` decimal places."""
    getcontext().prec = precision + 10
    
    C = 426880 * Decimal(10005).sqrt()
    K = Decimal(6)
    M = Decimal(1)
    X = Decimal(1)
    L = Decimal(13591409)
    S = Decimal(13591409)
    
    k = 1
    while True:
        M = M * (K**3 - 16*K) // (k**3)
        X *= Decimal(-262537412640768000)
        L += Decimal(545140134)
        
        term = M * L / X
        S += term
        
        if abs(term) < Decimal(10) ** (-precision - 10):
            break
        
        K += 12
        k += 1
    
    return C / S

# Compute pi to 50 decimal places
pi_50 = pi_chudnovsky(50)
print(f"pi (50 digits): {pi_50}")
# pi (50 digits): 3.14159265358979323846264338327950288419716939937510
```

### Symbolic Computation vs Numeric Approximation

Symbolic computation treats irrational numbers as exact mathematical objects, manipulating them algebraically without approximation:

```python
import sympy as sp

# Symbolic: exact representation
x = sp.sqrt(2)
y = sp.pi
expr = sp.sin(sp.pi / 4) + sp.sqrt(2)
print(sp.simplify(expr))  # 2*sqrt(2)

# Numeric: floating-point approximation
print(sp.N(sp.pi, 50))    # 3.1415926535897932384626433832795028841971693993751

# Mixed: substitute rational approximations
# This demonstrates the difference between symbolic and numeric
theta = sp.Symbol('theta')
identity = sp.sin(theta)**2 + sp.cos(theta)**2
print(identity)                                                           # sin(theta)^2 + cos(theta)^2
print(sp.simplify(identity.subs(theta, sp.pi/4)))                        # 1
print(sp.N(identity.subs(theta, math.pi/4)))                             # 1.000000000000000
```

The choice between symbolic and numeric approaches depends on the use case:

| Aspect | Symbolic | Numeric |
|--------|----------|---------|
| Representation | Exact (e.g., $\sqrt{2}$) | Approximate (e.g., 1.41421...) |
| Operations | Algebraic manipulation | Fast arithmetic |
| Equality | Decidable | Tolerance-based |
| Use cases | CAS, proofs, exact geometry | 3D graphics, physics sims, ML |
| Libraries | SymPy, Mathematica | NumPy, BLAS, GPU compute |

## Glossary

| Term | Definition |
|------|------------|
| Algebraic number | A number that is a root of a polynomial with integer coefficients |
| Cantor diagonal argument | A proof that $\mathbb{R}$ is uncountable by constructing a number not in any proposed enumeration |
| Catastrophic cancellation | Loss of significant digits when subtracting nearly equal floating-point numbers |
| Continued fraction | An expression of the form $a_0 + 1/(a_1 + 1/(a_2 + \dots))$ |
| Convergent | A rational approximation obtained by truncating a continued fraction |
| Countable set | A set that can be put into bijection with $\mathbb{N}$ |
| Irrational number | A real number that cannot be expressed as $p/q$ with $p, q \in \mathbb{Z}$ |
| Irrationality measure | The supremum of $\mu$ such that $|x - p/q| < 1/q^\mu$ has infinitely many solutions |
| Liouville number | A transcendental number that is extremely well-approximable by rationals |
| Normal number | A number whose decimal digits are uniformly distributed (conjectured for $\pi$) |
| Newton's method | Iterative root-finding algorithm $x_{n+1} = x_n - f(x_n)/f'(x_n)$ |
| Proof by contradiction | Assuming the negation of a statement and deriving a contradiction |
| Roth's theorem | Every algebraic irrational has irrationality measure $\mu \le 2$ |
| Transcendental number | A real number that is not algebraic — satisfies no polynomial equation with integer coefficients |
| Uncountable set | A set too large to be put into bijection with $\mathbb{N}$ |

## Quick References

- [Proof that pi is Irrational (Niven)](https://projecteuclid.org/journals/bulletin-of-the-american-mathematical-society/volume-53/issue-6/A-simple-proof-that-pi-is-irrational/bams/1183510788.full) — Niven's 1947 one-page proof
- [The Chudnovsky Algorithm for pi](https://en.wikipedia.org/wiki/Chudnovsky_algorithm) — Fastest known algorithm for computing $\pi$
- [Pi Digits](https://www.piday.org/million/) — First million digits of $\pi$
- [Liouville Numbers](https://mathworld.wolfram.com/LiouvilleNumber.html) — MathWorld entry on the first explicitly known transcendentals
- [Roth's Theorem](https://en.wikipedia.org/wiki/Roth%27s_theorem) — Rational approximation of algebraic numbers
- [SymPy Documentation](https://docs.sympy.org/) — Python library for symbolic mathematics

## Next Steps

- [Real Numbers](real-numbers.md) — the union of rationals and irrationals forming the complete ordered field
- [Floating-Point Numbers](floating-point-numbers.md) — how computers approximate real numbers with finite storage
- [Complex Numbers](complex-numbers.md) — extending the real numbers to include $i = \sqrt{-1}$
