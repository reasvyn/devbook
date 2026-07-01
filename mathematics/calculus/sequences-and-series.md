# Sequences and Series

## Description

Sequences are ordered lists of numbers; series are sums of sequences. They are essential for understanding algorithm convergence (how fast does gradient descent converge?), approximating functions (Taylor series for computing $\sin x$, $e^x$ in code), and signal processing (Fourier series for decomposing signals into frequencies).

## Prerequisites

- [Limits & Continuity](limits-and-continuity.md) — sequences are limits applied to discrete indices
- [Integration](integration.md) — integral test for convergence, connection to series

## Table of Contents

- [Sequences and Convergence](#sequences-and-convergence)
- [Series and Partial Sums](#series-and-partial-sums)
- [Convergence Tests](#convergence-tests)
- [Power Series](#power-series)
- [Interval of Convergence](#interval-of-convergence)
- [Taylor Series](#taylor-series)
- [Maclaurin Series](#maclaurin-series)
- [Fourier Series Basics](#fourier-series-basics)
- [Applications](#applications)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-References)
- [Next Steps](#next-steps)

## Content / Material

### Sequences and Convergence

A sequence is an ordered list $a_1, a_2, a_3, \dots$ that can be written as $\{a_n\}_{n=1}^{\infty}$.

**Convergence:** $\lim_{n \to \infty} a_n = L$ if for every $\epsilon > 0$, there exists $N$ such that $n > N$ implies $|a_n - L| < \epsilon$.

**Divergence:** A sequence that does not converge diverges. This can mean it goes to $\pm\infty$ or oscillates.

```python
import numpy as np

def sequence_plot(n_terms=50):
    """Generate and examine several sequences."""
    n = np.arange(1, n_terms + 1)

    seq1 = 1 / n                    # converges to 0
    seq2 = n / (n + 1)              # converges to 1
    seq3 = (-1)**n                  # diverges (oscillates)
    seq4 = (1 + 1/n)**n             # converges to e

    print("Convergent to 0:", seq1[-5:])
    print("Convergent to 1:", seq2[-5:])
    print("Oscillating:  ", seq3[-5:])
    print("Convergent to e:", seq4[-5:], f"(expected e = {np.e:.6f})")

sequence_plot()
```

**Properties of convergent sequences:**

- If $\lim a_n = L$ and $\lim b_n = M$, then:
  - $\lim (a_n + b_n) = L + M$
  - $\lim (a_n b_n) = LM$
  - $\lim (a_n / b_n) = L/M$ if $M \neq 0$

**Bounded and monotone sequences:**

- A sequence that is bounded above and monotonically increasing converges.
- A sequence that is bounded below and monotonically decreasing converges.

```python
def is_monotonic(seq):
    """Check if a sequence is monotonic."""
    inc = all(seq[i] <= seq[i+1] for i in range(len(seq)-1))
    dec = all(seq[i] >= seq[i+1] for i in range(len(seq)-1))
    if inc: return "increasing"
    if dec: return "decreasing"
    return "not monotonic"

seq_decreasing = 1 / (1 + np.arange(100))
print(f"1/(1+n) is {is_monotonic(seq_decreasing)}")

# The sequence a_n = (-1)^n / n converges to 0 even though it alternates
seq_alternating = (-1)**np.arange(100) / (1 + np.arange(100))
print(f"Last 5 values: {seq_alternating[-5:]}")
```

### Series and Partial Sums

A series is the sum of a sequence: $\sum_{n=1}^{\infty} a_n$.

The $k$-th partial sum is $S_k = \sum_{n=1}^{k} a_n$.

The series converges if $\lim_{k \to \infty} S_k$ exists (is finite).

**Geometric series:** $\sum_{n=0}^{\infty} ar^n = \frac{a}{1-r}$ for $|r| < 1$.

```python
import numpy as np

def geometric_partial_sum(a, r, n):
    """Compute the nth partial sum of geometric series."""
    if abs(r) < 1:
        return a * (1 - r**n) / (1 - r)
    else:
        return None

a, r = 1, 0.5
for n in [1, 5, 10, 20, 50]:
    s_n = geometric_partial_sum(a, r, n)
    print(f"S_{n:3d} = {s_n:.10f}")

print(f"Limit (n->inf): {a/(1-r):.10f}")
```

**Harmonic series:** $\sum_{n=1}^{\infty} \frac{1}{n}$ diverges, even though the terms approach 0.

```python
import numpy as np

def harmonic_partial_sum(N):
    return np.sum(1.0 / np.arange(1, N + 1))

for N in [10, 100, 1000, 10000, 100000]:
    s = harmonic_partial_sum(N)
    print(f"H_{N:6d} = {s:.6f} (ln({N}) + gamma ≈ {np.log(N) + 0.5772:.6f})")
```

### Convergence Tests

**Divergence test (n-th term test):**
If $\lim_{n \to \infty} a_n \neq 0$, then $\sum a_n$ diverges.

**Integral test:** For decreasing positive $f$, $\sum_{n=1}^{\infty} f(n)$ converges iff $\int_1^{\infty} f(x) \, dx$ converges.

```python
def integral_test(f, n_start=1, tol=1e-10):
    """Check convergence via integral test using numerical integration."""
    from scipy import integrate
    result, err = integrate.quad(f, n_start, np.inf)
    return np.isfinite(result), result

# p-series: ∑ 1/n^p converges if p > 1
for p in [0.5, 1.0, 1.5, 2.0]:
    converges, val = integral_test(lambda x: 1/x**p, 1)
    print(f"p={p:.1f}: converges={converges}, integral={val:.4f}")
```

**Comparison test:** If $0 \leq a_n \leq b_n$ and $\sum b_n$ converges, then $\sum a_n$ converges. If $\sum a_n$ diverges and $a_n \geq b_n \geq 0$, then $\sum b_n$ diverges.

**Limit comparison test:** If $\lim_{n \to \infty} a_n/b_n = c > 0$, then $\sum a_n$ and $\sum b_n$ either both converge or both diverge.

```python
def limit_comparison(a_func, b_func, n_max=1000):
    """Apply limit comparison test numerically."""
    n = np.arange(1, n_max + 1)
    ratio = a_func(n) / b_func(n)
    return np.mean(ratio[-100:])  # approximate limit

# Compare ∑ 1/(n^2 + 2n + 1) with ∑ 1/n^2
ratio = limit_comparison(lambda n: 1/(n**2 + 2*n + 1), lambda n: 1/n**2)
print(f"Ratio a_n/b_n ≈ {ratio:.4f} — both converge (p-series with p=2)")
```

**Ratio test:** $\lim_{n \to \infty} \left| \frac{a_{n+1}}{a_n} \right| = L$.
- If $L < 1$, the series converges absolutely.
- If $L > 1$, the series diverges.
- If $L = 1$, the test is inconclusive.

```python
def ratio_test_approx(a_func, n_max=1000):
    """Approximate the ratio test limit."""
    n = np.arange(1, n_max + 1)
    a_n = a_func(n)
    a_n1 = a_func(n + 1)
    ratios = np.abs(a_n1[:-1] / a_n[:-1])
    return ratios[-1]

# ∑ n!/n^n converges (ratio test gives L = 1/e < 1)
def a_func(n):
    return np.math.factorial(n) / n**n

ratio = ratio_test_approx(a_func)
print(f"Ratio test for n!/n^n: L ≈ {ratio:.4f} (expected 1/e ≈ {1/np.e:.4f})")
```

**Root test:** $\lim_{n \to \infty} \sqrt[n]{|a_n|} = L$. Same criteria as ratio test.

### Power Series

A power series is $\sum_{n=0}^{\infty} c_n (x - a)^n$. It represents a function within its interval of convergence.

$$
f(x) = \sum_{n=0}^{\infty} c_n (x - a)^n
$$

```python
import sympy as sp

x = sp.Symbol('x')
# Represent 1/(1-x) as a power series
n = sp.Symbol('n', integer=True, nonnegative=True)
series_1_over_1mx = sp.summation(x**n, (n, 0, sp.oo))
print(f"Sum x^n = {series_1_over_1mx} (for |x| < 1)")

# Truncated series approximation
def power_series_approx(x, n_terms=10):
    """Approximate 1/(1-x) using first n_terms of its power series."""
    return sum(x**n for n in range(n_terms))

x_test = 0.5
exact = 1 / (1 - x_test)
approx = power_series_approx(x_test, 10)
print(f"exact={exact:.10f}, approx={approx:.10f}, error={abs(exact-approx):.2e}")
```

**Radius of convergence:** The value $R$ such that the series converges for $|x - a| < R$ and diverges for $|x - a| > R$.

$$
R = \frac{1}{\limsup_{n \to \infty} \sqrt[n]{|c_n|}} \quad \text{or} \quad R = \lim_{n \to \infty} \left| \frac{c_n}{c_{n+1}} \right|
$$

### Interval of Convergence

The interval where a power series converges. Check endpoints separately using other convergence tests.

**Example:** $\sum_{n=1}^{\infty} \frac{x^n}{n}$ has $R = 1$.

- At $x = 1$: $\sum 1/n$ diverges (harmonic series).
- At $x = -1$: $\sum (-1)^n/n$ converges (alternating harmonic series).
- Interval of convergence: $[-1, 1)$.

```python
import numpy as np

def power_series_sum(c_func, x, n_terms=1000):
    """Evaluate truncated power series."""
    n = np.arange(0, n_terms)
    c_n = c_func(n)
    return np.sum(c_n * x**n)

# ∑ x^n / n, interval [-1, 1)
c_func = lambda n: 1 / (n + 1) if n >= 0 else 0
for x_test in [0.5, 0.9, -0.9, -1.0, 1.0]:
    val = power_series_sum(c_func, x_test, n_terms=10000)
    print(f"x={x_test:+.1f}: sum ≈ {val:.6f}")
```

### Taylor Series

The Taylor series of $f$ centered at $a$:

$$
f(x) = \sum_{n=0}^{\infty} \frac{f^{(n)}(a)}{n!} (x - a)^n
$$

```python
import sympy as sp
import numpy as np

x = sp.Symbol('x')
a = sp.Symbol('a')

# Taylor series of sin(x) at x=0
f = sp.sin(x)
taylor_sin = sp.series(f, x, 0, 10).removeO()
print(f"sin(x) ≈ {sp.latex(taylor_sin)}")

# Taylor series of exp(x) at x=0
f_exp = sp.exp(x)
taylor_exp = sp.series(f_exp, x, 0, 10).removeO()
print(f"exp(x) ≈ {sp.latex(taylor_exp)}")
```

**Taylor's theorem with remainder:** The error after $n$ terms is bounded by:

$$
|R_n(x)| \leq \frac{M}{(n+1)!} |x - a|^{n+1}
$$

where $M = \max |f^{(n+1)}(\xi)|$ on the interval between $x$ and $a$.

```python
def taylor_sin(x, n_terms=5):
    """Approximate sin(x) using Taylor series about 0."""
    result = 0
    for n in range(n_terms):
        term = (-1)**n * x**(2*n + 1) / np.math.factorial(2*n + 1)
        result += term
    return result

def taylor_error_bound(x, n_terms):
    """Error bound for sin(x) Taylor series: |R_n| ≤ |x|^(n+1)/(n+1)!"""
    return abs(x)**(n_terms + 1) / np.math.factorial(n_terms + 1)

x_test = 0.5
exact = np.sin(x_test)
approx = taylor_sin(x_test, 5)
error_bound = taylor_error_bound(x_test, 5)
print(f"exact={exact:.10f}, approx={approx:.10f}")
print(f"actual error={abs(exact-approx):.2e}, bound={error_bound:.2e}")
```

### Maclaurin Series

A Maclaurin series is a Taylor series centered at $a = 0$.

**Common Maclaurin series:**

| Function | Series | Convergence |
|----------|--------|-------------|
| $e^x$ | $\sum_{n=0}^{\infty} \frac{x^n}{n!}$ | All $\mathbb{R}$ |
| $\sin x$ | $\sum_{n=0}^{\infty} \frac{(-1)^n x^{2n+1}}{(2n+1)!}$ | All $\mathbb{R}$ |
| $\cos x$ | $\sum_{n=0}^{\infty} \frac{(-1)^n x^{2n}}{(2n)!}$ | All $\mathbb{R}$ |
| $\frac{1}{1-x}$ | $\sum_{n=0}^{\infty} x^n$ | $|x| < 1$ |
| $\ln(1+x)$ | $\sum_{n=1}^{\infty} \frac{(-1)^{n+1} x^n}{n}$ | $(-1, 1]$ |
| $\arctan x$ | $\sum_{n=0}^{\infty} \frac{(-1)^n x^{2n+1}}{2n+1}$ | $|x| \leq 1$ |

```python
import numpy as np

def maclaurin_exp(x, n_terms=10):
    """Approximate exp(x) using Maclaurin series."""
    result = 0
    term = 1  # n=0: x^0/0!
    for n in range(n_terms):
        result += term
        term *= x / (n + 1)  # next term: x^(n+1)/(n+1)!
    return result

for x_test in [-2, -1, 0, 1, 2]:
    approx = maclaurin_exp(x_test, 15)
    exact = np.exp(x_test)
    print(f"exp({x_test:+.0f}) ≈ {approx:.10f}, exact={exact:.10f}, err={abs(approx-exact):.2e}")
```

**Operation on series:** Series can be added, multiplied, composed, and differentiated term-by-term within their radius of convergence.

```python
import sympy as sp

x = sp.Symbol('x')

# sin(x) * cos(x) via series
sin_series = sp.series(sp.sin(x), x, 0, 10).removeO()
cos_series = sp.series(sp.cos(x), x, 0, 10).removeO()
product = sp.series(sin_series * cos_series, x, 0, 10).removeO()
# This should match 1/2 * sin(2x) series
print(f"sin(x)*cos(x) ≈ {sp.latex(product)}")

# Series of composition: sin(x^2)
composed = sp.series(sp.sin(x**2), x, 0, 10).removeO()
print(f"sin(x^2) ≈ {sp.latex(composed)}")
```

### Fourier Series Basics

While Taylor series approximate using polynomials, Fourier series approximate using sines and cosines — better for periodic functions.

$$
f(x) = a_0 + \sum_{n=1}^{\infty} a_n \cos\left(\frac{2\pi n x}{T}\right) + b_n \sin\left(\frac{2\pi n x}{T}\right)
$$

**Coefficients:**

$$
a_0 = \frac{1}{T} \int_0^T f(x) \, dx
$$

$$
a_n = \frac{2}{T} \int_0^T f(x) \cos\left(\frac{2\pi n x}{T}\right) \, dx
$$

$$
b_n = \frac{2}{T} \int_0^T f(x) \sin\left(\frac{2\pi n x}{T}\right) \, dx
$$

```python
import numpy as np
from scipy import integrate

def fourier_coeffs(f, T, N=10):
    """Compute first N Fourier coefficients of periodic f with period T."""
    a0 = (1/T) * integrate.quad(lambda x: f(x), 0, T)[0]
    an = np.zeros(N)
    bn = np.zeros(N)

    for n in range(1, N + 1):
        an[n-1] = (2/T) * integrate.quad(lambda x: f(x) * np.cos(2*np.pi*n*x/T), 0, T)[0]
        bn[n-1] = (2/T) * integrate.quad(lambda x: f(x) * np.sin(2*np.pi*n*x/T), 0, T)[0]

    return a0, an, bn

def fourier_approx(x, T, a0, an, bn):
    """Evaluate Fourier series approximation at x."""
    result = a0
    for n in range(1, len(an) + 1):
        result += an[n-1] * np.cos(2*np.pi*n*x/T)
        result += bn[n-1] * np.sin(2*np.pi*n*x/T)
    return result

# Square wave
def square_wave(x):
    return 1 if (x % 1) < 0.5 else -1

T = 1
a0, an, bn = fourier_coeffs(square_wave, T, N=10)
print(f"a0 = {a0:.4f}")
print(f"Non-zero bn terms: {[(i+1, b) for i, b in enumerate(bn) if abs(b) > 0.01]}")

# Evaluate approximation
xs = np.linspace(0, 2, 200)
approx_vals = [fourier_approx(x, T, a0, an, bn) for x in xs]
print(f"Fourier approx at x=0.25: {fourier_approx(0.25, T, a0, an, bn):.4f} (expected 1)")
print(f"Fourier approx at x=0.75: {fourier_approx(0.75, T, a0, an, bn):.4f} (expected -1)")
```

**Gibbs phenomenon:** Near discontinuities, the Fourier series overshoots by about 9%. This is not a bug — it is a fundamental property of Fourier series.

### Applications

**Function approximation in computing:** Computers compute $\sin x$, $e^x$, $\log x$ using truncated Taylor series or Chebyshev polynomials.

```python
import numpy as np

def rel_error(approx, exact):
    return abs(approx - exact) / abs(exact)

# How many terms to compute e^x to machine precision?
def terms_needed_for_exp(x, tol=1e-16):
    result = 1
    term = 1
    for n in range(1, 100):
        term *= x / n
        result += term
        if abs(term) < tol * abs(result):
            return n + 1
    return None

for x in [-10, -1, 1, 10]:
    n = terms_needed_for_exp(x)
    print(f"exp({x:+.0f}): {n} terms needed")
```

**Rate of convergence of gradient descent:** The convergence of gradient descent depends on the condition number $\kappa = \lambda_{\max} / \lambda_{\min}$ of the Hessian. The error decays as a geometric series:

$$
f(x_k) - f(x^*) \leq C \left( \frac{\kappa - 1}{\kappa + 1} \right)^{2k}
$$

```python
def gradient_descent_convergence_rate(kappa, k):
    """Return convergence factor for gradient descent after k steps."""
    r = (kappa - 1) / (kappa + 1)
    return r ** (2 * k)

for kappa in [1, 10, 100, 1000]:
    rate_10 = gradient_descent_convergence_rate(kappa, 10)
    print(f"kappa={kappa:4d}: error reduction after 10 steps = {rate_10:.2e}")
```

**Signal processing with Fourier series:** Decomposing a signal into frequencies.

```python
import numpy as np

# Create a signal from sum of two sine waves
t = np.linspace(0, 1, 1000)
signal = np.sin(2 * np.pi * 5 * t) + 0.5 * np.sin(2 * np.pi * 13 * t)

# The Fourier series coefficients reveal the frequency components
# (In practice, use FFT: np.fft.fft)
fft_coeffs = np.fft.fft(signal)
freqs = np.fft.fftfreq(len(t), t[1] - t[0])

# Find dominant frequencies
magnitudes = np.abs(fft_coeffs)
dominant_idx = np.argsort(magnitudes)[-5:]
print("Dominant frequencies:", freqs[dominant_idx])
```

**Taylor series in machine learning:**

The Taylor expansion of the loss function around the current parameters guides optimization:

$$
L(\theta + \Delta\theta) \approx L(\theta) + \nabla L(\theta)^T \Delta\theta + \frac{1}{2} \Delta\theta^T H \Delta\theta
$$

First-order methods (SGD) use the gradient term. Second-order methods (Newton, L-BFGS) also use the Hessian term.

```python
import numpy as np

def taylor_approx_loss(L, grad, H, theta, delta):
    """Second-order Taylor approximation of loss."""
    return (L(theta) +
            np.dot(grad(theta), delta) +
            0.5 * np.dot(delta, H(theta) @ delta))

def L_simple(theta):
    return theta[0]**2 + 10*theta[1]**2

def grad_simple(theta):
    return np.array([2*theta[0], 20*theta[1]])

def H_simple(theta):
    return np.diag([2, 20])

theta = np.array([1.0, 1.0])
delta = np.array([-0.1, -0.05])
true = L_simple(theta + delta)
approx = taylor_approx_loss(L_simple, grad_simple, H_simple, theta, delta)
print(f"True: {true:.6f}, 2nd-order approx: {approx:.6f}, err: {abs(true-approx):.2e}")
```

## Glossary

| Term | Definition |
|------|------------|
| Sequence | An ordered list of numbers |
| Series | The sum of a sequence's terms |
| Partial sum | Sum of the first $k$ terms of a series |
| Convergence | The property of approaching a finite limit |
| Divergence | Failure to converge (infinity or oscillation) |
| Geometric series | Series of the form $\sum ar^n$ |
| Harmonic series | Series $\sum 1/n$ — diverges slowly |
| Ratio test | Convergence test using the ratio of consecutive terms |
| Root test | Convergence test using the $n$-th root of terms |
| Power series | Series of the form $\sum c_n (x-a)^n$ |
| Radius of convergence | Distance within which a power series converges |
| Taylor series | Representation of a function as a power series centered at $a$ |
| Maclaurin series | Taylor series centered at $0$ |
| Fourier series | Representation of a periodic function using sines and cosines |
| Gibbs phenomenon | Overshoot of Fourier series at discontinuities |
| Lagrange remainder | Error bound for Taylor approximation |
| Alternating series | Series with alternating signs |
| p-series | Series $\sum 1/n^p$ |
| Condition number | Ratio $\lambda_{\max}/\lambda_{\min}$ affecting gradient descent convergence |

## Quick References

- [Paul's Online Math Notes — Sequences & Series](https://tutorial.math.lamar.edu/Classes/CalcII/SeriesIntro.aspx) — comprehensive reference
- [3Blue1Brown — Taylor Series](https://www.youtube.com/watch?v=3d6DsjIBzJ4) — visual intuition
- [3Blue1Brown — Fourier Series](https://www.youtube.com/watch?v=r6sGWTCMz2k) — visual intuition
- [NumPy FFT Documentation](https://numpy.org/doc/stable/reference/routines.fft.html) — fast Fourier transform
- [SymPy Series](https://docs.sympy.org/latest/modules/series.html) — symbolic series computation

## Next Steps

- [Differential Equations](differential-equations.md) — equations involving derivatives, often solved using series methods
