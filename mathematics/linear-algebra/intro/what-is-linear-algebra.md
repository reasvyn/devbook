# What Is Linear Algebra

## Description

Linear algebra is the study of vectors, vector spaces, and linear transformations. It is the mathematical foundation of computer graphics, machine learning, quantum computing, and virtually every field that processes data at scale. For developers, it is the single most practical branch of mathematics.

## Prerequisites

- [Why Math Matters](../../intro/why-math.md)

## Table of Contents

- [The Core Objects](#the-core-objects)
- [Why Linear](#why-linear)
- [Where It Shows Up in Code](#where-it-shows-up-in-code)
- [The Mindset Shift](#the-mindset-shift)

## Content / Material

### The Core Objects

Linear algebra revolves around a few simple objects:

- **Scalar** — a single number (5, -2.3, π).
- **Vector** — an ordered list of numbers, often representing a point in space.
- **Matrix** — a rectangular grid of numbers, representing a transformation or a dataset.
- **Tensor** — a multidimensional generalization (matrix is a 2D tensor).

### Why Linear

A transformation is *linear* if it satisfies two properties:

1. **Additivity**: $T(u + v) = T(u) + T(v)$
2. **Homogeneity**: $T(c \cdot u) = c \cdot T(u)$

Linear transformations are predictable, composable, and invertible — properties that make them the ideal building block for computation.

### Where It Shows Up in Code

- **Graphics**: every 3D transformation (rotate, scale, translate) is a matrix multiplication.
- **Machine learning**: neural network layers are matrix multiplications with activation functions.
- **Data**: a spreadsheet or DataFrame is a matrix. PCA, SVD, and factorization are linear algebra.
- **Search**: PageRank uses eigenvectors. Recommendation systems use matrix factorization.
- **Physics engines**: forces, velocities, and collisions are all vector operations.

### The Mindset Shift

Linear algebra teaches you to think in terms of *spaces* and *transformations* rather than individual numbers. A dataset is not a collection of rows — it's a point cloud in high-dimensional space. A function is not a sequence of steps — it's a transformation of that space.

## Glossary

| Term | Definition |
|------|------------|
| Vector | An ordered list of numbers, often representing magnitude and direction |
| Matrix | A 2D grid of numbers representing a linear transformation or data |
| Linear transformation | A function preserving vector addition and scalar multiplication |
| Eigenvector | A vector that, when transformed, only changes by a scalar factor |

## Next Steps

- [Vectors & Matrices](../beginner/vectors-and-matrices.md) — operations, dot product, cross product
