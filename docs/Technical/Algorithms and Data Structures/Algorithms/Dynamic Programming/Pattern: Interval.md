# Interval Dynamic Programming

The Interval dynamic programming pattern is used to solve problems where the DP subproblem is solved on every single interval (subarray) of the array. This pattern is particularly useful for problems involving contiguous subsequences or subarrays, such as finding the Longest Palindromic Subsequence.

## Approach Explanation:

In this pattern, we define a 2D array `dp[i][j]` to store the solution to the subproblem solved on the interval `[i, j]` of the array. We then iteratively fill in this array to compute the final answer.

## Top-Down Approach:

```python
def dp_top_down(i, j):
    if i > j:
        return 0  # Base case: Empty interval has zero length
    if i == j:
        return 1  # Base case: Single element interval has length 1
    if dp[i][j] is already computed:
        return dp[i][j]
    if s[i] == s[j]:
        dp[i][j] = 2 + dp_top_down(i+1, j-1)
    else:
        dp[i][j] = max(dp_top_down(i+1, j), dp_top_down(i, j-1))
    return dp[i][j]
```

## Bottom-Up Approach

```python
def dp_bottom_up():
    # Initialize base cases
    for i in range(n):
        dp[i][i] = 1  # Base case: Single element interval has length 1
    # Fill DP array
    for length in range(2, n+1):
        for i in range(n-length+1):
            j = i + length - 1
            if s[i] == s[j]:
                dp[i][j] = 2 + dp[i+1][j-1]
            else:
                dp[i][j] = max(dp[i+1][j], dp[i][j-1])
    return dp[0][n-1]  # Final answer
```

## Complexity:

- **Time Complexity**: The time complexity of the DP solution depends on the number of intervals considered and the complexity of comparing elements or computing the function `dp_top_down` or `dp_bottom_up`. Typically, it is $O(n^2)$, where $n$ is the length of the array.

- **Space Complexity**: The space complexity is determined by the size of the DP array, which is typically $O(n^2)$.
