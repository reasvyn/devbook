# Scientific Thinking

## Description

Scientific thinking is a framework for understanding the world through observation, hypothesis, experimentation, and refinement. For developers, it is the foundation of debugging, performance tuning, A/B testing, and any data-driven decision.

## Prerequisites

- [Why Physics Matters](why-physics.md)

## Table of Contents

- [The Scientific Method](#the-scientific-method)
- [Models Are Simplifications](#models-are-simplifications)
- [Hypothesis-Driven Debugging](#hypothesis-driven-debugging)
- [Measurement and Empiricism](#measurement-and-empiricism)

## Content / Material

### The Scientific Method

1. **Observe** — something is happening.
2. **Question** — why does it happen?
3. **Hypothesize** — propose an explanation.
4. **Predict** — if my hypothesis is correct, then ___ should happen.
5. **Experiment** — test the prediction.
6. **Analyze** — does the result support or refute the hypothesis?

This is exactly the cycle of debugging a production issue.

### Models Are Simplifications

Every scientific model is a simplified representation of reality. Newton's laws ignore relativity. Thermodynamics ignores individual molecules. This is not a weakness — it's the point.

In software, every abstraction is a model. HTTP hides TCP. TCP hides IP. IP hides the physical wire. Understanding that models are approximations — valid within certain bounds — helps you know when they break.

### Hypothesis-Driven Debugging

Instead of randomly changing code, apply the scientific method:

```
Bug: users report 500 errors on checkout

Hypothesis: database connection pool is exhausted
Prediction: increasing pool size will reduce errors
Experiment: increase pool size, monitor error rate
Analysis: errors dropped by 80% — hypothesis supported, but another factor remains
```

### Measurement and Empiricism

Science without measurement is philosophy. Software without metrics is guesswork.

- Measure before and after every change.
- Isolate variables — change one thing at a time.
- Be aware of observer effect — monitoring itself can change behavior (Heisenberg for DevOps).

## Glossary

| Term | Definition |
|------|------------|
| Hypothesis | A proposed explanation that can be tested |
| Model | A simplified representation of a system |
| Empiricism | The practice of relying on observation and experiment |
| Observer effect | The act of observing a system can alter its behavior |

## Next Steps

- [Systems and Models](systems-and-models.md) — thinking in terms of systems, feedback loops, and emergence
