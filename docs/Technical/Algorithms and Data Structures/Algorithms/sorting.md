# Sorting Algorithms


## Insertion Sort
```pseudocode
Algorithm InserstionSort(A):
    Input: an array A of n comparable elements
    Output: the array A with elements rearranged in non-decreasing order

    for k from 1 to n - 1 do:
       Insert A[k] at its proper locatio within A[0], A[1], ..., A[k] 
```

```python
def insertion_sort(A):
    """
    Sort list of comparable elements into non-decreasing order.
    """

    for k in range(1, len(A)):
        cur = A[k]
        j = k
        while j > 0 and A[j -1] < cur:
            A[j] = A[j-1]
            j -= 1
        A[j] = cur
```

**Time Complexity:** $\mathcal{O}(n^2)$

**Space Complexity:** $\mathcal{O}(n)$

