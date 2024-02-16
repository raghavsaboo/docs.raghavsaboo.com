# Numpy and PyTorch Tensors Guide

Two things we need to be able to do with data:

1. acquire
2. process

Acquiring data also requires us to store it, and the most convenient tool we have at our disposal are `tensors` or `n-dimensional arrays`.

## Initializing

```python
import numpy as np
import torch

# Tensor: multi-dimensional array of numerical values
# k = 1 axes is a vector
np_vector = np.random.rand(12)
torch_vector = torch.rand(12, dtype=torch.float32)
print(np_vector.shape)
print(torch_vector.shape)
# k = 2 axes is a matrix
np_matrix = np.random.rand(12,2)
torch_matrix = torch.rand((12,2), dtype=torch.float32)
print(np_matrix.shape)
print(torch_matrix.shape)
# k > 2 axes is a kth order tensor
np_tensor = np.random.rand(12,2,1)
torch_tensor = torch.rand((12,2,1), dtype=torch.float32)
print(np_tensor.shape)
print(torch_tensor.shape)
```

Other ways to initialize:

```python
torch.ones((2,3,4))
torch.zeros((2,3,4))
torch.randn((2,3,4)) # sample from normal distribution vs. uniform in rand
```

## Reshaping

Change the shape of a tensor by either not changing:

1. the number of elements
2. the values of elements

```python
np_tensor = np_tensor.reshape(6,4)
torch_tensor = torch_tensor.reshape(6,4)
```

Defining all dimensions is uneccessary - we only need to define n - 1 dims, the remaining is inferred.

```python
np_tensor = np_tensor.reshape(6,-1)
torch_tensor = torch_tensor.reshape(6,-1)
```

## Operations

### Elementwise operations

Apply a standard scalar operation to each element of on array, or for two tensor inputs apply elementwise operations on each pair of elements.

Thse include standard arithmetic operations (+, -, \*, / and \*\*).

```python
x = torch.tensor([1.0, 2, 4, 8])
y = torch.tensor([2, 2, 2, 2])
x + y, x - y, x * y, x / y, x ** y
```

#### Hadamard product (Elementwise multiplication of two matrices)

Specifically, elementwise multiplication of two matrices is called their
_Hadamard product_ (math notation $\odot$). Consider matrix
$\mathbf{B} \in \mathbb{R}^{m \times n}$ whose element of row
$i` and column $j$ is $b_{ij}$. The Hadamard product
of matrices $\mathbf{A}$ and
$\mathbf{B}$

$$
   \mathbf{A} \odot \mathbf{B} =
   \begin{bmatrix}
       a_{11}  b_{11} & a_{12}  b_{12} & \dots  & a_{1n}  b_{1n} \\
       a_{21}  b_{21} & a_{22}  b_{22} & \dots  & a_{2n}  b_{2n} \\
       \vdots & \vdots & \ddots & \vdots \\
       a_{m1}  b_{m1} & a_{m2}  b_{m2} & \dots  & a_{mn}  b_{mn}
   \end{bmatrix}.
$$

### Linear Algebra Operations

#### Transpose

```python
A = np.arange(20).reshape(5, 4)
A
A.T
```

As a special type of the square matrix, a _symmetric matrix_
$\mathbf{A}$ is equal to its transpose:
$\mathbf{A} = \mathbf{A}^\top$. Here we define a symmetric matrix
`B`.

```python
B = np.array([[1, 2, 3], [2, 0, 4], [3, 4, 5]])
B
B == B.T
```

#### Vector Dot Products

Given two vectors $\mathbf{x}, \mathbf{y} \in \mathbb{R}^d$, their _dot product_
$\mathbf{x}^\top \mathbf{y}$ (or
$\langle \mathbf{x}, \mathbf{y} \rangle$) is a sum over the
products of the elements at the same position:
$\mathbf{x}^\top \mathbf{y} = \sum_{i=1}^{d} x_i y_i$.

```python
x = torch.arange(4, dtype=torch.float32)
y = torch.ones(4, dtype = torch.float32)
x, y, torch.dot(x, y)
```

Dot products are useful in a wide range of contexts. For example, given
some set of values, denoted by a vector
$\mathbf{x} \in \mathbb{R}^d$ and a set of weights denoted by
$\mathbf{w} \in \mathbb{R}^d$, the weighted sum of the values in
$\mathbf{x}$ according to the weights $\mathbf{w}$ could be
expressed as the dot product $\mathbf{x}^\top \mathbf{w}$. When
the weights are non-negative and sum to one (i.e.,
$\left(\sum_{i=1}^{d} {w_i} = 1\right)$), the dot product
expresses a _weighted average_. After normalizing two vectors to have
the unit length, the dot products express the cosine of the angle
between them. We will formally introduce this notion of _length_ later
in this section.

#### Matrix Multiplications

### Concatentation and Stacking

Provide the list of tensors and axis to concatenate against.

```python
X = torch.arange(12, dtype=torch.float32).reshape((3,4))
Y = torch.tensor([[2.0, 1, 4, 3], [1, 2, 3, 4], [4, 3, 2, 1]])
torch.cat((X, Y), dim=0), torch.cat((X, Y), dim=1)
```

### Summation

Summing all the elements in the tensor yields a tensor with only one element. You can also sum along just a given axis.

```python
X = torch.sum(X, 1) # sum along dim 1
X = torch.sum() # sum all elements
```

#### Non-Reduction Sum

However, sometimes it can be useful to keep the number of axes unchanged when invoking the function for calculating the sum or mean.

```python
sum_A = A.sum(axis=1, keepdims=True)
sum_A
A / sum_A
```

### Cumulative Sum

If we want to calculate the cumulative sum of elements of A along some axis, say axis=0 (row by row), we can call the cumsum function. This function will not reduce the input tensor along any axis.

```python
A.cumsum(axis=0)
```

### Logical Operations

Sometimes, we want to construct a binary tensor via logical statements. Take X == Y as an example. For each position, if X and Y are equal at that position, the corresponding entry in the new tensor takes a value of 1, meaning that the logical statement X == Y is true at that position; otherwise that position takes 0.

```python
X == Y
```

## Broadcasting

Under certain conditions, even when shapes differ, we can still perform elementwise operations by invoking the broadcasting mechanism. This mechanism works in the following way: First, expand one or both arrays by copying elements appropriately so that after this transformation, the two tensors have the same shape. Second, carry out the elementwise operations on the resulting arrays.

```python
a = torch.arange(3).reshape((3, 1))
b = torch.arange(2).reshape((1, 2))
a, b
```

Since `a` and `b` are $3\times1$ and $1\times2$ matrices
respectively, their shapes do not match up if we want to add them. We
_broadcast_ the entries of both matrices into a larger $3\times2$
matrix as follows: for matrix `a` it replicates the columns and for
matrix `b` it replicates the rows before adding up both elementwise.

```python
a + b
```

## Indexing an Slicing

Just as in any other Python array, elements in a tensor can be accessed by index. As in any Python array, the first element has index 0 and ranges are specified to include the first but before the last element. As in standard Python lists, we can access elements according to their relative position to the end of the list by using negative indices.

Thus, [-1] selects the last element and [1:3] selects the second and the third elements as follows:

```python
X[-1], X[1:3]
```

Beyond reading, we can also write elements of a matrix by specifying indices.

```python
X[1, 2] = 9
X

X[0:2, :] = 12
X
```

## Saving Memory

Running operations can cause new memory to be allocated to host results. For example, if we write Y = X + Y, we will dereference the tensor that Y used to point to and instead point Y at the newly allocated memory. In the following example, we demonstrate this with Python’s id() function, which gives us the exact address of the referenced object in memory. After running Y = Y + X, we will find that id(Y) points to a different location. That is because Python first evaluates Y + X, allocating new memory for the result and then makes Y point to this new location in memory.

```python
before = id(Y)
Y = Y + X
id(Y) == before
```

This might be undesirable for two reasons. First, we do not want to run around allocating memory unnecessarily all the time. In machine learning, we might have hundreds of megabytes of parameters and update all of them multiple times per second. Typically, we will want to perform these updates in place. Second, we might point at the same parameters from multiple variables. If we do not update in place, other references will still point to the old memory location, making it possible for parts of our code to inadvertently reference stale parameters.

Fortunately, performing in-place operations is easy. We can assign the result of an operation to a previously allocated array with slice notation, e.g., Y[:] = <expression>. To illustrate this concept, we first create a new matrix Z with the same shape as another Y, using zeros_like to allocate a block of 0 entries.

```python
Z = torch.zeros_like(Y)
print('id(Z):', id(Z))
Z[:] = X + Y
print('id(Z):', id(Z))
```
