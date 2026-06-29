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
- [Variance and Standard Deviation](#variance-and-standard-deviation)
- [Moments and Moment Generating Functions](#moments-and-moment-generating-functions)
- [Transformations of Random Variables](#transformations-of-random-variables)
- [Inequalities](#inequalities)
- [Applications](#applications)
- [Study Cases](#study-cases)
- [Examples](#examples)
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

## Study Cases

### Case 1: Service Level Objective Compliance

A web API specifies that 99% of requests must complete within 200ms. Monitoring data shows response times follow an exponential distribution with mean 45ms under normal load.

What is the probability of meeting the SLO?

$$P(X \le 200) = 1 - e^{-200/45} = 1 - e^{-4.444} \approx 0.9883.$$

Under normal load, 98.83% of requests meet the SLO — slightly below the 99% target.

```python
import numpy as np

np.random.seed(42)
mean = 45
threshold = 200
n_days = 1000
n_requests_per_day = 10000

results = []
for _ in range(n_days):
    samples = np.random.exponential(mean, n_requests_per_day)
    slo_met = np.mean(samples <= threshold)
    results.append(slo_met)

print(f"Mean daily SLO: {np.mean(results):.4f}")
print(f"Days below 99%: {np.sum(np.array(results) < 0.99)} / {n_days}")
```

### Case 2: Portfolio Risk

A portfolio has two assets. Asset A has expected return 8% with 15% volatility. Asset B has expected return 12% with 25% volatility. The correlation between them is 0.3. The portfolio weights are 60% in A, 40% in B.

Compute the portfolio's expected return and variance:

$$E[R_p] = 0.6 \times 0.08 + 0.4 \times 0.12 = 0.096 \text{ (9.6%)}$$

$$\text{Var}(R_p) = 0.6^2 \times 0.15^2 + 0.4^2 \times 0.25^2 + 2 \times 0.6 \times 0.4 \times 0.3 \times 0.15 \times 0.25$$

$$= 0.0081 + 0.01 + 0.0054 = 0.0235$$

$$\text{SD}(R_p) = \sqrt{0.0235} \approx 0.1533 \text{ (15.33%)}$$

```python
import numpy as np

w = np.array([0.6, 0.4])
mu = np.array([0.08, 0.12])
sigma = np.array([0.15, 0.25])
corr = np.array([[1.0, 0.3], [0.3, 1.0]])
cov = np.outer(sigma, sigma) * corr

portfolio_mu = w @ mu
portfolio_var = w @ cov @ w
portfolio_sigma = np.sqrt(portfolio_var)

print(f"Portfolio expected return: {portfolio_mu:.4f}")
print(f"Portfolio volatility:      {portfolio_sigma:.4f}")
```

### Case 3: Insurance Claims Modeling

An insurance company models claim amounts as a random variable $X$ with PDF $f(x) = \lambda e^{-\lambda x}$ (exponential). The expected claim is $1/\lambda = \$5,000$. The company charges a premium of \$6,000 per policy. The probability of a loss exceeding the premium:

$$P(X > 6000) = e^{-6000/5000} = e^{-1.2} \approx 0.3012.$$

For 10,000 independent policies, the expected number of claims exceeding the premium is $10,000 \times 0.3012 = 3,012$. The aggregate loss distribution is the sum of 10,000 independent exponential variables — approximately normal by the CLT.

## Examples

### Example 1: Computing Expectation via Tail Sum

For a non-negative integer-valued random variable:

$$E[X] = \sum_{k=1}^\infty P(X \ge k).$$

**Proof:** $\sum_{k=1}^\infty P(X \ge k) = \sum_{k=1}^\infty \sum_{x=k}^\infty P(X = x) = \sum_{x=1}^\infty \sum_{k=1}^x P(X = x) = \sum_{x=1}^\infty x P(X = x) = E[X].$

This formula is often easier to compute than the direct definition.

### Example 2: Expectation of a Function is Not the Function of Expectation

$$E[g(X)] \ne g(E[X]) \text{ in general}.$$

For $g(x) = x^2$, by Jensen's inequality, $E[X^2] \ge (E[X])^2$ (with equality only if $X$ is constant). The difference is the variance.

For $g(x) = 1/x$, $E[1/X] \ne 1/E[X]$. This matters when estimating rates: the expected reciprocal of a time is not the reciprocal of the expected time.

### Example 3: Law of the Unconscious Statistician

For any function $g$, the expectation $E[g(X)]$ can be computed using the distribution of $X$ directly:

$$E[g(X)] = \sum_x g(x) p(x) \quad \text{or} \quad \int g(x) f(x) \, dx.$$

No need to first find the distribution of $g(X)$ — hence the name.

### Example 4: Standard Deviation and Scale

Consider measuring website load time in seconds vs. milliseconds:
- $X$: load time in seconds, $E[X] = 0.2$, $\text{SD}(X) = 0.05$
- $Y = 1000X$: load time in milliseconds, $E[Y] = 200$, $\text{SD}(Y) = 50$

The CV is the same: $0.05 / 0.2 = 50 / 200 = 0.25$. The CV allows meaningful comparison of dispersion across different measurement units.

### Example 5: Moment Generating Function of a Sum

If $X_1, \dots, X_n$ are independent Poisson($\lambda_i$), then $S_n = \sum_{i=1}^n X_i$ has MGF:

$$M_{S_n}(t) = \prod_{i=1}^n e^{\lambda_i (e^t - 1)} = e^{(\sum \lambda_i)(e^t - 1)}.$$

This is the MGF of Poisson($\sum \lambda_i$), proving the sum of independent Poisson variables is Poisson. This property extends to other distributions: sum of independent normals is normal, sum of independent gammas with the same rate is gamma.

## Glossary

| Term | Definition |
|------|------------|
| Random variable | A function mapping outcomes to real numbers |
| Discrete random variable | A random variable with countable support |
| Continuous random variable | A random variable with uncountable support (an interval) |
| Probability mass function (PMF) | Gives $P(X = x)$ for discrete $X$ |
| Probability density function (PDF) | Density whose integral gives probabilities for continuous $X$ |
| Cumulative distribution function (CDF) | $F(x) = P(X \le x)$, defined for all random variables |
| Expected value | Probability-weighted average of a random variable |
| Variance | Expected squared deviation from the mean |
| Standard deviation | Square root of variance, in the same units as $X$ |
| Moment | $E[X^k]$ (raw) or $E[(X - \mu)^k]$ (central) |
| Moment generating function | $E[e^{tX}]$, uniquely determines the distribution |
| Markov's inequality | $P(X \ge a) \le E[X]/a$ for non-negative $X$ |
| Chebyshev's inequality | $P(|X - \mu| \ge k\sigma) \le 1/k^2$ |
| Chernoff bound | Exponential bound for sums of independent variables |
| Coefficient of variation | $\text{SD}(X) / E[X]$, unitless measure of dispersion |
| Quantile function | Inverse of the CDF |
| Support | The set of values a random variable can take |
| Linearity of expectation | $E[X + Y] = E[X] + E[Y]$, no independence required |
| Inverse transform sampling | Generating $X$ by $F^{-1}(U)$ where $U \sim \text{Uniform}(0,1)$ |

## Quick References

- [Probability and Statistics for Computer Scientists, Baron](https://www.crcpress.com/Probability-and-Statistics-for-Computer-Scientists-Third-Edition/Baron/p/book/9781138044487) — accessible introduction with programming focus
- [Random Variables (Wikipedia)](https://en.wikipedia.org/wiki/Random_variable) — comprehensive reference
- [Seeing Theory: Random Variables](https://seeing-theory.brown.edu/random-variables/index.html) — interactive visualizations
- [Inverse Transform Sampling](https://en.wikipedia.org/wiki/Inverse_transform_sampling) — generating random samples from CDFs

## Next Steps

- [Probability Distributions](probability-distributions.md) — specific named distributions and their properties
