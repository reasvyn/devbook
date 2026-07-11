# Rational Numbers

## Description

Rational numbers are the bridge between integer arithmetic and the full continuum of real numbers. Every fraction, every terminating or repeating decimal, and every ratio that appears in code — from scaling factors in graphics to probability calculations — is a rational number. Understanding rationals is essential for avoiding floating-point pitfalls, implementing exact arithmetic, and reasoning about numeric algorithms.

## Prerequisites

- [Integers](integers.md) — division, GCD, and integer arithmetic used to define and operate on rational numbers

## Table of Contents

- [Definition and Set Notation](#definition-and-set-notation)
- [Fractions: Numerator and Denominator](#fractions-numerator-and-denominator)
- [Simplification and GCD Reduction](#simplification-and-gcd-reduction)
- [Equality of Rational Numbers](#equality-of-rational-numbers)
- [Addition and Subtraction](#addition-and-subtraction)
- [Multiplication](#multiplication)
- [Division](#division)
- [Decimal Representation](#decimal-representation)
- [Terminating Decimals](#terminating-decimals)
- [Repeating Decimals](#repeating-decimals)
- [Converting Repeating Decimals to Fractions](#converting-repeating-decimals-to-fractions)
- [Density of Q](#density-of-q)
- [Countability: Q is Countably Infinite](#countability-q-is-countably-infinite)
- [Cantors Diagonal Enumeration](#cantors-diagonal-enumeration)
- [Ordering and Comparison](#ordering-and-comparison)
- [Continued Fractions for Rationals](#continued-fractions-for-rationals)
- [Rational Numbers in Computing](#rational-numbers-in-computing)
- [Exact Rational Representation](#exact-rational-representation)
- [Fractions vs Floating-Point](#fractions-vs-floating-point)
- [Rational Arithmetic Libraries](#rational-arithmetic-libraries)
- [Python fractions.Fraction](#python-fractionsfraction)
- [Rust num_rational](#rust-numrational)
- [Applications in Programming](#applications-in-programming)
- [DDA Line Algorithm](#dda-line-algorithm)
- [Computer Algebra Systems](#computer-algebra-systems)
- [Currency and Financial Calculations](#currency-and-financial-calculations)
- [Probability and Combinatorics](#probability-and-combinatorics)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Definition and Set Notation

The set of rational numbers, denoted by $\mathbb{Q}$, is defined as:

$$
\mathbb{Q} = \left\{ \frac{p}{q} \mid p, q \in \mathbb{Z}, q \neq 0 \right\}
$$

Here $p$ is called the **numerator** and $q$ the **denominator**. The denominator cannot be zero because division by zero is undefined in the real numbers. Every integer $n$ is also a rational number because it can be written as $n/1$.

The name "rational" comes from the Latin *ratio* — a rational number represents a ratio of two integers. The set $\mathbb{Q}$ sits strictly between $\mathbb{Z}$ (integers) and $\mathbb{R}$ (real numbers):

$$
\mathbb{N} \subset \mathbb{Z} \subset \mathbb{Q} \subset \mathbb{R}
$$

### Fractions: Numerator and Denominator

A fraction $p/q$ consists of:

- **Numerator** ($p$): the integer above the fraction bar, indicating how many parts are taken.
- **Denominator** ($q$): the non-zero integer below the fraction bar, indicating how many equal parts the whole is divided into.

When $|p| < |q|$, the fraction is called **proper**. When $|p| \ge |q|$, it is **improper**. An improper fraction can be rewritten as a **mixed number**: a whole number plus a proper fraction.

```python
from math import gcd

class Fraction:
    def __init__(self, num: int, den: int) -> None:
        if den == 0:
            raise ValueError("Denominator cannot be zero")
        g = gcd(abs(num), abs(den))
        self.num = num // g
        self.den = den // g

    def __repr__(self) -> str:
        return f"{self.num}/{self.den}"

print(Fraction(6, 8))  # 3/4
print(Fraction(-4, 6)) # -2/3
```

### Simplification and GCD Reduction

A fraction is in **lowest terms** (or **simplest form**) when the numerator and denominator share no common positive divisor other than 1 — that is, $\gcd(p, q) = 1$. To reduce a fraction, divide both numerator and denominator by their greatest common divisor:

$$
\frac{p}{q} = \frac{p / \gcd(p, q)}{q / \gcd(p, q)}
$$

For example, $42/56$ reduces to $3/4$ because $\gcd(42, 56) = 14$.

```python
def reduce_fraction(num: int, den: int) -> tuple[int, int]:
    g = gcd(abs(num), abs(den))
    return (num // g, den // g)

print(reduce_fraction(42, 56))  # (3, 4)
print(reduce_fraction(17, 23))  # (17, 23)
```

### Equality of Rational Numbers

Two rational numbers $p/q$ and $r/s$ are equal if and only if the cross products are equal:

$$
\frac{p}{q} = \frac{r}{s} \iff p \cdot s = q \cdot r
$$

This is consistent with the idea that both represent the same point on the number line. For instance, $2/3 = 4/6$ because $2 \cdot 6 = 3 \cdot 4 = 12$.

### Addition and Subtraction

To add or subtract two rational numbers, rewrite them with a common denominator — preferably the least common multiple of the denominators:

$$
\frac{a}{b} + \frac{c}{d} = \frac{a \cdot d + b \cdot c}{b \cdot d}, \qquad
\frac{a}{b} - \frac{c}{d} = \frac{a \cdot d - b \cdot c}{b \cdot d}
$$

After computing the numerator, reduce the result:

```python
from math import lcm

def add_rationals(a: int, b: int, c: int, d: int) -> tuple[int, int]:
    common = lcm(b, d)
    num = a * (common // b) + c * (common // d)
    g = gcd(abs(num), common)
    return (num // g, common // g)

print(add_rationals(1, 3, 1, 6))  # (1, 2)
print(add_rationals(2, 5, 3, 7))  # (29, 35)
```

Using the least common multiple minimizes intermediate values and reduces the need for later simplification.

### Multiplication

Multiplication of rationals is straightforward — multiply numerators and multiply denominators:

$$
\frac{a}{b} \times \frac{c}{d} = \frac{a \cdot c}{b \cdot d}
$$

Cross-cancellation before multiplying reduces the size of intermediate numbers:

```python
def mul_rationals(a: int, b: int, c: int, d: int) -> tuple[int, int]:
    num = a * c
    den = b * d
    g = gcd(abs(num), abs(den))
    return (num // g, den // g)

print(mul_rationals(2, 3, 3, 4))  # (1, 2)
print(mul_rationals(7, 2, 1, 5))  # (7, 10)
```

### Division

Division is multiplication by the reciprocal:

$$
\frac{a}{b} \div \frac{c}{d} = \frac{a}{b} \times \frac{d}{c} = \frac{a \cdot d}{b \cdot c}
$$

The divisor $c/d$ must have $c \neq 0$ (since division by zero is undefined).

```python
def div_rationals(a: int, b: int, c: int, d: int) -> tuple[int, int]:
    if c == 0:
        raise ValueError("Division by zero")
    num = a * d
    den = b * c
    g = gcd(abs(num), abs(den))
    return (num // g, den // g)

print(div_rationals(1, 2, 3, 4))  # (2, 3)
print(div_rationals(5, 7, 2, 3))  # (15, 14)
```

### Decimal Representation

Every rational number has a decimal representation that either terminates or eventually repeats. This is a consequence of the long-division process: when dividing $p$ by $q$, the remainder at each step determines the next digit. Since there are only $q - 1$ possible non-zero remainders, the sequence must eventually repeat or reach zero.

Let the denominator's prime factorization be $q = 2^a \cdot 5^b \cdot m$ where $m$ is coprime to $10$.

- If $m = 1$, the decimal **terminates**.
- If $m > 1$, the decimal **repeats** with a period equal to the multiplicative order of $10$ modulo $m$.

### Terminating Decimals

A rational number has a terminating decimal if and only if, when written in lowest terms, its denominator has only the prime factors $2$ and $5$:

$$
\frac{1}{2} = 0.5, \quad \frac{3}{5} = 0.6, \quad \frac{7}{8} = 0.875, \quad \frac{13}{20} = 0.65
$$

```python
def is_terminating(num: int, den: int) -> bool:
    g = gcd(abs(num), abs(den))
    d = den // g
    while d % 2 == 0:
        d //= 2
    while d % 5 == 0:
        d //= 5
    return d == 1

print(is_terminating(1, 8))    # True
print(is_terminating(1, 3))    # False
print(is_terminating(7, 250))  # True  (250 = 2 * 5^3)
```

### Repeating Decimals

When the denominator has a prime factor other than $2$ or $5$, the decimal repeats. The repeating block is called the **repetend**:

$$
\frac{1}{3} = 0.\overline{3}, \quad
\frac{1}{7} = 0.\overline{142857}, \quad
\frac{1}{6} = 0.1\overline{6}, \quad
\frac{1}{11} = 0.\overline{09}
$$

The **period** is the length of the repetend. For $1/p$ where $p$ is prime and $p \neq 2, 5$, the period divides $p - 1$.

```python
def decimal_repetend(num: int, den: int) -> tuple[str, str, str, int]:
    integer_part = str(num // den)
    rem = num % den
    if rem == 0:
        return (integer_part, "", "", 0)

    digits = []
    states = {}
    idx = 0

    while rem != 0 and rem not in states:
        states[rem] = idx
        rem *= 10
        digits.append(str(rem // den))
        rem %= den
        idx += 1

    if rem == 0:
        return (integer_part, "".join(digits), "", 0)
    else:
        repeat_start = states[rem]
        non_rep = "".join(digits[:repeat_start])
        rep = "".join(digits[repeat_start:])
        return (integer_part, non_rep, rep, len(rep))

ip, non, rep, period = decimal_repetend(1, 7)
print(f"{ip}.{non}[{rep}] period={period}")  # 0.[142857] period=6
```

### Converting Repeating Decimals to Fractions

Every repeating decimal can be converted back to a fraction. For a purely repeating decimal $0.\overline{d_1 d_2 \dots d_n}$:

Let $x = 0.\overline{d_1 d_2 \dots d_n}$. Then $10^n x = d_1 d_2 \dots d_n.\overline{d_1 d_2 \dots d_n}$.

Subtracting: $10^n x - x = d_1 d_2 \dots d_n$, so $x = \frac{d_1 d_2 \dots d_n}{10^n - 1}$.

Examples:

$$
0.\overline{3} = \frac{3}{9} = \frac{1}{3}, \qquad
0.\overline{142857} = \frac{142857}{999999} = \frac{1}{7}
$$

For a mixed repeating decimal $0.a_1\dots a_m\overline{b_1\dots b_n}$:

Let $x = 0.a_1\dots a_m\overline{b_1\dots b_n}$. Multiply by $10^m$ to get $10^m x = a_1\dots a_m.\overline{b_1\dots b_n}$. Let $y = 10^m x$. Then $10^n y = a_1\dots a_m b_1\dots b_n.\overline{b_1\dots b_n}$. Subtracting: $(10^n - 1) y = a_1\dots a_m b_1\dots b_n - a_1\dots a_m$.

Therefore:

$$
x = \frac{a_1\dots a_m b_1\dots b_n - a_1\dots a_m}{10^m (10^n - 1)}
$$

```python
def repeating_to_fraction(integer: int, non_rep: str, rep: str) -> tuple[int, int]:
    m = len(non_rep)
    n = len(rep)
    non_rep_val = int(non_rep) if non_rep else 0
    full_val = int(non_rep + rep) if (non_rep + rep) else 0

    if n == 0:
        num = integer * (10 ** m) + non_rep_val
        den = 10 ** m
    else:
        num = (integer * (10 ** (m + n)) + full_val) - (integer * (10 ** m) + non_rep_val)
        den = (10 ** (m + n)) - (10 ** m)

    g = gcd(abs(num), den)
    return (num // g, den // g)

print(repeating_to_fraction(0, "", "3"))       # (1, 3)
print(repeating_to_fraction(0, "", "142857"))  # (1, 7)
print(repeating_to_fraction(0, "1", "6"))      # (1, 6)
```

### Density of Q

The rational numbers are **dense** in the real numbers: between any two distinct real numbers $a < b$, there exists a rational number. One constructive proof: let $n$ be an integer such that $n > 1/(b - a)$. Then the fractions $m/n$ for integer $m$ partition the real line into steps of size $1/n < b - a$, so at least one falls strictly between $a$ and $b$.

```python
def rational_between(a: float, b: float) -> tuple[int, int]:
    if a >= b:
        raise ValueError("Require a < b")
    n = int(1.0 / (b - a)) + 1
    m = int(a * n) + 1
    return (m, n)

r = rational_between(0.14159, 0.14160)
print(f"{r[0]}/{r[1]} = {r[0]/r[1]}")
```

Density implies that every real number can be approximated arbitrarily closely by rational numbers — a fact fundamental to numerical methods and approximation theory.

### Countability: Q is Countably Infinite

A set is **countable** if its elements can be put into a one-to-one correspondence with the natural numbers $\mathbb{N}$. Despite being dense in $\mathbb{R}$, $\mathbb{Q}$ is countable — a surprising result first proven by Georg Cantor.

### Cantors Diagonal Enumeration

Cantor's classic argument arranges all positive rationals $p/q$ in an infinite grid and traverses along diagonals, skipping fractions not in lowest terms:

```python
def enumerate_rationals(n: int) -> list[tuple[int, int]]:
    result = []
    seen = set()
    d = 2
    while len(result) < n:
        for row in range(1, d):
            col = d - row
            num, den = row, col
            g = gcd(num, den)
            reduced = (num // g, den // g)
            if reduced not in seen:
                seen.add(reduced)
                result.append(reduced)
                if len(result) >= n:
                    break
        d += 1
    return result[:n]

print(enumerate_rationals(10))
# [(1, 1), (2, 1), (1, 2), (3, 1), (1, 3), (4, 1), (3, 2), (2, 3), (1, 4), (5, 1)]
```

To include negative rationals, interleave positive and negative terms. The existence of this enumeration proves $\mathbb{Q}$ is countably infinite.

### Ordering and Comparison

Given two rationals $a/b$ and $c/d$ (in lowest terms, with positive denominators), compare them by cross-multiplying:

$$
\frac{a}{b} < \frac{c}{d} \iff a \cdot d < b \cdot c
$$

```python
def less_than(a: int, b: int, c: int, d: int) -> bool:
    return a * d < b * c

print(less_than(1, 3, 1, 2))  # True
print(less_than(3, 4, 2, 3))  # False
```

This avoids floating-point conversion entirely — critical for exact comparison in sorting, binary search, and interval arithmetic.

### Continued Fractions for Rationals

Every rational number can be expressed as a finite **simple continued fraction**:

$$
\frac{p}{q} = a_0 + \cfrac{1}{a_1 + \cfrac{1}{a_2 + \cfrac{1}{\ddots + \cfrac{1}{a_n}}}}
$$

where $a_0, a_1, \dots, a_n$ are integers and $a_i > 0$ for $i > 0$. The representation is obtained by repeatedly applying the Euclidean algorithm:

```python
def continued_fraction(num: int, den: int) -> list[int]:
    coeffs = []
    while den != 0:
        a = num // den
        coeffs.append(a)
        num, den = den, num - a * den
    return coeffs

print(continued_fraction(415, 93))  # [4, 2, 6, 7]
print(continued_fraction(3, 4))     # [0, 1, 3]
```

The convergents (truncated continued fractions) give the best rational approximations to a number for a given denominator size.

### Rational Numbers in Computing

Computers represent numbers in two fundamental ways: **floating-point** (approximate, fixed-width) and **rational** (exact, arbitrary-precision). Understanding when to use each is crucial for correctness.

### Exact Rational Representation

A rational number is stored as an ordered pair of integers $(p, q)$ with $q \neq 0$, always kept in lowest terms:

```python
class ExactRational:
    def __init__(self, num: int, den: int) -> None:
        if den == 0:
            raise ZeroDivisionError("Denominator zero")
        if den < 0:
            num = -num
            den = -den
        g = gcd(abs(num), den)
        self.num = num // g
        self.den = den // g

    def __add__(self, other: 'ExactRational') -> 'ExactRational':
        return ExactRational(
            self.num * other.den + other.num * self.den,
            self.den * other.den
        )

    def __mul__(self, other: 'ExactRational') -> 'ExactRational':
        return ExactRational(
            self.num * other.num,
            self.den * other.den
        )

    def __float__(self) -> float:
        return self.num / self.den

    def __repr__(self) -> str:
        return f"{self.num}/{self.den}"

a = ExactRational(1, 3)
b = ExactRational(1, 6)
print(a + b)         # 1/2
print(float(a + b))  # 0.5
```

### Fractions vs Floating-Point

Floating-point numbers (IEEE 754) represent rationals with a fixed number of bits — typically 53 bits of mantissa. This introduces rounding errors for many rationals:

```python
print(0.1 + 0.2)                      # 0.30000000000000004
print(0.1 + 0.2 == 0.3)               # False

from fractions import Fraction
a = Fraction(1, 10)
b = Fraction(2, 10)
print(a + b == Fraction(3, 10))       # True
```

The trade-offs are:

| Aspect | Rational | Floating-Point |
|--------|----------|---------------|
| Precision | Exact | Approximate |
| Performance | Slower (GCD on every op) | Hardware-accelerated |
| Memory | Grows with numerator/denominator | Fixed 32/64 bits |
| Overflow | Gradual (arbitrary integers) | Infinity after exponent overflow |
| Comparison | Exact equality safe | Use epsilon tolerances |

### Rational Arithmetic Libraries

Most modern languages provide rational number libraries. Here are two prominent examples.

### Python fractions.Fraction

Python's standard library `fractions.Fraction` is an immutable rational type backed by arbitrary-precision integers:

```python
from fractions import Fraction
from decimal import Decimal
import math

a = Fraction(3, 4)
b = Fraction('0.75')
c = Fraction(Decimal('0.75'))
print(a == b == c)  # True

x = Fraction(1, 3)
y = Fraction(2, 5)
print(x + y)  # 11/15
print(x * y)  # 2/15

# Limit denominator for best rational approximation
approx = Fraction(math.pi).limit_denominator(100)
print(approx)  # 311/399
```

Key features:
- Automatic reduction to lowest terms
- Arithmetic with int, float, Decimal, and other Fractions
- `limit_denominator()` for best rational approximation
- Hashable (usable in sets and as dict keys)

### Rust num_rational

The `num` crate provides `Ratio` (arbitrary precision) and `Rational32`/`Rational64` (fixed-size) types:

```rust
use num_rational::Ratio;
use num_bigint::BigInt;

let r = Ratio::new(BigInt::from(1), BigInt::from(3));
let s = Ratio::new(BigInt::from(2), BigInt::from(5));
let sum = r + s;
println!("{}", sum);  // 11/15

// Fixed-size 64-bit
use num_rational::Rational64;
let a = Rational64::new(1, 3);
let b = Rational64::new(2, 5);
println!("{}", a * b);  // 2/15
```

### Applications in Programming

Rational numbers appear in many software domains where exactness is required.

### DDA Line Algorithm

The Digital Differential Analyzer (DDA) line-drawing algorithm uses rational slopes to rasterize lines without floating-point drift:

```python
from fractions import Fraction

def dda_line(x0: int, y0: int, x1: int, y1: int) -> list[tuple[int, int]]:
    dx = x1 - x0
    dy = y1 - y0
    steps = max(abs(dx), abs(dy))
    x_inc = Fraction(dx, steps)
    y_inc = Fraction(dy, steps)
    x = Fraction(x0)
    y = Fraction(y0)
    pixels = []
    for _ in range(steps + 1):
        pixels.append((round(x), round(y)))
        x += x_inc
        y += y_inc
    return pixels

print(dda_line(0, 0, 5, 3))
# [(0, 0), (1, 1), (2, 1), (3, 2), (4, 2), (5, 3)]
```

### Computer Algebra Systems

CAS tools like SymPy represent all numbers as exact rationals unless the user requests approximation:

```python
import sympy as sp

x = sp.Rational(1, 3)
y = sp.Rational(2, 5)
print(x + y)  # 11/15

# Symbolic manipulation with rational coefficients
from sympy import symbols, expand
t = symbols('t')
expr = (sp.Rational(1, 3) * t + sp.Rational(1, 2)) ** 2
print(expand(expr))  # t**2/9 + t/3 + 1/4
```

### Currency and Financial Calculations

Financial applications require exact rational arithmetic because rounding errors in monetary calculations are unacceptable:

```python
from fractions import Fraction

price = Fraction(1999, 100)       # $19.99
tax_rate = Fraction(8, 100)       # 8%
total = price + price * tax_rate
print(total)           # 53973/2500 = 21.5892

# Round to nearest cent
cents = Fraction(1, 100)
rounded_total = Fraction(round(total * 100), 100)
print(rounded_total)   # 1079/50 = 21.58
```

Using floating-point for currency leads to known bugs (the $0.01 rounding error that accumulated in the Vancouver Stock Exchange index, for example). Rational arithmetic avoids these issues.

### Probability and Combinatorics

Probabilities are rational numbers — ratios of favorable outcomes to total outcomes:

```python
from fractions import Fraction
from math import comb

# Probability of rolling a 7 with two dice
p = Fraction(6, 36)   # (1,6), (2,5), (3,4), (4,3), (5,2), (6,1)
print(p)              # 1/6

def binomial_prob(n: int, k: int, p_success: Fraction) -> Fraction:
    return Fraction(comb(n, k)) * (p_success ** k) * ((Fraction(1) - p_success) ** (n - k))

prob = binomial_prob(10, 3, Fraction(1, 6))
print(prob)   # 78125/10077696
```

## Glossary

| Term | Definition |
|------|------------|
| Denominator | The integer $q$ in a fraction $p/q$, indicating the number of equal parts |
| Egyptian fraction | A sum of distinct unit fractions that equals a given rational |
| Farey sequence | The ordered sequence of reduced fractions between 0 and 1 with denominator at most $n$ |
| Fraction | A number expressed as $p/q$ where $p, q \in \mathbb{Z}$ and $q \neq 0$ |
| GCD | Greatest Common Divisor; the largest integer dividing two given integers |
| Improper fraction | A fraction where $|p| \ge |q|$ |
| Lowest terms | A fraction $p/q$ where $\gcd(p,q) = 1$ |
| Mixed number | A whole number plus a proper fraction |
| Numerator | The integer $p$ in a fraction $p/q$, indicating how many parts are taken |
| Period | The length of the repeating block in a decimal representation |
| Proper fraction | A fraction where $|p| < |q|$ |
| Rational number | A number that can be expressed as the ratio of two integers |
| Reduced fraction | A fraction in lowest terms |
| Repetend | The repeating digit block in a recurring decimal |
| Simple continued fraction | A fraction of the form $a_0 + 1/(a_1 + 1/(a_2 + \dots))$ |
| Terminating decimal | A decimal representation that ends after a finite number of digits |
| Unit fraction | A fraction with numerator 1 |

## Quick References

- [fractions — Rational numbers](https://docs.python.org/3/library/fractions.html) — Python standard library documentation for `Fraction`
- [num_rational crate](https://docs.rs/num-rational/) — Rust crate for rational number arithmetic
- [What Every Computer Scientist Should Know About Floating-Point Arithmetic](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html) — David Goldberg's paper on floating-point pitfalls
- [Continued Fractions](http://www.math.cornell.edu/~henderson/courses/Mech/2002/ContinuedFractions.pdf) — Cornell lecture notes
- [Farey Sequences and the Stern-Brocot Tree](https://www.cut-the-knot.org/blue/Stern.shtml) — Interactive exploration

## Next Steps

- [Irrational Numbers](irrational-numbers.md) — numbers that cannot be expressed as fractions, completing the gap between rationals and reals
- [Real Numbers](real-numbers.md) — the union of rational and irrational numbers forming the continuum
- [Floating-Point Numbers](floating-point-numbers.md) — how computers approximate real numbers in hardware
