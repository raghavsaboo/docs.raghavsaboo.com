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

## Linked Structure for General Trees

A natural way to represent a tree is through a **linked structure**, with nodes that maintain references to the elment stores at a position `p` and to the nodes associated with the children and parent of `p`.

For a general tree, there is no limit on the number of children that a node may have. A natural way to realize a general tree `T` as a linked structure is to have each node store a single **container** of references to its children.

The tree itself also maintainsan instance variable storing a reference to the root node, and a variable called `size` that represents the overall number of nodes of `T`.

```python
class TreeNode:
    __slots__ = '_element', '_parent', '_left', '_right'

    def __init__(self, element, parent=None, children=[]):
        self._element = element
        self._parent = parent
        self._children = children

class TreePosition(Position):

    def __init__(self, container, node):
        self._container = container
        self._node = node

    def element(self):
        return self._node._element

    def __eq__(self, other):
        return type(other) is type(self) and other._node is self._node

class GeneralTree(Tree):

    def __init__(self):
        self._root = None
        self._size = 0

    def _make_position(self, node):
        return self.Position(self, node) ifnode is not None else None

    def _validate(self, p):
        if not isinstance(p, self.Position):
            raise TypeError('p must be proper Position type')

        if p._container is not self:
            raise ValueError('p does not belong to this container')

        if p._node._parent is p._node:
            raise ValueError('p is no longer valid')

        return p._node

    def __len__(self):
        return self._size

    def root(self):
        return self._make_postion(self._root)

    def parent(self, p):
        node = self._validate(p)
        return self._make_position(node._parent)

    def children(self, p):
        node = self._validate(p)
        return self._make_position(node._children)

    def num_children(self, p):
        node = self._validate(p)
        return len(node._children)

    def __iter__(self):
        for p in self.positions():
            yield p.element()

    def positions(self):
        return self.preorder()

    def preorder(self):
        if not self.is_empty():
            for p in self._subtree_preorder(self.root()):
                yield p

    def _subtree_preorder(self, p):
        yield p
        for c in self.children(p):
            for other in self._subtree_preorder(c):
                yield other

    def postorder(self):
        if not self.is_empty():
            for p in self._subtree_postorder(self.root()):
                yield p

    def _subtree_postorder(self, p):
        for c in self.children(p):
            for other in self._subtree_postorder(c):
                yield other
        yield p

    def breadthfirst(self):
        if not self.is_empty():
            fringe = LinkedQueue()
            fringe.enqueue(self.root())
            while not fringe.is_empty():
                p = fringe.dequeue()
                yield p
                for c in self.children(p):
                    fringe.enqueue(c)
```

## Binary Trees

A **binary tree** is an ordered tree with the following properties:

1. Every node has at most two children
2. Each child node is labeled as being either a **left child** or a **right child**
3. A left child precedes a right child in the order of children of the node

The subtree rooted at the left child is called a **left subtree** and rooted at the right a **right subtree**.

### Recursive Definitions of a Binary Search Tree
A binary tree is either empty or consists of:

1. a node `r`, called the root of `T`, that stores an element
2. a binary tree (possibly empty), called the left subtree of `T`
3. a binary tree (possibly empty), called the right subtree of `T`

### Types

#### Proper/Full

A **Full** or **Proper** binary search tree is one where each node has either zero or two children. Therefore internal parent nodes always have exactly two children.
![wiki source: https://en.wikipedia.org/wiki/Binary_tree#/media/File:Full_binary.svg](./images/full%20binary%20tree.png)

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

#### Perfect
All interior nodes have two children and all leaves have the same depth or same level. A perfect binary tree is also a full binary tree.

#### Complete

All levels of the binary tree (0, 1, 2, ..., h -1) have maximum number of nodes possible (i.e. $2^i$ nodes for $i \leq i \leq h-1$) and the remaining nodes at level `h` reside in the **leftmost** possible positions at that level.
![wiki source: https://en.wikipedia.org/wiki/Binary_tree#/media/File:Complete_binary2.svg](./images/complete%20binary%20tree.png)

#### Balanced
Where left and rigt subtrees of every node differ in height by at most 1. i.e. where no leaf is much farther away from the root than any other leaf.

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

### Array Based Binary Tree

```
                (/)
                / \
               /   \
             (x)    (+)
            /  \    /  \
          (+)   3  (-)  2
         /  \      / \
        3    1    9   5

bfs: [/, x, +, +, 3, -, 2, 3, 1, 9, 5, Null, Null]
pre-order: [/, x, +, 3, Null, Null, 1, Null, Null, 3, Null, Null, +, -, 9, Null, Null, 5, Null, Null, 2, Null, Null]
```

BFS or level numbering of positions to build an array is fairly common. Here a position can be represented by a single integer - the main advantage of this data structure. But operations like deleting and promoting children takes `O(n)` time.

### Utilizing the General Tree Linked Structure

For a binary tree the children are defined as `left` and `right` child.

```python
class BinaryTreeNode:
    __slots__ = '_element', '_parent', '_left', '_right'

    def __init__(self, element, parent=None, left=None, right=None):
        self._element = element
        self._parent = parent
        self._left = left
        self._right = right

class BinaryTreePosition(Position):

    def __init__(self, container, node):
        self._container = container
        self._node = node

    def element(self):
        return self._node._element

    def __eq__(self, other):
        return type(other) is type(self) and other._node is self._node

class BinaryTree(Tree):

    def __init__(self):
        self._root = None
        self._size = 0

    def _make_position(self, node):
        return self.Position(self, node) ifnode is not None else None

    def _validate(self, p):
        if not isinstance(p, self.Position):
            raise TypeError('p must be proper Position type')

        if p._container is not self:
            raise ValueError('p does not belong to this container')

        if p._node._parent is p._node:
            raise ValueError('p is no longer valid')

        return p._node

    def __len__(self):
        return self._size

    def root(self):
        return self._make_postion(self._root)

    def parent(self, p):
        node = self._validate(p)
        return self._make_position(node._parent)

    def left(self, p):
        node = self._validate(p)
        return self._make_position(node._left)

    def right(self, p):
        node = self._validate(p)
        return self._make_position(node._right)

    def num_children(self, p):
        node = self._validate(p)
        count = 0:
        if node._left is not None:
            count += 1
        if node._right is not None:
            count += 1
        return len(count)

    def _add_root(self, e):
        if self._root is not None: raise ValueError('root exists')
        self._size = 1
        self._root = Node(e)
        return self._make_position(self._root)

    def _add_left(self, p, e):
        node = self._validate(p)
        if node._left is not None:
            raise ValueError('Left child exists')
        self._size += 1
        node._left = self._Node(e, node)
        return self._make_position(node._left)

    def _add_right(self, p, e):
        node = self._validate(p)
        if node._right is not None:
            raise ValueError('Right child exists')
        self._size += 1
        node._right = self._Node(e, node)
        return self._make_position(node._right)

    def _replace(self, p, e):
        node = self._validate(p)
        old = node._element
        node._element = e
        return old

    def _delete(self, p):
        node = self._validate(p)
        if self.num_children(p) == 2:
            raise ValueError('Position has two children')
        child = node._left if node._left else node._right
        if child is not None:
            child._parent = node._parent
        if node is self._root:
            self._root = child
        else:
            parent = node._parent
            if node is parent._left:
                parent._left = child
            else:
                parent._right = child
        self._size -= 1
        node._parent = node
        return node._element

    def _attach(self, p, t1, t2):
        node = self._validate(p)
        if not self.is_leaf(p):
            raise ValueError('position must be leaf')
        if not type(self) is type(t1) is type(t2):
            raise TypeError('Tree types must match')
        self._size += len(t1) + len(t2)
        if not t1.is_empty():
            t1._root._parent = node
            node._left = t1._root
            t1._root = None
            t1._size = 0
        if not t2.is_empty():
            t2._root._parent = node
            node._right = t2._root
            t2._root = None
            t2._size = 0
```

## Tree Traversal Algorithms

Go to [Algorithms >> Tree Traversals](../Algorithms/tree%20traversals.md)

We can also modify our base `Tree` class to implement the traversal methods as well, with the pre-order traversal being the default one.

Refer to [Tree Abstract Base Class](#tree-abstract-base-class) for the other parts of the base class.

Some key observations:

1. we implement the traversals as generator functions where we yield each position to the caller and let the caller decide what action to perform at that position
2. the `positions` method returns the iteration as an object - again acting as a generator
3. the `breadth first search` or `bfs` traversal is iterative - not recursive
4. the `inorder` traversal is only implemented for `binary trees` - it does not generalize for generic trees

```python
class Tree(ABC):

    ...
    def positions(self):
        self.preorder()

    def preorder(self):
        if not self.is_empty():
            for p in self._subtree_preorder(self.root()):
                yield p

    def _subtree_preorder(self, p):
        yield p
        for c in self.children(p):
            for other in self._subtree_preorder(c):
                yield other

    def postorder(self):
        if not self.is_empty():
            for p in self._subtree_postorder(self.root()):
                yield p

    def _subtree_postorder(self, p):
        for c in self.children(p):
            for other in self._subtree_postorder(c):
                yield other
        yield p 
        
    def bfs(self):
        if not self.is_empty():
            fringe = LinkedQueue()
            fringe.enqueue(self.root())
            while not fringe.is_empty():
                p = fringe.dequeue()
                yield p
                for c in self.children(p):
                    fringe.enqueue(c)

    def inorder(self):
        if not self.is_empty():
            for p in self._subtree_inorder(self.root()):
                yield p

    def _subtree_inorder(self, p):
        if self.left(p) is not None:
            for other in self._subtree_inorder(self.left(p)):
                yield other
        yield p
        if self.right(p) is not None:
            for other in self._subtree_inorder(self.right(p)):
                yield other
    ...
```

As can be seen, while we can implement specific traversals for the `Tree` class, or the `inorder` method for binary trees, the code is not general enough to capture the range of computations possible. These were all custom implementations with some repeating algorithmic patterns (e.g. blending of work performed before or after recursion of subtrees). 

A more general framework for implemting tree traversals is through [Euler tour traversals](../Algorithms/tree%20traversals.md##Euler%20Tours) which also uses the [Template method](../../Object%20Oriented%20Programming/template%20method%20pattern.md).

## Binary Search Trees

If we have a set of unique elements that have an order relation  (e.g. a set of integers), we can construct a **binary search tree** such that for each position `p` of the tree `T`:

1. Position `p` stores an element of the set, denoted as `e(p)`
2. Elements stored in the left subtree of `p` (if any) are less than `e(p)`
3. Elements stored in the right subtree of `p` (if any) are greater than `e(p)`

![wiki source: https://en.wikipedia.org/wiki/Binary_search_tree#/media/File:Binary_search_tree.svg](./images/binary%20search%20tree.png)

## Self Balancing Trees

### AVL Trees

### (2,4) Trees

### Red-Black Trees

### Splay Trees

## Minimum Spanning Trees

## Segment Trees

## Fenwick Trees

**References**
[^1]: Skiena, Steven S.. (2008). The Algorithm Design Manual, 2nd ed. (2). : Springer Publishing Company.
[^2]: Goodrich, M. T., Tamassia, R., & Goldwasser, M. H. (2013). Data Structures and Algorithms in Python (1st ed.). Wiley Publishing.
[^3]: Cormen, T. H., Leiserson, C. E., & Rivest, R. L. (1990). Introduction to algorithms. Cambridge, Mass. : New York, MIT Press.