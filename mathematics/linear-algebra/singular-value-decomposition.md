# Singular Value Decomposition

## Description

The singular value decomposition (SVD) is the fundamental matrix factorization — it exists for every matrix (square or rectangular, invertible or singular) and reveals its geometric structure completely. SVD decomposes a matrix into rotations, scaling, and another rotation, capturing the rank, range, null space, and optimal low-rank approximation. It powers PCA, dimensionality reduction, image compression, recommendation systems, latent semantic analysis, and virtually every data-driven algorithm.

## Prerequisites

- [Eigenvalues & Eigenvectors](eigenvalues-and-eigenvectors.md) — spectral decomposition, eigendecomposition of symmetric matrices
- [Orthogonality & Least Squares](orthogonality-and-least-squares.md) — orthogonal matrices, orthogonal projection, least squares

## Table of Contents

- [The SVD Theorem](#the-svd-theorem)
- [Full vs. Reduced SVD](#full-vs-reduced-svd)
- [Geometric Interpretation](#geometric-interpretation)
- [Computing the SVD](#computing-the-svd)
- [Properties and Relationships](#properties-and-relationships)
- [Low-Rank Approximation](#low-rank-approximation)
- [The Pseudoinverse](#the-pseudoinverse)
- [Applications](#applications)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The SVD Theorem

**Theorem:** Every matrix $A \in \mathbb{R}^{m \times n}$ can be factored as:

$$
A = U \Sigma V^T
$$

where:
- $U \in \mathbb{R}^{m \times m}$ is orthogonal ($U^T U = I_m$)
- $V \in \mathbb{R}^{n \times n}$ is orthogonal ($V^T V = I_n$)
- $\Sigma \in \mathbb{R}^{m \times n}$ is diagonal with non-negative entries $\sigma_1 \geq \sigma_2 \geq \dots \geq \sigma_r > 0$ (the **singular values**), where $r = \text{rank}(A)$

```python
import numpy as np

A = np.array([[3, 1, -1],
              [1, 3, 1],
              [-1, 1, 3]])
U, s, Vt = np.linalg.svd(A)
print(f"U shape: {U.shape}")
print(f"s: {s}")
print(f"Vt shape: {Vt.shape}")

# Reconstruct
Sigma = np.zeros(A.shape)
Sigma[:len(s), :len(s)] = np.diag(s)
A_reconstructed = U @ Sigma @ Vt
print(f"Reconstruction error: {np.linalg.norm(A - A_reconstructed):.2e}")
```

**The columns of $U$** are the **left singular vectors** (eigenvectors of $AA^T$). They form an orthonormal basis for the column space.

**The columns of $V$** (rows of $V^T$) are the **right singular vectors** (eigenvectors of $A^T A$). They form an orthonormal basis for the row space.

**The singular values** $\sigma_i$ are the square roots of the non-zero eigenvalues of $A^T A$ (or $AA^T$):

$$
\sigma_i = \sqrt{\lambda_i(A^T A)} = \sqrt{\lambda_i(A A^T)}
$$

```python
# Verify: singular values are sqrt(eigenvalues of A^T A)
A = np.array([[2, 1],
              [1, 2]])

U, s, Vt = np.linalg.svd(A)
ATA_evals = np.linalg.eigvalsh(A.T @ A)  # eigh for symmetric
print(f"Singular values from SVD: {np.sort(s)[::-1]}")
print(f"Sqrt(eigvals of A^T A): {np.sqrt(np.sort(ATA_evals)[::-1])}")
```

### Full vs. Reduced SVD

**Full SVD:** $U$ is $m \times m$, $\Sigma$ is $m \times n$, $V^T$ is $n \times n$.

**Reduced (economy) SVD:** Only the first $r = \text{rank}(A)$ columns of $U$ and rows of $V^T$ are kept:

$$
A = U_r \Sigma_r V_r^T
$$

where $U_r \in \mathbb{R}^{m \times r}$, $\Sigma_r \in \mathbb{R}^{r \times r}$, $V_r \in \mathbb{R}^{n \times r}$.

```python
A = np.array([[1, 2, 3],
              [4, 5, 6],
              [7, 8, 9],
              [10, 11, 12]])

# Full SVD
U_full, s_full, Vt_full = np.linalg.svd(A, full_matrices=True)
print(f"Full: U={U_full.shape}, Σ_vec={s_full.shape}, V^T={Vt_full.shape}")

# Economy SVD
U_econ, s_econ, Vt_econ = np.linalg.svd(A, full_matrices=False)
print(f"Economy: U={U_econ.shape}, Σ_vec={s_econ.shape}, V^T={Vt_econ.shape}")

# Economy matches: U_econ shape is (m, min(m,n))
assert U_econ.shape == (A.shape[0], min(A.shape))

# Both reconstruct equally
r = A.shape[1]
Sigma_full = np.zeros(A.shape)
Sigma_full[:r, :r] = np.diag(s_full)
print(f"Full reconstruction: {np.linalg.norm(A - U_full @ Sigma_full @ Vt_full):.2e}")

Sigma_econ = np.diag(s_econ)
print(f"Econ reconstruction: {np.linalg.norm(A - U_econ @ Sigma_econ @ Vt_econ):.2e}")
```

### Geometric Interpretation

SVD reveals the geometry of a linear transformation $T(\mathbf{x}) = A\mathbf{x}$ as a three-step process:

1. **Rotate/reflect** by $V^T$ (change of basis in the domain)
2. **Scale** by $\Sigma$ (stretch along coordinate axes)
3. **Rotate/reflect** by $U$ (change of basis in the codomain)

$$
A\mathbf{x} = U (\Sigma (V^T \mathbf{x}))
$$

```python
def svd_geometric(A, x):
    """Apply A @ x via the three SVD steps."""
    U, s, Vt = np.linalg.svd(A)

    # Step 1: rotate by V^T (domain rotation)
    y1 = Vt @ x

    # Step 2: scale by sigma
    y2 = s * y1[:len(s)]  # only r components are scaled

    # Step 3: rotate by U (codomain rotation)
    y3 = U[:, :len(s)] @ y2

    direct = A @ x
    print(f"Direct:  {np.round(direct, 4)}")
    print(f"SVD:     {np.round(y3, 4)}")
    print(f"Match? {np.allclose(direct, y3)}")
    return y3

A = np.array([[2, 1],
              [1, 2]])
x = np.array([1, 0])
svd_geometric(A, x)
```

**The image of the unit sphere:** Under $A$, the unit sphere in $\mathbb{R}^n$ maps to an ellipsoid in $\mathbb{R}^m$ whose semi-axis lengths are the singular values $\sigma_i$ and directions are the left singular vectors $\mathbf{u}_i$.

```python
# SVD of a 2x2 matrix shows the ellipse axes
A = np.array([[3, 1],
              [1, 2]])
U, s, Vt = np.linalg.svd(A)

# Unit circle vectors
theta = np.linspace(0, 2*np.pi, 100)
circle = np.column_stack([np.cos(theta), np.sin(theta)])

# Transformed circle (ellipse)
ellipse = circle @ A.T  # or (A @ circle.T).T

# The semi-axes of the ellipse are sigma_i * u_i
axes = s[:, None] * U.T  # each row is an axis direction
print(f"Singular values (axis lengths): {s}")
print(f"Axis directions (left singular vectors):\n{U}")
```

### Computing the SVD

**Relationship to eigendecomposition:**

The SVD can be computed via eigendecompositions of $A^T A$ and $A A^T$:

- $V$: eigenvectors of $A^T A$ (sorted by descending eigenvalues)
- $U$: eigenvectors of $A A^T$
- $\sigma_i = \sqrt{\lambda_i}$ where $\lambda_i$ are eigenvalues of $A^T A$ or $A A^T$

```python
def svd_via_eig(A):
    """Compute SVD via eigendecomposition of A^T A and A A^T."""
    # Right singular vectors: eigenvectors of A^T A
    ATA = A.T @ A
    evals_ATA, V = np.linalg.eigh(ATA)
    # Sort by descending eigenvalue
    idx = np.argsort(evals_ATA)[::-1]
    evals_ATA = evals_ATA[idx]
    V = V[:, idx]

    # Singular values
    s = np.sqrt(np.maximum(evals_ATA, 0))
    r = np.sum(s > 1e-10)

    # Left singular vectors: U = A V Sigma^{-1}
    U = A @ V / s

    return U, s, V.T  # V.T matches standard SVD convention

A = np.array([[2, 1],
              [1, 2],
              [0, 1]])
U_my, s_my, Vt_my = svd_via_eig(A)
U_np, s_np, Vt_np = np.linalg.svd(A)

print(f"My singular values: {np.round(s_my, 4)}")
print(f"NumPy singular values: {np.round(s_np, 4)}")
```

**In practice:** NumPy's `np.linalg.svd` uses highly optimized LAPACK routines (DGESDD divide-and-conquer). SciPy provides additional variants.

```python
# Different SVD implementations
from scipy.linalg import svd as svd_scipy

A = np.random.randn(100, 50)

# NumPy
U1, s1, Vt1 = np.linalg.svd(A, full_matrices=False)

# SciPy
U2, s2, Vt2 = svd_scipy(A, full_matrices=False)

print(f"Match: {np.allclose(s1, s2)}")
print(f"Singular value range: [{s1[-1]:.4e}, {s1[0]:.4f}]")
```

**Computational cost:** For an $m \times n$ matrix, SVD costs $O(mn \cdot \min(m, n))$ operations. For large matrices, randomized SVD provides a faster approximate alternative:

```python
# Randomized SVD (approximate, for large matrices)
# Using sklearn's implementation
from sklearn.utils.extmath import randomized_svd

A_large = np.random.randn(1000, 2000)
U_rsvd, s_rsvd, Vt_rsvd = randomized_svd(A_large, n_components=50, random_state=42)

# Full SVD (may be slower)
U_svd, s_svd, Vt_svd = np.linalg.svd(A_large, full_matrices=False)

print(f"Full SVD top-3 singular values: {s_svd[:3]}")
print(f"Randomized SVD top-3: {s_rsvd[:3]}")
print(f"Match: {np.allclose(s_svd[:50], s_rsvd, atol=1e-2)}")
```

### Properties and Relationships

**SVD vs. eigendecomposition:**

| Property | SVD | Eigendecomposition |
|----------|-----|--------------------|
| Exists for | Every matrix ($m \times n$) | Square diagonalizable matrices |
| Singular values / eigenvalues | Always real, non-negative | Can be complex |
| Factors | $U \Sigma V^T$ | $PDP^{-1}$ |
| Orthogonal | Yes ($U^T U = I$, $V^T V = I$) | Only for symmetric/normal |
| Column/row spaces | Directly revealed | Must compute separately |

**SVD reveals the four fundamental subspaces:**

- Column space: $\text{Col}(A) = \text{span}\{\mathbf{u}_1, \dots, \mathbf{u}_r\}$
- Null space: $\text{Null}(A) = \text{span}\{\mathbf{v}_{r+1}, \dots, \mathbf{v}_n\}$
- Row space: $\text{Row}(A) = \text{span}\{\mathbf{v}_1, \dots, \mathbf{v}_r\}$
- Left null space: $\text{Null}(A^T) = \text{span}\{\mathbf{u}_{r+1}, \dots, \mathbf{u}_m\}$

```python
def four_subspaces_svd(A):
    """Extract the four fundamental subspaces from SVD."""
    U, s, Vt = np.linalg.svd(A)
    r = np.sum(s > 1e-10)

    col_space = U[:, :r]
    null_space = Vt[r:, :].T
    row_space = Vt[:r, :].T
    left_null = U[:, r:]

    print(f"Column space basis: dim={r}")
    print(f"Null space basis: dim={A.shape[1] - r}")
    print(f"Row space basis: dim={r}")
    print(f"Left null space: dim={A.shape[0] - r}")

    # Verify orthogonality
    print(f"Col ⟂ Left null? {np.allclose(col_space.T @ left_null, 0)}")
    print(f"Row ⟂ Null? {np.allclose(row_space.T @ null_space, 0)}")

    return col_space, null_space, row_space, left_null

A = np.array([[1, 2, 0, -1],
              [0, 1, 3, 2],
              [2, 4, 0, -2],
              [1, 0, -3, -3]])
four_subspaces_svd(A)
```

**Key identities:**

- $\|A\|_F = \sqrt{\sum \sigma_i^2}$ (Frobenius norm)
- $\|A\|_2 = \sigma_{\max}$ (spectral norm)
- $\|A\|_* = \sum \sigma_i$ (nuclear norm)
- $\det(A) = \prod \sigma_i$ for square $A$
- $\text{rank}(A) = \text{number of non-zero singular values}$
- $\kappa(A) = \sigma_{\max} / \sigma_{\min}$ (condition number)

```python
A = np.random.randn(5, 3)
U, s, Vt = np.linalg.svd(A)

print(f"Frobenius norm: {np.linalg.norm(A, 'fro'):.4f}")
print(f"via SVD: {np.sqrt(np.sum(s**2)):.4f}")
print(f"Spectral norm: {np.linalg.norm(A, 2):.4f}")
print(f"via SVD: {s[0]:.4f}")
print(f"Nuclear norm (sum of σ): {np.sum(s):.4f}")
print(f"Condition number: {s[0]/s[-1]:.4f}")
print(f"Rank: {np.sum(s > 1e-10)}")
```

### Low-Rank Approximation

The **Eckart-Young-Mirsky theorem** states that the best rank-$k$ approximation to $A$ (in both Frobenius and spectral norms) is given by truncating the SVD to the $k$ largest singular values:

$$
A_k = \sum_{i=1}^{k} \sigma_i \mathbf{u}_i \mathbf{v}_i^T = U_k \Sigma_k V_k^T
$$

```python
def low_rank_approximation(A, k):
    """Best rank-k approximation via truncated SVD."""
    U, s, Vt = np.linalg.svd(A, full_matrices=False)
    U_k = U[:, :k]
    s_k = s[:k]
    Vt_k = Vt[:k, :]
    return U_k @ np.diag(s_k) @ Vt_k, s

A = np.random.randn(20, 15)
U, s, Vt = np.linalg.svd(A, full_matrices=False)

for k in [1, 3, 5, 10, 15]:
    A_k, _ = low_rank_approximation(A, k)
    error_frob = np.linalg.norm(A - A_k, 'fro')
    error_spec = np.linalg.norm(A - A_k, 2)
    print(f"k={k:2d}: Frobenius error = {error_frob:.4f}, "
          f"Spectral error = {error_spec:.4f}, "
          f"Next σ = {s[k] if k < len(s) else 0:.4f}")
```

**Error from optimal rank-$k$ approximation:**

$$
\|A - A_k\|_2 = \sigma_{k+1}
$$

$$
\|A - A_k\|_F = \sqrt{\sum_{i=k+1}^{r} \sigma_i^2}
```

```python
# Verify Eckart-Young theorem
k = 3
A_k, s = low_rank_approximation(A, k)
spec_error = np.linalg.norm(A - A_k, 2)
print(f"Actual spectral error: {spec_error:.6f}")
print(f"σ_{k+1} (predicted): {s[k]:.6f}")

frob_error = np.linalg.norm(A - A_k, 'fro')
predicted_frob = np.sqrt(np.sum(s[k:]**2))
print(f"Actual Frobenius error: {frob_error:.6f}")
print(f"Predicted (sqrt sum σ²): {predicted_frob:.6f}")
```

### The Pseudoinverse

The **Moore-Penrose pseudoinverse** $A^+$ generalizes the matrix inverse to non-square or singular matrices. Using SVD:

$$
A^+ = V \Sigma^+ U^T
$$

where $\Sigma^+$ is formed by taking the reciprocal of each non-zero singular value and transposing:

```python
def pseudoinverse(A):
    """Compute the Moore-Penrose pseudoinverse via SVD."""
    U, s, Vt = np.linalg.svd(A, full_matrices=False)
    r = np.sum(s > 1e-10)

    # Reciprocal of non-zero singular values
    s_inv = np.zeros_like(s)
    s_inv[:r] = 1.0 / s[:r]

    # A^+ = V @ diag(s_inv) @ U^T
    return Vt.T @ np.diag(s_inv) @ U.T

A = np.array([[1, 2, 3],
              [4, 5, 6]])
A_pinv = pseudoinverse(A)
A_pinv_np = np.linalg.pinv(A)

print(f"My pinv:\n{np.round(A_pinv, 4)}")
print(f"NumPy pinv:\n{np.round(A_pinv_np, 4)}")
print(f"Match: {np.allclose(A_pinv, A_pinv_np)}")

# Verify four Penrose conditions:
# 1. A A^+ A = A
print(f"1. A A⁺ A = A: {np.allclose(A @ A_pinv @ A, A)}")
# 2. A^+ A A^+ = A^+
print(f"2. A⁺ A A⁺ = A⁺: {np.allclose(A_pinv @ A @ A_pinv, A_pinv)}")
# 3. (A A^+) is symmetric
print(f"3. (A A⁺) symmetric: {np.allclose(A @ A_pinv, (A @ A_pinv).T)}")
# 4. (A^+ A) is symmetric
print(f"4. (A⁺ A) symmetric: {np.allclose(A_pinv @ A, (A_pinv @ A).T)}")
```

**Using pseudoinverse to solve linear systems:**

For $A\mathbf{x} \approx \mathbf{b}$, the minimum-norm least squares solution is $\mathbf{x} = A^+ \mathbf{b}$:

```python
A = np.array([[1, 2],
              [3, 4],
              [5, 6]])
b = np.array([3, 7, 11])

x_pinv = np.linalg.pinv(A) @ b
x_lstsq = np.linalg.lstsq(A, b, rcond=None)[0]
print(f"Via pseudoinverse: {x_pinv}")
print(f"Via lstsq: {x_lstsq}")
print(f"Match: {np.allclose(x_pinv, x_lstsq)}")
```

### Applications

**PCA via SVD:** Principal Component Analysis is computed directly from the SVD of the centered data matrix, avoiding the explicit covariance matrix.

```python
def pca_svd(X, n_components=None):
    """PCA via SVD (more numerically stable than eigendecomposition of cov)."""
    X_centered = X - X.mean(axis=0)
    U, s, Vt = np.linalg.svd(X_centered, full_matrices=False)

    if n_components is None:
        n_components = X.shape[1]

    components = Vt[:n_components].T
    explained_variance = s**2 / (X.shape[0] - 1)
    projected = X_centered @ components

    return projected, components, explained_variance[:n_components]

# Compare with sklearn
from sklearn.decomposition import PCA
from sklearn.datasets import load_digits

digits = load_digits()
X = digits.data

# Our SVD-based PCA
projected_our, comp_our, var_our = pca_svd(X, n_components=10)

# sklearn PCA
pca = PCA(n_components=10)
projected_sk = pca.fit_transform(X)

print(f"Explained variance ratio (first 5): {var_our[:5] / var_our.sum():.4f}")
print(f"Components match? {np.allclose(np.abs(projected_our), np.abs(projected_sk), atol=1e-6)}")
```

**Image compression:** Store a rank-$k$ approximation instead of the full image.

```python
from scipy.misc import face

# Load a grayscale image
img = face(gray=True)
print(f"Original shape: {img.shape}")

# Compress via truncated SVD
U, s, Vt = np.linalg.svd(img, full_matrices=False)

for k in [5, 20, 50, 100]:
    img_compressed = U[:, :k] @ np.diag(s[:k]) @ Vt[:k, :]
    compression_ratio = 1 - (k * (img.shape[0] + img.shape[1] + 1)) / (img.shape[0] * img.shape[1])
    error = np.linalg.norm(img - img_compressed) / np.linalg.norm(img)
    print(f"k={k:3d}: compression={1-compression_ratio:.3f}x, relative error={error:.4f}")
```

**Recommendation systems:** Matrix factorization for collaborative filtering uses truncated SVD (or similar) to predict user ratings.

```python
# Toy recommendation: 5 users, 4 items
R = np.array([
    [5, 3, 0, 1],
    [4, 0, 0, 1],
    [1, 1, 0, 5],
    [1, 0, 0, 4],
    [0, 1, 5, 4]
])

# Truncated SVD for prediction (k=2)
U, s, Vt = np.linalg.svd(R, full_matrices=False)
k = 2
R_pred = U[:, :k] @ np.diag(s[:k]) @ Vt[:k, :]
print(f"Predicted ratings:\n{np.round(R_pred, 1)}")
# The zeros are filled in with predicted values!
```

**Latent semantic analysis (LSA):** In NLP, SVD of the term-document matrix reveals latent topics.

```python
# Term-document matrix (5 terms, 4 documents)
term_doc = np.array([
    [3, 5, 0, 1],   # "math"
    [2, 4, 0, 0],   # "vector"
    [0, 0, 4, 3],   # "car"
    [1, 0, 5, 4],   # "engine"
    [0, 0, 2, 1],   # "tire"
])

U, s, Vt = np.linalg.svd(term_doc, full_matrices=False)
print(f"Singular values: {np.round(s, 2)}")

# Topics (right singular vectors = document concepts)
print(f"\nDocument-topic matrix (first 2 topics):\n{np.round(Vt[:2], 3)}")
# Terms loadings on topics
print(f"\nTerm-topic matrix (first 2 topics):\n{np.round(U[:, :2], 3)}")
```

## Study Cases

### Study Case 1: Image Compression

Compress a real image using truncated SVD and analyze the trade-off.

```python
from sklearn.datasets import load_sample_image
import numpy as np

# Load flower image
china = load_sample_image('china.jpg')
img = china.mean(axis=2).astype(float)  # grayscale
print(f"Image size: {img.shape}")

# SVD
U, s, Vt = np.linalg.svd(img, full_matrices=False)

total_pixels = img.shape[0] * img.shape[1]
total_storage = img.shape[0] * img.shape[1]  # bytes (per pixel)

print("\nCompression analysis:")
for k in [5, 10, 20, 50, 100, 200]:
    storage_k = k * (img.shape[0] + img.shape[1] + 1)
    ratio = storage_k / total_storage
    error = 1 - np.sum(s[:k]**2) / np.sum(s**2)
    print(f"k={k:3d}: storage={ratio:.1%} of original, rel_error={error:.4f}")

# The optimal k for 50% storage
target = 0.5
cumulative_energy = np.cumsum(s**2) / np.sum(s**2)
k_50 = np.searchsorted(cumulative_energy, target) + 1
print(f"\nTo retain 50% energy: k={k_50}")
```

### Study Case 2: Noise Reduction via Low-Rank Approximation

Noise is often spread across many singular modes; the signal concentrates in the top few.

```python
np.random.seed(42)
# Low-rank signal
true_signal = np.random.randn(50, 5) @ np.random.randn(5, 50)
noise = np.random.randn(50, 50) * 2
noisy = true_signal + noise

print(f"True rank: {np.linalg.matrix_rank(true_signal)}")
print(f"Noisy rank: {np.linalg.matrix_rank(noisy)}")

# SVD denoising
U, s, Vt = np.linalg.svd(noisy, full_matrices=False)
print(f"Singular values: {np.round(s[:10], 2)}")
# Typically a gap between signal and noise singular values

r_estimate = 5  # known from problem
denoised = U[:, :r_estimate] @ np.diag(s[:r_estimate]) @ Vt[:r_estimate, :]

print(f"Denoising MSE: {np.mean((denoised - true_signal)**2):.4f}")
print(f"Noisy MSE: {np.mean((noisy - true_signal)**2):.4f}")
```

### Study Case 3: Collaborative Filtering for Movie Recommendations

```python
# 6 users, 5 movies, ratings 1-5 (0 = unknown)
ratings = np.array([
    [5, 4, 0, 0, 2],
    [4, 5, 0, 0, 1],
    [0, 0, 4, 5, 0],
    [0, 0, 5, 4, 0],
    [3, 2, 0, 0, 5],
    [2, 3, 0, 0, 4]
])

# Matrix factorization with k=2 latent factors
U, s, Vt = np.linalg.svd(ratings, full_matrices=False)
k = 2
pred = U[:, :k] @ np.diag(s[:k]) @ Vt[:k, :]

print("Predicted ratings:\n", np.round(pred, 1))

# Rec For user 0: which unwatched movies to recommend?
user0 = 0
unwatched = np.where(ratings[user0] == 0)[0]
print(f"\nRecommendations for user {user0}:")
for movie in unwatched:
    print(f"  Movie {movie}: predicted {pred[user0, movie]:.2f}")

# User similarity via latent vectors
user_factors = U[:, :k] * s[:k]
user0_vec = user_factors[0]
user1_vec = user_factors[1]
similarity = (user0_vec @ user1_vec) / (np.linalg.norm(user0_vec) * np.linalg.norm(user1_vec))
print(f"\nUser 0 and User 1 similarity: {similarity:.3f}")
```

## Examples

### Example 1: SVD Fundamentals

```python
A = np.array([[1, 2],
              [3, 4],
              [5, 6]])
U, s, Vt = np.linalg.svd(A)

print("SVD components:")
print(f"U (3x3):\n{np.round(U, 3)}")
print(f"Σ diagonal: {np.round(s, 3)}")
print(f"V^T (2x2):\n{np.round(Vt, 3)}")

# Check orthogonality
print(f"U^T U = I? {np.allclose(U.T @ U, np.eye(3))}")
print(f"V V^T = I? {np.allclose(Vt @ Vt.T, np.eye(2))}")

# Reconstruct
Sigma = np.zeros((3, 2))
Sigma[:2, :2] = np.diag(s)
A_rec = U @ Sigma @ Vt
print(f"Reconstruction error: {np.linalg.norm(A - A_rec):.2e}")
```

### Example 2: Relationship to Eigendecomposition

```python
# For symmetric positive semidefinite matrices, SVD = eigendecomposition
A = np.array([[2, -1, 0],
              [-1, 2, -1],
              [0, -1, 2]])

# SVD
U_svd, s_svd, Vt_svd = np.linalg.svd(A)

# Eigendecomposition (for symmetric, V = eigenvectors)
evals, evecs = np.linalg.eigh(A)

print(f"SVD singular values: {np.round(s_svd, 4)}")
print(f"Eigenvalues (abs):   {np.round(np.abs(evals), 4)}")

# For symmetric PD, U = V (up to sign)
print(f"U == V?\n{np.round(U_svd - evecs, 4)}")

# SVD = spectral decomposition
print(f"A via SVD:\n{np.round(U_svd @ np.diag(s_svd) @ Vt_svd, 2)}")
print(f"A via spectral:\n{np.round(evecs @ np.diag(evals) @ evecs.T, 2)}")
```

### Example 3: Truncated SVD for Dimensionality Reduction

```python
# High-dimensional data with low intrinsic dimension
np.random.seed(42)
n, d, true_dim = 200, 50, 5
X = np.random.randn(n, true_dim) @ np.random.randn(true_dim, d)
X += np.random.randn(n, d) * 0.1

U, s, Vt = np.linalg.svd(X, full_matrices=False)

# Find effective rank from singular value elbow
cumulative = np.cumsum(s) / np.sum(s)
effective_rank = np.searchsorted(cumulative, 0.99) + 1
print(f"Effective rank (99% energy): {effective_rank} (true: {true_dim})")

# Reduce to 2D for visualization
X_2d = X @ Vt[:2].T
print(f"Reduced shape: {X_2d.shape}")

# Explained variance ratio
var_explained = s[:2]**2 / np.sum(s**2)
print(f"Variance explained by 2 components: {var_explained.sum():.3f}")
```

### Example 4: Solving Linear Systems with SVD

```python
# Solve Ax = b using SVD pseudoinverse
A = np.array([[1, 2, 3],
              [4, 5, 6],
              [7, 8, 10]])  # note: third row is not linear combo
b = np.array([6, 15, 25])

# Direct inverse
x_inv = np.linalg.solve(A, b)
print(f"Direct solve: {x_inv}")

# Via SVD
U, s, Vt = np.linalg.svd(A)
x_svd = Vt.T @ np.diag(1/s) @ U.T @ b
print(f"SVD solve: {x_svd}")

# Handle near-singular: truncate small singular values
A_ill = A.copy()
A_ill[2] = A_ill[0] * 2 + A_ill[1] * (-1)  # now singular
b_ill = np.array([6, 15, 3])

U, s, Vt = np.linalg.svd(A_ill)
print(f"\nNear-singular singular values: {np.round(s, 6)}")

# Regularized: ignore small σ
tol = 1e-10
s_inv = np.array([1/σ if σ > tol else 0 for σ in s])
x_reg = Vt.T @ np.diag(s_inv) @ U.T @ b_ill
x_lstsq = np.linalg.lstsq(A_ill, b_ill, rcond=None)[0]
print(f"Regularized SVD: {x_reg}")
print(f"lstsq: {x_lstsq}")
```

### Example 5: SVD for Data Whitening

```python
# Whitening: transform data to have identity covariance
X = np.random.randn(100, 3)
# Create correlation
X[:, 1] = 0.8 * X[:, 0] + 0.2 * X[:, 1]
X[:, 2] = 0.3 * X[:, 0] + 0.5 * X[:, 1] + 0.2 * X[:, 2]

X_centered = X - X.mean(axis=0)

# SVD-based whitening
U, s, Vt = np.linalg.svd(X_centered, full_matrices=False)
X_white = X_centered @ Vt.T @ np.diag(1/s)

print(f"Covariance (should be ~I):\n{np.round(np.cov(X_white.T), 3)}")
print(f"Mean: {np.round(X_white.mean(axis=0), 3)}")

# ZCA whitening (rotates back to original axes)
X_zca = X_white @ Vt
print(f"ZCA covariance:\n{np.round(np.cov(X_zca.T), 3)}")
```

## Glossary

| Term | Definition |
|------|------------|
| Singular Value Decomposition | $A = U \Sigma V^T$, the fundamental matrix factorization |
| Singular values | The diagonal entries $\sigma_i$ of $\Sigma$, non-negative and sorted descending |
| Left singular vectors | Columns of $U$, orthonormal basis for column space |
| Right singular vectors | Rows of $V^T$ (columns of $V$), orthonormal basis for row space |
| Full SVD | $U$ is $m \times m$, $\Sigma$ is $m \times n$, $V$ is $n \times n$ |
| Reduced SVD | Only the $r = \text{rank}(A)$ non-zero components |
| Truncated SVD | Approximation using only the top $k$ singular values |
| Eckart-Young theorem | The optimal rank-$k$ approximation is the truncated SVD |
| Moore-Penrose pseudoinverse | $A^+ = V \Sigma^+ U^T$; generalizes the inverse |
| Low-rank approximation | Approximating a matrix by a lower-rank matrix (via truncated SVD) |
| Principal component analysis | Dimensionality reduction via SVD of centered data |
| Spectral norm | $\|A\|_2 = \sigma_{\max}$, the largest singular value |
| Nuclear norm | $\|A\|_* = \sum \sigma_i$, sum of singular values |
| Condition number | $\kappa(A) = \sigma_{\max} / \sigma_{\min}$ |
| Latent semantic analysis | NLP technique using SVD on term-document matrices |
| Collaborative filtering | Recommendation via matrix factorization (truncated SVD) |
| Data whitening | Transforming data to have identity covariance matrix |
| Randomized SVD | Approximate SVD for large matrices using random sampling |

## Quick References

- [NumPy linalg.svd Documentation](https://numpy.org/doc/stable/reference/generated/numpy.linalg.svd.html) — reference for SVD computation
- [3Blue1Brown — SVD](https://www.youtube.com/watch?v=gXbThCXjZFM) — geometric intuition for the SVD
- [MIT 18.06: SVD (Strang)](https://ocw.mit.edu/courses/mathematics/18-06-linear-algebra-spring-2010/video-lectures/lecture-29-singular-value-decomposition/) — lecture on the SVD

## Next Steps
This concludes the core linear algebra sequence. You are now equipped with the foundational mathematics for:

- [Systems of Linear Equations](systems-of-linear-equations.md) — revisit from SVD perspective
- [Eigenvalues & Eigenvectors](eigenvalues-and-eigenvectors.md) — SVD generalizes eigendecomposition
- **Machine Learning:** Linear regression, PCA, neural networks
