# Rate-Distortion Theory

## Description

Rate-distortion theory characterizes the fundamental trade-off between compression rate and reconstruction fidelity for lossy compression. Given a source and a distortion measure, the rate-distortion function $R(D)$ specifies the minimum number of bits per symbol needed to reconstruct the source within an average distortion $D$. This theory underlies JPEG, MP3, video codecs, and neural network compression.

## Prerequisites

- [Source Coding](source-coding.md) — lossless compression, entropy, prefix codes, Kraft inequality
- [Channel Capacity](channel-capacity.md) — mutual information, Shannon's channel coding theorem, AWGN channel

## Table of Contents

- [Lossy Compression and Distortion](#lossy-compression-and-distortion)
- [Rate-Distortion Function](#rate-distortion-function)
- [Distortion Measures](#distortion-measures)
- [Shannon's Rate-Distortion Theorem](#shannons-rate-distortion-theorem)
- [The Rate-Distortion Function for Common Sources](#the-rate-distortion-function-for-common-sources)
- [Scalar Quantization](#scalar-quantization)
- [Vector Quantization](#vector-quantization)
- [The Blahut-Arimoto Algorithm for Rate-Distortion](#the-blahut-arimoto-algorithm-for-rate-distortion)
- [Rate-Distortion in Transform Coding](#rate-distortion-in-transform-coding)
- [Applications in Computing](#applications-in-computing)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Lossy Compression and Distortion

Lossless compression (source coding) is bounded by entropy — the data can be perfectly recovered. Lossy compression relaxes this requirement: the reconstructed data may differ from the original, as long as the difference is acceptable to the application.

**Examples of lossy compression:**

- JPEG: discards high-frequency information invisible to the human eye
- MP3: removes audio frequencies masked by louder sounds
- Video codecs (H.264, H.265): drop subtle spatial and temporal details
- Neural network quantization: reduces numerical precision of weights

The **rate-distortion trade-off** is fundamental: higher compression (lower rate) causes higher distortion. The goal is to find the minimal rate for a given distortion tolerance, or the minimal distortion for a given rate.

**Formal setup:**

- Source: random variable $X \sim p(x)$ with alphabet $\mathcal{X}$
- Reconstruction: $\hat{X}$ with alphabet $\hat{\mathcal{X}}$
- Distortion measure: $d(x, \hat{x})$, a non-negative function
- Expected distortion: $\mathbb{E}[d(X, \hat{X})]$

The encoder maps the source $x$ to an index $m \in \{1, \dots, 2^{nR}\}$, and the decoder maps the index to a reconstruction $\hat{x}$. The expected distortion must not exceed $D$.

### Rate-Distortion Function

The rate-distortion function $R(D)$ is the minimum rate required to achieve average distortion no greater than $D$:

$$
R(D) = \min_{p(\hat{x}|x): \mathbb{E}[d(X, \hat{X})] \leq D} I(X; \hat{X})
$$

The minimization is over all conditional distributions $p(\hat{x} | x)$ such that the expected distortion constraint is satisfied.

**Properties of $R(D)$:**

- **Non-increasing:** $R(D_1) \geq R(D_2)$ for $D_1 < D_2$ (more distortion allows lower rate)
- **Convex:** $R(\lambda D_1 + (1-\lambda)D_2) \leq \lambda R(D_1) + (1-\lambda)R(D_2)$
- **Zero distortion:** $R(0) = H(X)$ (at zero distortion, it equals lossless compression)
- **Infinite distortion:** $R(D) \to 0$ as $D \to \infty$
- **Non-negative:** $R(D) \geq 0$ for all $D$

**The distortion-rate function $D(R)$** is the inverse: the minimal distortion achievable at rate $R$.

There is a **dual relationship** between rate-distortion and channel capacity. In channel capacity, we fix $p(y|x)$ and maximize $I(X; Y)$ over $p(x)$. In rate-distortion, we fix $p(x)$ and minimize $I(X; \hat{X})$ over $p(\hat{x}|x)$ subject to a distortion constraint.

### Distortion Measures

The choice of distortion measure is problem-dependent. Common measures:

**Hamming distortion (for discrete alphabets):**

$$
d(x, \hat{x}) = \begin{cases}
0 & \text{if } x = \hat{x} \\
1 & \text{if } x \neq \hat{x}
\end{cases}
$$

Expected Hamming distortion equals the probability of error: $\mathbb{E}[d] = P(\hat{X} \neq X)$.

**Squared error distortion (for continuous alphabets):**

$$
d(x, \hat{x}) = (x - \hat{x})^2
$$

This is the most widely used distortion measure, analogous to mean squared error (MSE). For vectors:

$$
d(\mathbf{x}, \hat{\mathbf{x}}) = \|\mathbf{x} - \hat{\mathbf{x}}\|^2 = \sum_{i=1}^{k} (x_i - \hat{x}_i)^2
$$

**Absolute error (L1) distortion:**

$$
d(x, \hat{x}) = |x - \hat{x}|
$$

More robust to outliers than squared error.

**Perceptual distortion measures.** For audio and images, psychoacoustic and psychovisual models define distortion that correlates with human perception. These include:

- Structural Similarity Index (SSIM) for images
- Perceptual Evaluation of Audio Quality (PEAQ)
- Weighted MSE where weights depend on frequency content

### Shannon's Rate-Distortion Theorem

The rate-distortion theorem, proven by Shannon in 1959, establishes $R(D)$ as the fundamental limit:

> For a discrete memoryless source $X \sim p(x)$ and distortion measure $d(x, \hat{x})$:
>
> - **Achievability:** For any $R > R(D)$, there exists a sequence of rate-$R$ codes with average distortion $\leq D$.
>
> - **Converse:** For any rate-$R$ code with average distortion $\leq D$, we must have $R \geq R(D)$.

**Proof sketch (achievability):** Similar to the channel coding theorem, the proof uses random coding and joint typicality:

1. Generate $2^{nR}$ reconstruction sequences $\hat{X}^n$ at random according to $p(\hat{x})$.
2. Encode a source sequence $X^n$ by finding an index $m$ such that $(X^n, \hat{X}^n(m))$ are jointly typical.
3. If no such index exists, send a default index.

The probability that no jointly typical reconstruction exists vanishes when $R > I(X; \hat{X})$.

**Converse:** For any code with average distortion $\leq D$:

$$
nR \geq I(X^n; \hat{X}^n) = \sum_{i=1}^n I(X_i; \hat{X}_i) \geq n R(D)
$$

using the chain rule, data processing inequality, and convexity of $R(D)$.

### The Rate-Distortion Function for Common Sources

**Memoryless Gaussian source $X \sim \mathcal{N}(0, \sigma^2)$ with squared error:**

$$
R(D) = \begin{cases}
\frac{1}{2} \log_2 \frac{\sigma^2}{D} & 0 \leq D \leq \sigma^2 \\
0 & D > \sigma^2
\end{cases}
$$

**Distortion-rate form:**

$$
D(R) = \sigma^2 \cdot 2^{-2R}
$$

Each additional bit per sample reduces the MSE by a factor of 4 (6 dB). This is the **6 dB rule** for high-rate quantization:

```python
import numpy as np

def gaussian_rate_distortion(sigma2, D):
    if D >= sigma2:
        return 0.0
    return 0.5 * np.log2(sigma2 / D)

def gaussian_distortion_rate(sigma2, R):
    return sigma2 * 2**(-2 * R)

sigma2 = 1.0
for R in [0.5, 1, 2, 3, 4]:
    D = gaussian_distortion_rate(sigma2, R)
    print(f"R={R} bit/sample: D_min={D:.6f} (SNR={10*np.log10(sigma2/D):.2f} dB)")
```

**Memoryless Laplacian source (common for audio and DCT coefficients):**

For $p(x) = \frac{\lambda}{2} e^{-\lambda |x|}$:

$$
R(D) \approx \log_2 \left( \frac{\sqrt{2}\sigma}{D} \right) \quad \text{for high rates}
$$

**Bernoulli source with Hamming distortion:**

For $X \sim \text{Bernoulli}(p)$:

$$
R(D) = \begin{cases}
H_b(p) - H_b(D) & 0 \leq D \leq \min(p, 1-p) \\
0 & D \geq \min(p, 1-p)
\end{cases}
$$

where $H_b(p) = -p \log_2 p - (1-p) \log_2 (1-p)$.

```python
from scipy.special import entr
import math

def bernoulli_rate_distortion(p, D):
    h_p = -p * math.log2(p) - (1-p) * math.log2(1-p)
    h_D = -D * math.log2(D) - (1-D) * math.log2(1-D) if 0 < D < 1 else 0
    if D >= min(p, 1-p):
        return 0.0
    return h_p - h_D

for D in [0.0, 0.05, 0.1, 0.2, 0.4]:
    print(f"Bernoulli(p=0.3), D={D}: R={bernoulli_rate_distortion(0.3, D):.4f}")
```

### Scalar Quantization

Quantization maps a continuous value to a discrete set of reconstruction levels. Scalar quantization operates on individual samples.

**Uniform quantizer:** Partition the real line into intervals of equal length $\Delta$:

$$
Q(x) = \text{round}\left(\frac{x}{\Delta}\right) \cdot \Delta
$$

For a uniformly distributed source over $[-a, a]$ with $M$ quantization levels:

- Step size: $\Delta = 2a / M$
- Rate: $R = \log_2 M$ bits per sample
- Distortion (MSE): $D = \Delta^2 / 12$

```python
def uniform_quantize(x, delta):
    return np.round(x / delta) * delta

def uniform_quantization_distortion(a, M):
    delta = 2 * a / M
    return delta**2 / 12

a = 1.0
for M in [2, 4, 8, 16, 256]:
    D = uniform_quantization_distortion(a, M)
    R = np.log2(M)
    print(f"M={M}, R={R:.1f}, D={D:.6f}, SNR={10*np.log10(a**2/3/D):.2f} dB")
```

**Optimal (Lloyd-Max) quantizer:** For a non-uniform source, the optimal quantizer minimizes MSE by iteratively updating decision boundaries and reconstruction levels:

```python
def lloyd_max(source_samples, M, max_iter=100):
    # Initialize uniformly
    x_min, x_max = source_samples.min(), source_samples.max()
    levels = np.linspace(x_min, x_max, M)

    for iteration in range(max_iter):
        # Assign each sample to nearest level
        distances = np.abs(source_samples[:, np.newaxis] - levels[np.newaxis, :])
        assignments = np.argmin(distances, axis=1)

        # Update levels to centroid of assigned samples
        new_levels = np.array([
            source_samples[assignments == i].mean() if (assignments == i).sum() > 0
            else levels[i]
            for i in range(M)
        ])

        if np.max(np.abs(new_levels - levels)) < 1e-6:
            break
        levels = new_levels

    return levels

# Example: Gaussian source
source = np.random.randn(10000)
levels = lloyd_max(source, 8)
print("Optimal levels for N(0,1) with 8 levels:")
print(np.sort(levels))
```

**High-rate approximation (Bennett's formula):** For a source with density $p(x)$ and quantizer with level density $\lambda(x)$:

$$
D \approx \frac{1}{12} \int \frac{p(x)}{\lambda(x)^2} \, dx
$$

At high rates, the optimal level density is proportional to $p(x)^{1/3}$, giving:

$$
D \approx \frac{1}{12} \left( \int p(x)^{1/3} \, dx \right)^3 \cdot 2^{-2R}
$$

### Vector Quantization

Vector quantization (VQ) quantizes blocks of samples together. By Shannon's rate-distortion theorem, VQ can approach $R(D)$ arbitrarily closely as the block dimension increases.

**Advantages over scalar quantization:**

- Exploits correlation between samples
- Achieves lower distortion for the same rate
- Can use the shape of the distribution in high dimensions

**Linde-Buzo-Gray (LBG) algorithm (k-means clustering for VQ):**

```python
from sklearn.cluster import KMeans

def train_vq_codebook(training_vectors, K):
    kmeans = KMeans(n_clusters=K, random_state=42, n_init=10)
    kmeans.fit(training_vectors)
    return kmeans.cluster_centers_

def vq_encode(vectors, codebook):
    distances = np.linalg.norm(vectors[:, np.newaxis] - codebook[np.newaxis, :], axis=2)
    return np.argmin(distances, axis=1)

# Create training data: 2D correlated Gaussian
n = 10000
mean = [0, 0]
cov = [[1, 0.8], [0.8, 1]]
data = np.random.multivariate_normal(mean, cov, n)

K = 16  # 4-bit VQ
codebook = train_vq_codebook(data, K)
print(f"Codebook shape: {codebook.shape}")

# Test on new data
test_data = np.random.multivariate_normal(mean, cov, 1000)
indices = vq_encode(test_data, codebook)
reconstruction = codebook[indices]
mse = np.mean(np.sum((test_data - reconstruction)**2, axis=1))
print(f"VQ MSE at {np.log2(K):.1f} bits/dim: {mse:.4f}")
```

**The curse of dimensionality:** VQ codebook size grows exponentially with dimension, making full-search VQ impractical above dimension ~10. Structured VQ methods (tree-structured VQ, split VQ, product quantization) address this.

### The Blahut-Arimoto Algorithm for Rate-Distortion

The Blahut-Arimoto algorithm iteratively computes $R(D)$ by alternating between updating the conditional distribution $p(\hat{x}|x)$ and the marginal $p(\hat{x})$:

```python
def blahut_arimoto_RD(p_x, d_matrix, beta, max_iter=1000, tol=1e-10):
    """Compute R(D) at slope beta for a discrete source.
    beta = -dR/dD (the Lagrange multiplier).
    """
    n_src = len(p_x)
    n_rec = d_matrix.shape[1]

    # Initialize
    p_hat_given_x = np.ones((n_src, n_rec)) / n_rec
    p_hat = p_x @ p_hat_given_x

    for it in range(max_iter):
        # Update p_hat_given_x
        for i in range(n_src):
            log_num = np.log(p_hat + 1e-15) - beta * d_matrix[i, :]
            log_num = log_num - np.max(log_num)  # normalize
            num = np.exp(log_num)
            p_hat_given_x[i, :] = num / num.sum()

        # Update p_hat
        p_hat_new = p_x @ p_hat_given_x

        if np.max(np.abs(p_hat_new - p_hat)) < tol:
            break
        p_hat = p_hat_new

    # Compute rate and distortion
    p_xy = p_hat_given_x * p_x[:, np.newaxis]
    I = np.sum(p_xy * np.log2(p_xy / (p_hat[np.newaxis, :] * p_x[:, np.newaxis]) + 1e-15))
    D = np.sum(p_xy * d_matrix)
    return I, D

# Example: 3-symbol source, Hamming distortion
p_x = np.array([0.5, 0.3, 0.2])
d_matrix = 1 - np.eye(3)  # Hamming: 0 on diagonal, 1 off

betas = [0.5, 1, 2, 5, 10]
for beta in betas:
    R, D = blahut_arimoto_RD(p_x, d_matrix, beta)
    print(f"beta={beta:.1f}: R={R:.4f}, D={D:.4f}")
```

### Rate-Distortion in Transform Coding

Most practical lossy codecs use **transform coding**: apply an orthogonal transform, then quantize coefficients independently, allocating bits according to each coefficient's variance.

**The Karhunen-Loeve transform (KLT)** (also called PCA) decorrelates the source, making independent quantization near-optimal:

1. Compute covariance matrix $\Sigma$ of source blocks.
2. Apply eigen-decomposition: $\Sigma = U \Lambda U^T$.
3. Transform: $Y = U^T X$ (coefficients are uncorrelated).
4. Allocate $R_i$ bits to coefficient $i$ with variance $\lambda_i$:

$$
R_i = \frac{1}{2} \log_2 \frac{\lambda_i}{\prod_j \lambda_j^{1/n}} + \frac{R}{n}
$$

or equivalently, using water-filling:

$$
R_i = \max\left(0, \frac12 \log_2 \frac{\lambda_i}{\theta}\right) \quad \text{where} \quad \sum_i R_i = nR
$$

```python
def optimal_bit_allocation(eigenvalues, total_rate):
    n = len(eigenvalues)
    # Water-filling
    lo, hi = 0, max(eigenvalues) * 10
    for _ in range(100):
        theta = (lo + hi) / 2
        rates = np.maximum(0, 0.5 * np.log2(eigenvalues / theta))
        if np.sum(rates) < total_rate:
            hi = theta
        else:
            lo = theta
    return rates

eigvals = np.array([4.0, 1.0, 0.5, 0.1])
total_R = 4.0
rates = optimal_bit_allocation(eigvals, total_R)
print(f"Optimal bit allocation: {rates}")
print(f"Total rate: {np.sum(rates):.2f}")
```

**Practical transforms:**

- **DCT (Discrete Cosine Transform):** Used in JPEG, MPEG, H.264. Near-optimal for natural images (approximates KLT for AR(1) processes).
- **Wavelet transform:** Used in JPEG 2000. Better for images with discontinuities.
- **MDCT (Modified DCT):** Used in MP3, AAC, Vorbis. Overlapping transform for audio.

### Applications in Computing

**JPEG image compression.** JPEG divides the image into $8 \times 8$ blocks, applies DCT, quantizes coefficients using a quantization table, and Huffman/RLE encodes the result:

```python
import numpy as np
from scipy.fftpack import dct, idct

def jpeg_block_encode(block, Q=50):
    # Standard JPEG quantization table for quality Q
    base_q = np.array([
        [16, 11, 10, 16, 24, 40, 51, 61],
        [12, 12, 14, 19, 26, 58, 60, 55],
        [14, 13, 16, 24, 40, 57, 69, 56],
        [14, 17, 22, 29, 51, 87, 80, 62],
        [18, 22, 37, 56, 68, 109, 103, 77],
        [24, 35, 55, 64, 81, 104, 113, 92],
        [49, 64, 78, 87, 103, 121, 120, 101],
        [72, 92, 95, 98, 112, 100, 103, 99]
    ])
    # Scale quantization table
    if Q < 50:
        scale = 5000 / Q
    else:
        scale = 200 - 2 * Q
    q_table = np.maximum(1, (base_q * scale + 50) / 100)

    # DCT, quantize, dequantize, inverse DCT
    dct_block = dct(dct(block, axis=0), axis=1)
    quantized = np.round(dct_block / q_table)
    dequantized = quantized * q_table
    idct_block = idct(idct(dequantized, axis=0), axis=1)

    mse = np.mean((block - idct_block)**2)
    rate_estimate = np.sum(quantized != 0)  # rough
    return idct_block, mse, rate_estimate

block = np.random.randn(8, 8) * 100 + 128
reconstructed, mse, rate = jpeg_block_encode(block, Q=75)
print(f"JPEG quality=75: MSE={mse:.2f}, non-zero coefficients={rate}")
```

**MP3 audio compression.** MP3 uses a psychoacoustic model to determine the just-noticeable distortion for each frequency band. Bits are allocated to minimize perceived distortion:

- MDCT transform with 576 frequency lines
- Bit allocation based on the psychoacoustic masking threshold
- Huffman coding of quantized coefficients

**Video codecs (H.264, H.265).** Video compression adds temporal prediction (motion compensation) to spatial transform coding:

1. Partition frame into macroblocks ($16 \times 16$ pixels).
2. For each block, find the best match in reference frames.
3. Compute residual (difference), apply DCT, quantize.
4. Entropy code the quantized coefficients and motion vectors.

Rate-distortion optimization chooses the encoding parameters (block size, motion vector precision, quantization step) that minimize:

$$
J = D + \lambda R
$$

where $\lambda$ controls the trade-off.

**Neural network compression.** Deep learning models are compressed using rate-distortion principles:

- Weight quantization: reduce from FP32 to INT8 or binary (1 bit)
- Weight pruning: set small weights to zero (sparsity)
- Knowledge distillation: train smaller student network to match larger teacher

```python
def uniform_quantize_weights(weights, bits=8):
    max_val = np.max(np.abs(weights))
    scale = (2**(bits-1) - 1) / max_val
    quantized = np.round(weights * scale).astype(np.int8)
    return quantized, scale

def quantize_and_evaluate(model_weights, bits):
    quantized, scale = uniform_quantize_weights(model_weights, bits)
    reconstructed = quantized.astype(np.float32) / scale
    mse = np.mean((model_weights - reconstructed)**2)
    R = bits  # bits per weight
    D = mse
    print(f"Quantization: R={R} bits, D={D:.6f}, SNR={10*np.log10(np.var(model_weights)/D):.2f} dB")
    return reconstructed

weights = np.random.randn(100000)  # simplified weight vector
for bits in [1, 2, 4, 8]:
    quantize_and_evaluate(weights, bits)
```

## Glossary

| Term | Definition |
|---|---|
| Rate-distortion function | Minimum rate to achieve average distortion $\leq D$, $R(D) = \min I(X; \hat{X})$ |
| Distortion measure | Non-negative function $d(x, \hat{x})$ quantifying reconstruction error |
| Mean squared error | $d(x, \hat{x}) = (x - \hat{x})^2$, the most common distortion measure |
| Hamming distortion | $d(x, \hat{x}) = 0$ if $x = \hat{x}$, $1$ otherwise |
| Scalar quantization | Mapping a continuous scalar to one of $M$ discrete levels |
| Vector quantization | Mapping a block of samples to a codeword in a codebook |
| Lloyd-Max algorithm | Iterative algorithm for optimal scalar quantizer design |
| LBG algorithm | K-means clustering for vector quantizer design |
| Transform coding | Decorrelating by linear transform before quantization |
| Karhunen-Loeve transform (KLT) | Optimal linear transform for Gaussian sources (PCA) |
| Discrete cosine transform (DCT) | Near-optimal transform used in JPEG, MPEG |
| Water-filling | Optimal bit allocation across independent subchannels |
| Blahut-Arimoto algorithm | Iterative computation of rate-distortion function |
| Rate-distortion optimization | Minimizing $J = D + \lambda R$ in practical codecs |
| Psychoacoustic model | Perceptual distortion model for audio compression |

## Quick References

- [Shannon, C. E. (1959). Coding Theorems for a Discrete Source with a Fidelity Criterion](https://doi.org/10.1109/PROC.1959.28788) — the original rate-distortion paper
- [Berger, T. (1971). Rate Distortion Theory](https://link.springer.com/book/9780137531035) — the first comprehensive monograph on rate-distortion
- [Cover, T. M. & Thomas, J. A. (2006). Elements of Information Theory](https://www.wiley.com/en-us/Elements+of+Information+Theory%2C+2nd+Edition-p-9780471241959) — chapter 10 on rate-distortion theory
- [Gersho, A. & Gray, R. M. (1992). Vector Quantization and Signal Compression](https://link.springer.com/book/9780792391814) — comprehensive reference on quantization theory

## Next Steps

- [Error-Correcting Codes](error-correcting-codes.md) — protecting compressed data against channel noise, essential when lossy compression is followed by transmission over a noisy channel
- [Channel Capacity](channel-capacity.md) — the dual problem: given a channel, what is the maximum rate for reliable communication
