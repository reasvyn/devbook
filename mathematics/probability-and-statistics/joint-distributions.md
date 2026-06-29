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

## Study Cases

### Case 1: Feature Relationships in Credit Risk

A bank models the joint distribution of a borrower's income ($X$, in $1000s) and debt-to-income ratio ($Y$). Based on historical data, the joint distribution is approximately bivariate normal:

$$\boldsymbol{\mu} = \begin{pmatrix} 60 \\ 0.30 \end{pmatrix}, \quad \Sigma = \begin{pmatrix} 400 & 1.6 \\ 1.6 & 0.01 \end{pmatrix}.$$

Given a borrower with income of $80,000$, what is the conditional distribution of their debt-to-income ratio?

$$E[Y \mid X = 80] = 0.30 + \frac{1.6}{400} (80 - 60) = 0.30 + 0.08 = 0.38.$$

$$\text{Var}(Y \mid X = 80) = 0.01 - \frac{1.6^2}{400} = 0.01 - 0.0064 = 0.0036.$$

$$\text{SD}(Y \mid X = 80) = \sqrt{0.0036} = 0.06.$$

For a borrower earning 80K, the expected DTI is 38% with standard deviation 6%.

```python
import numpy as np
from scipy import stats

mu_X, mu_Y = 60, 0.30
var_X, var_Y = 400, 0.01
cov_XY = 1.6

x_given = 80
cond_mean = mu_Y + cov_XY / var_X * (x_given - mu_X)
cond_var = var_Y - cov_XY**2 / var_X
cond_sd = np.sqrt(cond_var)

print(f"E[Y | X={x_given}] = {cond_mean:.4f}")
print(f"SD[Y | X={x_given}] = {cond_sd:.4f}")
print(f"95% CI: [{cond_mean - 1.96*cond_sd:.4f}, {cond_mean + 1.96*cond_sd:.4f}]")
```

### Case 2: Portfolio Risk with Two Assets

A portfolio invests 60% in stock A and 40% in stock B. Annual returns are modeled as bivariate normal:

- $\mu_A = 0.10$, $\sigma_A = 0.20$
- $\mu_B = 0.15$, $\sigma_B = 0.30$
- $\rho_{AB} = 0.30$

Portfolio return: $R_p = 0.60 R_A + 0.40 R_B$

$$E[R_p] = 0.60 \times 0.10 + 0.40 \times 0.15 = 0.12$$

$$\sigma_p^2 = 0.60^2 \times 0.20^2 + 0.40^2 \times 0.30^2 + 2 \times 0.60 \times 0.40 \times 0.30 \times 0.20 \times 0.30$$

$$= 0.0144 + 0.0144 + 0.00864 = 0.03744$$

$$\sigma_p = \sqrt{0.03744} \approx 0.1935$$

Value at Risk (VaR) at 95% confidence: $\text{VaR}_{0.95} = \mu_p - 1.645 \sigma_p = 0.12 - 1.645 \times 0.1935 = -0.1983$.

There is a 5% chance the portfolio loses more than 19.83% in a year.

```python
import numpy as np
from scipy import stats

mu = np.array([0.10, 0.15])
sigma = np.array([0.20, 0.30])
rho = 0.30
w = np.array([0.60, 0.40])

cov_matrix = np.array([
    [sigma[0]**2, rho * sigma[0] * sigma[1]],
    [rho * sigma[0] * sigma[1], sigma[1]**2]
])

port_mu = w @ mu
port_var = w @ cov_matrix @ w
port_sigma = np.sqrt(port_var)

VaR_95 = port_mu - 1.645 * port_sigma
CVaR_95 = port_mu - port_sigma * stats.norm.pdf(stats.norm.ppf(0.05)) / 0.05

print(f"Portfolio expected return: {port_mu:.4f}")
print(f"Portfolio volatility:       {port_sigma:.4f}")
print(f"VaR (95%):                 {VaR_95:.4f}")
print(f"CVaR (95%):                {CVaR_95:.4f}")
```

### Case 3: Conditional Expectation in A/B Testing

In an A/B test, the conversion indicator $Y$ is Bernoulli with parameter depending on group $T$:

$$E[Y \mid T] = \begin{cases} p_{\text{control}}, & T = 0 \\ p_{\text{treatment}}, & T = 1 \end{cases}$$

Using the Law of Total Expectation, the overall conversion rate is:

$$E[Y] = E[E[Y \mid T]] = P(T=0) p_{\text{control}} + P(T=1) p_{\text{treatment}}.$$

If 50% of users are in each group and $p_{\text{control}} = 0.05$, $p_{\text{treatment}} = 0.06$:

$$E[Y] = 0.5 \times 0.05 + 0.5 \times 0.06 = 0.055.$$

The Law of Total Variance decomposes the variance:

$$\text{Var}(Y) = \underbrace{E[\text{Var}(Y \mid T)]}_{\text{within-group}} + \underbrace{\text{Var}(E[Y \mid T])}_{\text{between-group}}.$$

Between-group variance: $\text{Var}(E[Y \mid T]) = 0.5 \times (0.05 - 0.055)^2 + 0.5 \times (0.06 - 0.055)^2 = 0.000025$.

The $R^2$ of the treatment effect is $0.000025 / (0.055 \times 0.945) \approx 0.00048$ — only 0.048% of total variance is explained by the treatment.

## Examples

### Example 1: Computing Covariance from Joint PMF

Given joint PMF of $X$ and $Y$:

| | Y=0 | Y=1 |
|---|---|---|
| X=0 | 0.2 | 0.3 |
| X=1 | 0.1 | 0.4 |

Marginals:
- $P(X=0) = 0.5$, $P(X=1) = 0.5$
- $P(Y=0) = 0.3$, $P(Y=1) = 0.7$
- $E[X] = 0.5$, $E[Y] = 0.7$, $E[XY] = 0 \times 0 \times 0.2 + 0 \times 1 \times 0.3 + 1 \times 0 \times 0.1 + 1 \times 1 \times 0.4 = 0.4$

$$\text{Cov}(X, Y) = 0.4 - 0.5 \times 0.7 = 0.4 - 0.35 = 0.05.$$

$$\rho = \frac{0.05}{\sqrt{0.5 \times 0.5} \times \sqrt{0.3 \times 0.7}} = \frac{0.05}{0.5 \times 0.458} \approx 0.218.$$

### Example 2: Independence Does Not Imply Zero Covariance Is Sufficient

If $\text{Cov}(X, Y) = 0$, we say $X$ and $Y$ are **uncorrelated**. Independence implies uncorrelated, but the converse does not hold.

Counterexample: Let $X \sim \text{Uniform}(-1, 1)$ and $Y = X^2$.

$$\text{Cov}(X, Y) = E[X^3] - E[X] E[X^2] = 0 - 0 \times E[X^2] = 0.$$

But $X$ and $Y$ are clearly dependent ($Y$ is determined by $X$). Correlation only captures linear dependence.

```python
import numpy as np

np.random.seed(42)
X = np.random.uniform(-1, 1, 10000)
Y = X**2

print(f"Correlation: {np.corrcoef(X, Y)[0, 1]:.4f}")
# Despite perfect dependence, correlation is ~0
```

### Example 3: Conditional Expectation of Bivariate Normal

For $(X, Y)$ bivariate normal:

$$E[Y \mid X = x] = \mu_Y + \rho \frac{\sigma_Y}{\sigma_X} (x - \mu_X).$$

This is exactly the simple linear regression line. The conditional variance is:

$$\text{Var}(Y \mid X = x) = \sigma_Y^2 (1 - \rho^2).$$

The conditional variance does not depend on $x$ — this is the **homoscedasticity** property of the bivariate normal.

```python
# Bivariate normal conditional expectation
mu_X, mu_Y = 50, 100
sigma_X, sigma_Y = 10, 20
rho = 0.6

x_values = [30, 50, 70]
for x in x_values:
    cond_mean = mu_Y + rho * sigma_Y / sigma_X * (x - mu_X)
    cond_sd = sigma_Y * np.sqrt(1 - rho**2)
    print(f"E[Y|X={x}] = {cond_mean:.1f}, SD = {cond_sd:.1f}")
```

### Example 4: Variance of a Sum of Correlated Variables

If $X$ and $Y$ are not independent, the variance of their sum is not simply the sum of variances.

$$\text{Var}(X + Y) = \text{Var}(X) + \text{Var}(Y) + 2\text{Cov}(X, Y).$$

If $X$ and $Y$ are positively correlated, the variance of the sum is larger than if they were independent. For a portfolio, this is the diversification effect — negative correlation reduces portfolio variance.

```python
import numpy as np

# Effect of correlation on sum variance
sigma_X, sigma_Y = 1.0, 1.0
for rho in [-0.9, -0.5, 0.0, 0.5, 0.9]:
    var_sum = sigma_X**2 + sigma_Y**2 + 2 * rho * sigma_X * sigma_Y
    print(f"rho={rho:4.1f}: Var(X+Y)={var_sum:.2f}, SD={np.sqrt(var_sum):.2f}")
```

### Example 5: Law of Total Expectation for Algorithm Performance

An ML model's error depends on which data distribution it encounters. In production, 70% of traffic is "easy" (error rate 2%) and 30% is "hard" (error rate 15%).

Overall expected error rate:

$$E[\text{error}] = E[E[\text{error} \mid \text{difficulty}]] = 0.70 \times 0.02 + 0.30 \times 0.15 = 0.059 = 5.9\%.$$

The between-group variance of error:

$$E[\text{error} \mid \text{easy}] = 0.02, \quad E[\text{error} \mid \text{hard}] = 0.15$$
$$\text{Var}(E[\text{error} \mid \text{difficulty}]) = 0.7 \times (0.02 - 0.059)^2 + 0.3 \times (0.15 - 0.059)^2 \approx 0.00356.$$

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

## Quick References

- [Elements of Statistical Learning, Hastie et al.](https://hastie.su.domains/ElemStatLearn/) — joint distributions in predictive modeling
- [Multivariate Normal Distribution (Wikipedia)](https://en.wikipedia.org/wiki/Multivariate_normal_distribution) — properties and formulas
- [Law of Total Expectation (Wikipedia)](https://en.wikipedia.org/wiki/Law_of_total_expectation) — statement and proof
- [Copula (Wikipedia)](https://en.wikipedia.org/wiki/Copula_(probability_theory)) — dependence modeling beyond correlation

## Next Steps

- [Limit Theorems](limit-theorems.md) — LLN, CLT, and their implications
