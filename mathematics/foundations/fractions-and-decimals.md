# Fractions and Decimals

## Description

Fractions and decimals extend whole-number arithmetic into the domain of parts, proportions, and continuous quantities. Developers encounter them constantly — from floating-point arithmetic and percentage calculations to ratio-based scaling and precision-sensitive financial code. This document covers the theory and computation of fractions, decimals, percentages, and ratios, with explicit attention to how these concepts manifest (and misbehave) in programming languages.

## Prerequisites

- [Arithmetic Basics](arithmetic-basics.md) — addition, subtraction, multiplication, division, order of operations

## Table of Contents

- [Fractions](#fractions)
  - [What Is a Fraction?](#what-is-a-fraction)
  - [Simplifying Fractions](#simplifying-fractions)
  - [Common Denominators](#common-denominators)
  - [Adding and Subtracting Fractions](#adding-and-subtracting-fractions)
  - [Multiplying Fractions](#multiplying-fractions)
  - [Dividing Fractions](#dividing-fractions)
  - [Mixed Numbers and Improper Fractions](#mixed-numbers-and-improper-fractions)
- [Decimals](#decimals)
  - [Place Value Beyond the Decimal Point](#place-value-beyond-the-decimal-point)
  - [Converting Fractions to Decimals](#converting-fractions-to-decimals)
  - [Converting Decimals to Fractions](#converting-decimals-to-fractions)
  - [Decimal Operations](#decimal-operations)
- [Percentages](#percentages)
  - [What Percent Means](#what-percent-means)
  - [Converting Between Percent, Fraction, and Decimal](#converting-between-percent-fraction-and-decimal)
  - [Percentage Increase and Decrease](#percentage-increase-and-decrease)
- [Ratios](#ratios)
  - [What Ratios Are](#what-ratios-are)
  - [Proportional Reasoning](#proportional-reasoning)
- [Floating-Point in Programming](#floating-point-in-programming)
  - [Why 0.1 + 0.2 ≠ 0.3](#why-01--02--03)
  - [Integer Division vs. Float Division](#integer-division-vs-float-division)
  - [Practical Strategies for Precision](#practical-strategies-for-precision)
- [Learning Tips](#learning-tips)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Fractions

#### What Is a Fraction?

A **fraction** represents a part of a whole. It consists of two components:

```
    numerator
   ─────────
   denominator
```

The **numerator** (top) tells how many parts are taken. The **denominator** (bottom) tells how many equal parts the whole is divided into. The denominator cannot be zero — division by zero is undefined, a concept that recurs throughout mathematics and programming.

A fraction is equivalent to division: $\frac{3}{4} = 3 \div 4 = 0.75$

**Types of fractions:**

| Type | Definition | Example |
|------|-----------|---------|
| **Proper** | Numerator < Denominator | $\frac{3}{7}$, $\frac{1}{4}$ |
| **Improper** | Numerator ≥ Denominator | $\frac{7}{3}$, $\frac{9}{9}$ |
| **Mixed** | Whole number + proper fraction | $2\frac{1}{3}$, $5\frac{3}{4}$ |

**Python representation of fractions:**

```python
from fractions import Fraction
one_third = Fraction(1, 3)
improper = Fraction(7, 3)
print(one_third)        # 1/3
print(float(improper))  # 2.3333333333333335
```

The `fractions.Fraction` class preserves exact rational arithmetic.

#### Simplifying Fractions

A fraction is in **lowest terms** (or **simplified**) when the numerator and denominator share no common factors other than 1.

**The process:** find the **Greatest Common Divisor (GCD)** of the numerator and denominator, then divide both by it.

```
Example:  simplify 12/18

   Step 1: Find GCD(12, 18)
      Factors of 12: 1, 2, 3, 4, 6, 12
      Factors of 18: 1, 2, 3, 6, 9, 18
      Common factors: 1, 2, 3, 6
      GCD = 6

   Step 2: Divide both by 6
      12 ÷ 6 = 2
      18 ÷ 6 = 3

   Result: 12/18 = 2/3
```

**Why simplification matters:**

- Simplified fractions are easier to compare
- They reduce computational overhead in symbolic math
- They prevent overflow when working with large integers in programming

**Python — simplification is automatic:**

```python
from fractions import Fraction

# Fraction automatically simplifies
f = Fraction(12, 18)
print(f)               # 2/3 (already simplified)

# GCD computation
from math import gcd
print(gcd(12, 18))    # 6
```

#### Common Denominators

To add or subtract fractions, they must share a **common denominator** — the same bottom number.

**Finding a common denominator:** multiply each fraction's numerator and denominator by whatever is needed to make the denominators match.

```
Example:  1/3 + 1/4

   LCD (Least Common Denominator) = 12

   1/3  →  (1 × 4)/(3 × 4)  =  4/12
   1/4  →  (1 × 3)/(4 × 3)  =  3/12

   Now both fractions have denominator 12.
```

**Finding the LCD:** the LCD is the LCM of the denominators. List multiples and find the smallest common one, or prime-factor both denominators.

#### Adding and Subtracting Fractions

Once denominators match, add or subtract numerators and keep the denominator:

$$\frac{a}{c} + \frac{b}{c} = \frac{a + b}{c}$$

$$\frac{a}{c} - \frac{b}{c} = \frac{a - b}{c}$$

**Worked examples:**

```
Example 1:  2/5 + 1/5
   Denominators already match.
   (2 + 1)/5 = 3/5  ✓

Example 2:  3/8 - 1/4
   Convert 1/4 to eighths:  1/4 = 2/8
   3/8 - 2/8 = 1/8  ✓

Example 3:  5/6 + 2/3
   LCD(6, 3) = 6
   2/3 = 4/6
   5/6 + 4/6 = 9/6 = 3/2  (simplify: divide by 3)  ✓
```

**Python — addition and subtraction:**

```python
from fractions import Fraction

a = Fraction(3, 8)
b = Fraction(1, 4)

print(a + b)    # 5/8
print(a - b)    # 1/8

# Complex expression
c = Fraction(5, 6) + Fraction(2, 3)
print(c)        # 3/2
print(float(c)) # 1.5
```

#### Multiplying Fractions

Multiply numerators together and denominators together:

$$\frac{a}{b} \times \frac{c}{d} = \frac{a \times c}{b \times d}$$

No common denominator is required for multiplication — this is a significant simplification.

```
Example 1:  2/3 × 4/5
   = (2 × 4) / (3 × 5)
   = 8/15  (already simplified)

Example 2:  3/4 × 2/7
   = (3 × 2) / (4 × 7)
   = 6/28
   = 3/14  (simplify: divide by 2)
```

**Cross-cancellation** — simplify before multiplying to keep numbers small. Factor numerator-denominator pairs and cancel common factors:

```
Example:  6/8 × 4/9

   Factor:  (2 × 3)/(2³) × (2²)/(3²)
   Cancel:  3 × 2² / (2² × 3²) = 3/9 = 1/3  ✓
```

**Python:**

```python
from fractions import Fraction
print(Fraction(2, 3) * Fraction(4, 5))    # 8/15
print(Fraction(6, 8) * Fraction(4, 9))    # 1/3
```

#### Dividing Fractions

Division by a fraction is multiplication by its **reciprocal** (flip the numerator and denominator):

$$\frac{a}{b} \div \frac{c}{d} = \frac{a}{b} \times \frac{d}{c} = \frac{a \times d}{b \times c}$$

```
Example 1:  3/5 ÷ 2/7
   = 3/5 × 7/2     (flip 2/7 to get 7/2)
   = (3 × 7) / (5 × 2)
   = 21/10
   = 2.1

Example 2:  1/4 ÷ 3/8
   = 1/4 × 8/3
   = (1 × 8) / (4 × 3)
   = 8/12
   = 2/3  ✓
```

**Why does this work?** Division asks "how many times does the divisor fit into the dividend?" Flipping the divisor and multiplying achieves the same result because:

$$\frac{a}{b} \div \frac{c}{d} = \frac{a}{b} \times \frac{1}{c/d} = \frac{a}{b} \times \frac{d}{c}$$

**Python:**

```python
from fractions import Fraction

print(Fraction(3, 5) / Fraction(2, 7))    # 21/10
print(Fraction(1, 4) / Fraction(3, 8))    # 2/3
```

#### Mixed Numbers and Improper Fractions

**Converting mixed number to improper fraction:**

$$2\frac{3}{4} = \frac{(2 \times 4) + 3}{4} = \frac{11}{4}$$

**Converting improper fraction to mixed number:**

$$\frac{11}{4} = 11 \div 4 = 2 \text{ remainder } 3 = 2\frac{3}{4}$$

**Python — mixed number conversion:**

```python
from fractions import Fraction

f = Fraction(11, 4)
whole = f.numerator // f.denominator
remainder = f.numerator % f.denominator
print(f"{whole} {remainder}/{f.denominator}")  # 2 3/4
```

---

### Decimals

#### Place Value Beyond the Decimal Point

A **decimal number** extends place value to the right of the decimal point, where each position represents a negative power of 10:

```
   Hundreds  Tens   Ones  .  Tenths  Hundredths  Thousandths
      100      10     1   .    0.1      0.01        0.001

   Example:  347.258
   = 300 + 40 + 7 + 0.2 + 0.05 + 0.008
```

| Position | Power of 10 | Example digit in `347.258` |
|----------|-------------|--------------------------|
| Ones | $10^0$ | 7 |
| Tenths | $10^{-1}$ | 2 |
| Hundredths | $10^{-2}$ | 5 |
| Thousandths | $10^{-3}$ | 8 |

**Why this matters for developers:** currency requires exact hundredths ($0.01 precision), scientific computations involve tiny values, and database schemas define `DECIMAL(precision, scale)` where scale is the digits after the decimal point.

#### Converting Fractions to Decimals

Division of numerator by denominator yields the decimal equivalent:

```
Example 1:  3 ÷ 4 = 0.75       (terminates)
Example 2:  1 ÷ 3 = 0.333...   (repeating)
```

**Two outcomes:**

| Type | When it happens | Example |
|------|----------------|---------|
| **Terminating** | Denominator has only factors of 2 and/or 5 | $\frac{3}{4} = 0.75$ |
| **Repeating** | Denominator has prime factors other than 2 and 5 | $\frac{1}{3} = 0.\overline{3}$ |

```
   Why only 2 and 5? Because 10 = 2 × 5.
   Denominators with only factors of 2 and/or 5 terminate.
   Any other prime factor causes repetition.

     1/2  = 0.5          (factor 2)       → terminates
     1/5  = 0.2          (factor 5)       → terminates
     1/8  = 0.125        (factor 2³)      → terminates
     1/3  = 0.333...     (factor 3)       → repeats
     1/7  = 0.142857...  (factor 7)       → repeats
     1/6  = 0.1666...    (factors 2 × 3)  → repeats
```

**Python — fraction to decimal:**

```python
from fractions import Fraction
from decimal import Decimal

f = Fraction(1, 3)
print(float(f))        # 0.3333333333333333
d = Decimal(f.numerator) / Decimal(f.denominator)
print(d)               # 0.333333333333333333333333333
```

#### Converting Decimals to Fractions

**Step 1:** count the decimal places.

**Step 2:** place the digits over 10, 100, 1000, ... (matching the decimal places).

**Step 3:** simplify.

```
Example 1:  0.75

   Two decimal places → denominator = 100
   0.75 = 75/100

   Simplify: GCD(75, 100) = 25
   75/100 = 3/4  ✓

Example 2:  0.125

   Three decimal places → denominator = 1000
   0.125 = 125/1000

   Simplify: GCD(125, 1000) = 125
   125/1000 = 1/8  ✓
```

**Repeating decimals to fractions (algebraic method):**

```
Example:  0.444...

   Let x = 0.444...
   Then 10x = 4.444...

   10x - x = 4.444... - 0.444...
   9x = 4
   x = 4/9

   Verify: 4/9 = 0.444...  ✓
```

**Python — exact conversion from Decimal to Fraction:**

```python
from decimal import Decimal
from fractions import Fraction
print(Fraction(Decimal("0.125")))   # 1/8
```

For repeating decimals, the `Fraction` class works best when constructed from integers (numerator/denominator) rather than from the decimal representation.

#### Decimal Operations

Decimal arithmetic follows the same rules as whole-number arithmetic, with alignment at the decimal point:

```
Addition / Subtraction:  align decimal points, pad with zeros.
   3.470
 + 1.258
 ───────
   4.728

Multiplication:  ignore decimal points, multiply, then place the point.
   3.4 × 2.7 → 34 × 27 = 918 → total decimal places = 2 → 9.18

Division:  eliminate decimals by scaling both numbers equally.
   8.4 ÷ 2.4 → 84 ÷ 24 = 3.5
```

**Python — using the `decimal` module for precise arithmetic:**

```python
from decimal import Decimal, getcontext
getcontext().prec = 10
a = Decimal("0.1")
b = Decimal("0.2")
print(a + b)              # 0.3 (exact!)
print(0.1 + 0.2)          # 0.30000000000000004 (imprecise float)
```

---

### Percentages

#### What Percent Means

A **percentage** is a fraction expressed with a denominator of 100. The word "percent" literally means "per hundred."

$$45\% = \frac{45}{100} = 0.45$$

**Common percentage benchmarks:**

| Percentage | Fraction | Decimal | Meaning |
|-----------|----------|---------|---------|
| 25% | 1/4 | 0.25 | One quarter |
| 50% | 1/2 | 0.50 | One half |
| 75% | 3/4 | 0.75 | Three quarters |
| 10% | 1/10 | 0.10 | One tenth |
| 1% | 1/100 | 0.01 | One hundredth |
| 200% | 2/1 | 2.00 | Twice the whole |
| 0.5% | 1/200 | 0.005 | Half a percent |

**Finding a percentage of a quantity:**

$$\text{Value} = \text{Quantity} \times \frac{\text{Percentage}}{100}$$

```
Example:  What is 35% of 240?

   240 × (35/100) = 240 × 0.35 = 84
```

**Python:**

```python
def percentage_of(quantity, percent):
    return quantity * percent / 100

print(percentage_of(240, 35))    # 84.0
print(percentage_of(150, 7.5))   # 11.25
```

#### Converting Between Percent, Fraction, and Decimal

Three equivalent representations of the same value:

```
   Decimal → Percent:  multiply by 100, add %
   Percent → Decimal:  divide by 100, remove %
   Decimal → Fraction: construct Fraction(numerator, denominator)
   Fraction → Percent: divide numerator by denominator, multiply by 100
```

| Decimal | Fraction | Percent |
|---------|----------|---------|
| 0.75 | 3/4 | 75% |
| 0.125 | 1/8 | 12.5% |
| 0.333... | 1/3 | 33.33% |
| 1.5 | 3/2 | 150% |

| From → To | Method | Example |
|-----------|--------|---------|
| Decimal → Percent | Multiply by 100, add `%` | $0.83 \times 100 = 83\%$ |
| Percent → Decimal | Divide by 100 (remove `%`) | $45\% \div 100 = 0.45$ |
| Fraction → Percent | Divide numerator by denominator, multiply by 100 | $\frac{3}{8} = 0.375 = 37.5\%$ |
| Percent → Fraction | Place over 100, simplify | $60\% = \frac{60}{100} = \frac{3}{5}$ |

**Python:**

#### Percentage Increase and Decrease

**Percentage increase:** multiply the original by $(1 + \text{percent}/100)$.

$$\text{New value} = \text{Original} \times \left(1 + \frac{\text{Percent increase}}{100}\right)$$

```
Example:  A price increases from $80 by 15%.
   New price = 80 × 1.15 = $92
```

**Percentage decrease:** multiply the original by $(1 - \text{percent}/100)$.

$$\text{New value} = \text{Original} \times \left(1 - \frac{\text{Percent decrease}}{100}\right)$$

```
Example:  A price decreases from $200 by 25%.
   New price = 200 × 0.75 = $150
```

**Calculating the percentage change between two values:**

$$\text{Percent change} = \frac{\text{New} - \text{Original}}{\text{Original}} \times 100$$

```
Example:  A value goes from 50 to 75.
   Change = (75 - 50) / 50 × 100 = 50%
```

**Python:**

```python
def percent_increase(original, percent):
    return original * (1 + percent / 100)

def percent_decrease(original, percent):
    return original * (1 - percent / 100)

def percent_change(original, new):
    return (new - original) / original * 100

print(f"${percent_increase(80, 15):.2f}")    # $92.00
print(f"${percent_decrease(200, 25):.2f}")   # $150.00
print(f"{percent_change(50, 75):.1f}%")       # 50.0%
```

---

### Ratios

#### What Ratios Are

A **ratio** compares two or more quantities. It expresses the relative size of each part.

```
Ratio notation:  a : b    or    a/b    or    "a to b"

Example:  If a codebase has 300 Python lines and 200 JavaScript lines:
   Python : JavaScript = 300 : 200 = 3 : 2
```

A ratio is structurally identical to a fraction — it is a comparison of two numbers via division. The ratio $a : b$ is equivalent to the fraction $\frac{a}{b}$.

**Types of ratios:**

| Type | Example | Meaning |
|------|---------|---------|
| **Part-to-part** | 3:2 | For every 3 of A, there are 2 of B |
| **Part-to-whole** | 3:5 | 3 parts out of a total of 5 parts |
| **Rate** | 60 km/h | One quantity per unit of another |

**Simplifying ratios** follows the same process as simplifying fractions — divide all parts by their GCD:

```
Simplify 45 : 30 → GCD(45, 30) = 15 → 3 : 2
```

**Python:**

```python
from math import gcd

def simplify_ratio(a, b):
    g = gcd(a, b)
    return a // g, b // g

print(simplify_ratio(45, 30))    # (3, 2)
print(simplify_ratio(100, 250))  # (2, 5)
```

#### Proportional Reasoning

**Proportions** state that two ratios are equal:

$$\frac{a}{b} = \frac{c}{d}$$

This means $a \times d = b \times c$ (cross-multiplication).

```
If 5 servers handle 10,000 requests, how many for 30,000?
   5/10000 = x/30000 → x = 5 × 30000 / 10000 = 15 servers.
```

**Scale factors** — when all parts of a ratio are multiplied by the same number:

```
Original ratio:     3 : 4    (width : height)
Scale by factor 2:  6 : 8
Scale by factor 3:  9 : 12
```

**Common programming applications of ratios:**

| Application | Ratio type | Example |
|-------------|-----------|---------|
| Aspect ratio (images/video) | Part-to-part | 16:9, 4:3 |
| Load balancing | Part-to-whole | Server capacity per node |
| Mixing ratios (config) | Part-to-part | 3:1 compression ratio |
| Probabilities | Part-to-whole | 3 favorable : 5 total |
| Pixel density | Rate | 300 DPI |

**Python — solving proportions:**

```python
def solve_proportion(a, b, c, solve_for="d"):
    """Solve a/b = c/d for the unknown variable."""
    if solve_for == "d":
        return b * c / a
    elif solve_for == "c":
        return a * d / b

# 5 servers → 10,000 req; how many for 30,000?
print(solve_proportion(10000, 5, 30000))  # 15.0
```

---

### Floating-Point in Programming

#### Why 0.1 + 0.2 ≠ 0.3

This is one of the most famous results in computer science:

```python
>>> 0.1 + 0.2
0.30000000000000004
```

The reason is **floating-point representation**. Computers store decimal numbers in binary (base 2), but not all decimal fractions have exact binary representations.

**The core issue:** the decimal fraction 0.1 in binary is a repeating fraction, just as $\frac{1}{3}$ is a repeating decimal:

```
   Decimal 0.1 in binary:

   0.1 (decimal) = 0.000110011001100110011... (binary, repeating)

   This cannot be represented exactly in a finite number of bits.
   The computer stores the closest approximation.
```

**IEEE 754** is the standard that defines how floating-point numbers are stored. A `float64` (Python's default float) uses 64 bits: 1 sign bit, 11 exponent bits, and 52 mantissa (fraction) bits. The 52-bit mantissa provides roughly 15–17 significant decimal digits of precision.

```python
from decimal import Decimal

# Float: imprecise
print(0.1 + 0.2)                 # 0.30000000000000004

# Decimal: precise
print(Decimal("0.1") + Decimal("0.2"))  # 0.3

# Construct from strings, never from floats
a = Decimal("0.1")
b = Decimal("0.2")
print(a + b == Decimal("0.3"))   # True
```

**The `fractions` module preserves exact rational arithmetic:**

```python
from fractions import Fraction

a = Fraction(1, 10)
b = Fraction(2, 10)
print(a + b)                     # 3/10
print(a + b == Fraction(3, 10))  # True
```

#### Integer Division vs. Float Division

Python 3 distinguishes between two division operators:

| Operator | Name | Result | Example |
|----------|------|--------|---------|
| `/` | True division | Always returns float | `7 / 2 = 3.5` |
| `//` | Floor division | Truncates toward negative infinity | `7 // 2 = 3` |

```python
# True division — always returns float
print(7 / 2)      # 3.5
print(10 / 4)     # 2.5

# Floor division — truncates toward negative infinity
print(7 // 2)     # 3
print(-7 // 2)    # -4  (not -3!)
```

**Why floor division rounds toward negative infinity (not toward zero):**

This maintains the mathematical identity:

$$a = (a // b) \times b + (a \% b)$$

```
   7 // 2 = 3,   7 % 2 = 1    →  3 × 2 + 1 = 7  ✓
  -7 // 2 = -4,  -7 % 2 = 1   → -4 × 2 + 1 = -7  ✓
```

#### Practical Strategies for Precision

**Rule 1: Use `Decimal` for financial calculations, never floats.**

```python
from decimal import Decimal
price = Decimal("0.1") + Decimal("0.2")  # 0.3 — exact
```

**Rule 2: Never compare floats with `==`. Use tolerance.**

```python
import math
a = 0.1 + 0.2
print(math.isclose(a, 0.3))    # True
print(a == 0.3)                # False
```

**Rule 3: Use `Fraction` for exact rational arithmetic.**

```python
from fractions import Fraction
print(Fraction(1, 3) + Fraction(1, 6))  # 1/2 (exact, not 0.4999...)
```

**Rule 4: Round only the final result, never intermediate values.**

```python
# Bad: rounding at each step compounds error
x = round(10 / 3, 2)  # 3.33
y = round(20 / 3, 2)  # 6.67

# Good: round only at the end
result = round((10 / 3) + (20 / 3), 2)  # 10.0
```

**Rule 5: Know the limits of `float64`.**

| Property | Value |
|----------|-------|
| Significant digits | ~15–17 decimal digits |
| Smallest positive normal | ~2.2 × 10⁻³⁰⁸ |
| Largest finite | ~1.8 × 10³⁰⁸ |

---

## Learning Tips

- **Internalize the fraction ↔ decimal ↔ percent equivalence.** Practice expressing the same value in all three forms until conversion is automatic.
- **Use `Decimal` for money.** Floating-point errors accumulate to meaningful amounts — a rounding error of $0.0000001 becomes $0.10 after one billion transactions.
- **Test floating-point comparisons with tolerance.** Use `math.isclose(a, b)`, never `a == b`.
- **Practice cross-multiplication mentally.** Proportional reasoning appears in scaling, ratios, probabilities, and unit conversions.
- **Trace the full precision chain.** When debugging, write down the exact value at each step — the error source often becomes visible at the point of first deviation.
- **Memorize common equivalences:** 1/8 = 0.125 = 12.5%, 1/3 ≈ 0.333 = 33.33%, 3/8 = 0.375 = 37.5%.

## Glossary

| Term | Definition |
|------|------------|
| Cross-multiplication | Method to solve proportions: if $a/b = c/d$, then $ad = bc$ |
| Decimal | A number expressed with a decimal point, extending place value to fractions of a whole |
| Denominator | The bottom number in a fraction, indicating how many equal parts compose the whole |
| Floor division | Division that truncates the result toward negative infinity (`//` in Python) |
| Fraction | A number expressed as a ratio of two integers, $\frac{a}{b}$, where $b \neq 0$ |
| GCD (Greatest Common Divisor) | The largest integer that divides two numbers without remainder |
| Improper fraction | A fraction where the numerator is greater than or equal to the denominator |
| LCD (Least Common Denominator) | The smallest number that is a multiple of two or more denominators |
| Mixed number | A combination of a whole number and a proper fraction (e.g., $2\frac{3}{4}$) |
| Numerator | The top number in a fraction, indicating how many parts are taken |
| Percentage | A fraction expressed as a number out of 100, denoted with the `%` symbol |
| Place value | The value of a digit determined by its position relative to the decimal point |
| Proportion | An equation stating that two ratios are equal |
| Ratio | A comparison of two quantities expressed as $a : b$ or $\frac{a}{b}$ |
| Reciprocal | The result of flipping a fraction: the reciprocal of $\frac{a}{b}$ is $\frac{b}{a}$ |
| Repeating decimal | A decimal in which a finite sequence of digits repeats indefinitely |
| Simplified fraction | A fraction in lowest terms, where numerator and denominator share no common factors other than 1 |
| IEEE 754 | The international standard for floating-point arithmetic representation |
| Floating-point | A method of representing real numbers using a fixed number of significant digits and an exponent |
| Epsilon | A very small tolerance value used for approximate floating-point comparison |

## Quick References

- [Python fractions Module](https://docs.python.org/3/library/fractions.html) — official documentation for exact rational arithmetic
- [Python decimal Module](https://docs.python.org/3/library/decimal.html) — official documentation for decimal fixed-point arithmetic
- [What Every Computer Scientist Should Know About Floating-Point Arithmetic](https://docs.sun.com/source/806-3568/ncg_goldberg.html) — David Goldberg's seminal paper on IEEE 754
- [Khan Academy — Fractions](https://www.khanacademy.org/math/arithmetic/fraction-arithmetic) — interactive lessons on fraction operations
- [Khan Academy — Percentages](https://www.khanacademy.org/math/pre-algebra/percentages-precalc) — comprehensive percentage tutorials
- [IEEE 754-2019 Standard](https://ieeexplore.ieee.org/document/8766229) — the formal specification for floating-point arithmetic

## Next Steps

- [Math for Debugging](math-for-debugging.md) — apply mathematical reasoning to diagnose and fix bugs
- [Propositional Logic](../logic/propositional-logic.md) — formal reasoning, truth tables, and logical equivalence
- [Number Sense](number-sense.md) — revisit foundational number concepts and the number line
