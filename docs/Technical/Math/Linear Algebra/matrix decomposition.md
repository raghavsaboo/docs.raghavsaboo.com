# Matrix Decomposition Methods

## Basics

### What are Eigenvalues and Eigenvectors?

For a given square matrix $A$, an eigenvector is a non-zero vector $\mathbf{v}$ that, when multiplied by $A$, yields a scalar multiple of itself. This scalar is known as an eigenvalue. Mathematically, this relationship is defined as:

$$
A\mathbf{v} = \lambda\mathbf{v}
$$

where:
- $A$ is a square matrix,
- $\mathbf{v}$ is an eigenvector of $A$,
- $\lambda$ is the corresponding eigenvalue of $\mathbf{v}$.

### Intuitive Understanding

Intuitively, eigenvectors point in a direction that is unchanged by the application of $A$, while eigenvalues scale the eigenvector in that direction. This concept is crucial in understanding transformations represented by $A$.

### Common Properties

- **Characteristic Equation**: The eigenvalues of $A$ can be found by solving the characteristic equation $\det(A - \lambda I) = 0$, where $I$ is the identity matrix of the same size as $A$.
- **Multiplicity**: An eigenvalue's multiplicity is the number of times it is a root of the characteristic equation. It can have more than one corresponding eigenvector.

## Diagonalization

A matrix $A$ can be diagonalized if there exists an invertible matrix $P$ and a diagonal matrix $D$ such that:

$$
A = PDP^{-1}
$$

Here, $D$ contains the eigenvalues of $A$, and the columns of $P$ are the corresponding eigenvectors. For $A$ to be diagonalizable, it must have $n$ linearly independent eigenvectors.

## Eigenvalues and Eigenvectors of Symmetric Matrices

Symmetric matrices ($A = A^T$) have several special properties:

- All eigenvalues are real.
- Eigenvectors corresponding to different eigenvalues are orthogonal.
- The matrix can be diagonalized using an orthogonal matrix $Q$ (i.e., $A = Q\Lambda Q^T$), where $\Lambda$ is a diagonal matrix of eigenvalues and $Q$ is an orthogonal matrix of eigenvectors.

### Applications

- **Rotation Matrix**: Describes rotation in a space, with eigenvectors indicating invariant directions of rotation.
- **Scaling Matrix**: Diagonal elements (eigenvalues) represent scaling factors along principal axes.
- **Inverse and Easy Inversion**: If $A$ is symmetric and diagonalizable, its inverse (if it exists) is easily computed via its eigen decomposition.

## Checking Positive Definiteness

A symmetric matrix is positive definite if all its eigenvalues are positive. This property is crucial for optimizing quadratic forms and ensuring the existence of unique solutions in various problems.

## Singular Value Decomposition (SVD)

SVD decomposes a matrix $A$ into three matrices:

$$
A = U\Sigma V^T
$$

- $U$ and $V$ are orthogonal matrices containing the left and right singular vectors of $A$.
- $\Sigma$ is a diagonal matrix containing the singular values of $A$.

SVD is applicable to any $m \times n$ matrix and is particularly useful in solving problems that involve non-square matrices.

## Connection Between SVD and EVD

- For square matrices, EVD focuses on decomposing a matrix into its eigenvectors and eigenvalues, while SVD generalizes this concept to any matrix through singular values and vectors.
- If a matrix $A$ is symmetric, its SVD and EVD coincide, with singular values corresponding to the absolute values of the eigenvalues of $A$.

## Other Decomposition Methods

### LU Factorization

LU Factorization decomposes a matrix $A$ into the product of a lower triangular matrix $L$ and an upper triangular matrix $U$:

$$
A = LU
$$

This decomposition is useful for solving linear systems, calculating determinants, and inverting matrices. The process involves Gaussian elimination.

### QR Decomposition

QR Decomposition decomposes a matrix $A$ into the product of an orthogonal matrix $Q$ and an upper triangular matrix $R$:

$$
A = QR
$$

QR Decomposition is widely used in solving linear least squares problems and for eigenvalue computations in the QR algorithm.

### Cholesky Decomposition

For a positive definite symmetric matrix $A$, Cholesky Decomposition is a factorization into the product of a lower triangular matrix $L$ and its transpose:

$$
A = LL^T
$$

This method is more efficient than LU decomposition for solving systems of equations when $A$ is symmetric and positive definite.

#### Application to Multivariate Normal Distribution Sampling

Cholesky Decomposition can be applied to sample from a Multivariate Normal distribution. Given a covariance matrix $\Sigma$, which is symmetric and positive definite, we can decompose $\Sigma$ as $\Sigma = LL^T$ using Cholesky Decomposition.

To sample a vector $\mathbf{x}$ from a Multivariate Normal distribution with mean $\mu$ and covariance $\Sigma$, we can first sample a vector $\mathbf{z}$ from a standard Multivariate Normal distribution (mean $\mathbf{0}$ and covariance $I$), and then transform $\mathbf{z}$ using the Cholesky factor $L$:

$$
\mathbf{x} = \mu + L\mathbf{z}
$$

This method leverages the property that linear transformations of normally distributed variables are also normally distributed, with the transformation defining the new mean and covariance.
