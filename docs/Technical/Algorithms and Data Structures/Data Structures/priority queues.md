# Priority Queues and Heaps
A collection of prioritized elements that allow arbitrary element insertion, and allows the removal of the elmenet that has first priority. Priority is provided by an associated **key** - and the element with the *minimum* key will be the next to be removed from the queue. The key must define a natural order i.e. `a < b` for any instance `a` and `b`. 

## Priority Queue ADT

```python
from abc import ABC, abstractmethod

class PriorityQueueBase:
    class _Item:
        __slots__ = '_key', '_value'

        def __init__(self, k, v):
            self._key = k
            self._value = v

        def __lt__(self, other):
            return self._key < other._key

        def __repr__(self):
            return '({0},{1})'.format(self._key, self._value)

    def is_empty(self):
        return len(self) == 0

    def __len__(self):
        raise NotImplementedError('must be implemented by subclass')

    @abstractmethod
    def add(self, key, value):
        raise NotImplementedError('must be implemented by subclass')

    @abstractmethod
    def min(self):
        raise NotImplementedError('must be implemented by subclass')

    @abstractmethod
    def remove_min(self):
        raise NotImplementedError('must be implemented by subclass')
```

### Implementation with Unsorted List

In this we store entries within an unsorted list using a doubly-linked list (`PositionalList`) defined from [](../Data%20Structures/positional%20list.md##position%20abstraction)

The doubly linked list ensures all of the operations execute in `O(1)` time except for `min` and `_find_min` which would take `O(n)`

```python
class UnsortedPriorityQueue(PriorityQueueBase):
    def _find_min(self):
        if self.is_empty():
            raise Empty('Priority queue is empty')
        small = self._data.first()
        walk = self._data.after(small)
        while walk is not None:
            if walk.element() < small.element():
                small = walk
            walk = self._data.after(walk)
        return small

    def __init__(self):
        self._data = PositionalList()

    def __len__(self):
        return len(self._data)

    def add(self, key, value):
        self._data.add_last(self._Item(key, value))

    def min(self):
        p = self._find_min()
        item = p.element()
        return (item._key, item._value)

    def remove_min(self):
        p = self._find_min()
        item = self._data.delete(p)
        return (item._key, item._value)
```

### Implementation with a Sorted List
A alternative is to use a positional list but maintaining the entries sorted by non-decreasing keys. But in this case the insertion of a new entry has a cost of `O(n)` using insertion sort.

But `min` and `remove_min` take oly `O(1)` time now.

```python
class SortedPriorityQueue(PriorityQueueBase):
    def __init__(self):
        self._data = PositionalList()

    def __len__(self):
        return len(self._data)

    def add(self, key, value):
        newest = self._Item(key, value)
        walk = self._data.last()
        while walk is not None and newest < walk.element():
            walk = self._data.before(walk)
        if walk is None:
            self._data.add_first(newest)
        else:
            self._data.add_after(walk, newest)

    def min(self):
        if self.is_empty():
            raise Empty('Priority queue is empty.')
        p = self._data.first()
        item = p.element()
        return (item._key, item._value)

    def remove_min(self):
        if self.is_empty():
            raise Empty('Priority queue is empty.')
        item = self._data.delete(self._data.first())
        return (item._key, item._value)
```

## Heaps

A data structure that provides an efficient realization of a priority queue - both insertions and removals are in logarithmic time - which is a significant improvement over the list based implementations.

A **binary heap** uses a **binary tree** which helps find a compromise between elements being entirely unsorted and perfectly sorted.

In python this is implemented as a `heapq`.

### Properties

1. Stores a collection of items at its positions
2. **Heap Order Property** - for every position `p` other than the root, the key stored at `p` is greater than or equal to the key stored at `p's` parent
3. **Complete Binary Tree Property** - a heap with height `h` is [**complete**](../Data%20Structures/trees.md#complete)

Point 2. means that the minimum key is always stored at the root of the heap. Keys encountered on a path from the root to the leaf of a heap are in nondecreasing order.

4. **Height of a Heap** - given that it is complete, `height h = floor(log n)`

### Implementation


### Priority Queue as a Heap

### Array based representation

## Applications

### Sorting
- Selction Sort
- Insertion Sort
- Heap Sort

## Adaptable Priority Queues