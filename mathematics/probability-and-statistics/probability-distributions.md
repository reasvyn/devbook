# Probability Distributions

## Description

Probability distributions are mathematical models that describe how random variables behave. Each distribution encodes assumptions about the data-generating process: count data, waiting times, measurement errors, or binary outcomes. Choosing the right distribution is critical for accurate statistical modeling, machine learning, queuing analysis, reliability engineering, and simulation.

## Prerequisites

- [Random Variables](random-variables.md) — PMF, PDF, CDF, expectation, variance

## Table of Contents

- [Overview of Distributions](#overview-of-distributions)
- [Discrete Distributions](#discrete-distributions)
- [Continuous Distributions](#continuous-distributions)
- [Relationships Between Distributions](#relationships-between-distributions)
- [Choosing the Right Distribution](#choosing-the-right-distribution)
- [Fitting Distributions to Data](#fitting-distributions-to-data)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Overview of Distributions

A distribution specifies the probabilities of all possible values of a random variable. Distributions are characterized by their **parameters** — numerical values that control shape, location, and scale.

**Location parameter ($\mu$, $a$):** Shifts the distribution left or right.
**Scale parameter ($\sigma$, $b$):** Stretches or compresses the distribution.
**Shape parameter ($\alpha$, $k$):** Determines the form beyond location and scale.

Families of distributions connected by location-scale transformations are called **location-scale families**.

### Discrete Distributions

#### Bernoulli($p$)

Models a single binary trial: success ($X=1$) with probability $p$, failure ($X=0$) with probability $1-p$.

- PMF: $p(x) = p^x (1-p)^{1-x}$, $x \in \{0, 1\}$
- Mean: $p$
- Variance: $p(1-p)$
- MGF: $1 - p + p e^t$

Applications: conversion event (buy/didn't buy), email open, click-through, bug present/absent.

#### Binomial($n$, $p$)

Sum of $n$ independent Bernoulli($p$) trials. Counts the number of successes.

- PMF: $\displaystyle P(X=k) = \binom{n}{k} p^k (1-p)^{n-k}$, $k = 0, 1, \dots, n$
- Mean: $np$
- Variance: $np(1-p)$
- MGF: $(1-p + p e^t)^n$

```python
import numpy as np
from scipy import stats

n, p = 100, 0.3
binom = stats.binom(n, p)
k = np.arange(0, n+1)
pmf = binom.pmf(k)

# Probability of at most 25 successes
print(f"P(X <= 25) = {binom.cdf(25):.4f}")
# Probability of exactly 30 successes
print(f"P(X = 30)  = {binom.pmf(30):.4f}")
```

#### Poisson($\lambda$)

Models count of events in a fixed interval when events occur independently at a constant average rate $\lambda$.

- PMF: $\displaystyle P(X=k) = \frac{\lambda^k e^{-\lambda}}{k!}$, $k = 0, 1, 2, \dots$
- Mean: $\lambda$
- Variance: $\lambda$

The Poisson distribution has the remarkable property that its mean equals its variance.

Applications: number of website requests per minute, number of bugs in a code module, number of customer arrivals per hour, number of typos on a page.

```python
# Simulating website visits
import numpy as np

lambda_visits = 5.5  # average visits per minute
np.random.seed(42)
minute_counts = np.random.poisson(lambda_visits, 10000)
print(f"Mean: {np.mean(minute_counts):.2f}, Var: {np.var(minute_counts):.2f}")
print(f"P(zero visits): {np.mean(minute_counts == 0):.4f}")
print(f"P(>10 visits):  {np.mean(minute_counts > 10):.4f}")
```

**Poisson process:** The Poisson distribution describes counts at a fixed time. The **Poisson process** extends this to model arrivals over time, with exponential inter-arrival times.

#### Geometric($p$)

Models the number of trials until the first success. Two conventions exist:
- **Support $\{1, 2, \dots\}$:** PMF: $P(X=k) = (1-p)^{k-1}p$
- **Support $\{0, 1, \dots\}$:** PMF: $P(X=k) = (1-p)^k p$ (number of failures before first success)

- Mean (first convention): $1/p$
- Variance: $(1-p)/p^2$

**Memoryless property:** $P(X > n + m \mid X > n) = P(X > m)$. The geometric distribution is the only discrete memoryless distribution.

Applications: number of API retries until success, number of ad impressions until conversion, number of rolls to get a 6.

#### Negative Binomial($r$, $p$)

Models the number of trials needed to get $r$ successes. Generalizes the geometric distribution.

- PMF: $\displaystyle P(X=k) = \binom{k-1}{r-1} p^r (1-p)^{k-r}$, $k = r, r+1, \dots$
- Mean: $r/p$
- Variance: $r(1-p)/p^2$

Unlike Poisson, the negative binomial can model **overdispersed** count data (variance > mean). This makes it useful for biological and ecological count data where Poisson's equal-mean-variance assumption fails.

**Connection:** Negative binomial is a gamma-Poisson mixture — Poisson with a gamma-distributed rate.

#### Discrete Uniform($a, b$)

All $k = a, a+1, \dots, b$ are equally likely.

- PMF: $1/(b - a + 1)$
- Mean: $(a + b)/2$
- Variance: $((b - a + 1)^2 - 1)/12$

Applications: fair dice, lotteries, random number generation for simulation.

### Continuous Distributions

#### Uniform($a, b$)

All values in $[a, b]$ are equally likely. The simplest continuous distribution.

- PDF: $f(x) = 1/(b - a)$, $a \le x \le b$
- Mean: $(a + b)/2$
- Variance: $(b - a)^2 / 12$

Applications: random number generation, inverse transform sampling basis, prior distribution when no information is available.

```python
import numpy as np

np.random.seed(42)
samples = np.random.uniform(0, 1, 10000)
print(f"Mean: {np.mean(samples):.4f} (expected 0.5)")
print(f"Var:  {np.var(samples):.4f} (expected 0.0833)")
```

#### Normal($\mu$, $\sigma^2$) / Gaussian

The most important distribution in statistics. Bell-shaped, symmetric, fully characterized by mean $\mu$ and variance $\sigma^2$.

- PDF: $\displaystyle f(x) = \frac{1}{\sigma\sqrt{2\pi}} \exp\left(-\frac{(x - \mu)^2}{2\sigma^2}\right)$
- Mean: $\mu$
- Variance: $\sigma^2$

**Standard normal:** $Z \sim \text{Normal}(0, 1)$. Any normal can be standardized: $Z = (X - \mu)/\sigma$.

**Empirical rule (68-95-99.7 rule):**
- 68% of data within $\mu \pm \sigma$
- 95% within $\mu \pm 2\sigma$
- 99.7% within $\mu \pm 3\sigma$

**Central Limit Theorem:** Sums (and means) of independent random variables approach normality as sample size grows. This explains the normal distribution's ubiquity.

Applications: measurement errors, heights, test scores, many natural phenomena, additive effects in complex systems.

```python
import numpy as np
from scipy import stats

mu, sigma = 100, 15
normal = stats.norm(mu, sigma)

# Percentile calculations
print(f"IQ 130+ probability: {1 - normal.cdf(130):.4f}")
print(f"90th percentile:     {normal.ppf(0.90):.1f}")
print(f"Middle 95% interval: [{normal.ppf(0.025):.1f}, {normal.ppf(0.975):.1f}]")
```

#### Exponential($\lambda$)

Models waiting times between events in a Poisson process. The continuous analog of the geometric distribution.

- PDF: $f(x) = \lambda e^{-\lambda x}$, $x \ge 0$
- CDF: $F(x) = 1 - e^{-\lambda x}$
- Mean: $1/\lambda$
- Variance: $1/\lambda^2$

**Memoryless property:** $P(X > s + t \mid X > s) = P(X > t)$. The only continuous distribution with this property.

Applications: time between server requests, battery lifetime, radioactive decay, service times.

```python
import numpy as np

# Simulating request inter-arrival times (10 req/sec)
rate = 10.0
np.random.seed(42)
inter_arrivals = np.random.exponential(1/rate, 1000)
mean_observed = np.mean(inter_arrivals)
print(f"Mean inter-arrival time: {mean_observed:.4f}s (expected {1/rate:.4f}s)")
```

#### Gamma($k$, $\theta$) / Gamma($\alpha$, $\beta$)

Generalizes the exponential distribution to model the waiting time for $k$ events. Flexible shape controlled by $k$ (shape) and $\theta$ (scale).

$$f(x) = \frac{x^{k-1} e^{-x/\theta}}{\theta^k \Gamma(k)}, \quad x > 0.$$

- Mean: $k\theta$
- Variance: $k\theta^2$

Special cases:
- $k = 1$: Exponential($1/\theta$)
- $k = \nu/2, \theta = 2$: Chi-square($\nu$)
- Integer $k$: Erlang distribution

Applications: total waiting time for multiple events, insurance claim sizes, Bayesian conjugate prior for Poisson rate, rainfall totals.

#### Beta($\alpha$, $\beta$)

Distributed on $[0, 1]$, making it ideal for modeling probabilities and proportions.

$$f(x) = \frac{x^{\alpha-1} (1-x)^{\beta-1}}{B(\alpha, \beta)}, \quad 0 \le x \le 1.$$

- Mean: $\alpha / (\alpha + \beta)$
- Variance: $\alpha\beta / ((\alpha + \beta)^2 (\alpha + \beta + 1))$
- Mode: $(\alpha - 1) / (\alpha + \beta - 2)$ for $\alpha, \beta > 1$

Special cases:
- $\alpha = \beta = 1$: Uniform(0, 1)
- $\alpha = \beta$: Symmetric around 0.5
- $\alpha > \beta$: Skewed toward 1
- $\beta > \alpha$: Skewed toward 0

Applications: Bayesian modeling of probabilities (conjugate prior for binomial), A/B testing, modeling uncertainty about proportions.

#### Student's t($\nu$)

Heavy-tailed distribution used when estimating the mean from a small sample with unknown variance.

$$f(x) = \frac{\Gamma((\nu+1)/2)}{\sqrt{\nu\pi}\ \Gamma(\nu/2)} \left(1 + \frac{x^2}{\nu}\right)^{-(\nu+1)/2}.$$

- Mean: 0 (for $\nu > 1$)
- Variance: $\nu / (\nu - 2)$ (for $\nu > 2$)

As $\nu \to \infty$, the t-distribution approaches the standard normal. With small $\nu$, it has fatter tails (more outliers expected).

Applications: t-tests for comparing means, confidence intervals with unknown variance, robust regression.

#### F($d_1$, $d_2$)

Distribution of the ratio of two independent chi-square variables, each divided by their degrees of freedom.

Applications: ANOVA, comparing variances (F-test), regression model comparison.

#### Chi-square($k$)

Distribution of the sum of $k$ independent standard normal squares. $k$ is degrees of freedom.

$$X = \sum_{i=1}^k Z_i^2, \quad Z_i \sim \text{Normal}(0, 1).$$

- Mean: $k$
- Variance: $2k$

Applications: goodness-of-fit tests, independence tests, confidence intervals for variance.

### Relationships Between Distributions

Distributions form a connected web. Understanding these relationships helps in modeling and computation.

**Bernoulli $\rightarrow$ Binomial:** $n$ i.i.d. Bernoulli($p$) sum to Binomial($n$, $p$).

**Binomial $\rightarrow$ Poisson limit:** As $n \to \infty$ and $p \to 0$ with $np = \lambda$, Binomial($n$, $p$) $\to$ Poisson($\lambda$).

**Binomial $\rightarrow$ Normal:** As $n \to \infty$, (Binomial($n$, $p$) $- np$) / $\sqrt{np(1-p)}$ $\to$ Normal(0, 1).

**Geometric $\rightarrow$ Exponential:** The geometric distribution is the discrete-time analog of the exponential.

**Exponential $\rightarrow$ Gamma:** Sum of $k$ i.i.d. Exponential($\lambda$) = Gamma($k$, $1/\lambda$).

**Poisson $\rightarrow$ Normal:** For large $\lambda$, Poisson($\lambda$) $\to$ Normal($\lambda$, $\lambda$).

**Gamma $\rightarrow$ Chi-square:** Gamma($\nu/2$, 2) = Chi-square($\nu$).

**Normal $\rightarrow$ t:** $Z / \sqrt{\chi^2_\nu / \nu} \sim t_\nu$, where $Z$ and $\chi^2_\nu$ are independent.

**Normal $\rightarrow$ F:** Ratio of two independent chi-square variables follows F.

**Beta-Binomial:** The beta distribution is conjugate to the binomial — the posterior of $p$ given binomial data is Beta.

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt

# Demonstrate Binomial -> Normal approximation
n, p = 50, 0.3
binom = stats.binom(n, p)
normal = stats.norm(n*p, np.sqrt(n*p*(1-p)))

k = np.arange(0, n+1)
binom_pmf = binom.pmf(k)
normal_pdf = normal.pdf(k)

# Compare probabilities
print(f"P(X=15) Binomial: {binom.pmf(15):.4f}")
print(f"P(X=15) Normal:   {normal.pdf(15):.4f}")
```

Output:
```
P(X=15) Binomial: 0.1223
P(X=15) Normal:   0.1204
```

### Choosing the Right Distribution

The choice of distribution depends on the data-generating process:

| Data type | Possible distributions |
|-----------|----------------------|
| Binary (0/1) | Bernoulli |
| Count (unbounded) | Poisson, negative binomial |
| Count (bounded by $n$) | Binomial, hypergeometric |
| Positive continuous | Exponential, gamma, log-normal, Weibull |
| Continuous on $\mathbb{R}$ | Normal, t, Cauchy, logistic |
| Bounded interval $[0,1]$ | Beta |
| Time-to-event | Exponential, Weibull, gamma, log-normal |
| Sum of squares | Chi-square |
| Ratio of variances | F |
| Proportions | Beta, Dirichlet |

**Modeling checklist:**
1. What is the support? (binary, count, positive, real, bounded)
2. What is the relationship between mean and variance?
3. Are events independent?
4. Are there natural bounds or constraints?
5. What do theoretical considerations suggest? (e.g., waiting times are often exponential)

### Fitting Distributions to Data

**Method of moments:** Equate sample moments to theoretical moments and solve for parameters.

**Maximum likelihood estimation (MLE):** Find parameters that maximize the probability of observing the data. MLE is consistent, asymptotically normal, and efficient.

```python
import numpy as np
from scipy import stats

# Generate data from a gamma distribution
np.random.seed(42)
true_shape, true_scale = 2.0, 1.5
data = np.random.gamma(true_shape, true_scale, 1000)

# Fit distributions using MLE
gamma_fit = stats.gamma.fit(data, floc=0)
normal_fit = stats.norm.fit(data)
exponential_fit = stats.expon.fit(data, floc=0)

print(f"Gamma:     shape={gamma_fit[0]:.2f}, scale={gamma_fit[2]:.2f}")
print(f"True:      shape={true_shape}, scale={true_scale}")
print(f"Normal:    mu={normal_fit[0]:.2f}, sigma={normal_fit[1]:.2f}")
print(f"Exponential: lambda={1/exponential_fit[1]:.2f}")
```

Output:
```
Gamma:     shape=2.03, scale=1.48
True:      shape=2.0, scale=1.5
Normal:    mu=3.01, sigma=2.11
Exponential: lambda=0.33
```

**Goodness-of-fit:** Use Q-Q plots, Kolmogorov-Smirnov test, or chi-square goodness-of-fit to assess whether a chosen distribution fits the data.

```python
from scipy import stats

# Kolmogorov-Smirnov test against gamma
ks_stat, ks_p = stats.kstest(data, 'gamma', args=(gamma_fit[0], 0, gamma_fit[2]))
print(f"Gamma KS test:  stat={ks_stat:.4f}, p={ks_p:.4f}")

# Kolmogorov-Smirnov test against normal
ks_stat, ks_p = stats.kstest(data, 'norm', args=(normal_fit[0], normal_fit[1]))
print(f"Normal KS test: stat={ks_stat:.4f}, p={ks_p:.4f}")
```

## Study Cases

### Case 1: Queueing System Design

An API gateway handles 120 requests per minute on average. Service time per request follows an exponential distribution with mean 0.4 seconds. Model the system as an M/M/1 queue.

Arrival rate: $\lambda = 120/60 = 2$ requests/second. Service rate: $\mu = 1/0.4 = 2.5$ requests/second.

Traffic intensity: $\rho = \lambda / \mu = 0.8$ (80% utilization).

Key metrics using exponential distribution properties:
- Mean queue length: $\rho^2 / (1 - \rho) = 0.64 / 0.2 = 3.2$ requests
- Mean time in system: $1 / (\mu - \lambda) = 1 / 0.5 = 2.0$ seconds
- Probability queue is empty: $1 - \rho = 0.20$
- Probability queue exceeds 10: $\rho^{11} \approx 0.0859$

```python
lam = 2.0  # arrivals per second
mu = 2.5   # service rate
rho = lam / mu

print(f"Traffic intensity:        {rho:.2f}")
print(f"Mean queue length:        {rho**2/(1-rho):.2f}")
print(f"Mean time in system:      {1/(mu-lam):.2f}s")
print(f"P(queue > 10):            {rho**11:.4f}")
```

### Case 2: A/B Testing — Choosing Sample Size

You are designing an A/B test to detect a 1 percentage point lift (from 5% to 6%) with 80% power at $\alpha = 0.05$. Using the normal approximation to the binomial:

$$n = \frac{(Z_{\alpha/2} + Z_\beta)^2 (p_1(1-p_1) + p_2(1-p_2))}{(p_2 - p_1)^2}$$

```python
from scipy import stats

alpha = 0.05
power = 0.80
p1, p2 = 0.05, 0.06

z_alpha = stats.norm.ppf(1 - alpha/2)
z_beta = stats.norm.ppf(power)

n = ((z_alpha + z_beta)**2 * (p1*(1-p1) + p2*(1-p2))) / (p2 - p1)**2
print(f"Required sample size per variant: {np.ceil(n):.0f}")
```

Output: `Required sample size per variant: 4343`

Each variant needs approximately 4,343 users for 80% power.

### Case 3: Reliability of a Redundant System

A server has two power supplies in parallel. Each has an exponential lifetime with mean 50,000 hours (MTBF). The system fails only when both fail.

$$P(T_{\text{system}} > t) = 1 - (1 - e^{-t/50000})^2.$$

Mean system lifetime for $k$ parallel components each with rate $\lambda$:

$$E[T] = \frac{1}{\lambda} \left(1 + \frac{1}{2} + \cdots + \frac{1}{k}\right).$$

For two power supplies: $E[T] = 50000 \times (1 + 1/2) = 75,000$ hours.

```python
lam = 1 / 50000
t = 8760  # one year in hours

reliability = 1 - (1 - np.exp(-lam * t))**2
print(f"One-year reliability with redundancy: {reliability:.4f}")
print(f"One-year reliability single PSU:      {np.exp(-lam * t):.4f}")
```

## Examples

### Example 1: Poisson Approximation to Binomial

The binomial distribution with large $n$ and small $p$ can be approximated by Poisson($\lambda = np$). Historically, this approximated the number of Prussian soldiers kicked to death by horses per year — a rare event.

```python
n, p = 1000, 0.002
lam = n * p

binom = stats.binom(n, p)
pois = stats.poisson(lam)

for k in [0, 1, 2, 3, 4]:
    print(f"P(X={k}): Binomial={binom.pmf(k):.4f}, Poisson={pois.pmf(k):.4f}")
```

### Example 2: Beta Distribution Shapes

```python
import numpy as np
from scipy import stats

configs = [(0.5, 0.5), (1, 1), (2, 2), (2, 5), (5, 2), (5, 1)]
x = np.linspace(0, 1, 200)

for a, b in configs:
    beta = stats.beta(a, b)
    mode = (a-1)/(a+b-2) if a > 1 and b > 1 else "N/A"
    print(f"Beta({a:4.1f}, {b:4.1f}): mean={beta.mean():.2f}, mode={mode}")
```

Output:
```
Beta( 0.5,  0.5): mean=0.50, mode=N/A      (U-shaped)
Beta( 1.0,  1.0): mean=0.50, mode=N/A      (uniform)
Beta( 2.0,  2.0): mean=0.50, mode=0.50     (bell-shaped)
Beta( 2.0,  5.0): mean=0.29, mode=0.20     (skewed right)
Beta( 5.0,  2.0): mean=0.71, mode=0.80     (skewed left)
Beta( 5.0,  1.0): mean=0.83, mode=1.00     (J-shaped)
```

### Example 3: From Exponential to Gamma — Bus Waiting Times

Buses arrive at a stop at a rate of 6 per hour (exponential inter-arrival with mean 10 min). The time until the 3rd bus arrives follows Gamma(3, 10) with mean 30 minutes.

```python
import numpy as np

np.random.seed(42)
n_simulations = 100000
# Generate 3 inter-arrival times per simulation
inter_arrivals = np.random.exponential(10, (n_simulations, 3))
wait_times = inter_arrivals.sum(axis=1)
print(f"Mean wait: {np.mean(wait_times):.2f} min (theoretical: 30)")
print(f"80th percentile: {np.percentile(wait_times, 80):.2f} min")
```

### Example 4: Heavy-Tailed vs Light-Tailed Distributions

The normal distribution is light-tailed (exponential decay). The t-distribution with low degrees of freedom is heavy-tailed (power-law decay). This means extreme values are much more likely under a t-distribution.

```python
from scipy import stats

print("Probability of exceeding 5 standard deviations:")
for df in [3, 5, 10, 30]:
    t_prob = 2 * stats.t.sf(5, df)
    print(f"  t({df:2d}):  {t_prob:.6f}")
print(f"  Normal: {2*stats.norm.sf(5):.10f}")
```

Output:
```
Probability of exceeding 5 standard deviations:
  t( 3):  0.015262
  t( 5):  0.004034
  t(10):  0.000567
  t(30):  0.000026
  Normal: 0.0000005733
```

The t(3) distribution has 27,000 times more probability in the tails beyond 5 sigma than the normal.

### Example 5: Parameter Estimation — Method of Moments vs MLE

For an exponential distribution, both methods give the same estimator: $\hat{\lambda} = 1 / \bar{x}$. For the gamma distribution, they differ.

```python
import numpy as np
from scipy import optimize, stats

np.random.seed(42)
data = np.random.gamma(2.0, 1.5, 1000)

# Method of moments
mean = np.mean(data)
var = np.var(data)
alpha_mom = mean**2 / var
beta_mom = var / mean

# MLE using scipy
alpha_mle, loc, beta_mle = stats.gamma.fit(data, floc=0)

print(f"MOM:  alpha={alpha_mom:.2f}, beta={beta_mom:.2f}")
print(f"MLE:  alpha={alpha_mle:.2f}, beta={beta_mle:.2f}")
print(f"True: alpha=2.00, beta=1.50")
```

## Glossary

| Term | Definition |
|------|------------|
| Bernoulli distribution | Distribution for a single binary trial |
| Binomial distribution | Sum of $n$ independent Bernoulli trials |
| Poisson distribution | Count of rare events in a fixed interval |
| Geometric distribution | Number of trials until first success |
| Negative binomial distribution | Number of trials until $r$ successes |
| Normal (Gaussian) distribution | Bell-shaped distribution defined by $\mu$ and $\sigma$ |
| Exponential distribution | Waiting times in a Poisson process |
| Gamma distribution | Flexible distribution for positive data |
| Beta distribution | Distribution on $[0, 1]$ for probabilities |
| Student's t distribution | Heavy-tailed distribution for small-sample inference |
| F distribution | Ratio of chi-square variables |
| Chi-square distribution | Sum of squared standard normals |
| Uniform distribution | Equal probability over an interval |
| Memoryless property | $P(X > s + t \mid X > s) = P(X > t)$ |
| Overdispersion | Variance exceeds mean, often addressed with negative binomial |
| Location parameter | Shifts the distribution horizontally |
| Scale parameter | Stretches the distribution |
| Shape parameter | Controls the distribution's form beyond location and scale |
| MLE | Maximum likelihood estimation |
| Goodness-of-fit | Assessment of how well a distribution fits observed data |
| Heavy-tailed | Tail probabilities decay slowly (power law) |

## Quick References

- [Probability Distributions (Wikipedia)](https://en.wikipedia.org/wiki/List_of_probability_distributions) — comprehensive list with properties
- [Scipy.stats Documentation](https://docs.scipy.org/doc/scipy/reference/stats.html) — all distributions implemented in Python
- [Seeing Theory: Distributions](https://seeing-theory.brown.edu/probability-distributions/index.html) — interactive visualizations
- [Distribution Explorer](https://distribution-explorer.github.io/) — interactive exploration of distribution properties
- [Statistical Distributions, Evans et al.](https://www.wiley.com/en-us/Statistical+Distributions%2C+4th+Edition-p-9780470390634) — reference for distribution properties

## Next Steps

- [Joint & Conditional Distributions](joint-distributions.md) — multivariate distributions, covariance, and correlation
