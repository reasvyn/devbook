# Entropy and Information

## Description

Shannon entropy quantifies the average information content of a random source, measured in bits. It defines the fundamental limit of lossless data compression and serves as the foundation for mutual information, KL divergence, and cross-entropy — concepts widely used in machine learning loss functions, decision tree splitting criteria, and communication system design.

## Prerequisites

- [Probability Foundations](../probability-and-statistics/probability-foundations.md) — random variables, PMF, PDF, expectation, logarithms
- [Introduction to Information Theory](intro/index.md) — philosophical background and what information means

## Table of Contents

- [Shannon Entropy](#shannon-entropy)
- [Interpretation as Uncertainty](#interpretation-as-uncertainty)
- [Bits and Information Measurement](#bits-and-information-measurement)
- [Joint Entropy](#joint-entropy)
- [Conditional Entropy](#conditional-entropy)
- [Chain Rule for Entropy](#chain-rule-for-entropy)
- [Relative Entropy (KL Divergence)](#relative-entropy-kl-divergence)
- [Cross-Entropy](#cross-entropy)
- [Properties of Entropy](#properties-of-entropy)
- [Entropy and Coding](#entropy-and-coding)
- [Applications in Computing](#applications-in-computing)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Shannon Entropy

Claude Shannon, in his 1948 paper "A Mathematical Theory of Communication," defined the entropy of a discrete random variable $X$ with probability mass function $p(x) = P(X = x)$ as:

$$
H(X) = -\sum_{x \in \mathcal{X}} p(x) \log_2 p(x) = \sum_{x \in \mathcal{X}} p(x) \log_2 \frac{1}{p(x)}
$$

where $\mathcal{X}$ is the alphabet of $X$. Entropy is measured in **bits** when the logarithm is base 2, and in **nats** when the logarithm is natural. In information theory, base 2 is standard.

The quantity $\log_2 \frac{1}{p(x)}$ is called the **self-information** or **surprisal** of outcome $x$. Rare events have high self-information; certain events have zero self-information. Entropy is the expected value of self-information over the distribution.

For example, let $X$ be a fair coin flip:

$$
H(X) = -\frac{1}{2} \log_2 \frac{1}{2} - \frac{1}{2} \log_2 \frac{1}{2} = 1 \text{ bit}
$$

A fair coin has exactly 1 bit of entropy. A biased coin with $p = 0.9$ for heads:

$$
H(X) = -0.9 \log_2 0.9 - 0.1 \log_2 0.1 \approx 0.469 \text{ bits}
$$

A biased coin has less entropy because outcomes are more predictable.

A deterministic variable (e.g., a coin that always lands heads) has $H(X) = 0$ bits — there is no uncertainty.

### Interpretation as Uncertainty

Entropy directly measures the **average uncertainty** about the outcome of a random variable. High entropy means high uncertainty; low entropy means low uncertainty.

Consider the following distributions and their entropies:

| Distribution | Probabilities | Entropy (bits) |
|---|---|---|
| Deterministic | $[1.0, 0.0]$ | $0$ |
| Highly biased | $[0.9, 0.1]$ | $0.469$ |
| Moderately biased | $[0.75, 0.25]$ | $0.811$ |
| Fair | $[0.5, 0.5]$ | $1.0$ |

As the distribution becomes more uniform, uncertainty increases and entropy rises. The maximum entropy for a distribution over $n$ outcomes occurs when all outcomes are equally likely:

$$
H_{\max} = \log_2 n \text{ bits}
$$

This principle — **maximum entropy** — is used to derive default distributions in Bayesian statistics and physics.

Entropy also measures the **minimum number of bits needed to encode a random variable** on average. If you need to transmit the outcomes of $X$ over a channel, you cannot compress them below $H(X)$ bits per symbol without information loss.

### Bits and Information Measurement

The bit (binary digit) is the fundamental unit of information. One bit distinguishes between two equally likely possibilities. More precisely, receiving the outcome of a random variable reduces uncertainty by $H(X)$ bits.

**Self-information** measures the information gained from observing a specific outcome:

$$
I(x) = \log_2 \frac{1}{p(x)} = -\log_2 p(x)
$$

| Probability | Self-information | Interpretation |
|---|---|---|
| $1/2$ | $1$ bit | One yes/no question |
| $1/4$ | $2$ bits | Two yes/no questions |
| $1/8$ | $3$ bits | Three yes/no questions |
| $1/1024$ | $10$ bits | Ten yes/no questions |
| $1$ | $0$ bits | No information gained |

This logarithmic relationship is intuitive: if you know a rare event occurred, you gain more information. The logarithm makes information additive for independent events:

$$
I(x, y) = \log_2 \frac{1}{p(x)p(y)} = \log_2 \frac{1}{p(x)} + \log_2 \frac{1}{p(y)} = I(x) + I(y)
$$

### Joint Entropy

For a pair of random variables $(X, Y)$ with joint distribution $p(x, y)$, the joint entropy is:

$$
H(X, Y) = -\sum_{x \in \mathcal{X}} \sum_{y \in \mathcal{Y}} p(x, y) \log_2 p(x, y)
$$

Joint entropy measures the total uncertainty of the pair. If $X$ and $Y$ are independent:

$$
H(X, Y) = H(X) + H(Y)
$$

If they are dependent, $H(X, Y) < H(X) + H(Y)$ because some information is shared.

For example, let $X$ be a fair coin and $Y$ be the same coin (completely dependent). Then $H(X, Y) = H(X) = 1$ bit, not $2$ bits.

### Conditional Entropy

Conditional entropy measures the remaining uncertainty about $X$ given that we know $Y$:

$$
H(X | Y) = -\sum_{x, y} p(x, y) \log_2 p(x | y) = \sum_y p(y) H(X | Y = y)
$$

Key identity:

$$
H(X, Y) = H(X) + H(Y | X) = H(Y) + H(X | Y)
$$

The uncertainty of the pair equals the uncertainty of one variable plus the remaining uncertainty of the other given the first.

If $X$ and $Y$ are independent, $H(X | Y) = H(X)$ — knowing $Y$ gives no information about $X$.

If $Y$ is a deterministic function of $X$, then $H(Y | X) = 0$.

Conditional entropy is always non-negative and satisfies:

$$
0 \leq H(Y | X) \leq H(Y)
$$

### Chain Rule for Entropy

The chain rule extends to multiple variables:

$$
H(X_1, X_2, \dots, X_n) = \sum_{i=1}^n H(X_i | X_{i-1}, \dots, X_1)
$$

This decomposes the joint entropy of $n$ variables into a sum of conditional entropies. Each term $H(X_i | X_{i-1}, \dots, X_1)$ measures the new information contributed by $X_i$ after seeing all previous variables.

For independent identically distributed (i.i.d.) variables:

$$
H(X_1, \dots, X_n) = n H(X)
$$

The chain rule is critical for analyzing sequential data, Markov chains, and time series.

### Relative Entropy (KL Divergence)

Kullback-Leibler (KL) divergence measures how one probability distribution $P$ diverges from a reference distribution $Q$:

$$
D_{KL}(P \parallel Q) = \sum_{x \in \mathcal{X}} p(x) \log_2 \frac{p(x)}{q(x)}
$$

KL divergence has these properties:

- **Non-negativity:** $D_{KL}(P \parallel Q) \geq 0$, with equality iff $P = Q$ almost everywhere
- **Not symmetric:** $D_{KL}(P \parallel Q) \neq D_{KL}(Q \parallel P)$ in general
- **Not a metric:** does not satisfy the triangle inequality

KL divergence measures the **information loss** when using $Q$ to approximate $P$. It is the expected log-likelihood ratio.

For continuous distributions, the integral form:

$$
D_{KL}(P \parallel Q) = \int_{-\infty}^{\infty} p(x) \log_2 \frac{p(x)}{q(x)} \, dx
$$

**Example:** Let $P \sim \text{Bernoulli}(0.5)$ and $Q \sim \text{Bernoulli}(0.9)$:

$$
D_{KL}(P \parallel Q) = 0.5 \log_2 \frac{0.5}{0.9} + 0.5 \log_2 \frac{0.5}{0.1} \approx 0.5 \cdot (-0.848) + 0.5 \cdot 2.322 \approx 0.737 \text{ bits}
$$

KL divergence is ubiquitous in machine learning. Variational autoencoders minimize KL divergence between the approximate posterior and the true posterior. The information bottleneck method uses KL divergence to find optimal representations.

### Cross-Entropy

Cross-entropy between two distributions $P$ and $Q$ over the same alphabet is:

$$
H(P, Q) = -\sum_{x} p(x) \log_2 q(x)
$$

Cross-entropy relates to entropy and KL divergence:

$$
H(P, Q) = H(P) + D_{KL}(P \parallel Q)
$$

Since $H(P)$ is fixed for the true distribution, minimizing cross-entropy is equivalent to minimizing KL divergence.

In machine learning, cross-entropy is the default loss function for multi-class classification. For a true distribution $y$ (one-hot encoded) and predicted probabilities $\hat{y}$:

$$
\mathcal{L}(y, \hat{y}) = -\sum_{i} y_i \log \hat{y}_i
$$

Cross-entropy loss penalizes confident wrong predictions heavily. If the true class is $i$ and $\hat{y}_i = 0.01$, the loss contribution is $-\log(0.01) \approx 4.605$ nats.

### Properties of Entropy

**Non-negativity:** $H(X) \geq 0$ for all discrete random variables. (Continuous differential entropy can be negative.)

**Maximum entropy:** For a given alphabet size $n$, $H(X) \leq \log_2 n$, achieved by the uniform distribution.

**Concavity:** $H(p)$ is a concave function of $p$. This means mixing distributions increases entropy:

$$
H(\lambda p + (1 - \lambda) q) \geq \lambda H(p) + (1 - \lambda) H(q)
$$

**Additivity for independent variables:** If $X$ and $Y$ are independent, $H(X, Y) = H(X) + H(Y)$.

**Invariance under bijection:** If $f$ is a bijective function, then $H(f(X)) = H(X)$. A one-to-one mapping does not change entropy.

**Conditioning reduces entropy:** $H(X | Y) \leq H(X)$, with equality iff $X$ and $Y$ are independent.

**Data processing inequality:** If $X \to Y \to Z$ forms a Markov chain, then $I(X; Y) \geq I(X; Z)$ and $H(X) \geq I(X; Y) \geq I(X; Z)$.

### Entropy and Coding

The **source coding theorem** (Shannon's first theorem) states that the expected length of an optimal lossless code for a random variable $X$ satisfies:

$$
H(X) \leq \mathbb{E}[L] < H(X) + 1
$$

where $L$ is the codeword length. This is the fundamental limit — no lossless compression scheme can compress below $H(X)$ bits per symbol on average.

The redundancy of a code is $\mathbb{E}[L] - H(X)$. Huffman coding achieves optimal symbol-by-symbol coding with redundancy at most 1 bit per symbol.

For block coding of $n$ i.i.d. symbols, the average code length per symbol approaches $H(X)$ as $n \to \infty$:

$$
\lim_{n \to \infty} \frac{\mathbb{E}[L(X^n)]}{n} = H(X)
$$

### Applications in Computing

**Data compression limits.** Every lossless compressor (gzip, bzip2, PNG) is bounded by the empirical entropy of the source. The compression ratio achievable depends on how predictable the data is. Highly entropic data (encrypted, random) cannot be compressed at all.

**Cross-entropy loss in ML.** Neural networks for classification minimize categorical cross-entropy:

```python
import torch
import torch.nn as nn

loss_fn = nn.CrossEntropyLoss()
logits = torch.randn(8, 10)         # 8 samples, 10 classes
targets = torch.randint(0, 10, (8,))
loss = loss_fn(logits, targets)
print(f"Cross-entropy loss: {loss.item():.4f}")
```

**Decision tree splitting (ID3, C4.5).** Information gain, defined as $IG(T, A) = H(T) - H(T | A)$, measures how much entropy a feature reduces. Decision trees split on the feature with maximum information gain:

```python
import numpy as np

def entropy(y):
    _, counts = np.unique(y, return_counts=True)
    probs = counts / len(y)
    return -np.sum(probs * np.log2(probs + 1e-12))

def information_gain(X, y, feature_idx):
    parent_entropy = entropy(y)
    values = np.unique(X[:, feature_idx])
    weighted_child_entropy = 0
    for v in values:
        subset = y[X[:, feature_idx] == v]
        weighted_child_entropy += (len(subset) / len(y)) * entropy(subset)
    return parent_entropy - weighted_child_entropy
```

**Feature selection.** Mutual information and KL divergence rank features by their relevance to the target variable. Features with high mutual information are more useful for prediction.

**Anomaly detection.** Unusual events have high self-information. Logs and metrics can be scored by their surprisal to flag anomalies.

**Variational inference.** Variational autoencoders minimize $D_{KL}(q(z|x) \parallel p(z))$ plus reconstruction loss, balancing compression against fidelity:

```python
def kl_divergence(mu, logvar):
    return -0.5 * torch.sum(1 + logvar - mu.pow(2) - logvar.exp())
```

### Differential (Continuous) Entropy

For continuous random variables, Shannon entropy generalizes to **differential entropy**:

$$
h(X) = -\int_{-\infty}^{\infty} f(x) \log_2 f(x) \, dx
$$

Differential entropy differs from discrete entropy in important ways:

- It can be **negative**. A uniform distribution over $[0, 0.5]$ has $h(X) = -\log_2 2 = -1$ bit.
- It is not invariant under scaling. If $Y = aX$, then $h(Y) = h(X) + \log_2 |a|$.
- It does not have the same operational meaning — it is not a lower bound on average code length.

**Maximum entropy distributions:** Given constraints, the distribution maximizing differential entropy is:

| Constraint | Maximum entropy distribution |
|---|---|
| Bounded support $[a, b]$ | Uniform |
| Fixed mean $\mu$, support $[0, \infty)$ | Exponential |
| Fixed mean $\mu$ and variance $\sigma^2$ | Gaussian |

The Gaussian has maximum entropy among all distributions with a given mean and variance. This is why the normal distribution appears so often — it is the least informative distribution consistent with observed first and second moments.

**Mutual information for continuous variables:**

$$
I(X; Y) = \iint f(x, y) \log_2 \frac{f(x, y)}{f(x) f(y)} \, dx \, dy = h(X) + h(Y) - h(X, Y)
$$

This inherits the same properties as discrete mutual information: non-negativity, symmetry, and invariance under invertible transformations.

### Maximum Entropy Principle

The **maximum entropy principle** (Jaynes, 1957) states that the best probability distribution given partial information is the one with maximum entropy subject to the known constraints. This is the least biased distribution — it does not assume anything beyond what is known.

**Example: Constrained by mean only.** Suppose we know $E[X] = 5$ and $X \geq 0$. The maximum entropy distribution is Exponential($\lambda = 1/5$):

$$
f(x) = \frac{1}{5} e^{-x/5}, \quad x \geq 0
$$

**Example: Constrained by mean and variance.** Suppose $E[X] = 0$ and $E[X^2] = 1$. The maximum entropy distribution is the standard normal:

$$
f(x) = \frac{1}{\sqrt{2\pi}} e^{-x^2/2}
$$

**Applications:**
- **Spectral entropy in signal processing.** The entropy of a signal's power spectrum measures its complexity. White noise has high spectral entropy; a pure tone has low spectral entropy.
- **Entropy regularization in reinforcement learning.** Policy gradient methods add an entropy bonus to encourage exploration. Higher entropy policies try more diverse actions, preventing premature convergence to suboptimal policies.
- **Feature engineering.** Entropy-based discretization (Fayyad & Irani, 1993) finds optimal cut points for continuous features by minimizing the weighted entropy of the resulting intervals.

### Entropy in Practice: Code Examples

**Computing entropy from empirical data:**

```python
import numpy as np
from collections import Counter

def empirical_entropy(data, base=2):
    """Compute Shannon entropy of empirical distribution."""
    counts = Counter(data)
    total = len(data)
    probs = np.array([c / total for c in counts.values()])
    return -np.sum(probs * np.log(probs) / np.log(base))

text = "hello world"
print(f"Character entropy of '{text}': {empirical_entropy(text):.4f} bits")

random_bytes = np.random.randint(0, 256, 1000)
print(f"Entropy of random bytes: {empirical_entropy(random_bytes):.4f} bits")
```

**Entropy-based feature selection in text classification (pointwise mutual information):**

```python
def pointwise_mutual_information(word, category, docs, categories):
    """PMI between a word and a category."""
    n = len(docs)
    p_word = sum(1 for d in docs if word in d) / n
    p_cat = sum(1 for c in categories if c == category) / n
    p_word_cat = sum(1 for d, c in zip(docs, categories) 
                     if word in d and c == category) / n
    if p_word_cat == 0 or p_word == 0:
        return 0
    return np.log2(p_word_cat / (p_word * p_cat))
```

**Entropy of a password as a security measure:**

```python
import math
import string

def password_entropy(password):
    """Estimate password entropy based on character set size."""
    charset = 0
    if any(c.islower() for c in password): charset += 26
    if any(c.isupper() for c in password): charset += 26
    if any(c.isdigit() for c in password): charset += 10
    if any(c in string.punctuation for c in password): charset += 32
    if charset == 0: return 0
    return len(password) * math.log2(charset)

for pw in ["password", "Tr0ub4dor&3", "correct-horse-battery-staple"]:
    print(f"'{pw}': {password_entropy(pw):.1f} bits")
```

This calculation assumes each character is chosen uniformly from the character set, which overestimates entropy for human-chosen passwords but is a useful baseline.

## Glossary

| Term | Definition |
|---|---|
| Entropy | Average information content or uncertainty of a random variable, measured in bits |
| Self-information | Information content of a specific outcome, $-\log_2 p(x)$ |
| Bit | Fundamental unit of information, representing a binary choice |
| Joint entropy | Entropy of a pair of random variables $(X, Y)$ |
| Conditional entropy | Remaining uncertainty of $X$ given knowledge of $Y$ |
| KL divergence | Asymmetric measure of how one distribution diverges from another |
| Cross-entropy | Average number of bits needed to encode samples from $P$ using code optimized for $Q$ |
| Surprisal | Alternative term for self-information |
| Redundancy | Difference between actual code length and entropy |
| Source coding theorem | Shannon's result bounding lossless compression by entropy |
| Concavity | Property where $H(\lambda p + (1-\lambda)q) \geq \lambda H(p) + (1-\lambda)H(q)$ |
| Data processing inequality | Processing can only reduce information content |
| Information gain | Reduction in entropy from knowing a feature, $H(T) - H(T\|A)$ |
| Maximum entropy | Highest possible entropy for a given alphabet size, $\log_2 n$ |
| NAT | Unit of information using natural logarithm (base $e$) |

## Quick References

- [Shannon, C. E. (1948). A Mathematical Theory of Communication](https://people.math.harvard.edu/~ctm/home/text/others/shannon/entropy/entropy.pdf) — the original paper that founded information theory
- [Cover, T. M. & Thomas, J. A. (2006). Elements of Information Theory](https://www.wiley.com/en-us/Elements+of+Information+Theory%2C+2nd+Edition-p-9780471241959) — comprehensive textbook covering all core topics
- [MacKay, D. J. C. (2003). Information Theory, Inference, and Learning Algorithms](https://www.inference.org.uk/mackay/itila/) — free online textbook connecting information theory to machine learning

## Next Steps

- [Mutual Information](mutual-information.md) — measure of dependence between random variables, fundamental to feature selection and channel capacity
- [Source Coding](source-coding.md) — lossless compression algorithms and the source coding theorem
- [Channel Capacity](channel-capacity.md) — maximum rate of reliable communication over a noisy channel
