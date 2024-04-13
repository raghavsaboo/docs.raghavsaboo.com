# Linear Sequences with Non-Constant Transition Dynamic Programming

The "Linear Sequences with Non-Constant Transition" dynamic programming pattern is used to solve problems where the DP problem is solved on every prefix of the array, similar to the "Group 2" pattern. However, the transitions in this pattern may not be simple and may require considering a linear amount of options from indices $j < i$.

In this pattern, we still consider solving the DP problem on every prefix of the array. However, the transition from one state to another may not be constant and may depend on a linear number of options from previous indices.

## Top-Down Approach:

```python
def dp_top_down(i):
    if i == 0:
        return base_case_value
    if dp[i] is already computed:
        return dp[i]
    # Initialize with a value that works for the base case
    dp[i] = initial_value
    # Iterate over possible transitions from indices j < i
    for j in range(i-1, -1, -1):
        # Update dp[i] based on transitions from previous indices
        dp[i] = max(dp[i], some_function(dp[j], ...))
    return dp[i]
```

## Bottom-Up Approach:

```python
def dp_bottom_up():
    # Initialize base cases
    dp[0] = base_case_value
    # Fill DP array
    for i in range(1, n):
        # Initialize with a value that works for the base case
        dp[i] = initial_value
        # Iterate over possible transitions from indices j < i
        for j in range(i-1, -1, -1):
            # Update dp[i] based on transitions from previous indices
            dp[i] = max(dp[i], some_function(dp[j], ...))
    return max(dp)  # Final answer
```