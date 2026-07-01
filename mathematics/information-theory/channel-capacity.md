# Channel Capacity

## Description

Channel capacity is the maximum rate at which information can be reliably transmitted over a communication channel. Shannon's noisy channel coding theorem proves that for any rate below capacity, there exists a code achieving arbitrarily low error probability, and that no code can exceed capacity without errors. Capacity governs the design of modems, wireless networks, storage systems, and satellite links.

## Prerequisites

- [Mutual Information](mutual-information.md) — mutual information, data processing inequality, conditional entropy
- [Entropy and Information](entropy-and-information.md) — entropy, joint entropy, basic probability

## Table of Contents

- [Communication Channel Model](#communication-channel-model)
- [Discrete Memoryless Channels](#discrete-memoryless-channels)
- [Channel Capacity Definition](#channel-capacity-definition)
- [Noisy Channel Coding Theorem](#noisy-channel-coding-theorem)
- [Binary Symmetric Channel](#binary-symmetric-channel)
- [Binary Erasure Channel](#binary-erasure-channel)
- [Symmetric Channels](#symmetric-channels)
- [Capacity of Common Channels](#capacity-of-common-channels)
- [Channel Coding and Decoding](#channel-coding-and-decoding)
- [The Channel Coding Gap](#the-channel-coding-gap)
- [Applications in Computing](#applications-in-computing)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Communication Channel Model

A communication channel is defined by an input alphabet $\mathcal{X}$, an output alphabet $\mathcal{Y}$, and a conditional probability distribution $p(y | x)$ that describes the probability of receiving output $y$ when transmitting input $x$.

```
Sender  ----[x]---->  Channel p(y|x)  ----[y]---->  Receiver
```

The channel is **memoryless** if the output at any time depends only on the current input and not on past inputs or outputs:

$$
p(y_1, y_2, \dots, y_n | x_1, x_2, \dots, x_n) = \prod_{i=1}^n p(y_i | x_i)
$$

**Key quantities:**

- **Input distribution:** $p(x)$, chosen by the transmitter
- **Joint distribution:** $p(x, y) = p(x) p(y | x)$
- **Output distribution:** $p(y) = \sum_x p(x) p(y | x)$
- **Mutual information:** $I(X; Y)$, the information that the output provides about the input

The goal of communication is to send a message $M \in \{1, \dots, 2^{nR}\}$ at rate $R$ bits per channel use, using $n$ channel uses, such that the receiver can decode the message with vanishing error probability as $n$ increases.

### Discrete Memoryless Channels

A **discrete memoryless channel (DMC)** is defined by the triple $(\mathcal{X}, \mathcal{Y}, P)$, where:

- $\mathcal{X}$ is a finite input alphabet (size $|\mathcal{X}|$)
- $\mathcal{Y}$ is a finite output alphabet (size $|\mathcal{Y}|$)
- $P$ is a $|\mathcal{X}| \times |\mathcal{Y}|$ channel transition matrix with entries $P_{ij} = p(y_j | x_i)$

Each row of $P$ sums to 1: $\sum_j p(y_j | x_i) = 1$ for all $i$.

**Example:** A $2 \times 3$ channel matrix:

$$
P = \begin{bmatrix}
0.8 & 0.15 & 0.05 \\
0.1 & 0.2 & 0.7
\end{bmatrix}
$$

Row 1: when input is $x_1$, output is $y_1$ with prob 0.8, $y_2$ with prob 0.15, $y_3$ with prob 0.05.

The channel capacity is computed by maximizing $I(X; Y)$ over all possible input distributions.

### Channel Capacity Definition

The capacity of a discrete memoryless channel is:

$$
C = \max_{p(x)} I(X; Y)
$$

**Units:** bits per channel use (or bits per transmission).

**Properties of capacity:**

- $0 \leq C \leq \log_2 |\mathcal{X}|$ (cannot exceed the logarithm of input alphabet size)
- $C \leq \log_2 |\mathcal{Y}|$ (cannot exceed the logarithm of output alphabet size)
- Capacity depends only on the channel transition matrix $P$, not on the actual data

**Computing capacity** typically requires solving a convex optimization problem, since $I(X; Y)$ is a concave function of $p(x)$:

$$
C = \max_{p(x)} \sum_{x, y} p(x) p(y|x) \log_2 \frac{p(y|x)}{\sum_{x'} p(x') p(y|x')}
$$

The capacity can be found using the **Blahut-Arimoto algorithm**, an iterative method that alternates between updating the input distribution and the conditional distribution.

### Noisy Channel Coding Theorem

Shannon's second theorem (the noisy channel coding theorem) is the central result of information theory:

> For a DMC with capacity $C$:
>
> - **Achievability:** For any $R < C$, there exists a sequence of $(2^{nR}, n)$ codes with maximum error probability $\lambda^{(n)} \to 0$ as $n \to \infty$.
>
> - **Converse:** For any $R > C$, the error probability of any $(2^{nR}, n)$ code is bounded away from zero for all sufficiently large $n$. Specifically, $P_e^{(n)} \geq 1 - \frac{C}{R}$.

**Interpretation:** Reliable communication is possible at any rate below capacity by using sufficiently long codes. Communication at rates above capacity is impossible — errors are inevitable regardless of code design.

**Proof sketch (achievability):** The proof uses **random coding** and **joint typicality decoding**:

1. Generate $2^{nR}$ codewords independently at random according to $p(x)$.
2. Encode message $m$ to the $m$-th codeword $X^n(m)$.
3. Transmit through the channel, receive $Y^n$.
4. Decode to message $m$ if $(X^n(m), Y^n)$ is jointly typical and no other codeword is jointly typical with $Y^n$.

By the asymptotic equipartition property, the probability that the transmitted codeword is not jointly typical with $Y^n$ goes to 0, and the probability that a different codeword is jointly typical is approximately $2^{nR} \cdot 2^{-n(I(X;Y) - \epsilon)} \to 0$ when $R < I(X;Y)$.

The converse uses Fano's inequality to show that if $R > C$, the error probability is bounded below by $(R - C)/R$.

### Binary Symmetric Channel

The **binary symmetric channel (BSC)** is the simplest noisy channel model:

- $\mathcal{X} = \{0, 1\}$, $\mathcal{Y} = \{0, 1\}$
- With probability $1 - p$, the bit is received correctly
- With probability $p$, the bit is flipped (0 becomes 1, 1 becomes 0)

```
    1-p
  0 ----> 0
  |       |
p|       |p
  v       v
  1 ----> 1
    1-p
```

**Transition matrix:**

$$
P = \begin{bmatrix}
1-p & p \\
p & 1-p
\end{bmatrix}
$$

**Capacity of BSC(p):**

$$
C_{\text{BSC}}(p) = 1 - H_b(p) = 1 + p \log_2 p + (1-p) \log_2 (1-p)
$$

where $H_b(p) = -p \log_2 p - (1-p) \log_2 (1-p)$ is the binary entropy function.

```python
import numpy as np

def bsc_capacity(p):
    if p == 0 or p == 1:
        return 1.0
    return 1 - (-p * np.log2(p) - (1 - p) * np.log2(1 - p))

for p in [0.0, 0.01, 0.05, 0.1, 0.25, 0.5]:
    print(f"BSC(p={p}): C = {bsc_capacity(p):.4f} bits/use")
```

| $p$ | Capacity (bits) |
|---|---|
| 0.0 | 1.000 |
| 0.01 | 0.919 |
| 0.05 | 0.714 |
| 0.1 | 0.531 |
| 0.25 | 0.189 |
| 0.5 | 0.000 |

When $p = 0.5$, the output is independent of the input — no information can be transmitted. The capacity-maximizing input distribution is uniform: $p(0) = p(1) = 0.5$.

### Binary Erasure Channel

The **binary erasure channel (BEC)** captures the behavior of packet-switched networks where data is either received correctly or lost:

- $\mathcal{X} = \{0, 1\}$, $\mathcal{Y} = \{0, 1, e\}$ (where $e$ denotes erasure)
- With probability $1 - \alpha$, the bit is received correctly
- With probability $\alpha$, the bit is erased (received as $e$)

```
    1-α
  0 ----> 0
  |       |
 α|       |α
  v       v
  e <-----|
  ^
 α|
  |
  1 ----> 1
    1-α
```

**Transition matrix:**

$$
P = \begin{bmatrix}
1-\alpha & \alpha & 0 \\
0 & \alpha & 1-\alpha
\end{bmatrix}
$$

**Capacity of BEC($\alpha$):**

$$
C_{\text{BEC}}(\alpha) = 1 - \alpha
```

Unlike the BSC, the BEC capacity is a simple linear function of the erasure probability. The receiver knows which bits were erased, so no information is lost on correctly received bits — only the erased bits contribute zero information.

```python
def bec_capacity(alpha):
    return 1 - alpha

for alpha in [0.0, 0.1, 0.25, 0.5, 0.9]:
    print(f"BEC(alpha={alpha}): C = {bec_capacity(alpha):.4f} bits/use")
```

### Symmetric Channels

A channel is **symmetric** if the rows of the transition matrix are permutations of each other and the columns are permutations of each other.

More formally, a channel is symmetric if:

- For each output $y$, the set of transition probabilities $\{p(y|x) : x \in \mathcal{X}\}$ is the same for all inputs up to permutation.
- For each input $x$, the set of transition probabilities $\{p(y|x) : y \in \mathcal{Y}\}$ is the same for all outputs up to permutation.

**Capacity of symmetric channels:**

For a symmetric channel, the capacity-maximizing input distribution is uniform, and:

$$
C = \log_2 |\mathcal{Y}| - H(\text{row of } P)
$$

where $H(\text{row of } P)$ is the entropy of any row of the transition matrix (all rows have the same entropy for symmetric channels).

**Example: $q$-ary symmetric channel.** Input and output alphabets of size $q$, probability $1-p$ of correct transmission and $p/(q-1)$ of transitioning to any other symbol:

$$
C = \log_2 q - \left( -(1-p) \log_2 (1-p) - p \log_2 \frac{p}{q-1} \right)
$$

For $q=4, p=0.1$:

```python
def qsc_capacity(q, p):
    row_entropy = -(1-p) * np.log2(1-p) - p * np.log2(p/(q-1))
    return np.log2(q) - row_entropy

print(f"4-ary SC capacity: {qsc_capacity(4, 0.1):.4f} bits/use")
```

### Capacity of Common Channels

**Binary symmetric channel (BSC):** $C = 1 - H_b(p)$

**Binary erasure channel (BEC):** $C = 1 - \alpha$

**Binary input, AWGN output channel (BI-AWGN):**

For binary input $X \in \{\pm 1\}$ and output $Y = X + Z$ where $Z \sim \mathcal{N}(0, \sigma^2)$:

$$
C = 1 - \int_{-\infty}^{\infty} \frac{e^{-z^2/2\sigma^2}}{\sqrt{2\pi\sigma^2}} \log_2 \left(1 + e^{-2z/\sigma^2}\right) \, dz
$$

This has no closed form but can be computed numerically.

**AWGN channel (continuous input, power constraint $P$):**

$$
C = \frac{1}{2} \log_2 \left(1 + \frac{P}{\sigma^2}\right) \quad \text{bits per channel use}
$$

This is the famous **Shannon-Hartley theorem** for the additive white Gaussian noise channel. With bandwidth $W$:

$$
C = W \log_2 \left(1 + \frac{P}{N_0 W}\right) \quad \text{bits per second}
$$

where $N_0$ is the noise power spectral density and $P$ is the signal power.

**Multiple-input multiple-output (MIMO) channel:**

For an $n_t \times n_r$ MIMO channel with channel matrix $H$:

$$
C = \log_2 \det\left(I_{n_r} + \frac{P}{n_t \sigma^2} H H^T\right)
$$

MIMO capacity grows approximately as $\min(n_t, n_r) \log_2(1 + \text{SNR})$ at high SNR.

```python
def awgn_capacity(snr_db):
    snr = 10 ** (snr_db / 10)
    return 0.5 * np.log2(1 + snr)

for snr_db in [-10, 0, 10, 20, 30]:
    print(f"AWGN SNR={snr_db}dB: C = {awgn_capacity(snr_db):.4f} bits/use")
```

### Channel Coding and Decoding

Channel coding adds structured redundancy to enable error correction. Main families:

**Block codes:** Divide data into $k$-bit blocks and map to $n$-bit codewords ($n > k$). The **code rate** is $R = k/n$.

**Convolutional codes:** Continuous encoding using shift registers, producing $n$ output bits for every $k$ input bits.

**Decoding approaches:**

- **Hard-decision decoding:** First demodulate bits, then decode (works on binary values).
- **Soft-decision decoding:** Use channel output probabilities directly for decoding (better performance, used by LDPC and turbo codes).

**Maximum a posteriori (MAP) decoding:**

Given received $y$, choose $\hat{x} = \arg\max_{x} p(x | y)$.

**Maximum likelihood (ML) decoding:**

Choose $\hat{x} = \arg\max_{x} p(y | x)$. Equivalent to MAP when the input distribution is uniform.

**Typical set decoding** is used in the achievability proof of the channel coding theorem. The decoder searches for a codeword that is jointly typical with the received sequence. This is asymptotically optimal but impractical for long codes.

### The Channel Coding Gap

The **channel coding gap** is the difference between the achievable rate of a practical coding scheme and the Shannon capacity. Modern codes (LDPC, turbo) operate within fractions of a dB of capacity:

| Code | Gap to capacity | Year |
|---|---|---|
| Hamming code | Several dB | 1950 |
| Convolutional + Viterbi | ~2-3 dB | 1967 |
| Turbo codes | ~0.5 dB | 1993 |
| LDPC codes | ~0.1-0.3 dB | 1960/1996 |
| Polar codes | ~0.1 dB (asymptotic) | 2009 |

**Finite-length effects:** At finite block lengths, code performance is bounded by the normal approximation:

$$
R \approx C - \sqrt{\frac{V}{n}} Q^{-1}(\epsilon)
$$

where $V$ is the channel dispersion, $n$ is block length, $\epsilon$ is error probability, and $Q^{-1}$ is the inverse Q-function. This is known as the **finite block length regime**.

### Applications in Computing

**Wireless communication (Wi-Fi, 5G).** Modern wireless standards use adaptive modulation and coding (AMC). The transmitter selects a modulation and coding rate based on the current channel SNR, staying as close to capacity as possible:

- 4G LTE: turbo codes, rate $\frac{1}{3}$ to $\frac{5}{6}$
- 5G NR: LDPC for data, polar codes for control
- Wi-Fi 6 (802.11ax): LDPC codes, rates from $\frac{1}{2}$ to $\frac{5}{6}$

```python
def capacity_achieving_rate(snr_db, target_bler=0.01):
    snr = 10 ** (snr_db / 10)
    C = 0.5 * np.log2(1 + snr)
    return C  # In practice, codes operate ~0.5-1 dB from capacity

snr_values = range(-5, 21)
rates = [capacity_achieving_rate(snr) for snr in snr_values]
```

**Digital subscriber lines (DSL).** DSL modems use DMT (discrete multi-tone) modulation, dividing the frequency spectrum into hundreds of narrow subchannels. Each subchannel is treated as an independent AWGN channel. The transmitter allocates bits per subchannel according to its SNR:

$$
b_i = \log_2\left(1 + \frac{P_i |H_i|^2}{\Gamma \sigma_i^2}\right)
$$

where $\Gamma$ is the SNR gap (about 9 dB for uncoded QAM).

**Data storage.** Hard disk drives (HDDs) and solid-state drives (SSDs) use channel codes to correct read errors. Modern HDDs use LDPC codes with iterative decoding. SSDs use BCH or LDPC codes depending on the NAND flash technology:

```python
def ssd_capacity_page(num_pages=128, page_size=16384, raw_ber=1e-3):
    # Simplified model: each page corrected by ECC
    bits_per_page = page_size * 8
    # Shannon capacity bound for binary channel with raw_ber
    p = raw_ber
    C = 1 - (-p * np.log2(p) - (1-p) * np.log2(1-p))
    info_bits = bits_per_page * C
    print(f"Raw BER: {raw_ber}")
    print(f"Capacity: {C:.4f} bits/channel use")
    print(f"Max info per page: {info_bits:.0f} bits")
    return info_bits

ssd_capacity_page(raw_ber=1e-3)
```

**Deep space communication.** NASA's Deep Space Network uses concatenated codes (Reed-Solomon + convolutional) and turbo codes for reliable communication at extremely low SNRs (great distances mean very weak signals).

## Glossary

| Term | Definition |
|---|---|
| Channel capacity | Maximum mutual information over all input distributions, $C = \max_{p(x)} I(X; Y)$ |
| Discrete memoryless channel | Channel with finite alphabets where output depends only on current input |
| Noisy channel coding theorem | Reliability is possible iff rate $<$ capacity |
| Random coding | Probabilistic code construction used in achievability proof |
| Joint typicality | Sequences $(x^n, y^n)$ that are typical under the joint distribution |
| Binary symmetric channel (BSC) | Binary channel where bits flip with probability $p$ |
| Binary erasure channel (BEC) | Channel where bits are either received correctly or erased |
| Shannon-Hartley theorem | $C = W \log_2(1 + P/N_0 W)$ for AWGN channels |
| Code rate | Ratio $k/n$ for an $(n, k)$ block code |
| Blahut-Arimoto algorithm | Iterative algorithm for computing channel capacity |
| Finite block length regime | Capacity analysis for finite-size codes $n < \infty$ |
| Channel dispersion | Parameter $V$ characterizing the finite-length penalty |
| SNR gap | Difference between required SNR in practice and Shannon limit |
| AWGN | Additive white Gaussian noise channel model |

## Quick References

- [Shannon, C. E. (1948). A Mathematical Theory of Communication](https://people.math.harvard.edu/~ctm/home/text/others/shannon/entropy/entropy.pdf) — original paper establishing the noisy channel coding theorem
- [Cover, T. M. & Thomas, J. A. (2006). Elements of Information Theory](https://www.wiley.com/en-us/Elements+of+Information+Theory%2C+2nd+Edition-p-9780471241959) — chapters 7 and 8 on channel capacity and coding
- [Blahut, R. E. (1972). Computation of Channel Capacity and Rate-Distortion Functions](https://doi.org/10.1109/TIT.1972.1054855) — the Blahut-Arimoto algorithm
- [El Gamal, A. & Kim, Y. H. (2011). Network Information Theory](https://www.cambridge.org/us/academic/subjects/engineering/communications-and-signal-processing/network-information-theory) — advanced topics including multi-user channels

## Next Steps

- [Rate-Distortion](rate-distortion.md) — lossy compression theory, the dual problem to channel capacity
- [Error-Correcting Codes](error-correcting-codes.md) — practical codes that approach capacity (LDPC, turbo, polar)
- [Source Coding](source-coding.md) — lossless compression, the source-channel separation theorem
