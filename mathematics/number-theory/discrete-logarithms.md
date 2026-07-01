# Primitive Roots and Discrete Logarithms

## Description

The discrete logarithm problem (DLP) is one of the most important computational problems in cryptography. While computing a modular exponentiation is easy, finding the exponent given the result is believed to be hard for certain groups. This document covers the order of elements, primitive roots, the discrete logarithm problem, quadratic residues, and their applications in Diffie-Hellman key exchange, ElGamal encryption, and digital signatures.

## Prerequisites

- [Euler & Fermat](euler-fermat.md) -- Euler theorem, totient function, modular exponentiation

## Table of Contents

- [Order of an Element](#order-of-an-element)
- [Properties of Order](#properties-of-order)
- [Primitive Roots](#primitive-roots)
- [Existence of Primitive Roots](#existence-of-primitive-roots)
- [Finding Primitive Roots](#finding-primitive-roots)
- [The Discrete Logarithm Problem](#the-discrete-logarithm-problem)
- [DLP Difficulty](#dlp-difficulty)
- [Baby-Step Giant-Step Algorithm](#baby-step-giant-step-algorithm)
- [Pohlig-Hellman Algorithm](#pohlig-hellman-algorithm)
- [Index Calculus](#index-calculus)
- [Quadratic Residues](#quadratic-residues)
- [Legendre Symbol](#legendre-symbol)
- [Jacobi Symbol](#jacobi-symbol)
- [Quadratic Reciprocity](#quadratic-reciprocity)
- [Applications in Cryptography](#applications-in-cryptography)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Order of an Element

Let G be a finite group (typically Z_n^* under multiplication). The order of an element a in G is the smallest positive integer k such that:

$$
a^k \equiv 1 \pmod{n}
$$

If no such k exists, a has infinite order. In a finite group, every element has finite order.

```python
def multiplicative_order(a: int, n: int) -> int:
    """Return the order of a modulo n, or -1 if a and n are not coprime."""
    from math import gcd
    if gcd(a, n) != 1:
        return -1  # a is not in the group
    for k in range(1, n):
        if pow(a, k, n) == 1:
            return k
    return -1

print(multiplicative_order(2, 7))   # 3  (2^3 = 8 = 1 mod 7)
print(multiplicative_order(3, 7))   # 6  (3^6 = 729 = 1 mod 7)
print(multiplicative_order(2, 13))  # 12 (2^12 = 4096 = 1 mod 13)
print(multiplicative_order(4, 7))   # 3  (4^3 = 64 = 1 mod 7)
```

### Properties of Order

**Lagrange theorem:** The order of any element divides the order of the group. For Z_n^*, the group order is phi(n).

Thus ord_n(a) always divides phi(n).

```python
def order_divides_phi(a: int, n: int) -> bool:
    """Verify that the order of a divides phi(n)."""
    from math import gcd
    if gcd(a, n) != 1:
        return True  # vacuously
    ord_a = multiplicative_order(a, n)
    phi_n = phi(n)  # from euler-fermat
    return phi_n % ord_a == 0

def phi(n: int) -> int:
    """Compute Euler totient."""
    result = n
    p = 2
    temp = n
    while p * p <= temp:
        if temp % p == 0:
            while temp % p == 0:
                temp //= p
            result -= result // p
        p += 1 if p == 2 else 2
    if temp > 1:
        result -= result // temp
    return result

print(order_divides_phi(2, 7))   # True (ord=3, phi=6)
print(order_divides_phi(3, 7))   # True (ord=6, phi=6)
print(order_divides_phi(4, 7))   # True (ord=3, phi=6)
```

**Computing order via factorization of phi(n):**

To compute ord_n(a) efficiently, factor phi(n) and test divisors:

```python
def prime_factors(n: int) -> set[int]:
    """Return distinct prime factors of n."""
    factors = set()
    d = 2
    while d * d <= n:
        while n % d == 0:
            factors.add(d)
            n //= d
        d += 1 if d == 2 else 2
    if n > 1:
        factors.add(n)
    return factors

def order_fast(a: int, n: int) -> int:
    """Compute order using factorization of phi(n)."""
    from math import gcd
    if gcd(a, n) != 1:
        return -1
    phi_n = phi(n)
    order = phi_n
    for p in prime_factors(phi_n):
        while order % p == 0 and pow(a, order // p, n) == 1:
            order //= p
    return order

print(order_fast(2, 7))   # 3
print(order_fast(3, 7))   # 6
print(order_fast(5, 13))  # 4 (since 5^4 = 625 = 1 mod 13)
```

### Primitive Roots

A primitive root modulo n is an integer g whose multiplicative order is exactly phi(n). In other words, g generates the entire group Z_n^*.

If g is a primitive root modulo n, then every element of Z_n^* can be expressed as g^k for some k.

```python
def is_primitive_root(g: int, n: int) -> bool:
    """Check if g is a primitive root modulo n."""
    return order_fast(g, n) == phi(n)

# Primitive roots modulo 7
for g in range(1, 7):
    if is_primitive_root(g, 7):
        print(f"{g} is a primitive root mod 7")

# Primitive roots modulo 13
for g in range(1, 13):
    if is_primitive_root(g, 13):
        print(f"{g} is a primitive root mod 13")
```

**Output for modulo 7:**

```
3 is a primitive root mod 7
5 is a primitive root mod 7
```

**Output for modulo 13:**

```
2 is a primitive root mod 13
6 is a primitive root mod 13
7 is a primitive root mod 13
11 is a primitive root mod 13
```

### Existence of Primitive Roots

Primitive roots exist only for specific moduli:

| Modulus Type | Examples | Has Primitive Roots? |
|-------------|----------|---------------------|
| Prime p | 2, 3, 5, 7, 11, ... | Yes |
| 2p^k where p is odd prime | 2*3=6, 2*5=10, 2*7=14 | Yes |
| 2, 4 | 2, 4 | Yes |
| p^k where p is odd prime | 9, 25, 27, 49 | Yes |
| All other | 8, 12, 15, 20, 24 | No |

```python
def has_primitive_root(n: int) -> bool:
    """Check if a primitive root exists modulo n."""
    if n == 2 or n == 4:
        return True
    # Find odd prime factor
    factors = prime_factors(n)
    if len(factors) == 1 and n % 2 == 1:
        return True  # p^k for odd prime p
    if len(factors) == 1 and n % 2 == 0:
        return n == 2 or n == 4
    if 2 in factors:
        # 2 * p^k form
        if n % 4 == 0:
            return False
        odd_factors = factors - {2}
        return len(odd_factors) == 1
    return False

for n in range(2, 25):
    print(f"n={n}: has_primitive_root={has_primitive_root(n)}")
```

### Finding Primitive Roots

**Algorithm to find a primitive root modulo prime p:**

1. Factor phi(p) = p - 1 to get its distinct prime factors q_1, q_2, ..., q_k
2. For candidate g = 2, 3, ..., test if g^{(p-1)/q_i} != 1 (mod p) for all i
3. The first g that passes for all i is a primitive root

```python
def find_primitive_root(p: int) -> int | None:
    """Find a primitive root modulo prime p."""
    if not has_primitive_root(p):
        return None
    
    phi_p = phi(p)
    prime_factors_list = list(prime_factors(phi_p))
    
    for g in range(2, p):
        found = True
        for q in prime_factors_list:
            if pow(g, phi_p // q, p) == 1:
                found = False
                break
        if found:
            return g
    return None

primes = [7, 13, 17, 19, 23, 29]
for p in primes:
    g = find_primitive_root(p)
    print(f"Primitive root mod {p}: {g}")
```

**Number of primitive roots:** If a primitive root exists modulo n, there are exactly phi(phi(n)) of them.

```python
def count_primitive_roots(n: int) -> int:
    """Count primitive roots modulo n if they exist."""
    if not has_primitive_root(n):
        return 0
    count = 0
    for g in range(1, n):
        if is_primitive_root(g, n):
            count += 1
    return count

for p in [7, 13, 17, 19]:
    print(f"Primitive roots mod {p}: count={count_primitive_roots(p)}, phi(phi(p))={phi(phi(p))}")
```

### The Discrete Logarithm Problem

Given a generator g of a group G and an element h in G, find the unique integer x such that:

$$
g^x \equiv h \quad (\text{in } G)
$$

The integer x is called the discrete logarithm of h to the base g, written x = log_g(h).

```python
def discrete_log_brute(g: int, h: int, n: int) -> int | None:
    """Compute discrete logarithm by brute force (exponential)."""
    cur = 1
    for x in range(n):
        if cur == h:
            return x
        cur = (cur * g) % n
    return None

# Example: find x such that 3^x = 5 (mod 7)
x = discrete_log_brute(3, 5, 7)
print(f"3^{x} = 5 (mod 7)")  # 3^5 = 243 = 5 (mod 7), so x=5
```

### DLP Difficulty

The DLP is believed to be hard in well-chosen groups:

| Group | Best Attack | Complexity |
|-------|-------------|------------|
| Z_p^* (prime modulus) | Number Field Sieve | subexponential |
| Elliptic curve group | Pollard rho | exponential (sqrt(p)) |
| Z_n^* (composite) | Factoring + Pohlig-Hellman | depends on factors |

**Security requirement:** For DLP-based cryptography (Diffie-Hellman, ElGamal, DSA), the group order should have a large prime factor to prevent Pohlig-Hellman attacks.

### Baby-Step Giant-Step Algorithm

The Baby-Step Giant-Step (BSGS) algorithm solves DLP in O(sqrt(n)) time and space. It is a meet-in-the-middle attack.

Let n be the order of g. Write x = i*m + j where m = ceil(sqrt(n)):

1. Compute baby steps: g^j for j = 0, 1, ..., m-1, store in hash table
2. Compute giant steps: h * (g^{-m})^i for i = 0, 1, ..., m-1, check against hash table

```python
def baby_step_giant_step(g: int, h: int, n: int) -> int | None:
    """
    Compute discrete logarithm using Baby-Step Giant-Step.
    Returns x such that g^x = h (mod n), or None.
    """
    import math
    
    m = math.isqrt(n) + 1
    
    # Baby step: compute g^j for j in [0, m)
    baby = {}
    cur = 1
    for j in range(m):
        if cur not in baby:  # store first occurrence
            baby[cur] = j
        cur = (cur * g) % n
    
    # Giant step: compute h * (g^{-m})^i
    g_m = pow(g, m, n)
    g_m_inv = pow(g_m, -1, n)
    gamma = h
    for i in range(m):
        if gamma in baby:
            return i * m + baby[gamma]
        gamma = (gamma * g_m_inv) % n
    
    return None

# Example
g, h, n = 3, 5, 7
x = baby_step_giant_step(g, h, n)
print(f"BSGS: 3^{x} = 5 (mod 7)")  # x = 5
print(f"Verification: {pow(g, x, n)} == {h}")
```

### Pohlig-Hellman Algorithm

If the group order n = phi(p) has small prime factors, the DLP can be solved by reducing to smaller subgroups using CRT.

Let n = p_1^{e_1} * p_2^{e_2} * ... * p_k^{e_k}. For each prime factor p_i:

1. Compute x_i = x mod p_i^{e_i} by solving in the subgroup of order p_i
2. Combine using CRT to get x mod n

```python
def pohlig_hellman(g: int, h: int, n: int) -> int | None:
    """
    Solve DLP using Pohlig-Hellman algorithm.
    Requires factorization of group order.
    """
    from math import gcd
    
    # Get prime factorization of group order
    order = multiplicative_order(g, n)
    if order == -1:
        return None
    
    factors = prime_factors(order)
    if not factors:
        return discrete_log_brute(g, h, n)
    
    # Solve for each prime power
    congruences = []
    from collections import Counter
    # Count prime powers
    temp = order
    prime_powers = {}
    for p in prime_factors(order):
        count = 0
        while temp % p == 0:
            temp //= p
            count += 1
        prime_powers[p] = count
    
    for p, e in prime_powers.items():
        pe = p ** e
        # Reduce: work in subgroup of order p^e
        g_i = pow(g, order // pe, n)
        h_i = pow(h, order // pe, n)
        
        # Solve g_i^x_i = h_i in subgroup of order p^e
        # Use iterative approach for each digit in base p
        x_i = 0
        gamma = 1
        for k in range(e):
            # Find digit d_k
            h_k = pow(h_i * pow(gamma, -1, n), order // p**(k+1), n)
            g_k = pow(g_i, order // p, n)
            
            # Brute force digit (p is small)
            d_k = 0
            for j in range(p):
                if pow(g_k, j, n) == h_k:
                    d_k = j
                    break
            
            x_i = x_i + d_k * (p ** k)
            gamma = (gamma * pow(g_i, d_k * (p ** k), n)) % n
        
        congruences.append((x_i, pe))
    
    # Combine using CRT
    if not congruences:
        return None
    
    from math import prod
    M = prod(m for _, m in congruences)
    result = 0
    for a, m in congruences:
        M_i = M // m
        result += a * M_i * pow(M_i, -1, m)
    
    return result % M

# Test with small example
g, h, n = 2, 8, 11  # 2^x = 8 (mod 11), x = 3
x = pohlig_hellman(g, h, n)
print(f"Pohlig-Hellman: 2^{x} = 8 (mod 11)")
```

### Index Calculus

Index calculus is a subexponential algorithm for DLP in Z_p^*. It uses a factor base of small primes.

**Basic idea:**

1. Choose a factor base B = {p_1, p_2, ..., p_t} of small primes
2. Collect relations: find random k such that g^k mod p factors over B
3. Solve linear system for log_g(p_i) for each p_i in B
4. To find log_g(h), find random r such that h * g^r factors over B

This is the algorithm that makes DLP in Z_p^* subexponential (rather than exponential), which is why elliptic curve groups (which lack index calculus) are preferred for higher security.

### Quadratic Residues

An integer a is a quadratic residue modulo n if there exists x such that:

$$
x^2 \equiv a \pmod{n}
$$

If no such x exists, a is a quadratic non-residue.

```python
def quadratic_residues(n: int) -> list[int]:
    """Return all quadratic residues modulo n (non-zero)."""
    residues = set()
    for x in range(1, n):
        residues.add((x * x) % n)
    return sorted(residues)

for p in [5, 7, 11]:
    print(f"QR mod {p}: {quadratic_residues(p)}")
    print(f"  Count: {len(quadratic_residues(p))} = (p-1)/2 = {(p-1)//2}")
```

**Output:**

```
QR mod 5: [1, 4]
  Count: 2 = (p-1)/2 = 2
QR mod 7: [1, 2, 4]
  Count: 3 = (p-1)/2 = 3
QR mod 11: [1, 3, 4, 5, 9]
  Count: 5 = (p-1)/2 = 5
```

For an odd prime p, exactly half of the non-zero elements are quadratic residues.

### Legendre Symbol

The Legendre symbol (a/p) indicates whether a is a quadratic residue modulo p:

$$
\left(\frac{a}{p}\right) = \begin{cases}
0 & \text{if } p \mid a \\
1 & \text{if } a \text{ is a quadratic residue mod } p \\
-1 & \text{otherwise}
\end{cases}
$$

**Euler criterion:** For odd prime p:

$$
\left(\frac{a}{p}\right) \equiv a^{(p-1)/2} \pmod{p}
$$

```python
def legendre_symbol(a: int, p: int) -> int:
    """Compute Legendre symbol (a/p) using Euler criterion."""
    if a % p == 0:
        return 0
    result = pow(a, (p - 1) // 2, p)
    return 1 if result == 1 else -1

for a in range(1, 12):
    ls = legendre_symbol(a, 11)
    print(f"({a}/11) = {ls}")
```

**Properties:**

| Property | Formula |
|----------|---------|
| Multiplicativity | (a*b/p) = (a/p)*(b/p) |
| Value of 1 | (1/p) = 1 |
| Value of -1 | (-1/p) = (-1)^{(p-1)/2} |
| Value of 2 | (2/p) = (-1)^{(p^2-1)/8} |

### Jacobi Symbol

The Jacobi symbol extends the Legendre symbol to odd composite moduli. For odd n with prime factorization n = p_1^{e_1} * ... * p_k^{e_k}:

$$
\left(\frac{a}{n}\right) = \prod_{i=1}^{k} \left(\frac{a}{p_i}\right)^{e_i}
$$

Warning: (a/n) = 1 does NOT imply a is a quadratic residue modulo n; it may be a pseudo-square.

```python
def prime_factorization(n: int) -> dict[int, int]:
    """Return dict of prime factors with exponents."""
    factors = {}
    d = 2
    while d * d <= n:
        while n % d == 0:
            factors[d] = factors.get(d, 0) + 1
            n //= d
        d += 1 if d == 2 else 2
    if n > 1:
        factors[n] = 1
    return factors

def jacobi_symbol(a: int, n: int) -> int:
    """Compute Jacobi symbol (a/n) using the law of quadratic reciprocity."""
    if n <= 0 or n % 2 == 0:
        raise ValueError("n must be positive odd")
    
    t = 1
    while a != 0:
        while a % 2 == 0:
            a //= 2
            r = n % 8
            if r == 3 or r == 5:
                t = -t
        a, n = n, a
        if a % 4 == 3 and n % 4 == 3:
            t = -t
        a = a % n
    return t if n == 1 else 0

for a, n in [(2, 15), (7, 15), (4, 15), (11, 15)]:
    print(f"Jacobi({a}/{n}) = {jacobi_symbol(a, n)}")
```

### Quadratic Reciprocity

The law of quadratic reciprocity relates Legendre symbols for two odd primes:

For odd primes p and q:

$$
\left(\frac{p}{q}\right) \cdot \left(\frac{q}{p}\right) = (-1)^{\frac{p-1}{2} \cdot \frac{q-1}{2}}
$$

Equivalently:

$$
\left(\frac{p}{q}\right) = \begin{cases}
\left(\frac{q}{p}\right) & \text{if } p \equiv 1 \pmod{4} \text{ or } q \equiv 1 \pmod{4} \\
-\left(\frac{q}{p}\right) & \text{if } p \equiv q \equiv 3 \pmod{4}
\end{cases}
```

```python
def quadratic_reciprocity(p: int, q: int) -> int:
    """Compute (p/q) using quadratic reciprocity."""
    # Assuming p and q are odd primes
    if p == q:
        return 0
    # Reduce p modulo q
    p = p % q
    if p == 0:
        return 0
    
    # Use Jacobi calculation which implements reciprocity
    return jacobi_symbol(p, q)

print(f"(3/7) = {quadratic_reciprocity(3, 7)}")  # -1 (3 is not QR mod 7)
print(f"(5/7) = {quadratic_reciprocity(5, 7)}")  # -1
print(f"(3/11) = {quadratic_reciprocity(3, 11)}") # 1 (3 is QR mod 11: 5^2=25=3 mod 11)
```

## Glossary

| Term | Definition |
|------|------------|
| Order of an element | Smallest positive k such that a^k = 1 (mod n) |
| Primitive root | Element whose order equals phi(n), generating the entire group |
| Discrete logarithm | Integer x such that g^x = h (mod n) |
| DLP | Discrete Logarithm Problem: finding x given g and g^x |
| Baby-Step Giant-Step | O(sqrt(n)) meet-in-the-middle DLP algorithm |
| Pohlig-Hellman | DLP algorithm that exploits small prime factors of group order |
| Index calculus | Subexponential DLP algorithm using a factor base |
| Quadratic residue | Integer a for which x^2 = a (mod n) has a solution |
| Quadratic non-residue | Integer a for which x^2 = a (mod n) has no solution |
| Legendre symbol (a/p) | Indicates if a is a QR modulo prime p |
| Jacobi symbol (a/n) | Generalization of Legendre symbol to odd composite n |
| Quadratic reciprocity | Law relating (p/q) and (q/p) for odd primes |
| Euler criterion | a^{(p-1)/2} = (a/p) (mod p) for odd prime p |
| Diffie-Hellman | Key exchange protocol based on DLP |
| ElGamal | Public-key cryptosystem based on DLP |

## Quick References

- [Discrete logarithm on Wikipedia](https://en.wikipedia.org/wiki/Discrete_logarithm)
- [Primitive root modulo n on Wikipedia](https://en.wikipedia.org/wiki/Primitive_root_modulo_n)
- [Quadratic reciprocity on Wikipedia](https://en.wikipedia.org/wiki/Quadratic_reciprocity)
- [Baby-Step Giant-Step on Wikipedia](https://en.wikipedia.org/wiki/Baby-step_giant-step)
- [Pohlig-Hellman algorithm on Wikipedia](https://en.wikipedia.org/wiki/Pohlig%E2%80%93Hellman_algorithm)
- [Index calculus algorithm on Wikipedia](https://en.wikipedia.org/wiki/Index_calculus_algorithm)
- [Handbook of Applied Cryptography, Chapter 2](http://cacr.uwaterloo.ca/hac/) -- number theory and DLP

## Next Steps

- [Cryptographic Applications](cryptographic-applications.md) -- RSA, Diffie-Hellman, ElGamal, ECC, digital signatures
