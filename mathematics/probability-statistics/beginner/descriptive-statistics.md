# Descriptive Statistics

## Description

Descriptive statistics summarize and describe the main features of a dataset using numbers and visualizations. They are the first step in any data analysis — before ML, before inference, before decisions. Every developer should know how to measure central tendency, spread, and correlation.

## Prerequisites

- [Basic Probability](basic-probability.md)

## Table of Contents

- [Central Tendency](#central-tendency)
- [Spread and Variability](#spread-and-variability)
- [Percentiles and Box Plots](#percentiles-and-box-plots)
- [Correlation](#correlation)

## Content / Material

### Central Tendency

| Measure | Definition | When to Use |
|---------|------------|-------------|
| **Mean** | Sum of values divided by count | Symmetric data, no outliers |
| **Median** | Middle value when sorted | Skewed data, has outliers |
| **Mode** | Most frequent value | Categorical data |

```python
import numpy as np
data = [2, 3, 5, 5, 7, 8, 100]

mean = np.mean(data)      # 18.57 (pulled by outlier)
median = np.median(data)  # 5.0 (resistant to outlier)
```

**Rule**: if mean and median differ significantly, your data is skewed or has outliers.

### Spread and Variability

| Measure | What It Tells You |
|---------|-------------------|
| **Variance** | Average squared distance from mean |
| **Standard deviation** | Square root of variance — same units as data |
| **Range** | Max - min |
| **IQR** | 75th percentile - 25th percentile (robust to outliers) |

```python
variance = np.var(data)      # 1203.7
std_dev = np.std(data)       # 34.69
q75, q25 = np.percentile(data, [75, 25])
iqr = q75 - q25
```

### Percentiles and Box Plots

Percentiles divide sorted data into 100 equal parts:

- **P50** = median
- **P25** = first quartile (Q1)
- **P75** = third quartile (Q3)
- **P99** = 99th percentile (important for latency: "P99 latency is 200ms")

```python
percentiles = np.percentile(data, [25, 50, 75, 99])
```

Box plots visualize the five-number summary: min, Q1, median, Q3, max.

### Correlation

Correlation measures how two variables move together:

$$ r = \frac{\sum (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum (x_i - \bar{x})^2 \sum (y_i - \bar{y})^2}} $$

```python
height = [160, 170, 180, 190]
weight = [55, 70, 75, 85]
r = np.corrcoef(height, weight)[0, 1]  # ≈ 0.98 (strong positive)
```

- $r = 1$: perfect positive correlation
- $r = -1$: perfect negative correlation
- $r = 0$: no linear correlation

**Remember**: correlation does not imply causation.

## Glossary

| Term | Definition |
|------|------------|
| Mean | The arithmetic average of values |
| Standard deviation | A measure of the spread of values around the mean |
| Percentile | The value below which a given percentage of data falls |
| Correlation | A measure of linear relationship between two variables |

## Next Steps

- [Distributions](distributions.md) — probability distributions and their properties
