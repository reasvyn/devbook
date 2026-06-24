# Probability Distributions

## Description

A probability distribution describes all possible values of a random variable and how likely they are. Distributions are the vocabulary of statistics and machine learning — every model assumes one. Understanding the common distributions helps you choose the right model and interpret data correctly.

## Prerequisites

- [Basic Probability](basic-probability.md)

## Table of Contents

- [Discrete Distributions](#discrete-distributions)
- [Continuous Distributions](#continuous-distributions)
- [Central Limit Theorem](#central-limit-theorem)
- [Using Distributions in Code](#using-distributions-in-code)

## Content / Material

### Discrete Distributions

| Distribution | Use Case | Example |
|-------------|----------|---------|
| **Uniform** | All outcomes equally likely | Rolling a fair die |
| **Binomial** | Number of successes in n trials | 8 heads in 10 coin flips |
| **Poisson** | Count of rare events in a time interval | 3 emails per hour, website visits per minute |

```python
import numpy as np

# Binomial: 10 coin flips, P(heads) = 0.5
binomial = np.random.binomial(n=10, p=0.5, size=1000)

# Poisson: average 3 events per interval
poisson = np.random.poisson(lam=3, size=1000)
```

### Continuous Distributions

| Distribution | Use Case | Shape |
|-------------|----------|-------|
| **Normal** | Natural phenomena, errors, ML features | Bell curve |
| **Uniform** | Random number generation | Flat |
| **Exponential** | Waiting times, service times | Decaying |

The **normal distribution** is the most important:

$$ f(x) = \frac{1}{\sigma\sqrt{2\pi}} e^{-\frac{1}{2}\left(\frac{x-\mu}{\sigma}\right)^2} $$

```python
# Standard normal: mean=0, std=1
normal = np.random.normal(loc=0, scale=1, size=1000)
```

### Central Limit Theorem

The CLT states: averaging enough independent random variables (from any distribution with finite variance) produces a normal distribution.

```python
# Roll 6 dice, average them, repeat 10000 times — the averages are normal
means = [np.mean(np.random.randint(1, 7, 6)) for _ in range(10000)]
```

This is why the normal distribution appears everywhere — and why many ML models assume normality.

### Using Distributions in Code

```python
from scipy import stats

# PDF: P(X = 1.5) for normal(0, 1)
stats.norm.pdf(1.5, loc=0, scale=1)

# CDF: P(X < 1.5) — cumulative probability
stats.norm.cdf(1.5, loc=0, scale=1)

# Inverse CDF: what value gives P(X < x) = 0.95?
stats.norm.ppf(0.95, loc=0, scale=1)
```

## Glossary

| Term | Definition |
|------|------------|
| PDF | Probability Density Function — density at a point (continuous) |
| CDF | Cumulative Distribution Function — P(X ≤ x) |
| Normal distribution | A symmetric, bell-shaped continuous distribution |
| CLT | Central Limit Theorem — sums of independent variables tend toward normality |

## Next Steps

- [Descriptive Statistics](descriptive-statistics.md) — summarizing data with numbers
