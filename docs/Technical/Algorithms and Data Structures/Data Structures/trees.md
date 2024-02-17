# Trees

Trees are a non-linear data structure that allow us to implement a range of agorithms that are faster than using linear data structures like array-based lists or linked lists. Trees also provide a natural organizationof data, and have become ubiquitous structures in file systems, GUIs, databses etc.

Trees are **hierarchical** representation of a set of **nodes** storing elements such that the nodes have a **parent-child** relationship with the following definitions and properties:

- **root** - top node of a tree
- **parent** - except for the root node, all nodes in a tree have a parent
- **children** - nodes in a tree may have zero or more children
- **depth** $d_p$ - for a position `p` of a node in a tree `T`, the depth of `p` is the number of ancestors of `p`, excluding `p` itself. (but it includes the root node)
  - e.g. if `p` is the root, then its depth is 0, otherwise it is `1 + depth of the parent`
- **height** $h_p$ - defined from the leaf of the tree, defined recursively
  - e.g. if `p` is the leaf of a tree, its height is 0, otherwise it is `1 + max(height of children)`
  - the height of a non-empty tree T is equal to the maximum of the depths of its leaf positions

In a tree, each node of the tree apart from the root node has a unique parent node, and every node with a parent is a child node.

Trees can be empty (i.e. don't even have a root node) which allows for recursive definitions such that:

1. a tree `T` is either empty or consists of a root node `r`
2. root node `r` has a set of set of subtrees whose roots are the children of `r`

## Ordered Trees

A tree is **ordered** if there is a meaningful linear order among the children of each node - i.e. we can identify the children of a node as being the first, second, third and so on. This is usually visualized by arranng siblings left to right, according to their order.

## Tree Abstract Base Class

```python
from abc import ABC, abstractmethod

class Position(ABC):

    def element(self):
        raise NotImplementedError('must be implemented by subclass')

    def __eq__(self, other):
        raise NotImplementedError('must be implemented by subclass')

    def __ne__(self, other):
        return not self == other

class Tree(ABC):

    def root(self):
        raise NotImplementedError('must be implemented by subclass')

    def parent(self, position):
        raise NotImplementedError('must be implemented by subclass')

    def num_children(self, position):
        raise NotImplementedError('must be implemented by subclass')

    def children(self, position):
        raise NotImplementedError('must be implemented by subclass')

    def __len__(self):
        raise NotImplementedError('must be implemented by subclass')

    def is_root(self, position):
        return self.root() == position

    def is_leaf(self, position):
        return self.num_children(position) == 0

    def is_empty(self):
        return len(self) == 0

    def depth(self, position): # O(d_p + 1)
        if self.is_root(position):
            return 0
        else:
            return 1 + self.depth(self.parent(p))

    def _height(self, position): # O(n)
        if self.is_leaf(position):
            return 0
        else:
            return 1 + max(self.height(child) for child in self.children(position))

    def height(self, position=None):
        if not position:
            position = self.root()

        return self._height(position)
```

## Binary Trees

A **binary tree** is an ordered tree with the following properties:

1. Every node has at most two children
2. Each child node is labeled as being either a **left child** or a **right child**
3. A left child precedes a right child in the order of children of the node

The subtree rooted at the left child is called a **left subtree** and rooted at the right a **right subtree**.

A **Full** or **Proper** binary search tree is one where each node has either zero or two children. Therefore internal parent nodes always have exactly two children.

An example is **expression trees** that capture the arithmetic expression e.g. `(((3+1)x3)/((9-5)+2)` as shown below:

```
                (/)
                / \
               /   \
             (x)    (+)
            /  \    /  \
          (+)   3  (-)  2
         /  \      / \
        3    1    9   5
```

### Recursive Definitions of a Binary Search Tree

A binary tree is either empty or consists of:

1. a node `r`, called the root of `T`, that stores an element
2. a binary tree (possibly empty), called the left subtree of `T`
3. a binary tree (possibly empty), called the right subtree of `T`

### Properties

**Level** of a tree is a set of nodes at the same depth.

So in a binary tree, level 0 has at most 1 node. Level 1 has at most 2 nodes. Level 2 has at most 4 nodes etc.

So in general, a level `d` has at most $2^d$ nodes.

Therefore from this we can see that the number of nodes at any given level is exponential. From this we can define the following properties where - $n_E$ is number of external nodes, $n_I$ is the number of internal nodes, $n$ is the numberof nodes, and $h$ is the height of the tree:

1. $h + 1 \leq n \leq 2^{h+1}-1$
2. $1 \leq n_E \leq 2^h$
3. $h \leq n_I \leq 2^h - 1$
4. $log(n+1) - 1 \leq h \leq n - 1$

Also if the tree is proper then:

1. $2h + 1 \leq n \leq w^{h+1} - 1$
2. $h + 1 \leq n_E \leq 2^h$
3. $h \leq n_I \leq 2^h - 1$
4. $log(n+1) - 1 \leq h \leq (n-1)/2$
5. $n_E = n_I + 1$

For reference, $n_E$ = external nodes = nodes with no children. $n_I$ = internal nodes = nodes which are not leaves.

### Implementation of Abstract Class

```python

```

## Binary Search Trees

## Self Balancing Trees

### AVL Trees

### (2,4) Trees

### Red-Black Trees

### Splay Trees


## Minimum Spanning Trees

## Segment Trees

## Fenwick Trees