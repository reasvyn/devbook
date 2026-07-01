# Bayesian Statistics

## Description

Bayesian statistics is a framework for reasoning under uncertainty that treats parameters as random variables and updates beliefs as data arrives. It provides a principled way to incorporate prior knowledge, quantify uncertainty with credible intervals, and make sequential decisions. Bayesian methods power A/B testing, spam filters, recommendation systems, and many machine learning algorithms.

## Prerequisites

- [Probability Foundations](probability-foundations.md) — Bayes theorem, conditional probability, law of total probability
- [Estimation & Hypothesis Testing](estimation-and-testing.md) — frequentist inference, confidence intervals, MLE

## Table of Contents

- [Bayesian vs Frequentist Philosophy](#bayesian-vs-frequentist-philosophy)
- [The Bayesian Framework](#the-bayesian-framework)
- [Prior Distributions](#prior-distributions)
- [Likelihood](#likelihood)
- [Posterior Distribution](#posterior-distribution)
- [Conjugate Priors](#conjugate-priors)
- [Bayesian Updating](#bayesian-updating)
- [Credible Intervals](#credible-intervals)
- [Bayes Factors](#bayes-factors)
- [Markov Chain Monte Carlo](#markov-chain-monte-carlo)
- [Bayesian A/B Testing](#bayesian-ab-testing)
- [Bayesian vs Frequentist: Practical Guide](#bayesian-vs-frequentist-practical-guide)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Bayesian vs Frequentist Philosophy

The fundamental difference: what is a probability?

**Frequentist:** Probability is the long-run frequency of events. Parameters are fixed (unknown) constants. Probability statements apply only to data (which are random), not parameters.

**Bayesian:** Probability represents a degree of belief. Parameters are random variables. Probability statements can apply to both data and parameters.

| Aspect | Frequentist | Bayesian |
|--------|-------------|----------|
| Parameter | Fixed unknown constant | Random variable with distribution |
| Probability | Long-run frequency | Degree of belief |
| Inference | Point estimate + confidence interval | Posterior distribution |
| Prior knowledge | Not used formally | Incorporated via prior |
| "95% interval" | 95% of intervals contain the parameter | 95% probability parameter is in the interval |
| Key tool | MLE, p-values, confidence intervals | MCMC, posterior samples, credible intervals |

**p-value vs posterior probability:** A frequentist p-value is $P(\text{data} \mid H_0)$. A Bayesian posterior probability is $P(H_0 \mid \text{data})$ — the quantity scientists actually want.

**Analogy:** Frequentist: "If this coin is fair, what is the chance of seeing 9 heads in 10 flips?" Bayesian: "Given 9 heads in 10 flips, what is the chance this coin is fair?"

### The Bayesian Framework

Bayes' theorem for inference:

$$P(\theta \mid \text{data}) = \frac{P(\text{data} \mid \theta) P(\theta)}{P(\text{data})}.$$

In words:

$$\text{Posterior} = \frac{\text{Likelihood} \times \text{Prior}}{\text{Evidence}}.$$

**Prior $P(\theta)$:** Our initial belief about the parameter before seeing data.

**Likelihood $P(\text{data} \mid \theta)$:** The probability of the data given the parameter. Same as in frequentist statistics.

**Evidence $P(\text{data})$:** The total probability of the data under all possible parameter values:

$$P(\text{data}) = \int P(\text{data} \mid \theta) P(\theta) \, d\theta.$$

This is a normalizing constant ensuring the posterior integrates to 1.

**Posterior $P(\theta \mid \text{data})$:** Our updated belief after seeing data.

### Prior Distributions

**Informative prior:** Based on strong previous evidence. Example: using previous year's conversion rate as a prior for this year's A/B test.

**Weakly informative prior:** Contains some information but lets the data dominate. Example: Beta(1, 1) for a probability is uniform; Beta(2, 2) is weakly informative (slightly favors 0.5).

**Non-informative (flat) prior:** Represents ignorance. Example: $p \sim \text{Uniform}(0, 1)$ for a proportion. Different parameterizations of "ignorance" can lead to different priors (Jeffreys prior addresses this).

**Improper prior:** Does not integrate to 1. Example: $p(\mu) \propto 1$ for a normal mean. Often leads to proper posteriors.

**Conjugate prior:** A prior that, when combined with the likelihood, yields a posterior in the same family. Convenient for computation.

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt

theta = np.linspace(0, 1, 200)

# Different priors for a probability
priors = {
    'Uniform Beta(1,1)': stats.beta(1, 1),
    'Weakly informative Beta(2,2)': stats.beta(2, 2),
    'Informative Beta(50, 50)': stats.beta(50, 50),
    'Skeptical Beta(1, 10)': stats.beta(1, 10),
}

for name, prior in priors.items():
    print(f"{name:30s}: mean={prior.mean():.3f}, mode={prior.stats('m'):.3f}")
```

**Prior selection guidelines:**
- Use domain knowledge when available
- Start with weakly informative priors for main analysis
- Conduct sensitivity analysis: do conclusions change with different priors?
- For parameters with natural bounds (probabilities, variances), use appropriate priors

### Likelihood

The likelihood function is the same as in frequentist statistics:

$$L(\theta \mid \text{data}) = \prod_{i=1}^n f(x_i \mid \theta).$$

**Common likelihoods:**
- Binary data: Bernoulli($p$), Binomial($n$, $p$)
- Count data: Poisson($\lambda$)
- Continuous data: Normal($\mu$, $\sigma^2$), Exponential($\lambda$)

The likelihood captures the information in the data about the parameter.

### Posterior Distribution

The posterior is the combination of prior and data. It is the complete description of uncertainty about the parameter after seeing data.

**Posterior summaries:**
- **Posterior mean:** $E[\theta \mid \text{data}]$ — the Bayes estimator under squared error loss
- **Posterior median:** The 50th percentile — robust summary
- **Posterior mode:** The maximum a posteriori (MAP) estimate
- **Posterior variance:** Quantifies remaining uncertainty

```python
import numpy as np
from scipy import stats

# Beta-Binomial conjugate example
# Prior: Beta(2, 2), Data: 8 successes out of 20 trials
alpha_prior, beta_prior = 2, 2
successes, trials = 8, 20

alpha_post = alpha_prior + successes
beta_post = beta_prior + trials - successes

posterior = stats.beta(alpha_post, beta_post)

print(f"Prior mean: {alpha_prior/(alpha_prior+beta_prior):.3f}")
print(f"Posterior mean: {posterior.mean():.3f}")
print(f"Posterior mode: {(alpha_post-1)/(alpha_post+beta_post-2):.3f}")
print(f"Posterior sd:   {posterior.std():.3f}")
print(f"95% credible interval: [{posterior.ppf(0.025):.3f}, {posterior.ppf(0.975):.3f}]")
```

### Conjugate Priors

A conjugate prior leads to a posterior in the same distribution family as the prior. This avoids numerical integration and enables analytical updates.

| Likelihood | Conjugate Prior | Posterior |
|-----------|----------------|-----------|
| Bernoulli($p$), Binomial($n$, $p$) | Beta($\alpha$, $\beta$) | Beta($\alpha + s$, $\beta + n - s$) |
| Poisson($\lambda$) | Gamma($a$, $b$) | Gamma($a + s$, $b + n$) |
| Normal($\mu$, known $\sigma^2$) | Normal($\mu_0$, $\sigma_0^2$) | Normal($\frac{\mu_0/\sigma_0^2 + n\bar{x}/\sigma^2}{1/\sigma_0^2 + n/\sigma^2}$, $\frac{1}{1/\sigma_0^2 + n/\sigma^2}$) |
| Normal($\mu$, unknown $\sigma^2$) | Inverse-Gamma($a$, $b$) | Inverse-Gamma($a + n/2$, $b + \sum(x_i - \mu)^2/2$) |
| Exponential($\lambda$) | Gamma($a$, $b$) | Gamma($a + n$, $b + \sum x_i$) |

**Posterior mean as weighted average:**

For the normal-normal conjugate case:

$$\mu_{\text{post}} = \frac{\frac{\mu_0}{\sigma_0^2} + \frac{n\bar{x}}{\sigma^2}}{\frac{1}{\sigma_0^2} + \frac{n}{\sigma^2}}.$$

The posterior mean is a precision-weighted average of the prior mean and the sample mean. As $n$ grows, the data dominates. As $\sigma_0^2 \to \infty$ (flat prior), the posterior mean converges to $\bar{x}$.

```python
import numpy as np
from scipy import stats

# Normal-Normal conjugate example
mu_prior, sigma_prior = 100, 20  # prior: mean IQ is 100
mu_data, sigma_data = 110, 15   # observed sample mean and known sigma
n = 25

# Posterior
post_precision = 1/sigma_prior**2 + n/sigma_data**2
mu_post = (mu_prior/sigma_prior**2 + n*mu_data/sigma_data**2) / post_precision
sigma_post = np.sqrt(1 / post_precision)

print(f"Prior:    N({mu_prior}, {sigma_prior}^2)")
print(f"Data:     mean={mu_data}, n={n}")
print(f"Posterior: N({mu_post:.2f}, {sigma_post:.2f}^2)")

# Compare: sample mean alone
print(f"Sample mean SE: {sigma_data/np.sqrt(n):.2f}")
print(f"Posterior SD:   {sigma_post:.2f}")
```

### Bayesian Updating

Bayesian inference is naturally sequential. Today's posterior becomes tomorrow's prior.

**Sequential updating:** If two experiments produce data $D_1$ and $D_2$:

$$P(\theta \mid D_1, D_2) \propto P(D_2 \mid \theta) P(\theta \mid D_1) \propto P(D_2 \mid \theta) P(D_1 \mid \theta) P(\theta).$$

The order of updating does not matter — Bayes' theorem integrates information commutatively.

```python
import numpy as np
from scipy import stats

# Sequential updating: Beta-Binomial
alpha, beta = 2, 2  # prior

# Batch 1: 5 successes out of 10
alpha += 5
beta += 5
post1 = stats.beta(alpha, beta)
print(f"After batch 1: mean={post1.mean():.3f}, sd={post1.std():.3f}")

# Batch 2: 3 successes out of 10
alpha += 3
beta += 7
post2 = stats.beta(alpha, beta)
print(f"After batch 2: mean={post2.mean():.3f}, sd={post2.std():.3f}")

# Equivalent to updating on all data at once
alpha_all, beta_all = 2 + 5 + 3, 2 + 5 + 7
post_all = stats.beta(alpha_all, beta_all)
print(f"All at once:   mean={post_all.mean():.3f}, sd={post_all.std():.3f}")
```

This sequential property makes Bayesian inference ideal for:
- Online learning (models that update as data arrives)
- Adaptive clinical trials (early stopping rules)
- Recommendation systems (user preferences update with interactions)

### Credible Intervals

A **credible interval** is the Bayesian analog of a confidence interval. It has a more intuitive interpretation: there is a 95% probability the parameter lies in the 95% credible interval given the data.

**Equal-tailed interval (ETI):** The 2.5th and 97.5th percentiles of the posterior. The most common choice.

**Highest Posterior Density (HPD) interval:** The narrowest interval containing 95% of posterior probability. For symmetric posteriors, ETI and HPD coincide. For skewed posteriors, the HPD is shorter.

```python
import numpy as np
from scipy import stats

# Beta posterior for a proportion
alpha_post, beta_post = 10, 25
posterior = stats.beta(alpha_post, beta_post)

# Equal-tailed 95% interval
eti = posterior.ppf([0.025, 0.975])
print(f"95% ETI: [{eti[0]:.4f}, {eti[1]:.4f}]")

# Probability the parameter exceeds 0.3
prob_exceeds = 1 - posterior.cdf(0.3)
print(f"P(theta > 0.3 | data) = {prob_exceeds:.4f}")

# What the frequentist cannot say:
# "There is a 95% probability theta is in this interval."
```

**Interpretation advantage:** "There is a 95% probability that the conversion rate is between 20% and 40%" — a natural statement that frequentist confidence intervals cannot make.

**Decision-making with credible intervals:**
- If the entire interval is above a threshold: strong evidence the parameter exceeds the threshold
- If the interval includes the threshold: inconclusive
- The posterior probability of any hypothesis can be directly computed

### Bayes Factors

The **Bayes factor** compares the evidence from data for two competing models (or hypotheses):

$$BF_{12} = \frac{P(\text{data} \mid H_1)}{P(\text{data} \mid H_2)}.$$

**Interpretation (Kass & Raftery, 1995):**

| BF | Evidence against $H_2$ |
|----|----------------------|
| 1-3 | Not worth more than a bare mention |
| 3-20 | Positive |
| 20-150 | Strong |
| >150 | Very strong |

**Bayes factor vs p-value:** Bayes factors can quantify evidence for the null hypothesis ($BF < 1$), which p-values cannot. A large p-value may reflect insufficient data, not true null.

**Bayesian model comparison:**
- Bayes factors penalize complexity automatically (Occam's razor) — simpler models are favored unless the data strongly support a more complex one
- The Bayesian Information Criterion (BIC) is an approximation to the log Bayes factor

```python
import numpy as np
from scipy import stats

# Bayes factor for proportion: H0: p=0.5 vs H1: p~Beta(1,1)
# Data: 15 heads out of 20 flips
n, k = 20, 15
p0 = 0.5

# P(data | H0)
p_data_H0 = stats.binom.pmf(k, n, p0)

# P(data | H1) = integral of Binomial(k|n,p) * Beta(p|1,1) dp
# = Beta-Binomial(1, 1, n, k)
from scipy.special import comb, beta as beta_func
def beta_binomial_pmf(k, n, alpha, beta):
    return comb(n, k) * beta_func(k + alpha, n - k + beta) / beta_func(alpha, beta)

p_data_H1 = beta_binomial_pmf(k, n, 1, 1)
bf = p_data_H1 / p_data_H0

print(f"P(data | H0): {p_data_H0:.4f}")
print(f"P(data | H1): {p_data_H1:.4f}")
print(f"Bayes factor BF10: {bf:.2f}")
```

### Markov Chain Monte Carlo

**MCMC** is a computational method for sampling from posterior distributions when analytical solutions are unavailable. It constructs a Markov chain whose stationary distribution is the target posterior.

**Metropolis-Hastings algorithm:**
1. Start at current parameter value $\theta_t$
2. Propose a new value $\theta^*$ from a proposal distribution $q(\theta^* \mid \theta_t)$
3. Compute acceptance ratio: $r = \frac{P(\theta^* \mid \text{data})}{P(\theta_t \mid \text{data})} \times \frac{q(\theta_t \mid \theta^*)}{q(\theta^* \mid \theta_t)}$
4. Accept $\theta^*$ with probability $\min(1, r)$
5. Repeat

**Hamiltonian Monte Carlo (HMC):** Uses gradient information to propose efficient moves. Used by Stan and PyMC.

**Diagnostics:**
- Trace plots: ensure the chain mixes well (no drift or stuck values)
- $\hat{R}$ (Gelman-Rubin): compare within-chain to between-chain variance (should be $< 1.01$)
- Effective sample size (ESS): how many independent samples the chain is equivalent to
- Autocorrelation: high autocorrelation means inefficient sampling

```python
import numpy as np

# Simple Metropolis-Hastings for a Beta-Binomial model
np.random.seed(42)

# Data: 8 successes out of 20 trials
successes, trials = 8, 20

# Prior: Beta(2, 2)
alpha_prior, beta_prior = 2, 2

# Log-posterior (up to constant)
def log_posterior(p):
    if p <= 0 or p >= 1:
        return -np.inf
    # Binomial likelihood
    log_lik = successes * np.log(p) + (trials - successes) * np.log(1 - p)
    # Beta prior
    log_prior = (alpha_prior - 1) * np.log(p) + (beta_prior - 1) * np.log(1 - p)
    return log_lik + log_prior

# Metropolis-Hastings
n_iter = 10000
samples = np.zeros(n_iter)
current = 0.5
samples[0] = current
accepted = 0

for i in range(1, n_iter):
    # Proposal: random walk on logit scale
    proposal = current + np.random.normal(0, 0.1)
    if proposal < 0 or proposal > 1:
        log_accept_ratio = -np.inf
    else:
        log_accept_ratio = log_posterior(proposal) - log_posterior(current)
    if np.log(np.random.uniform()) < log_accept_ratio:
        current = proposal
        accepted += 1
    samples[i] = current

# Diagnostics
print(f"Acceptance rate: {accepted/n_iter:.3f}")
print(f"Posterior mean:  {np.mean(samples):.3f}")
print(f"Posterior sd:    {np.std(samples):.3f}")
print(f"95% CI:          [{np.percentile(samples, 2.5):.3f}, {np.percentile(samples, 97.5):.3f}]")
```

### Bayesian A/B Testing

In Bayesian A/B testing, we model conversion rates as Beta-distributed and compute the posterior probability that one variant is better.

**Model:**
- $p_A \sim \text{Beta}(\alpha_0, \beta_0)$ (prior)
- $p_B \sim \text{Beta}(\alpha_0, \beta_0)$ (prior)
- $X_A \sim \text{Binomial}(n_A, p_A)$ (likelihood)
- $X_B \sim \text{Binomial}(n_B, p_B)$ (likelihood)

**Posteriors:**
- $p_A \mid \text{data} \sim \text{Beta}(\alpha_0 + x_A, \beta_0 + n_A - x_A)$
- $p_B \mid \text{data} \sim \text{Beta}(\alpha_0 + x_B, \beta_0 + n_B - x_B)$

**Decision metrics:**
- $P(p_B > p_A \mid \text{data})$: Probability B beats A
- Expected loss: risk associated with choosing one variant
- Value of information: expected improvement from further data

```python
import numpy as np
from scipy import stats

np.random.seed(42)

# A/B test data
n_a, n_b = 2000, 2000
conv_a, conv_b = 104, 122

# Prior: Beta(1, 1) — uniform
alpha_prior, beta_prior = 1, 1

# Posteriors
post_a = stats.beta(alpha_prior + conv_a, beta_prior + n_a - conv_a)
post_b = stats.beta(alpha_prior + conv_b, beta_prior + n_b - conv_b)

# Monte Carlo: probability B beats A
n_samples = 100000
samples_a = post_a.rvs(n_samples)
samples_b = post_b.rvs(n_samples)
prob_b_better = np.mean(samples_b > samples_a)

print(f"Posterior mean A: {post_a.mean():.4f}")
print(f"Posterior mean B: {post_b.mean():.4f}")
print(f"P(B > A):          {prob_b_better:.4f}")

# Expected loss if we choose A when B is better
expected_loss = np.mean((samples_b - samples_a) * (samples_b > samples_a))
print(f"Expected loss of choosing A: {expected_loss:.6f}")

# 95% credible interval for the difference
diff = samples_b - samples_a
ci_diff = np.percentile(diff, [2.5, 97.5])
print(f"95% CI for difference: [{ci_diff[0]:.6f}, {ci_diff[1]:.6f}]")
```

### Bayesian vs Frequentist: Practical Guide

**Use Bayesian methods when:**
- You have strong prior information to incorporate
- You need intuitive probability statements about parameters
- Your problem involves sequential updating (online learning)
- You have small data and realistic priors provide regularization
- You need to make decisions under uncertainty (decision theory)
- You need to incorporate multiple sources of uncertainty

**Use frequentist methods when:**
- You need to control long-run error rates (regulatory, FDA)
- You have very large data (priors are irrelevant)
- You need widely accepted standard methods (textbook t-tests)
- Computational simplicity is important

**In practice:** Many modern data scientists use both. Bayesian for modeling and uncertainty quantification; frequentist for validation and controlling error rates.

**Calibration check:** For a well-calibrated Bayesian model, 95% credible intervals should contain the true value approximately 95% of the time in repeated sampling.

## Glossary

| Term | Definition |
|------|------------|
| Prior distribution | Initial belief about a parameter before seeing data |
| Likelihood | Probability of data given the parameter |
| Posterior distribution | Updated belief about a parameter after seeing data |
| Conjugate prior | Prior that yields a posterior in the same family |
| Bayes factor | Ratio of marginal likelihoods comparing two models |
| Credible interval | Interval containing a given posterior probability |
| Highest posterior density (HPD) | Narrowest interval with given posterior probability |
| MAP estimate | Maximum a posteriori — posterior mode |
| MCMC | Markov Chain Monte Carlo — sampling from complex posteriors |
| Metropolis-Hastings | MCMC algorithm using accept/reject proposals |
| Hamiltonian Monte Carlo | Gradient-based MCMC for efficient sampling |
| Effective sample size | Number of independent MCMC samples equivalent |
| Bayesian updating | Sequential application of Bayes theorem |
| Partial pooling | Shrinkage estimates toward a common mean |
| Hierarchical model | Multi-level model with parameters at multiple levels |
| Empirical Bayes | Using data to estimate prior hyperparameters |
| Thompson sampling | Exploration strategy using posterior samples |
| Bayesian model averaging | Weighting predictions by posterior model probabilities |
| Ridge regression | Bayesian linear regression with normal prior on coefficients |
| Laplace smoothing | Beta(1,1) prior for categorical counts |
| Jeffreys prior | Non-informative prior invariant to reparameterization |
| Improper prior | Prior that does not integrate to a finite number |

## Quick References

- [Bayesian Data Analysis, Gelman et al.](http://www.stat.columbia.edu/~gelman/book/) — the definitive textbook
- [Statistical Rethinking, McElreath](https://xcelab.net/rm/statistical-rethinking/) — accessible introduction with R code
- [PyMC Documentation](https://docs.pymc.io/) — probabilistic programming in Python
- [Bayesian Methods for Hackers](https://github.com/CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers) — practical examples
- [Kass & Raftery (1995): Bayes Factors](https://www.tandfonline.com/doi/abs/10.1080/01621459.1995.10476572) — standard reference for Bayes factors
- [Gelman-Rubin Diagnostic](https://www.jstor.org/stable/2246136) — MCMC convergence assessment

## Next Steps

This module is the final topic in the Probability & Statistics sequence. From here, you can explore:

- Linear Regression in the Mathematics library
- Machine Learning fundamentals in the AI/ML section
- Bayesian Networks and graphical models
- Probabilistic programming with PyMC or Stan
