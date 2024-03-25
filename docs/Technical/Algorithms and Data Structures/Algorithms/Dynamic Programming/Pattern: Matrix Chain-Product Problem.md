# Matrix Chain-Product Problem: Overview

## Problem Statement:
Given a chain of $n$ matrices $A_1, A_2, \ldots, A_n$, where the dimensions of matrix $A_i$ are $p_{i-1} \times p_i$ for $i = 1, 2, \ldots, n$, determine the optimal order of multiplication that minimizes the total number of scalar multiplications needed to compute the product.

## Example:
Consider a chain of matrices with dimensions $p = [10, 20, 30, 40]$. The dimensions of the matrices are as follows:
- Matrix $A_1$ has dimensions $10 \times 20$.
- Matrix $A_2$ has dimensions $20 \times 30$.
- Matrix $A_3$ has dimensions $30 \times 40$.

The goal is to determine the optimal order of multiplication that minimizes the total number of scalar multiplications needed to compute $A_1 \times A_2 \times A_3$.

## Dynamic Programming Approach

### Approach:
The dynamic programming approach is used to solve the Matrix Chain-Product Problem efficiently. The key idea is to compute the minimum number of scalar multiplications needed to compute the product of matrices of different lengths, starting from smaller subchains and gradually building up to the entire chain.

### Steps:
1. **Subproblem Definition**: Define subproblems by considering all possible ways to split the chain of matrices into two subchains.
2. **Recurrence Relation**: Define a recurrence relation to compute the minimum number of scalar multiplications needed to compute the product of matrices in each subchain.
3. **Fill the Table**: Use dynamic programming to fill a table with the results of the subproblems, starting from smaller subchains and gradually building up to the entire chain.
4. **Backtracking**: Once the table is filled, backtrack to reconstruct the optimal order of multiplication.

### Implementation
```python
def matrix_chain_order(p):
    n = len(p) - 1  # Number of matrices in the chain
    m = [[0] * n for _ in range(n)]  # Table to store minimum scalar multiplications
    s = [[0] * n for _ in range(n)]  # Table to store optimal split points
    
    # Fill the table using bottom-up dynamic programming
    for l in range(2, n + 1):  # l is the chain length
        for i in range(n - l + 1):
            j = i + l - 1  # End index of current chain
            m[i][j] = float('inf')  # Initialize minimum scalar multiplications to infinity
            for k in range(i, j):
                # Compute the number of scalar multiplications for each split point
                cost = m[i][k] + m[k + 1][j] + p[i] * p[k + 1] * p[j + 1]
                if cost < m[i][j]:
                    m[i][j] = cost
                    s[i][j] = k
    
    # Backtrack to reconstruct the optimal order of multiplication
    def print_optimal_order(i, j):
        if i == j:
            return "A" + str(i + 1)
        else:
            return "(" + print_optimal_order(i, s[i][j]) + " × " + print_optimal_order(s[i][j] + 1, j) + ")"
    
    optimal_order = print_optimal_order(0, n - 1)
    return m[0][n - 1], optimal_order

# Example usage
dimensions = [10, 20, 30, 40]
min_scalar_multiplications, optimal_order = matrix_chain_order(dimensions)
print("Minimum scalar multiplications:", min_scalar_multiplications)
print("Optimal order of multiplication:", optimal_order)
```

### Time Complexity
The time complexity of the dynamic programming solution for the Matrix Chain-Product Problem is $O(n^3)$, where $n$ is the number of matrices in the chain. This is because we fill an $n \times n$ table, and each cell requires constant time to compute based on the values of its neighbors.