# Euler Theorem and Fermat Little Theorem

## Description

Euler theorem and Fermat little theorem are cornerstones of modern cryptography. Fermat little theorem states that for a prime p, a^{p-1} = 1 (mod p). Euler theorem generalizes this to composite moduli using the totient function phi(n). These theorems underpin RSA key generation, primality testing, and modular exponentiation shortcuts used in virtually every secure communication protocol.

## Prerequisites

- [Modular Arithmetic](modular-arithmetic.md) -- modular operations, residue systems, modular inverses

## Table of Contents

- [Euler Totient Function](#euler-totient-function)
- [Computing the Totient](#computing-the-totient)
- [Properties of the Totient](#properties-of-the-totient)
- [Fermat Little Theorem](#fermat-little-theorem)
- [Proof of Fermat Little Theorem](#proof-of-fermat-little-theorem)
- [Euler Theorem](#euler-theorem)
- [Proof of Euler Theorem](#proof-of-euler-theorem)
- [Relationship Between the Theorems](#relationship-between-the-theorems)
- [Applications in Cryptography](#applications-in-cryptography)
- [Primality Testing with Fermat](#primality-testing-with-fermat)
- [Carmichael Numbers](#carmichael-numbers)
- [Modular Exponentiation Shortcuts](#modular-exponentiation-shortcuts)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Euler Totient Function

The Euler totient function phi(n) counts the number of integers in the range 1 <= k <= n that are coprime to n:

$$
\phi(n) = |\{ k \in \mathbb{Z} : 1 \le k \le n, \gcd(k, n) = 1 \}|
$$

**Examples:**

- phi(1) = 1 (by convention)
- phi(6) = 2 (the numbers 1 and 5 are coprime to 6)
- phi(12) = 4 (the numbers 1, 5, 7, 11 are coprime to 12)

```python
def phi_naive(n: int) -> int:
    """Compute Euler totient by naive counting."""
    from math import gcd
    count = 0
    for k in range(1, n + 1):
        if gcd(k, n) == 1:
            count += 1
    return count

print(phi_naive(1))   # 1
print(phi_naive(6))   # 2
print(phi_naive(12))  # 4
print(phi_naive(100)) # 40
```

### Computing the Totient

For efficient computation, use the formula based on prime factorization:

If the prime factorization of n is:

$$
n = \prod_{i=1}^{k} p_i^{e_i}
$$

then:

$$
\phi(n) = n \cdot \prod_{i=1}^{k} \left(1 - \frac{1}{p_i}\right)
$$

**Equivalently:**

$$
\phi(n) = \prod_{i=1}^{k} p_i^{e_i - 1} \cdot (p_i - 1)
$$

```python
def prime_factorization(n: int) -> dict[int, int]:
    """Return a dict mapping prime factors to their exponents."""
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

def phi(n: int) -> int:
    """Compute Euler totient using prime factorization."""
    if n == 1:
        return 1
    result = n
    factors = prime_factorization(n)
    for p in factors:
        result -= result // p
    return result

print(phi(6))    # 2
print(phi(12))   # 4
print(phi(100))  # 40
print(phi(97))   # 96 (since 97 is prime)
```

**Special cases:**

| n | phi(n) | Reason |
|---|--------|--------|
| Prime p | p - 1 | Every number 1..p-1 is coprime to p |
| p^k | p^k - p^{k-1} = p^{k-1}(p-1) | Remove multiples of p |
| p * q | (p-1)(q-1) | Multiplication property for coprime factors |

### Properties of the Totient

**Multiplicativity:**

If gcd(m, n) = 1, then phi(m*n) = phi(m) * phi(n).

```python
# Verify: phi(3*5) = phi(3) * phi(5) = 2 * 4 = 8
print(phi(15))  # 8
```

**Sum over divisors:**

$$
\sum_{d|n} \phi(d) = n
$$

```python
def sum_phi_over_divisors(n: int) -> int:
    """Sum of phi(d) for all divisors d of n."""
    from math import sqrt
    total = 0
    for i in range(1, int(sqrt(n)) + 1):
        if n % i == 0:
            total += phi(i)
            if i != n // i:
                total += phi(n // i)
    return total

for n in [6, 10, 12]:
    print(f"n={n}: sum phi(d) = {sum_phi_over_divisors(n)} (expected {n})")
```

**Totient of a product of distinct primes:**

If n = p * q where p and q are distinct primes:

$$
\phi(n) = (p-1)(q-1)
$$

This formula is critical for RSA key generation.

### Fermat Little Theorem

If p is a prime number and a is any integer not divisible by p, then:

$$
a^{p-1} \equiv 1 \pmod{p}
$$

**Alternative formulation:**

For any integer a:

$$
a^p \equiv a \pmod{p}
$$

```python
def fermat_test(a: int, p: int) -> bool:
    """Verify Fermat little theorem: a^{p-1} = 1 (mod p)."""
    if p <= 1:
        return False
    if a % p == 0:
        return True  # a^p = a (mod p) holds even for multiples
    return pow(a, p - 1, p) == 1

# Test with known primes
for p in [7, 13, 97, 101]:
    print(f"p={p}: 2^{p-1} mod {p} = {pow(2, p-1, p)}")  # All 1
```

### Proof of Fermat Little Theorem

**Proof using group theory:**

Consider the set {1, 2, ..., p-1} modulo p. Multiply each element by a:

{a, 2a, 3a, ..., (p-1)a}

Since gcd(a, p) = 1, these are all distinct modulo p (if ai = aj then p|a(i-j) so p|(i-j) so i=j). Thus they are a permutation of {1, 2, ..., p-1}.

Taking the product of both sets:

1 * 2 * ... * (p-1) = a * 2a * 3a * ... * (p-1)a (mod p)

(p-1)! = a^{p-1} * (p-1)! (mod p)

Since (p-1)! is invertible modulo p (none of its factors are divisible by p):

a^{p-1} = 1 (mod p)

**Proof using induction on a:**

Base case: 1^{p-1} = 1 (mod p) trivially.

Inductive step: Assume a^{p-1} = 1 (mod p). Consider (a+1)^p.

By the binomial theorem:

$$
(a+1)^p = \sum_{k=0}^{p} \binom{p}{k} a^k
$$

For 0 < k < p, p divides binom(p, k). So:

(a+1)^p = a^p + 1 (mod p)

By the induction hypothesis, a^p = a (mod p), so (a+1)^p = a + 1 (mod p).

By induction on a, a^p = a (mod p) for all non-negative a. For negative a, the result follows from the representation of negative numbers.

### Euler Theorem

Euler theorem generalizes Fermat little theorem to composite moduli:

If gcd(a, n) = 1, then:

$$
a^{\phi(n)} \equiv 1 \pmod{n}
$$

```python
def euler_theorem(a: int, n: int) -> bool:
    """Verify Euler theorem: a^{phi(n)} = 1 (mod n) for coprime a, n."""
    from math import gcd
    if gcd(a, n) != 1:
        return False
    return pow(a, phi(n), n) == 1

# Verify
print(euler_theorem(3, 10))   # phi(10) = 4, 3^4 = 81 = 1 mod 10
print(euler_theorem(2, 15))   # phi(15) = 8, 2^8 = 256 = 1 mod 15
print(euler_theorem(5, 21))   # phi(21) = 12, 5^12 = 244140625 = 1 mod 21
```

### Proof of Euler Theorem

Consider the reduced residue system modulo n: R = {r_1, r_2, ..., r_{phi(n)}}.

Since gcd(a, n) = 1, the set aR = {a * r_1, a * r_2, ..., a * r_{phi(n)}} is also a reduced residue system modulo n (if ar_i = ar_j then n|a(r_i-r_j) and since gcd(a,n)=1, n|(r_i-r_j) so r_i=r_j).

Since both sets are reduced residue systems, they are the same modulo n (up to order). Taking the product:

r_1 * r_2 * ... * r_{phi(n)} = (a*r_1)*(a*r_2)*...*(a*r_{phi(n)}) (mod n)

= a^{phi(n)} * r_1 * r_2 * ... * r_{phi(n)} (mod n)

Since all r_i are coprime to n, their product is coprime to n, so it is invertible modulo n. Canceling:

a^{phi(n)} = 1 (mod n)

### Relationship Between the Theorems

The relationship is clear:

| Theorem | Modulus | Condition | Result |
|---------|---------|-----------|--------|
| Fermat | p (prime) | a not divisible by p | a^{p-1} = 1 (mod p) |
| Euler | n (any) | gcd(a, n) = 1 | a^{phi(n)} = 1 (mod n) |

Fermat theorem is a special case of Euler theorem because for prime p, phi(p) = p-1.

```python
# Euler implies Fermat for prime moduli
p = 97
print(phi(p))  # 96 = p-1
print(pow(2, phi(p), p))  # 1
```

### Applications in Cryptography

**RSA Key Generation:**

1. Choose two large primes p, q
2. Compute n = p * q
3. Compute phi(n) = (p-1)(q-1)
4. Choose e such that gcd(e, phi(n)) = 1
5. Compute d = e^{-1} mod phi(n)

Encryption: c = m^e mod n
Decryption: m = c^d mod n = m^{e*d} mod n = m^{k*phi(n)+1} mod n = m mod n

The last step relies on Euler theorem: if gcd(m, n) = 1, then m^{phi(n)} = 1 (mod n), so m^{k*phi(n)+1} = m (mod n).

```python
def rsa_keygen(p: int, q: int, e: int) -> dict:
    """Generate RSA key pair."""
    n = p * q
    phi_n = (p - 1) * (q - 1)
    d = pow(e, -1, phi_n)  # modular inverse
    return {"public": (n, e), "private": (n, d)}

keys = rsa_keygen(61, 53, 17)
print(f"Public: {keys['public']}")
print(f"Private: {keys['private']}")

# Verify
m = 42
c = pow(m, keys['public'][1], keys['public'][0])
m2 = pow(c, keys['private'][1], keys['public'][0])
print(f"m={m}, c={c}, m'={m2}")  # m' should equal m
```

**Modular Inverse Computation:**

The inverse of a modulo m exists iff gcd(a, m) = 1. Euler theorem gives:

a^{-1} = a^{phi(m)-1} (mod m)

When m is prime: a^{-1} = a^{p-2} (mod p)

```python
def mod_inverse_euler(a: int, m: int) -> int:
    """Modular inverse using Euler theorem."""
    from math import gcd
    if gcd(a, m) != 1:
        raise ValueError("Inverse does not exist")
    return pow(a, phi(m) - 1, m)

print(mod_inverse_euler(3, 10))  # 7
print(mod_inverse_euler(17, 43)) # 38
```

### Primality Testing with Fermat

Fermat theorem provides a probabilistic primality test:

```python
import random

def fermat_primality_test(n: int, k: int = 10) -> bool:
    """
    Probabilistic primality test using Fermat theorem.
    Returns True if n is probably prime, False if composite.
    """
    if n < 2:
        return False
    if n == 2 or n == 3:
        return True
    if n % 2 == 0:
        return False
    
    for _ in range(k):
        a = random.randint(2, n - 2)
        if pow(a, n - 1, n) != 1:
            return False  # composite (witness)
    
    return True  # probably prime

print(fermat_primality_test(97))    # True (prime)
print(fermat_primality_test(100))   # False (composite)
print(fermat_primality_test(561))   # True (FAILS! 561 is Carmichael)
```

### Carmichael Numbers

The Fermat test fails for Carmichael numbers -- composite numbers n where a^{n-1} = 1 (mod n) for all a coprime to n. The smallest Carmichael number is 561 = 3 * 11 * 17.

```python
def is_carmichael(n: int) -> bool:
    """Check if n is a Carmichael number."""
    from math import gcd
    if n < 2:
        return False
    # Check if n is composite
    if all(n % i != 0 for i in range(2, int(n**0.5) + 1)):
        return False
    # Check Korselt criterion
    for a in range(2, n):
        if gcd(a, n) == 1 and pow(a, n - 1, n) != 1:
            return False
    return True

# Known Carmichael numbers
for n in [561, 1105, 1729, 2465]:
    print(f"{n}: Carmichael={is_carmichael(n)}")
```

**Korselt criterion:** A composite n is Carmichael iff:
1. n is square-free
2. For every prime p|n, (p-1)|(n-1)

```python
def is_carmichael_korselt(n: int) -> bool:
    """Check Carmichael using Korselt criterion."""
    factors = prime_factorization(n)
    if any(exp > 1 for exp in factors.values()):
        return False  # not square-free
    for p in factors:
        if (n - 1) % (p - 1) != 0:
            return False
    return len(factors) > 1  # must be composite

for n in [561, 1105, 1729]:
    print(f"{n}: Carmichael={is_carmichael_korselt(n)}")
```

Because of Carmichael numbers, the Miller-Rabin test is preferred over the Fermat test in practice.

### Modular Exponentiation Shortcuts

Euler theorem allows reducing exponents modulo phi(n) when the base is coprime to n:

$$
a^b \bmod n = a^{b \bmod \phi(n)} \bmod n \quad \text{if } \gcd(a, n) = 1
$$

```python
def mod_pow_shortcut(a: int, b: int, n: int) -> int:
    """Compute a^b mod n using Euler theorem for exponent reduction."""
    from math import gcd
    if gcd(a, n) == 1:
        b = b % phi(n)
    return pow(a, b, n)

# Without shortcut: 2^1000 mod 97
print(pow(2, 1000, 97))  # 87
# With shortcut: 2^(1000 mod 96) = 2^40 mod 97
print(pow(2, 1000 % 96, 97))  # 87

# Much larger exponent
b = 10**100  # astronomically large
result = mod_pow_shortcut(2, b, 97)
print(f"2^(10^100) mod 97 = {result}")
```

## Glossary

| Term | Definition |
|------|------------|
| Euler totient function phi(n) | Count of integers 1 <= k <= n with gcd(k, n) = 1 |
| Fermat little theorem | For prime p and a not divisible by p: a^{p-1} = 1 (mod p) |
| Euler theorem | For gcd(a, n) = 1: a^{phi(n)} = 1 (mod n) |
| Carmichael number | Composite n where a^{n-1} = 1 (mod n) for all a coprime to n |
| Korselt criterion | Characterization of Carmichael numbers as square-free n where (p-1)|(n-1) for all p|n |
| Fermat witness | Integer a such that a^{n-1} != 1 (mod n), proving n is composite |
| Reduced residue system | Set of phi(n) integers coprime to n, pairwise distinct modulo n |
| Multiplicative function | Function f where f(mn) = f(m)*f(n) for coprime m, n |
| Modular inverse | a^{-1} such that a * a^{-1} = 1 (mod m) |
| RSA | Public-key cryptosystem based on the difficulty of factoring n = p*q |
| Probabilistic primality test | Test that may incorrectly classify composites as prime with small probability |
| Miller-Rabin test | Stronger probabilistic primality test not fooled by Carmichael numbers |
| Exponent reduction | Reducing exponent modulo phi(n) for modular exponentiation shortcuts |

## Quick References

- [Euler theorem on Wikipedia](https://en.wikipedia.org/wiki/Euler%27s_theorem)
- [Fermat little theorem on Wikipedia](https://en.wikipedia.org/wiki/Fermat%27s_little_theorem)
- [Carmichael number on Wikipedia](https://en.wikipedia.org/wiki/Carmichael_number)
- [Euler totient function on Wikipedia](https://en.wikipedia.org/wiki/Euler%27s_totient_function)
- [Handbook of Applied Cryptography, Chapter 4](http://cacr.uwaterloo.ca/hac/) -- number theory for cryptography

## Next Steps

- [Chinese Remainder Theorem](chinese-remainder-theorem.md) -- solving systems of congruences
- [Cryptographic Applications](cryptographic-applications.md) -- RSA, Diffie-Hellman, ElGamal, ECC
