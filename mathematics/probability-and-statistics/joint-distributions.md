# Joint and Conditional Distributions

## Description

Real-world data rarely involves a single variable. Joint distributions model multiple random variables simultaneously, capturing how features relate to each other. Understanding joint, marginal, and conditional distributions is essential for multivariate data analysis, correlation analysis, feature relationships in machine learning, portfolio risk, and any application involving dependencies between variables.

## Prerequisites

- [Probability Foundations](probability-foundations.md) — conditional probability, independence, Bayes theorem
- [Probability Distributions](probability-distributions.md) — univariate distributions

## Table of Contents

- [Joint Distributions](#joint-distributions)
- [Marginal Distributions](#marginal-distributions)
- [Conditional Distributions](#conditional-distributions)
- [Independence for Random Variables](#independence-for-random-variables)
- [Covariance and Correlation](#covariance-and-correlation)
- [Conditional Expectation](#conditional-expectation)
- [Law of Total Expectation](#law-of-total-expectation)
- [Law of Total Variance](#law-of-total-variance)
- [Multivariate Normal Distribution](#multivariate-normal-distribution)
- [Joint Distributions in Machine Learning](#joint-distributions-in-machine-learning)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Joint Distributions

For two random variables $X$ and $Y$, the **joint distribution** describes the probability of their combined behavior.

**Discrete case — Joint PMF:**

$$p_{X,Y}(x, y) = P(X = x, Y = y).$$

Properties:
- $0 \le p_{X,Y}(x, y) \le 1$
- $\sum_x \sum_y p_{X,Y}(x, y) = 1$

**Continuous case — Joint PDF:**

$$f_{X,Y}(x, y) \ge 0, \quad \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} f_{X,Y}(x, y) \, dx \, dy = 1.$$

For a region $A$:

$$P((X, Y) \in A) = \iint_A f_{X,Y}(x, y) \, dx \, dy.$$

**Example — Joint PMF of two dice:** Let $X$ be the value of die 1, $Y$ the value of die 2. Each of the 36 pairs has probability $1/36$.

```python
import numpy as np
import pandas as pd

# Joint PMF of two fair dice
values = range(1, 7)
joint = np.ones((6, 6)) / 36
df = pd.DataFrame(joint, index=values, columns=values)
df.index.name = 'X'
df.columns.name = 'Y'
print("Joint PMF P(X, Y):")
print(df)
```

### Marginal Distributions

The **marginal distribution** recovers the distribution of one variable by summing (or integrating) over the other.

**Discrete:** $p_X(x) = \sum_y p_{X,Y}(x, y)$

**Continuous:** $f_X(x) = \int_{-\infty}^{\infty} f_{X,Y}(x, y) \, dy$

The term "marginal" comes from the practice of summing joint probabilities in the margins of a contingency table.

**Example — Marginal PMF of die 1 from joint:** $p_X(x) = \sum_{y=1}^6 1/36 = 1/6$ for each $x$. This matches the marginal uniform distribution of a single die.

```python
# Marginal from joint PMF
p_X = joint.sum(axis=1)  # sum over columns (Y)
p_Y = joint.sum(axis=0)  # sum over rows (X)
print(f"P(X) = {p_X}")
print(f"P(Y) = {p_Y}")
```

### Conditional Distributions

The **conditional distribution** of $Y$ given $X = x$ describes the behavior of $Y$ when $X$ is known.

**Discrete:** $p_{Y \mid X}(y \mid x) = \frac{p_{X,Y}(x, y)}{p_X(x)}$, provided $p_X(x) > 0$.

**Continuous:** $f_{Y \mid X}(y \mid x) = \frac{f_{X,Y}(x, y)}{f_X(x)}$, provided $f_X(x) > 0$.

This mirrors conditional probability for events: $P(A \mid B) = P(A \cap B) / P(B)$.

**Example — Conditional probability in a 2x2 table:**

Consider website visit data: 60% of visitors are on mobile, 40% on desktop. Conversion rates: mobile 2%, desktop 5%.

Joint distribution:
- $P(\text{mobile}, \text{convert}) = 0.60 \times 0.02 = 0.012$
- $P(\text{mobile}, \text{no convert}) = 0.60 \times 0.98 = 0.588$
- $P(\text{desktop}, \text{convert}) = 0.40 \times 0.05 = 0.020$
- $P(\text{desktop}, \text{no convert}) = 0.40 \times 0.95 = 0.380$

Conditional distribution of conversion given device:
- $P(\text{convert} \mid \text{mobile}) = 0.012 / 0.60 = 0.02$
- $P(\text{convert} \mid \text{desktop}) = 0.020 / 0.40 = 0.05$

This illustrates that knowing the device type changes the conversion probability.

### Independence for Random Variables

$X$ and $Y$ are **independent** if the joint distribution factorizes:

$$p_{X,Y}(x, y) = p_X(x) p_Y(y) \quad \text{(discrete)}$$
$$f_{X,Y}(x, y) = f_X(x) f_Y(y) \quad \text{(continuous)}$$

Equivalently, $F_{X,Y}(x, y) = F_X(x) F_Y(y)$, or $p_{Y \mid X}(y \mid x) = p_Y(y)$.

**Properties of independent variables:**
- $E[XY] = E[X]E[Y]$
- $\text{Var}(X + Y) = \text{Var}(X) + \text{Var}(Y)$
- $M_{X+Y}(t) = M_X(t) M_Y(t)$ (MGF factorization)

**Conditional independence:** $X$ and $Y$ are conditionally independent given $Z$ if:

$$f_{X,Y \mid Z}(x, y \mid z) = f_{X \mid Z}(x \mid z) f_{Y \mid Z}(y \mid z).$$

This is a cornerstone of Bayesian networks and probabilistic graphical models.

**Caveat:** Independence does not imply causation. Two variables may be independent in distribution yet have no causal relationship — or they may have a causal relationship that cancels out in marginal distribution.

### Covariance and Correlation

**Covariance** measures the direction of the linear relationship:

$$\text{Cov}(X, Y) = E[(X - E[X])(Y - E[Y])] = E[XY] - E[X]E[Y].$$

Properties:
- $\text{Cov}(X, X) = \text{Var}(X)$
- $\text{Cov}(X, Y) = \text{Cov}(Y, X)$
- $\text{Cov}(aX + b, cY + d) = ac\ \text{Cov}(X, Y)$
- $\text{Var}(X + Y) = \text{Var}(X) + \text{Var}(Y) + 2\text{Cov}(X, Y)$

**Correlation coefficient (Pearson's $\rho$):**

$$\rho_{X,Y} = \frac{\text{Cov}(X, Y)}{\sigma_X \sigma_Y}, \quad -1 \le \rho \le 1.$$

- $\rho = 1$: perfect positive linear relationship
- $\rho = -1$: perfect negative linear relationship
- $\rho = 0$: no linear relationship (but other relationships may exist)

```python
import numpy as np

np.random.seed(42)

# Generate correlated data
mean = [0, 0]
cov_matrix = [[1.0, 0.7], [0.7, 1.0]]  # correlation 0.7
data = np.random.multivariate_normal(mean, cov_matrix, 1000)
x, y = data[:, 0], data[:, 1]

print(f"Sample correlation: {np.corrcoef(x, y)[0, 1]:.4f}")
print(f"Sample covariance:  {np.cov(x, y)[0, 1]:.4f}")
```

**Limitations of Pearson correlation:**
- Only measures linear relationships
- Sensitive to outliers
- Not robust for non-normal data (Spearman's rank correlation is an alternative)

**Spearman's rank correlation:** Computes Pearson correlation on ranks. Measures monotonic (not just linear) relationships.

```python
from scipy import stats

# Non-linear relationship with strong monotonic trend
x = np.linspace(0, 5, 100)
y = np.exp(x) + np.random.normal(0, 2, 100)

pearson, _ = stats.pearsonr(x, y)
spearman, _ = stats.spearmanr(x, y)
print(f"Pearson:  {pearson:.4f}")
print(f"Spearman: {spearman:.4f}")
```

### Conditional Expectation

The **conditional expectation** $E[Y \mid X = x]$ is the expected value of $Y$ given that $X$ takes the value $x$.

**Discrete:** $E[Y \mid X = x] = \sum_y y \cdot p_{Y \mid X}(y \mid x)$

**Continuous:** $E[Y \mid X = x] = \int y \cdot f_{Y \mid X}(y \mid x) \, dy$

$E[Y \mid X]$ is a random variable — a function of $X$. This is a key concept in regression, where $E[Y \mid X]$ is the regression function.

**Properties:**
- $E[g(Y) \mid X] = g(E[Y \mid X])$ if $g$ is linear (not generally true for non-linear $g$)
- $E[Y \mid X] = E[Y]$ if $X$ and $Y$ are independent

**Regression interpretation:** In linear regression, we model $E[Y \mid X] = \beta_0 + \beta_1 X$. The conditional expectation captures how the mean of $Y$ changes with $X$.

### Law of Total Expectation

$$E[Y] = E[E[Y \mid X]].$$

This is one of the most useful identities in probability. It decomposes an expectation into the average of conditional expectations.

**Proof sketch:** $E[Y] = \int y f_Y(y) dy = \int y \left( \int f_{Y \mid X}(y \mid x) f_X(x) dx \right) dy = \int \left( \int y f_{Y \mid X}(y \mid x) dy \right) f_X(x) dx = \int E[Y \mid X = x] f_X(x) dx = E[E[Y \mid X]].$

**Example — Total expected response time:** Server response time depends on load state. In normal load (90% of time), mean is 100ms. In high load (10% of time), mean is 500ms.

$$E[T] = E[E[T \mid \text{load}]] = 0.9 \times 100 + 0.1 \times 500 = 140 \text{ms}.$$

```python
# Simulating Law of Total Expectation
import numpy as np

np.random.seed(42)
n = 100000

# Load state: 0=normal (90%), 1=high (10%)
load = np.random.choice([0, 1], n, p=[0.9, 0.1])
# Response time: ~Exponential(mean) depending on load
mean_normal, mean_high = 100, 500
times = np.where(load == 0,
                 np.random.exponential(mean_normal, n),
                 np.random.exponential(mean_high, n))

print(f"E[T] by conditioning: {0.9*100 + 0.1*500:.1f}ms")
print(f"E[T] by simulation:   {np.mean(times):.1f}ms")
```

### Law of Total Variance

$$\text{Var}(Y) = E[\text{Var}(Y \mid X)] + \text{Var}(E[Y \mid X]).$$

The total variance decomposes into:
1. **Within-group variance:** $E[\text{Var}(Y \mid X)]$ — average variance around each conditional mean
2. **Between-group variance:** $\text{Var}(E[Y \mid X])$ — variance of the conditional means

**Example — Variance decomposition in A/B testing:**

$$\text{Var}(\text{conversion}) = E[\text{Var}(\text{conversion} \mid \text{group})] + \text{Var}(E[\text{conversion} \mid \text{group}]).$$

The first term is the average variance within groups. The second term captures the variance attributable to the treatment effect. The ratio of between-group variance to total variance is related to $R^2$ in ANOVA.

### Multivariate Normal Distribution

The multivariate normal (MVN) is the most important multivariate distribution. For a $d$-dimensional random vector $\mathbf{X} = (X_1, \dots, X_d)^T$:

$$f(\mathbf{x}) = \frac{1}{\sqrt{(2\pi)^d |\Sigma|}} \exp\left(-\frac{1}{2} (\mathbf{x} - \boldsymbol{\mu})^T \Sigma^{-1} (\mathbf{x} - \boldsymbol{\mu})\right).$$

Where $\boldsymbol{\mu}$ is the mean vector and $\Sigma$ is the covariance matrix.

**Properties:**
- Any marginal distribution is normal
- Any conditional distribution is normal
- Linear combinations are normal
- Zero covariance implies independence (unique to MVN)

```python
import numpy as np

# Generate multivariate normal data
mu = np.array([0, 0, 0])
Sigma = np.array([
    [1.0, 0.5, 0.3],
    [0.5, 2.0, 0.1],
    [0.3, 0.1, 0.5]
])

np.random.seed(42)
data = np.random.multivariate_normal(mu, Sigma, size=1000)
print(f"Sample covariance:\n{np.cov(data, rowvar=False)}")
print(f"Marginal variance of X1: {np.var(data[:, 0]):.4f}")
```

**Conditional distribution of a multivariate normal:** If $\mathbf{X} = (\mathbf{X}_1, \mathbf{X}_2)$, then $\mathbf{X}_1 \mid \mathbf{X}_2 = \mathbf{x}_2$ is normal with:

$$E[\mathbf{X}_1 \mid \mathbf{X}_2 = \mathbf{x}_2] = \boldsymbol{\mu}_1 + \Sigma_{12} \Sigma_{22}^{-1} (\mathbf{x}_2 - \boldsymbol{\mu}_2)$$
$$\text{Var}[\mathbf{X}_1 \mid \mathbf{X}_2 = \mathbf{x}_2] = \Sigma_{11} - \Sigma_{12} \Sigma_{22}^{-1} \Sigma_{21}$$

This conditional expectation is linear in $\mathbf{x}_2$ — the theoretical basis for linear regression.

### Joint Distributions in Machine Learning

**Naive Bayes classifier:** Assumes features are conditionally independent given the class:

$$P(Y, X_1, \dots, X_d) = P(Y) \prod_{i=1}^d P(X_i \mid Y).$$

Despite the strong independence assumption, Naive Bayes works well for high-dimensional problems like text classification.

**Gaussian Mixture Models (GMM):** Model the joint distribution as a weighted sum of multivariate normals:

$$f(\mathbf{x}) = \sum_{k=1}^K \pi_k \mathcal{N}(\mathbf{x} \mid \boldsymbol{\mu}_k, \Sigma_k).$$

Each component represents a cluster. The posterior $P(k \mid \mathbf{x})$ gives cluster membership probabilities.

**Graphical models:** Use graphs to represent conditional independence relationships. In a Bayesian network, the joint distribution factorizes as:

$$P(X_1, \dots, X_d) = \prod_{i=1}^d P(X_i \mid \text{parents}(X_i)).$$

**Copulas:** Separate the marginal distributions from the dependence structure. For any continuous joint distribution:

$$F_{X,Y}(x, y) = C(F_X(x), F_Y(y)).$$

The copula $C$ captures the dependence structure independent of margins. This is used in finance for modeling asset dependencies beyond linear correlation.

```python
import numpy as np
from scipy import stats

# Gaussian copula with t-distributed margins
np.random.seed(42)
n = 1000
rho = 0.7

# Generate MVN copula
mvn = stats.multivariate_normal(mean=[0, 0], cov=[[1, rho], [rho, 1]])
u = mvn.cdf(np.random.multivariate_normal([0, 0], [[1, rho], [rho, 1]], n))

# Transform to t-distributed margins (df=3)
df = 3
x = stats.t.ppf(u[:, 0], df)
y = stats.t.ppf(u[:, 1], df)
print(f"Correlation: {np.corrcoef(x, y)[0, 1]:.4f}")
```

### Copula Families and Their Properties

Different copula families capture different dependence structures:

**Gaussian copula:** Derived from the multivariate normal distribution. It captures linear correlation but has no tail dependence — extreme events in one variable do not increase the probability of extreme events in the other.

$$
C_{\rho}(u, v) = \Phi_{\rho}(\Phi^{-1}(u), \Phi^{-1}(v))
$$

**Clayton copula:** Exhibits strong lower tail dependence — variables tend to be correlated during extreme negative events. Used in finance for modeling asset returns during market crashes.

**Frank copula:** Has no tail dependence and allows both positive and negative dependence over its full range. Suitable for data with weak symmetric dependence.

**Gumbel copula:** Exhibits upper tail dependence — variables are correlated during extreme positive events. Used in hydrology for modeling flood peaks and in insurance for extreme claims.

```python
import numpy as np
from scipy import stats

def clayton_copula_samples(theta, n=1000):
    """Generate samples from a Clayton copula."""
    v = np.random.exponential(1, n)
    u1 = np.random.uniform(0, 1, n)
    u2 = (1 + v**(-1/theta) * (u1**(-theta) - 1))**(-1/theta)
    return u1, u2

theta = 2.0  # Controls dependence strength
u1, u2 = clayton_copula_samples(theta, 5000)
kendall_tau = stats.kendalltau(u1, u2)[0]
print(f"Clayton(theta={theta}): Kendall's tau = {kendall_tau:.4f}")
```

### Transformations of Random Vectors

When transforming a random vector $\mathbf{X}$ to $\mathbf{Y} = g(\mathbf{X})$, the joint distribution changes according to the **change of variables** formula.

For a bijective, differentiable transformation $\mathbf{Y} = g(\mathbf{X})$:

$$
f_{\mathbf{Y}}(\mathbf{y}) = f_{\mathbf{X}}(g^{-1}(\mathbf{y})) \cdot |\det J_{g^{-1}}(\mathbf{y})|
$$

where $J_{g^{-1}}$ is the Jacobian matrix of the inverse transformation.

**Linear transformations:** If $\mathbf{Y} = A\mathbf{X} + \mathbf{b}$ where $A$ is invertible:

$$
f_{\mathbf{Y}}(\mathbf{y}) = \frac{1}{|\det A|} f_{\mathbf{X}}(A^{-1}(\mathbf{y} - \mathbf{b}))
$$

**Sum of random variables:** For $Z = X + Y$:

$$
f_Z(z) = \int_{-\infty}^{\infty} f_{X,Y}(x, z - x) \, dx
$$

If $X$ and $Y$ are independent, this becomes the convolution:

$$
f_Z(z) = \int_{-\infty}^{\infty} f_X(x) f_Y(z - x) \, dx
$$

**Applications:**
- **Portfolio theory.** The return of a portfolio is a linear combination of asset returns. The distribution of portfolio returns determines value-at-risk and expected shortfall.
- **Image processing.** Color space transformations (RGB to HSV) are bijective transformations of random vectors. Understanding how noise propagates through these transformations guides filter design.
- **Kalman filters.** The prediction step applies a linear transformation to the state estimate. The covariance update uses the Jacobian to transform uncertainty.

### Order Statistics

Order statistics are the sorted values of a random sample: $X_{(1)} \leq X_{(2)} \leq \dots \leq X_{(n)}$.

**Distribution of the $k$th order statistic:**

$$
f_{X_{(k)}}(x) = \frac{n!}{(k-1)!(n-k)!} F_X(x)^{k-1} (1 - F_X(x))^{n-k} f_X(x)
$$

**Joint distribution of minimum and maximum:**

$$
f_{X_{(1)}, X_{(n)}}(x, y) = n(n-1) (F_X(y) - F_X(x))^{n-2} f_X(x) f_X(y), \quad x < y
$$

**Applications:**
- **Outlier detection.** The distribution of the maximum provides a threshold for identifying extreme values under the null hypothesis.
- **Extreme value theory.** The Fisher-Tippett-Gnedenko theorem states that the distribution of the normalized maximum converges to one of three extreme value distributions (Gumbel, Frechet, Weibull), regardless of the original distribution.
- **Rank-based statistics.** The Wilcoxon rank-sum test and the Mann-Whitney U test are based on order statistics. They provide non-parametric alternatives to the $t$-test that do not assume normality.

```python
import numpy as np
from scipy import stats

# Distribution of the maximum of n exponential(1) variables
n = 10
samples = np.random.exponential(1, (50000, n))
maxima = np.max(samples, axis=1)
# Theoretical: P(max <= x) = (1 - exp(-x))^n
x = np.linspace(0, 8, 100)
empirical_cdf = np.mean(maxima[:, None] <= x, axis=0)
theoretical_cdf = (1 - np.exp(-x))**n
print(f"Max deviation: {np.max(np.abs(empirical_cdf - theoretical_cdf)):.4f}")
```

### Applications in Machine Learning

Joint distributions appear throughout ML:

**Linear discriminant analysis (LDA):** Models each class as a multivariate normal distribution with a shared covariance matrix. The decision boundary between classes is linear in feature space.

**Gaussian processes:** Define a joint distribution over function values. Any finite set of points has a multivariate normal distribution specified by a mean function and covariance kernel. Predictions condition on observed data using the conditional normal formula.

**Variational autoencoders:** Model the joint distribution $p(x, z)$ where $x$ is the data and $z$ is a latent representation. The encoder approximates $p(z|x)$ and the decoder approximates $p(x|z)$. Training maximizes the evidence lower bound (ELBO).

**Generative adversarial networks (GANs):** Learn the joint distribution of data and noise implicitly through adversarial training. The generator learns $p(x|z)$ while the discriminator learns to distinguish real from generated samples.

## Glossary

| Term | Definition |
|------|------------|
| Joint distribution | Distribution of two or more random variables together |
| Marginal distribution | Distribution of a single variable, obtained by summing/integrating over others |
| Conditional distribution | Distribution of one variable given fixed values of others |
| Independence | Factorization of joint distribution into product of marginals |
| Covariance | Measure of linear relationship between two variables |
| Correlation coefficient | Normalized covariance, ranging from -1 to 1 |
| Conditional expectation | Expected value of one variable conditional on another |
| Law of Total Expectation | $E[Y] = E[E[Y \mid X]]$ |
| Law of Total Variance | $\text{Var}(Y) = E[\text{Var}(Y \mid X)] + \text{Var}(E[Y \mid X])$ |
| Multivariate normal | The most important multivariate distribution |
| Conditional independence | Independence given a third variable |
| Copula | Separates marginal distributions from dependence structure |
| Uncorrelated | Covariance equals zero (does not imply independence) |
| Homoscedasticity | Constant conditional variance |
| Regression function | $E[Y \mid X]$ as a function of $X$ |
| Gaussian mixture model | Weighted sum of multivariate normal distributions |
| Bayesian network | Graphical model encoding conditional independence |
| Order statistics | Sorted values of a random sample |
| Change of variables | Formula for distribution of transformed random vectors |
| Extreme value distribution | Limiting distribution of sample maxima |
| Gaussian process | Distribution over functions with multivariate normal finite marginals |
| LDA | Linear discriminant analysis — classification via class-conditional MVN |

## Quick References

- [Elements of Statistical Learning, Hastie et al.](https://hastie.su.domains/ElemStatLearn/) — joint distributions in predictive modeling
- [Multivariate Normal Distribution (Wikipedia)](https://en.wikipedia.org/wiki/Multivariate_normal_distribution) — properties and formulas
- [Law of Total Expectation (Wikipedia)](https://en.wikipedia.org/wiki/Law_of_total_expectation) — statement and proof
- [Copula (Wikipedia)](https://en.wikipedia.org/wiki/Copula_(probability_theory)) — dependence modeling beyond correlation

## Next Steps

- [Limit Theorems](limit-theorems.md) — LLN, CLT, and their implications
