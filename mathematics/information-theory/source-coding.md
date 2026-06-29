# Source Coding Theorem

## Description

Shannon's source coding theorem establishes the fundamental limit of lossless data compression: the expected length of an optimal code cannot be less than the entropy of the source. Huffman coding, arithmetic coding, and Lempel-Ziv algorithms are practical compression schemes that approach this limit, forming the backbone of every compression tool from gzip to PNG.

## Prerequisites

- [Entropy and Information](entropy-and-information.md) — entropy, self-information, expected code length
- [Probability Foundations](../probability-and-statistics/probability-foundations.md) — discrete probability, expected value

## Table of Contents

- [Shannon's Source Coding Theorem](#shannons-source-coding-theorem)
- [Optimal Codes and Prefix Codes](#optimal-codes-and-prefix-codes)
- [Kraft Inequality](#kraft-inequality)
- [Huffman Coding](#huffman-coding)
- [Arithmetic Coding](#arithmetic-coding)
- [Lempel-Ziv (LZ77)](#lempel-ziv-lz77)
- [Lempel-Ziv (LZ78)](#lempel-ziv-lz78)
- [Comparison of Compression Algorithms](#comparison-of-compression-algorithms)
- [Redundancy and Code Efficiency](#redundancy-and-code-efficiency)
- [Applications in Computing](#applications-in-computing)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Shannon's Source Coding Theorem

The source coding theorem, also called the **noiseless coding theorem**, states:

> For a discrete memoryless source with entropy $H(X)$, the expected code length $L$ of any optimal instantaneous code satisfies:
>
> $$
> H(X) \leq L < H(X) + 1
> $$

This theorem establishes that entropy is the **fundamental limit** of lossless compression. No lossless compression scheme can compress data below its entropy on average, and codes exist that come arbitrarily close.

**Achievability bound for block codes.** When encoding $n$ symbols together, the per-symbol code length converges to the entropy:

$$
H(X) \leq \frac{\mathbb{E}[L(X^n)]}{n} < H(X) + \frac{1}{n}
$$

As $n \to \infty$, the average code length approaches $H(X)$ from above. This is why modern compressors process data in blocks.

**Asymptotic equipartition property (AEP).** The AEP states that for large $n$, the set of all $n$-length sequences can be divided into a **typical set** containing approximately $2^{nH(X)}$ sequences (each with probability $\approx 2^{-nH(X)}$) and an atypical set with negligible total probability. The source coding theorem works by assigning short codewords to typical sequences and longer ones to atypical sequences.

### Optimal Codes and Prefix Codes

A **code** is a mapping from source symbols to binary strings (codewords). The expected code length:

$$
L = \sum_{x \in \mathcal{X}} p(x) \cdot \ell(x)
$$

where $\ell(x)$ is the length of the codeword for symbol $x$.

**Prefix code (instantaneous code):** No codeword is a prefix of any other codeword. This allows decoding without lookahead — as soon as a complete codeword is received, it can be decoded immediately.

Example of a prefix code for $\{A, B, C, D\}$:

| Symbol | Codeword |
|---|---|
| A | 0 |
| B | 10 |
| C | 110 |
| D | 111 |

This is a prefix code because no codeword is a prefix of another. The sequence `0110111` uniquely decodes to `A C D`.

**Non-prefix code (not recommended):**

| Symbol | Codeword |
|---|---|
| A | 0 |
| B | 01 |
| C | 011 |

`011` could be `A B`, `A C`, or `B B` — ambiguous decode.

### Kraft Inequality

The Kraft inequality provides a necessary and sufficient condition for the existence of a prefix code with given codeword lengths $\ell_1, \ell_2, \dots, \ell_n$:

$$
\sum_{i=1}^{n} 2^{-\ell_i} \leq 1
$$

If the sum is exactly 1, the code is **complete** (every available bit sequence is used). If it is strictly less than 1, there is slack — the code could be made more efficient.

**Proof sketch:** Consider a full binary tree of depth $L_{\max} = \max_i \ell_i$. Each codeword of length $\ell_i$ occupies $2^{L_{\max} - \ell_i}$ leaf nodes (all descendants in the full tree). Codewords must occupy disjoint subtrees for a prefix code. The total number of leaf nodes is $2^{L_{\max}}$, so:

$$
\sum_{i=1}^{n} 2^{L_{\max} - \ell_i} \leq 2^{L_{\max}} \implies \sum_{i=1}^{n} 2^{-\ell_i} \leq 1
$$

**McMillan theorem:** The Kraft inequality is also a sufficient condition for the existence of a prefix code. Given lengths satisfying the inequality, a prefix code can always be constructed.

**Optimal code length theorem.** For an optimal prefix code, the codeword lengths must satisfy:

$$
-\log_2 p(x) \leq \ell(x) < -\log_2 p(x) + 1
$$

The ideal length for symbol $x$ is $-\log_2 p(x)$ bits (its self-information), but code lengths must be integers, hence the rounding up by at most 1 bit.

### Huffman Coding

David Huffman's 1952 algorithm constructs an optimal prefix code for a given distribution. It produces a code with minimum expected codeword length among all prefix codes.

**Algorithm:**

1. Create a leaf node for each symbol with its probability.
2. Merge the two lowest-probability nodes into a new node whose probability is the sum.
3. Assign 0 to one branch and 1 to the other.
4. Repeat until one node remains (the root).
5. Read codewords by traversing from root to leaf.

```python
import heapq
from collections import Counter

class HuffmanNode:
    def __init__(self, prob, symbol=None, left=None, right=None):
        self.prob = prob
        self.symbol = symbol
        self.left = left
        self.right = right

    def __lt__(self, other):
        return self.prob < other.prob

def build_huffman_tree(probs):
    heap = [HuffmanNode(p, s) for s, p in probs.items()]
    heapq.heapify(heap)

    while len(heap) > 1:
        left = heapq.heappop(heap)
        right = heapq.heappop(heap)
        merged = HuffmanNode(left.prob + right.prob, left=left, right=right)
        heapq.heappush(heap, merged)

    return heap[0]

def build_huffman_codes(node, prefix="", codebook=None):
    if codebook is None:
        codebook = {}
    if node.symbol is not None:
        codebook[node.symbol] = prefix or "0"
    else:
        build_huffman_codes(node.left, prefix + "0", codebook)
        build_huffman_codes(node.right, prefix + "1", codebook)
    return codebook

probs = {"A": 0.4, "B": 0.25, "C": 0.2, "D": 0.15}
tree = build_huffman_tree(probs)
codes = build_huffman_codes(tree)
for s, c in sorted(codes.items()):
    print(f"{s}: {c}")
```

**Output (varies slightly by tie-breaking):**

```
A: 0
B: 10
C: 110
D: 111
```

**Expected length:** $L = 0.4 \times 1 + 0.25 \times 2 + 0.2 \times 3 + 0.15 \times 3 = 1.95$ bits.

Compare to entropy: $H = -0.4 \log_2 0.4 - 0.25 \log_2 0.25 - 0.2 \log_2 0.2 - 0.15 \log_2 0.15 \approx 1.86$ bits.

Redundancy: $1.95 - 1.86 = 0.09$ bits per symbol (under 5%).

Huffman coding guarantees $L < H + 1$ and is optimal among symbol-by-symbol codes. However, it requires knowledge of the source distribution and is suboptimal when symbol probabilities are not powers of 1/2.

### Arithmetic Coding

Arithmetic coding overcomes Huffman's limitation of integer-length codewords by encoding the entire input as a single number in $[0, 1)$. It can achieve near-optimal compression with redundancy arbitrarily close to zero bits per symbol.

**Algorithm:**

1. Maintain an interval $[low, high)$ initialized to $[0, 1)$.
2. For each symbol, divide the current interval proportionally to the cumulative distribution.
3. Narrow the interval to the sub-interval for the observed symbol.
4. After processing all symbols, output a binary representation of any number in the final interval.

```
Input: "A B C" with p(A)=0.5, p(B)=0.3, p(C)=0.2

Step 1 (initial):  [0.0, 1.0)
Step 2 (A):        [0.0, 0.5)     CDF: A=[0.0,0.5), B=[0.5,0.8), C=[0.8,1.0)
Step 3 (B):        [0.25, 0.40)   B within [0.0,0.5): [0.5*0.5, 0.5*0.8)=[0.25,0.40)
Step 4 (C):        [0.37, 0.40)   C within [0.25,0.40): [0.25+0.15*0.8, 0.25+0.15*1.0)
                                   = [0.25+0.12, 0.25+0.15) = [0.37, 0.40)

Output: any binary number in [0.37, 0.40), e.g., 0.375 = 0.011 in binary
```

```python
import math

def arithmetic_encode(symbols, probs, seq):
    cdf = {}
    cum = 0.0
    for s, p in sorted(probs.items()):
        cdf[s] = (cum, cum + p)
        cum += p

    low, high = 0.0, 1.0
    for s in seq:
        r = high - low
        s_low, s_high = cdf[s]
        low = low + r * s_low
        high = low + r * (s_high - s_low)

    return (low + high) / 2

def arithmetic_decode(cdf_dict, encoded, n_symbols):
    low, high = 0.0, 1.0
    symbols = []
    for _ in range(n_symbols):
        r = high - low
        val = (encoded - low) / r
        for s, (s_low, s_high) in cdf_dict.items():
            if s_low <= val < s_high:
                symbols.append(s)
                low = low + r * s_low
                high = low + r * (s_high - s_low)
                break
    return symbols

probs_abc = {"A": 0.5, "B": 0.3, "C": 0.2}
seq = "ABC"
encoded = arithmetic_encode(probs_abc, seq)
print(f"Encoded value: {encoded:.6f}")

cdf = {}
cum = 0.0
for s, p in sorted(probs_abc.items()):
    cdf[s] = (cum, cum + p)
    cum += p
decoded = arithmetic_decode(cdf, encoded, 3)
print(f"Decoded: {''.join(decoded)}")
```

**Output size.** For a $k$-symbol alphabet and $n$ input symbols, the output is a binary fraction of length approximately $nH(X)$ bits. Arithmetic coding can compress arbitrarily close to the entropy for any distribution, making it the preferred method in high-performance compressors.

### Lempel-Ziv (LZ77)

LZ77 (1977) takes a fundamentally different approach — it does not require a known source distribution. Instead, it exploits repeating patterns in the data by storing **pointers** to previous occurrences.

**Algorithm:**

- Maintain a sliding window (typically 32 KB to 64 KB).
- At each position, find the longest match of the upcoming bytes in the window.
- Output a triple: (distance back, match length, next character).
- If no match exists, output (0, 0, character).

```
Input: "abracadabracadabra"

Window: [a b r a c a d a b r a c a d a b r a]

Position 14: looking for match...
  "abracadabra" at position 12  ->  distance=2, length=11
  Output: (2, 11)

Encoding: literals and (distance, length) pairs
```

The sliding window approach exploits **local** redundancy, which makes LZ77 work well for text, source code, and logs.

```python
def lz77_encode(data, window_size=256):
    i = 0
    encoded = []
    while i < len(data):
        match_len = 0
        match_dist = 0
        start = max(0, i - window_size)
        for j in range(start, i):
            k = 0
            while i + k < len(data) and j + k < i and data[j + k] == data[i + k]:
                k += 1
            if k > match_len:
                match_len = k
                match_dist = i - j
        if match_len > 2:
            next_char = data[i + match_len] if i + match_len < len(data) else ""
            encoded.append((match_dist, match_len, next_char))
            i += match_len + 1
        else:
            encoded.append((0, 0, data[i]))
            i += 1
    return encoded

encoded = lz77_encode("abracadabracadabra")
print(encoded)
```

### Lempel-Ziv (LZ78)

LZ78 (1978) constructs a dictionary incrementally, replacing repeated phrases with dictionary indices:

**Algorithm:**

- Maintain a dictionary of phrases seen so far (initially empty).
- At each position, find the longest prefix that matches a dictionary entry.
- Output (index, next character).
- Add the new phrase (matched prefix + next character) to the dictionary.

| Step | Input | Dictionary | Output |
|---|---|---|---|
| 1 | a | 1 = "a" | (0, 'a') |
| 2 | b | 2 = "b" | (0, 'b') |
| 3 | r | 3 = "r" | (0, 'r') |
| 4 | a | (already in dict) | — |
| 5 | c | 4 = "ac" | (1, 'c') |
| 6 | a | (already in dict) | — |
| 7 | d | 5 = "ad" | (1, 'd') |
| ... | ... | ... | ... |

LZ78 builds an explicit dictionary that grows indefinitely (unlike LZ77's sliding window). Its variant LZW is used in GIF and Unix `compress`.

### Comparison of Compression Algorithms

| Algorithm | Type | Distribution needed? | Memory | Redundancy | Typical use |
|---|---|---|---|---|---|
| Huffman | Symbol-wise | Yes | Low | Up to 1 bit/symbol | DEFLATE (zip, gzip), JPEG |
| Arithmetic | Block | Yes | Low | Near zero | JPEG 2000, H.264, bzip2 |
| LZ77 | Dictionary | No | Medium (window) | Varies | gzip, PNG, ZIP, HTTP |
| LZ78/LZW | Dictionary | No | High (dictionary) | Varies | GIF, Unix compress |
| LZMA | Dictionary + range | No | High | Very low | 7z, xz |

**Combined approaches:** Most modern compressors combine LZ77 with entropy coding:
- **DEFLATE** (zip, gzip, PNG): LZ77 + Huffman coding.
- **LZMA** (7z, xz): LZ77-like + range coding (similar to arithmetic coding).
- **bzip2**: Burrows-Wheeler transform + move-to-front + Huffman coding.

The effectiveness of a compression algorithm depends on the data type. Text files with repeated patterns compress well with LZ-family algorithms. Executables compress better with prediction-based algorithms. Already-compressed data (JPEG, MP3, video) cannot be compressed further — in fact, attempting to compress them often increases file size.

### Redundancy and Code Efficiency

**Code efficiency** measures how close a code is to the entropy bound:

$$
\eta = \frac{H(X)}{\mathbb{E}[L]} \times 100\%
$$

An efficiency of 95% means the code uses 5% more bits than the theoretical minimum.

**Redundancy** is the difference:

$$
R = \mathbb{E}[L] - H(X)
$$

**Sources of redundancy:**

1. **Integer codeword length constraint:** Huffman codes round lengths up to the nearest integer, losing at most 1 bit per symbol. Arithmetic codes avoid this entirely.

2. **Model mismatch:** If the estimated distribution differs from the true distribution, the code will be suboptimal. Cross-entropy measures the penalty:

   $$
   \mathbb{E}_{p}[L] = H(p) + D_{KL}(p \parallel q)
   $$

3. **Block coding overhead:** Fixed-size blocks may end with partially filled codewords.

4. **Algorithmic suboptimality:** LZ algorithms can miss patterns that would be caught by more sophisticated models.

### Applications in Computing

**gzip and DEFLATE.** The DEFLATE algorithm (used in gzip, zip, PNG) combines LZ77 with Huffman coding. LZ77 finds duplicate strings and replaces them with (distance, length) pairs. The resulting literal/length and distance streams are Huffman coded:

```python
import gzip
import os

data = b"Hello world! " * 1000
compressed = gzip.compress(data)
print(f"Original: {len(data)} bytes")
print(f"Compressed: {len(compressed)} bytes")
print(f"Ratio: {100 * len(compressed) / len(data):.1f}%")
```

**PNG image format.** PNG uses DEFLATE on prediction residuals. Each pixel is predicted from its neighbors (filtering), and the prediction errors are less entropic than raw pixel values, making DEFLATE more effective.

**Video compression (H.264, H.265).** Video codecs use motion compensation (finding similar blocks in adjacent frames) to compute residuals, then apply DCT transformation, quantization, and arithmetic coding to the residual signal.

**Huffman coding in JPEG:**

```python
def jpeg_huffman_style_encoding(symbols, probs):
    tree = build_huffman_tree(probs)
    codes = build_huffman_codes(tree)
    encoded = "".join(codes[s] for s in symbols)
    avg_len = sum(len(codes[s]) * probs[s] for s in probs)
    entropy_val = -sum(p * math.log2(p) for p in probs.values())
    print(f"Average code length: {avg_len:.4f}")
    print(f"Entropy: {entropy_val:.4f}")
    print(f"Efficiency: {100 * entropy_val / avg_len:.2f}%")
    return encoded
```

## Study Cases

### Case 1: Designing a Compression Scheme for Log Files

Server logs contain repeated patterns: timestamps, log levels, and module names. Design a specialized compressor:

```python
import re
from collections import Counter

log_sample = """
2024-01-15 10:30:01 INFO  server.started listening on port 8080
2024-01-15 10:30:02 INFO  database.connected connection pool size=10
2024-01-15 10:30:05 WARN  cache.miss key=user:12345
2024-01-15 10:30:06 INFO  request.get /api/users/12345
2024-01-15 10:30:07 INFO  database.query SELECT * FROM users
2024-01-15 10:30:08 WARN  cache.miss key=user:67890
2024-01-15 10:30:09 INFO  request.get /api/users/67890
2024-01-15 10:30:10 ERROR database.timeout query exceeded 5000ms
"""

tokens = re.findall(r'\S+', log_sample)
freq = Counter(tokens)
probs = {t: freq[t] / len(tokens) for t in freq}

print(f"Token entropy: {entropy(list(probs.values())):.3f} bits/token")
print(f"Token count: {len(tokens)}")
print(f"Theoretical min size: {len(tokens) * entropy(list(probs.values())) / 8:.0f} bytes")
```

### Case 2: Comparing Huffman and LZW on Text

```python
import numpy as np

text = "the quick brown fox jumps over the lazy dog the quick brown fox jumps over the lazy dog"
freq = Counter(text)
probs = {c: freq[c] / len(text) for c in freq}

# Huffman
tree = build_huffman_tree(probs)
codes = build_huffman_codes(tree)
huffman_bits = sum(len(codes[c]) * probs[c] for c in probs) * len(text)
print(f"Huffman: {huffman_bits / 8:.0f} bytes")

# LZ77 naive estimate
encoded_lz = lz77_encode(text)
unique_literals = sum(1 for d, l, c in encoded_lz if d == 0)
matches = len(encoded_lz) - unique_literals
print(f"LZ77: {len(encoded_lz)} tokens ({matches} matches, {unique_literals} literals)")
```

### Case 3: Redundancy in Natural Language

English text has significant redundancy, about 50-75%. This is why compression works:

```python
text = """Information theory is the scientific study of the quantification,
storage, and communication of information. The field was fundamentally
established by the works of Harry Nyquist and Ralph Hartley, and later
Claude Shannon in the 1940s."""

# Character-level entropy
char_freq = Counter(text.lower())
char_probs = [c / len(text) for c in char_freq.values()]
char_entropy = -sum(p * np.log2(p) for p in char_probs)
print(f"Character entropy: {char_entropy:.3f} bits/char")
print(f"ASCII uses 8 bits/char")
print(f"Redundancy: {100 * (1 - char_entropy / 8):.1f}%")
```

## Examples

### Example 1: Optimal Code for a Biased Coin

```python
probs = {"H": 0.8, "T": 0.2}
tree = build_huffman_tree(probs)
codes = build_huffman_codes(tree)
H_val = entropy(list(probs.values()))
L_val = sum(len(codes[s]) * probs[s] for s in probs)
print(f"Entropy: {H_val:.4f}")
print(f"Avg length: {L_val:.4f}")
print(f"Codes: {codes}")
```

### Example 2: Kraft Inequality Check

```python
def kraft_check(lengths):
    return sum(2**(-l) for l in lengths)

lengths1 = [1, 2, 2, 3]  # e.g., {0, 10, 11, 100}
lengths2 = [1, 2, 2, 2]  # e.g., {0, 10, 11, 111}
print(f"Kraft for [1,2,2,3]: {kraft_check(lengths1):.4f} (<=1: {kraft_check(lengths1) <= 1})")
print(f"Kraft for [1,2,2,2]: {kraft_check(lengths2):.4f} (<=1: {kraft_check(lengths2) <= 1})")
```

### Example 3: Arithmetic Coding vs. Huffman for a Non-Dyadic Distribution

```python
probs = {"A": 0.7, "B": 0.15, "C": 0.15}

# Huffman
tree = build_huffman_tree(probs)
codes = build_huffman_codes(tree)
L_huff = sum(len(codes[s]) * probs[s] for s in probs)
H_val = entropy(list(probs.values()))

# Arithmetic (approximate bits = -log2(interval width))
seq = "A" * 50 + "B" * 20 + "C" * 20
encoded_val = arithmetic_encode(probs, seq)
# Theoretical min arithmetic bits = len(seq) * H_val
arith_bits = len(seq) * H_val
huff_bits = sum(len(codes[c]) for c in seq)  # actual

print(f"Entropy: {H_val:.4f} bits/symbol")
print(f"Huffman avg: {L_huff:.4f} bits/symbol")
print(f"Sequence entropy bound: {arith_bits:.0f} bits")
print(f"Huffman actual: {huff_bits} bits")
print(f"Arithmetic achieves near-entropy by not rounding to integers")
```

### Example 4: Prefix Code Tree Construction

```python
def print_code_tree(node, depth=0):
    if node.symbol:
        print("  " * depth + f"Leaf: {node.symbol} (p={node.prob})")
    else:
        print("  " * depth + f"Node (p={node.prob:.2f})")
        if node.left:
            print_code_tree(node.left, depth + 1)
        if node.right:
            print_code_tree(node.right, depth + 1)

probs = {"A": 0.35, "B": 0.25, "C": 0.2, "D": 0.12, "E": 0.08}
tree = build_huffman_tree(probs)
print_code_tree(tree)
```

## Glossary

| Term | Definition |
|---|---|
| Source coding theorem | Lossless compression cannot exceed entropy; optimal codes exist achieving $L < H + 1$ |
| Prefix code | Code where no codeword is a prefix of another, enabling instantaneous decoding |
| Kraft inequality | $\sum 2^{-\ell_i} \leq 1$, necessary and sufficient condition for prefix code existence |
| Huffman coding | Optimal prefix code for a given distribution using a binary tree merging algorithm |
| Arithmetic coding | Block code encoding entire message as a fraction in $[0, 1)$, approaching entropy arbitrarily closely |
| LZ77 | Sliding-window dictionary compression using (distance, length, next) triples |
| LZ78 | Incremental dictionary compression using (index, character) pairs |
| DEFLATE | Combined LZ77 + Huffman coding, used in gzip, zip, PNG |
| Asymptotic equipartition property (AEP) | For large $n$, sequences partition into a typical set of size $\approx 2^{nH}$ |
| Redundancy | Difference between expected code length and entropy: $R = \mathbb{E}[L] - H$ |
| Code efficiency | $\eta = H / \mathbb{E}[L] \times 100\%$ |
| McMillan theorem | Kraft inequality is also sufficient for the existence of a prefix code |
| Instantaneous code | Another name for a prefix code |
| Self-information | Information content of a single outcome: $-\log_2 p(x)$ |

## Quick References

- [Huffman, D. A. (1952). A Method for the Construction of Minimum-Redundancy Codes](https://doi.org/10.1109/JRPROC.1952.273898) — the original Huffman coding paper
- [Ziv, J. & Lempel, A. (1977). A Universal Algorithm for Sequential Data Compression](https://doi.org/10.1109/TIT.1977.1055714) — LZ77 original paper
- [Witten, I. H., Neal, R. M., & Cleary, J. G. (1987). Arithmetic Coding for Data Compression](https://doi.org/10.1145/214762.214771) — practical arithmetic coding
- [Deutsch, P. (1996). DEFLATE Compressed Data Format Specification (RFC 1951)](https://tools.ietf.org/html/rfc1951) — the DEFLATE standard used in gzip and PNG

## Next Steps

- [Rate-Distortion](rate-distortion.md) — lossy compression framework: how much distortion must be accepted for a given compression rate
- [Error-Correcting Codes](error-correcting-codes.md) — adding redundancy to protect against channel noise (inverse problem of source coding)
- [Channel Capacity](channel-capacity.md) — maximum rate of reliable communication over noisy channels
