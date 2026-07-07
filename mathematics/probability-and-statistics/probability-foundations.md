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
- [Common Probability Fallacies](#common-probability-fallacies)
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

We can compute this in Python to verify and explore:

```python
def posterior_disease(sensitivity, specificity, prevalence):
    p_pos_given_disease = sensitivity
    p_pos_given_healthy = 1 - specificity
    p_disease = prevalence
    p_healthy = 1 - prevalence
    p_pos = p_pos_given_disease * p_disease + p_pos_given_healthy * p_healthy
    return (p_pos_given_disease * p_disease) / p_pos

print(posterior_disease(0.99, 0.99, 0.01))
# Output: 0.5
```

Varying the inputs reveals how strongly the base rate affects the result:

```python
for prevalence in [0.001, 0.01, 0.1, 0.5]:
    p = posterior_disease(0.99, 0.99, prevalence)
    print(f"prevalence={prevalence}: P(disease | positive)={p:.4f}")
# Prevalence   Posterior
#   0.001        0.0902
#   0.01         0.5000
#   0.1          0.9167
#   0.5          0.9900
```

### The Law of Total Probability

If $B_1, B_2, \dots, B_n$ partition the sample space (mutually exclusive and exhaustive), then for any event $A$:

$$P(A) = \sum_{i=1}^{n} P(A \mid B_i) P(B_i).$$

This is one of the most useful tools in probability. It lets you compute an unconditional probability by conditioning on known cases.

**Example — Software bugs:** Two code modules contribute to a system. Module 1 handles 60% of requests with a 2% bug rate; Module 2 handles 40% with a 5% bug rate. The overall probability a request encounters a bug is:

$$P(\text{bug}) = 0.02 \times 0.60 + 0.05 \times 0.40 = 0.012 + 0.020 = 0.032.$$

```python
def total_probability(cond_probs, weights):
    return sum(p * w for p, w in zip(cond_probs, weights))

p_bug = total_probability([0.02, 0.05], [0.60, 0.40])
print(f"P(bug) = {p_bug}")
# Output: 0.032
```

### Bayes Theorem

Bayes' theorem reverses the direction of conditioning:

$$P(A \mid B) = \frac{P(B \mid A) P(A)}{P(B)}.$$

Using the law of total probability for the denominator:

$$P(A \mid B) = \frac{P(B \mid A) P(A)}{\sum_i P(B \mid A_i) P(A_i)}.$$

**Interpretation:** $P(A)$ is the **prior** probability (before seeing evidence $B$). $P(A \mid B)$ is the **posterior** (after seeing $B$). The ratio $P(B \mid A) / P(B)$ is the evidence update.

Bayes' theorem is the foundation of Bayesian statistics, spam filters, diagnostic systems, and many machine learning algorithms.

**Example — Spam filtering:** Suppose 20% of emails are spam. The word "free" appears in 60% of spam emails and 5% of legitimate emails. Given an email containing "free", the probability it is spam is:

$$P(\text{spam} \mid \text{"free"}) = \frac{0.60 \times 0.20}{0.60 \times 0.20 + 0.05 \times 0.80} = \frac{0.12}{0.12 + 0.04} = 0.75.$$

```python
def bayes_spam(p_spam, p_free_given_spam, p_free_given_ham):
    p_ham = 1 - p_spam
    p_free = p_free_given_spam * p_spam + p_free_given_ham * p_ham
    return (p_free_given_spam * p_spam) / p_free

p = bayes_spam(0.20, 0.60, 0.05)
print(f"P(spam | 'free') = {p:.4f}")
# Output: 0.7500
```

We can extend this to multiple words by assuming conditional independence (the Naive Bayes assumption):

```python
def naive_bayes_spam(p_spam, p_word_given_spam, p_word_given_ham, words):
    log_odds_spam = math.log(p_spam / (1 - p_spam))
    for w in words:
        log_odds_spam += math.log(p_word_given_spam[w] / p_word_given_ham[w])
    return 1 / (1 + math.exp(-log_odds_spam))

import math
p_word_spam = {"free": 0.60, "win": 0.40, "money": 0.55}
p_word_ham = {"free": 0.05, "win": 0.02, "money": 0.03}
p = naive_bayes_spam(0.20, p_word_spam, p_word_ham, ["free", "money"])
print(f"P(spam | 'free', 'money') = {p:.4f}")
# Output: 0.9929
```

With two spammy words together, the posterior probability jumps dramatically.

### Independence

Two events are **independent** if knowledge of one does not change the probability of the other:

$$P(A \cap B) = P(A) P(B).$$

Equivalently, $P(A \mid B) = P(A)$ and $P(B \mid A) = P(B)$.

**Independence vs. mutual exclusivity:** These are fundamentally different. Mutually exclusive events have $P(A \cap B) = 0$, so unless one has probability zero, they cannot be independent — knowing one occurs tells you the other did not.

**Conditional independence:** Events can be independent conditional on a third event:

$$P(A \cap B \mid C) = P(A \mid C) P(B \mid C).$$

This is a crucial concept in graphical models and Bayesian networks.

**Example — Feature independence in Naive Bayes:** The Naive Bayes classifier assumes features are conditionally independent given the class label. Despite being a strong (often false) assumption, it works well for text classification.

```python
# Simulating independence: coin flips
import random
def simulate_independent_events(n_trials=100000):
    heads_heads = 0
    heads = 0
    for _ in range(n_trials):
        a = random.choice(["H", "T"])
        b = random.choice(["H", "T"])
        if a == "H":
            heads += 1
            if b == "H":
                heads_heads += 1
    p_a = heads / n_trials
    p_b = 0.5
    p_ab = heads_heads / n_trials
    print(f"P(A)={p_a:.4f}, P(B)={p_b:.4f}, P(A∩B)={p_ab:.4f}, P(A)P(B)={p_a*p_b:.4f}")

simulate_independent_events()
# Output: P(A)≈0.5000, P(B)=0.5000, P(A∩B)≈0.2500, P(A)P(B)≈0.2500
```

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

```python
import math
def comb(n, k):
    return math.comb(n, k)

def perm(n, k):
    return math.perm(n, k)

print(f"P(10,3) = {perm(10, 3)}")   # 720
print(f"C(10,3) = {comb(10, 3)}")   # 120
```

**Example — Lottery:** The number of ways to choose 6 numbers from 49 is $\binom{49}{6} = 13,983,816$. Your chance of winning with one ticket is $1 / 13,983,816 \approx 7.15 \times 10^{-8}$.

```python
lottery_odds = 1 / math.comb(49, 6)
print(f"Lottery odds: {lottery_odds:.2e}")
# Output: 7.15e-08
```

### Combinatorics and Probability

Classical probability for equally likely outcomes:

$$P(A) = \frac{|A|}{|\Omega|} = \frac{\text{favorable outcomes}}{\text{total outcomes}}.$$

**Example — Poker hand:** Probability of a flush (5 cards of the same suit) in a 5-card poker hand:

$$P(\text{flush}) = \frac{4 \times \binom{13}{5}}{\binom{52}{5}} = \frac{4 \times 1287}{2,598,960} \approx 0.00198.$$

```python
p_flush = 4 * math.comb(13, 5) / math.comb(52, 5)
print(f"P(flush) = {p_flush:.5f}")
# Output: 0.00198
```

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

We can simulate the birthday problem to verify:

```python
import random
def simulate_birthday(n, trials=100000):
    matches = 0
    for _ in range(trials):
        birthdays = [random.randint(1, 365) for _ in range(n)]
        if len(birthdays) != len(set(birthdays)):
            matches += 1
    return matches / trials

for n in [23, 30, 50]:
    sim = simulate_birthday(n)
    exact = birthday_probability(n)
    print(f"n={n}: simulated={sim:.4f}, exact={exact:.4f}")
# Output:
# n=23: simulated=0.5070, exact=0.5073
# n=30: simulated=0.7060, exact=0.7063
# n=50: simulated=0.9705, exact=0.9704
```

The simulation confirms the analytic result — a hallmark of Monte Carlo methods.

### Probability in Practice

**A/B testing:** Given two versions of a webpage (A and B), we observe conversion counts. Probability theory lets us compute the chance that the observed difference is due to random variation.

```python
import random
import math

def ab_test_p_value(conv_a, total_a, conv_b, total_b, n_simulations=100000):
    rate_a = conv_a / total_a
    rate_b = conv_b / total_b
    observed_diff = abs(rate_a - rate_b)
    pooled_rate = (conv_a + conv_b) / (total_a + total_b)
    count_extreme = 0
    for _ in range(n_simulations):
        sim_a = sum(1 for _ in range(total_a) if random.random() < pooled_rate)
        sim_b = sum(1 for _ in range(total_b) if random.random() < pooled_rate)
        sim_diff = abs(sim_a / total_a - sim_b / total_b)
        if sim_diff >= observed_diff:
            count_extreme += 1
    return count_extreme / n_simulations

p_value = ab_test_p_value(120, 1000, 150, 1000)
print(f"p-value ≈ {p_value:.4f}")
# A large p-value (> 0.05) suggests the difference could be due to chance.
```

**Spam filtering:** Naive Bayes uses Bayes' theorem with conditional independence assumptions to classify emails. Each word contributes evidence toward spam or ham classification.

**Diagnostic testing:** Bayes' theorem corrects for base rates. A 99% accurate test for a rare disease (0.1% prevalence) yields only about 9% posterior probability of having the disease after a positive result.

**Risk assessment:** Financial risk models use probability to quantify Value at Risk (VaR) — the maximum loss with a given confidence level over a specified period.

**Reliability engineering:** System reliability is computed from component reliabilities using probability rules for series and parallel configurations.

```python
def system_reliability_series(component_reliabilities):
    product = 1.0
    for r in component_reliabilities:
        product *= r
    return product

def system_reliability_parallel(component_reliabilities):
    product_of_failures = 1.0
    for r in component_reliabilities:
        product_of_failures *= (1 - r)
    return 1 - product_of_failures

print(system_reliability_series([0.99, 0.98, 0.97]))
# 0.94 — each component reduces reliability
print(system_reliability_parallel([0.99, 0.98, 0.97]))
# 0.999994 — redundancy improves reliability
```

**Monte Carlo simulation:** Complex systems are modeled by simulating random inputs and aggregating output distributions.

```python
def estimate_pi(num_points=100000):
    inside = 0
    for _ in range(num_points):
        x = random.uniform(-1, 1)
        y = random.uniform(-1, 1)
        if x*x + y*y <= 1:
            inside += 1
    return 4 * inside / num_points

print(f"π ≈ {estimate_pi(50000)}")
# Output: approximately 3.14159
```

This classic Monte Carlo simulation estimates $\pi$ by computing the ratio of points inside a unit circle to points in the enclosing square. The accuracy improves with more samples, scaling as $1/\sqrt{n}$.

**Machine learning loss functions:** Cross-entropy loss derives from the negative log-likelihood of a probabilistic model. Probabilistic classifiers output class probabilities, not just labels.

**Randomized algorithms:** Algorithms like Quicksort use randomization to achieve good expected performance on any input.

### Common Probability Fallacies

**The Gambler's Fallacy:** Believing that past independent events affect future probabilities. After five heads in a row, a fair coin still has a 50% chance of landing heads on the next flip. The misconception arises because the sequence HHHHHH looks less random than HTHTHT, but both are equally likely.

**The Prosecutor's Fallacy:** Confusing $P(\text{evidence} \mid \text{innocence})$ with $P(\text{innocence} \mid \text{evidence})$. A DNA match with probability 1 in a million does not mean the defendant is guilty with 99.9999% certainty — it depends on the prior probability of guilt and the size of the suspect pool.

**Base Rate Neglect:** Ignoring the prevalence of a condition when interpreting test results. A test that is 99% accurate for a disease affecting 0.1% of the population yields a posterior probability of only 9%, not 99%.

**The Birthday Paradox Intuition:** Most people guess the probability of a shared birthday in a group of 23 is much lower than 50%. The mistake comes from thinking about the chance that someone shares *your* birthday (about 1/365 per person) rather than considering all $\binom{23}{2} = 253$ possible pairs.

**The Monty Hall Problem:** A contestant picks one of three doors, one hiding a car. The host opens a door with a goat. Switching doors gives a 2/3 chance of winning, not 1/2. The fallacy is treating the two remaining doors as equally likely when the host's action provides information.

```python
def monty_hall_simulation(trials=100000, switch=True):
    wins = 0
    for _ in range(trials):
        car = random.randint(0, 2)
        pick = random.randint(0, 2)
        opened = random.choice([d for d in range(3) if d != car and d != pick])
        if switch:
            pick = [d for d in range(3) if d != pick and d != opened][0]
        if pick == car:
            wins += 1
    return wins / trials

print(f"Win rate (stay): {monty_hall_simulation(switch=False):.4f}")
print(f"Win rate (switch): {monty_hall_simulation(switch=True):.4f}")
# Output:
# Win rate (stay): 0.3334
# Win rate (switch): 0.6666
```

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
| Gambler's fallacy | The mistaken belief that past independent events affect future probabilities |
| Base rate fallacy | Ignoring the base rate when evaluating conditional probabilities |
| Prosecutor's fallacy | Confusing P(evidence | innocence) with P(innocence | evidence) |
| Monte Carlo method | Estimating outcomes through repeated random sampling and simulation |
| Expected value | The long-run average value of a random variable |
| Variance | A measure of how spread out a distribution is |
| Standard deviation | The square root of variance, measured in the same units as the data |
| Law of large numbers | As sample size increases, the sample average converges to the expected value |
| Central limit theorem | The distribution of sample means approaches a normal distribution as sample size grows |
| A/B testing | A randomized experiment comparing two variants to determine which performs better |
| Null hypothesis | The default assumption that there is no effect or no difference |
| p-value | The probability of observing results at least as extreme as those measured, assuming the null hypothesis is true |

## Quick References

- [Probability: Theory and Examples, Durrett](https://www.cambridge.org/us/academic/subjects/statistics-probability/probability-theory-and-stochastic-processes/probability-theory-and-examples-5th-edition) — rigorous treatment of probability theory
- [Naive Bayes Spam Filtering](https://en.wikipedia.org/wiki/Naive_Bayes_spam_filtering) — Wikipedia article on practical application
- [Seeing Theory](https://seeing-theory.brown.edu/) — interactive visual introduction to probability
- [Kolmogorov's Axioms](https://en.wikipedia.org/wiki/Probability_axioms) — the three axioms that underpin all of probability
- [Introduction to Probability, Harvard](https://projects.iq.harvard.edu/stat110) — free online course with lectures and problem sets
- [Probability Cheatsheet](https://github.com/wzchen/probability_cheatsheet) — concise reference for probability concepts and formulas
- [Monty Hall Problem Simulation](https://www.mathwarehouse.com/monty-hall-simulation-online/) — interactive simulation of the paradox
- [3Blue1Brown: Bayes Theorem](https://www.youtube.com/watch?v=HZGCoVF3YvM) — visual explanation of Bayes' theorem

## Next Steps

- [Random Variables](random-variables.md) — quantifying outcomes numerically, PMFs, PDFs, and expectations
- [Distributions](distributions.md) — common probability distributions and their properties
