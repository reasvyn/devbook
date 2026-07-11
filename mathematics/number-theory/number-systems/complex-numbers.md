# Complex Numbers

## Description

Complex numbers ($\mathbb{C}$) extend the real number line into a two-dimensional plane, providing solutions to equations like $x^2 + 1 = 0$ that have no real solutions. They are indispensable in modern computing — signal processing via the FFT, quantum computing amplitudes, fractal generation, 3D graphics rotations (quaternions), AC circuit analysis, and control theory all depend on complex arithmetic. Understanding complex numbers unlocks a unified framework for representing oscillation, rotation, and waves.

## Prerequisites

- [Real Numbers](real-numbers.md) — real number line, absolute value, intervals

## Table of Contents

- [The Need for Complex Numbers](#the-need-for-complex-numbers)
- [Definition of a Complex Number](#definition-of-a-complex-number)
- [Arithmetic of Complex Numbers](#arithmetic-of-complex-numbers)
- [The Complex Conjugate](#the-complex-conjugate)
- [Modulus and Argument](#modulus-and-argument)
- [Polar Form and Euler's Formula](#polar-form-and-eulers-formula)
- [De Moivre's Theorem](#de-moivres-theorem)
- [Roots of Unity](#roots-of-unity)
- [The Complex Plane](#the-complex-plane)
- [Complex Functions](#complex-functions)
- [Applications to Computing](#applications-to-computing)
- [Quaternions: A Brief Extension](#quaternions-a-brief-extension)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Need for Complex Numbers

The equation $x^2 + 1 = 0$ has no solution in $\mathbb{R}$ — no real number squares to $-1$. To solve it, we define the **imaginary unit**:

$$
i = \sqrt{-1}, \quad i^2 = -1
$$

This single extension unlocks solutions to all polynomial equations. The **fundamental theorem of algebra** states that every non-constant polynomial with complex coefficients has at least one complex root. Over $\mathbb{R}$, $x^2 + 1 = 0$ has no roots. Over $\mathbb{C}$, it has two: $x = i$ and $x = -i$.

**Historical development:**

| Period | Contribution |
|--------|-------------|
| 1545 | Cardano solves cubic equations, encountering square roots of negatives |
| 1572 | Bombelli formalizes rules for manipulating imaginary numbers |
| 1748 | Euler publishes Euler's formula $e^{i\theta} = \cos\theta + i\sin\theta$ |
| 1806 | Argand introduces the complex plane |
| 1835 | Hamilton defines complex numbers as ordered pairs of reals |
| 1851 | Cauchy develops complex analysis |

### Definition of a Complex Number

A complex number is an ordered pair of real numbers $(a, b)$ written as:

$$
z = a + bi
$$

where $a = \Re(z)$ is the **real part** and $b = \Im(z)$ is the **imaginary part** (both real numbers).

$$
\Re(z) = a, \quad \Im(z) = b
$$

**Classification:**

| Condition | Classification |
|-----------|---------------|
| $b = 0$ | $z$ is purely real ($z \in \mathbb{R}$) |
| $a = 0, b \neq 0$ | $z$ is purely imaginary |
| $b \neq 0$ | $z$ is non-real complex |

```python
from typing import Tuple

class Complex:
    """A complex number z = a + bi."""
    def __init__(self, a: float, b: float):
        self.real = a
        self.imag = b
    
    def __repr__(self) -> str:
        if self.imag >= 0:
            return f"{self.real} + {self.imag}i"
        return f"{self.real} - {abs(self.imag)}i"
    
    def is_real(self) -> bool:
        return self.imag == 0.0
    
    def is_imaginary(self) -> bool:
        return self.real == 0.0 and self.imag != 0.0

# Python has built-in complex type
z = complex(3, 4)  # 3 + 4j
print(z)           # (3+4j)
print(z.real)      # 3.0
print(z.imag)      # 4.0
```

### Arithmetic of Complex Numbers

**Addition:** $(a + bi) + (c + di) = (a + c) + (b + d)i$

```python
def add(z1: complex, z2: complex) -> complex:
    return complex(z1.real + z2.real, z1.imag + z2.imag)

print(complex(3, 4) + complex(1, 2))  # (4+6j)
```

**Subtraction:** $(a + bi) - (c + di) = (a - c) + (b - d)i$

**Multiplication:** $(a + bi)(c + di) = (ac - bd) + (ad + bc)i$

Derivation: expand and use $i^2 = -1$.

```python
def multiply(z1: complex, z2: complex) -> complex:
    a, b = z1.real, z1.imag
    c, d = z2.real, z2.imag
    return complex(a*c - b*d, a*d + b*c)

print(multiply(complex(3, 4), complex(1, 2)))  # (-5+10j)
print(complex(3, 4) * complex(1, 2))           # (-5+10j)
```

**Division:** $\frac{a + bi}{c + di} = \frac{(a + bi)(c - di)}{(c + di)(c - di)} = \frac{ac + bd}{c^2 + d^2} + \frac{bc - ad}{c^2 + d^2}i$

```python
def divide(z1: complex, z2: complex) -> complex:
    """Divide z1 by z2 using the conjugate."""
    a, b = z1.real, z1.imag
    c, d = z2.real, z2.imag
    den = c*c + d*d
    return complex((a*c + b*d)/den, (b*c - a*d)/den)

print(divide(complex(3, 4), complex(1, 2)))  # (2.2-0.4j)
```

**Geometric interpretation of multiplication:** Multiplying by $i$ rotates a point $90^\circ$ counterclockwise about the origin.

| Multiplication | Effect on complex plane |
|---------------|------------------------|
| $z \cdot i$ | Rotate $90^\circ$ CCW |
| $z \cdot (-i)$ | Rotate $90^\circ$ CW |
| $z \cdot (-1)$ | Rotate $180^\circ$ |
| $z \cdot e^{i\theta}$ | Rotate by $\theta$ |

```python
z = complex(1, 0)
print(z * 1j)        # 1j  (rotation 90° CCW)
print(z * 1j * 1j)   # (-1+0j) (rotation 180°)
print(z * 1j**3)     # (-0-1j) (rotation 270°)
```

### The Complex Conjugate

The **complex conjugate** of $z = a + bi$ is:

$$
\bar{z} = a - bi
$$

Geometrically, conjugation reflects across the real axis.

**Properties:**

| Property | Formula |
|----------|---------|
| Conjugate of sum | $\overline{z_1 + z_2} = \bar{z}_1 + \bar{z}_2$ |
| Conjugate of product | $\overline{z_1 \cdot z_2} = \bar{z}_1 \cdot \bar{z}_2$ |
| Conjugate of conjugate | $\overline{\bar{z}} = z$ |
| Real check | $z \in \mathbb{R} \iff \bar{z} = z$ |
| Imaginary check | $z$ is purely imaginary $\iff \bar{z} = -z$ |
| Real part | $\Re(z) = \frac{z + \bar{z}}{2}$ |
| Imaginary part | $\Im(z) = \frac{z - \bar{z}}{2i}$ |

```python
def conjugate(z: complex) -> complex:
    return complex(z.real, -z.imag)

z = complex(3, 4)
print(conjugate(z))  # (3-4j)
print(z.conjugate()) # (3-4j) — built-in method

# Real and imaginary parts via conjugate
def real_part(z: complex) -> float:
    return (z + z.conjugate()) / 2

def imag_part(z: complex) -> float:
    return (z - z.conjugate()) / (2j)
```

**Application: rationalizing denominators.** The conjugate is used to divide complex numbers, as shown above.

### Modulus and Argument

The **modulus** (absolute value, magnitude) of $z = a + bi$ is its distance from the origin:

$$
|z| = \sqrt{a^2 + b^2}
$$

**Properties:**

| Property | Formula |
|----------|---------|
| Non-negativity | $|z| \ge 0$, equality iff $z = 0$ |
| Multiplicativity | $|z_1 z_2| = |z_1||z_2|$ |
| Triangle inequality | $|z_1 + z_2| \le |z_1| + |z_2|$ |
| Conjugate relation | $|z|^2 = z \bar{z}$ |
| Reciprocal | $|1/z| = 1/|z|$ for $z \neq 0$ |

The **argument** (angle, phase) of $z = a + bi$ is the angle from the positive real axis to the point $(a, b)$:

$$
\arg(z) = \tan^{-1}\left(\frac{b}{a}\right), \quad \text{adjusted for quadrant}
$$

The argument is multi-valued: if $\theta$ is an argument, then $\theta + 2\pi k$ is also an argument for any integer $k$. The **principal argument** $\operatorname{Arg}(z)$ lies in $(-\pi, \pi]$ (or $[0, 2\pi)$, depending on convention).

```python
import math

def modulus(z: complex) -> float:
    return math.sqrt(z.real**2 + z.imag**2)

def argument(z: complex) -> float:
    """Principal argument in (-pi, pi]."""
    return math.atan2(z.imag, z.real)

z = complex(1, 1)
print(f"|z| = {modulus(z):.4f}")           # 1.4142
print(f"arg(z) = {argument(z):.4f} rad")   # 0.7854 (pi/4)

# Python's built-in
print(abs(z))              # 1.4142135623730951
import cmath
print(cmath.phase(z))      # 0.7853981633974483
```

**Quadrant-aware argument:**

| Quadrant | Sign pattern | Argument range |
|----------|-------------|----------------|
| I | $a > 0, b > 0$ | $0 < \theta < \pi/2$ |
| II | $a < 0, b > 0$ | $\pi/2 < \theta < \pi$ |
| III | $a < 0, b < 0$ | $-\pi < \theta < -\pi/2$ |
| IV | $a > 0, b < 0$ | $-\pi/2 < \theta < 0$ |

### Polar Form and Euler's Formula

A complex number can be expressed in **polar form** using modulus $r$ and argument $\theta$:

$$
z = r(\cos\theta + i\sin\theta)
$$

**Euler's formula** connects exponentials to trigonometry:

$$
e^{i\theta} = \cos\theta + i\sin\theta
$$

This gives the compact polar form:

$$
z = re^{i\theta}
$$

**Special cases of Euler's formula:**

$$
e^{i\pi} + 1 = 0
$$

This is Euler's identity, relating the five fundamental constants: $e$, $i$, $\pi$, $1$, and $0$.

| $\theta$ | $e^{i\theta}$ | Meaning |
|----------|---------------|---------|
| $0$ | $1$ | Positive real axis |
| $\pi/2$ | $i$ | Positive imaginary axis |
| $\pi$ | $-1$ | Negative real axis |
| $3\pi/2$ | $-i$ | Negative imaginary axis |
| $2\pi$ | $1$ | Full rotation |

```python
import cmath

def polar(z: complex) -> tuple[float, float]:
    """Return (modulus, argument)."""
    return abs(z), cmath.phase(z)

def from_polar(r: float, theta: float) -> complex:
    """Create complex number from polar coordinates."""
    return r * cmath.exp(1j * theta)

z = from_polar(2.0, math.pi / 3)
print(f"z = {z:.4f}")  # (1.0000+1.7321i) = 2(cos(60°) + i sin(60°))
print(f"Check: 2 * (cos(pi/3) + i sin(pi/3)) = 2*(0.5 + i*0.8660)")

# Euler's identity
print(cmath.exp(1j * math.pi) + 1)  # (0+1.2246467991473532e-16j) — ~0
```

**Conversion formulas:**

| Cartesian to polar | Polar to Cartesian |
|--------------------|--------------------|
| $r = \sqrt{a^2 + b^2}$ | $a = r\cos\theta$ |
| $\theta = \text{atan2}(b, a)$ | $b = r\sin\theta$ |

**Multiplication in polar form:** Multiply moduli, add arguments.

$$
(r_1 e^{i\theta_1})(r_2 e^{i\theta_2}) = r_1 r_2 e^{i(\theta_1 + \theta_2)}
$$

```python
z1 = 2 * cmath.exp(1j * math.pi / 6)   # 2 * e^(i*pi/6)
z2 = 3 * cmath.exp(1j * math.pi / 3)   # 3 * e^(i*pi/3)
prod = z1 * z2
print(abs(prod))       # 6.0 = 2*3
print(cmath.phase(prod))  # 1.5708 = pi/2 = pi/6 + pi/3
```

### De Moivre's Theorem

For any complex number $z = re^{i\theta}$ and integer $n$:

$$
z^n = (re^{i\theta})^n = r^n e^{in\theta} = r^n(\cos n\theta + i\sin n\theta)
$$

This is **De Moivre's theorem**. It follows directly from the polar form and Euler's formula.

```python
def de_moivre(z: complex, n: int) -> complex:
    """Raise complex z to integer power n using De Moivre."""
    r, theta = polar(z)
    return from_polar(r ** n, theta * n)

z = complex(1, 1)  # sqrt(2) * e^(i*pi/4)
z5 = de_moivre(z, 5)
print(f"(1+i)^5 = {z5}")  # (-4-4j)

# Direct computation for verification
print((1+1j)**5)  # (-4-4j)
```

**Applications:**
- Computing powers of complex numbers efficiently (avoids expanding binomials)
- Deriving trigonometric identities: $\cos(3\theta) = 4\cos^3\theta - 3\cos\theta$
- Solving polynomial equations

**Trigonometric identities via De Moivre:**

$$
(\cos\theta + i\sin\theta)^n = \cos n\theta + i\sin n\theta
$$

Expanding the left side with the binomial theorem and equating real and imaginary parts yields:

$$
\cos 2\theta = \cos^2\theta - \sin^2\theta, \quad \sin 2\theta = 2\sin\theta\cos\theta
$$

```python
# cos(nθ) can be computed as Re(e^{iθ}^n):
print((cmath.exp(1j * math.pi/6) ** 3).real)  # cos(pi/2) ≈ 0
```

### Roots of Unity

The **$n$th roots of unity** are the solutions to $z^n = 1$:

$$
z_k = e^{2\pi i k / n} = \cos\frac{2\pi k}{n} + i\sin\frac{2\pi k}{n}, \quad k = 0, 1, \ldots, n-1
$$

They form a regular $n$-gon on the unit circle, with one vertex at $1$.

```python
def roots_of_unity(n: int) -> list[complex]:
    """Return all n-th roots of unity."""
    return [cmath.exp(2j * math.pi * k / n) for k in range(n)]

roots = roots_of_unity(5)
for k, z in enumerate(roots):
    print(f"  z_{k} = {z:.4f}, |z| = {abs(z):.1f}")
```

**Primitive roots:** An $n$th root of unity $\omega$ is **primitive** if $\omega^k \neq 1$ for any $k < n$. The primitive $n$th roots are $e^{2\pi i k / n}$ where $\gcd(k, n) = 1$.

**The DFT matrix:** The discrete Fourier transform uses roots of unity. The $n \times n$ DFT matrix has entries:

$$
F_{jk} = \omega_n^{jk}, \quad \omega_n = e^{-2\pi i / n}
$$

```python
import numpy as np

def dft_matrix(n: int) -> np.ndarray:
    """Build the n x n DFT matrix."""
    omega = cmath.exp(-2j * math.pi / n)
    return np.array([[omega ** (j * k) for k in range(n)] for j in range(n)])

# DFT of a signal
def dft(signal: list[complex]) -> list[complex]:
    """Naive O(n^2) DFT."""
    n = len(signal)
    omega = cmath.exp(-2j * math.pi / n)
    return [sum(signal[k] * (omega ** (j * k)) for k in range(n)) for j in range(n)]

# The FFT exploits roots of unity symmetries to compute this in O(n log n)
```

**FFT connection:** The Fast Fourier Transform factorizes the DFT matrix using the property $\omega_n^{k + n/2} = -\omega_n^k$ (for even $n$). This divides-and-conquers the computation from $O(n^2)$ to $O(n \log n)$, which makes real-time signal processing possible.

### The Complex Plane

The **complex plane** (Argand diagram) plots $\Re(z)$ on the $x$-axis and $\Im(z)$ on the $y$-axis.

$$
z = a + bi \quad \longleftrightarrow \quad \text{point } (a, b) \in \mathbb{R}^2
$$

```python
def plot_complex(axes, z: complex, label: str = "", **kwargs):
    """Plot a complex number on an Argand diagram."""
    axes.plot([0, z.real], [0, z.imag], **kwargs)
    axes.scatter(z.real, z.imag, **kwargs)
    if label:
        axes.annotate(label, (z.real, z.imag))

# Conceptual plotting — actual rendering would use matplotlib
```

**Key geometric operations:**

| Operation | Geometric meaning |
|-----------|-------------------|
| $z + w$ | Vector addition (parallelogram) |
| $z - w$ | Vector from $w$ to $z$ |
| $z \cdot e^{i\theta}$ | Rotation by $\theta$ CCW |
| $\bar{z}$ | Reflection across real axis |
| $|z - w|$ | Euclidean distance between $z$ and $w$ |

**The Riemann sphere:** The complex plane can be mapped to a sphere (the Riemann sphere) by stereographic projection. The point at infinity $\infty$ is added, compactifying $\mathbb{C}$ into the **extended complex plane** $\hat{\mathbb{C}} = \mathbb{C} \cup \{\infty\}$.

### Complex Functions

A **complex function** $f: \mathbb{C} \to \mathbb{C}$ maps the complex plane to itself.

**Polynomials:** $P(z) = a_n z^n + a_{n-1} z^{n-1} + \cdots + a_0$

**Exponential:** $e^z = e^{a + bi} = e^a(\cos b + i\sin b)$ — periodic in the imaginary direction!

```python
import cmath

# Complex exponential
z = complex(0, math.pi)
print(cmath.exp(z))  # (-1+0j) = e^(i*pi)

# Complex logarithm (multi-valued)
z = complex(1, 1)
log_z = cmath.log(z)
print(f"log(1+i) = {log_z:.4f}")  # log(sqrt(2)) + i*pi/4
```

**Analytic (holomorphic) functions:** A complex function is analytic if it is complex-differentiable at every point in its domain. Complex differentiability is much stronger than real differentiability — it implies infinite differentiability and power series representation.

**Cauchy-Riemann equations:** For $f(z) = u(x,y) + iv(x,y)$, analyticity requires:

$$
\frac{\partial u}{\partial x} = \frac{\partial v}{\partial y}, \quad \frac{\partial u}{\partial y} = -\frac{\partial v}{\partial x}
$$

```python
def check_cauchy_riemann(f, z: complex, h: float = 1e-8) -> bool:
    """Numerically check Cauchy-Riemann equations at z."""
    x, y = z.real, z.imag
    # Central differences for partial derivatives
    u = lambda x, y: f(complex(x, y)).real
    v = lambda x, y: f(complex(x, y)).imag
    
    ux = (u(x+h, y) - u(x-h, y)) / (2*h)
    uy = (u(x, y+h) - u(x, y-h)) / (2*h)
    vx = (v(x+h, y) - v(x-h, y)) / (2*h)
    vy = (v(x, y+h) - v(x, y-h)) / (2*h)
    
    return abs(ux - vy) < 1e-6 and abs(uy + vx) < 1e-6

# f(z) = z^2 is analytic everywhere
f_sq = lambda z: z**2
print(check_cauchy_riemann(f_sq, complex(1, 1)))  # True

# f(z) = conj(z) is nowhere analytic
f_bar = lambda z: z.conjugate()
print(check_cauchy_riemann(f_bar, complex(1, 1)))  # False
```

**Consequences of analyticity:**
- If $f$ is analytic and bounded on $\mathbb{C}$, it is constant (Liouville's theorem)
- The real and imaginary parts of an analytic function are harmonic: $\nabla^2 u = \nabla^2 v = 0$
- Contour integrals of analytic functions around closed loops are zero (Cauchy integral theorem)

### Applications to Computing

**Fourier transform and the FFT:**

The Fourier transform decomposes a signal into its frequency components using complex exponentials:

$$
\hat{f}(\omega) = \int_{-\infty}^{\infty} f(t) e^{-2\pi i \omega t} dt
$$

The discrete Fourier transform (DFT) operates on sampled data:

$$
X_k = \sum_{n=0}^{N-1} x_n e^{-2\pi i k n / N}
$$

```python
import math

def dft_naive(signal: list[float]) -> list[complex]:
    """Compute the DFT of a real signal (naive O(n^2))."""
    N = len(signal)
    result = []
    for k in range(N):
        s = 0j
        for n in range(N):
            angle = -2 * math.pi * k * n / N
            s += signal[n] * cmath.exp(1j * angle)
        result.append(s)
    return result

# Example: detect frequency components
t = [n / 100 for n in range(100)]  # 100 samples at 100 Hz
signal = [math.sin(2 * math.pi * 5 * x) + 0.5 * math.sin(2 * math.pi * 12 * x) for x in t]
spectrum = dft_naive(signal)
magnitudes = [abs(z) for z in spectrum]
# Peaks at k=5 and k=12 (and their mirrors) indicate 5 Hz and 12 Hz components
```

The FFT reduces complexity from $O(N^2)$ to $O(N \log N)$, enabling real-time audio processing, image compression (JPEG uses DCT — a variant of DFT), and spectral analysis.

**Quantum computing:**

Quantum states are represented as complex vectors (amplitudes) in Hilbert space. A qubit state is:

$$
|\psi\rangle = \alpha|0\rangle + \beta|1\rangle, \quad \alpha, \beta \in \mathbb{C}, \; |\alpha|^2 + |\beta|^2 = 1
$$

```python
# A qubit state as a complex vector
alpha = complex(1/math.sqrt(2), 0)  # |0⟩ amplitude
beta = complex(0, 1/math.sqrt(2))   # |1⟩ amplitude

print(f"|ψ⟩ = ({alpha.real:.4f})|0⟩ + ({beta.imag:.4f}i)|1⟩")
print(f"Probability of |0⟩: {abs(alpha)**2:.4f}")  # 0.5
print(f"Probability of |1⟩: {abs(beta)**2:.4f}")   # 0.5

# Pauli-X gate (quantum NOT) as a unitary matrix
import numpy as np
pauli_x = np.array([[0, 1], [1, 0]], dtype=complex)
state = np.array([alpha, beta])
result = pauli_x @ state
print(f"After X gate: |ψ'⟩ = ({result[0]:.4f})|0⟩ + ({result[1]:.4f})|1⟩")
```

The **Bloch sphere** represents pure qubit states on the surface of a unit sphere:

$$
|\psi\rangle = \cos(\theta/2)|0\rangle + e^{i\phi}\sin(\theta/2)|1\rangle
$$

Quantum gates are unitary matrices $U^\dagger U = I$ — complex matrices whose inverse equals their conjugate transpose.

**Fractals — the Mandelbrot set:**

The Mandelbrot set is defined by iterating $z_{n+1} = z_n^2 + c$ with $z_0 = 0$, where $c$ is a complex number. The point $c$ is in the set if the sequence remains bounded.

```python
def mandelbrot(c: complex, max_iter: int = 1000) -> int:
    """Return the iteration count for point c, or max_iter if in set."""
    z = 0j
    for n in range(max_iter):
        if abs(z) > 2:
            return n
        z = z * z + c
    return max_iter

# Render: iterate over a grid of c values in the complex plane
# The boundary of the Mandelbrot set is a fractal with infinite detail
# Coloring based on iteration count produces the characteristic images
```

The **Julia set** is similar but fixes $c$ and varies $z_0$: the iteration $z_{n+1} = z_n^2 + c$ produces a fractal whose shape depends on the choice of $c$.

**Graphics — rotations with quaternions:**

3D rotations are efficiently represented with quaternions (see below). Complex numbers handle 2D rotations directly: multiplying by $e^{i\theta}$ rotates by angle $\theta$.

```python
def rotate_2d(point: tuple[float, float], angle: float) -> tuple[float, float]:
    """Rotate a 2D point counterclockwise by angle radians."""
    z = complex(point[0], point[1])
    rot = z * cmath.exp(1j * angle)
    return (rot.real, rot.imag)

p = (1.0, 0.0)
p_rot = rotate_2d(p, math.pi / 4)
print(f"(1,0) rotated 45°: {p_rot}")  # (~0.707, ~0.707)
```

**AC circuit analysis:**

In AC circuits, impedance $Z$ is a complex quantity. Resistors have real impedance ($R$), capacitors and inductors have imaginary impedance ($-j/\omega C$ and $j\omega L$ respectively).

| Component | Impedance |
|-----------|-----------|
| Resistor | $Z_R = R$ |
| Capacitor | $Z_C = 1/(j\omega C) = -j/(\omega C)$ |
| Inductor | $Z_L = j\omega L$ |

Series impedance: $Z = Z_1 + Z_2$. Parallel impedance: $1/Z = 1/Z_1 + 1/Z_2$.

```python
import cmath

def impedance_resistor(R: float) -> complex:
    return complex(R, 0)

def impedance_capacitor(C: float, omega: float) -> complex:
    """Capacitive impedance at angular frequency omega."""
    return -1j / (omega * C)

def impedance_inductor(L: float, omega: float) -> complex:
    """Inductive impedance at angular frequency omega."""
    return 1j * omega * L

# RLC series circuit
R, L, C = 100.0, 0.01, 1e-6
omega = 2 * math.pi * 1000  # 1 kHz
Z = impedance_resistor(R) + impedance_inductor(L, omega) + impedance_capacitor(C, omega)
print(f"Total impedance: |Z| = {abs(Z):.2f} Ω, phase = {cmath.phase(Z):.4f} rad")
```

**Control theory:**

Transfer functions in the Laplace $s$-domain are rational functions of the complex variable $s = \sigma + j\omega$. Poles and zeros in the complex plane determine system stability and response.

$$
H(s) = \frac{b_m s^m + b_{m-1} s^{m-1} + \cdots + b_0}{a_n s^n + a_{n-1} s^{n-1} + \cdots + a_0}
$$

A system is stable if all poles have negative real parts (lie in the left half-plane).

```python
def is_stable(numerator_coeffs: list[float], denominator_coeffs: list[float]) -> bool:
    """Check if a transfer function is stable (all poles in left half-plane)."""
    import numpy as np
    poles = np.roots(denominator_coeffs)
    return all(pole.real < 0 for pole in poles)

# Example: H(s) = 1 / (s^2 + 2s + 1) — stable (poles at s = -1)
print(is_stable([1], [1, 2, 1]))  # True

# H(s) = 1 / (s^2 - 1) — unstable (pole at s = 1)
print(is_stable([1], [1, 0, -1]))  # False
```

### Quaternions: A Brief Extension

Quaternions $\mathbb{H}$ (discovered by Hamilton in 1843) extend complex numbers to three imaginary units:

$$
q = a + bi + cj + dk, \quad i^2 = j^2 = k^2 = ijk = -1
$$

**Comparison:**

| Number system | Dimensions | Units | Use |
|---------------|-----------|-------|-----|
| Real $\mathbb{R}$ | 1 | $1$ | Continuous quantities |
| Complex $\mathbb{C}$ | 2 | $1, i$ | 2D rotations, waves |
| Quaternion $\mathbb{H}$ | 4 | $1, i, j, k$ | 3D rotations, graphics |
| Octonion $\mathbb{O}$ | 8 | $1, e_1, \ldots, e_7$ | Theoretical physics |

**Quaternion multiplication** is non-commutative: $ij = k$ but $ji = -k$.

```python
class Quaternion:
    """A quaternion q = w + xi + yj + zk."""
    def __init__(self, w: float, x: float, y: float, z: float):
        self.w, self.x, self.y, self.z = w, x, y, z
    
    def __mul__(self, o: 'Quaternion') -> 'Quaternion':
        w1, x1, y1, z1 = self.w, self.x, self.y, self.z
        w2, x2, y2, z2 = o.w, o.x, o.y, o.z
        return Quaternion(
            w1*w2 - x1*x2 - y1*y2 - z1*z2,
            w1*x2 + x1*w2 + y1*z2 - z1*y2,
            w1*y2 - x1*z2 + y1*w2 + z1*x2,
            w1*z2 + x1*y2 - y1*x2 + z1*w2
        )
    
    def conjugate(self) -> 'Quaternion':
        return Quaternion(self.w, -self.x, -self.y, -self.z)
    
    def norm(self) -> float:
        return math.sqrt(self.w**2 + self.x**2 + self.y**2 + self.z**2)
    
    def inverse(self) -> 'Quaternion':
        n = self.norm()**2
        c = self.conjugate()
        return Quaternion(c.w/n, c.x/n, c.y/n, c.z/n)
    
    def rotate_vector(self, v: tuple[float, float, float]) -> tuple[float, float, float]:
        """Rotate 3D vector v by this unit quaternion."""
        p = Quaternion(0, v[0], v[1], v[2])
        q_inv = self.inverse()
        rotated = self * p * q_inv
        return (rotated.x, rotated.y, rotated.z)
```

**3D rotation:** A unit quaternion $q = \cos(\theta/2) + \mathbf{u}\sin(\theta/2)$ (where $\mathbf{u}$ is a unit axis vector) rotates a vector by $\theta$ around $\mathbf{u}$. This avoids gimbal lock and is the standard in 3D graphics engines.

```python
def rotation_quaternion(axis: tuple[float, float, float], angle: float) -> Quaternion:
    """Create a quaternion for rotation by angle (radians) around axis."""
    norm = math.sqrt(axis[0]**2 + axis[1]**2 + axis[2]**2)
    ux, uy, uz = axis[0]/norm, axis[1]/norm, axis[2]/norm
    s = math.sin(angle / 2)
    c = math.cos(angle / 2)
    return Quaternion(c, ux*s, uy*s, uz*s)

# Rotate (1, 0, 0) by 90° around z-axis → (0, 1, 0)
q = rotation_quaternion((0, 0, 1), math.pi / 2)
v = (1.0, 0.0, 0.0)
print(f"Rotated: {q.rotate_vector(v)}")  # (~0, ~1, ~0)
```

## Glossary

| Term | Definition |
|------|------------|
| Complex number | Number of the form $a + bi$ where $a,b \in \mathbb{R}$, $i^2 = -1$ |
| Imaginary unit | $i = \sqrt{-1}$, satisfies $i^2 = -1$ |
| Real part | $\Re(z) = a$, the real component of $z = a + bi$ |
| Imaginary part | $\Im(z) = b$, the real coefficient of $i$ in $z = a + bi$ |
| Complex conjugate | $\bar{z} = a - bi$, reflection of $z$ across the real axis |
| Modulus | $|z| = \sqrt{a^2 + b^2}$, distance from origin |
| Argument | $\arg(z) = \text{atan2}(b, a)$, angle from positive real axis |
| Polar form | $z = re^{i\theta}$ for modulus $r$ and argument $\theta$ |
| Euler's formula | $e^{i\theta} = \cos\theta + i\sin\theta$ |
| Euler's identity | $e^{i\pi} + 1 = 0$ |
| De Moivre's theorem | $(re^{i\theta})^n = r^n e^{in\theta}$ |
| Roots of unity | Solutions to $z^n = 1$: $e^{2\pi i k/n}$ for $k = 0,\ldots,n-1$ |
| Primitive root | An $n$th root of unity $\omega$ such that $\omega^k \neq 1$ for $k < n$ |
| Argand diagram | Plot of $\Im(z)$ vs $\Re(z)$, aka the complex plane |
| Analytic function | Complex-differentiable at every point in its domain |
| Cauchy-Riemann equations | $\partial u/\partial x = \partial v/\partial y$, $\partial u/\partial y = -\partial v/\partial x$ |
| FFT | Fast Fourier Transform, computes DFT in $O(n \log n)$ |
| Quaternion | $q = w + xi + yj + zk$, $i^2 = j^2 = k^2 = ijk = -1$ |
| Bloch sphere | Unit sphere representation of a qubit state |
| Impedance | Complex resistance in AC circuits |
| Transfer function | Complex rational function in Laplace domain describing system dynamics |

## Quick References

- [Complex Analysis on Wikipedia](https://en.wikipedia.org/wiki/Complex_analysis) — comprehensive reference
- [The FFT Demo](https://www.jezzamon.com/fourier/) — interactive visual introduction to Fourier transforms
- [Visualizing Complex Functions](https://acko.net/blog/how-to-fold-a-julia-fractal/) — intuitive explanation of complex maps
- [Quaternions for 3D Graphics](https://www.3dgep.com/understanding-quaternions/) — practical guide

## Next Steps

- [Floating-Point Numbers](floating-point-numbers.md) — IEEE 754, rounding, and numerical algorithms
- [Number Systems](index.md) — revisit the hierarchy from naturals to complex numbers
