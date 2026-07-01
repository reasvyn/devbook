# Estimation and Hypothesis Testing

## Description

Statistical inference uses data to draw conclusions about populations. Point estimation provides best guesses; confidence intervals express uncertainty; hypothesis tests decide between competing claims. These tools are essential for conversion rate analysis, code performance comparisons, model evaluation, A/B testing, and any situation where decisions must account for random variation.

## Prerequisites

- [Limit Theorems](limit-theorems.md) — CLT, LLN, sampling distributions, standard error
- [Descriptive Statistics](descriptive-statistics.md) — summary statistics, data visualization

## Table of Contents

- [Statistical Inference Framework](#statistical-inference-framework)
- [Point Estimation](#point-estimation)
- [Maximum Likelihood Estimation](#maximum-likelihood-estimation)
- [Properties of Estimators](#properties-of-estimators)
- [Confidence Intervals](#confidence-intervals)
- [Hypothesis Testing Framework](#hypothesis-testing-framework)
- [Type I and Type II Errors](#type-i-and-type-ii-errors)
- [p-values](#p-values)
- [t-Tests](#t-tests)
- [Chi-Square Tests](#chi-square-tests)
- [ANOVA](#anova)
- [Power Analysis](#power-analysis)
- [Multiple Testing Correction](#multiple-testing-correction)
- [Effect Size](#effect-size)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Statistical Inference Framework

Statistical inference answers questions like:
- What is the true conversion rate? (estimation)
- Is the new version better than the old? (hypothesis test)
- By how much? (effect size)
- How certain are we? (confidence)

The framework:
1. Define the population and parameter of interest
2. Collect a representative sample
3. Compute a statistic (estimate) from the sample
4. Use the sampling distribution (via CLT) to quantify uncertainty

### Point Estimation

A **point estimate** is a single number that best approximates a population parameter.

| Parameter | Estimator | Formula |
|-----------|-----------|---------|
| Population mean $\mu$ | Sample mean $\bar{x}$ | $\frac{1}{n}\sum x_i$ |
| Population proportion $p$ | Sample proportion $\hat{p}$ | $x / n$ |
| Population variance $\sigma^2$ | Sample variance $s^2$ | $\frac{1}{n-1}\sum (x_i - \bar{x})^2$ |
| Population std $\sigma$ | Sample std $s$ | $\sqrt{s^2}$ |

**Common sense:** The sample mean is a natural estimate of the population mean. But why use $n-1$ for variance? Bessel's correction makes $s^2$ unbiased — $E[s^2] = \sigma^2$.

### Maximum Likelihood Estimation

**Maximum Likelihood Estimation (MLE)** finds the parameter values that maximize the probability of observing the data.

For i.i.d. data with PDF/PMF $f(x \mid \theta)$, the likelihood is:

$$L(\theta) = \prod_{i=1}^n f(x_i \mid \theta).$$

The MLE is $\hat{\theta} = \arg\max_\theta L(\theta)$, often computed by maximizing the log-likelihood $\ell(\theta) = \log L(\theta)$.

**Example — MLE for Bernoulli($p$):**

$$L(p) = \prod_{i=1}^n p^{x_i} (1-p)^{1-x_i} = p^{\sum x_i} (1-p)^{n - \sum x_i}.$$

$$\ell(p) = \left(\sum x_i\right) \log p + \left(n - \sum x_i\right) \log(1-p).$$

Set derivative to zero:

$$\frac{d\ell}{dp} = \frac{\sum x_i}{p} - \frac{n - \sum x_i}{1-p} = 0 \implies \hat{p} = \frac{\sum x_i}{n} = \bar{x}.$$

The sample proportion is the MLE — intuitive and theoretically justified.

```python
import numpy as np
from scipy import optimize, stats

np.random.seed(42)
data = np.random.binomial(1, 0.3, 100)
p_mle = np.mean(data)
print(f"MLE for p: {p_mle:.3f}")

# MLE for normal distribution
mu_mle = np.mean(data)
sigma_mle = np.std(data, ddof=0)  # MLE uses n, not n-1
print(f"MLE for mu:    {mu_mle:.3f}")
print(f"MLE for sigma: {sigma_mle:.3f}")
```

**Properties of MLE:**
- **Consistency:** $\hat{\theta}_n \xrightarrow{p} \theta$ as $n \to \infty$
- **Asymptotic normality:** $\sqrt{n}(\hat{\theta}_n - \theta) \xrightarrow{d} \text{Normal}(0, I(\theta)^{-1})$
- **Efficiency:** Among consistent estimators, MLE has the lowest asymptotic variance
- **Invariance:** If $\hat{\theta}$ is MLE for $\theta$, then $g(\hat{\theta})$ is MLE for $g(\theta)$

### Properties of Estimators

**Unbiasedness:** $E[\hat{\theta}] = \theta$. The estimator is correct on average.

**Consistency:** $\hat{\theta}_n \xrightarrow{p} \theta$ as $n \to \infty$. The estimator converges to the true value with more data.

**Efficiency:** Among estimators, the one with the smallest variance is most efficient.

**MSE (Mean Squared Error):** $\text{MSE}(\hat{\theta}) = E[(\hat{\theta} - \theta)^2] = \text{Bias}(\hat{\theta})^2 + \text{Var}(\hat{\theta})$.

The bias-variance tradeoff: sometimes a biased estimator with lower variance has lower MSE than an unbiased estimator with high variance.

```python
import numpy as np

np.random.seed(42)

# Compare variance estimators
pop = np.random.normal(0, 1, 1000)
n_samples = 1000
n = 20

mle_estimates = []
unbiased_estimates = []

for _ in range(n_samples):
    sample = np.random.choice(pop, n)
    mle_estimates.append(np.var(sample, ddof=0))
    unbiased_estimates.append(np.var(sample, ddof=1))

print(f"MLE variance:     mean={np.mean(mle_estimates):.4f}, var={np.var(mle_estimates):.4f}")
print(f"Unbiased variance: mean={np.mean(unbiased_estimates):.4f}, var={np.var(unbiased_estimates):.4f}")
```

### Confidence Intervals

A **confidence interval** (CI) is a range of plausible values for a parameter, computed from the sample. A 95% CI means: if we repeated the sampling process many times, 95% of intervals would contain the true parameter.

**Confidence interval for the mean (known $\sigma$):**

$$\bar{x} \pm z_{\alpha/2} \times \frac{\sigma}{\sqrt{n}}.$$

**Confidence interval for the mean (unknown $\sigma$):**

$$\bar{x} \pm t_{\alpha/2, n-1} \times \frac{s}{\sqrt{n}}.$$

The $t$ distribution replaces the normal when $\sigma$ is estimated, giving wider intervals for small $n$.

**Confidence interval for a proportion:**

$$\hat{p} \pm z_{\alpha/2} \times \sqrt{\frac{\hat{p}(1-\hat{p})}{n}}.$$

Requires $n\hat{p} \ge 10$ and $n(1-\hat{p}) \ge 10$. For small samples, use the Wilson or Clopper-Pearson interval.

**Confidence interval for the difference of two means:**

$$(\bar{x}_1 - \bar{x}_2) \pm t_{\alpha/2, \text{df}} \times \sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}.$$

```python
import numpy as np
from scipy import stats

np.random.seed(42)
data = np.random.normal(100, 15, 50)
n = len(data)
x_bar = np.mean(data)
s = np.std(data, ddof=1)
se = s / np.sqrt(n)

# 95% CI using t-distribution
t_crit = stats.t.ppf(0.975, n-1)
ci = (x_bar - t_crit * se, x_bar + t_crit * se)
print(f"Sample mean: {x_bar:.2f}")
print(f"95% CI: [{ci[0]:.2f}, {ci[1]:.2f}]")

# Compare with normal-based CI (slightly narrower)
z_crit = stats.norm.ppf(0.975)
ci_normal = (x_bar - z_crit * se, x_bar + z_crit * se)
print(f"95% CI (normal): [{ci_normal[0]:.2f}, {ci_normal[1]:.2f}]")
```

**Interpretation warning:** A 95% CI does NOT mean "there is a 95% probability the parameter lies in this interval." The parameter is fixed (it either is or is not in the interval). The 95% refers to the procedure's long-run coverage rate.

### Hypothesis Testing Framework

A **hypothesis test** decides between two competing claims using data.

**Null hypothesis ($H_0$):** The default assumption, typically "no effect" or "no difference."

**Alternative hypothesis ($H_1$ or $H_a$):** The claim we want to establish. Can be:
- Two-sided: $\mu \ne \mu_0$
- One-sided (greater): $\mu > \mu_0$
- One-sided (less): $\mu < \mu_0$

**Test statistic:** A number computed from the data that measures the discrepancy from $H_0$.

**Steps:**
1. State $H_0$ and $H_1$
2. Choose a significance level $\alpha$ (typically 0.05)
3. Compute the test statistic
4. Compute the p-value
5. If $p < \alpha$, reject $H_0$; otherwise, do not reject

**Example — One-sample t-test:** Is the mean CPU time different from 200ms?

$$H_0: \mu = 200, \quad H_1: \mu \ne 200$$

$$t = \frac{\bar{x} - 200}{s / \sqrt{n}}.$$

### Type I and Type II Errors

| | $H_0$ true | $H_0$ false |
|---|---|---|
| **Reject $H_0$** | Type I error (false positive) $\alpha$ | Correct decision (true positive) |
| **Fail to reject $H_0$** | Correct decision (true negative) | Type II error (false negative) $\beta$ |

**Type I error rate ($\alpha$):** The significance level. Probability of rejecting a true null. Usually set to 0.05.

**Type II error rate ($\beta$):** Probability of failing to reject a false null. Depends on sample size, effect size, and $\alpha$.

**Power ($1 - \beta$):** Probability of correctly rejecting a false null. A study should be designed to have adequate power (typically 80% or higher).

**Analogy — Criminal trial:**
- $H_0$: defendant is innocent
- Type I error: convict an innocent person
- Type II error: acquit a guilty person

```python
import numpy as np
from scipy import stats

# Simulate Type I error rate
np.random.seed(42)
n_tests = 10000
n = 30
alpha = 0.05
rejections = 0

for _ in range(n_tests):
    sample = np.random.normal(0, 1, n)  # H0 is true (mu = 0)
    t_stat, p_value = stats.ttest_1samp(sample, 0)
    if p_value < alpha:
        rejections += 1

print(f"Type I error rate: {rejections/n_tests:.4f} (expected {alpha})")
```

### p-values

The **p-value** is the probability of observing a test statistic as extreme as (or more extreme than) the one observed, assuming $H_0$ is true.

**Common misconceptions:**
- The p-value is NOT the probability that $H_0$ is true
- The p-value is NOT the probability that the result was due to chance
- A large p-value does NOT mean $H_0$ is true — the test may lack power
- 0.05 is a convention, not a magical threshold

**ASA Statement on p-values (2016):**
- p-values can indicate how incompatible the data are with a specified statistical model
- p-values do not measure the probability that the studied hypothesis is true
- Scientific conclusions should not be based only on whether a p-value passes a specific threshold

```python
import numpy as np
from scipy import stats

# Two-sample t-test
np.random.seed(42)
group_a = np.random.normal(100, 15, 50)
group_b = np.random.normal(105, 15, 50)

t_stat, p_value = stats.ttest_ind(group_a, group_b)
print(f"t-statistic: {t_stat:.3f}")
print(f"p-value:     {p_value:.4f}")

if p_value < 0.05:
    print("Reject H0: significant difference")
else:
    print("Cannot reject H0: no significant difference detected")
```

### t-Tests

**One-sample t-test:** Compare sample mean to a known value.

$$t = \frac{\bar{x} - \mu_0}{s / \sqrt{n}}, \quad \text{df} = n - 1.$$

**Two-sample t-test (independent):** Compare means of two independent groups.

$$t = \frac{\bar{x}_1 - \bar{x}_2}{\sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}}, \quad \text{df} \approx \text{Welch-Satterthwaite}.$$

Always use the Welch version (unequal variance assumed) unless you have strong evidence of equal variances.

**Paired t-test:** Compare two measurements on the same subjects (before/after).

$$t = \frac{\bar{d}}{s_d / \sqrt{n}}, \quad d_i = x_{\text{after},i} - x_{\text{before},i}.$$

**Assumptions:**
- Independence of observations
- Data approximately normal (robust for $n > 30$ by CLT)
- For paired test: normality of differences

```python
import numpy as np
from scipy import stats

np.random.seed(42)

# Paired t-test: pre/post test scores
pre = np.random.normal(70, 10, 30)
post = pre + np.random.normal(5, 5, 30)  # improvement

t_stat, p_value = stats.ttest_rel(pre, post)
print(f"Paired t-test: t={t_stat:.3f}, p={p_value:.4f}")

# Effect size (Cohen's d)
d = np.mean(post - pre) / np.std(post - pre, ddof=1)
print(f"Cohen's d: {d:.3f}")
```

### Chi-Square Tests

**Chi-square goodness-of-fit test:** Tests whether observed frequencies match expected frequencies.

$$\chi^2 = \sum_{i=1}^k \frac{(O_i - E_i)^2}{E_i}, \quad \text{df} = k - 1 - \text{parameters estimated}.$$

**Chi-square test of independence:** Tests whether two categorical variables are associated.

$$\chi^2 = \sum_{i=1}^r \sum_{j=1}^c \frac{(O_{ij} - E_{ij})^2}{E_{ij}}, \quad \text{df} = (r-1)(c-1).$$

Expected frequency under independence: $E_{ij} = \text{row total}_i \times \text{column total}_j / \text{grand total}$.

**Assumptions:**
- Expected frequencies $\ge 5$ (some texts allow $\ge 1$ with correction)
- Independent observations
- Categorical data

```python
import numpy as np
from scipy import stats

# Chi-square test of independence
# Contingency table: device type x conversion
observed = np.array([
    [120, 880],   # mobile: 120 converted, 880 not
    [80, 920]     # desktop: 80 converted, 920 not
])

chi2, p_value, dof, expected = stats.chi2_contingency(observed)
print(f"Chi-square: {chi2:.3f}, p={p_value:.4f}, df={dof}")
print(f"Expected under independence:\n{expected}")
```

### ANOVA

**Analysis of Variance (ANOVA)** tests whether means of three or more groups differ. Despite the name, it compares means by analyzing variance.

**One-way ANOVA:**

$$F = \frac{\text{between-group variance}}{\text{within-group variance}} = \frac{\text{SS}_{\text{between}} / (k-1)}{\text{SS}_{\text{within}} / (N - k)}.$$

Where $k$ is the number of groups and $N$ is total observations. Under $H_0$, $F \sim F_{k-1, N-k}$.

**Assumptions:**
- Independence
- Normality within groups (robust for large $n$)
- Homogeneity of variance (check with Levene's test)

```python
import numpy as np
from scipy import stats

np.random.seed(42)

# Three groups with different means
group1 = np.random.normal(100, 10, 30)
group2 = np.random.normal(105, 10, 30)
group3 = np.random.normal(110, 10, 30)

f_stat, p_value = stats.f_oneway(group1, group2, group3)
print(f"One-way ANOVA: F={f_stat:.3f}, p={p_value:.4f}")

# Post-hoc: which groups differ significantly?
# (Tukey HSD would be used here, but scipy doesn't have it built-in)
```

**Why ANOVA and not multiple t-tests?** If you test $k$ groups pairwise, you need $\binom{k}{2}$ tests, inflating Type I error. ANOVA tests all groups simultaneously.

**Two-way ANOVA:** Tests the effects of two categorical factors and their interaction on a continuous outcome.

### Power Analysis

**Power** is the probability of detecting an effect when one truly exists. Power depends on:

1. **Effect size** (larger effect = higher power)
2. **Sample size** (larger sample = higher power)
3. **Significance level** (larger $\alpha$ = higher power)
4. **Variability** (lower variance = higher power)

**Cohen's effect size conventions:**
- $d = 0.2$: small
- $d = 0.5$: medium
- $d = 0.8$: large

```python
import numpy as np
from scipy import stats

def power_two_sample_t(d, n1, n2, alpha=0.05):
    """Compute power for a two-sample t-test."""
    df = n1 + n2 - 2
    t_crit = stats.t.ppf(1 - alpha/2, df)
    # Non-centrality parameter
    ncp = d * np.sqrt(n1 * n2 / (n1 + n2))
    power = 1 - stats.nct.cdf(t_crit, df, ncp) + stats.nct.cdf(-t_crit, df, ncp)
    return power

# Power to detect d=0.3 with n=100 per group
print(f"Power (d=0.3, n=100): {power_two_sample_t(0.3, 100, 100):.3f}")
print(f"Power (d=0.5, n=100): {power_two_sample_t(0.5, 100, 100):.3f}")
print(f"Power (d=0.3, n=500): {power_two_sample_t(0.3, 500, 500):.3f}")
```

**Sample size calculation:** Rearranging the power formula tells you how many samples you need.

```python
def required_n_per_group(d, power=0.80, alpha=0.05):
    """Find n needed per group for desired power."""
    n = 10
    while power_two_sample_t(d, n, n, alpha) < power:
        n += 1
    return n

for d in [0.2, 0.3, 0.5, 0.8]:
    n = required_n_per_group(d)
    print(f"d={d}: need n={n} per group for 80% power")
```

### Multiple Testing Correction

When testing many hypotheses simultaneously, the chance of at least one false positive increases. If you test 100 independent hypotheses with $\alpha = 0.05$, you expect 5 false positives by chance.

**Bonferroni correction:** Adjust $\alpha$ by dividing by the number of tests:

$$\alpha_{\text{adjusted}} = \frac{\alpha}{m}.$$

Very conservative — controls the **Family-Wise Error Rate (FWER)** but reduces power.

**Benjamini-Hochberg (FDR):** Controls the **False Discovery Rate** — the expected proportion of false positives among rejected hypotheses. Less conservative than Bonferroni.

Procedure:
1. Sort p-values: $p_{(1)} \le p_{(2)} \le \cdots \le p_{(m)}$
2. Find largest $k$ such that $p_{(k)} \le \frac{k}{m} \alpha$
3. Reject all $H_{(i)}$ for $i = 1, \dots, k$

```python
import numpy as np

# Simulate 1000 tests, 50 of which have true effects
np.random.seed(42)
m = 1000
true_effects = np.zeros(m)
true_effects[:50] = 1

# Generate p-values
p_values = np.where(
    true_effects == 1,
    np.random.beta(1, 30, m),  # small p-values for true effects
    np.random.uniform(0, 1, m)  # uniform for null
)

# Bonferroni
bonf_threshold = 0.05 / m
bonf_rejections = np.sum(p_values < bonf_threshold)

# Benjamini-Hochberg
sorted_idx = np.argsort(p_values)
sorted_p = p_values[sorted_idx]
thresholds = np.arange(1, m + 1) / m * 0.05
below_threshold = sorted_p < thresholds
if np.any(below_threshold):
    k = np.max(np.where(below_threshold))
    bh_rejections = k + 1
else:
    bh_rejections = 0

print(f"Bonferroni rejections: {bonf_rejections}")
print(f"BH rejections:         {bh_rejections}")
print(f"True positives:        {np.sum(true_effects)}")
```

### Effect Size

A statistically significant result is not necessarily practically significant. **Effect size** measures the magnitude of the difference.

**Cohen's d:** $d = (\bar{x}_1 - \bar{x}_2) / s_{\text{pooled}}$.

**Hedges' g:** Like Cohen's d but with a correction for small sample bias.

**Odds ratio:** For binary outcomes, $\text{OR} = \frac{p_1/(1-p_1)}{p_2/(1-p_2)}$.

**Correlation coefficient $r$:** Can be interpreted as an effect size ($r = 0.1$ small, $r = 0.3$ medium, $r = 0.5$ large).

```python
import numpy as np
from scipy import stats

np.random.seed(42)

# Two groups with a small but statistically significant difference
n = 500
group_a = np.random.normal(100, 15, n)
group_b = np.random.normal(103, 15, n)

t_stat, p_value = stats.ttest_ind(group_a, group_b)
cohens_d = (np.mean(group_b) - np.mean(group_a)) / np.sqrt(
    (np.var(group_a, ddof=1) + np.var(group_b, ddof=1)) / 2
)

print(f"p-value:    {p_value:.4f}")
print(f"Cohen's d:  {cohens_d:.3f}")
print(f"Difference: {np.mean(group_b) - np.mean(group_a):.2f}")
```

**Report both:** "The new algorithm reduced latency by 12ms (t(998) = 2.31, p = 0.021, d = 0.15)." The p-value shows statistical significance; the effect size shows practical significance.

## Glossary

| Term | Definition |
|------|------------|
| Point estimate | A single number estimating a population parameter |
| MLE | Maximum likelihood estimator — maximizes probability of observed data |
| Bias | Difference between expected value of estimator and true parameter |
| Consistency | Estimator converges to true value as sample size grows |
| Confidence interval | Range of plausible values for a parameter |
| Significance level $\alpha$ | Probability of Type I error (rejecting a true null) |
| p-value | Probability of observed or more extreme data under $H_0$ |
| Type I error | False positive — rejecting a true null hypothesis |
| Type II error | False negative — failing to reject a false null |
| Power | Probability of correctly rejecting a false null ($1 - \beta$) |
| t-test | Test for comparing means using the t-distribution |
| ANOVA | Analysis of variance — comparing multiple group means |
| Chi-square test | Test for categorical data (independence or goodness-of-fit) |
| Effect size | Magnitude of a difference, independent of sample size |
| Cohen's d | Standardized mean difference |
| FWER | Family-wise error rate — probability of any false positive |
| FDR | False discovery rate — expected proportion of false positives |
| Bonferroni correction | Divides $\alpha$ by number of tests |
| Benjamini-Hochberg | FDR-controlling procedure |
| Bootstrap | Resampling method for estimating sampling distributions |
| Welch's t-test | Two-sample t-test without assuming equal variances |
| Paired test | Test for dependent observations (before/after) |
| Post-hoc test | Follow-up test after significant ANOVA |
| Non-centrality parameter | Parameter of non-central t/F distributions used in power calculations |

## Quick References

- [OpenIntro Statistics, Diez et al.](https://www.openintro.org/book/os/) — free, accessible textbook with excellent examples
- [Scipy Stats Tests](https://docs.scipy.org/doc/scipy/reference/stats.html#statistical-tests) — comprehensive list of hypothesis tests in scipy
- [StatQuest: Hypothesis Testing](https://statquest.org/) — video explanations of statistical concepts
- [Statistical Tests Cheatsheet (Wikipedia)](https://en.wikipedia.org/wiki/List_of_statistical_tests) — when to use which test
- [Improving Your Statistical Inferences, Lakens](https://lakens.github.io/statistical_inferences/) — modern best practices

## Next Steps

- [Bayesian Statistics](bayesian-statistics.md) — alternative approach to inference incorporating prior knowledge
