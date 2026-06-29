# Chinese Remainder Theorem

## Description

The Chinese Remainder Theorem (CRT) is a powerful result that solves systems of simultaneous congruences. It states that given pairwise coprime moduli, there exists a unique solution modulo their product. The CRT has direct applications in cryptography (RSA acceleration), distributed computing (secret sharing), and high-performance integer arithmetic (residue number systems).

## Prerequisites

- [Modular Arithmetic](modular-arithmetic.md) -- congruence relations, modular inverses, residue systems

## Table of Contents

- [CRT Statement](#crt-statement)
- [Proof of the Chinese Remainder Theorem](#proof-of-the-chinese-remainder-theorem)
- [Constructive Algorithm](#constructive-algorithm)
- [Garner Algorithm](#garner-algorithm)
- [Generalization to Non-Coprime Moduli](#generalization-to-non-coprime-moduli)
- [CRT in Programming](#crt-in-programming)
- [Applications of CRT](#applications-of-crt)
- [CRT for RSA Acceleration](#crt-for-rsa-acceleration)
- [Residue Number Systems](#residue-number-systems)
- [Secret Sharing with CRT](#secret-sharing-with-crt)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### CRT Statement

Let m_1, m_2, ..., m_k be pairwise coprime positive integers. For any integers a_1, a_2, ..., a_k, the system of congruences:

$$
x \equiv a_1 \pmod{m_1}
$$
$$
x \equiv a_2 \pmod{m_2}
$$
$$
\vdots
$$
$$
x \equiv a_k \pmod{m_k}
$$

has a unique solution modulo M = m_1 * m_2 * ... * m_k.

**Pairwise coprime** means gcd(m_i, m_j) = 1 for all i != j.

In simpler terms: if you know the remainder of an integer modulo several pairwise coprime moduli, you can determine its remainder modulo their product.

### Proof of the Chinese Remainder Theorem

**Existence:** For each i, define M_i = M / m_i. Since m_i is coprime to every other modulus, gcd(M_i, m_i) = 1. Therefore M_i has a modular inverse modulo m_i. Let y_i be the inverse:

$$
M_i \cdot y_i \equiv 1 \pmod{m_i}
$$

Construct the solution:

$$
x = \sum_{i=1}^{k} a_i \cdot M_i \cdot y_i
$$

Check: modulo m_i, every term with j != i is divisible by m_i (since M_j contains m_i as a factor), so:

$$
x \equiv a_i \cdot (M_i \cdot y_i) \equiv a_i \cdot 1 \equiv a_i \pmod{m_i}
$$

Thus x satisfies all congruences.

**Uniqueness:** If x and x' are both solutions, then x - x' is divisible by every m_i. Since the moduli are pairwise coprime, x - x' is divisible by their product M, so x = x' (mod M).

### Constructive Algorithm

The proof above directly gives an algorithm:

```python
def crt(congruences: list[tuple[int, int]]) -> tuple[int, int]:
    """
    Solve a system of congruences using the Chinese Remainder Theorem.
    
    Each element of congruences is (a_i, m_i) representing x = a_i (mod m_i).
    Returns (x, M) where x is the unique solution modulo M.
    """
    M = 1
    for _, m in congruences:
        M *= m
    
    x = 0
    for a_i, m_i in congruences:
        M_i = M // m_i
        y_i = pow(M_i, -1, m_i)  # modular inverse
        x += a_i * M_i * y_i
    
    return (x % M, M)

# Example: x = 2 (mod 3), x = 3 (mod 5), x = 2 (mod 7)
congruences = [(2, 3), (3, 5), (2, 7)]
x, M = crt(congruences)
print(f"x = {x} (mod {M})")  # x = 23 (mod 105)

# Verify
for a_i, m_i in congruences:
    print(f"  {x} mod {m_i} = {x % m_i} (expected {a_i})")
```

**Step-by-step walkthrough** for the example above:

| i | a_i | m_i | M_i | y_i (M_i^-1 mod m_i) | a_i * M_i * y_i |
|---|---|-----|-----|----------------------|-----------------|
| 1 | 2 | 3 | 35 | 2 (35*2 = 70 = 1 mod 3) | 140 |
| 2 | 3 | 5 | 21 | 1 (21*1 = 21 = 1 mod 5) | 63 |
| 3 | 2 | 7 | 15 | 1 (15*1 = 15 = 1 mod 7) | 30 |

Sum = 140 + 63 + 30 = 233. 233 mod 105 = 23.

### Garner Algorithm

The constructive algorithm above requires large intermediate products (a_i * M_i * y_i can be huge). Garner algorithm computes the solution incrementally without large intermediates.

**Algorithm:**

Express x in mixed-radix form:

$$
x = v_1 + v_2 \cdot m_1 + v_3 \cdot m_1 \cdot m_2 + \ldots + v_k \cdot m_1 \cdots m_{k-1}
$$

where 0 <= v_i < m_i.

```python
def garner(congruences: list[tuple[int, int]]) -> tuple[int, int]:
    """
    Solve CRT using Garner algorithm (mixed-radix representation).
    
    Each element of congruences is (a_i, m_i).
    Returns (x, M) where x is the unique solution modulo M.
    """
    k = len(congruences)
    a = [ai for ai, mi in congruences]
    m = [mi for ai, mi in congruences]
    
    # Compute v_i coefficients
    v = [0] * k
    for i in range(k):
        t = a[i]
        for j in range(i):
            t = ((t - v[j]) * pow(m[j], -1, m[i])) % m[i]
        v[i] = t
    
    # Reconstruct x from mixed-radix form
    x = v[-1]
    for i in range(k - 2, -1, -1):
        x = x * m[i] + v[i]
    
    M = 1
    for mi in m:
        M *= mi
    
    return (x % M, M)

congruences = [(2, 3), (3, 5), (2, 7)]
x, M = garner(congruences)
print(f"x = {x} (mod {M})")  # x = 23 (mod 105)
```

Garner algorithm is preferred when many operations are performed in the same residue system (e.g., RSA with CRT).

### Generalization to Non-Coprime Moduli

If the moduli are not pairwise coprime, a solution exists iff for all i, j:

$$
a_i \equiv a_j \pmod{\gcd(m_i, m_j)}
$$

```python
def crt_generalized(congruences: list[tuple[int, int]]) -> tuple[int, int] | None:
    """
    Solve CRT for non-coprime moduli.
    Returns (x, M) or None if no solution exists.
    """
    from math import gcd
    
    # Merge congruences pairwise
    a1, m1 = congruences[0]
    for a2, m2 in congruences[1:]:
        g = gcd(m1, m2)
        if (a1 - a2) % g != 0:
            return None  # No solution
        
        # Merge x = a1 (mod m1) and x = a2 (mod m2)
        # New modulus: lcm(m1, m2)
        l = m1 // g * m2
        # Use extended GCD to find combined solution
        # x = a1 + m1 * ((a2 - a1) // g * inv(m1/g mod m2/g))
        from math import gcd
        _, p, q = extended_gcd(m1 // g, m2 // g)
        a1 = a1 + m1 * ((a2 - a1) // g * p) % (m2 // g)
        a1 = (a1 % l + l) % l
        m1 = l
    
    return (a1, m1)

def extended_gcd(a: int, b: int) -> tuple[int, int, int]:
    """Return (gcd, x, y) such that a*x + b*y = gcd."""
    if b == 0:
        return (abs(a), 1 if a > 0 else -1, 0)
    g, x1, y1 = extended_gcd(b, a % b)
    return (g, y1, x1 - (a // b) * y1)

# Compatible non-coprime example
result = crt_generalized([(2, 6), (8, 10)])
print(result)  # (38, 30) since 38 mod 6 = 2 and 38 mod 10 = 8

# Incompatible non-coprime example
result = crt_generalized([(2, 6), (3, 10)])
print(result)  # None
```

### CRT in Programming

**Standard CRT implementation:**

```python
def crt_standard(remainders: list[int], moduli: list[int]) -> int:
    """Solve CRT given lists of remainders and moduli."""
    M = 1
    for m in moduli:
        M *= m
    
    x = 0
    for a_i, m_i in zip(remainders, moduli):
        M_i = M // m_i
        inv = pow(M_i, -1, m_i)
        x += a_i * M_i * inv
    
    return x % M

print(crt_standard([2, 3, 2], [3, 5, 7]))  # 23
```

**Handling large M that exceeds integer precision:**

When M is too large for native integers (e.g., in RSA with 4096-bit moduli), use arbitrary-precision arithmetic or the Garner algorithm.

### Applications of CRT

**RSA Decryption Acceleration:**

RSA decryption computes m = c^d mod n where n = p * q. Using CRT, we can compute:

$$
m_p = c^d \mod p = c^{d \mod (p-1)} \mod p
$$
$$
m_q = c^d \mod q = c^{d \mod (q-1)} \mod q
$$

Then combine using CRT to get m mod n. This is roughly 4x faster than direct computation.

```python
def rsa_decrypt_crt(c: int, d: int, p: int, q: int) -> int:
    """RSA decryption using CRT for acceleration."""
    # Precompute
    d_p = d % (p - 1)
    d_q = d % (q - 1)
    q_inv = pow(q, -1, p)
    
    m_p = pow(c, d_p, p)
    m_q = pow(c, d_q, q)
    
    # Combine using CRT
    h = (q_inv * (m_p - m_q)) % p
    m = m_q + h * q
    return m

p, q = 61, 53
n = p * q
e = 17
d = 2753
c = pow(42, e, n)  # encrypted 42
m = rsa_decrypt_crt(c, d, p, q)
print(f"Decrypted: {m}")  # 42
```

### Residue Number Systems

A Residue Number System (RNS) represents an integer by its residues modulo a set of pairwise coprime moduli. Arithmetic operations (addition, subtraction, multiplication) can be performed independently on each residue.

```python
class RNS:
    """Residue Number System implementation."""
    
    def __init__(self, moduli: list[int]):
        self.moduli = moduli
        self.M = 1
        for m in moduli:
            self.M *= m
    
    def encode(self, x: int) -> list[int]:
        """Encode an integer into RNS representation."""
        return [x % m for m in self.moduli]
    
    def decode(self, residues: list[int]) -> int:
        """Decode RNS representation back to integer using CRT."""
        return crt_standard(residues, self.moduli)
    
    def add(self, a: list[int], b: list[int]) -> list[int]:
        """Add two RNS numbers component-wise."""
        return [(ai + bi) % mi for ai, bi, mi in zip(a, b, self.moduli)]
    
    def multiply(self, a: list[int], b: list[int]) -> list[int]:
        """Multiply two RNS numbers component-wise."""
        return [(ai * bi) % mi for ai, bi, mi in zip(a, b, self.moduli)]

# Example
rns = RNS([3, 5, 7])
a = rns.encode(23)
b = rns.encode(42)
print(f"23 in RNS: {a}")
print(f"42 in RNS: {b}")
print(f"23 + 42 = {rns.decode(rns.add(a, b))}")  # 65
print(f"23 * 42 = {rns.decode(rns.multiply(a, b))}")  # 966
```

**Advantages of RNS:**
- Parallel computation on each residue
- No carry propagation between residues
- Useful in digital signal processing, cryptography, and hardware design

**Disadvantages:**
- Comparison and division are difficult
- Overflow detection requires conversion to standard form

### Secret Sharing with CRT

CRT-based secret sharing (Mignotte scheme) divides a secret into shares such that a threshold number of shares can reconstruct it.

```python
def mignotte_secret_sharing(secret: int, n: int, k: int) -> list[tuple[int, int]]:
    """
    Create n shares using CRT-based secret sharing.
    Any k shares can reconstruct the secret.
    Uses Mignotte sequences of pairwise coprime moduli.
    """
    # Generate n pairwise coprime moduli
    # In practice, use a proper Mignotte sequence
    moduli = [3, 5, 7, 11, 13, 17, 19, 23, 29, 31][:n]
    
    # Ensure secret is in valid range
    # Product of smallest k moduli > secret >= product of largest (k-1) moduli
    
    shares = [(secret % m, m) for m in moduli]
    return shares

def reconstruct_secret(shares: list[tuple[int, int]], k: int) -> int:
    """Reconstruct secret from any k shares using CRT."""
    return crt(shares[:k])[0]

# Example: share 42 into 5 shares, require 3 to reconstruct
shares = mignotte_secret_sharing(42, 5, 3)
print(f"Shares: {shares}")

# Reconstruct with first 3 shares
secret = reconstruct_secret(shares[:3], 3)
print(f"Reconstructed: {secret}")  # 42

# Try with only 2 shares (should fail for large enough moduli)
partial = reconstruct_secret(shares[:2], 2)
print(f"Partial (2 shares): {partial}")  # Wrong value
```

## Study Cases

### Case 1: CRT for Large Integer Arithmetic

Compute 123456789 * 987654321 mod 1000000007 without overflow:

```python
# Use CRT with intermediate moduli
def large_mul_mod(a: int, b: int, m: int) -> int:
    """Compute (a * b) % m using CRT for safety."""
    # Use pairwise coprime moduli larger than expected result
    moduli = [1000000007, 1000000009, 1000000021]
    residues_a = [a % mi for mi in moduli]
    residues_b = [b % mi for mi in moduli]
    residues_p = [(ai * bi) % mi for ai, bi, mi in zip(residues_a, residues_b, moduli)]
    
    # Use CRT to reconstruct (result is guaranteed to be correct)
    return crt_standard(residues_p, moduli) % m

result = large_mul_mod(123456789, 987654321, 1000000007)
print(result)  # Verify: (123456789 * 987654321) % 1000000007

# Direct computation
print((123456789 * 987654321) % 1000000007)
```

### Case 2: CRT-Based Fault Attack Protection

In RSA-CRT, a fault attack injects an error during computation. The result is incorrect modulo one prime but correct for the other, allowing factorization of n.

```python
def rsa_crt_with_check(c: int, d: int, p: int, q: int) -> int | None:
    """RSA-CRT decryption with fault attack protection."""
    d_p = d % (p - 1)
    d_q = d % (q - 1)
    q_inv = pow(q, -1, p)
    
    m_p = pow(c, d_p, p)
    m_q = pow(c, d_q, q)
    
    # Recompute for verification
    if pow(m_p, c % (p - 1), p) != pow(c, d_p * (c % (p - 1)), p):
        return None  # Fault detected
    
    m = m_q + q * ((q_inv * (m_p - m_q)) % p)
    
    # Verify against original ciphertext
    if pow(m, (d * c) % ((p-1)*(q-1)), p*q) != c:
        return None  # Fault detected
    
    return m

# In practice, simpler checks are used
def rsa_crt_blinded(c: int, d: int, p: int, q: int) -> int:
    """RSA-CRT with blinding to prevent fault attacks."""
    # Blind with random r
    r = 42  # In practice, use cryptographically random
    r_inv = pow(r, -1, p * q)
    c_blind = (c * pow(r, d, p * q)) % (p * q)
    
    d_p = d % (p - 1)
    d_q = d % (q - 1)
    q_inv = pow(q, -1, p)
    
    m_p = pow(c_blind, d_p, p)
    m_q = pow(c_blind, d_q, q)
    
    m_blind = m_q + q * ((q_inv * (m_p - m_q)) % p)
    m = (m_blind * r_inv) % (p * q)
    return m
```

### Case 3: CRT for Go/Nim Games (Period Finding)

Three bells ring every 3, 5, and 7 minutes respectively. They all ring together at midnight. When will they next ring together?

This is equivalent to solving x = 0 (mod 3), x = 0 (mod 5), x = 0 (mod 7).

```python
# Trivial solution: x = 0 (mod 105), so in 105 minutes (1:45 AM)
print(f"Next ring together in {crt_standard([0, 0, 0], [3, 5, 7])} minutes")
```

## Examples

### Example 1: Solving a 3-Congruence System

```python
def solve_and_verify(congruences: list[tuple[int, int]]):
    x, M = crt(congruences)
    print(f"Solution: x = {x} (mod {M})")
    for a_i, m_i in congruences:
        assert x % m_i == a_i, f"Failed: {x} mod {m_i} != {a_i}"
    print("All checks passed.")

solve_and_verify([(1, 2), (2, 3), (3, 5)])
solve_and_verify([(0, 7), (3, 11), (5, 13)])
```

### Example 2: Finding Missing CRT Modulus

Given x = 5 (mod 7), x = 3 (mod 11), and x = 8 (mod ?), find the missing modulus m if the solution is x = 327 (mod 1001) and 1001 = 7 * 11 * m.

```python
# 1001 = 7 * 11 * 13, so m = 13
x = 327
print(f"x mod 13 = {327 % 13}")  # 8, matches "x = 8 (mod 13)"
```

### Example 3: CRT for Polynomial Interpolation (Lagrange)

CRT and Lagrange interpolation share the same structure:

```python
def lagrange_interpolation(points: list[tuple[float, float]], x: float) -> float:
    """Lagrange interpolation (note: over reals, not modulo)."""
    n = len(points)
    result = 0.0
    for i in range(n):
        xi, yi = points[i]
        term = yi
        for j in range(n):
            if i != j:
                xj, _ = points[j]
                term *= (x - xj) / (xi - xj)
        result += term
    return result

# Equivalent structure to CRT: sum of y_i * (product of (x - x_j) / (x_i - x_j))
```

### Example 4: Fast CRT Precomputation

When solving many CRT systems with the same moduli, precompute M_i and y_i:

```python
class CRTPrecomputed:
    """CRT solver with precomputed inverses for fixed moduli."""
    
    def __init__(self, moduli: list[int]):
        self.moduli = moduli
        self.M = 1
        for m in moduli:
            self.M *= m
        self.M_i = [self.M // m for m in moduli]
        self.inv = [pow(Mi, -1, m) for Mi, m in zip(self.M_i, moduli)]
    
    def solve(self, remainders: list[int]) -> int:
        """Solve CRT for the precomputed moduli."""
        x = sum(a * Mi * yi for a, Mi, yi in zip(remainders, self.M_i, self.inv))
        return x % self.M

crt_fast = CRTPrecomputed([3, 5, 7])
print(crt_fast.solve([2, 3, 2]))  # 23
print(crt_fast.solve([1, 2, 3]))  # 52
```

### Example 5: CRT Error Detection

```python
def crt_with_error_detection(remainders: list[int], moduli: list[int]) -> int | None:
    """
    Solve CRT with error detection.
    Uses an extra modulus for redundancy.
    """
    if len(remainders) < 2:
        return None
    
    # Solve with all moduli
    x_all = crt_standard(remainders, moduli)
    
    # Solve with all but last, then verify
    x_partial = crt_standard(remainders[:-1], moduli[:-1])
    
    # Verify consistency
    if x_partial % moduli[-1] != remainders[-1]:
        return None  # Error detected
    
    return x_all

print(crt_with_error_detection([2, 3, 2], [3, 5, 7]))  # 23
print(crt_with_error_detection([2, 3, 3], [3, 5, 7]))  # None (error)
```

## Glossary

| Term | Definition |
|------|------------|
| Chinese Remainder Theorem | Theorem solving systems of simultaneous congruences with pairwise coprime moduli |
| Pairwise coprime | A set of integers where every pair has gcd = 1 |
| Constructive algorithm | Algorithm derived from proof that builds the CRT solution |
| Garner algorithm | Incremental CRT algorithm using mixed-radix representation |
| Residue Number System (RNS) | Representation of integers by residues modulo pairwise coprime moduli |
| Mignotte scheme | CRT-based secret sharing scheme |
| RSA-CRT | Using CRT to accelerate RSA decryption |
| Fault attack | Injecting computation errors to leak secret information |
| Mixed-radix representation | Expressing a number as v_1 + v_2*m_1 + v_3*m_1*m_2 + ... |
| Lagrange interpolation | Polynomial interpolation with structure analogous to CRT |
| Blinding | Cryptographic technique to randomize inputs for side-channel protection |
| Modulus product | M = product of all moduli in a CRT system |

## Quick References

- [Chinese Remainder Theorem on Wikipedia](https://en.wikipedia.org/wiki/Chinese_remainder_theorem)
- [Garner Algorithm on Wikipedia](https://en.wikipedia.org/wiki/Garner%27s_algorithm)
- [Residue Number System on Wikipedia](https://en.wikipedia.org/wiki/Residue_number_system)
- [Handbook of Applied Cryptography, Chapter 14](http://cacr.uwaterloo.ca/hac/) -- CRT applications in cryptography

## Next Steps

- [Discrete Logarithms](discrete-logarithms.md) -- primitive roots, discrete logarithm problem, Diffie-Hellman
