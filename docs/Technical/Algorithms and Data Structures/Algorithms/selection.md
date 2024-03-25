# Selection
Here we are interested in identifying a single element in terms of its rank relative to the sorted order of the entire set. Examples are: minimum, and maximum elements, median element etc.

In general, queries that ask for an element with a given rank are called **order statistics**.

## General Selection Problem
The general order-statistc problem of selecting the kth smallest element from an unsorted collection of `n` comparable elements is the **selection** problem. This can be solved by sorting the collection and then indexing into the sorted sequence at `k-1` but this would take `O(nlogn)`.

However `O(n)` running time can be achieved (worst case `k=n`) including the interesting case of finding the median where `k=floor(n/2)`. 

## Prune and Search

To get `O(n)` running time for any value fo `k` we use a design pattern known as **prune and search** where on a collection of `n` objects we prune away a fraction of the `n` objects and recursively solve the smaller problem. When we have finally reduced the probnlem to one defined on a constant-sized collection of objercts, we then solve the problem using some brute-force method. 

## Randomized Quick Select

Runs in `O(n)` **expected time** taken over all possible random choices made by the algorithm. Worst case is `O(n^2)`. 

Suppose we are given an **unordered** sequence S of `n` comparable elements together with an integer `k` we find the `kth` smallest element by:

- picking a pivot element from S at random
- subdividing S into three subsequences L, E, and G, storing the elements of S less than, equal to, and great than the pivot respectively
- determine hich of these subsets contain the desired element based on the value of `k` and sizes of the subsets
- recure on the appropriate subset, noting that the desired elements rank in the subset may differ from its rank in the full set.

```python
def quick_select(S, k):
    """Return the kth smallest element of list S< for k from 1 to len(S)"""

    if len(S) == 1:
        return S[0]

    pivot = random.choice(S)
    L = [x for x in S if x < pivot]
    E = [x for x in S if x == pivot]
    G = [x for x in S if x > pivot]

    if k <= len(L):
        return quick_select(L, k) # kth smallest lies in L
    elif k <= len(L) + len(E):
        return pivot # kth smallest is equal to pivot
    else:
        j = k - len(L) - len(E) # new selection parameter
        return quick_select(G, j) # kth smallest is jth in G
```