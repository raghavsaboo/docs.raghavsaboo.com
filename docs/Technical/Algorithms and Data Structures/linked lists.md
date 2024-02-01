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

1. *Arrays provide `O(1)` time access to an element based on an integer index. Linked lists requires `O(k)` time to traverse the list from the beginning or possibly end of a doubly linked list.
2. *Operations with equivalent asymptotic bounds typically run a constant factor more efficiently with an array-based structure versus a linked structure*. For example appending a new element to a linked list requires instantiation of a node, appropriate linking of nodes, and an increment of an integer. While this operation completes in `O(1)` time the actual number of CPU operations will be more in the linked version.
3. *Array based representations typically use proportionally less memory than linked structures*. Even though a dynamic array may, in the worst case, be 