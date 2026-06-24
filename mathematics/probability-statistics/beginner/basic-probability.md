# Basic Probability

## Description

Probability quantifies uncertainty. It answers: given a process, how likely is each outcome? This is the foundation of ML, A/B testing, risk analysis, and every system that makes predictions or decisions under uncertainty.

## Prerequisites

- [What Is Probability & Statistics](../intro/what-is-probability-statistics.md)

## Table of Contents

- [Core Concepts](#core-concepts)
- [Rules of Probability](#rules-of-probability)
- [Bayes' Theorem](#bayes-theorem)
- [Random Variables](#random-variables)

## Content / Material

### Core Concepts

- **Experiment**: any process with uncertain outcomes (flip a coin).
- **Sample space** ($\Omega$): all possible outcomes: $\\{H, T\\}$.
- **Event**: a subset of outcomes (e.g., "heads").
- **Probability**: $P(E) = \frac{\text{favorable}}{\text{total}}$ (for equally likely outcomes).

### Rules of Probability

| Rule | Formula |
|------|---------|
| Range | $0 \leq P(E) \leq 1$ |
| Complement | $P(\text{not }E) = 1 - P(E)$ |
| Union | $P(A \cup B) = P(A) + P(B) - P(A \cap B)$ |
| Intersection (independent) | $P(A \cap B) = P(A) \cdot P(B)$ |
| Conditional | $P(A \mid B) = \frac{P(A \cap B)}{P(B)}$ |

```python
# Simulating probability
import random
trials = 100000
heads = sum(random.random() < 0.5 for _ in range(trials))
print(f"P(heads) ≈ {heads / trials}")  # ≈ 0.5
```

### Bayes' Theorem

Bayes' theorem updates probabilities given evidence:

$$ P(A \mid B) = \frac{P(B \mid A) \cdot P(A)}{P(B)} $$

This is the mathematical foundation of learning from data. Spam filters, medical tests, and ML classification all use Bayes.

### Random Variables

A random variable maps outcomes to numbers:

```python
# Binomial random variable: number of heads in 10 flips
import numpy as np
X = np.random.binomial(10, 0.5, size=1000)
print(X.mean())  # ≈ 5.0 (expected value)
```

- **Discrete**: finite outcomes (coin flip, dice roll).
- **Continuous**: infinite outcomes (temperature, time).

## Glossary

| Term | Definition |
|------|------------|
| Sample space | The set of all possible outcomes |
| Conditional probability | Probability of one event given another has occurred |
| Bayes' theorem | Formula for updating probabilities given evidence |
| Random variable | A variable whose value depends on random outcomes |

## Next Steps

- [Distributions](distributions.md) — normal, binomial, Poisson, and their use cases
