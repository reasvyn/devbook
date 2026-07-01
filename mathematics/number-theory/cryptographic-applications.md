# Cryptographic Applications

## Description

Number theory is the mathematical foundation of modern public-key cryptography. This document brings together RSA encryption and signing, Diffie-Hellman key exchange, ElGamal encryption, elliptic curve cryptography basics, digital signatures, and practical considerations like key sizes, padding schemes, and implementation pitfalls. Every concept is connected to its number-theoretic roots in prime numbers, modular arithmetic, and discrete logarithms.

## Prerequisites

- [Discrete Logarithms](discrete-logarithms.md) -- primitive roots, DLP, Diffie-Hellman foundations
- [Euler & Fermat](euler-fermat.md) -- Euler theorem, totient function, modular exponentiation

## Table of Contents

- [RSA Overview](#rsa-overview)
- [RSA Key Generation](#rsa-key-generation)
- [RSA Encryption and Decryption](#rsa-encryption-and-decryption)
- [RSA Signing and Verification](#rsa-signing-and-verification)
- [RSA Security and Padding](#rsa-security-and-padding)
- [Diffie-Hellman Key Exchange](#diffie-hellman-key-exchange)
- [ElGamal Encryption](#elgamal-encryption)
- [Elliptic Curve Cryptography (Basic Concepts)](#elliptic-curve-cryptography-basic-concepts)
- [Digital Signatures](#digital-signatures)
- [Key Sizes and Security Levels](#key-sizes-and-security-levels)
- [Practical Considerations](#practical-considerations)
- [Implementation Pitfalls](#implementation-pitfalls)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### RSA Overview

RSA (Rivest-Shamir-Adleman, 1977) is the most widely deployed public-key cryptosystem. Its security rests on the difficulty of factoring the product of two large primes.

**Core idea:** Computing c = m^e mod n is easy (modular exponentiation). Given c, finding m without knowing the private exponent d requires factoring n, which is believed to be hard for sufficiently large n.

### RSA Key Generation

```python
import random
from math import gcd

def generate_large_prime(bits: int) -> int:
    """Generate a probable prime of specified bit length."""
    while True:
        # Generate random odd number of required bit length
        n = random.getrandbits(bits)
        n |= (1 << bits - 1) | 1  # Set top bit and make odd
        
        if is_probable_prime(n):
            return n

def is_probable_prime(n: int, k: int = 20) -> bool:
    """Miller-Rabin probabilistic primality test."""
    if n < 2: return False
    if n == 2 or n == 3: return True
    if n % 2 == 0: return False
    
    r, d = 0, n - 1
    while d % 2 == 0:
        r += 1
        d //= 2
    
    for _ in range(k):
        a = random.randint(2, n - 2)
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

def rsa_keygen(bits: int = 2048, e: int = 65537) -> dict:
    """
    Generate RSA key pair.
    
    Args:
        bits: Total bit length of modulus n
        e: Public exponent (commonly 65537 = 2^16 + 1)
    
    Returns:
        dict with public and private keys
    """
    # Generate two primes of half the bit length
    p = generate_large_prime(bits // 2)
    q = generate_large_prime(bits // 2)
    while q == p:
        q = generate_large_prime(bits // 2)
    
    n = p * q
    phi_n = (p - 1) * (q - 1)
    
    # Ensure e is coprime to phi_n
    if gcd(e, phi_n) != 1:
        # Choose different e or regenerate primes
        e = 65537
        while gcd(e, phi_n) != 1:
            e = random.randrange(3, phi_n - 1, 2)
    
    d = pow(e, -1, phi_n)
    
    return {
        "public_key": {"n": n, "e": e},
        "private_key": {"n": n, "d": d},
        "p": p, "q": q,
        "phi_n": phi_n
    }

# Generate a small RSA key for demonstration
keys = rsa_keygen(bits=32)
print(f"p = {keys['p']}")
print(f"q = {keys['q']}")
print(f"n = {keys['public_key']['n']}")
print(f"e = {keys['public_key']['e']}")
print(f"d = {keys['private_key']['d']}")
```

**Key generation steps:**

1. Choose two large primes p and q (e.g., 2048-bit n uses 1024-bit primes)
2. Compute n = p * q
3. Compute phi(n) = (p - 1)(q - 1)
4. Choose e with gcd(e, phi(n)) = 1
5. Compute d = e^{-1} mod phi(n)

**Why 65537 as e?** It is prime, has only two 1-bits (making exponentiation fast), and is large enough to avoid small-exponent attacks.

### RSA Encryption and Decryption

**Encryption:** c = m^e mod n
**Decryption:** m = c^d mod n

```python
def rsa_encrypt(m: int, pub_key: dict) -> int:
    """Encrypt message m using RSA public key."""
    n = pub_key["n"]
    e = pub_key["e"]
    return pow(m, e, n)

def rsa_decrypt(c: int, priv_key: dict) -> int:
    """Decrypt ciphertext c using RSA private key."""
    n = priv_key["n"]
    d = priv_key["d"]
    return pow(c, d, n)

# Test
keys = rsa_keygen(32)
m = 42
c = rsa_encrypt(m, keys["public_key"])
m2 = rsa_decrypt(c, keys["private_key"])
print(f"Plaintext: {m}")
print(f"Ciphertext: {c}")
print(f"Decrypted: {m2}")
print(f"Success: {m == m2}")
```

**Why it works (Euler theorem):**

If gcd(m, n) = 1:
c^d = (m^e)^d = m^{e*d} = m^{k*phi(n)+1} = m * (m^{phi(n)})^k = m * 1^k = m (mod n)

If gcd(m, n) != 1 (m is multiple of p or q), the proof still holds using CRT.

### RSA Signing and Verification

RSA can also provide digital signatures. The signer uses their private key; anyone can verify with the public key.

```python
def rsa_sign(m: int, priv_key: dict) -> int:
    """Sign message m using RSA private key."""
    n = priv_key["n"]
    d = priv_key["d"]
    return pow(m, d, n)

def rsa_verify(m: int, signature: int, pub_key: dict) -> bool:
    """Verify RSA signature."""
    n = pub_key["n"]
    e = pub_key["e"]
    return pow(signature, e, n) == m

# Test
keys = rsa_keygen(32)
m = 42
sig = rsa_sign(m, keys["private_key"])
print(f"Signature: {sig}")
print(f"Verify (correct): {rsa_verify(m, sig, keys['public_key'])}")
print(f"Verify (wrong msg): {rsa_verify(43, sig, keys['public_key'])}")
```

**Important:** In practice, you sign a hash of the message, not the message itself. See PKCS#1 v2.2 for proper signing schemes.

### RSA Security and Padding

**Textbook RSA (no padding) is insecure:**

- **Deterministic:** Same plaintext produces same ciphertext
- **Malleable:** E(m1) * E(m2) = E(m1 * m2)
- **Small messages:** If m < n^{1/e}, m^e < n, so c^{1/e} recovers m

```python
# Small message attack demonstration (textbook RSA)
# If m^e < n, simply take e-th root
def small_message_attack(c: int, e: int) -> int | None:
    """Try to recover small message by taking e-th root."""
    import math
    # Approximate integer e-th root
    low, high = 0, 2 ** (c.bit_length() // e + 1)
    while low <= high:
        mid = (low + high) // 2
        mid_e = mid ** e
        if mid_e == c:
            return mid
        elif mid_e < c:
            low = mid + 1
        else:
            high = mid - 1
    return None

# Demonstrate: small message, small e
n = 100003 * 100019  # large enough
e = 3
m = 42
c = pow(m, e, n)
# But if m^3 < n, we can recover
n_small = 100000  # too small!
c_small = pow(m, e, n_small)
recovered = small_message_attack(c_small, e)
print(f"Original: {m}, Recovered: {recovered}")
```

**OAEP (Optimal Asymmetric Encryption Padding):**

OAEP adds randomness and ensures that RSA is semantically secure. Simplified structure:

```
m_padded = (m || 0^k) XOR H(r) || r XOR G(m || 0^k)
```

where r is random, H and G are hash functions.

```python
def oaep_pad(m: bytes, n_bits: int) -> bytes:
    """Simplified OAEP padding for demonstration."""
    import hashlib
    k = n_bits // 8 - 1
    h_len = 32  # SHA-256 output length
    
    # 1. Pad message to k - h_len - 1 bytes
    m_padded = m + b'\x00' * (k - h_len - 1 - len(m))
    
    # 2. Generate random seed
    seed = random.randbytes(h_len)
    
    # 3. Compute masked seed and masked message
    # (simplified - real OAEP uses MGF1)
    seed_mask = hashlib.sha256(seed + b"data").digest()[:len(m_padded)]
    masked_msg = bytes(a ^ b for a, b in zip(m_padded, seed_mask))
    
    msg_mask = hashlib.sha256(masked_msg).digest()[:h_len]
    masked_seed = bytes(a ^ b for a, b in zip(seed, msg_mask))
    
    return b'\x00' + masked_seed + masked_msg

# In practice, use a library
# from cryptography.hazmat.primitives import padding
```

**PSS (Probabilistic Signature Scheme):**

For signatures, PSS is the recommended padding scheme (PKCS#1 v2.2). It includes randomization and is provably secure.

### Diffie-Hellman Key Exchange

Diffie-Hellman (DH) allows two parties to agree on a shared secret over an insecure channel, without ever transmitting the secret itself.

```python
def dh_generate_params(bits: int = 2048) -> tuple[int, int]:
    """Generate DH parameters: prime p and generator g."""
    q = generate_large_prime(bits - 1)
    # Find safe prime p = 2q + 1
    while True:
        p = 2 * q + 1
        if is_probable_prime(p):
            break
        q = generate_large_prime(bits - 1)
    
    # Generator of the large prime-order subgroup
    g = 2
    while pow(g, 2, p) == 1 or pow(g, q, p) == 1:
        g += 1
    
    return (p, g, q)

def dh_keygen(p: int, g: int) -> tuple[int, int]:
    """Generate DH key pair. Returns (private_key, public_key)."""
    private_key = random.randint(2, p - 2)
    public_key = pow(g, private_key, p)
    return (private_key, public_key)

def dh_shared_secret(their_pub: int, my_priv: int, p: int) -> int:
    """Compute DH shared secret."""
    return pow(their_pub, my_priv, p)

# Demonstrate
p, g, q = dh_generate_params(64)
alice_priv, alice_pub = dh_keygen(p, g)
bob_priv, bob_pub = dh_keygen(p, g)

shared_alice = dh_shared_secret(bob_pub, alice_priv, p)
shared_bob = dh_shared_secret(alice_pub, bob_priv, p)

print(f"Alice shared: {shared_alice}")
print(f"Bob shared: {shared_bob}")
print(f"Match: {shared_alice == shared_bob}")
```

**DH protocol:**

1. Alice and Bob agree on p (prime) and g (generator)
2. Alice picks private a, sends A = g^a mod p
3. Bob picks private b, sends B = g^b mod p
4. Alice computes S = B^a = g^{ab} mod p
5. Bob computes S = A^b = g^{ab} mod p

Both now share S = g^{ab} mod p. An eavesdropper sees p, g, g^a, g^b but cannot compute g^{ab} efficiently (CDH assumption).

**Perfect Forward Secrecy:**

If long-term keys are compromised, past session keys remain secure because each session uses ephemeral (temporary) DH keys. This is DHE (Ephemeral Diffie-Hellman).

### ElGamal Encryption

ElGamal is a public-key cryptosystem based on the DLP, invented by Taher Elgamal in 1985.

```python
def elgamal_keygen(p: int, g: int) -> tuple[tuple[int, int, int], int]:
    """Generate ElGamal keys. Returns (pub, priv)."""
    x = random.randint(2, p - 2)  # private key
    h = pow(g, x, p)              # public key
    return ((p, g, h), x)

def elgamal_encrypt(m: int, pub: tuple) -> tuple[int, int]:
    """Encrypt message m using ElGamal public key."""
    p, g, h = pub
    y = random.randint(2, p - 2)
    c1 = pow(g, y, p)
    s = pow(h, y, p)
    c2 = (m * s) % p
    return (c1, c2)

def elgamal_decrypt(c: tuple, priv: int, p: int) -> int:
    """Decrypt ElGamal ciphertext."""
    c1, c2 = c
    s = pow(c1, priv, p)
    s_inv = pow(s, -1, p)
    return (c2 * s_inv) % p

# Example
p, g, _ = dh_generate_params(64)
pub, priv = elgamal_keygen(p, g)
m = 123456
c = elgamal_encrypt(m, pub)
m2 = elgamal_decrypt(c, priv, p)
print(f"Original: {m}, Decrypted: {m2}, Match: {m == m2}")
```

**Properties:**

- **Randomized:** Same plaintext produces different ciphertext each time (due to random y)
- **Homomorphic:** E(m1) * E(m2) = E(m1 * m2)
- **Expanded ciphertext:** Ciphertext is 2x the plaintext size

### Elliptic Curve Cryptography (Basic Concepts)

ECC provides equivalent security to RSA/DH with much smaller key sizes. It uses the algebraic structure of elliptic curves over finite fields.

**Elliptic curve equation (Weierstrass form):**

$$
y^2 = x^3 + a \cdot x + b
$$

**Group operation:** Points on the curve (plus a point at infinity) form an abelian group. Point addition has a geometric interpretation.

```python
# Simplified elliptic curve arithmetic over Z_p
class ECCurve:
    """Elliptic curve y^2 = x^3 + ax + b over Z_p."""
    
    def __init__(self, a: int, b: int, p: int):
        self.a = a
        self.b = b
        self.p = p
        # Check discriminant
        assert (4 * a**3 + 27 * b**2) % p != 0
    
    def is_on_curve(self, point: tuple[int, int]) -> bool:
        """Check if a point is on the curve."""
        if point is None:  # point at infinity
            return True
        x, y = point
        return (y * y - x**3 - self.a * x - self.b) % self.p == 0
    
    def point_add(self, P: tuple[int, int] | None, Q: tuple[int, int] | None) -> tuple[int, int] | None:
        """Add two points on the elliptic curve."""
        if P is None:  # identity
            return Q
        if Q is None:
            return P
        
        x1, y1 = P
        x2, y2 = Q
        
        if x1 == x2 and y1 != y2:
            return None  # point at infinity (inverse)
        
        if P == Q:
            # Point doubling
            if y1 == 0:
                return None
            s = (3 * x1 * x1 + self.a) * pow(2 * y1, -1, self.p) % self.p
        else:
            s = (y2 - y1) * pow(x2 - x1, -1, self.p) % self.p
        
        x3 = (s * s - x1 - x2) % self.p
        y3 = (s * (x1 - x3) - y1) % self.p
        return (x3, y3)
    
    def scalar_mult(self, k: int, P: tuple[int, int] | None) -> tuple[int, int] | None:
        """Multiply point P by scalar k using double-and-add."""
        result = None
        addend = P
        while k:
            if k & 1:
                result = self.point_add(result, addend)
            addend = self.point_add(addend, addend)
            k >>= 1
        return result

# secp256k1-like curve (not the actual parameters)
p = 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFEFFFFFC2F
a = 0
b = 7
curve = ECCurve(a, b, p)

# Generator point (secp256k1 G)
G = (0x79BE667EF9DCBBAC55A06295CE870B07029BFCDB2DCE28D959F2815B16F81798,
     0x483ADA7726A3C4655DA4FBFC0E1108A8FD17B448A68554199C47D08FFB10D4B8)

# Generate ECC key pair
private_key = random.randint(1, p - 1)
public_key = curve.scalar_mult(private_key, G)
print(f"EC private key: {hex(private_key)[:20]}...")
print(f"EC public key: ({hex(public_key[0])[:20]}..., {hex(public_key[1])[:20]}...)")

# ECDH key exchange
alice_priv = random.randint(1, p - 1)
bob_priv = random.randint(1, p - 1)
alice_pub = curve.scalar_mult(alice_priv, G)
bob_pub = curve.scalar_mult(bob_priv, G)

shared_alice = curve.scalar_mult(alice_priv, bob_pub)
shared_bob = curve.scalar_mult(bob_priv, alice_pub)
print(f"ECDH shared secret match: {shared_alice == shared_bob}")
```

**Key sizes comparison (NIST recommendations, 2020-2030):**

| Security Level (bits) | RSA Key Size | DH/DSA Key Size | ECC Key Size |
|----------------------|-------------|-----------------|--------------|
| 80 | 1024 | 1024 | 160-223 |
| 112 | 2048 | 2048 | 224-255 |
| 128 | 3072 | 3072 | 256-383 |
| 192 | 7680 | 7680 | 384-511 |
| 256 | 15360 | 15360 | 512+ |

### Digital Signatures

Digital signatures provide authentication, integrity, and non-repudiation.

**DSA (Digital Signature Algorithm):**

```python
def dsa_sign(m: int, p: int, q: int, g: int, x: int) -> tuple[int, int]:
    """Create a DSA signature."""
    k = random.randint(1, q - 1)
    r = pow(g, k, p) % q
    if r == 0:
        # Retry with different k
        return dsa_sign(m, p, q, g, x)
    k_inv = pow(k, -1, q)
    s = (k_inv * (m + x * r)) % q
    if s == 0:
        return dsa_sign(m, p, q, g, x)
    return (r, s)

def dsa_verify(m: int, sig: tuple, p: int, q: int, g: int, y: int) -> bool:
    """Verify a DSA signature."""
    r, s = sig
    if not (1 <= r <= q - 1) or not (1 <= s <= q - 1):
        return False
    w = pow(s, -1, q)
    u1 = (m * w) % q
    u2 = (r * w) % q
    v = (pow(g, u1, p) * pow(y, u2, p)) % p % q
    return v == r

# Example (small params for demo)
p = 0x8000000000000000000000000000000000000000000000000000000000000431
q = 0x8000000000000000000000000000000000000000000000000000000000000421
g = 2
x = random.randint(1, q - 1)
y = pow(g, x, p)

m = 123456789
sig = dsa_sign(m, p, q, g, x)
print(f"Signature: r={sig[0]}, s={sig[1]}")
print(f"Verify: {dsa_verify(m, sig, p, q, g, y)}")
print(f"Verify (tampered): {dsa_verify(m + 1, sig, p, q, g, y)}")
```

**ECDSA (Elliptic Curve DSA):**

ECDSA is the elliptic curve variant of DSA, widely used in Bitcoin, Ethereum, TLS, and SSH.

### Key Sizes and Security Levels

| Algorithm | Key Size (bits) | Security Level (bits) | Status |
|-----------|----------------|----------------------|--------|
| RSA | 2048 | 112 | Acceptable (legacy) |
| RSA | 3072 | 128 | Recommended |
| RSA | 4096 | 128-140 | Overkill but common |
| DH | 2048 | 112 | Acceptable (legacy) |
| DH | 3072 | 128 | Recommended |
| ECDH/ECDSA | 256 | 128 | Recommended |
| ECDH/ECDSA | 384 | 192 | High security |
| EdDSA (Ed25519) | 256 | 128 | Recommended (fast) |

NIST recommends 128-bit security as the minimum for new systems (2030+).

### Practical Considerations

**Hybrid Encryption:**

Public-key encryption is slow and cannot encrypt messages larger than the modulus. Therefore, hybrid encryption is used:

1. Generate a random symmetric key (e.g., AES-256)
2. Encrypt the message with the symmetric key (fast)
3. Encrypt the symmetric key with the recipient's public key

```python
def hybrid_encrypt(message: bytes, rsa_pub: dict) -> tuple[bytes, int]:
    """Hybrid encryption: RSA + AES."""
    import hashlib
    # Generate random AES-like key
    symmetric_key = random.randbytes(32)
    
    # Encrypt symmetric key with RSA
    encrypted_key = rsa_encrypt(int.from_bytes(symmetric_key, 'big'), rsa_pub)
    
    # Simple symmetric encryption (XOR-based, not real AES)
    # In practice, use AES-GCM or ChaCha20-Poly1305
    key_stream = hashlib.sha256(symmetric_key).digest() * (len(message) // 32 + 1)
    ciphertext = bytes(m ^ k for m, k in zip(message, key_stream[:len(message)]))
    
    return (ciphertext, encrypted_key)

def hybrid_decrypt(ciphertext: bytes, encrypted_key: int, rsa_priv: dict) -> bytes:
    """Hybrid decryption."""
    import hashlib
    symmetric_key = rsa_decrypt(encrypted_key, rsa_priv).to_bytes(32, 'big')
    key_stream = hashlib.sha256(symmetric_key).digest() * (len(ciphertext) // 32 + 1)
    return bytes(c ^ k for c, k in zip(ciphertext, key_stream[:len(ciphertext)]))
```

**Side-Channel Attacks:**

- **Timing attacks:** Measure operation time to infer secrets. Mitigated by constant-time implementations.
- **Power analysis:** Measure power consumption during computation.
- **Fault attacks:** Inject errors during computation to leak key material.

```python
# Constant-time comparison to prevent timing attacks
def constant_time_equal(a: bytes, b: bytes) -> bool:
    """Compare two byte strings in constant time."""
    if len(a) != len(b):
        return False
    result = 0
    for x, y in zip(a, b):
        result |= x ^ y
    return result == 0

# Constant-time modular exponentiation (natural for pow)
# Python's pow with modulus is already constant-time for fixed-length exponents
```

### Implementation Pitfalls

**Common pitfalls:**
- Not checking that p and q are different (n is a perfect square, easy to factor)
- Using predictable random number generators for key generation
- Not validating DH public key subgroup membership (small subgroup attack)
- Not validating ciphertext range before RSA decryption (Bleichenbacher attack)
- Using textbook RSA without padding (deterministic, malleable, small message attack)
- Reusing nonces in DSA/ECDSA (private key recovery when k is reused)
- Not using constant-time comparison for MAC/tag verification

## Glossary

| Term | Definition |
|------|------------|
| RSA | Public-key cryptosystem based on factoring difficulty |
| Public key | (n, e) shared openly for encryption and signature verification |
| Private key | (n, d) kept secret for decryption and signing |
| Modulus n | Product of two large primes p and q |
| Public exponent e | Typically 65537, must be coprime to phi(n) |
| Private exponent d | e^{-1} mod phi(n) |
| OAEP | Optimal Asymmetric Encryption Padding for RSA |
| PSS | Probabilistic Signature Scheme for RSA |
| Diffie-Hellman (DH) | Key exchange protocol based on DLP |
| ElGamal | Public-key cryptosystem based on DLP |
| ECC | Elliptic Curve Cryptography |
| ECDH | Elliptic Curve Diffie-Hellman key exchange |
| DSA | Digital Signature Algorithm |
| ECDSA | Elliptic Curve Digital Signature Algorithm |
| Hybrid encryption | Combining asymmetric and symmetric encryption |
| Perfect forward secrecy | Ephemeral keys ensure past sessions remain secure |
| Side-channel attack | Attack based on timing, power, or electromagnetic leakage |
| Fault attack | Inducing computation errors to extract key material |
| Constant-time | Implementation that runs in fixed time regardless of inputs |
| Semantic security | Ciphertext reveals no information about plaintext |
| Malleability | Property that ciphertext can be transformed to related plaintext |

## Quick References

- [Handbook of Applied Cryptography](http://cacr.uwaterloo.ca/hac/) -- comprehensive reference on cryptographic algorithms
- [PKCS #1 v2.2: RSA Cryptography Standard](https://tools.ietf.org/html/rfc8017) -- RSA padding specifications
- [NIST SP 800-57 Part 1](https://csrc.nist.gov/publications/detail/sp/800-57-part-1/rev-5/final) -- key management recommendations
- [SEC 1: Elliptic Curve Cryptography](https://www.secg.org/sec1-v2.pdf) -- ECC standard
- [SafeCurves Project](https://safecurves.cr.yp.to/) -- choosing safe elliptic curves
- [IETF RFC 8446: TLS 1.3](https://tools.ietf.org/html/rfc8446) -- modern TLS protocol using cryptographic primitives

## Next Steps

- [Primality & Hashing](primality-and-hashing.md) -- Miller-Rabin, AKS, hash functions, SHA, birthday attack
