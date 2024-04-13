# Knapsack Dynamic Programming

The Knapsack dynamic programming pattern is used to solve problems where items have certain values and weights, and we aim to maximize the value of items included in a knapsack (or a backpack) with a given capacity.

In this pattern, we define a 2D array `dp[i][w]` to store the maximum value that can be achieved using the first `i` items and a knapsack capacity of `w`. We then iteratively fill in this array to compute the final answer.

## Top-Down Approach:

```python
def knapsack_top_down(i, w):
    if i == 0 or w == 0:
        return 0  # Base case: No items or knapsack capacity is 0
    if dp[i][w] is already computed:
        return dp[i][w]
    if weights[i] > w:
        dp[i][w] = knapsack_top_down(i-1, w)
    else:
        dp[i][w] = max(values[i] + knapsack_top_down(i-1, w - weights[i]), knapsack_top_down(i-1, w))
    return dp[i][w]
```

## Bottom-Up Approach:
```python
def knapsack_bottom_up():
    # Initialize base cases
    for i in range(n+1):
        dp[i][0] = 0
    for w in range(W+1):
        dp[0][w] = 0
    # Fill DP array
    for i in range(1, n+1):
        for w in range(1, W+1):
            if weights[i] > w:
                dp[i][w] = dp[i-1][w]
            else:
                dp[i][w] = max(values[i] + dp[i-1][w - weights[i]], dp[i-1][w])
    return dp[n][W]  # Final answer

```

## Complexity:

- **Time Complexity**: The time complexity of both top-down and bottom-up approaches is $O(nW)$, where $n$ is the number of items and $W$ is the capacity of the knapsack.

- **Space Complexity**: The space complexity is determined by the size of the DP array, which is typically $O(nW)$.
