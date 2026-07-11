# Floating-Point Numbers

## Description

Floating-point arithmetic is the closest computers get to real-number arithmetic, but it is not the same. Every developer who writes numerical code — whether computing coordinates, aggregating metrics, training models, or comparing values — must understand IEEE 754 floating-point representation, its finite precision, and the catastrophic pitfalls that arise when intuition about real numbers is applied to floating-point. This document covers the IEEE 754 standard from the ground up, then drills into every practical trap: rounding, cancellation, absorption, comparison failures, non-determinism, and the real-world disasters they have caused.

## Prerequisites

- [Real Numbers](real-numbers.md) — the mathematical concept of real numbers and their properties

## Table of Contents

- [The Fundamental Problem](#the-fundamental-problem)
- [Scientific Notation and Fixed-Point](#scientific-notation-and-fixed-point)
- [IEEE 754 Overview](#ieee-754-overview)
- [Single Precision (32-bit)](#single-precision-32-bit)
- [Double Precision (64-bit)](#double-precision-64-bit)
- [Half Precision (16-bit) and Quad Precision (128-bit)](#half-precision-16-bit-and-quad-precision-128-bit)
- [Normalized and Denormalized Numbers](#normalized-and-denormalized-numbers)
- [Special Values](#special-values)
- [Rounding Modes](#rounding-modes)
- [Why 0.1 + 0.2 != 0.3](#why-01--02--03)
- [Machine Epsilon and ULP](#machine-epsilon-and-ulp)
- [Catastrophic Cancellation](#catastrophic-cancellation)
- [Absorption](#absorption)
- [Floating-Point Comparison](#floating-point-comparison)
- [Error Propagation and Conditioning](#error-propagation-and-conditioning)
- [Kahan Summation](#kahan-summation)
- [Determinism and Portability](#determinism-and-portability)
- [IEEE 754 Across Languages](#ieee-754-across-languages)
- [Tools and Libraries](#tools-and-libraries)
- [When to Use Floating-Point vs Fixed-Point vs Arbitrary Precision](#when-to-use-floating-point-vs-fixed-point-vs-arbitrary-precision)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Fundamental Problem

Real numbers are infinite, continuous, and dense. Between any two distinct reals there are infinitely many more. Computers are finite, discrete, and bounded. A 64-bit machine can represent at most $2^{64}$ distinct values. Since there are infinitely many real numbers between 0 and 1 alone, the vast majority of real numbers have no exact floating-point representation. Every floating-point number is an approximation of some real number, and every arithmetic operation introduces rounding. This gap is the source of nearly every floating-point bug.

### Scientific Notation and Fixed-Point

Floating-point is binary scientific notation. Any number can be expressed as:

$$
x = m \times b^{e}
$$

where $m$ is the mantissa (significand), $b$ is the base (always 2 for IEEE 754 binary formats), and $e$ is the exponent. The mantissa determines the significant digits; the exponent determines the magnitude. Moving the decimal point left or right (floating the point) by adjusting the exponent gives the system its name.

Before floating-point hardware was cheap, fixed-point was common. A fixed-point number stores an integer and fractional part with a fixed bit budget. For example, Q16.16 uses 16 integer bits and 16 fractional bits:

```python
def q16_16_to_float(bits: int) -> float:
    integer_part = bits >> 16
    fractional_part = (bits & 0xFFFF) / 2**16
    if integer_part >= 0x8000:
        integer_part -= 0x10000
    return integer_part + fractional_part
```

Fixed-point is predictable — every operation has exactly the same precision — but the range is severely limited. A Q16.16 number cannot represent $10^6$ or $10^{-10}$. This rigidity drove the adoption of floating-point.

### IEEE 754 Overview

The IEEE 754 standard (1985, revised 2008, 2019) defines how floating-point arithmetic works on virtually every modern computer. It specifies binary and decimal formats, normalized and denormalized numbers, special values ($+\infty$, $-\infty$, NaN), four rounding modes, and five exception flags (invalid operation, division by zero, overflow, underflow, inexact).

| Name | Bits | Sign | Exponent | Mantissa | Approx decimal digits |
|------|------|------|----------|----------|----------------------|
| Half (binary16) | 16 | 1 | 5 | 10 | ~3.3 |
| Single (binary32) | 32 | 1 | 8 | 23 | ~7.2 |
| Double (binary64) | 64 | 1 | 11 | 52 | ~15.9 |
| Quad (binary128) | 128 | 1 | 15 | 112 | ~34.0 |

### Single Precision (32-bit)

A 32-bit float consists of 1 sign bit, 8 exponent bits, and 23 mantissa bits:

```
bit 31     bits 30:23       bits 22:0
[SIGN]     [EXPONENT]       [MANTISSA]
```

The value of a normalized single-precision number is:

$$
\text{value} = (-1)^{\text{sign}} \times 1.\text{mantissa} \times 2^{\text{exponent} - 127}
$$

The exponent is stored as an unsigned integer biased by 127. Exponent fields 0 and 255 are reserved.

```python
import struct

def float32_bits(f: float) -> str:
    packed = struct.pack('>f', f)
    bits = format(struct.unpack('>I', packed)[0], '032b')
    return f"{bits[0]} {bits[1:9]} {bits[9:]}"

print(float32_bits(1.0))      # 0 01111111 00000000000000000000000
print(float32_bits(-2.5))     # 1 10000000 01000000000000000000000
print(float32_bits(0.1))      # 0 01111011 10011001100110011001101
```

The largest finite value is $\approx 3.4 \times 10^{38}$. The smallest positive normalized value is $\approx 1.17 \times 10^{-38}$.

### Double Precision (64-bit)

Same scheme with larger fields: 1 sign bit, 11 exponent bits (bias 1023), 52 mantissa bits:

```python
def float64_bits(f: float) -> str:
    packed = struct.pack('>d', f)
    bits = format(struct.unpack('>Q', packed)[0], '064b')
    return f"{bits[0]} {bits[1:12]} {bits[12:]}"

print(float64_bits(1.0))   # 0 01111111111 0000...(52 zeros)
print(float64_bits(0.1))   # 0 01111111011 100110011001...1010
```

Double precision is the default in Python, JavaScript, Ruby, etc. Range: $\approx \pm 1.8 \times 10^{308}$, precision: ~15-17 significant decimal digits.

### Half Precision (16-bit) and Quad Precision (128-bit)

Half precision (binary16) uses 1 sign bit, 5 exponent bits (bias 15), 10 mantissa bits. Max value ~65504, ~3.3 decimal digits. Widely used in ML for throughput:

```python
import numpy as np
x = np.float16(0.1)
y = np.float16(0.2)
print(x + y)  # 0.30005 (much less accurate than float64)
```

Quad precision (binary128) uses 1 sign bit, 15 exponent bits (bias 16383), 112 mantissa bits — ~34 decimal digits. Hardware support is limited; most implementations are software-based.

### Normalized and Denormalized Numbers

When the exponent field is neither all-zeros nor all-ones, the number is **normalized** — the leading bit of the mantissa is implicitly 1. This gives an extra bit of precision: single stores 23 bits but has 24-bit precision; double stores 52 bits with 53-bit precision.

When the exponent field is all-zeros and the mantissa is non-zero, the number is **denormalized** (subnormal). The implicit leading bit becomes 0, allowing **gradual underflow** — representation of values closer to zero than the smallest normalized value:

$$
\text{denormal value} = (-1)^{\text{sign}} \times 0.\text{mantissa} \times 2^{-126}
$$

For single precision, the smallest positive denormal is $\approx 1.4 \times 10^{-45}$ vs the smallest normalized $\approx 1.17 \times 10^{-38}$.

```python
def classify_float(f: float) -> str:
    import math
    if math.isnan(f): return "NaN"
    if math.isinf(f): return "Infinity"
    if f == 0.0: return "Zero"
    exp = (struct.pack('>Q', struct.unpack('>d', f)[0]) >> 52) & 0x7FF
    return "Denormalized" if exp == 0 else "Normalized"

print(classify_float(1e-310))  # Denormalized
```

Denormals have a significant performance cost (10-100x slower). Many systems flush denormals to zero (FTZ mode) for speed.

### Special Values

IEEE 754 reserves the all-zeros and all-ones exponent fields for special values.

**Zero:** Exponent = 0, mantissa = 0. Both $+0$ and $-0$ exist. They compare equal but have different sign bits:

```python
print(1.0 / 0.0)   # inf
print(1.0 / -0.0)  # -inf
```

**Infinity:** Exponent = all-ones, mantissa = 0. Represents overflow and division by zero:

```python
import math
print(1e308 * 2)       # inf (overflow)
print(1.0 / 0.0)       # inf
print(-1.0 / 0.0)      # -inf
print(math.inf - math.inf)  # nan
```

**NaN (Not a Number):** Exponent = all-ones, mantissa $\neq 0$. Two types exist:
- **Quiet NaN (qNaN):** Propagates silently. Most significant mantissa bit = 1.
- **Signaling NaN (sNaN):** Raises an exception when accessed. Most significant mantissa bit = 0.

NaNs have a critical property: they are never equal to anything, including themselves:

```python
nan = float('nan')
print(nan == nan)      # False
print(nan < 0.0)       # False
print(nan > 0.0)       # False
```

Always use `math.isnan(x)` — never `x == float('nan')`.

### Rounding Modes

When an operation produces an unrepresentable result, rounding must occur. IEEE 754 defines four modes:

| Mode | Direction | Notes |
|------|-----------|-------|
| Round to nearest, ties to even (RN) | Nearest | Tie goes to even LSB (default) |
| Round toward zero (RZ) | Truncate | Chop excess bits |
| Round toward $+\infty$ (RU) | Up | Ceiling |
| Round toward $-\infty$ (RD) | Down | Floor |

Round-to-nearest-even is default in virtually every language. When the exact result is exactly halfway between two representable values, the one with an even least significant bit is chosen. This avoids statistical bias:

```python
print(f"{2.5:.0f}")   # 2 (rounds to even)
print(f"{3.5:.0f}")   # 4 (rounds to even)
print(f"{2.675:.2f}") # 2.67 (not 2.68 — 2.675 is not exact in binary)
```

### Why 0.1 + 0.2 != 0.3

This is the most famous floating-point demonstration:

```python
print(0.1 + 0.2)             # 0.30000000000000004
print(0.1 + 0.2 == 0.3)      # False
```

Neither $0.1$ nor $0.2$ is exactly representable in binary. The decimal number $0.1$ in binary is an infinitely repeating fraction:

$$
0.1_{10} = 0.0001100110011001100110011..._{2}
$$

It repeats indefinitely, just as $1/3 = 0.333...$ repeats in decimal. IEEE 754 double precision stores only 52 bits of mantissa, so the infinite expansion is truncated:

```python
from decimal import Decimal
print(Decimal(0.1))              # 0.1000000000000000055511151231257827021181583404541015625
print(Decimal(0.2))              # 0.2000000000000000111022302462515654042363166809082031250
print(Decimal(0.1 + 0.2))        # 0.3000000000000000444089209850062616169452667236328125
print(Decimal(0.3))              # 0.2999999999999999888977697537484345957636833190917968750
```

The stored sum of $0.1$ and $0.2$ is not the stored $0.3$. This is not a bug — it is the correct result of IEEE 754 arithmetic.

### Machine Epsilon and ULP

**Machine epsilon** $\varepsilon$ is the maximum relative error due to rounding — the distance from 1.0 to the next representable float:

- Single: $\varepsilon = 2^{-23} \approx 1.19 \times 10^{-7}$
- Double: $\varepsilon = 2^{-52} \approx 2.22 \times 10^{-16}$

```python
import numpy as np
print(np.finfo(np.float32).eps)  # 1.1920929e-07
print(np.finfo(np.float64).eps)  # 2.220446049250313e-16
```

**Unit in the Last Place (ULP)** is the spacing between consecutive floats at a given magnitude:

$$
\text{ULP}(x) = 2^{\lfloor \log_2 |x| \rfloor} \times 2^{-p}
$$

where $p$ is the precision in bits (24 for single, 53 for double).

```python
import math

def ulp(x: float) -> float:
    if x == 0.0:
        return sys.float_info.min * 2**-52
    exp = math.floor(math.log2(abs(x)))
    return 2.0 ** (exp - 52)

print(ulp(1.0))          # 2.22e-16
print(ulp(1e6))          # 1.16e-10
print(ulp(1e-10))        # 1.29e-26
```

The ULP at $x = 1.0$ equals $\varepsilon$. The ULP at $x = 10^6$ is ~$10^{-10}$, meaning values at $10^6$ have a spacing of about $10^{-10}$.

### Catastrophic Cancellation

Catastrophic cancellation occurs when two nearly equal numbers are subtracted. The leading significant digits cancel, leaving only rounding error in the result.

```python
import math

# Quadratic formula with b >> a,c
a, b, c = 1.0, 1e8, 1.0
discriminant = math.sqrt(b*b - 4*a*c)

# -b + sqrt(...) where b ~ sqrt(...) → catastrophic cancellation
x1 = (-b + discriminant) / (2*a)
x2 = (-b - discriminant) / (2*a)
print(x1)  # -1.0e-8 (accurate)
print(x2)  # -99999999.0 (accurate)

# Avoid cancellation by reformulating: x1 = 2c / (-b ∓ sqrt(...))
x1_alt = (2*c) / (-b - discriminant)
print(x1_alt)  # same here, but often dramatically different
```

More dramatic with limited precision:

```python
import numpy as np
a = np.float16(10000.0)
b = np.float16(9999.5)
c_bad = np.float16(a*a) - np.float16(b*b)
c_good = np.float16(a - b) * np.float16(a + b)
print(f"Bad: {c_bad}")    # 8192 (way off from 9999.75)
print(f"Good: {c_good}")  # 10000
```

**Rule: rearrange formulas to avoid subtracting nearly equal values.** If unavoidable, increase precision.

### Absorption

Absorption happens when adding a very small number to a large one produces no change because the small value falls below the ULP of the large value:

```python
x = 1e16
y = 1.0
print(x + y == x)  # True — 1 is absorbed into 1e16
```

In double precision, numbers at $10^{16}$ have ULP $\approx 2$, so adding 1.0 does nothing. The practical consequence: naive summation of many small values into a large accumulator loses them entirely.

```python
total = 0.0
for i in range(10_000_000):
    total += 1e-16
print(total)  # Much less than 1e-9 (expected)
```

### Floating-Point Comparison

**Never use `==` to compare floating-point numbers.** This is the single most important practical rule.

```python
# WRONG
if 0.1 + 0.2 == 0.3: ...

# RIGHT
def approx_equal(a: float, b: float, rel_tol: float = 1e-9, abs_tol: float = 0.0) -> bool:
    return abs(a - b) <= max(rel_tol * max(abs(a), abs(b)), abs_tol)
```

| Strategy | When to use | Formula |
|----------|-------------|---------|
| Absolute tolerance | Numbers near zero | `abs(a - b) <= tol` |
| Relative tolerance | Varying magnitudes | `abs(a - b) <= rel_tol * max(abs(a), abs(b))` |
| ULP-based | Bit-exact comparison | `abs(a - b) <= n * ulp(a)` |
| Combined | General case | `abs(a - b) <= max(rel_tol * norm, abs_tol)` |

Python's `math.isclose` (3.5+) implements the combined approach:

```python
import math
print(math.isclose(0.1 + 0.2, 0.3, rel_tol=1e-9))     # True
print(math.isclose(1e-20, 2e-20, abs_tol=1e-15))       # True (absolute near zero)
```

Special care for zero:

```python
def safe_isclose(a: float, b: float, rel_tol=1e-9, abs_tol=1e-12) -> bool:
    return abs(a - b) <= max(rel_tol * max(abs(a), abs(b), 1.0), abs_tol)
```

### Error Propagation and Conditioning

Every floating-point operation introduces small errors. How they accumulate determines numerical stability.

**Absolute error:** $E_{\text{abs}} = |\hat{x} - x|$

**Relative error:** $E_{\text{rel}} = |\hat{x} - x| / |x|$ (scale-invariant — more meaningful)

The **condition number** $\kappa$ measures how much a function's output changes relative to its input:

$$
\kappa \approx \left|\frac{x \cdot f'(x)}{f(x)}\right|
$$

A problem with large $\kappa$ is **ill-conditioned**: small input errors produce large output errors, regardless of the algorithm:

```python
def condition_number(f, df, x: float) -> float:
    return abs(x * df(x) / f(x))

# sqrt(x): kappa = 0.5 (well-conditioned everywhere)
# log(x): kappa = 1/|log(x)| (ill-conditioned near x=1)
```

### Kahan Summation

Naive summation of $n$ floating-point numbers accumulates $O(n\varepsilon)$ error. Kahan's compensated summation reduces this to $O(\varepsilon + n\varepsilon^2)$, effectively independent of $n$:

```python
def kahan_sum(values: list[float]) -> float:
    s = 0.0  # running sum
    c = 0.0  # compensation term
    for v in values:
        y = v - c
        t = s + y
        c = (t - s) - y  # recover lost low bits
        s = t
    return s

# Demonstration
large = 1e8
values = [large] + [1e-8] * 10_000_000
naive = sum(values)  # Python uses pairwise, but for illustration...
print(f"Loss from absorption: {sum(values[1:]):.10f}")  # expected 0.1
```

The compensation variable `c` tracks the low-order bits lost in each addition. On the next iteration, those bits are first subtracted from the input, partially recovering the loss.

### Determinism and Portability

Floating-point is not necessarily deterministic across platforms, compilers, or optimization levels.

| Source | Example |
|--------|---------|
| Compiler optimization | `-ffast-math` may reorder operations |
| FMA | `x*y + z` as one operation with different rounding |
| Register width | x87 80-bit vs SSE 32/64-bit |
| Parallel reduction | Summation order varies with thread count |
| Math libraries | Different `sin`, `exp`, `log` implementations |

```c
// C: FMA vs non-FMA changes the result
double fma_vs_non(double a, double b, double c) {
    return a * b + c;  // FMA on some targets, separate on others
}
```

For determinism in C/C++:

```bash
gcc -O2 -fno-fast-math -fno-associative-math
```

For critical applications (financial settlement, scientific reproducibility), use fixed-point or arbitrary-precision arithmetic.

### IEEE 754 Across Languages

| Language | Single | Double | Half | Notes |
|----------|--------|--------|------|-------|
| Python | `numpy.float32` | `float` | `numpy.float16` | `float` is always double |
| JavaScript | — | `Number` | — | All numbers are double; safe integers to $2^{53}$ |
| Java | `float` | `double` | — | `strictfp` for exact FP |
| C/C++ | `float` | `double` | `_Float16` (C23) | `long double` is x87 80-bit on x86 |
| Rust | `f32` | `f64` | nightly | Stronger determinism; no `Eq` trait for floats |
| Go | `float32` | `float64` | — | |
| Julia | `Float32` | `Float64` | `Float16` | |

**Python:** `float` is a C double:

```python
import sys
print(sys.float_info)  # Double-precision limits
```

**JavaScript:** `Number` is always double. Integer-safe range is $2^{53} - 1$:

```javascript
console.log(9007199254740993);  // 9007199254740992 (wrong!)
```

**C/C++:** `long double` is platform-dependent — 80-bit extended on x86, 128-bit quad on ARM, identical to `double` on MSVC.

**Rust:** `f32` and `f64` are strict IEEE 754 without x87 surprises. `Eq` is intentionally not implemented — use `abs(a - b) < eps`.

### Tools and Libraries

| Tool | Description |
|------|-------------|
| `math.isclose(a, b, rel_tol, abs_tol)` | Tolerance-based comparison |
| `math.nextafter(x, target)` | Next representable float toward target |
| `math.frexp(x)` | Extract mantissa and exponent |
| `math.ldexp(mant, exp)` | Construct float from mantissa and exponent |
| `math.ulp(x)` | ULP at x (Python 3.9+) |
| `decimal.Decimal` | Arbitrary-precision decimal arithmetic |
| `fractions.Fraction` | Exact rational arithmetic |

```python
import math

# Explore adjacent representable values
x = 1.0
next_up = math.nextafter(x, math.inf)
next_down = math.nextafter(x, -math.inf)
print(f"ULP at 1.0: {next_up - x}")  # 2.22e-16

# Decompose and reconstruct
mant, exp = math.frexp(12.75)
print(f"12.75 = {mant} * 2^{exp}")   # 0.796875 * 2^4
```

**Decimal module** — exact decimal arithmetic, no binary rounding:

```python
from decimal import Decimal
print(Decimal('0.1') + Decimal('0.2'))             # 0.3
print(Decimal('0.1') + Decimal('0.2') == Decimal('0.3'))  # True
```

Decimal is 10-100x slower than float. Use for money, configurable precision, and legal rounding.

### When to Use Floating-Point vs Fixed-Point vs Arbitrary Precision

| Domain | Best choice | Why |
|--------|-------------|-----|
| Physics, simulation, ML | Double-precision float | Wide dynamic range, fast hardware |
| Finance, accounting | Decimal or integer cents | Legal rounding, no surprises |
| Cryptography | Arbitrary-precision integers | Exact integer arithmetic |
| Geometry, graphics | Single/double float | Standard hardware, sufficient precision |
| Embedded (no FPU) | Fixed-point | Deterministic timing, no FPU |
| Scientific computing | Double-precision float | Standard, well-understood |
| High-precision numerics | Quad or arbitrary precision | Avoid accumulated error |

**Rule of thumb:** if you need exact decimal fractions (money, tax), use Decimal or integer cents. If you need wide dynamic range with sufficient precision (simulation, ML), use double. If you need ULP-level control, use float with `math.nextafter` and tolerance-based comparisons.

## Glossary

| Term | Definition |
|------|------------|
| IEEE 754 | International standard for floating-point arithmetic — formats, rounding, special values, exceptions |
| Mantissa (significand) | The significant digits of a floating-point number; normalized form has implicit leading 1 |
| Exponent bias | Constant subtracted from stored exponent to allow negative exponents |
| Normalized number | Float with implicit leading 1 in mantissa, giving one extra bit of precision |
| Denormalized (subnormal) number | Float with exponent all-zeros and implicit leading 0, enabling gradual underflow |
| Machine epsilon ($\varepsilon$) | Distance from 1.0 to next representable float; max relative rounding error |
| Unit in the Last Place (ULP) | Spacing between consecutive floats at a given magnitude |
| Round-to-nearest-even | Default rounding mode; ties go to the even least significant bit |
| NaN (Not a Number) | Special value for invalid operation results; never equal to itself |
| Quiet NaN (qNaN) | NaN that propagates silently (most significant mantissa bit = 1) |
| Signaling NaN (sNaN) | NaN that raises an exception when accessed (most significant mantissa bit = 0) |
| Catastrophic cancellation | Loss of significant digits when subtracting nearly equal numbers |
| Absorption | Loss of a small value when added to a much larger one (falls below ULP) |
| Condition number ($\kappa$) | Ratio of relative output change to relative input change; high $\kappa$ = ill-conditioned |
| Kahan summation | Compensated summation algorithm with $O(\varepsilon + n\varepsilon^2)$ error |
| Fused multiply-add (FMA) | Single operation $a \times b + c$ with one rounding step |
| Fixed-point | Representation with fixed bit count for integer and fractional parts |
| Gradual underflow | Progressive loss of precision via denormals as numbers approach zero |
| FTZ (Flush To Zero) | Mode replacing denormals with zero for performance |
| Overflow | Result exceeding max finite value, producing $\pm\infty$ |
| Underflow | Result closer to zero than smallest normal, producing denormal or zero |
| `math.isclose` | Python's tolerance-based float comparison |
| `math.nextafter` | Next representable float in a given direction |
| `math.frexp` | Decompose float into mantissa and exponent |
| `math.ulp` | Return ULP size at a value (Python 3.9+) |
| Decimal | Python's arbitrary-precision decimal arithmetic module |
| `pytest.approx` | Pytest helper for tolerant float comparison |
| Binary64 | IEEE 754 double-precision (64-bit) format |
| Binary32 | IEEE 754 single-precision (32-bit) format |
| Binary16 | IEEE 754 half-precision (16-bit) format |

## Quick References

- [What Every Computer Scientist Should Know About Floating-Point Arithmetic](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html) — David Goldberg's definitive paper
- [Floating Point Guide](https://floating-point-gui.de/) — practical reference for everyday developers
- [IEEE 754 Converter](https://www.h-schmidt.net/FloatConverter/IEEE754.html) — interactive single-precision bit viewer
- [Float Exposed](https://float.exposed/) — explore IEEE 754 bit patterns interactively
- [0.30000000000000004.com](https://0.30000000000000004.com/) — the classic example across languages
- [The Patriot Missile Failure (GAO Report)](https://www.gao.gov/products/IMTEC-92-26) — official investigation
- [Vancouver Stock Exchange Index](https://en.wikipedia.org/wiki/Vancouver_Stock_Exchange) — the truncation story

## Next Steps

None — this is the final file in the Number Systems submodule. To continue learning number theory, see:

- [Divisibility & Primes](../divisibility-and-primes.md) — division algorithm, prime numbers, GCD, Euclidean algorithm
