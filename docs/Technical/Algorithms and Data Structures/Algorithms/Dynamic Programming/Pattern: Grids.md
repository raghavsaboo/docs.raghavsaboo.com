# Pattern: Grids
This dynamic programming pattern is used to solve problems where you have a grid structure, and you need to find an optimal solution by considering subgrids within the original grid.

## Memoization (Top-Down) Approach
Define the state:

- **dp[i][j]:** [Define the meaning of the state dp[i][j].]

Initialize the memoization array:

- Initialize a memoization array `memo` with dimensions equal to the grid.

Memoization function:

- Define a function to recursively compute the state `dp[i][j]` while memoizing the results.

Pseudocode:

```python
def dp(i, j):
    if (i, j) has already been computed:
        return memo[i][j]
    if base case:
        return base case value
    dp[i][j] = [Define transition based on previous states, recursively calling dp function]
    memo[i][j] = dp[i][j]
    return dp[i][j]

def solve_grid_problem(grid):
    initialize memoization array memo
    return dp(n, m)  # n and m are the dimensions of the grid
```

## Bottom-up Approach
Define the state:

- **dp[i][j]:** [Define the meaning of the state dp[i][j].]

Initialize the base case:

- Initialize dp[0][0], dp[0][1], ..., dp[n][m] based on the problem.

Transition:

- **dp[i][j] = [Define the transition based on the problem statement and previous states].**

Return:

- Return the desired result, e.g., dp[n][m].

Pseudocode:

```python
def solve_grid_problem(grid):
    n, m = dimensions of the grid
    # Initialize dp array
    dp = [[0] * m for _ in range(n)]
    
    # Base cases
    dp[0][0] = [Define base case]
    
    # Populate dp array bottom-up
    for i from 0 to n-1:
        for j from 0 to m-1:
            if (i, j) is not in the boundary:
                dp[i][j] = [Define transition based on dp[i-1][j], dp[i][j-1], and/or dp[i-1][j-1]]
    
    # Return the desired result
    return dp[n-1][m-1]
```