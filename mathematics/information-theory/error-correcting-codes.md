# Error-Correcting Codes

## Description

Error-correcting codes add structured redundancy to data, enabling the receiver to detect and correct transmission errors. From Hamming's simple block codes to modern LDPC and turbo codes that approach Shannon capacity, error correction is essential for reliable communication, data storage, QR codes, and deep-space networking.

## Prerequisites

- [Entropy and Information](entropy-and-information.md) — entropy, mutual information, self-information
- [Channel Capacity](channel-capacity.md) — noisy channel model, BSC, capacity notions

## Table of Contents

- [Need for Error Correction](#need-for-error-correction)
- [Hamming Distance and Weight](#hamming-distance-and-weight)
- [Block Codes and Linear Codes](#block-codes-and-linear-codes)
- [Hamming Codes](#hamming-codes)
- [Cyclic Codes and Reed-Solomon Codes](#cyclic-codes-and-reed-solomon-codes)
- [Low-Density Parity-Check Codes](#low-density-parity-check-codes)
- [Turbo Codes](#turbo-codes)
- [Convolutional Codes](#convolutional-codes)
- [LDPC vs. Turbo vs. Polar](#ldpc-vs-turbo-vs-polar)
- [Applications in Computing](#applications-in-computing)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Need for Error Correction

Information transmitted over any real channel is subject to noise. Without error correction, even a single bit flip can corrupt a message, leading to corrupted files, garbled audio, or failed data recovery.

**Error detection vs. correction:**

- **Error detection codes** (e.g., CRC, checksums) can detect that an error occurred but cannot locate or fix it. The receiver must request retransmission.
- **Error correction codes (ECC)** can locate and fix errors without retransmission.

**Types of errors:**

- **Random errors:** Independent bit flips, modeled by the BSC.
- **Burst errors:** Consecutive bits corrupted (e.g., scratches on a CD, wireless fading).
- **Erasures:** Known positions where data is lost (modeled by the BEC).

**The trade-off.** Adding redundancy reduces the effective data rate. For an $(n, k)$ code that maps $k$ data bits into $n$ transmitted bits, the **code rate** is $R = k/n$. Higher redundancy (lower rate) allows correcting more errors.

**Shannon's promise.** The noisy channel coding theorem guarantees that for any rate $R < C$, there exists a code achieving arbitrarily low error probability. Error-correcting codes are the practical realization of this promise.

### Hamming Distance and Weight

The **Hamming distance** between two equal-length vectors is the number of positions where they differ:

$$
d_H(\mathbf{x}, \mathbf{y}) = |\{i : x_i \neq y_i\}|
$$

Properties:

- Non-negative: $d_H(\mathbf{x}, \mathbf{y}) \geq 0$, zero iff $\mathbf{x} = \mathbf{y}$
- Symmetric: $d_H(\mathbf{x}, \mathbf{y}) = d_H(\mathbf{y}, \mathbf{x})$
- Triangle inequality: $d_H(\mathbf{x}, \mathbf{z}) \leq d_H(\mathbf{x}, \mathbf{y}) + d_H(\mathbf{y}, \mathbf{z})$

**Hamming weight** of a vector is the number of non-zero entries: $w_H(\mathbf{x}) = d_H(\mathbf{x}, \mathbf{0})$.

**Minimum distance** of a code $C$:

$$
d_{\min} = \min_{\mathbf{x}, \mathbf{y} \in C, \mathbf{x} \neq \mathbf{y}} d_H(\mathbf{x}, \mathbf{y})
$$

The minimum distance determines the error correction capability:

- **Detect up to $d_{\min} - 1$ errors**
- **Correct up to $\lfloor (d_{\min} - 1) / 2 \rfloor$ errors**

```python
def hamming_distance(x, y):
    return sum(a != b for a, b in zip(x, y))

def min_distance(codewords):
    n = len(codewords)
    d_min = len(codewords[0])
    for i in range(n):
        for j in range(i+1, n):
            d = hamming_distance(codewords[i], codewords[j])
            if d < d_min:
                d_min = d
    return d_min

codewords = ["000", "111"]
print(f"Repetition code d_min: {min_distance(codewords)}")
print(f"Corrects up to: {(min_distance(codewords)-1)//2} errors")
```

**Sphere-packing bound (Hamming bound):** For an $(n, k)$ binary code that corrects $t$ errors:

$$
2^{n-k} \geq \sum_{i=0}^{t} \binom{n}{i}
$$

Codes that meet this bound are called **perfect codes**. Hamming codes are perfect $t=1$ codes.

### Block Codes and Linear Codes

A **block code** of length $n$ and dimension $k$ (denoted $(n, k)$) maps each $k$-bit message to a unique $n$-bit codeword from a set of $2^k$ codewords.

A **linear code** is a block code where the codewords form a $k$-dimensional subspace of $\mathbb{F}_2^n$ (binary field). Linear codes are convenient because encoding and decoding can use linear algebra.

**Generator matrix $G$:** A $k \times n$ matrix whose rows form a basis for the code:

$$
\mathbf{c} = \mathbf{m} G
$$

where $\mathbf{m}$ is the $k$-bit message vector and $\mathbf{c}$ is the $n$-bit codeword.

**Parity-check matrix $H$:** An $(n-k) \times n$ matrix such that $G H^T = 0$. A vector $\mathbf{c}$ is a valid codeword iff $H \mathbf{c}^T = \mathbf{0}$.

**Systematic form:** A generator matrix in systematic form has the identity matrix as its left $k \times k$ submatrix:

$$
G = [I_k | P]
$$

where $P$ is the $k \times (n-k)$ parity matrix. The codeword is the message followed by parity bits.

```python
import numpy as np

def encode_linear(G, message):
    return (message @ G) % 2

def syndrome(H, received):
    return (H @ received) % 2

# Simple (7,4) linear code
G = np.array([
    [1, 0, 0, 0, 1, 1, 0],
    [0, 1, 0, 0, 1, 0, 1],
    [0, 0, 1, 0, 0, 1, 1],
    [0, 0, 0, 1, 1, 1, 1]
], dtype=int)

H = np.array([
    [1, 1, 1, 0, 1, 0, 0],
    [1, 0, 1, 1, 0, 1, 0],
    [0, 1, 1, 1, 0, 0, 1]
], dtype=int)

msg = np.array([1, 0, 1, 0])
codeword = encode_linear(G, msg)
print(f"Message: {msg}")
print(f"Codeword: {codeword}")

# Introduce error
received = codeword.copy()
received[2] ^= 1  # flip bit 2
print(f"Received (with error): {received}")

# Syndrome decoding
s = syndrome(H, received)
print(f"Syndrome: {s}")
```

**Syndrome decoding:** For a linear code with parity-check matrix $H$, the syndrome $\mathbf{s} = H \mathbf{r}^T$ identifies the error pattern. If $\mathbf{r} = \mathbf{c} + \mathbf{e}$, then $\mathbf{s} = H \mathbf{e}^T$ (since $H \mathbf{c}^T = \mathbf{0}$). The decoder finds the minimum-weight error $\mathbf{e}$ with syndrome $\mathbf{s}$.

### Hamming Codes

Hamming codes are a family of perfect $(2^r - 1, 2^r - r - 1)$ linear codes with minimum distance $3$, capable of correcting all single-bit errors.

**Parameters ($r \geq 2$):**

| $r$ | $n = 2^r - 1$ | $k = n - r$ | Rate $k/n$ | $d_{\min}$ |
|---|---|---|---|---|
| 2 | 3 | 1 | 0.333 | 3 |
| 3 | 7 | 4 | 0.571 | 3 |
| 4 | 15 | 11 | 0.733 | 3 |
| 5 | 31 | 26 | 0.839 | 3 |
| 6 | 63 | 57 | 0.905 | 3 |

**Construction:** For the $(7,4)$ Hamming code, the parity-check matrix $H$ has columns that are all non-zero binary vectors of length 3:

$$
H = \begin{bmatrix}
1 & 1 & 1 & 0 & 1 & 0 & 0 \\
1 & 0 & 1 & 1 & 0 & 1 & 0 \\
0 & 1 & 1 & 1 & 0 & 0 & 1
\end{bmatrix}
$$

The columns are $[1,1,0]^T$, $[1,0,1]^T$, $[1,1,1]^T$, $[0,1,1]^T$, $[1,0,0]^T$, $[0,1,0]^T$, $[0,0,1]^T$.

**Decoding:** Compute the syndrome $\mathbf{s} = H \mathbf{r}^T$. If $\mathbf{s} = \mathbf{0}$, no error. Otherwise, $\mathbf{s}$ is the column of $H$ corresponding to the error position.

```python
class HammingCode:
    def __init__(self, r=3):
        self.r = r
        self.n = 2**r - 1
        self.k = self.n - r

        # Build H with all non-zero r-bit columns
        H_cols = []
        for i in range(1, 2**r):
            col = [(i >> j) & 1 for j in range(r)]
            H_cols.append(col)
        self.H = np.array(H_cols, dtype=int).T

        # Build systematic G
        # Find pivot columns in H (first r columns of H will work)
        H_sys = self.H.copy()
        # Convert to systematic form [P | I_{n-k}]
        H_sys = self._to_systematic(H_sys)
        # G = [I_k | P^T]
        P = H_sys[:, :self.k].T
        self.G = np.hstack([np.eye(self.k, dtype=int), P])

    def _to_systematic(self, H):
        # Row reduce to [P | I]
        H_sys = H.copy()
        n = H.shape[1]
        # Gaussian elimination
        for col in range(H_sys.shape[0]):
            pivot = None
            for row in range(col, H_sys.shape[0]):
                if H_sys[row, n - H_sys.shape[0] + col] == 1:
                    pivot = row
                    break
            if pivot is not None:
                H_sys[[col, pivot]] = H_sys[[pivot, col]]
                for row in range(H_sys.shape[0]):
                    if row != col and H_sys[row, n - H_sys.shape[0] + col] == 1:
                        H_sys[row] ^= H_sys[col]
        return H_sys

    def encode(self, message):
        return (message @ self.G) % 2

    def decode(self, received):
        s = (self.H @ received) % 2
        if np.all(s == 0):
            return received[:self.k], False
        # Find column of H matching syndrome
        s_col = np.argmin(np.sum((self.H.T != s) ** 2, axis=1))
        corrected = received.copy()
        corrected[s_col] ^= 1
        return corrected[:self.k], True

# Test (7,4) Hamming code
hamming = HammingCode(r=3)
msg = np.array([1, 0, 1, 1])
codeword = hamming.encode(msg)
print(f"Original: {msg}")
print(f"Codeword:  {codeword}")

received = codeword.copy()
received[3] ^= 1  # flip bit 3
print(f"Received:  {received}")

decoded, corrected = hamming.decode(received)
print(f"Decoded:   {decoded}")
print(f"Corrected: {corrected}")
```

**Extended Hamming code:** Adding an overall parity bit creates an $(n+1, k)$ code with $d_{\min} = 4$, capable of correcting 1 error and detecting 2 errors.

### Cyclic Codes and Reed-Solomon Codes

**Cyclic codes** are linear codes where any cyclic shift of a codeword is also a codeword. They are described using polynomial algebra over finite fields.

A codeword polynomial $c(x)$ of degree $< n$ is a multiple of the **generator polynomial** $g(x)$ of degree $n-k$:

$$
c(x) = m(x) g(x)
$$

where $m(x)$ is the message polynomial.

**Reed-Solomon (RS) codes** are powerful non-binary cyclic codes widely used in practice:

- **Parameters:** RS$(n, k)$ over $\mathbb{F}_{q}$, where $n = q - 1$ (typically $q = 2^m$, so codewords are bytes)
- **Minimum distance:** $d_{\min} = n - k + 1$
- **Singleton bound:** RS codes are optimal — they achieve the Singleton bound $d_{\min} \leq n - k + 1$
- **Corrects:** Up to $\lfloor (n - k) / 2 \rfloor$ symbol errors or $n - k$ erasures

For RS(255, 223) over GF($2^8$), each codeword contains 255 bytes (223 data + 32 parity), correcting up to 16 byte errors. This is the NASA-standard code used in deep space communication. Reed-Solomon decoding uses the Berlekamp-Massey algorithm to find the error locator polynomial, followed by Chien search to find error positions and Forney's algorithm to compute error values.

**Applications of Reed-Solomon codes:**

- **CD and DVD:** RS(28, 24) and RS(32, 28) in the CIRC (Cross-Interleaved Reed-Solomon Code)
- **QR codes:** Varying RS codes depending on version and error correction level (L, M, Q, H)
- **DSL:** RS codes for impulse noise protection
- **Satellite communications:** CCSDS standard uses concatenated RS + convolutional codes
- **RAID 6 storage:** P + Q redundancy uses RS-like erasure coding

### Low-Density Parity-Check Codes

LDPC codes, invented by Gallager in 1960 and rediscovered in the 1990s, are linear codes with a sparse parity-check matrix. They approach Shannon capacity remarkably closely.

**Properties:**

- Parity-check matrix $H$ has very few 1s (low density)
- Not constructed algebraically — often designed randomly or with structured patterns
- Decoded using **belief propagation** (sum-product algorithm) on a factor graph
- Performance improves with block length: $n = 10^4$ to $10^6$ is typical

```python
import numpy as np

class SimpleLDPC:
    def __init__(self, n, k):
        self.n = n
        self.k = k
        self.m = n - k

    def build_regular_h(self, dv=3, dc=6):
        """Build a regular (dv, dc) LDPC parity-check matrix."""
        H = np.zeros((self.m, self.n), dtype=int)

        # Column weight = dv, row weight = dc
        # Simple construction (not optimized)
        col_ones = np.zeros(self.n, dtype=int)
        row_ones = np.zeros(self.m, dtype=int)

        # Distribute ones
        for col in range(self.n):
            for _ in range(dv):
                row = np.random.randint(0, self.m)
                while H[row, col] == 1 or row_ones[row] >= dc:
                    row = np.random.randint(0, self.m)
                H[row, col] = 1
                col_ones[col] += 1
                row_ones[row] += 1

        # Remove short cycles (4-cycles) — simplified
        # In practice, use progressive edge-growth (PEG) algorithm
        return H

    def encode(self, message):
        # Systematic encoding using Gaussian elimination on H
        # Simplified: treat first k bits as message, compute parity bits
        # In practice, use efficient encoding (e.g., Richardson-Urbanke)
        H = self.build_regular_h()

        # [H1 | H2] -> find H2 invertible
        H1 = H[:, :self.k]
        H2 = H[:, self.k:]

        # Parity bits: p = H2^{-1} @ H1 @ m
        try:
            H2_inv = np.linalg.inv(H2.astype(float))
        except:
            return None

        parity = (H2_inv @ (H1 @ message)) % 2
        return np.concatenate([message, parity.astype(int)])

    def decode_bp(self, received, max_iter=50):
        """Belief propagation decoding (simplified)."""
        # This is a placeholder — full BP is complex
        # Real implementations use log-likelihood ratios and message passing
        pass

print("LDPC codes approach Shannon capacity within 0.1 dB at long block lengths")
print("Used in: DVB-S2, Wi-Fi (802.11n/ac/ax), 10GBase-T, 5G NR data channels")
```

**Belief propagation decoding:**

1. Initialize variable nodes with channel LLRs (log-likelihood ratios).
2. Variable nodes send messages to check nodes.
3. Check nodes compute parity constraints and send messages back.
4. Variable nodes combine incoming messages.
5. Repeat until syndrome is zero or max iterations reached.

The sparsity of $H$ makes BP decoding tractable. The complexity scales as $O(n \cdot \text{iterations})$, with typical iterations around 10-50.

**Structured LDPC codes** (IEEE 802.11n, 802.16e, DVB-S2) use quasi-cyclic (QC) construction: $H$ is composed of circulant permutation matrices, enabling efficient encoding and decoding in hardware.

### Turbo Codes

Turbo codes, introduced by Berrou, Glavieux, and Thitimajshima in 1993, achieve near-capacity performance using parallel concatenated convolutional codes with iterative decoding.

**Structure:**

```
┌────────────┐    ┌──────────────────┐
│             │    │  RSC Encoder 1   │────┐
│   Data      │────┤                  │    │
│             │    └──────────────────┘    │    ┌──────────┐
│             │                            ├────│  MUX     │──► Codeword
│             │    ┌──────────────────┐    │    └──────────┘
│             │    │  Interleaver     │────┤
│             │    └────────┬─────────┘    │
│             │             │              │
│             │    ┌────────v──────────┐   │
│             │    │  RSC Encoder 2   │───┘
│             │    └──────────────────┘
└────────────┘
```

**Key components:**

- **Recursive systematic convolutional (RSC) encoders:** Two identical RSC codes
- **Interleaver:** Permutes the data bits, making the two encoders see independent inputs
- **Puncturing:** Adjusts the code rate by selectively omitting parity bits

**Iterative (turbo) decoding:**

The two decoders exchange soft information (extrinsic LLRs) iteratively:

1. Decoder 1 processes the received sequence and generates extrinsic information.
2. The interleaved extrinsic information becomes a priori input to Decoder 2.
3. Decoder 2 produces its own extrinsic information.
4. After de-interleaving, this becomes a priori for Decoder 1.
5. Repeat for 4-10 iterations.

Each iteration improves the reliability of bit estimates. The process resembles the feedback loop of a turbo engine — hence the name.

**Performance:** Turbo codes with rate 1/2 and block length 65536 achieve BER $10^{-5}$ at $E_b/N_0 = 0.7$ dB, within 0.5 dB of the Shannon limit for the AWGN channel.

```python
class SimpleTurboEncoder:
    def __init__(self, constraint_length=4, generator_polynomials=[0o13, 0o15]):
        self.constraint = constraint_length
        self.gens = generator_polynomials
        self.memory = 0

    def rsc_encode(self, bits):
        """Simplified RSC encoding for one encoder."""
        state = 0
        output = []
        for b in bits:
            # Feedback: state AND generator polynomial
            feedback = state & 0b111
            fb_bit = bin(feedback).count('1') % 2  # XOR of feedback bits
            in_bit = b ^ fb_bit
            # Output bit
            out_bit = (in_bit ^ (state >> 1 & 1)) if (self.gens[0] & 4) else in_bit
            output.append(out_bit)
            state = (state >> 1) | (in_bit << (self.constraint - 2))
        return output

    def turbo_encode(self, data):
        # First encoder
        enc1 = self.rsc_encode(data)

        # Interleaver (simple block interleaver)
        interleaved = data.copy()
        mid = len(data) // 2
        interleaved = np.concatenate([data[mid:], data[:mid]])

        # Second encoder
        enc2 = self.rsc_encode(interleaved)

        # Multiplex: systematic + parity1 + parity2
        codeword = np.zeros(3 * len(data), dtype=int)
        for i in range(len(data)):
            codeword[3*i] = data[i]
            codeword[3*i+1] = enc1[i]
            codeword[3*i+2] = enc2[i]

        return codeword

turbo = SimpleTurboEncoder()
data = np.random.randint(0, 2, 100)
encoded = turbo.turbo_encode(data)
print(f"Data length: {len(data)}")
print(f"Encoded length (rate 1/3): {len(encoded)}")
print(f"Rate: {len(data)/len(encoded):.4f}")
```

### Convolutional Codes

Convolutional codes operate on a continuous stream of data using shift registers, producing $n$ output bits for every $k$ input bits. Unlike block codes, they have memory — the encoding of each bit depends on previous bits.

**Parameters:** $(n, k, K)$ where $K$ is the constraint length (number of registers).

**Generator polynomials** define the connections from the shift register to the output. For the $(2, 1, 3)$ code with generators $(7, 5)$ in octal:

```
Input ──► [D] ──► [D] ──► [D]
            │      │
            ▼      ▼
           (+)───► Output 1 (g0 = 111, octal 7)
            │      │
            ▼      ▼
           (+)───► Output 2 (g1 = 101, octal 5)
```

```python
def convolutional_encode(bits, generators, constraint=3):
    """Encode bits with a (n,1,K) convolutional code."""
    n = len(generators)
    state = 0
    output = []

    for b in bits:
        # Shift in new bit
        state = (state << 1) | b
        state_mask = (1 << constraint) - 1
        state &= state_mask

        # Compute each output bit
        for gen in generators:
            parity = bin(state & gen).count('1') % 2
            output.append(parity)

    return output

# (2, 1, 3) code with generators (7, 5)
gen7 = 0b111  # octal 7
gen5 = 0b101  # octal 5
bits = [1, 0, 1, 1, 0, 0, 1]
encoded = convolutional_encode(bits, [gen7, gen5], 3)
print(f"Input:    {bits}")
print(f"Encoded:  {encoded}")
```

**Viterbi algorithm (maximum likelihood decoding):**

The Viterbi algorithm finds the most likely transmitted sequence given the received sequence by dynamic programming on the trellis:

1. For each state at each time step, compute the path metric (cumulative Hamming or Euclidean distance).
2. Keep only the survivor path (lowest metric) for each state.
3. Trace back the best path to find the decoded sequence.

The complexity is $O(2^{K-1})$ per decoded bit, exponential in constraint length $K$.

**Punctured convolutional codes** achieve higher rates by periodically deleting output bits. For example, to get rate 2/3 from a rate 1/2 code, transmit every 1st and 2nd bit from every pair of input bits but skip the 4th bit.

### LDPC vs. Turbo vs. Polar

| Property | LDPC | Turbo | Polar |
|---|---|---|---|
| Invented | 1960 (Gallager), 1996 (rediscovery) | 1993 (Berrou et al.) | 2009 (Arikan) |
| Decoding | Belief propagation (parallel) | Iterative MAP (serial) | Successive cancellation |
| Complexity | $O(n \log n)$ | $O(n \cdot \text{iter})$ | $O(n \log n)$ |
| Error floor | Low with optimized design | Moderate (interleaver design) | Vanishing (proven) |
| Near capacity | 0.1-0.3 dB | 0.5-1 dB | 0.1-0.3 dB (with SC-List) |
| Standards | DVB-S2, WiFi, 5G data | 3G, 4G LTE, DVB-RCS | 5G control channel |
| Block length | $10^3$ - $10^6$ | $10^3$ - $10^5$ | $2^n$ (power of 2) |

**Polar codes** (Arikan, 2009) are the first provably capacity-achieving codes for symmetric channels with explicit construction. They use **channel polarization**: recursively combine and split channels to create extreme channels (noiseless or pure-noise). Data is sent over the noiseless channels (frozen bits for noisy ones).

### Applications in Computing

**CD and DVD.** The CIRC (Cross-Interleaved Reed-Solomon Code) corrects burst errors from scratches:

- C1 decoder: RS(32, 28) corrects 2 errors per 32-byte block
- Cross-interleaver spreads burst errors across multiple blocks
- C2 decoder: RS(28, 24) corrects additional errors

A scratch affecting thousands of consecutive bytes is spread into correctable single-byte errors.

**QR codes.** QR codes use Reed-Solomon error correction with four levels:

| Level | Recovery | Typical use |
|---|---|---|
| L (Low) | 7% | Clean environments |
| M (Medium) | 15% | General purpose |
| Q (Quartile) | 25% | Outdoor, industrial |
| H (High) | 30% | Harsh environments |

```python
import qrcode

def qr_error_correction_demo(data, version=5):
    # Simulate the RS code parameters for a QR code
    # Version 5-L: (134, 106), 14 error correction codewords
    n = 134
    k = 106
    t = (n - k) // 2  # 14 symbol correction
    print(f"QR version {version} data: {len(data)} bytes")
    print(f"RS codewords: {n - k} (corrects {t} errors)")
    return t

qr_error_correction_demo(b"Hello, DevBook!", version=5)
```

**Satellite communication.** Deep space missions use concatenated coding: an outer RS(255, 223) code followed by an inner rate-1/2 convolutional code. This achieves high reliability at very low SNRs:

- Voyager: Golay(24, 12) then concatenated RS + convolutional
- Mars rovers: turbo codes
- Modern missions: LDPC codes per CCSDS standard

**SSD error correction.** NAND flash memory uses ECC to correct read errors that increase as cells wear:

| NAND type | Typical ECC required | Correction per 1 KB |
|---|---|---|
| SLC (1 bit/cell) | None to BCH | ~1-4 bits |
| MLC (2 bits/cell) | BCH or LDPC | ~8-24 bits |
| TLC (3 bits/cell) | LDPC | ~30-60 bits |
| QLC (4 bits/cell) | Strong LDPC | ~60-120 bits |

```python
def ssd_ecc_requirement(bit_error_rate, page_size=16384, target_uncorrectable=1e-15):
    bits = page_size * 8
    # Simplified: estimate needed corrected errors per page
    expected_errors = bits * bit_error_rate
    # Using the Poisson approximation
    from scipy.stats import poisson
    for t in range(1, 500):
        # P(more than t errors) = 1 - CDF(t)
        fail_prob = 1 - poisson.cdf(t, expected_errors)
        if fail_prob < target_uncorrectable:
            return t
    return None

for ber in [1e-6, 1e-5, 1e-4, 1e-3]:
    t = ssd_ecc_requirement(ber)
    print(f"BER={ber}: need to correct ≥{t} errors per 16 KB page")
```

**Ethernet (10GBASE-T).** Uses LDPC codes with 2048-bit codewords and rate 0.85, operating within 1 dB of Shannon capacity at BER $10^{-12}$.

## Glossary

| Term | Definition |
|---|---|
| Hamming distance | Number of positions where two equal-length vectors differ |
| Hamming weight | Number of non-zero entries in a vector |
| Minimum distance | Smallest Hamming distance between any two codewords, determines error correction capability |
| Block code | Code mapping $k$-bit blocks to $n$-bit codewords |
| Linear code | Code forming a vector subspace, closed under addition |
| Generator matrix | $k \times n$ matrix mapping messages to codewords |
| Parity-check matrix | $(n-k) \times n$ matrix for syndrome computation |
| Syndrome | $H \mathbf{r}^T$, used for error detection and correction |
| Hamming code | Perfect $(2^r-1, 2^r-r-1)$ code correcting single errors |
| Reed-Solomon code | Optimal non-binary code correcting burst errors |
| LDPC | Low-density parity-check code with sparse $H$ and BP decoding |
| Turbo code | Parallel concatenated convolutional code with iterative decoding |
| Convolutional code | Code using shift registers, decoded by Viterbi algorithm |
| Belief propagation | Iterative message-passing algorithm on factor graphs |
| Viterbi algorithm | Dynamic programming ML decoder for convolutional codes |
| Code rate | $R = k/n$, ratio of data bits to transmitted bits |
| Erasure | Known position where data is lost (modeled by BEC) |
| Singleton bound | $d_{\min} \leq n - k + 1$; codes achieving it are maximum distance separable (MDS) |
| Sphere-packing bound | Fundamental upper bound on code size given $n$ and $t$ |
| Channel polarization | Polar codes recursively create extreme channels |

## Quick References

- [Gallager, R. G. (1963). Low-Density Parity-Check Codes](https://doi.org/10.5555/1096445) — the original LDPC monograph
- [Berrou, C., Glavieux, A., & Thitimajshima, P. (1993). Near Shannon Limit Error-Correcting Coding and Decoding: Turbo-Codes](https://doi.org/10.1109/ICC.1993.397511) — the turbo codes breakthrough
- [MacKay, D. J. C. (2003). Information Theory, Inference, and Learning Algorithms](https://www.inference.org.uk/mackay/itila/) — chapters on error-correcting codes, including LDPC
- [Moon, T. K. (2005). Error Correction Coding: Mathematical Methods and Algorithms](https://www.wiley.com/en-us/Error+Correction+Coding%3A+Mathematical+Methods+and+Algorithms-p-9780471648000) — comprehensive coding theory textbook
- [Wikipedia: Hamming Code](https://en.wikipedia.org/wiki/Hamming_code) — detailed explanation with worked examples

## Next Steps

This concludes the core Information Theory module. Review the full sequence:

- [Entropy and Information](entropy-and-information.md) — foundation of all information theory
- [Mutual Information](mutual-information.md) — dependence measure, feature selection
- [Source Coding](source-coding.md) — lossless compression
- [Channel Capacity](channel-capacity.md) — reliable communication limits
- [Rate-Distortion](rate-distortion.md) — lossy compression theory
- [This document](error-correcting-codes.md) — protecting data against noise

For advanced topics, explore:

- [Network Information Theory](../network-information-theory/index.md) — multi-user channels, broadcast, multiple access
- [Probability & Statistics](../probability-and-statistics/index.md) — the mathematical foundations for information theory
- [AI / ML](../../ai-ml/index.md) — information-theoretic approaches to machine learning
