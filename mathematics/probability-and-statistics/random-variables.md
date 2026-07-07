# Random Variables

## Description

A random variable is a numerical summary of a random experiment. Random variables bridge events and real numbers, enabling the use of algebra and calculus for probability. They are essential for simulation, risk analysis, machine learning feature engineering, and quantifying uncertainty in any data-driven system.

## Prerequisites

- [Probability Foundations](probability-foundations.md) — sample spaces, events, conditional probability, Bayes theorem

## Table of Contents

- [Definition and Types](#definition-and-types)
- [Probability Mass Function (PMF)](#probability-mass-function-pmf)
- [Probability Density Function (PDF)](#probability-density-function-pdf)
- [Cumulative Distribution Function (CDF)](#cumulative-distribution-function-cdf)
- [Expectation](#expectation)
- [The Law of the Unconscious Statistician](#the-law-of-the-unconscious-statistician)
- [Properties of Expectation](#properties-of-expectation)
- [Variance and Standard Deviation](#variance-and-standard-deviation)
- [Moments and Moment Generating Functions](#moments-and-moment-generating-functions)
- [Transformations of Random Variables](#transformations-of-random-variables)
- [Inequalities](#inequalities)
- [Applications](#applications)
- [Learning Tips](#learning-tips)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Definition and Types

A **random variable** $X$ is a function $X: \Omega \to \mathbb{R}$ that maps outcomes in the sample space to real numbers.

**Example — Coin flip:** Define $X(H) = 1$, $X(T) = 0$. $X$ counts the number of heads in one flip.

**Example — Two dice sum:** $X$ maps the pair $(i, j)$ to $i + j$.

Random variables come in two flavors:

**Discrete random variables** take a countable number of values (finite or countably infinite). Examples: number of website visitors, count of bugs in a release, binary conversion indicator.

**Continuous random variables** take uncountably many values over an interval. Examples: response time in milliseconds, temperature, revenue.

### Probability Mass Function (PMF)

For a discrete random variable $X$, the **probability mass function** $p(x)$ gives the probability that $X$ equals a specific value:

$$p(x) = P(X = x).$$

Properties:
- $0 \le p(x) \le 1$ for all $x$
- $\sum_{x \in \mathcal{X}} p(x) = 1$, where $\mathcal{X}$ is the support of $X$

**Example — Sum of two dice:**

```python
import numpy as np
from collections import Counter

outcomes = [(i, j) for i in range(1, 7) for j in range(1, 7)]
sums = [i + j for i, j in outcomes]
pmf = Counter(sums)
for s in range(2, 13):
    print(f"P(X={s:2d}) = {pmf[s]:2d}/36 = {pmf[s]/36:.4f}")
```

Output:
```
P(X= 2) =  1/36 = 0.0278
P(X= 3) =  2/36 = 0.0556
P(X= 4) =  3/36 = 0.0833
P(X= 5) =  4/36 = 0.1111
P(X= 6) =  5/36 = 0.1389
P(X= 7) =  6/36 = 0.1667
P(X= 8) =  5/36 = 0.1389
P(X= 9) =  4/36 = 0.1111
P(X=10) =  3/36 = 0.0833
P(X=11) =  2/36 = 0.0556
P(X=12) =  1/36 = 0.0278
```

### Probability Density Function (PDF)

For a continuous random variable $X$, the **probability density function** $f(x)$ is a non-negative function such that for any interval $[a, b]$:

$$P(a \le X \le b) = \int_a^b f(x) \, dx.$$

Properties:
- $f(x) \ge 0$ for all $x$
- $\int_{-\infty}^{\infty} f(x) \, dx = 1$
- $P(X = a) = 0$ for any specific value $a$ (continuous variables have zero probability at any single point)

**Important:** $f(x)$ is not a probability — it is a density. Probabilities come from areas under the curve, not from individual values.

**Example — Uniform distribution on $[0, 1]$:**
$$f(x) = \begin{cases} 1, & 0 \le x \le 1 \\ 0, & \text{otherwise} \end{cases}$$

$$P(0.2 \le X \le 0.6) = \int_{0.2}^{0.6} 1 \, dx = 0.4.$$

### Cumulative Distribution Function (CDF)

The **cumulative distribution function** $F(x)$ gives the probability that $X$ does not exceed $x$:

$$F(x) = P(X \le x).$$

Properties:
- $F$ is non-decreasing
- $\lim_{x \to -\infty} F(x) = 0$
- $\lim_{x \to \infty} F(x) = 1$
- $P(a < X \le b) = F(b) - F(a)$

For discrete $X$: $F(x) = \sum_{x_i \le x} p(x_i)$.
For continuous $X$: $F(x) = \int_{-\infty}^x f(t) \, dt$.

The CDF unifies discrete and continuous random variables. Every random variable has a CDF.

**Inverse CDF (quantile function):** $F^{-1}(p) = \inf\{x : F(x) \ge p\}$. This is used for generating random samples (inverse transform sampling) and computing percentiles.

```python
import numpy as np
import matplotlib.pyplot as plt

# Inverse transform sampling for exponential(lambda=1)
# F(x) = 1 - exp(-x),  F^{-1}(p) = -log(1 - p)
np.random.seed(42)
u = np.random.uniform(0, 1, 10000)
x = -np.log(1 - u)

# Compare empirical CDF to theoretical
sorted_x = np.sort(x)
empirical_cdf = np.arange(1, len(x) + 1) / len(x)
theoretical_cdf = 1 - np.exp(-sorted_x)
```

### Expectation

The **expected value** (mean) of a random variable is the probability-weighted average of its values.

**Discrete:** $\displaystyle E[X] = \sum_{x \in \mathcal{X}} x \cdot p(x)$

**Continuous:** $\displaystyle E[X] = \int_{-\infty}^{\infty} x \cdot f(x) \, dx$

Intuition: The long-run average if you repeat the experiment infinitely many times.

**Properties (Linearity of expectation):**
- $E[aX + b] = aE[X] + b$
- $E[X + Y] = E[X] + E[Y]$ (no independence needed!)

Linearity of expectation is one of the most powerful tools in probability. It holds regardless of dependence between variables.

**Example — Expected value of a die roll:**
$$E[X] = 1 \cdot \frac{1}{6} + 2 \cdot \frac{1}{6} + \cdots + 6 \cdot \frac{1}{6} = 3.5.$$

**Expected value of a function:** $E[g(X)] = \sum_x g(x) p(x)$ for discrete, $\int g(x) f(x) dx$ for continuous.

### The Law of the Unconscious Statistician

The **Law of the Unconscious Statistician** (LOTUS) states that to compute $E[g(X)]$, you do not need to derive the distribution of $g(X)$ first. You can compute it directly using the distribution of $X$:

**Discrete:** $\displaystyle E[g(X)] = \sum_{x \in \mathcal{X}} g(x) \cdot p(x)$

**Continuous:** $\displaystyle E[g(X)] = \int_{-\infty}^{\infty} g(x) \cdot f(x) \, dx$

The name comes from the fact that it is easy to use this formula "unconsciously" without realizing you are computing an expectation of a transformed variable. LOTUS works because the expectation operator integrates over the probability space, and transforming the values is equivalent to integrating the transformed function against the original measure.

**Example — LOTUS for $X^2$ with a fair die:**

$$E[X^2] = \sum_{x=1}^{6} x^2 \cdot \frac{1}{6} = \frac{1 + 4 + 9 + 16 + 25 + 36}{6} = \frac{91}{6} \approx 15.167.$$

This is much simpler than deriving the PMF of $Y = X^2$ (which would require mapping $Y=4$ back to both $X=2$ and $X=-2$, not applicable here since the die only takes positive values).

**Example — LOTUS with a continuous distribution:**

For $X \sim \text{Uniform}(0, 1)$, find $E[X^2]$:

$$E[X^2] = \int_{0}^{1} x^2 \cdot 1 \, dx = \left[\frac{x^3}{3}\right]_{0}^{1} = \frac{1}{3}.$$

**Python verification:**

```python
import numpy as np
np.random.seed(42)
X = np.random.uniform(0, 1, 100000)
E_X2 = np.mean(X**2)
print(f"E[X^2] ≈ {E_X2:.4f} (theoretical: {1/3:.4f})")
# Output: E[X^2] ≈ 0.3337 (theoretical: 0.3333)
```

### Properties of Expectation

Beyond linearity, expectation satisfies several important properties useful in both theory and practice.

**Law of Total Expectation (Tower Property):**

$$E[X] = E[E[X | Y]].$$

This says the expectation of $X$ can be computed by first conditioning on another variable $Y$, computing the conditional expectation, and then averaging over $Y$. This is useful when direct computation of $E[X]$ is difficult but conditioning simplifies the problem.

**Example — Expected number of coin flips until heads:**

Let $X$ be the number of coin flips needed to get the first head, with $P(H) = p$. Condition on the first flip:

$$E[X] = E[X | \text{first is H}] \cdot p + E[X | \text{first is T}] \cdot (1-p)$$
$$E[X] = 1 \cdot p + (1 + E[X]) \cdot (1-p)$$

Solving: $E[X] = p + (1-p) + (1-p)E[X] = 1 + (1-p)E[X]$, so $E[X] = 1/p$.

For a fair coin ($p = 0.5$), the expected number of flips is 2.

**Jensen's Inequality:**

For a convex function $\varphi$:

$$\varphi(E[X]) \le E[\varphi(X)].$$

For a concave function $\varphi$:

$$\varphi(E[X]) \ge E[\varphi(X)].$$

This is fundamental in finance (risk-aversion), information theory ($-\log$ is convex, so $H(X) \le \log|\mathcal{X}|$), and machine learning (the EM algorithm uses Jensen to construct lower bounds).

**Example — Jensen's inequality in action:**

For $\varphi(x) = x^2$ (convex): $(E[X])^2 \le E[X^2]$. This means variance is always non-negative, since $\text{Var}(X) = E[X^2] - (E[X])^2 \ge 0$.

```python
import numpy as np

# Verify Jensen for a skewed distribution
np.random.seed(42)
X = np.random.exponential(scale=1.0, size=100000)
E_X = np.mean(X)
E_X2 = np.mean(X**2)
print(f"(E[X])^2 = {E_X**2:.4f}")
print(f"E[X^2]   = {E_X2:.4f}")
print(f"Jensen holds: {(E_X**2 <= E_X2)}")
```

**Cauchy-Schwarz Inequality:**

$$(E[XY])^2 \le E[X^2] E[Y^2].$$

This implies $|\text{Cov}(X, Y)| \le \sqrt{\text{Var}(X) \text{Var}(Y)}$, which bounds correlation between $-1$ and $1$.

**Expectation of indicator variables:**

If $I_A$ is the indicator of event $A$ ($I_A = 1$ if $A$ occurs, $0$ otherwise):

$$E[I_A] = P(A).$$

This connection between expectation and probability is surprisingly useful. Many probability problems can be solved by computing expectations of indicator variables.

**Example — Expected number of fixed points in a random permutation:**

Let $X$ be the number of positions $i$ such that $\pi(i) = i$ in a random permutation of $n$ elements. Define $I_i = 1$ if $\pi(i) = i$. Then $X = \sum I_i$, and by linearity:

$$E[X] = \sum_{i=1}^n E[I_i] = \sum_{i=1}^n P(\pi(i) = i) = \sum_{i=1}^n \frac{1}{n} = 1.$$

The expected number of fixed points is 1, regardless of $n$. This holds without computing the distribution of $X$ at all.

### Variance and Standard Deviation

The **variance** measures the spread of a distribution around its mean:

$$\text{Var}(X) = E[(X - E[X])^2] = E[X^2] - (E[X])^2.$$

The **standard deviation** is $\text{SD}(X) = \sqrt{\text{Var}(X)}$.

Properties:
- $\text{Var}(aX + b) = a^2 \text{Var}(X)$
- $\text{Var}(X + Y) = \text{Var}(X) + \text{Var}(Y) + 2\text{Cov}(X, Y)$
- If $X$ and $Y$ are independent: $\text{Var}(X + Y) = \text{Var}(X) + \text{Var}(Y)$

**Example — Variance of a die roll:**
$$E[X^2] = 1^2 \cdot \frac{1}{6} + 2^2 \cdot \frac{1}{6} + \cdots + 6^2 \cdot \frac{1}{6} = \frac{91}{6} \approx 15.167.$$
$$\text{Var}(X) = 15.167 - 3.5^2 = 15.167 - 12.25 = 2.917.$$
$$\text{SD}(X) = \sqrt{2.917} \approx 1.708.$$

**Coefficient of variation:** $\text{CV}(X) = \text{SD}(X) / E[X]$. Unitless measure of relative dispersion, useful for comparing variability across different scales.

```python
import numpy as np

# Simulate die rolls
np.random.seed(42)
rolls = np.random.randint(1, 7, size=100000)
empirical_var = np.var(rolls, ddof=0)  # population variance
empirical_std = np.std(rolls, ddof=0)
print(f"Empirical Var = {empirical_var:.3f} (theoretical: 2.917)")
print(f"Empirical SD  = {empirical_std:.3f} (theoretical: 1.708)")
```

### Moments and Moment Generating Functions

The $k$-th **raw moment** of $X$ is $\mu'_k = E[X^k]$.
The $k$-th **central moment** is $\mu_k = E[(X - E[X])^k]$.

- $\mu'_1 = E[X]$ (mean)
- $\mu_2 = \text{Var}(X)$ (variance)
- $\mu_3 / \sigma^3$ (skewness — asymmetry)
- $\mu_4 / \sigma^4$ (kurtosis — tail heaviness)

The **moment generating function** (MGF) is:

$$M_X(t) = E[e^{tX}].$$

When it exists, the MGF uniquely determines the distribution. Moments can be recovered by differentiation:

$$E[X^k] = M_X^{(k)}(0).$$

**Example — MGF of Bernoulli($p$):**
$$M_X(t) = E[e^{tX}] = p e^{t} + (1-p).$$
$$M'_X(0) = p = E[X].$$
$$M''_X(0) = p = E[X^2].$$

The MGF also simplifies finding distributions of sums of independent variables: if $X$ and $Y$ are independent, $M_{X+Y}(t) = M_X(t) M_Y(t)$.

### Transformations of Random Variables

Given a random variable $X$ and a function $g$, what is the distribution of $Y = g(X)$?

**Discrete case:** $P(Y = y) = \sum_{x: g(x) = y} P(X = x)$.

**Continuous case (monotonic $g$):** If $g$ is differentiable and strictly monotonic:

$$f_Y(y) = f_X(g^{-1}(y)) \left| \frac{d}{dy} g^{-1}(y) \right|.$$

**Example — Linear transformation:** If $X \sim \text{Normal}(0, 1)$ and $Y = \sigma X + \mu$, then $Y \sim \text{Normal}(\mu, \sigma^2)$.

**Example — Log-normal:** If $X \sim \text{Normal}(\mu, \sigma^2)$, then $Y = e^X$ follows a log-normal distribution. This is used in financial modeling (stock prices, options pricing) where values are positive and multiplicative.

```python
import numpy as np

# Generate log-normal returns
np.random.seed(42)
log_returns = np.random.normal(0.05, 0.20, 10000)  # 5% mean, 20% volatility
prices = 100 * np.exp(np.cumsum(log_returns))
print(f"Final price: ${prices[-1]:.2f}")
print(f"Mean price:   ${np.mean(prices):.2f}")
```

### Inequalities

Important probability inequalities provide bounds when exact distributions are unknown.

**Markov's inequality:** For a non-negative random variable $X$ and $a > 0$:

$$P(X \ge a) \le \frac{E[X]}{a}.$$

**Chebyshev's inequality:** For any $k > 0$:

$$P(|X - E[X]| \ge k\sigma) \le \frac{1}{k^2}.$$

This bounds the probability of being $k$ or more standard deviations from the mean. For $k=2$, $P(|X - \mu| \ge 2\sigma) \le 0.25$.

**Chernoff bound:** Tighter bound for sums of independent variables:

$$P(X \ge (1 + \delta)\mu) \le \left( \frac{e^\delta}{(1+\delta)^{1+\delta}} \right)^\mu.$$

Chernoff bounds are widely used in theoretical computer science and machine learning to bound generalization error.

```python
import numpy as np

# Chebyshev check on empirical data
np.random.seed(42)
data = np.random.exponential(scale=1.0, size=100000)
mu, sigma = np.mean(data), np.std(data)
k = 3
within_k = np.sum(np.abs(data - mu) < k * sigma) / len(data)
print(f"P(|X - mu| < {k}*sigma) = {within_k:.4f} (Chebyshev says >= {1 - 1/k**2:.4f})")
```

Output:
```
P(|X - mu| < 3*sigma) = 0.9541 (Chebyshev says >= 0.8889)
```

### Applications

**Risk analysis:** Value at Risk (VaR) is a quantile of the loss distribution. The 95% VaR is the 5th percentile of the profit/loss distribution.

**Software performance:** Response time measurements are random variables. The 99th percentile latency (instead of mean) is used for service level objectives because it captures tail behavior.

**Simulation:** Generating random variables from specified distributions enables Monte Carlo methods for pricing, optimization, and numerical integration.

**Feature engineering:** Many ML features are transformations of random variables — log transforms, scaling, binning. Understanding how transformations affect distributions is critical.

**Reinforcement learning:** The Bellman equation involves expectations over random state transitions and rewards.

**Online learning:** Algorithms like Thompson sampling treat unknown parameters as random variables with posterior distributions that update as data arrives.

**A/B testing:** The difference in conversion rates between two groups is a random variable. Understanding its distribution (via the central limit theorem) allows significance testing.

### Learning Tips

**Simulate everything:** Use Python or R to simulate random variables and verify theoretical expectations, variances, and inequalities. Seeing the empirical results converge to theory builds intuition.

**Practice with indicator variables:** Many expectation problems become trivial when decomposed into indicator variables. Practice rewriting events as sums of indicators.

**Memorize the big three properties:** Linearity of expectation, law of total expectation, and LOTUS handle 80% of expectation calculations. Know when to apply each one.

**Draw the CDF:** For any distribution, sketching the CDF helps understand quantiles, median, and probability intervals. Compare CDFs of different distributions on the same axes.

**Compute both $E[X]$ and $E[X^2]$:** Many problems require variance, which needs both. Computing $E[X^2]$ via LOTUS is often easier than deriving the full distribution of $X^2$.

**Watch for infinite expectation:** Some distributions (Cauchy, Pareto with low alpha) have undefined expectation. Always check that $E[|X|] < \infty$ before applying expectation properties.

## Glossary

| Term | Definition |
|------|------------|
| Random variable | A function mapping outcomes to real numbers |
| Discrete random variable | A random variable with countable support |
| Continuous random variable | A random variable with uncountable support (an interval) |
| Probability mass function (PMF) | Gives $P(X = x)$ for discrete $X$ |
| Probability density function (PDF) | Density whose integral gives probabilities for continuous $X$ |
| Cumulative distribution function (CDF) | $F(x) = P(X \le x)$, defined for all random variables |
| Expected value | Probability-weighted average of a random variable, also called mean |
| Variance | Expected squared deviation from the mean |
| Standard deviation | Square root of variance, in the same units as $X$ |
| Moment | $E[X^k]$ (raw) or $E[(X - \mu)^k]$ (central) |
| Moment generating function | $E[e^{tX}]$, uniquely determines the distribution |
| Law of Total Expectation | $E[X] = E[E[X \mid Y]]$ — conditioning simplifies expectation |
| Law of the Unconscious Statistician (LOTUS) | $E[g(X)]$ computed using $X$'s distribution directly |
| Jensen's inequality | $\varphi(E[X]) \le E[\varphi(X)]$ for convex $\varphi$ |
| Markov's inequality | $P(X \ge a) \le E[X]/a$ for non-negative $X$ |
| Chebyshev's inequality | $P(|X - \mu| \ge k\sigma) \le 1/k^2$ |
| Chernoff bound | Exponential bound for sums of independent variables |
| Coefficient of variation | $\text{SD}(X) / E[X]$, unitless measure of dispersion |
| Quantile function | Inverse of the CDF, $F^{-1}(p)$ |
| Support | The set of values a random variable can take |
| Linearity of expectation | $E[X + Y] = E[X] + E[Y]$, no independence required |
| Inverse transform sampling | Generating $X$ by $F^{-1}(U)$ where $U \sim \text{Uniform}(0,1)$ |
| Indicator variable | $I_A = 1$ if event $A$ occurs, $0$ otherwise; $E[I_A] = P(A)$ |
| Skewness | $\mu_3 / \sigma^3$, measures asymmetry of a distribution |
| Kurtosis | $\mu_4 / \sigma^4$, measures tail heaviness |
| Cauchy-Schwarz inequality | $(E[XY])^2 \le E[X^2] E[Y^2]$ |
| Covariance | $\text{Cov}(X, Y) = E[(X - E[X])(Y - E[Y])]$, measures linear dependence |
| Central limit theorem | Sum of i.i.d. random variables approaches normal as $n \to \infty$ |

## Quick References

- [Probability and Statistics for Computer Scientists, Baron](https://www.crcpress.com/Probability-and-Statistics-for-Computer-Scientists-Third-Edition/Baron/p/book/9781138044487) — accessible introduction with programming focus
- [Random Variables (Wikipedia)](https://en.wikipedia.org/wiki/Random_variable) — comprehensive reference
- [Seeing Theory: Random Variables](https://seeing-theory.brown.edu/random-variables/index.html) — interactive visualizations
- [Inverse Transform Sampling](https://en.wikipedia.org/wiki/Inverse_transform_sampling) — generating random samples from CDFs
- [LOTUS (Wikipedia)](https://en.wikipedia.org/wiki/Law_of_the_unconscious_statistician) — formal statement and proof of LOTUS
- [Jensen's Inequality Explained](https://brilliant.org/wiki/jensens-inequality/) — intuitive explanation with diagrams
- [Linearity of Expectation — MIT OCW](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-042j-mathematics-for-computer-science-fall-2010/video-lectures/lecture-19-linearity-of-expectation/) — video and notes
- [Probability Cheatsheet (Stanford)](https://stanford.edu/~shervine/teaching/cme-106/cheatsheet-probability) — quick reference for distributions and properties
- [Monte Carlo Methods — Stanford Encyclopedia](https://plato.stanford.edu/entries/monte-carlo/) — foundations of simulation

## Next Steps

- [Probability Distributions](probability-distributions.md) — specific named distributions and their properties
