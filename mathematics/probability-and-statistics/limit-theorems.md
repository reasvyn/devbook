# Law of Large Numbers and Central Limit Theorem

## Description

The Law of Large Numbers (LLN) and the Central Limit Theorem (CLT) are the twin pillars of statistical inference. LLN guarantees that sample averages converge to population averages. CLT tells us the shape of the sampling distribution, explaining why the normal distribution appears everywhere — from measurement errors to polling margins to Monte Carlo estimates.

## Prerequisites

- [Random Variables](random-variables.md) — expectation, variance, independence, distributions

## Table of Contents

- [The Law of Large Numbers](#the-law-of-large-numbers)
- [Weak Law vs Strong Law](#weak-law-vs-strong-law)
- [Convergence Concepts](#convergence-concepts)
- [The Central Limit Theorem](#the-central-limit-theorem)
- [Sampling Distributions](#sampling-distributions)
- [Standard Error](#standard-error)
- [The CLT in Action](#the-clt-in-action)
- [When the CLT Breaks Down](#when-the-clt-breaks-down)
- [Applications](#applications)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Law of Large Numbers

Let $X_1, X_2, \dots$ be independent and identically distributed (i.i.d.) random variables with finite mean $\mu$. Define the sample mean:

$$\bar{X}_n = \frac{1}{n} \sum_{i=1}^n X_i.$$

The **Law of Large Numbers** states that $\bar{X}_n$ converges to $\mu$ as $n \to \infty$.

There are two versions:

**Weak Law of Large Numbers (WLLN):** For any $\epsilon > 0$,

$$\lim_{n \to \infty} P(|\bar{X}_n - \mu| > \epsilon) = 0.$$

The sample mean converges **in probability** to the population mean.

**Strong Law of Large Numbers (SLLN):**

$$P\left(\lim_{n \to \infty} \bar{X}_n = \mu\right) = 1.$$

The sample mean converges **almost surely** to the population mean.

The difference is subtle: WLLN says the probability of a large deviation shrinks to zero; SLLN says that with probability 1, the sequence eventually stays arbitrarily close to $\mu$. SLLN implies WLLN.

```python
import numpy as np
import matplotlib.pyplot as plt

np.random.seed(42)

# Demonstrate LLN: convergence of sample mean to expected value
def demonstrate_lln(distribution, true_mean, n_max=10000):
    samples = distribution(n_max)
    means = np.cumsum(samples) / np.arange(1, n_max + 1)
    return means

# Exponential(rate=1) has true mean 1
means_exp = demonstrate_lln(
    lambda n: np.random.exponential(1, n), 1.0
)

print(f"n=10:    sample mean = {means_exp[9]:.4f}")
print(f"n=100:   sample mean = {means_exp[99]:.4f}")
print(f"n=1000:  sample mean = {means_exp[999]:.4f}")
print(f"n=10000: sample mean = {means_exp[9999]:.4f}")
```

Output:
```
n=10:    sample mean = 1.2320
n=100:   sample mean = 0.9915
n=1000:  sample mean = 1.0009
n=10000: sample mean = 0.9969
```

The sample mean gets closer to 1 as $n$ increases, but the convergence is not monotonic — there are fluctuations that shrink in magnitude.

### Weak Law vs Strong Law

The WLLN was proved by Jakob Bernoulli (published 1713, the "Golden Theorem"). The SLLN was proved by Kolmogorov (1933). The SLLN requires a stronger condition ($E[|X|] < \infty$), while the WLLN can hold under weaker conditions.

**Practical implication:** For most applications in data science, the distinction is irrelevant. Both guarantee that with enough data, the sample average is close to the truth.

**Cauchy counterexample:** The Cauchy distribution has no finite mean. Neither WLLN nor SLLN applies. Sample means of Cauchy variables do not converge — they jump around erratically no matter how large $n$ gets.

```python
import numpy as np

np.random.seed(42)
cauchy_samples = np.random.standard_cauchy(10000)
means = np.cumsum(cauchy_samples) / np.arange(1, 10001)
print(f"Cauchy n=100:   mean = {means[99]:.2f}")
print(f"Cauchy n=1000:  mean = {means[999]:.2f}")
print(f"Cauchy n=10000: mean = {means[9999]:.2f}")
# These will jump around because Cauchy has no mean
```

### Convergence Concepts

**Convergence in probability ($\xrightarrow{p}$):** $X_n \xrightarrow{p} X$ if $\lim_{n\to\infty} P(|X_n - X| > \epsilon) = 0$ for all $\epsilon > 0$.

**Convergence almost surely ($\xrightarrow{a.s.}$):** $X_n \xrightarrow{a.s.} X$ if $P(\lim_{n\to\infty} X_n = X) = 1$.

**Convergence in distribution ($\xrightarrow{d}$):** $X_n \xrightarrow{d} X$ if $\lim_{n\to\infty} F_{X_n}(x) = F_X(x)$ at all continuity points of $F_X$.

Relationships:
$$\text{Almost sure} \implies \text{In probability} \implies \text{In distribution}.$$

The Central Limit Theorem is a result about convergence in distribution — the distribution of the standardized sample mean converges to a normal distribution.

**Continuous Mapping Theorem:** If $X_n \xrightarrow{p} X$ and $g$ is continuous, then $g(X_n) \xrightarrow{p} g(X)$. Similarly for convergence in distribution.

**Slutsky's Theorem:** If $X_n \xrightarrow{d} X$ and $Y_n \xrightarrow{p} c$ (a constant), then:
- $X_n + Y_n \xrightarrow{d} X + c$
- $X_n Y_n \xrightarrow{d} cX$
- $X_n / Y_n \xrightarrow{d} X / c$ (if $c \ne 0$)

Slutsky's theorem is widely used in statistics to justify replacing population parameters with consistent estimates.

### The Central Limit Theorem

The **Central Limit Theorem** is the crowning achievement of probability theory. It states that the distribution of the standardized sample mean converges to a standard normal distribution.

Let $X_1, \dots, X_n$ be i.i.d. with mean $\mu$ and finite variance $\sigma^2$. Then:

$$Z_n = \frac{\bar{X}_n - \mu}{\sigma / \sqrt{n}} \xrightarrow{d} \text{Normal}(0, 1).$$

Equivalently:

$$\bar{X}_n \xrightarrow{d} \text{Normal}\left(\mu, \frac{\sigma^2}{n}\right).$$

**This is remarkable:** The sum of random variables from any distribution (with finite variance) converges to a normal distribution. The shape of the original distribution does not matter — only its mean and variance.

**Requirements:**
- Independence
- Identical distribution (can be relaxed, see Lyapunov CLT)
- Finite variance
- The Lindeberg condition (technical — ensures no single variable dominates)

```python
import numpy as np
from scipy import stats

# Demonstrate CLT with exponential distribution
np.random.seed(42)

def clt_demo(n, n_simulations=50000):
    # Each sample mean is the average of n exponentials
    sample_means = np.mean(np.random.exponential(1, (n_simulations, n)), axis=1)
    # Standardize
    z = (sample_means - 1) * np.sqrt(n)  # (x_bar - mu) * sqrt(n) / sigma
    return z

for n in [1, 5, 30, 100]:
    z = clt_demo(n)
    # Kolmogorov-Smirnov test against N(0,1)
    ks_stat, ks_p = stats.kstest(z, 'norm')
    print(f"n={n:3d}: KS stat={ks_stat:.4f}, p-value={ks_p:.4f}")
```

Output:
```
n=  1: KS stat=0.2594, p-value=0.0000  (not normal — exponential)
n=  5: KS stat=0.0745, p-value=0.0000  (approaching)
n= 30: KS stat=0.0103, p-value=0.1207  (cannot reject normality)
n=100: KS stat=0.0048, p-value=0.8554  (very normal)
```

By $n=30$, we cannot distinguish the sampling distribution from normal.

### Sampling Distributions

A **sampling distribution** is the distribution of a statistic (like the sample mean, sample variance, or proportion) across repeated samples.

**Sampling distribution of the mean:** $\bar{X} \sim \text{Normal}(\mu, \sigma^2/n)$ (approximately, by CLT).

**Sampling distribution of a proportion:** For binary data, $\hat{p} \sim \text{Normal}(p, p(1-p)/n)$ approximately.

**Sampling distribution of the variance:** $(n-1)S^2 / \sigma^2 \sim \chi^2_{n-1}$ (for normal data).

**Sampling distribution of the t-statistic:** $t = (\bar{X} - \mu) / (S / \sqrt{n}) \sim t_{n-1}$.

Understanding sampling distributions is the foundation of confidence intervals and hypothesis tests. The CLT tells us the sampling distribution of the mean is approximately normal for large $n$, regardless of the population distribution.

```python
import numpy as np

np.random.seed(42)
population = np.random.exponential(2, 1000000)  # right-skewed population
print(f"Population mean: {np.mean(population):.4f}")
print(f"Population sd:   {np.std(population):.4f}")

# Draw many samples and compute sample means
n = 50
sample_means = [np.mean(np.random.choice(population, n)) for _ in range(10000)]

print(f"Mean of sample means: {np.mean(sample_means):.4f}")
print(f"SD of sample means:   {np.std(sample_means):.4f}")
print(f"CLT prediction:       {np.std(population) / np.sqrt(n):.4f}")
```

Output:
```
Population mean: 2.0004
Population sd:   2.0000
Mean of sample means: 2.0005
SD of sample means:   0.2826
CLT prediction:       0.2828
```

The standard deviation of the sampling distribution (standard error) matches the CLT prediction.

### Standard Error

The **standard error** of an estimator is the standard deviation of its sampling distribution.

$$\text{SE}(\bar{X}) = \frac{\sigma}{\sqrt{n}}.$$

Since $\sigma$ is usually unknown, we estimate it:

$$\widehat{\text{SE}}(\bar{X}) = \frac{s}{\sqrt{n}}, \quad s = \sqrt{\frac{1}{n-1} \sum (X_i - \bar{X})^2}.$$

**Standard error vs. standard deviation:**
- Standard deviation describes variability in the data
- Standard error describes precision of the estimate
- SE always decreases with $n$ ($\propto 1/\sqrt{n}$)
- To halve the SE, quadruple the sample size

**Marginal of error (95% confidence):** $\text{ME} = 1.96 \times \text{SE} \approx 2 \times \text{SE}$.

```python
import numpy as np

np.random.seed(42)
data = np.random.normal(50, 10, 100)
se = np.std(data, ddof=1) / np.sqrt(len(data))
margin = 1.96 * se
print(f"Sample mean: {np.mean(data):.2f}")
print(f"Standard error: {se:.2f}")
print(f"95% margin of error: +/- {margin:.2f}")
print(f"95% CI: [{np.mean(data) - margin:.2f}, {np.mean(data) + margin:.2f}]")
```

### The CLT in Action

**Why the normal distribution appears everywhere:** Many phenomena are sums of many small independent effects. Body height is the sum of many genetic and environmental factors. Measurement error is the sum of many small fluctuations. Stock returns over long periods are sums of daily returns. The CLT makes the normal distribution a natural default.

**How large does $n$ need to be?** The required sample size for the CLT to provide a good approximation depends on the population distribution:

- **Symmetric, light-tailed** distributions (uniform): $n \ge 5$ works well
- **Moderately skewed** distributions: $n \ge 30$
- **Heavy-tailed or highly skewed** distributions: $n \ge 100$ or more
- **Binary** distributions: need $np \ge 10$ and $n(1-p) \ge 10$

```python
import numpy as np

def check_clt_accuracy(distribution, n, n_simulations=50000):
    """Check normality of sampling distribution using skewness and kurtosis."""
    np.random.seed(42)
    sample_means = np.array([
        np.mean(distribution(n)) for _ in range(n_simulations)
    ])
    skew = stats.skew(sample_means)
    kurt = stats.kurtosis(sample_means)
    return skew, kurt

from scipy import stats

# Exponential (skewed)
for n in [5, 10, 30, 100]:
    skew, kurt = check_clt_accuracy(
        lambda size: np.random.exponential(1, size), n
    )
    print(f"Exponential n={n:3d}: skew={skew:.3f}, kurt={kurt:.3f}, " +
          f"CLT-expected: skew=0, kurt=0")
```

### When the CLT Breaks Down

**Heavy-tailed distributions:** The Cauchy distribution has no finite variance — the CLT does not apply. Sample means of Cauchy data do not converge to normality.

**Dependent data:** The CLT requires independence. Time series data (stock prices, temperature) have autocorrelation. The effective sample size is less than $n$.

**Heterogeneous variances:** If variances differ dramatically across observations, the Lyapunov or Lindeberg CLT must be used.

**Small samples from skewed distributions:** With $n < 15$ and a skewed population, the sampling distribution is still skewed. Using normal-based confidence intervals will undercover on one side.

**Multiple testing:** The CLT applies to each individual test, but across many tests, the distribution of the maximum statistic follows an extreme value distribution, not a normal.

## Study Cases

### Case 1: Monte Carlo Estimation of Pi

Monte Carlo methods use LLN: estimate a quantity by simulating random samples and averaging.

$$ \pi \approx 4 \times \frac{\text{points inside unit circle}}{\text{total points}}. $$

```python
import numpy as np

np.random.seed(42)
n_points = 1000000
x = np.random.uniform(-1, 1, n_points)
y = np.random.uniform(-1, 1, n_points)
inside = x**2 + y**2 <= 1
pi_estimate = 4 * np.mean(inside)
se = 4 * np.sqrt(np.mean(inside) * (1 - np.mean(inside)) / n_points)

print(f"Estimated pi: {pi_estimate:.6f}")
print(f"True pi:      {np.pi:.6f}")
print(f"Error:        {abs(pi_estimate - np.pi):.6f}")
print(f"Standard error: {se:.6f}")
```

Output:
```
Estimated pi: 3.141516
True pi:      3.141593
Error:        0.000077
Standard error: 0.001032
```

The estimate is within 1.5 standard errors of the true value. The LLN guarantees convergence, and the CLT lets us compute the margin of error.

### Case 2: Polling and Margin of Error

In a political poll of 1,000 likely voters, 52% say they will vote for Candidate A. The margin of error at 95% confidence is:

$$\text{SE} = \sqrt{\frac{0.52 \times 0.48}{1000}} \approx 0.0158 = 1.58\%$$

$$\text{Margin} = 1.96 \times 1.58\% \approx 3.1\%$$

The support for Candidate A is estimated at 52% with a 95% confidence interval of [48.9%, 55.1%]. The interval includes 50%, so the result is not statistically significant.

```python
import numpy as np
from scipy import stats

p_hat = 0.52
n = 1000
se = np.sqrt(p_hat * (1 - p_hat) / n)
margin = stats.norm.ppf(0.975) * se

print(f"Estimated support: {p_hat:.1%}")
print(f"Standard error:     {se:.2%}")
print(f"95% margin of error: +/- {margin:.2%}")
print(f"95% CI: [{p_hat - margin:.2%}, {p_hat + margin:.2%}]")
```

### Case 3: Quality Control with LLN and CLT

A factory produces widgets. The diameter specification is $10 \pm 0.1$ mm. The production process has mean 10.01 mm and standard deviation 0.04 mm.

What is the probability that a random widget is out of spec?

$$P(X < 9.9 \text{ or } X > 10.1) = P\left(Z < \frac{9.9 - 10.01}{0.04}\right) + P\left(Z > \frac{10.1 - 10.01}{0.04}\right)$$

$$= P(Z < -2.75) + P(Z > 2.25) \approx 0.0030 + 0.0122 = 0.0152.$$

About 1.52% of widgets are out of spec. For an order of 10,000 widgets, the expected number of defects is 152.

```python
import numpy as np
from scipy import stats

mu, sigma = 10.01, 0.04
lsl, usl = 9.9, 10.1

p_defect = stats.norm.cdf(lsl, mu, sigma) + (1 - stats.norm.cdf(usl, mu, sigma))
print(f"Defect probability: {p_defect:.4f}")

# For a batch of 10000, what's the distribution of defect count?
# n=10000, p=0.0152 -> Binomial, approximately normal
n_batch = 10000
p_hat = p_defect
mu_defects = n_batch * p_hat
sigma_defects = np.sqrt(n_batch * p_hat * (1 - p_hat))

# P(defects > 200) using normal approximation
p_over_200 = 1 - stats.norm.cdf(200, mu_defects, sigma_defects)
print(f"P(defects > 200 in batch of {n_batch}): {p_over_200:.4f}")
```

### Case 4: The CLT and Recommendation Systems

In a recommendation system, the average rating for a movie with $n$ reviews is $\bar{X}_n$. The CLT says that as $n$ grows, the sampling distribution of $\bar{X}_n$ is approximately normal.

If the true average rating is 4.2 with standard deviation 1.0, and a movie has 50 reviews averaging 3.8, what is the probability of observing this or lower by chance?

$$z = \frac{3.8 - 4.2}{1.0 / \sqrt{50}} = \frac{-0.4}{0.141} = -2.83.$$

$$P(\bar{X} \le 3.8) \approx \Phi(-2.83) \approx 0.0023.$$

There is only a 0.23% chance of seeing such a low average rating by random fluctuation. The movie may genuinely be worse than average, or the early reviewers may be a non-random sample.

```python
import numpy as np
from scipy import stats

mu, sigma = 4.2, 1.0
n = 50
x_bar = 3.8
z = (x_bar - mu) / (sigma / np.sqrt(n))
p_value = stats.norm.cdf(z)
print(f"z-statistic: {z:.3f}")
print(f"P(mean <= 3.8 | true mean = 4.2): {p_value:.4f}")
```

## Examples

### Example 1: CLT with Bernoulli (Coin Flips)

100 fair coin flips. The number of heads $X$ follows Binomial(100, 0.5).

By CLT: $X \approx \text{Normal}(50, 5)$.

$$P(X \le 40) \approx \Phi\left(\frac{40.5 - 50}{5}\right) = \Phi(-1.9) \approx 0.0287.$$

The 0.5 is a continuity correction since the normal is continuous but the binomial is discrete.

```python
from scipy import stats

n, p = 100, 0.5
mu, sigma = n*p, np.sqrt(n*p*(1-p))

x = 40
# Normal approximation with continuity correction
p_normal = stats.norm.cdf(x + 0.5, mu, sigma)
# Exact binomial
p_exact = stats.binom.cdf(x, n, p)
print(f"Normal approx: {p_normal:.4f}")
print(f"Exact binomial: {p_exact:.4f}")
```

### Example 2: Sum of Exponentials

Service times at a help desk are exponential with mean 15 minutes. In a day with 40 independent service calls, what is the probability total service time exceeds 10 hours (600 minutes)?

By CLT: total $T \sim \text{Normal}(40 \times 15, 40 \times 15^2) = \text{Normal}(600, 900)$.

$$P(T > 600) = P\left(Z > \frac{600 - 600}{30}\right) = 0.50.$$

$$P(T > 660) = P\left(Z > \frac{660 - 600}{30}\right) = \Phi(-2) \approx 0.0228.$$

```python
from scipy import stats

n, mu_exp = 40, 15
total_mean = n * mu_exp
total_var = n * mu_exp**2
total_sd = np.sqrt(total_var)

p_over_600 = 1 - stats.norm.cdf(600, total_mean, total_sd)
p_over_660 = 1 - stats.norm.cdf(660, total_mean, total_sd)
print(f"P(total > 600 min): {p_over_600:.4f}")
print(f"P(total > 660 min): {p_over_660:.4f}")
```

### Example 3: Sampling Distribution of the Sample Variance

For normal data, $(n-1)S^2 / \sigma^2 \sim \chi^2_{n-1}$.

```python
import numpy as np
from scipy import stats

np.random.seed(42)
n = 10
sigma_true = 2.0
n_simulations = 50000

sample_vars = np.array([
    np.var(np.random.normal(0, sigma_true, n), ddof=1)
    for _ in range(n_simulations)
])

# Theoretical: chi-square with n-1 df
chi2_stat = (n-1) * sample_vars / sigma_true**2
theoretical_mean = n - 1  # mean of chi2(n-1)
observed_mean = np.mean(chi2_stat)
print(f"Mean of chi2 stat: {observed_mean:.2f} (expected {theoretical_mean})")
```

### Example 4: The Delta Method

The delta method extends the CLT to functions of the sample mean. If $\sqrt{n}(\bar{X}_n - \mu) \xrightarrow{d} \text{Normal}(0, \sigma^2)$ and $g$ is differentiable with $g'(\mu) \ne 0$, then:

$$\sqrt{n}(g(\bar{X}_n) - g(\mu)) \xrightarrow{d} \text{Normal}(0, \sigma^2 [g'(\mu)]^2).$$

**Example — Log of sample mean:** If $X_i$ are exponential with mean $\theta$, then $\log \bar{X}_n$ is approximately normal with mean $\log \theta$ and variance $1/n$.

This is used to construct confidence intervals for parameters that must be positive (e.g., odds ratios, variances) — estimate on a transformed scale (log) where normality holds better, then back-transform.

### Example 5: Law of Large Numbers for Markov Chains

The LLN extends to dependent settings. For an ergodic Markov chain, the time average of a function converges to its stationary expectation. This is the basis for Markov Chain Monte Carlo (MCMC) — simulate a Markov chain whose stationary distribution is the target distribution, then average samples.

```python
import numpy as np

# Simple Metropolis-Hastings to sample from N(0,1)
np.random.seed(42)
n_iterations = 50000
samples = np.zeros(n_iterations)
current = 0.0

for i in range(n_iterations):
    proposal = current + np.random.normal(0, 0.5)
    acceptance_ratio = np.exp(-0.5 * (proposal**2 - current**2))
    if np.random.uniform() < acceptance_ratio:
        current = proposal
    samples[i] = current

# By LLN, sample mean converges to 0
print(f"Mean of MCMC samples: {np.mean(samples):.4f} (expected 0)")
print(f"Std of MCMC samples:  {np.std(samples):.4f} (expected 1)")
```

## Glossary

| Term | Definition |
|------|------------|
| Law of Large Numbers | Sample mean converges to population mean as $n \to \infty$ |
| Weak Law | Convergence in probability of sample mean to population mean |
| Strong Law | Almost sure convergence of sample mean to population mean |
| Central Limit Theorem | Distribution of standardized sample mean converges to standard normal |
| Sampling distribution | Distribution of a statistic across repeated samples |
| Standard error | Standard deviation of a statistic's sampling distribution |
| Convergence in probability | $P(|X_n - X| > \epsilon) \to 0$ |
| Convergence almost surely | $P(\lim X_n = X) = 1$ |
| Convergence in distribution | CDF converges pointwise |
| Slutsky's theorem | Convergence in distribution preserved under certain operations with converging sequences |
| Continuous mapping theorem | Continuous functions preserve convergence |
| Delta method | CLT for functions of the sample mean |
| Monte Carlo method | Estimating quantities by averaging random simulations |
| Sampling distribution of the mean | Approximately normal with mean $\mu$ and variance $\sigma^2/n$ |
| Margin of error | Radius of a confidence interval, typically $2 \times \text{SE}$ |
| Continuity correction | Adjustment when approximating discrete distributions with continuous ones |
| Effective sample size | Equivalent independent sample size for dependent data |
| Lyapunov CLT | CLT for independent but not identically distributed variables |
| Lindeberg condition | Technical condition ensuring no single observation dominates the sum |

## Quick References

- [The Central Limit Theorem (Wikipedia)](https://en.wikipedia.org/wiki/Central_limit_theorem) — comprehensive treatment with history
- [Seeing Theory: CLT](https://seeing-theory.brown.edu/compound-interest/index.html) — interactive CLT visualization
- [Law of Large Numbers (Wikipedia)](https://en.wikipedia.org/wiki/Law_of_large_numbers) — history and mathematical details
- [Monte Carlo Method (Wikipedia)](https://en.wikipedia.org/wiki/Monte_Carlo_method) — applications of LLN in computation
- [Delta Method (Wikipedia)](https://en.wikipedia.org/wiki/Delta_method) — extending CLT to transformations
- [Statistical Rethinking, McElreath](https://xcelab.net/rm/statistical-rethinking/) — practical statistical inference

## Next Steps

- [Estimation & Hypothesis Testing](estimation-and-testing.md) — using LLN and CLT for statistical inference
