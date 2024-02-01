# Searching

## Binary Search

A classic recursive algorithm, **binary search**, that is used to locate a target value within a **sorted sequence** of n elements.

Time complexity: `O(log n)`

When the sequence is **unsorted** then we can use **sequential search** algorithms that run in O(n) time (i.e. linear time).


```python
def binary_search(data, target, left, right):
    """
    Return True if target is found in indicated portion of a Python list.
    
    The search only considers the portion from data[left] to data[right] inclusive.
    """

    if left > right:
        return False, None # interval is empty, no match
    else:
        mid = (left + right) // 2
        if target == data[mid]: # found a match
            return True, mid
        elif target < data[mid]:
            # recur on the portion left of the middle
            return binary_search(data, target, left, mid - 1)
        else:
            # recur on the portion right of the middle
            return binary_search(data, target, mid + 1, right)
```

[^1]: Data Structures and Algorithms in Python by M. Goodrich, R. Tamassia, M. Goldwasser
