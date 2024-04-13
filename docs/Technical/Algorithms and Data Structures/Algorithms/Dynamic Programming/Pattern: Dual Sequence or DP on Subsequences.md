# Dual Sequence or DP on Subsequences

The "Dual Sequence" dynamic programming pattern is used to solve problems that involve calculating some value related to two sequences. This pattern is particularly useful when the solution depends on both sequences, and we need to consider all possible subsequences of both sequences.

In this pattern, we define a 2D array `cache[i][j]` to store the answer to the subproblem solved on a prefix of sequence 1 with length `i`, and a prefix of sequence 2 with length `j`. We then iteratively fill in this array to compute the final answer.

## Top-Down Approach:

```python
def dp_top_down(i, j):
    if i == 0 or j == 0:
        return base_case_value
    if cache[i][j] is already computed:
        return cache[i][j]
    # Compute the value based on previous results and current elements
    cache[i][j] = some_function(dp_top_down(i-1, j), dp_top_down(i, j-1), ...)
    return cache[i][j]
```

## Bottom-Up Approach

```python
def dp_bottom_up():
    # Initialize base cases
    for i in range(m+1):
        cache[i][0] = base_case_value
    for j in range(n+1):
        cache[0][j] = base_case_value
    # Fill DP array
    for i in range(1, m+1):
        for j in range(1, n+1):
            cache[i][j] = some_function(cache[i-1][j], cache[i][j-1], ...)
    return cache[m][n]  # Final answer
```

## Complexity:

- **Time Complexity**: The time complexity of the DP solution depends on the size of the input sequences and the number of subproblems computed. Typically, it is $O(mn)$, where $m$ and $n$ are the lengths of the input sequences.

- **Space Complexity**: The space complexity also depends on the size of the DP array, which is typically $O(mn)$.

This dynamic programming pattern is versatile and can be applied to various problems that involve two sequences, such as finding the longest common subsequence, edit distance, or maximum sum of increasing subsequences.
