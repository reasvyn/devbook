# What Is AI & ML

## Description

Artificial intelligence and machine learning are transforming how software is built — from recommendation systems to code completion to autonomous systems. This document introduces the key concepts every developer should know to understand and work with AI/ML.

## Prerequisites

- [Why Math Matters](../../mathematics/intro/why-math.md) — basic statistics and linear algebra

## Table of Contents

- [AI vs. ML vs. Deep Learning](#ai-vs-ml-vs-deep-learning)
- [How Machine Learning Works](#how-machine-learning-works)
- [Types of Machine Learning](#types-of-machine-learning)
- [ML in the Developer Workflow](#ml-in-the-developer-workflow)

## Content / Material

### AI vs. ML vs. Deep Learning

```
Artificial Intelligence  (broad — machines mimicking intelligence)
├── Machine Learning      (learn from data without explicit rules)
│   ├── Deep Learning     (neural networks with many layers)
│   └── Traditional ML    (decision trees, SVMs, etc.)
└── Symbolic AI           (rule-based systems, expert systems)
```

Most of what powers modern AI products (LLMs, recommendations, vision) is deep learning.

### How Machine Learning Works

Traditional programming:

```
Rules + Data → Answers
```

Machine learning:

```
Data + Answers → Rules (model)
```

Instead of writing rules by hand, you feed examples to an algorithm that learns the rules itself.

### Types of Machine Learning

| Type | Description | Example |
|------|-------------|---------|
| **Supervised** | Labeled data — learn input-to-output mapping | Spam detection, image classification |
| **Unsupervised** | Unlabeled data — find patterns and structure | Customer segmentation, anomaly detection |
| **Reinforcement** | Agent learns through rewards/punishments | Game playing, robotics |
| **Transfer learning** | Reuse a pre-trained model for a new task | Fine-tuning LLMs |

### ML in the Developer Workflow

- **Data preparation** — cleaning, normalization, feature engineering
- **Model selection** — choosing the right algorithm for the task
- **Training** — the model learns from data
- **Evaluation** — measuring accuracy, precision, recall
- **Deployment** — serving predictions via API
- **Monitoring** — tracking model drift and performance

## Glossary

| Term | Definition |
|------|------------|
| Model | A mathematical representation of patterns learned from data |
| Training | The process of teaching a model using data |
| Feature | An individual measurable property of the data being used |
| Supervised learning | Learning from labeled input-output pairs |

## Next Steps

- [Supervised Learning](../beginner/supervised-learning.md) — regression and classification fundamentals
- [Neural Networks](../beginner/neural-networks.md) — perceptrons, activation functions, and backpropagation
