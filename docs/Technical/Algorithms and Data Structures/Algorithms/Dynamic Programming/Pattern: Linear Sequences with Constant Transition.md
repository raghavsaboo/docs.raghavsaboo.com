# Pattern: Linear Sequences with Constant Transition

This dynamic programming pattern deals with problems where you have to compute a value for each index in a sequence based on the values of previous indices, and the transition from one index to the next is constant. In other words, the transition from `dp[i]` to `dp[i+1]` depends only on a fixed number of previous states and not on the current index i.

## Memoization (Top-Down) Approach

Define the state:

- **dp[i]:** [Define the meaning of the state dp[i].]

Initialize the memoization array:

- Initialize a memoization array `memo` with the size of the problem.

Memoization function:

- Define a function to recursively compute the state `dp[i]` while memoizing the results.

Pseudocode:

```python
def dp(i):
    if i has already been computed:
        return dp[i]
    if base case:
        return base case value
    dp[i] = [Define transition based on previous states, recursively calling dp function]
    return dp[i]

def solve_linear_sequences_with_constant_transition(nums):
    initialize memoization array memo
    return dp(n)
```

## Bottom-up Approach

Define the state:

- **dp[i]:** [Define the meaning of the state dp[i].]

Initialize the base case:

- Initialize dp[0], dp[1], ... [Depends on the problem.]

Transition:

- **dp[i] = [Define the transition based on the problem statement and previous states].**

Return:

- Return the desired result, e.g., dp[n].

Pseudocode:

```python
def solve_linear_sequences_with_constant_transition(nums):
    n = len(nums)
    # Initialize dp array
    dp = [0] * (n + 1)
    
    # Base cases
    dp[0] = [Define base case]
    dp[1] = [Define base case]
    
    # Populate dp array bottom-up
    for i in range(2, n + 1):
        dp[i] = [Define transition based on dp[i-1] and/or dp[i-2]]
    
    # Return the desired result
    return dp[n]
```