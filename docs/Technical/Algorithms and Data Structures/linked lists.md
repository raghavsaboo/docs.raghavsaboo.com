# Linked Lists

There are some notable disadvantages of dynamic arrays like `list` in Python

1. the length of the dynamic array might be longer than the actual number of elements it stores
2. amortized bounds for operations may be unacceptable in real-time systems
3. insertions and deletions at interior positions of an array are expensive

Linked lists provide an alternative to array-based ordered sequences. 

While an array provides more centralized representation with one large chunk of memory capable of accommodating references to many elements, a linked list relies on a distributed representation using a lightweight object called a **node** for each element. 

Each node maintains a reference to its element and one or more references to neighboring nodes in order to collectively represent the linear order of the sequence.

## Trade-offs between Linked Lists and Arrays

### Advantages of Array-Based Sequences

1. *Arrays provide `O(1)` time access to an element based on an integer index.* Linked lists requires `O(k)` time to traverse the list from the beginning or possibly end of a doubly linked list.
2. *Operations with equivalent asymptotic bounds typically run a constant factor more efficiently with an array-based structure versus a linked structure*. For example appending a new element to a linked list requires instantiation of a node, appropriate linking of nodes, and an increment of an integer. While this operation completes in `O(1)` time the actual number of CPU operations will be more in the linked version.
3. *Array based representations typically use proportionally less memory than linked structures*. A dynamic array may be, in the worst case, allocated memory for 2n object references. With linked lists however the memory must be devoted not only to store a reference to each contained object, but also explicit references that link the nodes. So a singly linked list of length n already requires 2n references, and a doubly linked list 3n references.

### Advantages of Link-based sequences

1. *Link based structures provide worst-case time bounds for their operations*. This is in contrast to amortized bounds associated with the expansion and contraction of a dynamic array. In real-time systems designed for more immediate responses, a long delay caused a single (amortized) operation may have an adverse effect. Worst-case bound gives a guarantee on the sum of the time spent on the individual operations.
2. *Link based structures support `O(1)` time insertions and deletions at arbitrary positions*. This is perhaps the biggest advantage of linked lists.

## Singly Linked List
A collection of nodes that collectively form a linear sequence. Each node stores a reference to an object that is an element of the sequence, as well as a reference to the next node of the list. 

Each node is represented as a unique object, with that instance storing a reference to its element and a reference to the next node (or None). The linked list must keep a reference to the head of the list - without an explicit reference to the head, there would be no way to locate that node. 

### Efficiencies
Space Complexity: `O(n)`

Time Complexities:

![Singly Linked List](./drawio_diagrams/singly%20linked%20list.png)

### Stack using a Singly Linked List

Using a singly linked list, we implement a stack. Since for a linked list we can insert and delete elements in constant time only at the head, we will use the head as the `top` of the stack.

When implementing the node we also intentionally define `__slots__` to streamline the memory usage as there may potentially be many node instances in a single list. 

```python
class _Node:
    __slots__ = '_element', '_next'

    def __init__(self, element, next):
        self._element = element
        self._next = next


class LinkedStack:

    def __init__(self):
        self._head = None
        self._size = 0

    def __len__(self):
        return self._size

    def is_empty(self):
        return self._size == 0

    def push(self, e):
        self._head = self._Node(e, self._head)
        self._size += 1

    def top(self):
        if self.is_empty():
            raise Empty('Stack is Empty')
        return self._head._element

    def pop(self):
        if self.is_empty():
            raise Empty('Stack is Empty')
        
        answer = self._head._element

        self._head = self._head._next
        self._size -= 1
        return answer
```

### Queue using a Singly Linked List

```python
class LinkedQueue:

    def __init__(self):
        
```