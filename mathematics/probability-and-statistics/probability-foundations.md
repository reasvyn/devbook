# Probability Foundations

## Description

Probability theory quantifies uncertainty. It is the mathematical language for reasoning about random events, forming the backbone of modern statistics, machine learning, A/B testing, risk assessment, and spam filtering. Understanding probability foundations lets you answer questions like "How likely is this conversion rate difference due to chance?" and "What is the chance a transaction is fraudulent given its features?"

## Prerequisites

- [Mathematical Thinking](../intro/mathematical-thinking.md) — comfort with set notation, functions, and basic combinatorics

## Table of Contents

- [Sample Spaces and Events](#sample-spaces-and-events)
- [Axioms of Probability](#axioms-of-probability)
- [Conditional Probability](#conditional-probability)
- [The Law of Total Probability](#the-law-of-total-probability)
- [Bayes Theorem](#bayes-theorem)
- [Independence](#independence)
- [Probability vs Statistics](#probability-vs-statistics)
- [Counting Methods](#counting-methods)
- [Combinatorics and Probability](#combinatorics-and-probability)
- [Probability in Practice](#probability-in-practice)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Sample Spaces and Events

A **random experiment** is any process whose outcome is uncertain. The set of all possible outcomes is the **sample space**, denoted by $\Omega$ or $S$.

**Example — Coin flip:** $\Omega = \{H, T\}$.

**Example — Six-sided die roll:** $\Omega = \{1, 2, 3, 4, 5, 6\}$.

**Example — Website visit duration (continuous):** $\Omega = \{t \in \mathbb{R} \mid t \ge 0\}$.

An **event** is any subset of the sample space. Events are typically denoted by capital letters $A, B, C$.

- $A = \text{"die roll is even"} = \{2, 4, 6\}$
- $B = \text{"die roll > 4"} = \{5, 6\}$
- $C = \text{"visit lasts more than 30 seconds"} = \{t \mid t > 30\}$

Set operations on events mirror logical combinations:

- **Union** $A \cup B$ — at least one of $A$ or $B$ occurs
- **Intersection** $A \cap B$ — both $A$ and $B$ occur
- **Complement** $A^c$ — $A$ does not occur
- **Difference** $A \setminus B$ — $A$ occurs but $B$ does not

Two events are **mutually exclusive** (disjoint) if $A \cap B = \varnothing$. They cannot happen simultaneously.

### Axioms of Probability

A **probability measure** $P$ is a function mapping events to numbers in $[0, 1]$ satisfying three axioms (Kolmogorov, 1933):

1. **Non-negativity:** $P(A) \ge 0$ for any event $A$.
2. **Normalization:** $P(\Omega) = 1$.
3. **Countable additivity:** For any countable sequence of mutually exclusive events $A_1, A_2, \dots$,
   $$P\left(\bigcup_{i=1}^{\infty} A_i\right) = \sum_{i=1}^{\infty} P(A_i).$$

From these three axioms, all of probability theory follows.

**Derived properties:**

- $P(\varnothing) = 0$
- $P(A^c) = 1 - P(A)$
- If $A \subseteq B$, then $P(A) \le P(B)$
- $P(A \cup B) = P(A) + P(B) - P(A \cap B)$ (inclusion-exclusion)

**Example:** In a fair six-sided die, $P(\{2, 4, 6\}) = P(2) + P(4) + P(6) = 1/6 + 1/6 + 1/6 = 1/2$.

### Conditional Probability

The probability that $A$ occurs given that $B$ has already occurred is:

$$P(A \mid B) = \frac{P(A \cap B)}{P(B)}, \quad \text{provided } P(B) > 0.$$

**Intuition:** Knowing $B$ occurred restricts the sample space to $B$. The probability of $A$ is the proportion of $B$ where $A$ also happens.

**Example — Medical test:** If 1% of the population has a disease ($P(D) = 0.01$), and a test is 99% accurate ($P(T^+ \mid D) = 0.99$, $P(T^- \mid D^c) = 0.99$), then the probability of having the disease given a positive test is:

$$P(D \mid T^+) = \frac{P(T^+ \mid D) P(D)}{P(T^+)} = \frac{0.99 \times 0.01}{0.99 \times 0.01 + 0.01 \times 0.99} \approx 0.50.$$

This counterintuitive result — only 50% despite 99% accuracy — illustrates the **base rate fallacy**.

### The Law of Total Probability

If $B_1, B_2, \dots, B_n$ partition the sample space (mutually exclusive and exhaustive), then for any event $A$:

$$P(A) = \sum_{i=1}^{n} P(A \mid B_i) P(B_i).$$

This is one of the most useful tools in probability. It lets you compute an unconditional probability by conditioning on known cases.

**Example — Software bugs:** Two code modules contribute to a system. Module 1 handles 60% of requests with a 2% bug rate; Module 2 handles 40% with a 5% bug rate. The overall probability a request encounters a bug is:

$$P(\text{bug}) = 0.02 \times 0.60 + 0.05 \times 0.40 = 0.012 + 0.020 = 0.032.$$

### Bayes Theorem

Bayes' theorem reverses the direction of conditioning:

$$P(A \mid B) = \frac{P(B \mid A) P(A)}{P(B)}.$$

Using the law of total probability for the denominator:

$$P(A \mid B) = \frac{P(B \mid A) P(A)}{\sum_i P(B \mid A_i) P(A_i)}.$$

**Interpretation:** $P(A)$ is the **prior** probability (before seeing evidence $B$). $P(A \mid B)$ is the **posterior** (after seeing $B$). The ratio $P(B \mid A) / P(B)$ is the evidence update.

Bayes' theorem is the foundation of Bayesian statistics, spam filters, diagnostic systems, and many machine learning algorithms.

**Example — Spam filtering:** Suppose 20% of emails are spam. The word "free" appears in 60% of spam emails and 5% of legitimate emails. Given an email containing "free", the probability it is spam is:

$$P(\text{spam} \mid \text{"free"}) = \frac{0.60 \times 0.20}{0.60 \times 0.20 + 0.05 \times 0.80} = \frac{0.12}{0.12 + 0.04} = 0.75.$$

### Independence

Two events are **independent** if knowledge of one does not change the probability of the other:

$$P(A \cap B) = P(A) P(B).$$

Equivalently, $P(A \mid B) = P(A)$ and $P(B \mid A) = P(B)$.

**Independence vs. mutual exclusivity:** These are fundamentally different. Mutually exclusive events have $P(A \cap B) = 0$, so unless one has probability zero, they cannot be independent — knowing one occurs tells you the other did not.

**Conditional independence:** Events can be independent conditional on a third event:

$$P(A \cap B \mid C) = P(A \mid C) P(B \mid C).$$

This is a crucial concept in graphical models and Bayesian networks.

**Example — Feature independence in Naive Bayes:** The Naive Bayes classifier assumes features are conditionally independent given the class label. Despite being a strong (often false) assumption, it works well for text classification.

### Probability vs Statistics

The two fields are complementary but have opposite directions:

| Aspect | Probability | Statistics |
|--------|-------------|------------|
| Direction | Model $\rightarrow$ Data | Data $\rightarrow$ Model |
| Known | Distribution parameters | Observed data |
| Compute | Likelihood of outcomes | Parameters from data |
| Question | "If we flip this fair coin, how often do we get 3 heads?" | "We got 3 heads out of 5 flips — is this coin fair?" |

**Analogy:** Probability is physics (forward simulation); statistics is archaeology (reverse inference).

### Counting Methods

Many probability calculations reduce to counting outcomes.

**Multiplication principle:** If a process has $k$ stages with $n_i$ choices at stage $i$, the total number of outcomes is $\prod_{i=1}^k n_i$.

**Permutations** (ordered arrangements without replacement):

$$P(n, k) = \frac{n!}{(n-k)!}.$$

**Combinations** (unordered subsets without replacement):

$$\binom{n}{k} = \frac{n!}{k!(n-k)!}.$$

**Example — Lottery:** The number of ways to choose 6 numbers from 49 is $\binom{49}{6} = 13,983,816$. Your chance of winning with one ticket is $1 / 13,983,816 \approx 7.15 \times 10^{-8}$.

### Combinatorics and Probability

Classical probability for equally likely outcomes:

$$P(A) = \frac{|A|}{|\Omega|} = \frac{\text{favorable outcomes}}{\text{total outcomes}}.$$

**Example — Poker hand:** Probability of a flush (5 cards of the same suit) in a 5-card poker hand:

$$P(\text{flush}) = \frac{4 \times \binom{13}{5}}{\binom{52}{5}} = \frac{4 \times 1287}{2,598,960} \approx 0.00198.$$

**Example — Birthday problem:** In a group of 23 people, the probability that at least two share a birthday exceeds 50%. With $n$ people:

$$P(\text{no match}) = \frac{365}{365} \times \frac{364}{365} \times \cdots \times \frac{365 - n + 1}{365}.$$

```python
def birthday_probability(n):
    p_no_match = 1.0
    for i in range(n):
        p_no_match *= (365 - i) / 365
    return 1 - p_no_match

for n in [23, 30, 50, 70]:
    print(f"n={n}: P(match) = {birthday_probability(n):.4f}")
```

Output:
```
n=23: P(match) = 0.5073
n=30: P(match) = 0.7063
n=50: P(match) = 0.9704
n=70: P(match) = 0.9992
```

### Probability in Practice

**A/B testing:** Given two versions of a webpage (A and B), we observe conversion counts. Probability theory lets us compute the chance that the observed difference is due to random variation.

**Spam filtering:** Naive Bayes uses Bayes' theorem with conditional independence assumptions to classify emails. Each word contributes evidence toward spam or ham classification.

**Diagnostic testing:** Bayes' theorem corrects for base rates. A 99% accurate test for a rare disease (0.1% prevalence) yields only about 9% posterior probability of having the disease after a positive result.

**Risk assessment:** Financial risk models use probability to quantify Value at Risk (VaR) — the maximum loss with a given confidence level over a specified period.

**Reliability engineering:** System reliability is computed from component reliabilities using probability rules for series and parallel configurations.

**Monte Carlo simulation:** Complex systems are modeled by simulating random inputs and aggregating output distributions — used in everything from physics to finance to game development.

**Machine learning loss functions:** Cross-entropy loss derives from the negative log-likelihood of a probabilistic model. Probabilistic classifiers output class probabilities, not just labels.

**Randomized algorithms:** Algorithms like Quicksort use randomization to achieve good expected performance on any input.

## Glossary

| Term | Definition |
|------|------------|
| Sample space | The set of all possible outcomes of a random experiment |
| Event | A subset of the sample space |
| Probability measure | A function assigning probabilities to events satisfying the Kolmogorov axioms |
| Conditional probability | The probability of one event given that another has occurred |
| Bayes theorem | A formula for reversing conditional probabilities |
| Law of total probability | Expresses an unconditional probability as a weighted average of conditional probabilities |
| Independence | Two events are independent if the occurrence of one does not affect the probability of the other |
| Mutual exclusivity | Two events cannot occur simultaneously |
| Prior probability | The probability assigned before observing evidence |
| Posterior probability | The probability after incorporating evidence |
| Base rate | The prevalence of a condition in the population |
| Sensitivity | True positive rate — P(positive | condition present) |
| Specificity | True negative rate — P(negative | condition absent) |
| Permutation | An ordered arrangement of items |
| Combination | An unordered selection of items |
| Union bound | An upper bound on the probability of a union of events |
| Simpson's paradox | A trend appears in groups but disappears or reverses when groups are combined |
| Random experiment | A process whose outcome is uncertain |
| Inclusion-exclusion | A formula for the probability of a union of events |

## Quick References

- [Probability: Theory and Examples, Durrett](https://www.cambridge.org/us/academic/subjects/statistics-probability/probability-theory-and-stochastic-processes/probability-theory-and-examples-5th-edition) — rigorous treatment of probability theory
- [Naive Bayes Spam Filtering](https://en.wikipedia.org/wiki/Naive_Bayes_spam_filtering) — Wikipedia article on practical application
- [Seeing Theory](https://seeing-theory.brown.edu/) — interactive visual introduction to probability
- [Kolmogorov's Axioms](https://en.wikipedia.org/wiki/Probability_axioms) — the three axioms that underpin all of probability

## Next Steps

- [Random Variables](random-variables.md) — quantifying outcomes numerically, PMFs, PDFs, and expectations
