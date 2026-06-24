# Ethics and Bias

## Description

Machine learning models learn from data — and data reflects historical biases, systemic inequalities, and human prejudices. Building responsible AI means understanding where bias comes from, how it propagates through models, and what principles guide ethical development.

## Prerequisites

- [What Is AI & ML](what-is-ai-ml.md)

## Table of Contents

- [Bias in, Bias out](#bias-in-bias-out)
- [Sources of Bias](#sources-of-bias)
- [Fairness Definitions](#fairness-definitions)
- [Principles for Responsible AI](#principles-for-responsible-ai)

## Content / Material

### Bias in, Bias out

A model is only as good as its training data. If historical hiring data favors one demographic, a model trained on it will learn and amplify that bias.

```
Data: Past hiring decisions (biased) → Model learns patterns → Model predicts future hires
                                                                      ↓
                                                              Bias is codified and scaled
```

The model is not "biased" in a moral sense — it faithfully learned the patterns in the data. The problem is the data.

### Sources of Bias

| Source | Example |
|--------|---------|
| **Historical bias** | Training data reflects past discrimination |
| **Representation bias** | Certain groups are underrepresented in training data |
| **Measurement bias** | The features used don't accurately measure what you care about |
| **Aggregation bias** | A single model is applied to groups with different characteristics |
| **Evaluation bias** | Benchmarks don't test for fairness across groups |
| **Deployment bias** | The system is used in a context different from training |

### Fairness Definitions

Fairness is not a single concept — there are multiple, often conflicting definitions:

- **Demographic parity** — outcomes are independent of group membership.
- **Equal opportunity** — true positive rates are equal across groups.
- **Equal accuracy** — error rates are equal across groups.
- **Individual fairness** — similar individuals receive similar outcomes.

These definitions can't all be satisfied simultaneously. Choosing which definition to use is an ethical decision, not a technical one.

### Principles for Responsible AI

- **Accountability** — someone is responsible for the system's behavior.
- **Transparency** — decisions can be explained and audited.
- **Fairness** — outcomes do not systematically disadvantage groups.
- **Privacy** — data is collected and used with consent and protection.
- **Robustness** — the system performs reliably across diverse conditions.

## Glossary

| Term | Definition |
|------|------------|
| Bias | Systematic error in data or models that produces unfair outcomes |
| Demographic parity | A fairness criterion requiring equal outcomes across groups |
| Responsible AI | A framework for developing AI that is ethical, transparent, and accountable |
| Explainability | The ability to understand and interpret a model's decisions |

## Next Steps

- [Supervised Learning](../beginner/supervised-learning.md) — regression and classification fundamentals
