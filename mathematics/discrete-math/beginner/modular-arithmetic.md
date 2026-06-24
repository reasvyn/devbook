# Modular Arithmetic

## Description

Modular arithmetic (clock arithmetic) performs math on a finite set of numbers that "wrap around" after reaching a modulus. It is the mathematical foundation of cryptography, hash functions, checksums, and many algorithmic techniques like circular buffers and hash tables.

## Prerequisites

- [Integers](../../number-systems/beginner/integers.md)
- [Set Theory](set-theory.md)

## Table of Contents

- [The Modulo Operation](#the-modulo-operation)
- [Congruence](#congruence)
- [Arithmetic Modulo n](#arithmetic-modulo-n)
- [Modular Inverses](#modular-inverses)
- [Applications](#applications)

## Content / Material

### The Modulo Operation

The modulo operation finds the remainder after division:

$$ 17 \bmod 5 = 2 \quad \text{(17 ÷ 5 = 3 remainder 2)} $$

```python
17 % 5   # 2
-7 % 3   # 2 (in Python, result is non-negative)
```

### Congruence

Two numbers are congruent modulo $n$ if they have the same remainder:

$$ a \equiv b \pmod{n} \iff n \mid (a - b) $$

```
17 ≡ 2 (mod 5)
22 ≡ 2 (mod 5)
-3 ≡ 2 (mod 5)
```

All of these have remainder 2 when divided by 5.

### Arithmetic Modulo n

Modular arithmetic works like regular arithmetic, but results wrap around:

$$ (a + b) \bmod n = ((a \bmod n) + (b \bmod n)) \bmod n $$

$$ (a \times b) \bmod n = ((a \bmod n) \times (b \bmod n)) \bmod n $$

```python
# Clock arithmetic: 7 hours after 10 o'clock
(10 + 7) % 12  # 5 o'clock

# Prevent overflow in large computations
result = (a * b) % n  # compute step by step for large numbers
```

### Modular Inverses

In regular arithmetic, the inverse of $a$ is $1/a$. In modular arithmetic, the inverse of $a$ modulo $n$ is a number $b$ such that:

$$ a \times b \equiv 1 \pmod{n} $$

An inverse exists only if $\gcd(a, n) = 1$ (a and n are coprime).

```python
# Extended Euclidean algorithm — find modular inverse
def mod_inverse(a, m):
    """Find x such that (a * x) % m = 1"""
    def egcd(a, b):
        if b == 0: return (1, 0, a)
        x, y, g = egcd(b, a % b)
        return (y, x - (a // b) * y, g)
    x, _, g = egcd(a, m)
    if g != 1: return None  # no inverse
    return x % m

print(mod_inverse(3, 10))  # 7, because 3 * 7 = 21 ≡ 1 (mod 10)
```

### Applications

```python
# Circular buffer
index = (index + 1) % buffer_size

# Hash table: index = hash(key) % table_size

# Even/odd check
is_even = (n % 2 == 0)

# Checksum (ISBN-10)
# The last digit makes the sum divisible by 11

# RSA encryption: c = m^e mod n
```

| Application | How Modulo Is Used |
|-------------|-------------------|
| **Hash tables** | Map hash to array index |
| **Cryptography** | RSA, Diffie-Hellman, ECC |
| **Checksums** | Verify data integrity |
| **Circular buffers** | Wrap around fixed-size buffer |
| **Calendar arithmetic** | Days of the week, date offsets |
| **Random number generation** | Linear congruential generators |

## Glossary

| Term | Definition |
|------|------------|
| Modulus | The number $n$ that defines the wrap-around point |
| Congruence | $a \equiv b \pmod{n}$ means $a$ and $b$ have the same remainder when divided by $n$ |
| Modular inverse | A number $b$ such that $a \times b \equiv 1 \pmod{n}$ |
| Coprime | Two numbers whose greatest common divisor is 1 |

## Next Steps

- [Graph Theory](graph-theory.md) — graphs as discrete structures
