# Primality Testing and Hashing

## Description

Primality testing and cryptographic hashing are two fundamental applications of number theory in computing. Primality tests determine whether a number is prime (essential for RSA key generation), while hash functions map arbitrary data to fixed-size digests (essential for integrity verification, password storage, and blockchain). This document covers deterministic and probabilistic primality tests, the Miller-Rabin and AKS algorithms, SHA hash functions, collision resistance, the birthday attack, and practical applications.

## Prerequisites

- [Modular Arithmetic](modular-arithmetic.md) -- modular exponentiation, fast exponentiation
- [Cryptographic Applications](cryptographic-applications.md) -- RSA, key generation, digital signatures

## Table of Contents

- [Why Primality Testing Matters](#why-primality-testing-matters)
- [Deterministic vs Probabilistic Tests](#deterministic-vs-probabilistic-tests)
- [Trial Division](#trial-division)
- [Fermat Primality Test](#fermat-primality-test)
- [Miller-Rabin Primality Test](#miller-rabin-primality-test)
- [Miller-Rabin Deterministic Variants](#miller-rabin-deterministic-variants)
- [AKS Primality Test](#aks-primality-test)
- [Comparison of Primality Tests](#comparison-of-primality-tests)
- [Cryptographic Hash Functions](#cryptographic-hash-functions)
- [Properties of Hash Functions](#properties-of-hash-functions)
- [SHA-256 Structure](#sha-256-structure)
- [SHA-3 (Keccak) Overview](#sha-3-keccak-overview)
- [Collision Resistance and the Birthday Attack](#collision-resistance-and-the-birthday-attack)
- [Checksums and Error Detection](#checksums-and-error-detection)
- [Applications](#applications)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Why Primality Testing Matters

Generating large primes is essential for public-key cryptography. RSA requires two large primes for its modulus n = p * q. The primes must be:

- Large enough (2048-bit RSA uses 1024-bit primes)
- Truly random (predictable primes break security)
- Independent (p and q must not be close to each other)

Primality testing determines if a candidate is prime. For RSA-sized primes (1024+ bits), we use probabilistic tests that are fast but have a tiny chance of error.

### Deterministic vs Probabilistic Tests

| Type | Examples | Guarantee | Speed on Large Numbers |
|------|----------|-----------|----------------------|
| Deterministic | Trial division, AKS | 100% correct | Slow or very slow |
| Probabilistic | Fermat, Miller-Rabin, Solovay-Strassen | Correct with high probability | Fast |
| Hybrid | Trial division + Miller-Rabin | Correct (in practice) | Fast |

In practice, cryptographic libraries use trial division to filter obvious composites, then Miller-Rabin with multiple bases.

### Trial Division

The simplest deterministic test: check divisibility by all primes up to sqrt(n).

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

# Test
print(is_prime_trial(97))    # True
print(is_prime_trial(100))   # False
print(is_prime_trial(561))   # False (3*11*17)
```

**Complexity:** O(sqrt(n)) -- exponential in the bit length. Completely impractical for 1024-bit numbers.

**Optimized version with precomputed primes:**

```python
def trial_division_with_primes(n: int, primes: list[int]) -> bool:
    """Trial division using a list of small primes."""
    if n < 2:
        return False
    for p in primes:
        if p * p > n:
            break
        if n % p == 0:
            return False
    return True

# Precompute small primes
def small_primes(limit: int) -> list[int]:
    """Generate list of primes up to limit using sieve."""
    is_prime = [True] * (limit + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(limit ** 0.5) + 1):
        if is_prime[i]:
            for j in range(i * i, limit + 1, i):
                is_prime[j] = False
    return [i for i, p in enumerate(is_prime) if p]

primes_up_to_100k = small_primes(100000)

def is_prime_filtered(n: int) -> bool:
    """Trial division up to 100k, then assume prime (for large n)."""
    return trial_division_with_primes(n, primes_up_to_100k)
```

### Fermat Primality Test

Based on Fermat little theorem: if p is prime and gcd(a, p) = 1, then a^{p-1} = 1 (mod p).

If a^{n-1} != 1 (mod n) for some a, then n is definitely composite (a is a witness). If a^{n-1} = 1 (mod n) for all tested a, n is probably prime.

```python
import random

def fermat_test(n: int, k: int = 10) -> bool:
    """
    Fermat primality test.
    Returns False if n is composite, True if probably prime.
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
            return False  # composite
    return True  # probably prime

print(fermat_test(97))     # True
print(fermat_test(561))    # True (BUG! 561 is Carmichael)
```

**Limitation: Carmichael numbers.** Composite numbers n where a^{n-1} = 1 (mod n) for all a coprime to n. The Fermat test falsely classifies these as prime.

```python
def list_carmichael(limit: int) -> list[int]:
    """List Carmichael numbers up to limit."""
    carmichaels = []
    for n in range(3, limit + 1, 2):
        if is_prime_trial(n):
            continue  # skip actual primes
        is_pseudoprime = True
        for a in range(2, min(n, 100)):
            if math.gcd(a, n) == 1 and pow(a, n - 1, n) != 1:
                is_pseudoprime = False
                break
        if is_pseudoprime:
            carmichaels.append(n)
    return carmichaels

import math
print(list_carmichael(10000))  # [561, 1105, 1729, 2465, 2821, 6601, 8911]
```

The Fermat test is not used in practice because of Carmichael numbers. The Miller-Rabin test does not have this weakness.

### Miller-Rabin Primality Test

The Miller-Rabin test is the most widely used probabilistic primality test. It strengthens the Fermat test by also checking for non-trivial square roots of 1 modulo n.

**Algorithm:**

Write n - 1 = 2^r * d where d is odd.

For each base a:

1. Compute x_0 = a^d mod n
2. If x_0 = 1 or x_0 = n - 1, n passes for this base
3. For i = 1 to r - 1: compute x_i = x_{i-1}^2 mod n
4. If x_i = n - 1, n passes; if x_i = 1, n is composite (found non-trivial sqrt of 1)
5. If no x_i = n - 1, n is composite

```python
def miller_rabin(n: int, k: int = 10) -> bool:
    """
    Miller-Rabin primality test.
    
    Args:
        n: Number to test
        k: Number of test rounds
    
    Returns:
        False if n is definitely composite
        True if n is probably prime
    """
    if n < 2:
        return False
    if n == 2 or n == 3:
        return True
    if n % 2 == 0:
        return False
    
    # Write n-1 as 2^r * d
    r, d = 0, n - 1
    while d % 2 == 0:
        r += 1
        d //= 2
    
    # Test each base
    for _ in range(k):
        a = random.randint(2, n - 2)
        x = pow(a, d, n)
        
        if x == 1 or x == n - 1:
            continue  # passes this round
        
        for _ in range(r - 1):
            x = pow(x, 2, n)
            if x == n - 1:
                break  # passes
        else:
            # Loop finished without break: composite
            return False
    
    return True  # probably prime

# Test
print(miller_rabin(97))     # True
print(miller_rabin(561))    # False (correctly identifies Carmichael)
print(miller_rabin(100))    # False
```

**Why Miller-Rabin beats Fermat:**

The Miller-Rabin test catches Carmichael numbers because it checks for non-trivial square roots of 1. If n is composite, there exists a non-trivial square root of 1 (an x != 1, n-1 where x^2 = 1 mod n), which the test detects.

```python
# Finding non-trivial square roots of 1
def nontrivial_sqrt_1(n: int) -> list[int]:
    """Find non-trivial square roots of 1 modulo n (if n is composite)."""
    roots = []
    for x in range(2, n - 1):
        if (x * x) % n == 1:
            roots.append(x)
    return roots

print(f"mod 7 (prime): {nontrivial_sqrt_1(7)}")       # [] (none)
print(f"mod 15 (composite): {nontrivial_sqrt_1(15)}")  # [4, 11]
```

**Error probability:**

The probability that Miller-Rabin incorrectly classifies a composite as prime after k rounds is at most (1/4)^k. For k = 20, the error probability is approximately (1/4)^20 = 9 * 10^{-13}.

### Miller-Rabin Deterministic Variants

For n < specific bounds, Miller-Rabin can be made deterministic by testing a fixed set of bases:

```python
def miller_rabin_deterministic(n: int) -> bool:
    """
    Deterministic Miller-Rabin for n < 3,317,044,064,679,887,385,096,981.
    Uses known sets of bases that make the test deterministic within bounds.
    """
    if n < 2:
        return False
    if n == 2 or n == 3:
        return True
    if n % 2 == 0:
        return False
    
    # Write n-1 as 2^r * d
    r, d = 0, n - 1
    while d % 2 == 0:
        r += 1
        d //= 2
    
    # Deterministic bases for different ranges
    if n < 2047:
        bases = [2]
    elif n < 1373653:
        bases = [2, 3]
    elif n < 9080191:
        bases = [31, 73]
    elif n < 25326001:
        bases = [2, 3, 5]
    elif n < 3215031751:
        bases = [2, 3, 5, 7]
    elif n < 2152302898747:
        bases = [2, 3, 5, 7, 11]
    elif n < 3474749660383:
        bases = [2, 3, 5, 7, 11, 13]
    elif n < 341550071728321:
        bases = [2, 3, 5, 7, 11, 13, 17]
    else:
        bases = [2, 3, 5, 7, 11, 13, 17, 19, 23]
    
    for a in bases:
        if a >= n:
            continue
        x = pow(a, d, n)
        if x == 1 or x == n - 1:
            continue
        for _ in range(r - 1):
            x = pow(x, 2, n)
            if x == n - 1:
                break
        else:
            return False
    return True

# Test on known primes
test_primes = [97, 101, 103, 107, 109, 1000000007, 2147483647]
for p in test_primes:
    assert miller_rabin_deterministic(p), f"Failed on {p}"
print("All deterministic tests passed")
```

### AKS Primality Test

The AKS (Agrawal-Kayal-Saxena) test, published in 2002, is the first deterministic polynomial-time primality test. It runs in O(log(n)^{12}) time (improved to O(log(n)^6)).

**Theoretical importance:** AKS proved that primality testing is in P (polynomial time), settling a long-standing open problem. However, the constant factors make it slower than Miller-Rabin for practical key sizes.

**Simplified AKS algorithm:**

1. Check if n = a^b for some a, b (perfect power test)
2. Find smallest r such that order_r(n) > (log n)^2
3. If gcd(a, n) != 1 for some a <= r, n is composite
4. If n <= r, n is prime
5. For a = 1 to floor(sqrt(phi(r)) * log n):
   Check if (x + a)^n = x^n + a (mod x^r - 1, n)
6. If all pass, n is prime

**Why AKS is not used in practice:**

| Factor | AKS | Miller-Rabin |
|--------|-----|-------------|
| Runtime for 1024-bit | Hours | Milliseconds |
| Error probability | 0 (deterministic) | < (1/4)^20 (negligible) |
| Implementation complexity | Very high | Low |
| Parallelizable | Partially | Yes |

### Comparison of Primality Tests

| Test | Type | Time Complexity | Error | Use Case |
|------|------|----------------|-------|----------|
| Trial division | Deterministic | O(sqrt(n)) | None | Small numbers (< 10^12) |
| Fermat | Probabilistic | O(k log n) | Carmichael numbers | Historical only |
| Miller-Rabin | Probabilistic | O(k log n) | < (1/4)^k | Cryptographic key generation |
| AKS | Deterministic | O(log^6 n) | None | Theoretical |
| ECPP | Deterministic | O(log^6 n) | None | Certificates of primality |
| Lucas-Lehmer | Deterministic | O(log^2 n) | None | Mersenne primes only |

### Cryptographic Hash Functions

A cryptographic hash function H maps arbitrary-length input to a fixed-length output (the digest):

$$
H: \{0, 1\}^* \to \{0, 1\}^n
$$

```python
import hashlib

# SHA-256 produces 256-bit (32-by) digests
message = b"Hello, world!"
digest = hashlib.sha256(message).hexdigest()
print(f"SHA-256 of '{message.decode()}':")
print(f"  {digest}")
print(f"  Length: {len(digest) * 4} bits")  # 256 bits
```

**Common hash functions:**

| Algorithm | Output Size | Internal State | Security Level | Status |
|-----------|-------------|----------------|---------------|--------|
| MD5 | 128 bits | 128 bits | < 64 bits | Broken (collisions found) |
| SHA-1 | 160 bits | 160 bits | < 80 bits | Broken (SHAttered attack) |
| SHA-256 | 256 bits | 256 bits | 128 bits | Recommended |
| SHA-512 | 512 bits | 512 bits | 256 bits | Recommended |
| SHA-3-256 | 256 bits | 1600 bits | 128 bits | Recommended |
| BLAKE2b | 256-512 bits | 512 bits | 128-256 bits | Recommended |
| BLAKE3 | 256 bits (configurable) | 512 bits | 128 bits | Emerging |

### Properties of Hash Functions

A cryptographic hash function must satisfy:

**1. Preimage resistance (one-wayness):** Given y, it is computationally infeasible to find any x such that H(x) = y.

**2. Second preimage resistance (weak collision resistance):** Given x, it is computationally infeasible to find x' != x such that H(x) = H(x').

**3. Collision resistance (strong collision resistance):** It is computationally infeasible to find any distinct x, x' such that H(x) = H(x').

```python
# Demonstration: even small changes produce completely different digests
m1 = b"The quick brown fox jumps over the lazy dog"
m2 = b"The quick brown fox jumps over the lazy dog."

d1 = hashlib.sha256(m1).hexdigest()
d2 = hashlib.sha256(m2).hexdigest()

print(f"d1: {d1}")
print(f"d2: {d2}")
print(f"First byte differs: {d1[0] != d2[0]}")
```

**Avalanche effect:** Changing one bit of input changes approximately half of the output bits. This is a key property of any secure hash function.

### SHA-256 Structure

SHA-256 processes messages in 512-bit (64-byte) blocks using the Merkle-Damgard construction.

**Padding:** Append 1 bit, then zeros, then a 64-bit length field to reach a multiple of 512 bits.

**Compression function:**

```
For each 512-bit block:
1. Expand 16 words to 64 words using message schedule
2. Initialize working variables from hash state
3. Apply 64 rounds of compression using the round function
4. Add result to hash state
```

```python
# SHA-256 processes in 64-byte blocks
small_msg = b"a" * 55
boundary_msg = b"a" * 56
print(f"55 bytes: {hashlib.sha256(small_msg).hexdigest()}")
print(f"56 bytes: {hashlib.sha256(boundary_msg).hexdigest()}")
```

### SHA-3 (Keccak) Overview

SHA-3 is based on the Keccak sponge construction, which differs fundamentally from SHA-2's Merkle-Damgard construction.

**Sponge construction:**

1. **Absorb phase:** XOR input blocks into the state, then apply the permutation f
2. **Squeeze phase:** Extract output blocks from the state, applying f between reads

```
State size = 1600 bits (for SHA-3)
Rate + Capacity = 1600
Rate = 1088 bits for SHA-3-256
Capacity = 512 bits for SHA-3-256 (security level = capacity/2)
```

```python
# SHA-3 usage in Python
digest = hashlib.sha3_256(b"Hello, world!").hexdigest()
print(f"SHA3-256: {digest}")

# SHAKE (extendable output functions)
shake = hashlib.shake_128(b"Hello, world!")
print(f"SHAKE-128 (16 bytes): {shake.hexdigest(16)}")
print(f"SHAKE-128 (32 bytes): {shake.hexdigest(32)}")
```

**Key differences SHA-2 vs SHA-3:**

| Property | SHA-2 (Merkle-Damgard) | SHA-3 (Sponge) |
|----------|----------------------|-----------------|
| Structure | MD with Davies-Meyer | Sponge construction |
| State size | 256-512 bits | 1600 bits |
| Susceptible to length extension | Yes | No |
| Security proof | Weaker | Stronger |
| Performance (software) | Faster | Slower |
| Hardware implementation | Moderate | Efficient |

### Collision Resistance and the Birthday Attack

The birthday attack finds collisions in a hash function with effort O(2^{n/2}) (much less than the expected O(2^n) by brute force).

**The birthday paradox:** In a group of 23 people, there is a > 50% chance that two share a birthday. Generalizing: to find a collision in an n-bit hash function, we expect to find one after approximately 2^{n/2} evaluations.

```python
import random

def birthday_attack(hash_func, n_bits: int, max_attempts: int = 10**6):
    """
    Find a collision in a hash function using birthday attack.
    
    Args:
        hash_func: Function mapping bytes to int
        n_bits: Bit length of hash output
        max_attempts: Maximum hash evaluations
    
    Returns:
        (m1, m2) such that hash(m1) == hash(m2) and m1 != m2, or None
    """
    seen = {}
    for i in range(max_attempts):
        msg = random.randbytes(128)
        h = hash_func(msg)
        if h in seen:
            m1 = seen[h]
            m2 = msg
            if m1 != m2:
                return (m1, m2)
        seen[h] = msg
    return None

# Demonstrate on truncated hash (16 bits for quick demo)
def truncated_hash(msg: bytes, bits: int = 16) -> int:
    full = hashlib.sha256(msg).digest()
    mask = (1 << bits) - 1
    return int.from_bytes(full[: (bits + 7) // 8], 'big') & mask

# This will find a collision quickly with only 16 bits
result = birthday_attack(lambda m: truncated_hash(m, 16), 16, 50000)
if result:
    m1, m2 = result
    h1 = truncated_hash(m1, 16)
    h2 = truncated_hash(m2, 16)
    print(f"Collision found!")
    print(f"  H(m1) = H(m2) = {h1:04x}")
    print(f"  Evaluations needed: ~2^8 = 256 (theoretical minimum)")
```

**Security implications:**

| Hash | Output Bits | Birthday Bound (2^{n/2}) | Security Status |
|------|-------------|------------------------|-----------------|
| MD5 | 128 | 2^64 | Broken (practical collisions at 2^18) |
| SHA-1 | 160 | 2^80 | Broken (SHAttered at 2^63) |
| SHA-256 | 256 | 2^128 | Safe for foreseeable future |
| SHA-512 | 512 | 2^256 | Safe for foreseeable future |

The SHAttered attack (2017) produced the first practical SHA-1 collision:

```python
# The two SHAttered PDFs have the same SHA-1 hash
# https://shattered.io/
# sha1(D0V3.pdf) = sha1(D0V4.pdf) = 38762cf7f55934b34d179ae6a4c80cadccbb7f0a
```

### Checksums and Error Detection

Non-cryptographic checksums detect accidental errors (transmission noise, storage corruption) but are not secure against malicious modification.

```python
# Simple checksums
def parity_byte(data: bytes) -> int:
    """Simple XOR checksum (not cryptographically secure)."""
    result = 0
    for byte in data:
        result ^= byte
    return result

def simple_mod_checksum(data: bytes, modulus: int = 256) -> int:
    """Modular sum checksum."""
    return sum(data) % modulus

# CRC-32 (Cyclic Redundancy Check)
import zlib
def crc32_checksum(data: bytes) -> int:
    """CRC-32 checksum."""
    return zlib.crc32(data)

data = b"Hello, World!"
print(f"XOR parity: {parity_byte(data):02x}")
print(f"Mod 256 sum: {simple_mod_checksum(data):02x}")
print(f"CRC-32: {crc32_checksum(data):08x}")
```

**Comparison of error detection methods:**

| Method | Output Bits | Collision Resistance | Speed | Use Case |
|--------|-------------|---------------------|-------|----------|
| Parity bit | 1 | None (50% detection) | Very fast | RAM, serial |
| XOR checksum | 8-32 | Very weak | Very fast | Simple protocols |
| CRC-32 | 32 | Weak (malicious) | Fast | Network (Ethernet), storage |
| MD5 | 128 | Broken | Fast | Legacy file integrity |
| SHA-256 | 256 | Strong | Moderate | Cryptographic integrity |

## Glossary

| Term | Definition |
|------|------------|
| Primality test | Algorithm to determine if a number is prime |
| Trial division | Testing divisibility by all primes up to sqrt(n) |
| Fermat test | Probabilistic test using Fermat little theorem |
| Miller-Rabin test | Probabilistic test based on non-trivial square roots of 1 |
| Carmichael number | Composite n where a^{n-1} = 1 (mod n) for all a coprime to n |
| AKS test | First deterministic polynomial-time primality test |
| Witness | Integer a that proves n is composite in a primality test |
| Hash function | Maps arbitrary input to fixed-size output |
| Preimage resistance | Given y, infeasible to find x with H(x) = y |
| Collision resistance | Infeasible to find x != y with H(x) = H(y) |
| Avalanche effect | Small input change causes large output change |
| Merkle-Damgard | Construction used by MD5, SHA-1, SHA-2 |
| Sponge construction | Construction used by SHA-3 (Keccak) |
| Birthday attack | Finding collisions in O(2^{n/2}) time |
| Birthday paradox | Probability of shared birthdays exceeds 50% with 23 people |
| SHA-256 | 256-bit hash function in the SHA-2 family |
| SHA-3 | Latest NIST hash standard using Keccak sponge |
| HMAC | Keyed-hash message authentication code |
| Salt | Random value added to passwords before hashing |
| PBKDF2 | Password-based key derivation function |
| Merkle tree | Tree of hashes for efficient data integrity verification |
| CRC | Cyclic Redundancy Check for error detection |
| Mersenne prime | Prime of the form 2^p - 1 |
| Lucas-Lehmer test | Efficient test for Mersenne primes |

## Quick References

- [NIST FIPS 186-5: Digital Signature Standard](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-5.pdf) -- includes DSA, ECDSA, EdDSA
- [NIST FIPS 180-4: Secure Hash Standard](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf) -- SHA-1, SHA-2 specification
- [NIST FIPS 202: SHA-3 Standard](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.202.pdf) -- SHA-3 specification
- [PKCS #1 v2.2: RSA Cryptography Standard](https://tools.ietf.org/html/rfc8017) -- RSA including prime generation
- [SHAttered.io](https://shattered.io/) -- practical SHA-1 collision
- [Alfred J. Menezes et al.: Handbook of Applied Cryptography](http://cacr.uwaterloo.ca/hac/) -- comprehensive reference
- [Prime Numbers: A Computational Perspective](https://link.springer.com/book/10.1007/978-0-387-28979-5) -- Crandall and Pomerance

## Next Steps

This is the final document in the Number Theory module. Review the [Number Theory Index](index.md) for a complete topic map. Recommended next subjects:

- [Discrete Mathematics: Combinatorics](discrete-mathematics/index.md) -- counting, permutations, combinations
- [Probability: Randomness and Distributions](probability-and-statistics/index.md) -- randomness, distributions, statistical tests
- [Systems Design: Cryptography](systems-design/index.md) -- system-level cryptographic architecture
