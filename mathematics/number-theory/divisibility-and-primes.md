# Divisibility and Primes

## Description

Divisibility and prime numbers form the bedrock of number theory. Every cryptographic algorithm, hash function, and primality test rests on understanding when one integer divides another and how primes compose every natural number uniquely. This document covers the division algorithm, the fundamental theorem of arithmetic, the Euclidean algorithm for greatest common divisors, and Bezout's identity -- tools that appear directly in modular inverse computation, RSA key generation, and fraction reduction in code.

## Prerequisites

- [Mathematical Thinking](../intro/mathematical-thinking.md) -- the foundations of proof, induction, and logical reasoning used throughout this document

## Table of Contents

- [The Division Algorithm](#the-division-algorithm)
- [Divisibility and Notation](#divisibility-and-notation)
- [Properties of Divisibility](#properties-of-divisibility)
- [Prime Numbers](#prime-numbers)
- [The Sieve of Eratosthenes](#the-sieve-of-eratosthenes)
- [The Fundamental Theorem of Arithmetic](#the-fundamental-theorem-of-arithmetic)
- [Greatest Common Divisor](#greatest-common-divisor)
- [The Euclidean Algorithm](#the-euclidean-algorithm)
- [The Extended Euclidean Algorithm](#the-extended-euclidean-algorithm)
- [Bezout's Identity](#bezouts-identity)
- [Relatively Prime Numbers](#relatively-prime-numbers)
- [Least Common Multiple](#least-common-multiple)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Division Algorithm

Given two integers $$a$$ (dividend) and $$b$$ (divisor) with $$b > 0$$, there exist unique integers $$q$$ (quotient) and $$r$$ (remainder) such that:

$$
a = b \cdot q + r, \quad 0 \le r < b
$$

This is the division algorithm. It does not describe an algorithm in the programming sense; it is a theorem guaranteeing the existence of a quotient and remainder. Every programming language implements it via the division and modulo operators:

```python
def divide(a: int, b: int) -> tuple[int, int]:
    """Return (quotient, remainder) for a divided by b."""
    q = a // b
    r = a % b
    return (q, r)

print(divide(47, 5))  # (9, 2) because 47 = 5*9 + 2
```

In mathematics, both the quotient and remainder are defined for positive divisors. In programming, negative numbers cause complications (see the Modular Arithmetic document for details on handling negatives).

### Divisibility and Notation

If $$b$$ divides $$a$$ with remainder zero, we say $$b$$ is a divisor of $$a$$ and write:

$$
b \mid a \quad \text{iff} \quad \exists k \in \mathbb{Z}: a = b \cdot k
$$

If $$b$$ does not divide $$a$$, we write $$b \nmid a$$.

Examples:

- $$3 \mid 12$$ because $$12 = 3 \cdot 4$$
- $$5 \mid 0$$ because $$0 = 5 \cdot 0$$
- $$7 \nmid 20$$ because $$20 = 7 \cdot 2 + 6$$

In programming, checking divisibility uses the modulo operator:

```python
def is_divisible(a: int, b: int) -> bool:
    """Return True if b divides a evenly."""
    return a % b == 0

print(is_divisible(12, 3))  # True
print(is_divisible(20, 7))  # False
```

### Properties of Divisibility

Let $$a, b, c$$ be integers. The following properties hold:

| Property | Statement |
|----------|-----------|
| Reflexivity | $$a \mid a$$ |
| Transitivity | If $$a \mid b$$ and $$b \mid c$$, then $$a \mid c$$ |
| Linearity | If $$a \mid b$$ and $$a \mid c$$, then $$a \mid (bx + cy)$$ for any integers $$x, y$$ |
| Multiplication | If $$a \mid b$$, then $$ac \mid bc$$ |
| Cancellation | If $$ac \mid bc$$ and $$c \neq 0$$, then $$a \mid b$$ |

**Proof of linearity (for demonstration):**

If $$a \mid b$$, then $$b = a k_1$$ for some integer $$k_1$$. If $$a \mid c$$, then $$c = a k_2$$ for some integer $$k_2$$. Then:

$$
b x + c y = a k_1 x + a k_2 y = a (k_1 x + k_2 y)
$$

so $$a \mid (b x + c y)$$. This property is used extensively in the extended Euclidean algorithm.

### Prime Numbers

A prime number is an integer $$p > 1$$ whose only positive divisors are $$1$$ and $$p$$. A composite number is an integer $$n > 1$$ that is not prime.

**First few primes:**

$$
2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97
$$

**Properties of primes for developers:**

- 2 is the only even prime. Every other even number is divisible by 2.
- Primes become sparser as numbers grow. The prime number theorem states that the number of primes less than $$n$$ is approximately $$n / \ln(n)$$.
- Testing whether a number is prime (primality testing) is a key computational problem. See the Primality Testing document for algorithms.
- Generating large primes is essential for RSA key generation. Typically, 2048-bit or 4096-bit primes are used.

**Trial division primality test:**

```python
import math

def is_prime_trial(n: int) -> bool:
    """Test primality by trial division up to sqrt(n)."""
    if n < 2:
        return False
    if n == 2:
        return True
    if n % 2 == 0:
        return False
    for i in range(3, int(math.isqrt(n)) + 1, 2):
        if n % i == 0:
            return False
    return True

print(is_prime_trial(97))   # True
print(is_prime_trial(100))  # False
```

Trial division works well for small numbers (up to $$10^{12}$$) but is far too slow for RSA-sized primes. Probabilistic tests like Miller-Rabin are used instead.

### The Sieve of Eratosthenes

The Sieve of Eratosthenes generates all primes up to a given limit $$n$$ in $$O(n \log \log n)$$ time.

**Algorithm:**

1. Create a boolean array `is_prime[0..n]` initialized to `True`.
2. Set `is_prime[0] = is_prime[1] = False`.
3. For each $$p$$ from 2 to $$\sqrt{n}$$, if `is_prime[p]` is True, mark all multiples of $$p$$ as False.
4. Return the indices that are still True.

```python
def sieve_of_eratosthenes(n: int) -> list[int]:
    """Return all primes up to n."""
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False
    for p in range(2, int(n ** 0.5) + 1):
        if is_prime[p]:
            for multiple in range(p * p, n + 1, p):
                is_prime[multiple] = False
    return [i for i, prime in enumerate(is_prime) if prime]

print(sieve_of_eratosthenes(50))
# [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47]
```

**Optimizations:**

- Start marking from $$p^2$$ instead of $$2p$$, since all smaller multiples are already marked by smaller primes.
- Use a segmented sieve when $$n$$ is too large for memory (e.g., $$n > 10^7$$).

```python
def segmented_sieve(n: int) -> list[int]:
    """Segmented sieve for large n."""
    limit = int(n ** 0.5) + 1
    small_primes = sieve_of_eratosthenes(limit)
    primes = list(small_primes)
    segment_size = limit
    low = limit
    high = min(low + segment_size, n + 1)
    while low < n:
        is_prime = [True] * (high - low)
        for p in small_primes:
            start = max(p * p, ((low + p - 1) // p) * p)
            for j in range(start, high, p):
                is_prime[j - low] = False
        primes.extend(i + low for i, v in enumerate(is_prime) if v)
        low += segment_size
        high = min(low + segment_size, n + 1)
    return primes
```

### The Fundamental Theorem of Arithmetic

Every integer $$n > 1$$ can be expressed uniquely as a product of primes, up to the order of the factors:

$$
n = p_1^{e_1} \cdot p_2^{e_2} \cdot p_3^{e_3} \cdots p_k^{e_k}
$$

where each $$p_i$$ is prime and each $$e_i$$ is a positive integer.

**Examples:**

- $$84 = 2^2 \cdot 3 \cdot 7$$
- $$100 = 2^2 \cdot 5^2$$
- $$97 = 97^1$$ (since 97 is prime)

**Prime factorization in code:**

```python
def prime_factorization(n: int) -> dict[int, int]:
    """Return a dict mapping prime factors to their exponents."""
    factors = {}
    d = 2
    while d * d <= n:
        while n % d == 0:
            factors[d] = factors.get(d, 0) + 1
            n //= d
        d += 1 if d == 2 else 2  # after 2, check only odd numbers
    if n > 1:
        factors[n] = 1
    return factors

print(prime_factorization(84))   # {2: 2, 3: 1, 7: 1}
print(prime_factorization(100))  # {2: 2, 5: 2}
```

**Significance:** The fundamental theorem tells us that primes are the building blocks of all integers. This is why primality is critical for cryptography -- factoring a large composite number into its prime factors is computationally hard, which underpins the security of RSA.

### Greatest Common Divisor

The greatest common divisor of two integers $$a$$ and $$b$$, written $$\gcd(a, b)$$, is the largest positive integer that divides both $$a$$ and $$b$$.

**Properties:**

- $$\gcd(a, b) = \gcd(b, a)$$
- $$\gcd(a, 0) = |a|$$
- $$\gcd(a, b) = \gcd(a - b, b)$$ (key insight behind the Euclidean algorithm)
- $$\gcd(a, b) \cdot \operatorname{lcm}(a, b) = |a \cdot b|$$

**Computing GCD from prime factorization:**

If $$a = \prod p_i^{a_i}$$ and $$b = \prod p_i^{b_i}$$, then:

$$
\gcd(a, b) = \prod p_i^{\min(a_i, b_i)}
$$

```python
def gcd_from_factors(a: int, b: int) -> int:
    """Compute GCD using prime factorization (inefficient for large numbers)."""
    fa = prime_factorization(a)
    fb = prime_factorization(b)
    result = 1
    for p, exp_a in fa.items():
        exp_b = fb.get(p, 0)
        result *= p ** min(exp_a, exp_b)
    return result

print(gcd_from_factors(84, 100))  # 4
```

### The Euclidean Algorithm

The Euclidean algorithm computes $$\gcd(a, b)$$ efficiently without factorization. It uses the key observation:

$$
\gcd(a, b) = \gcd(b, a \bmod b)
$$

**Algorithm (iterative):**

```python
def gcd_euclidean(a: int, b: int) -> int:
    """Compute GCD using the Euclidean algorithm."""
    while b != 0:
        a, b = b, a % b
    return abs(a)

print(gcd_euclidean(84, 100))  # 4
print(gcd_euclidean(1071, 462))  # 21
```

**Step-by-step trace for $$\gcd(1071, 462)$$:**

| Step | a | b | a mod b |
|------|---|---|---------|
| 1 | 1071 | 462 | 147 |
| 2 | 462 | 147 | 21 |
| 3 | 147 | 21 | 0 |
| 4 | 21 | 0 | done |

Result: $$\gcd(1071, 462) = 21$$

**Recursive implementation:**

```python
def gcd_recursive(a: int, b: int) -> int:
    """Recursive Euclidean algorithm."""
    return abs(a) if b == 0 else gcd_recursive(b, a % b)

print(gcd_recursive(1071, 462))  # 21
```

The Euclidean algorithm runs in $$O(\log \min(a, b))$$ time, making it extremely efficient even for 4096-bit numbers.

### The Extended Euclidean Algorithm

The extended Euclidean algorithm not only computes $$\gcd(a, b)$$ but also finds integers $$x$$ and $$y$$ such that:

$$
a \cdot x + b \cdot y = \gcd(a, b)
$$

This is called Bezout's identity. The coefficients $$x$$ and $$y$$ are called Bezout coefficients.

```python
def extended_gcd(a: int, b: int) -> tuple[int, int, int]:
    """Return (gcd, x, y) such that a*x + b*y = gcd."""
    if b == 0:
        return (abs(a), 1 if a > 0 else -1, 0)
    gcd, x1, y1 = extended_gcd(b, a % b)
    x = y1
    y = x1 - (a // b) * y1
    return (gcd, x, y)

# Example: find x, y for 1071*x + 462*y = gcd(1071, 462)
gcd, x, y = extended_gcd(1071, 462)
print(f"gcd = {gcd}, x = {x}, y = {y}")  # gcd = 21, x = -7, y = 13
print(f"Verification: 1071*{x} + 462*{y} = {1071*x + 462*y}")  # 21
```

**Iterative version:**

```python
def extended_gcd_iterative(a: int, b: int) -> tuple[int, int, int]:
    """Iterative extended Euclidean algorithm."""
    old_r, r = a, b
    old_s, s = 1, 0
    old_t, t = 0, 1
    while r != 0:
        quotient = old_r // r
        old_r, r = r, old_r - quotient * r
        old_s, s = s, old_s - quotient * s
        old_t, t = t, old_t - quotient * t
    return (abs(old_r), old_s, old_t)

gcd, x, y = extended_gcd_iterative(1071, 462)
print(f"gcd = {gcd}, x = {x}, y = {y}")  # gcd = 21, x = -7, y = 13
```

### Bezout's Identity

Bezout's identity states that for any integers $$a$$ and $$b$$, there exist integers $$x$$ and $$y$$ such that:

$$
a \cdot x + b \cdot y = \gcd(a, b)
$$

**Key consequences:**

| Application | Description |
|-------------|-------------|
| Modular inverses | If $$\gcd(a, m) = 1$$, then $$a \cdot x \equiv 1 \pmod{m}$$ where $$x$$ is the Bezout coefficient |
| Solving linear Diophantine equations | $$ax + by = c$$ has solutions iff $$\gcd(a, b) \mid c$$ |
| Fraction reduction | To reduce $$\frac{a}{b}$$, divide numerator and denominator by $$\gcd(a, b)$$ |

**Solving linear Diophantine equations:**

Given $$ax + by = c$$, solutions exist iff $$\gcd(a, b) \mid c$$. If $$(x_0, y_0)$$ is one solution, the general solution is:

$$
x = x_0 + \frac{b}{\gcd(a,b)} \cdot t, \quad y = y_0 - \frac{a}{\gcd(a,b)} \cdot t
$$

```python
def solve_diophantine(a: int, b: int, c: int) -> tuple[int, int] | None:
    """Return a particular solution (x, y) to ax + by = c, or None."""
    g, x0, y0 = extended_gcd(a, b)
    if c % g != 0:
        return None
    return (x0 * (c // g), y0 * (c // g))

sol = solve_diophantine(1071, 462, 63)
print(sol)  # (-21, 39) because 1071*(-21) + 462*39 = 63
```

### Relatively Prime Numbers

Two integers $$a$$ and $$b$$ are relatively prime (or coprime) if $$\gcd(a, b) = 1$$.

**Examples:**

- 14 and 15 are relatively prime (gcd = 1), even though neither is prime.
- 14 and 21 are not relatively prime (gcd = 7).

In programming, checking coprimality is straightforward:

```python
def are_coprime(a: int, b: int) -> bool:
    """Return True if a and b are coprime."""
    return gcd_euclidean(a, b) == 1

print(are_coprime(14, 15))  # True
print(are_coprime(14, 21))  # False
```

**Importance in cryptography:** RSA requires selecting two large primes $$p$$ and $$q$$ that are (obviously) coprime to each other. The public exponent $$e$$ must be coprime to $$\phi(n) = (p-1)(q-1)$$.

### Least Common Multiple

The least common multiple of $$a$$ and $$b$$, written $$\operatorname{lcm}(a, b)$$, is the smallest positive integer that is a multiple of both $$a$$ and $$b$$.

**Relationship with GCD:**

$$
\operatorname{lcm}(a, b) = \frac{|a \cdot b|}{\gcd(a, b)}
$$

```python
def lcm(a: int, b: int) -> int:
    """Compute the least common multiple."""
    return abs(a * b) // gcd_euclidean(a, b)

print(lcm(12, 18))  # 36
```

**Applications:**

- Adding fractions: $$\frac{a}{b} + \frac{c}{d} = \frac{a \cdot d + c \cdot b}{\operatorname{lcm}(b, d)}$$ after simplification
- Scheduling problems: when two periodic events align
- Cyclic array indices in programming

## Study Cases

### Case 1: Fraction Reduction

Reduce the fraction $$\frac{84}{108}$$ to its lowest terms.

```python
def reduce_fraction(num: int, den: int) -> tuple[int, int]:
    """Reduce a fraction to lowest terms."""
    g = gcd_euclidean(num, den)
    return (num // g, den // g)

print(reduce_fraction(84, 108))  # (7, 9)
```

Since $$\gcd(84, 108) = 12$$, dividing both numerator and denominator by 12 gives $$\frac{7}{9}$$.

### Case 2: Computing a Modular Inverse

Find the modular inverse of 17 modulo 43. That is, find $$x$$ such that $$17x \equiv 1 \pmod{43}$$.

```python
def modular_inverse(a: int, m: int) -> int | None:
    """Return the modular inverse of a modulo m, or None if it doesn't exist."""
    g, x, _ = extended_gcd(a, m)
    if g != 1:
        return None  # inverse doesn't exist
    return x % m

inv = modular_inverse(17, 43)
print(inv)  # 38 (since 17 * 38 = 646 = 15*43 + 1)
```

Verification: $$17 \cdot 38 = 646 = 15 \cdot 43 + 1$$, so $$17 \cdot 38 \equiv 1 \pmod{43}$$.

### Case 3: Sieve-Aided Prime Factorization

Factor 999,999 using trial division enhanced by a pre-computed sieve:

```python
def sieve_factorize(n: int) -> dict[int, int]:
    """Factor n using a precomputed sieve of primes up to sqrt(n)."""
    import math
    factors = {}
    primes = sieve_of_eratosthenes(int(math.isqrt(n)) + 1)
    for p in primes:
        while n % p == 0:
            factors[p] = factors.get(p, 0) + 1
            n //= p
        if n == 1:
            break
    if n > 1:
        factors[n] = 1
    return factors

print(sieve_factorize(999999))  # {3: 3, 7: 1, 11: 1, 13: 1, 37: 1}
```

### Case 4: RSA Key Pair Setup

Select two primes $$p = 61$$ and $$q = 53$$. Compute $$n = p \cdot q = 3233$$ and $$\phi(n) = (p-1)(q-1) = 3120$$. Choose $$e = 17$$ (must be coprime to 3120). Compute the private key $$d$$ such that $$e \cdot d \equiv 1 \pmod{\phi(n)}$$:

```python
p, q = 61, 53
n = p * q
phi = (p - 1) * (q - 1)
e = 17
g, d, _ = extended_gcd(e, phi)
if g == 1:
    d = d % phi
    print(f"Public key: (n={n}, e={e})")
    print(f"Private key: (n={n}, d={d})")
```

This yields $$d = 2753$$, since $$17 \cdot 2753 = 46801 = 15 \cdot 3120 + 1$$.

## Examples

### Example 1: Finding All Divisors

```python
def all_divisors(n: int) -> list[int]:
    """Return all positive divisors of n in sorted order."""
    divisors = []
    for i in range(1, int(n ** 0.5) + 1):
        if n % i == 0:
            divisors.append(i)
            if i != n // i:
                divisors.append(n // i)
    return sorted(divisors)

print(all_divisors(36))  # [1, 2, 3, 4, 6, 9, 12, 18, 36]
```

### Example 2: Checking Coprimality Across a Set

```python
def pairwise_coprime(numbers: list[int]) -> bool:
    """Check if all numbers in a list are pairwise coprime."""
    for i in range(len(numbers)):
        for j in range(i + 1, len(numbers)):
            if gcd_euclidean(numbers[i], numbers[j]) != 1:
                return False
    return True

print(pairwise_coprime([14, 15, 77]))  # True
print(pairwise_coprime([14, 15, 21]))  # False (21 and 14 share factor 7)
```

### Example 3: GCD of Multiple Numbers

```python
def gcd_many(numbers: list[int]) -> int:
    """Compute GCD of a list of numbers."""
    result = 0
    for n in numbers:
        result = gcd_euclidean(result, n)
    return result

print(gcd_many([48, 72, 108]))  # 12
```

### Example 4: Generating Primes in a Range

```python
def primes_in_range(start: int, end: int) -> list[int]:
    """Return all primes in [start, end]."""
    all_primes = sieve_of_eratosthenes(end)
    return [p for p in all_primes if p >= start]

print(primes_in_range(100, 120))  # [101, 103, 107, 109, 113]
```

### Example 5: Counting Prime Factors

```python
def count_prime_factors(n: int) -> int:
    """Return the number of distinct prime factors of n (omega function)."""
    factors = prime_factorization(n)
    return len(factors)

print(count_prime_factors(84))    # 3 (2, 3, 7)
print(count_prime_factors(1024))  # 1 (only 2)
```

## Glossary

| Term | Definition |
|------|------------|
| Division algorithm | Theorem stating that for any $$a$$ and $$b>0$$, there exist unique $$q$$ and $$r$$ with $$a = bq + r$$ and $$0 \le r < b$$ |
| Divisor | An integer that divides another with remainder zero |
| Prime | An integer $$p > 1$$ with no positive divisors other than 1 and itself |
| Composite | An integer $$n > 1$$ that is not prime |
| Fundamental theorem of arithmetic | Every integer has a unique prime factorization |
| Greatest common divisor (GCD) | The largest positive integer dividing both $$a$$ and $$b$$ |
| Euclidean algorithm | Efficient algorithm for computing GCD using repeated modulo operations |
| Extended Euclidean algorithm | Algorithm that finds GCD along with Bezout coefficients |
| Bezout's identity | The existence of integers $$x, y$$ such that $$ax + by = \gcd(a,b)$$ |
| Bezout coefficients | The integers $$x$$ and $$y$$ in Bezout's identity |
| Coprime (relatively prime) | Two numbers with GCD = 1 |
| Sieve of Eratosthenes | Ancient algorithm for generating all primes up to a limit |
| Least common multiple (LCM) | Smallest positive integer divisible by both $$a$$ and $$b$$ |
| Linear Diophantine equation | Equation of the form $$ax + by = c$$ with integer coefficients |
| Modular inverse | Integer $$x$$ such that $$ax \equiv 1 \pmod{m}$$ |

## Quick References

- [The Prime Pages](https://primes.utm.edu/) -- comprehensive prime number resources and largest known primes
- [Euclidean Algorithm on Wikipedia](https://en.wikipedia.org/wiki/Euclidean_algorithm) -- detailed explanation and variations
- [Number Theory: Structures, Examples, and Problems](https://link.springer.com/book/10.1007/978-0-8176-4645-5) -- Titu Andreescu and Dorin Andrica

## Next Steps

- [Modular Arithmetic](modular-arithmetic.md) -- congruence, modular operations, residue systems
- [Chinese Remainder Theorem](chinese-remainder-theorem.md) -- solving systems of congruences
