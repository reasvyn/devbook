# Descriptive Statistics

## Description

Descriptive statistics summarize and visualize data — the first step in any data analysis. Measures of central tendency, dispersion, and shape reveal the story hidden in raw numbers. Exploratory Data Analysis (EDA), data quality checks, and outlier detection all rely on descriptive statistics before any modeling begins.

## Prerequisites

- [Random Variables](random-variables.md) — expectation, variance, distributions

## Table of Contents

- [The Role of Descriptive Statistics](#the-role-of-descriptive-statistics)
- [Measures of Central Tendency](#measures-of-central-tendency)
- [Measures of Dispersion](#measures-of-dispersion)
- [Quartiles and Percentiles](#quartiles-and-percentiles)
- [The Five-Number Summary](#the-five-number-summary)
- [Visualizing Distributions](#visualizing-distributions)
- [Shape: Skewness and Kurtosis](#shape-skewness-and-kurtosis)
- [Correlation Coefficient](#correlation-coefficient)
- [Data Quality Checks](#data-quality-checks)
- [Outlier Detection](#outlier-detection)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Role of Descriptive Statistics

Descriptive statistics are the foundation of every data analysis project. Before building models or running tests, you must understand your data.

**Tasks that rely on descriptive statistics:**
- Checking for missing values and anomalies
- Understanding the scale and distribution of features
- Detecting outliers and data entry errors
- Comparing groups before formal testing
- Communicating findings with summary tables and plots

**Inferential vs. descriptive:** Descriptive statistics describe the sample; inferential statistics draw conclusions about the population.

### Measures of Central Tendency

**Mean (arithmetic mean):** $\bar{x} = \frac{1}{n} \sum_{i=1}^n x_i$.

The mean is the balance point of the data. It is sensitive to outliers — a single extreme value can pull the mean arbitrarily.

**Weighted mean:** $\bar{x}_w = \frac{\sum w_i x_i}{\sum w_i}$.

**Trimmed mean:** Remove the top and bottom $k\%$ of observations, then compute the mean. Reduces outlier influence.

**Median:** The middle value when data is sorted. The 50th percentile. Robust to outliers — the median of $(1, 2, 3, 4, 1000)$ is 3, while the mean is 202.

**Mode:** The most frequent value. Useful for categorical data and discrete distributions. A distribution can be unimodal, bimodal, or multimodal.

**Choosing a measure:**
- Symmetric data without outliers: mean (most efficient)
- Skewed data or with outliers: median (robust)
- Categorical data: mode
- When comparing groups: mean is standard for inference; median for robust comparisons

```python
import numpy as np
from scipy import stats

np.random.seed(42)
data = np.random.exponential(2, 100)  # right-skewed

mean = np.mean(data)
median = np.median(data)
mode = stats.mode(np.round(data, 1))[0][0]

print(f"Mean:   {mean:.3f}")
print(f"Median: {median:.3f}")
print(f"Mode:   {mode:.3f}")

# Add an outlier
data_with_outlier = np.append(data, 100)
print(f"\nWith outlier:")
print(f"Mean:   {np.mean(data_with_outlier):.3f}")
print(f"Median: {np.median(data_with_outlier):.3f}")
```

### Measures of Dispersion

**Range:** $\max(x) - \min(x)$. Simple but extremely sensitive to outliers.

**Interquartile Range (IQR):** $Q_3 - Q_1$. Contains the middle 50% of data. Robust.

**Variance:** $s^2 = \frac{1}{n-1} \sum_{i=1}^n (x_i - \bar{x})^2$.

The sample variance uses $n-1$ in the denominator (Bessel's correction) to make it an unbiased estimator of the population variance. The $n-1$ accounts for the fact that we used the sample mean, which is itself estimated from the data.

**Standard deviation:** $s = \sqrt{s^2}$. In the same units as the data.

**Why $n-1$?** The sample mean minimizes the sum of squared deviations from itself. This makes the average squared deviation from the sample mean slightly smaller than from the population mean. The $n-1$ correction adjusts for this bias.

```python
import numpy as np

data = np.array([2, 4, 6, 8, 10])
var_ddof0 = np.var(data, ddof=0)  # population variance (divide by n)
var_ddof1 = np.var(data, ddof=1)  # sample variance (divide by n-1)

print(f"Population variance: {var_ddof0:.2f}")
print(f"Sample variance:     {var_ddof1:.2f}")
print(f"Standard deviation:  {np.std(data, ddof=1):.2f}")
print(f"IQR:                 {stats.iqr(data):.2f}")
```

**Coefficient of Variation (CV):** $CV = s / \bar{x}$. Unitless, useful for comparing variability across different scales.

**Mean Absolute Deviation (MAD):** $\text{MAD} = \frac{1}{n} \sum |x_i - \bar{x}|$. More robust to outliers than standard deviation.

**Median Absolute Deviation:** $\text{MAD}_m = \text{median}(|x_i - \text{median}(x)|)$. Very robust.

```python
from scipy import stats

np.random.seed(42)
data = np.random.normal(100, 15, 100)
data_with_outlier = np.append(data, 500)

print(f"Std without outlier:  {np.std(data, ddof=1):.2f}")
print(f"Std with outlier:    {np.std(data_with_outlier, ddof=1):.2f}")
print(f"MAD without outlier: {stats.median_abs_deviation(data):.2f}")
print(f"MAD with outlier:    {stats.median_abs_deviation(data_with_outlier):.2f}")
```

### Quartiles and Percentiles

**Percentiles:** The $p$-th percentile is the value below which $p\%$ of the data falls.

**Quartiles:** Divide data into four equal parts.
- $Q_1$ (25th percentile): First quartile
- $Q_2$ (50th percentile): Median
- $Q_3$ (75th percentile): Third quartile

**Computing percentiles:**

The $p$-th percentile is found at rank $R = p/100 \times (n + 1)$.

If $R$ is not an integer, interpolate between adjacent values. Different software uses different interpolation methods (linear, lower, higher, midpoint).

```python
import numpy as np

np.random.seed(42)
data = np.random.exponential(3, 100)

percentiles = [5, 25, 50, 75, 95]
values = np.percentile(data, percentiles)

for p, v in zip(percentiles, values):
    print(f"{p:2d}th percentile: {v:.2f}")

print(f"Q1 (25%): {np.percentile(data, 25):.2f}")
print(f"Q3 (75%): {np.percentile(data, 75):.2f}")
print(f"IQR:      {stats.iqr(data):.2f}")
```

### The Five-Number Summary

The five-number summary provides a complete picture of a distribution in five numbers:

$$\text{Minimum}, Q_1, \text{Median}, Q_3, \text{Maximum}.$$

This is the basis for box plots. The box spans $Q_1$ to $Q_3$, the line inside is the median, and the whiskers extend to the most extreme points within $1.5 \times \text{IQR}$ of the box. Points beyond are plotted individually as potential outliers.

```python
import numpy as np
import pandas as pd

np.random.seed(42)
data = np.concatenate([
    np.random.normal(0, 1, 95),
    np.random.normal(5, 1, 5)  # outliers in a separate cluster
])

df = pd.DataFrame(data, columns=['value'])
print(df.describe())
```

Output (approximately):
```
           value
count  100.000000
mean     0.250000
std      1.500000
min     -2.500000
25%     -0.600000
50%      0.100000
75%      0.700000
max      6.800000
```

### Visualizing Distributions

**Histogram:** Partition the data range into bins and count observations per bin. The choice of bin width dramatically affects the histogram's appearance.

**Sturges' rule:** $k = \lceil \log_2(n) + 1 \rceil$ bins.
**Freedman-Diaconis rule:** $h = 2 \times \text{IQR} / \sqrt[3]{n}$.

```python
import numpy as np
import matplotlib.pyplot as plt

np.random.seed(42)
data = np.random.exponential(2, 1000)

# Using Freedman-Diaconis for bin width
iqr = stats.iqr(data)
h = 2 * iqr / (len(data) ** (1/3))
n_bins = int((data.max() - data.min()) / h)

print(f"Freedman-Diaconis bins: {n_bins}")
```

**Kernel Density Estimate (KDE):** A smoothed version of the histogram. Places a kernel (often Gaussian) at each data point and sums them.

$$\hat{f}_h(x) = \frac{1}{nh} \sum_{i=1}^n K\left(\frac{x - x_i}{h}\right).$$

The bandwidth $h$ controls smoothness — too small gives a noisy estimate, too large oversmooths.

**Box plots:** Show the five-number summary with outliers. Useful for comparing multiple groups.

**Violin plots:** Combine box plots with KDE. Show the full distribution shape.

```python
import seaborn as sns
import pandas as pd

np.random.seed(42)
df = pd.DataFrame({
    'group': np.repeat(['A', 'B', 'C'], 200),
    'value': np.concatenate([
        np.random.normal(0, 1, 200),
        np.random.normal(1, 1.5, 200),
        np.random.exponential(2, 200)
    ])
})

# Group statistics
print(df.groupby('group')['value'].describe())
```

**Q-Q Plot (Quantile-Quantile Plot):** Plots the quantiles of the data against the quantiles of a theoretical distribution. If the data follows the theoretical distribution, the points fall roughly on a diagonal line.

Deviation from the line reveals:
- Curvature: skewness
- S-shape: heavy or light tails
- Points far from the line at extremes: outliers

```python
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt

np.random.seed(42)

# Normal data vs normal Q-Q plot
normal_data = np.random.normal(0, 1, 100)
# stats.probplot(normal_data, dist="norm", plot=plt)

# Skewed data vs normal Q-Q plot
skewed_data = np.random.exponential(2, 100)
# Expected: points deviate from line in a curve (right skew)
```

### Shape: Skewness and Kurtosis

**Skewness** measures asymmetry:

$$\gamma_1 = \frac{1}{n} \sum_{i=1}^n \left(\frac{x_i - \bar{x}}{s}\right)^3.$$

- Zero: symmetric (normal distribution)
- Positive: right tail longer (exponential, log-normal, income distributions)
- Negative: left tail longer (some bounded distributions)

**Kurtosis** measures tail heaviness:

$$\gamma_2 = \frac{1}{n} \sum_{i=1}^n \left(\frac{x_i - \bar{x}}{s}\right)^4 - 3.$$

The "minus 3" makes the normal distribution have kurtosis 0 (excess kurtosis).

- Positive (leptokurtic): heavy tails, more outliers than normal (t-distribution)
- Zero: normal tails
- Negative (platykurtic): light tails, fewer outliers than normal (uniform)

```python
import numpy as np
from scipy import stats

np.random.seed(42)

# Normal distribution
normal = np.random.normal(0, 1, 10000)
print(f"Normal: skew={stats.skew(normal):.3f}, kurt={stats.kurtosis(normal):.3f}")

# Exponential (right-skewed)
exponential = np.random.exponential(1, 10000)
print(f"Exponential: skew={stats.skew(exponential):.3f}, kurt={stats.kurtosis(exponential):.3f}")

# Uniform (light tails)
uniform = np.random.uniform(-1, 1, 10000)
print(f"Uniform: skew={stats.skew(uniform):.3f}, kurt={stats.kurtosis(uniform):.3f}")
```

Output:
```
Normal: skew=0.008, kurt=-0.016
Exponential: skew=1.994, kurt=5.985
Uniform: skew=0.008, kurt=-1.199
```

**Why these matter:**
- Skewness affects the validity of normal-based confidence intervals
- Kurtosis indicates outlier propensity
- Many ML algorithms (especially linear models) assume symmetric, light-tailed data

### Correlation Coefficient

**Pearson correlation coefficient:**

$$r = \frac{\sum_{i=1}^n (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum_{i=1}^n (x_i - \bar{x})^2 \sum_{i=1}^n (y_i - \bar{y})^2}}.$$

- Ranges from $-1$ to $1$
- Measures linear relationship only
- $r = 0$ does not imply no relationship — there may be a non-linear relationship

**Spearman rank correlation:** Pearson correlation on ranks. Measures monotonic relationships.

**Kendall's tau:** Another rank-based correlation, more robust to ties.

```python
import numpy as np
from scipy import stats

np.random.seed(42)

# Linear relationship
x = np.random.normal(0, 1, 100)
y = 2 * x + 0.5 * np.random.normal(0, 1, 100)
print(f"Pearson: {stats.pearsonr(x, y)[0]:.4f}")

# Non-linear but monotonic
x = np.linspace(0, 5, 100)
y = np.exp(x) + np.random.normal(0, 5, 100)
print(f"Pearson (non-linear):  {stats.pearsonr(x, y)[0]:.4f}")
print(f"Spearman (non-linear): {stats.spearmanr(x, y)[0]:.4f}")

# Non-monotonic relationship
x = np.linspace(-3, 3, 100)
y = x**2 + np.random.normal(0, 0.5, 100)
print(f"Pearson (quadratic):   {stats.pearsonr(x, y)[0]:.4f}")
print(f"Spearman (quadratic):  {stats.spearmanr(x, y)[0]:.4f}")
```

**Correlation matrix for multiple variables:**

```python
import pandas as pd

np.random.seed(42)
n = 100
df = pd.DataFrame({
    'age': np.random.normal(40, 10, n),
    'income': np.random.normal(60000, 15000, n),
    'education_years': np.random.poisson(14, n),
    'satisfaction': np.random.uniform(1, 10, n)
})

# Introduce some correlations
df['income'] += df['education_years'] * 2000 + np.random.normal(0, 5000, n)
df['satisfaction'] += df['income'] / 20000 + np.random.normal(0, 1, n)

print(df.corr().round(3))
```

### Data Quality Checks

**Missing values:** Check count and percentage per column. Decide on imputation or exclusion.

**Duplicates:** Identify and remove duplicate rows.

**Data type consistency:** Ensure numeric columns are numeric, categorical columns are categorical.

**Range checks:** Identify values outside expected ranges (negative ages, temperatures below absolute zero).

**Cardinality:** For categorical variables, check the number of unique values. High cardinality may indicate data entry issues.

**Distribution shifts:** Compare distributions across time periods or data sources.

```python
import pandas as pd
import numpy as np

# Simulate a dataset with quality issues
np.random.seed(42)
n = 1000
df = pd.DataFrame({
    'age': np.random.normal(35, 10, n),
    'salary': np.random.exponential(50000, n),
    'department': np.random.choice(['Engineering', 'Sales', 'HR', None], n,
                                    p=[0.4, 0.3, 0.2, 0.1]),
    'years_experience': np.random.poisson(5, n)
})

# Introduce issues
df.loc[0, 'age'] = -5  # impossible age
df.loc[1, 'salary'] = 0  # zero salary
df.loc[2, 'years_experience'] = 200  # impossible

print("Missing values:")
print(df.isnull().sum())
print("\nBasic info:")
print(df.info())
print("\nDescribe:")
print(df.describe())
```

### Outlier Detection

**Z-score method:** Flag points where $|z_i| > 3$, where $z_i = (x_i - \bar{x}) / s$.

Limitation: The mean and standard deviation themselves are influenced by outliers (masking effect).

**Modified Z-score:** Uses median and MAD: $M_i = 0.6745 \times (x_i - \text{median}(x)) / \text{MAD}$. Flag $|M_i| > 3.5$.

**IQR method:** Flag points below $Q_1 - 1.5 \times \text{IQR}$ or above $Q_3 + 1.5 \times \text{IQR}$.

**Isolation Forest:** Tree-based method for multivariate outlier detection.

**DBSCAN:** Density-based clustering that identifies points in low-density regions as outliers.

```python
import numpy as np
from scipy import stats

np.random.seed(42)

data = np.concatenate([
    np.random.normal(0, 1, 95),
    np.random.normal(0, 1, 5) * 4 + 6  # outliers
])

# Z-score method
z_scores = np.abs(stats.zscore(data))
outliers_z = np.where(z_scores > 3)[0]

# IQR method
q1, q3 = np.percentile(data, [25, 75])
iqr = q3 - q1
lower_bound = q1 - 1.5 * iqr
upper_bound = q3 + 1.5 * iqr
outliers_iqr = np.where((data < lower_bound) | (data > upper_bound))[0]

print(f"Outliers by Z-score: {len(outliers_z)}")
print(f"Outliers by IQR:     {len(outliers_iqr)}")
print(f"Indices (z-score):   {outliers_z}")
print(f"Indices (IQR):       {outliers_iqr}")
```

**Multivariate outlier detection — Mahalanobis distance:**

$$D_i = \sqrt{(x_i - \boldsymbol{\mu})^T \Sigma^{-1} (x_i - \boldsymbol{\mu})}.$$

Flags points that are unusual in multiple dimensions simultaneously, accounting for correlations.

```python
import numpy as np
from scipy import stats

np.random.seed(42)
mean = [0, 0]
cov = [[1, 0.7], [0.7, 1]]
data = np.random.multivariate_normal(mean, cov, 100)

# Add a multivariate outlier
data = np.vstack([data, [4, 4]])

# Mahalanobis distance
mu = np.mean(data, axis=0)
sigma = np.cov(data, rowvar=False)
inv_sigma = np.linalg.inv(sigma)

distances = np.array([
    np.sqrt((x - mu).T @ inv_sigma @ (x - mu)) for x in data
])

# Outliers: chi-square with 2 df, p < 0.001
threshold = np.sqrt(stats.chi2.ppf(0.999, 2))
outliers_mahalanobis = np.where(distances > threshold)[0]
print(f"Mahalanobis outliers: {len(outliers_mahalanobis)}")
print(f"Threshold: {threshold:.2f}")
print(f"Max distance: {distances.max():.2f}")
```

## Study Cases

### Case 1: EDA for Customer Churn

A dataset contains 10,000 customer records with features: tenure, monthly charges, contract type, and churn indicator.

```python
import pandas as pd
import numpy as np

np.random.seed(42)
n = 10000
df = pd.DataFrame({
    'tenure_months': np.random.exponential(24, n).astype(int),
    'monthly_charges': np.random.normal(65, 20, n),
    'contract_type': np.random.choice(['Month-to-month', 'One year', 'Two year'], n,
                                       p=[0.5, 0.3, 0.2]),
    'churned': np.random.choice([0, 1], n, p=[0.75, 0.25])
})

# Cap charges at reasonable values
df['monthly_charges'] = df['monthly_charges'].clip(0, 200)

# Summary by churn status
summary = df.groupby('churned').agg({
    'tenure_months': ['mean', 'median', 'std'],
    'monthly_charges': ['mean', 'median', 'std']
})
print("Summary by churn status:")
print(summary)

# Correlation analysis
print("\nCorrelation with churn:")
print(df.corr(numeric_only=True)['churned'].sort_values(ascending=False))
```

Key findings from descriptive analysis:
- Churned customers have shorter average tenure (8 months vs 28 months)
- Churned customers have higher monthly charges ($72 vs $62)
- Month-to-month contracts have the highest churn rate (40%)

### Case 2: Detecting Data Drift in Production

An ML model was trained on data from 2024. In 2025, the distribution of a key feature (transaction amount) may have shifted. Descriptive statistics detect the drift.

```python
import numpy as np
from scipy import stats

np.random.seed(42)
# Training data distribution
train_data = np.random.lognormal(mean=4.0, sigma=0.5, size=10000)
# Production data (shifted)
prod_data = np.random.lognormal(mean=4.3, sigma=0.6, size=5000)

print(f"Train: mean={np.mean(train_data):.1f}, sd={np.std(train_data):.1f}, " +
      f"p99={np.percentile(train_data, 99):.1f}")
print(f"Prod:  mean={np.mean(prod_data):.1f}, sd={np.std(prod_data):.1f}, " +
      f"p99={np.percentile(prod_data, 99):.1f}")

# Kolmogorov-Smirnov test for distribution shift
ks_stat, ks_p = stats.ks_2samp(train_data, prod_data)
print(f"KS test: stat={ks_stat:.4f}, p={ks_p:.4f}")
```

### Case 3: Comparing Software Performance

Two versions of an API are benchmarked with 100 runs each. Descriptive statistics reveal whether the new version is consistently faster.

```python
import numpy as np
from scipy import stats

np.random.seed(42)
v1_times = np.random.exponential(200, 100)  # old version
v2_times = np.random.exponential(180, 100)  # new version (slightly faster)

print(f"Version 1: mean={np.mean(v1_times):.1f}ms, median={np.median(v1_times):.1f}ms, " +
      f"p99={np.percentile(v1_times, 99):.1f}ms")
print(f"Version 2: mean={np.mean(v2_times):.1f}ms, median={np.median(v2_times):.1f}ms, " +
      f"p99={np.percentile(v2_times, 99):.1f}ms")

# Mean difference
diff = np.mean(v1_times) - np.mean(v2_times)
# Pooled standard error
se = np.sqrt(np.var(v1_times, ddof=1)/100 + np.var(v2_times, ddof=1)/100)
print(f"Difference: {diff:.1f}ms +/- 1.96*{se:.1f}ms")
```

## Examples

### Example 1: Symmetric vs Skewed Data

```python
import numpy as np
from scipy import stats

# Symmetric
symmetric = np.random.normal(0, 1, 1000)
print(f"Symmetric: mean={np.mean(symmetric):.2f}, median={np.median(symmetric):.2f}")
print(f"  mean - median = {np.mean(symmetric) - np.median(symmetric):.4f}")

# Right-skewed
right_skewed = np.random.exponential(1, 1000)
print(f"Right-skewed: mean={np.mean(right_skewed):.2f}, median={np.median(right_skewed):.2f}")
print(f"  mean - median = {np.mean(right_skewed) - np.median(right_skewed):.4f}")
```

Output:
```
Symmetric: mean=0.01, median=0.00
  mean - median = 0.0078
Right-skewed: mean=1.02, median=0.70
  mean - median = 0.3222
```

In symmetric distributions, mean equals median. In right-skewed, mean exceeds median.

### Example 2: Anscombe's Quartet

Anscombe's quartet demonstrates why visualization is essential. Four datasets with nearly identical descriptive statistics (mean $x$, mean $y$, variance, correlation, regression line) but dramatically different relationships.

```python
import numpy as np
from scipy import stats

# Anscombe's quartet
anscombe = {
    'I': {'x': [10, 8, 13, 9, 11, 14, 6, 4, 12, 7, 5],
          'y': [8.04, 6.95, 7.58, 8.81, 8.33, 9.96, 7.24, 4.26, 10.84, 4.82, 5.68]},
    'II': {'x': [10, 8, 13, 9, 11, 14, 6, 4, 12, 7, 5],
           'y': [9.14, 8.14, 8.74, 8.77, 9.26, 8.10, 6.13, 3.10, 9.13, 7.26, 4.74]},
    'III': {'x': [10, 8, 13, 9, 11, 14, 6, 4, 12, 7, 5],
            'y': [7.46, 6.77, 12.74, 7.11, 7.81, 8.84, 6.08, 5.39, 8.15, 6.42, 5.73]},
    'IV': {'x': [8, 8, 8, 8, 8, 8, 8, 19, 8, 8, 8],
           'y': [6.58, 5.76, 7.71, 8.84, 8.47, 7.04, 5.25, 12.50, 5.56, 7.91, 6.89]}
}

for name, data in anscombe.items():
    x, y = data['x'], data['y']
    r, _ = stats.pearsonr(x, y)
    print(f"Dataset {name}: mean_x={np.mean(x):.2f}, mean_y={np.mean(y):.2f}, " +
          f"r={r:.4f}")
```

All four datasets have $\bar{x} = 9.0$, $\bar{y} = 7.5$, $r \approx 0.816$, but their scatter plots tell very different stories.

### Example 3: Log Transformation for Skewed Data

Right-skewed data (like income or response times) often benefits from log transformation before analysis.

```python
import numpy as np
from scipy import stats

np.random.seed(42)
data = np.random.lognormal(mean=3, sigma=0.8, size=1000)

print("Original data:")
print(f"  skewness: {stats.skew(data):.2f}")
print(f"  kurtosis: {stats.kurtosis(data):.2f}")

log_data = np.log(data)
print("Log-transformed data:")
print(f"  skewness: {stats.skew(log_data):.2f}")
print(f"  kurtosis: {stats.kurtosis(log_data):.2f}")
```

### Example 4: Simpson's Paradox in Descriptive Statistics

Aggregate statistics can be misleading when groups have different sizes.

```python
import pandas as pd

# Department admissions data simulating gender bias
data = pd.DataFrame({
    'department': ['A']*100 + ['B']*100 + ['C']*100,
    'gender': ['F']*80 + ['M']*20 + ['M']*80 + ['F']*20 + ['F']*50 + ['M']*50,
    'admitted': [60, 15, 70, 15, 25, 25]  # within each gender/dep combo
})

# Overall admission rate by gender
print("Overall admission rates:")
print(data.groupby('gender')['admitted'].mean())

# Department-level admission rates
print("\nDepartment-level admission rates:")
print(data.groupby(['department', 'gender'])['admitted'].mean().unstack())
```

### Example 5: The Importance of Scaling

Variables on different scales can distort analyses like PCA, clustering, and regression.

```python
import numpy as np
from scipy import stats

np.random.seed(42)
df = pd.DataFrame({
    'age_years': np.random.normal(40, 10, 1000),
    'income_dollars': np.random.normal(60000, 20000, 1000),
    'satisfaction_score': np.random.uniform(1, 5, 1000)
})

print("Before scaling:")
print(df.std())

# Standard scaling
df_scaled = (df - df.mean()) / df.std()
print("\nAfter scaling:")
print(df_scaled.std())
```

## Glossary

| Term | Definition |
|------|------------|
| Mean | Arithmetic average of data |
| Median | Middle value when data is sorted |
| Mode | Most frequent value |
| Variance | Average squared deviation from the mean |
| Standard deviation | Square root of variance |
| IQR | Interquartile range, $Q_3 - Q_1$ |
| Range | Maximum minus minimum |
| Percentile | Value below which a given percentage of data falls |
| Five-number summary | Min, Q1, Median, Q3, Max |
| Box plot | Visual summary using the five-number summary |
| Histogram | Bar chart of binned frequencies |
| Q-Q plot | Quantile-quantile plot for distribution comparison |
| KDE | Kernel density estimate, smoothed histogram |
| Skewness | Measure of asymmetry |
| Kurtosis | Measure of tail heaviness |
| Pearson correlation | Linear correlation coefficient |
| Spearman correlation | Rank-based correlation coefficient |
| Z-score | Number of standard deviations from the mean |
| Mahalanobis distance | Multivariate distance accounting for correlation |
| Bessel's correction | $n-1$ in sample variance denominator |
| Coefficient of variation | $s / \bar{x}$, unitless dispersion measure |
| MAD | Mean/median absolute deviation |
| Freedman-Diaconis rule | Optimal histogram bin width |
| Anscombe's quartet | Four datasets with identical statistics but different distributions |

## Quick References

- [Pandas Descriptive Statistics Docs](https://pandas.pydata.org/docs/user_guide/basics.html#descriptive-statistics) — `df.describe()` and related methods
- [Scipy Stats Documentation](https://docs.scipy.org/doc/scipy/reference/stats.html) — statistical functions
- [Exploratory Data Analysis with Python, McKinney](https://wesmckinney.com/book/) — practical EDA in Python
- [Anscombe's Quartet (Wikipedia)](https://en.wikipedia.org/wiki/Anscombe%27s_quartet) — cautionary tale about summary statistics
- [Seaborn Tutorial: Statistical Estimation](https://seaborn.pydata.org/tutorial/statistical_estimation.html) — visualization-centric descriptive stats

## Next Steps

- [Estimation & Hypothesis Testing](estimation-and-testing.md) — moving from description to inference
