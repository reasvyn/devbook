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

### Applications in Practice

**Financial risk management.** Value at Risk (VaR) and Expected Shortfall rely on the CLT for parametric estimates. A portfolio's daily returns are sums of many small factors. Under the CLT, the portfolio return distribution is approximately normal, allowing calculation of the 1% or 5% worst-case loss. For heavy-tailed assets, Monte Carlo simulation (LLN-based) provides more accurate estimates by drawing from fitted distributions.

**Quality assurance in manufacturing.** Acceptance sampling plans use the CLT to determine whether a batch meets quality standards. A sample of $n$ items is tested; the defect proportion $\hat{p}$ follows an approximately normal distribution. If $\hat{p} + z \times \sqrt{\hat{p}(1-\hat{p})/n}$ exceeds the acceptable quality limit, the batch is rejected. This statistical foundation underpins Six Sigma methodology.

**Monte Carlo simulation.** The LLN justifies estimating expectations via simulation. If we need to compute $E[f(X)]$ and cannot solve it analytically, we draw samples $X_1, \dots, X_n$ and compute $\frac{1}{n} \sum f(X_i)$. By the LLN, this converges to the true expectation.

```python
import numpy as np

def monte_carlo_pi(n):
    """Estimate pi using Monte Carlo method."""
    points = np.random.uniform(-1, 1, (n, 2))
    inside = np.sum(np.linalg.norm(points, axis=1) <= 1)
    return 4 * inside / n

for n in [100, 1000, 10000, 100000]:
    estimate = monte_carlo_pi(n)
    print(f"n={n:6d}: pi ≈ {estimate:.6f} (error: {abs(estimate - np.pi):.6f})")
```

**Polling and margin of error.** A poll of $n$ voters estimates the proportion supporting a candidate. The CLT tells us the sampling distribution of $\hat{p}$ is approximately normal with mean $p$ and variance $p(1-p)/n$. The margin of error $1.96 \times \sqrt{p(1-p)/n}$ is derived directly from the CLT. Pollsters report "margin of error $\pm 3\%$" based on $n \approx 1067$, which gives $1.96 \times \sqrt{0.5 \times 0.5 / 1067} \approx 0.03$.

**A/B testing.** In an A/B test comparing two conversion rates, the difference in sample proportions follows (by the CLT) an approximately normal distribution. The $z$-test and associated $p$-value rely on the CLT for validity. This is why A/B tests require a minimum sample size — below that, the normal approximation breaks down and false positives increase.

**Control charts in manufacturing.** Statistical process control uses control limits at $\mu \pm 3\sigma / \sqrt{n}$. Points outside these limits signal that the process has shifted. These limits come from the CLT: if the process is in control, the probability of exceeding 3-sigma limits is around 0.3%. Japanese manufacturing adopted these methods in the 1950s, leading to the quality revolution.

**Delta method for transformations.** The delta method extends the CLT to functions of the sample mean. If $\sqrt{n}(\bar{X}_n - \mu) \xrightarrow{d} N(0, \sigma^2)$ and $g$ is differentiable, then:

$$
\sqrt{n}(g(\bar{X}_n) - g(\mu)) \xrightarrow{d} N(0, \sigma^2 [g'(\mu)]^2)
$$

This is used for variance-stabilizing transformations like the log transform for ratios and the Fisher transform for correlations.

**Bootstrap approximation.** The bootstrap resamples from the empirical distribution to approximate sampling distributions. The theoretical justification comes from the LLN and CLT: the empirical distribution converges to the true distribution, so resampling from it approximates sampling from the population. The bootstrap percentile interval relies on the CLT for coverage guarantees.

```python
import numpy as np

def bootstrap_ci(data, stat_func=np.mean, n_bootstrap=10000, ci=0.95):
    """Compute bootstrap percentile confidence interval."""
    n = len(data)
    bootstrap_stats = np.array([
        stat_func(np.random.choice(data, n, replace=True))
        for _ in range(n_bootstrap)
    ])
    alpha = (1 - ci) / 2
    lower, upper = np.percentile(bootstrap_stats, [alpha * 100, (1 - alpha) * 100])
    return lower, upper

np.random.seed(42)
data = np.random.exponential(1, 50)
ci_low, ci_high = bootstrap_ci(data)
print(f"Sample mean: {np.mean(data):.4f}")
print(f"95% bootstrap CI: [{ci_low:.4f}, {ci_high:.4f}]")
```

**The 30-sample rule of thumb.** The guideline $n \geq 30$ comes from the observation that for moderately skewed distributions, the CLT approximation is reasonable by $n = 30$. This is not universal. For log-normal distributions with high variance, $n$ may need to be 300 or more. For symmetric light-tailed distributions like the uniform, $n = 5$ suffices. The required sample size depends on the population distribution's skewness and kurtosis.

**Confidence intervals and the CLT.** The CLT is the foundation for constructing confidence intervals for the population mean. The $z$-interval $\bar{X} \pm z_{\alpha/2} \times \sigma / \sqrt{n}$ relies on the CLT for its coverage guarantee. When $\sigma$ is unknown and estimated by $s$, the $t$-interval $\bar{X} \pm t_{\alpha/2, n-1} \times s / \sqrt{n}$ accounts for the additional uncertainty in the standard error estimate. The $t$-distribution is wider than the normal for small $n$, converging to the normal as $n$ grows. This convergence is itself a CLT result — the $t$-statistic converges in distribution to the standard normal as $n \to \infty$.

**The CLT for proportions.** For binary data, the sample proportion $\hat{p} = \frac{1}{n} \sum X_i$ where $X_i \in \{0, 1\}$ follows the CLT: $\hat{p} \xrightarrow{d} N(p, p(1-p)/n)$. This is the basis for confidence intervals for proportions, sample size calculations for polls, and hypothesis tests for A/B experiments. The normal approximation works well when $np \geq 10$ and $n(1-p) \geq 10$, a guideline derived from the symmetry and boundedness of the binomial distribution.

**Edgeworth expansions.** For improved accuracy beyond the basic CLT, Edgeworth expansions add correction terms involving skewness and kurtosis of the population distribution. These corrections improve the approximation quality for small samples from skewed distributions, reducing coverage error in confidence intervals from $O(1/\sqrt{n})$ to $O(1/n)$. The expansion includes a term proportional to the population skewness divided by $\sqrt{n}$, which shifts the quantiles to account for asymmetry in the sampling distribution.

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
