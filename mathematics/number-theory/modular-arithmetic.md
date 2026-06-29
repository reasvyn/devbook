# Modular Arithmetic

## Description

Modular arithmetic -- the mathematics of remainders -- is one of the most practically useful branches of number theory for software developers. Every use of the modulo operator, every cryptographic algorithm, every hash function, and every cyclic data structure relies on modular arithmetic. This document covers the congruence relation, modular operations, residue systems, modular inverses, fast exponentiation, and practical programming concerns like negative numbers and overflow.

## Prerequisites

- [Divisibility and Primes](divisibility-and-primes.md) -- division algorithm, GCD, Euclidean algorithm as foundations

## Table of Contents

- [The Congruence Relation](#the-congruence-relation)
- [Basic Properties of Congruences](#basic-properties-of-congruences)
- [Modular Addition and Subtraction](#modular-addition-and-subtraction)
- [Modular Multiplication](#modular-multiplication)
- [Modular Exponentiation](#modular-exponentiation)
- [Fast Exponentiation (Exponentiation by Squaring)](#fast-exponentiation-exponentiation-by-squaring)
- [Complete Residue Systems](#complete-residue-systems)
- [Reduced Residue Systems](#reduced-residue-systems)
- [Modular Inverses](#modular-inverses)
- [The Modulo Operator in Programming](#the-modulo-operator-in-programming)
- [Modular Arithmetic with Negative Numbers](#modular-arithmetic-with-negative-numbers)
- [Avoiding Overflow in Modular Arithmetic](#avoiding-overflow-in-modular-arithmetic)
- [Applications in Programming](#applications-in-programming)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Congruence Relation

Two integers $$a$$ and $$b$$ are congruent modulo $$m$$ if their difference is divisible by $$m$$. We write:

$$
a \equiv b \pmod{m} \quad \text{iff} \quad m \mid (a - b)
$$

The integer $$m$$ is called the modulus.

**Examples:**

- $$17 \equiv 5 \pmod{12}$$ because $$17 - 5 = 12$$
- $$100 \equiv 1 \pmod{99}$$ because $$100 - 1 = 99$$
- $$-3 \equiv 2 \pmod{5}$$ because $$-3 - 2 = -5$$

In programming, checking congruence uses the modulo operator:

```python
def is_congruent(a: int, b: int, m: int) -> bool:
    """Return True if a is congruent to b modulo m."""
    return (a - b) % m == 0

print(is_congruent(17, 5, 12))   # True
print(is_congruent(100, 1, 99))  # True
print(is_congruent(17, 6, 12))   # False
```

The congruence relation is an equivalence relation:

| Property | Statement |
|----------|-----------|
| Reflexive | $$a \equiv a \pmod{m}$$ |
| Symmetric | If $$a \equiv b \pmod{m}$$ then $$b \equiv a \pmod{m}$$ |
| Transitive | If $$a \equiv b \pmod{m}$$ and $$b \equiv c \pmod{m}$$ then $$a \equiv c \pmod{m}$$ |

### Basic Properties of Congruences

If $$a \equiv b \pmod{m}$$ and $$c \equiv d \pmod{m}$$, then:

| Operation | Rule |
|-----------|------|
| Addition | $$a + c \equiv b + d \pmod{m}$$ |
| Subtraction | $$a - c \equiv b - d \pmod{m}$$ |
| Multiplication | $$a \cdot c \equiv b \cdot d \pmod{m}$$ |
| Exponentiation | $$a^k \equiv b^k \pmod{m}$$ for any $$k \ge 0$$ |

**Proof of addition:**

If $$a \equiv b \pmod{m}$$, then $$a - b = m k_1$$. If $$c \equiv d \pmod{m}$$, then $$c - d = m k_2$$. Adding:

$$
(a + c) - (b + d) = (a - b) + (c - d) = m(k_1 + k_2)
$$

Therefore $$a + c \equiv b + d \pmod{m}$$.

**Important warning:** Division is not always defined in modular arithmetic. Unlike the other operations, $$a \equiv b \pmod{m}$$ does not imply $$a/c \equiv b/c \pmod{m}$$. Division corresponds to multiplication by a modular inverse, which exists only when the divisor is coprime to the modulus.

### Modular Addition and Subtraction

Addition and subtraction modulo m simply involve computing (a + b) mod m and (a - b) mod m.

```python
def mod_add(a: int, b: int, m: int) -> int:
    """Return (a + b) mod m."""
    return (a + b) % m

def mod_sub(a: int, b: int, m: int) -> int:
    """Return (a - b) mod m."""
    return (a - b) % m

print(mod_add(17, 25, 12))  # 6  (17 + 25 = 42, 42 mod 12 = 6)
print(mod_sub(5, 17, 12))   # 0  (5 - 17 = -12, -12 mod 12 = 0)
```

These operations form a finite group -- the set Z_m = {0, 1, 2, ..., m-1} under addition modulo m is closed, associative, has an identity (0), and every element has an additive inverse.

### Modular Multiplication

Multiplication modulo m is equally straightforward:

```python
def mod_mul(a: int, b: int, m: int) -> int:
    """Return (a * b) mod m."""
    return (a * b) % m

print(mod_mul(7, 8, 12))  # 8  (7 * 8 = 56, 56 mod 12 = 8)
print(mod_mul(11, 3, 12))  # 9  (11 * 3 = 33, 33 mod 12 = 9)
```

The set Z_m is a commutative ring under addition and multiplication. However, not every element has a multiplicative inverse.

**Multiplication tables modulo 6:**

```
    0  1  2  3  4  5
0   0  0  0  0  0  0
1   0  1  2  3  4  5
2   0  2  4  0  2  4
3   0  3  0  3  0  3
4   0  4  2  0  4  2
5   0  5  4  3  2  1
```

Notice that 2 has no multiplicative inverse modulo 6 because gcd(2, 6) = 2 != 1. The elements with inverses (1 and 5) are those coprime to 6.

### Modular Exponentiation

Computing a^b mod m is a fundamental operation in cryptography. RSA encryption, Diffie-Hellman key exchange, and primality testing all rely on efficient modular exponentiation.

**Naive approach (inefficient for large exponents):**

```python
def mod_pow_naive(a: int, b: int, m: int) -> int:
    """Compute a^b mod m by repeated multiplication."""
    result = 1
    for _ in range(b):
        result = (result * a) % m
    return result

print(mod_pow_naive(3, 5, 7))  # 5  (3^5 = 243, 243 mod 7 = 5)
```

This approach is O(b) and unusable for b = 2^256 (common in cryptography).

### Fast Exponentiation (Exponentiation by Squaring)

Fast exponentiation -- also called exponentiation by squaring -- computes a^b mod m in O(log b) time.

**Algorithm:**

To compute a^b, observe:

$$
a^b = \begin{cases}
1 & \text{if } b = 0 \\
(a^{b/2})^2 & \text{if } b \text{ is even} \\
a \cdot a^{b-1} & \text{if } b \text{ is odd}
\end{cases}
$$

**Recursive implementation:**

```python
def mod_pow_recursive(a: int, b: int, m: int) -> int:
    """Fast modular exponentiation (recursive)."""
    if b == 0:
        return 1
    half = mod_pow_recursive(a, b // 2, m)
    half = (half * half) % m
    if b % 2 == 1:
        half = (half * a) % m
    return half

print(mod_pow_recursive(3, 5, 7))    # 5
print(mod_pow_recursive(2, 10, 1000)) # 24
```

**Iterative implementation (more common in practice):**

```python
def mod_pow(a: int, b: int, m: int) -> int:
    """Fast modular exponentiation (iterative)."""
    result = 1
    base = a % m
    while b > 0:
        if b & 1:  # if b is odd
            result = (result * base) % m
        base = (base * base) % m
        b >>= 1   # b = b // 2
    return result

print(mod_pow(3, 5, 7))    # 5
print(mod_pow(5, 117, 19)) # 1
```

Python's built-in `pow(a, b, m)` function is highly optimized and should be preferred in production code:

```python
result = pow(3, 5, 7)   # 5
result = pow(5, 117, 19) # 1
```

### Complete Residue Systems

A complete residue system modulo m is a set of m integers that covers every residue class exactly once. The standard complete residue system is:

$$
\{0, 1, 2, \ldots, m-1\}
$$

Any set of m integers that are pairwise incongruent modulo m forms a complete residue system.

**Examples for m = 5:**

- Standard: {0, 1, 2, 3, 4}
- Alternative: {-2, -1, 0, 1, 2}
- Valid: {10, 21, 32, 43, 54} (all congruent to 0, 1, 2, 3, 4 mod 5)

```python
def is_complete_residue_system(nums: list[int], m: int) -> bool:
    """Check if nums forms a complete residue system modulo m."""
    return len(set(n % m for n in nums)) == m

print(is_complete_residue_system([-2, -1, 0, 1, 2], 5))  # True
print(is_complete_residue_system([0, 1, 2, 3, 5], 5))    # False
```

### Reduced Residue Systems

A reduced residue system modulo m is a set of integers that are each coprime to m and whose residues are pairwise distinct modulo m. Its size is phi(m) (Euler totient function).

**Examples:**

- m = 6: reduced system {1, 5} (phi(6) = 2)
- m = 10: reduced system {1, 3, 7, 9} (phi(10) = 4)
- m = 12: reduced system {1, 5, 7, 11} (phi(12) = 4)

```python
def reduced_residue_system(m: int) -> list[int]:
    """Return a reduced residue system modulo m."""
    from math import gcd
    return [a for a in range(m) if gcd(a, m) == 1]

print(reduced_residue_system(6))   # [1, 5]
print(reduced_residue_system(10))  # [1, 3, 7, 9]
print(reduced_residue_system(12))  # [1, 5, 7, 11]
```

Reduced residue systems are central to Euler theorem, which states that if gcd(a, m) = 1 then a^{phi(m)} = 1 (mod m).

### Modular Inverses

The modular inverse of a modulo m is an integer a^{-1} such that:

$$
a \cdot a^{-1} \equiv 1 \pmod{m}
$$

A modular inverse exists iff gcd(a, m) = 1.

**Computing the modular inverse using the extended Euclidean algorithm:**

```python
def extended_gcd(a: int, b: int) -> tuple[int, int, int]:
    """Return (gcd, x, y) such that a*x + b*y = gcd."""
    if b == 0:
        return (abs(a), 1 if a > 0 else -1, 0)
    g, x1, y1 = extended_gcd(b, a % b)
    return (g, y1, x1 - (a // b) * y1)

def mod_inverse(a: int, m: int) -> int | None:
    """Return the modular inverse of a modulo m, or None."""
    g, x, _ = extended_gcd(a, m)
    if g != 1:
        return None
    return x % m

print(mod_inverse(3, 10))   # 7  (3 * 7 = 21 = 2*10 + 1)
print(mod_inverse(17, 43))  # 38 (17 * 38 = 646 = 15*43 + 1)
print(mod_inverse(2, 6))    # None (gcd(2, 6) = 2)
```

**Using Python built-in pow:**

```python
inv = pow(3, -1, 10)  # 7 (Python 3.8+)
print(inv)
```

**Alternative: Fermat little theorem approach:**

If m is prime and a != 0 (mod m):

$$
a^{-1} \equiv a^{m-2} \pmod{m}
$$

```python
def mod_inverse_fermat(a: int, p: int) -> int:
    """Modular inverse using Fermat little theorem (p must be prime)."""
    return pow(a, p - 2, p)

print(mod_inverse_fermat(3, 7))   # 5
print(mod_inverse_fermat(2, 13))  # 7
```

### The Modulo Operator in Programming

Different programming languages handle the modulo operator differently for negative numbers:

```python
# Python: result has the same sign as the divisor (always non-negative)
print(7 % 3)    # 1
print(-7 % 3)   # 2  (not -1!)
print(7 % -3)   # -2 (Python-specific behavior)
```

```javascript
// JavaScript: result has the same sign as the dividend
console.log(7 % 3);   // 1
console.log(-7 % 3);  // -1
console.log(7 % -3);  // 1
```

| Language | -7 mod 3 | Sign Convention |
|----------|----------|-----------------|
| Python | 2 | Divisor (always non-negative when divisor > 0) |
| JavaScript | -1 | Dividend |
| C99+ / C++ | -1 | Truncated toward zero |
| Java | -1 | Dividend |
| Rust (%) | -1 | Dividend |
| Rust (rem_euclid) | 2 | Divisor (mathematical) |
| Go | -1 | Dividend |

**Mathematically correct modulo** always returns a value in [0, m-1]. Normalize:

```python
def mod_math(a: int, m: int) -> int:
    """Mathematically correct modulo, always in [0, m-1]."""
    return ((a % m) + m) % m

print(mod_math(-7, 3))  # 2
```

### Modular Arithmetic with Negative Numbers

Always normalize negative intermediate results:

```python
def mod_sub_safe(a: int, b: int, m: int) -> int:
    """Safe modular subtraction handling negatives."""
    return (a % m - b % m) % m

print(mod_sub_safe(3, 7, 10))  # 6
```

### Avoiding Overflow in Modular Arithmetic

When computing (a * b) mod m for very large numbers, use Russian peasant multiplication:

```python
def mod_mul_safe(a: int, b: int, m: int) -> int:
    """Compute (a * b) % m without overflow, using repeated doubling."""
    result = 0
    a = a % m
    while b > 0:
        if b & 1:
            result = (result + a) % m
        a = (a + a) % m
        b >>= 1
    return result

print(mod_mul_safe(10**18, 10**18, 10**9 + 7))  # 49
```

In Python, arbitrary-precision integers make this less critical, but the technique is important for languages with 64-bit or 32-bit integer limits.

### Applications in Programming

**Cyclic data structures:**

```python
# Circular buffer indexing
buffer = [None] * 8
write_index = 0
for i in range(20):
    buffer[write_index] = f"data_{i}"
    write_index = (write_index + 1) % len(buffer)
print(buffer)  # Last 8 items
```

**Hash table indexing:**

```python
def hash_index(key: str, table_size: int) -> int:
    """Simple hash function using modular arithmetic."""
    h = 0
    for char in key:
        h = (h * 31 + ord(char)) % table_size
    return h

print(hash_index("hello", 16))  # 14
```

**Checksums and error detection:**

```python
def simple_checksum(data: bytes) -> int:
    """Compute a simple modular checksum."""
    return sum(data) % 256

data = b"Hello, World!"
print(simple_checksum(data))  # 93
```

**Cryptography (RSA):**

```python
p, q = 61, 53
n = p * q
e = 17
d = 2753

def rsa_encrypt(m: int, n: int, e: int) -> int:
    return pow(m, e, n)

def rsa_decrypt(c: int, n: int, d: int) -> int:
    return pow(c, d, n)

plaintext = 42
ciphertext = rsa_encrypt(plaintext, n, e)
decrypted = rsa_decrypt(ciphertext, n, d)
print(f"Plain: {plaintext}, Cipher: {ciphertext}, Decrypted: {decrypted}")
```

## Study Cases

### Case 1: Finding the Day of the Week

Using Zeller congruence:

```python
def day_of_week(year: int, month: int, day: int) -> str:
    """Return the day of week for a given date."""
    days = ["Saturday", "Sunday", "Monday", "Tuesday", "Wednesday",
            "Thursday", "Friday"]
    if month < 3:
        month += 12
        year -= 1
    K = year % 100
    J = year // 100
    h = (day + (13 * (month + 1)) // 5 + K + K // 4 + J // 4 + 5 * J) % 7
    return days[h]

print(day_of_week(2024, 12, 25))  # Wednesday
```

### Case 2: Large Fibonacci Modulo m

Compute the nth Fibonacci number modulo m to prevent explosive growth:

```python
def fib_mod(n: int, m: int) -> int:
    """Return the nth Fibonacci number modulo m."""
    if n == 0:
        return 0
    a, b = 0, 1
    for _ in range(n - 1):
        a, b = b, (a + b) % m
    return b

print(fib_mod(1000, 1000))       # 875
```

For very large n, use fast doubling (O(log n)):

```python
def fib_mod_fast(n: int, m: int) -> int:
    """Return nth Fibonacci modulo m using fast doubling."""
    if n == 0:
        return 0
    def fib_pair(k: int):
        if k == 0:
            return (0, 1)
        a, b = fib_pair(k >> 1)
        c = (a * ((2 * b - a) % m)) % m
        d = (a * a + b * b) % m
        if k & 1:
            return (d, (c + d) % m)
        return (c, d)
    return fib_pair(n)[0]

print(fib_mod_fast(10**12, 10**9 + 7))  # 21
```

### Case 3: Implementing a Ring Buffer

```python
class RingBuffer:
    """Fixed-size ring buffer using modular arithmetic."""
    def __init__(self, capacity: int):
        self.buffer = [None] * capacity
        self.capacity = capacity
        self.head = 0
        self.tail = 0
        self.full = False

    def write(self, item) -> None:
        self.buffer[self.head] = item
        if self.full:
            self.tail = (self.tail + 1) % self.capacity
        self.head = (self.head + 1) % self.capacity
        self.full = self.head == self.tail

    def read(self):
        if self.is_empty():
            raise IndexError("Buffer is empty")
        item = self.buffer[self.tail]
        self.tail = (self.tail + 1) % self.capacity
        self.full = False
        return item

    def is_empty(self) -> bool:
        return not self.full and self.head == self.tail

buf = RingBuffer(3)
buf.write(1); buf.write(2); buf.write(3)
print(buf.read())  # 1
buf.write(4)
print(buf.read())  # 2
```

## Examples

### Example 1: Computing 7^1000 mod 13 by Hand

Using Fermat little theorem: phi(13) = 12 so 7^12 = 1 (mod 13).

$$
7^{1000} = 7^{12 \cdot 83 + 4} = (7^{12})^{83} \cdot 7^4 \equiv 1^{83} \cdot 7^4 \equiv 2401 \equiv 9 \pmod{13}
$$

```python
print(pow(7, 1000, 13))  # 9
```

### Example 2: Modular Arithmetic for Wrapping Integers

```python
def wrap_add(a: int, b: int, low: int, high: int) -> int:
    """Add b to a, wrapping within [low, high]."""
    range_len = high - low + 1
    return ((a - low + b) % range_len) + low

print(wrap_add(8, 5, 1, 10))   # 2
print(wrap_add(3, -5, 1, 10))  # 9
```

### Example 3: Computing Chinese Remainder

```python
# Solve x = 2 (mod 3), x = 3 (mod 5), x = 2 (mod 7)
M = 3 * 5 * 7
x = 0
x += 2 * (M // 3) * pow(M // 3, -1, 3)
x += 3 * (M // 5) * pow(M // 5, -1, 5)
x += 2 * (M // 7) * pow(M // 7, -1, 7)
print(x % M)  # 23
```

### Example 4: Montgomery Reduction Concept

```python
def montgomery_reduce(a: int, b: int, m: int, r: int) -> int:
    """Simple illustration of Montgomery reduction concept."""
    r_inv = pow(r, -1, m)
    return (a * b * r_inv) % m

m = 97
r = 100
a, b = 42, 33
print(montgomery_reduce(a, b, m, r))
print((a * b) % m)  # Should match
```

### Example 5: Multiplicative Order

```python
def multiplicative_order(a: int, m: int) -> int:
    """Return the smallest k > 0 such that a^k = 1 (mod m)."""
    from math import gcd
    if gcd(a, m) != 1:
        return -1
    for k in range(1, m):
        if pow(a, k, m) == 1:
            return k
    return -1

print(multiplicative_order(3, 7))   # 6
print(multiplicative_order(2, 7))   # 3
```

## Glossary

| Term | Definition |
|------|------------|
| Congruence | a = b (mod m) means m divides (a - b) |
| Modulus | The integer m in modular arithmetic |
| Residue class | The set of all integers congruent to a given a modulo m |
| Complete residue system | A set of m integers, each from a different residue class modulo m |
| Reduced residue system | A set of phi(m) integers coprime to m, each from a different residue class |
| Modular inverse | a^{-1} such that a * a^{-1} = 1 (mod m) |
| Fast exponentiation | Computing a^b mod m in O(log b) time using squaring |
| Exponentiation by squaring | Algorithm that halves the exponent each iteration |
| Finite group | Algebraic structure with closure, associativity, identity, and inverses |
| Ring | Algebraic structure with addition (group) and multiplication (semigroup) |
| Euler totient | phi(m) = count of integers 1 <= k <= m with gcd(k, m) = 1 |
| Fermat little theorem | For prime p and a != 0 (mod p): a^{p-1} = 1 (mod p) |

## Quick References

- [Modular arithmetic on Wikipedia](https://en.wikipedia.org/wiki/Modular_arithmetic)
- [Python pow function documentation](https://docs.python.org/3/library/functions.html#pow)
- [Fast modular exponentiation (Khan Academy)](https://www.khanacademy.org/computing/computer-science/cryptography/modarithmetic/a/fast-modular-exponentiation)

## Next Steps

- [Euler & Fermat](euler-fermat.md) -- Euler theorem, Fermat little theorem, and the totient function
- [Chinese Remainder Theorem](chinese-remainder-theorem.md) -- solving systems of congruences
