# What Is Probability & Statistics

## Description

Probability is the mathematics of uncertainty — how likely is an event? Statistics is the inverse: given data, what can we infer about the underlying process? Together, they form the foundation of data science, machine learning, A/B testing, and every data-driven decision in engineering.

## Prerequisites

- [Why Math Matters](../../intro/why-math.md)

## Table of Contents

- [Probability vs. Statistics](#probability-vs-statistics)
- [Why Developers Need Both](#why-developers-need-both)
- [The Mindset: Thinking in Distributions](#the-mindset-thinking-in-distributions)
- [Common Pitfalls](#common-pitfalls)

## Content / Material

### Probability vs. Statistics

```
Probability: Given the process → what data do we expect?
    P(heads) = 0.5   →   In 100 flips, expect ~50 heads

Statistics:   Given data → what process produced it?
    Observed 60 heads in 100 flips   →   Is the coin fair?
```

Probability reasons forward. Statistics reasons backward.

### Why Developers Need Both

- **A/B testing**: is the new feature actually better, or is the difference just noise?
- **Machine learning**: training is statistical inference. Predictions are probabilistic.
- **Performance monitoring**: is this latency spike a real degradation or random variance?
- **Capacity planning**: estimate resource needs based on traffic distributions, not averages.
- **Error handling**: rate limiting, retry logic, and circuit breakers all depend on probability models.

### The Mindset: Thinking in Distributions

The most important mental shift: stop thinking in single values and start thinking in distributions.

```
Average latency: 200ms        ❌ hides the 1% of requests that take 5 seconds
Latency distribution:         ✅ shows p50=150ms, p99=500ms, p99.9=3s
```

Distributions capture uncertainty and variability. Averages lie.

### Common Pitfalls

| Pitfall | What It Means |
|---------|---------------|
| **Confusing correlation with causation** | Ice cream sales and drowning both increase in summer — they're correlated, not causally linked |
| **Survivorship bias** | Looking only at successes (startups that survived) and ignoring failures |
| **p-hacking** | Running many statistical tests until one shows significance |
| **Base rate fallacy** | Ignoring how rare something is before interpreting a positive test |

## Glossary

| Term | Definition |
|------|------------|
| Distribution | A function describing the possible values of a variable and their probabilities |
| P-value | The probability of observing data at least as extreme, assuming the null hypothesis is true |
| Correlation | A measure of how two variables move together |
| Bayes' theorem | A formula describing how to update probabilities given new evidence |

## Next Steps

- [Basic Probability](../beginner/basic-probability.md) — events, independence, Bayes' rule
- [Distributions](../beginner/distributions.md) — normal, binomial, Poisson, and their use cases
