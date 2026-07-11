# Mutual Information

## Description

Mutual information measures the amount of information one random variable contains about another. It generalizes correlation to arbitrary dependencies and is the foundation of channel capacity, feature selection algorithms, and the information bottleneck method in machine learning.

## Prerequisites

- [Entropy and Information](entropy-and-information.md) — entropy, joint entropy, conditional entropy, KL divergence
- [Probability Foundations](../probability-and-statistics/probability-foundations.md) — random variables, joint distributions, expectation

## Table of Contents

- [Definition of Mutual Information](#definition-of-mutual-information)
- [Relationship to Entropy](#relationship-to-entropy)
- [Venn Diagram Interpretation](#venn-diagram-interpretation)
- [Properties of Mutual Information](#properties-of-mutual-information)
- [Conditional Mutual Information](#conditional-mutual-information)
- [Chain Rule for Mutual Information](#chain-rule-for-mutual-information)
- [Data Processing Inequality](#data-processing-inequality)
- [Fano's Inequality](#fanos-inequality)
- [Mutual Information for Continuous Variables](#mutual-information-for-continuous-variables)
- [Estimation of Mutual Information](#estimation-of-mutual-information)
- [Applications in Computing](#applications-in-computing)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Definition of Mutual Information

Mutual information $I(X; Y)$ measures the reduction in uncertainty about $X$ given knowledge of $Y$, or equivalently, the amount of information that $X$ and $Y$ share:

$$
I(X; Y) = \sum_{x \in \mathcal{X}} \sum_{y \in \mathcal{Y}} p(x, y) \log_2 \frac{p(x, y)}{p(x) p(y)}
$$

When $X$ and $Y$ are independent, $p(x, y) = p(x) p(y)$, so the ratio is 1 and $I(X; Y) = 0$. When $X$ and $Y$ are perfectly dependent, $I(X; Y)$ equals the entropy of either variable.

Mutual information is the **KL divergence** between the joint distribution and the product of the marginals:

$$
I(X; Y) = D_{KL}(p(x, y) \parallel p(x) p(y))
$$

This makes mutual information a measure of how far the joint distribution is from independence.

### Relationship to Entropy

Mutual information connects to entropy through several equivalent expressions:

$$
\begin{aligned}
I(X; Y) &= H(X) - H(X | Y) \\
&= H(Y) - H(Y | X) \\
&= H(X) + H(Y) - H(X, Y) \\
&= H(X, Y) - H(X | Y) - H(Y | X)
\end{aligned}
$$

**Intuition:** $I(X; Y)$ is the reduction in uncertainty about $X$ after observing $Y$. If $Y$ perfectly predicts $X$, then $H(X | Y) = 0$ and $I(X; Y) = H(X)$. If $Y$ gives no information about $X$, then $H(X | Y) = H(X)$ and $I(X; Y) = 0$.

The relationship is always:

$$
0 \leq I(X; Y) \leq \min(H(X), H(Y))
$$

### Venn Diagram Interpretation

Visualize the relationship between entropies as a Venn-like diagram:

```
    +-------------------+
    |   H(X,Y)          |
    |  +-------+------+ |
    |  | H(X|Y)| I    | |
    |  |       |(X;Y) | |
    |  +-------+------+ |
    |          | H(Y|X)| |
    |          +-------+ |
    +--------------------+
```

- $H(X, Y)$: the entire rectangle (total uncertainty of the pair)
- $I(X; Y)$: the overlapping region (shared information)
- $H(X | Y)$: the part of $X$'s entropy not explained by $Y$
- $H(Y | X)$: the part of $Y$'s entropy not explained by $X$

This diagram makes the identities intuitive:

$$
H(X, Y) = H(X | Y) + I(X; Y) + H(Y | X)
$$

### Properties of Mutual Information

**Non-negativity:** $I(X; Y) \geq 0$ with equality iff $X$ and $Y$ are independent.

This follows from the non-negativity of KL divergence: $D_{KL}(p(x,y) \parallel p(x)p(y)) \geq 0$.

**Symmetry:** $I(X; Y) = I(Y; X)$.

Mutual information is symmetric in its arguments, unlike conditional entropy which is not.

**Relationship to correlation for Gaussian variables:**

For jointly Gaussian $(X, Y)$ with correlation coefficient $\rho$:

$$
I(X; Y) = -\frac{1}{2} \log_2(1 - \rho^2)
$$

Unlike Pearson correlation, mutual information captures all forms of dependence, not just linear.

**Additivity for independent pairs:** If $(X_1, Y_1)$ and $(X_2, Y_2)$ are independent, then:

$$
I(X_1, X_2; Y_1, Y_2) = I(X_1; Y_1) + I(X_2; Y_2)
$$

**Invariance under bijections:** For invertible functions $f$ and $g$:

$$
I(f(X); g(Y)) = I(X; Y)
```

Mutual information is unaffected by one-to-one transformations. This is why it is used in feature selection — scaling a feature does not change its mutual information with the target.

### Conditional Mutual Information

Conditional mutual information measures the shared information between $X$ and $Y$ given knowledge of $Z$:

$$
I(X; Y | Z) = \sum_{z \in \mathcal{Z}} p(z) \sum_{x, y} p(x, y | z) \log_2 \frac{p(x, y | z)}{p(x | z) p(y | z)}
$$

Equivalently:

$$
I(X; Y | Z) = H(X | Z) - H(X | Y, Z)
$$

**Interpretation:** $I(X; Y | Z)$ is the information shared between $X$ and $Y$ that is not already explained by $Z$. If $X$ and $Y$ become independent when conditioning on $Z$, then $I(X; Y | Z) = 0$.

Conditional mutual information satisfies:

$$
0 \leq I(X; Y | Z) \leq H(X | Z)
$$

It can be larger or smaller than $I(X; Y)$. For example, Simpson's paradox can cause $I(X; Y | Z) > I(X; Y)$ when a confounding variable hides the true relationship.

### Chain Rule for Mutual Information

The chain rule decomposes the mutual information between a set of variables and a target:

$$
I(X_1, X_2, \dots, X_n; Y) = \sum_{i=1}^n I(X_i; Y | X_{i-1}, \dots, X_1)
$$

This is directly analogous to the chain rule for entropy. It measures the total information that the set $\{X_i\}$ provides about $Y$ as a sum of incremental contributions.

The chain rule is fundamental to sequential feature selection and decision tree algorithms that greedily choose the next best feature.

**Chain rule for conditional mutual information:**

$$
I(X_1, \dots, X_n; Y | Z) = \sum_{i=1}^n I(X_i; Y | X_{i-1}, \dots, X_1, Z)
$$

### Data Processing Inequality

If $X \to Y \to Z$ forms a Markov chain (meaning $p(x, z | y) = p(x | y) p(z | y)$), then:

$$
I(X; Y) \geq I(X; Z)
$$

**Interpretation:** Processing $Y$ to produce $Z$ cannot increase the information about $X$. Data processing can only destroy or preserve information, never create it.

The inequality chain:

$$
I(X; Y) \geq I(X; Z) \geq I(X; W) \quad \text{for} \quad X \to Y \to Z \to W
$$

**Consequences:**

- No amount of post-processing of a dataset can extract information that is not already present in the raw data.
- In supervised learning, the learned model $Z$ cannot contain more information about the label $X$ than the training data $Y$ does.
- Feature engineering is bounded by the information in the original features.

The data processing inequality is the reason why **encryption** works: an encrypted message $Y$ has $I(\text{plaintext}; \text{ciphertext}) = 0$ for an attacker without the key, but $I(\text{plaintext}; \text{ciphertext}) = H(\text{plaintext})$ for someone with the key. The inequality does not preclude adding side information (the key).

**Sufficient statistic:** A statistic $T(X)$ is sufficient for parameter $\theta$ if $I(\theta; X) = I(\theta; T(X))$, meaning $T$ preserves all information about $\theta$. The data processing inequality guarantees that no further processing can increase this information.

### Fano's Inequality

Fano's inequality relates the probability of error in estimating $X$ from $Y$ to the conditional entropy $H(X | Y)$:

$$
H(X | Y) \leq H(e) + P_e \log_2(|\mathcal{X}| - 1)
$$

where $P_e = P(\hat{X} \neq X)$ is the probability of error, $H(e) = -P_e \log_2 P_e - (1 - P_e) \log_2(1 - P_e)$ is the binary entropy of the error, and $|\mathcal{X}|$ is the alphabet size.

A simpler bound:

$$
P_e \geq \frac{H(X | Y) - 1}{\log_2 |\mathcal{X}|}
$$

**Interpretation:** If $Y$ provides little information about $X$ (high $H(X | Y)$), then any estimator must have high error probability. Fano's inequality provides a fundamental lower bound on error in classification, estimation, and hypothesis testing.

**Application to ML:** In $k$-class classification with $H(Y | X)$ being the conditional entropy of labels given features:

$$
P_e \geq \frac{H(Y | X) - 1}{\log_2 k}
$$

This gives a theoretical lower bound on the classifier's error rate based on the information in the features.

### Mutual Information for Continuous Variables

For continuous random variables, mutual information generalizes using differential entropy:

$$
I(X; Y) = \iint p(x, y) \log_2 \frac{p(x, y)}{p(x) p(y)} \, dx \, dy = h(X) - h(X | Y) = h(X) + h(Y) - h(X, Y)
$$

where $h(X)$ is differential entropy (continuous entropy).

For the multivariate Gaussian $\mathcal{N}(\boldsymbol{\mu}, \Sigma)$, mutual information between two subsets of variables has a closed form:

$$
I(X; Y) = \frac{1}{2} \log_2 \frac{|\Sigma_{XX}| |\Sigma_{YY}|}{|\Sigma|}
$$

where $\Sigma$ is the full covariance matrix and $|\cdot|$ denotes the determinant.

### Estimation of Mutual Information

In practice, the true distribution is unknown and must be estimated from data:

**k-nearest neighbors method (Kraskov-Stogbauer-Grassberger):**

```python
import numpy as np
from sklearn.feature_selection import mutual_info_regression
from sklearn.neighbors import NearestNeighbors

def mutual_information_kraskov(x, y, k=3):
    """Estimate I(X; Y) using the KSG estimator."""
    n = len(x)
    points = np.column_stack([x, y])

    # Find k-th nearest neighbor distances in joint space
    nn_joint = NearestNeighbors(n_neighbors=k+1).fit(points)
    distances, _ = nn_joint.kneighbors(points)
    epsilon = distances[:, k]

    # Count neighbors in marginal spaces within epsilon
    mi = 0
    for i in range(n):
        nx = np.sum(np.abs(x - x[i]) < epsilon[i]) - 1
        ny = np.sum(np.abs(y - y[i]) < epsilon[i]) - 1
        mi += np.digamma(k) + np.digamma(n) - np.digamma(nx + 1) - np.digamma(ny + 1)
    return mi / n

x = np.random.randn(500)
y = x + 0.5 * np.random.randn(500)
print(f"KSG MI: {mutual_information_kraskov(x, y):.4f} nats")
```

**Binning-based estimation:**

```python
def mutual_information_discrete(x, y, bins=10):
    """Estimate MI using histogram binning."""
    c_xy = np.histogram2d(x, y, bins=bins)[0]
    c_xy = c_xy / c_xy.sum()
    c_x = c_xy.sum(axis=1)
    c_y = c_xy.sum(axis=0)
    mi = 0
    for i in range(bins):
        for j in range(bins):
            if c_xy[i, j] > 0:
                mi += c_xy[i, j] * np.log(c_xy[i, j] / (c_x[i] * c_y[j]))
    return mi
```

### Applications in Computing

**Feature selection.** Mutual information ranks features by relevance to the target. Unlike correlation, it captures non-linear dependencies:

```python
from sklearn.feature_selection import mutual_info_classif
import numpy as np

X = np.random.randn(200, 10)
y = (X[:, 0]**2 + 0.3 * X[:, 1] + 0.1 * np.random.randn(200) > 0.5).astype(int)

mi_scores = mutual_info_classif(X, y, random_state=42)
for i, score in enumerate(mi_scores):
    print(f"Feature {i}: MI = {score:.4f}")
```

**Decision tree splitting (ID3, C4.5).** The information gain criterion for splitting:

$$
IG(T, A) = I(T; A) = H(T) - H(T | A)
$$

C4.5 uses the gain ratio to normalize for feature cardinality:

$$
GR(T, A) = \frac{IG(T, A)}{H(A)}
$$

```python
def gain_ratio(y, X_feature):
    ig = information_gain(y, X_feature)
    split_info = entropy(X_feature)
    return ig / split_info if split_info > 0 else 0
```

**Information bottleneck.** The information bottleneck method finds a compressed representation $Z$ of $X$ that preserves information about $Y$:

$$
\min_{p(z|x)} I(X; Z) - \beta I(Z; Y)
```

This is used for clustering, representation learning, and explaining deep neural networks.

**Channel capacity.** The capacity of a communication channel is defined as the maximum mutual information over all input distributions:

$$
C = \max_{p(x)} I(X; Y)
$$

This is the central quantity in Shannon's noisy channel coding theorem.

**Granger causality.** In time series analysis, conditional mutual information $I(X_t; Y_{<t} | X_{<t})$ measures whether $Y$ Granger-causes $X$ by quantifying the additional information past $Y$ provides about current $X$ beyond past $X$.

### Interaction Information and Higher-Order Dependencies

Interaction information extends mutual information to three or more variables:

$$
I(X; Y; Z) = I(X; Y) - I(X; Y | Z) = I(X; Z) - I(X; Z | Y) = I(Y; Z) - I(Y; Z | X)
$$

**Interpretation:**
- $I(X; Y; Z) > 0$ indicates **redundancy** — the information that $X$ and $Y$ share about $Z$ is partially overlapping. Knowing $Z$ reduces the mutual information between $X$ and $Y$.
- $I(X; Y; Z) < 0$ indicates **synergy** — $X$ and $Y$ together provide more information about $Z$ than either alone. The mutual information between $X$ and $Y$ increases when conditioning on $Z$.

**Example — XOR gate:** For binary variables $X, Y, Z$ where $Z = X \oplus Y$ (XOR), each pair is independent but the triple is dependent. Here $I(X; Y) = 0$ (independence), but $I(X; Y | Z) = 1$ (knowing $Z$ makes $X$ and $Y$ dependent). This gives $I(X; Y; Z) = -1$, indicating pure synergy.

**Partial information decomposition (PID):** PID decomposes the information that two source variables $X_1, X_2$ provide about a target $Y$ into four components:

1. **Unique information** from $X_1$ — information only $X_1$ provides about $Y$
2. **Unique information** from $X_2$ — information only $X_2$ provides about $Y$
3. **Redundant information** — information both $X_1$ and $X_2$ provide about $Y$
4. **Synergistic information** — information that emerges from the combination of $X_1$ and $X_2$

PID has applications in neuroscience (analyzing how different brain regions encode information), feature selection (detecting redundant vs. synergistic feature interactions), and ensemble learning (understanding how different models complement each other).

### Practical Estimation of Mutual Information

Estimating mutual information from finite samples requires careful consideration of bias and variance:

**k-Nearest Neighbors (KSG) estimator:** The Kraskov-Stogbauer-Grassberger estimator adapts the neighborhood size based on local density, reducing bias compared to fixed-bin methods:

```python
import numpy as np
from sklearn.neighbors import NearestNeighbors

def kraskov_mi(x, y, k=5):
    """KSG mutual information estimator."""
    n = len(x)
    points = np.column_stack([x, y])

    # Joint space distances
    nn_joint = NearestNeighbors(n_neighbors=k+1).fit(points)
    d, _ = nn_joint.kneighbors(points)
    eps = d[:, k]

    # Count neighbors in marginal spaces
    mi = 0
    for i in range(n):
        nx = np.sum(np.abs(x - x[i]) < eps[i]) - 1
        ny = np.sum(np.abs(y - y[i]) < eps[i]) - 1
        mi += np.digamma(k) + np.digamma(n) - np.digamma(nx + 1) - np.digamma(ny + 1)
    return mi / n

np.random.seed(42)
x = np.random.randn(1000)
y = x + 0.3 * np.random.randn(1000)
print(f"KSG MI (k=5): {kraskov_mi(x, y):.4f} nats")
```

**Bias correction:** The KSG estimator has negative bias for small samples ($n < 100$). Shuffle the data to estimate and subtract the baseline:

```python
def bias_corrected_mi(x, y, k=5, n_shuffles=50):
    """Bias-corrected MI using shuffled baselines."""
    mi_observed = kraskov_mi(x, y, k)
    mi_shuffled = np.array([
        kraskov_mi(x, np.random.permutation(y), k)
        for _ in range(n_shuffles)
    ])
    return mi_observed - np.mean(mi_shuffled)

mi_corrected = bias_corrected_mi(x, y)
print(f"Bias-corrected MI: {mi_corrected:.4f} nats")
```

**Gaussian approximation:** For approximately Gaussian data, the closed-form formula provides a fast estimate:

```python
def gaussian_mi(x, y):
    """MI for approximately Gaussian data using correlation."""
    r = np.corrcoef(x, y)[0, 1]
    return -0.5 * np.log2(1 - r**2)

print(f"Gaussian approximation: {gaussian_mi(x, y):.4f} bits")
```

**Choosing the right estimator:**
- **Discrete data** with small alphabets: direct plug-in estimator with bias correction (Miller-Madow)
- **Continuous data** with simple dependencies: Gaussian approximation
- **Continuous data** with complex dependencies: KSG estimator ($k = 3$ to $k = 10$)
- **High-dimensional data:** Neural estimation (MINE — Mutual Information Neural Estimation)

## Glossary

| Term | Definition |
|---|---|
| Mutual information | Measure of dependence between two random variables, $I(X;Y) = D_{KL}(p(x,y) \parallel p(x)p(y))$ |
| Conditional mutual information | Mutual information of $X$ and $Y$ given knowledge of $Z$ |
| Data processing inequality | Post-processing cannot increase information content |
| Fano's inequality | Lower bound on error probability given conditional entropy |
| Information bottleneck | Framework for finding minimal sufficient representations |
| Markov chain | Sequence $X \to Y \to Z$ where $X$ and $Z$ are conditionally independent given $Y$ |
| Sufficient statistic | Statistic that preserves all information about a parameter |
| Information gain | Reduction in entropy from knowing a feature, equivalent to mutual information |
| Gain ratio | Information gain normalized by feature entropy (C4.5) |
| Symmetry | Property $I(X;Y) = I(Y;X)$ |
| Kraskov estimator | Nearest-neighbor based MI estimator (KSG) |
| Channel capacity | Maximum mutual information over all input distributions |

## Quick References

- [Cover, T. M. & Thomas, J. A. (2006). Elements of Information Theory](https://www.wiley.com/en-us/Elements+of+Information+Theory%2C+2nd+Edition-p-9780471241959) — chapters on mutual information and the data processing inequality
- [Kraskov, A., Stogbauer, H., & Grassberger, P. (2004). Estimating Mutual Information](https://arxiv.org/abs/cond-mat/0305641) — the KSG estimator paper
- [Tishby, N., Pereira, F. C., & Bialek, W. (2000). The Information Bottleneck Method](https://arxiv.org/abs/physics/0004057) — foundational paper on the information bottleneck
- [Wikipedia: Mutual Information](https://en.wikipedia.org/wiki/Mutual_information) — comprehensive reference with formulas and properties

## Next Steps

- [Channel Capacity](channel-capacity.md) — maximum rate of reliable communication over noisy channels, defined as $\max_{p(x)} I(X;Y)$
- [Source Coding](source-coding.md) — lossless compression algorithms that achieve entropy bounds
- [Rate-Distortion](rate-distortion.md) — lossy compression framework, trade-off between rate and distortion
