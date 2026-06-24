# Complex Numbers

## Description

Complex numbers ($\mathbb{C}$) extend the real numbers by adding the imaginary unit $i = \sqrt{-1}$. They are essential in signal processing, control systems, quantum mechanics, and computer graphics (fractals, rotations). Every real number is also a complex number.

## Prerequisites

- [Real Numbers](real-numbers.md)

## Table of Contents

- [Definition](#definition)
- [Operations](#operations)
- [Geometric Interpretation](#geometric-interpretation)
- [Complex Numbers in Code](#complex-numbers-in-code)

## Content / Material

### Definition

A complex number has a real part and an imaginary part:

$$ z = a + bi $$

Where $a, b \in \mathbb{R}$ and $i = \sqrt{-1}$ (so $i^2 = -1$).

$$ \mathbb{C} = \\{ a + bi \mid a, b \in \mathbb{R} \\} $$

### Operations

```python
# Complex arithmetic in Python
a = 3 + 2j
b = 1 - 4j

print(a + b)   # (4-2j)
print(a * b)   # (11-10j)
print(a / b)   # (-0.2941176470588235+0.8235294117647058j)
print(abs(a))  # 3.605551275463989 (magnitude)
```

| Operation | Formula |
|-----------|---------|
| Addition | $(a+bi) + (c+di) = (a+c) + (b+d)i$ |
| Multiplication | $(a+bi)(c+di) = (ac-bd) + (ad+bc)i$ |
| Magnitude | $\|a+bi\| = \sqrt{a^2 + b^2}$ |
| Conjugate | $\overline{a+bi} = a - bi$ |

### Geometric Interpretation

Complex numbers live in a 2D plane (Argand diagram):

```
    Im
    ↑
  3 │  • 3 + 2i
  2 │
  1 │
─────┼──────────────────→ Re
    │  1   2   3   4
```

- **Addition** = vector addition in 2D space
- **Multiplication** = rotation + scaling
- **Magnitude** = distance from origin
- **Argument** = angle from positive real axis

### Complex Numbers in Code

```c
// C99 complex numbers
#include <complex.h>
double complex z = 3.0 + 2.0*I;
double complex w = 1.0 - 4.0*I;
double complex sum = z + w;
```

```rust
// Rust num crate
use num::complex::Complex;
let a = Complex::new(3.0, 2.0);
let b = Complex::new(1.0, -4.0);
let product = a * b;
```

```python
import cmath  # complex math module
# cmath is essential for signal processing (FFT, filters)
```

## Glossary

| Term | Definition |
|------|------------|
| Imaginary unit | $i = \sqrt{-1}$, where $i^2 = -1$ |
| Real part | The $a$ in $a + bi$ |
| Imaginary part | The $b$ in $a + bi$ |
| Complex conjugate | Flipping the sign of the imaginary part |
| Argand diagram | A 2D plot of complex numbers on the complex plane |

## Next Steps

- [Vectors & Matrices](../../linear-algebra/beginner/vectors-and-matrices.md) — vectors as ordered lists of numbers
