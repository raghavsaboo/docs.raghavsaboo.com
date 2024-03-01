# Tree Traversals

Systematic way of accessing all of the positions of a tree. An example of implementing these in an abstree `Tree` datastructure is presented in [Trees](../Data%20Structures/trees.md##Tree%20Traversal%20Algorithms)

## Pre-Order, Post-Order, and In-Order

### Pre-Order

in **pre-order traversal** the root of the tree is visited first, and then the subtrees rooted at its children are traversed recursively. If the tree is ordered, then the subtrees are traversed according to the order of the children.

```python
pre_ordered_list = []
def pre_order(tree, position, pre_ordered_list):
    pre_ordered_list.append(position)
    for child in tree.children(position):
        pre_order(tree, child, visited)
```

### Post-Order

Opposite of pre-order traversal, because it recursively traverses the subtrees rooted at the children of the root first, and then visits the root.

```python
post_ordered_list = []
def post_order(tree, position, post_ordered_list):
    for child in tree.children(position):
        post_order(tree, child, post_ordered_list)
    post_ordered_list.add(position)
```
### In-Order
This traversal is specific to **binary trees**. We visit a position between the recursive traversal of the left and right subtrees of a binary tree. i.e. visiting nodes from `left to right`. For every position `p`, inorder traversal visits `p` after all of the positions in the left subtree of `p` and before all the positions in the right subtree of `p`.

```python
in_order_list = []
def inorder(tree, position, in_order_list):
    if tree.left(position):
        inorder(tree, tree.left(position), in_order_list)
    in_order_list.append(position)
    if tree.right(position):
        inorder(tree, tree.right(position), in_order_list)
```

## Breadth First Search

Traverse a tree so that we visit all of the positions at a given depth before we visit the positions at the next depth/level.

We use a queue to produce a FIFO sematic for the order in which we visit nodes without making it recursive.

```python
def breadth_first_search(tree):
    queue = deque([tree.root])
    bfs_order = []
    while queue:
        p = queue.popleft()
        bfs_order.append(p)
        for child in tree.children(p):
            queue.enqueue(child)
```

## Depth First Search

## Prim-Jarnik

## Kruskal's

## Euler Tours
The Euler tour traversal of a general tree `T` is defined as a "walk" around `T`, where we start by going from the root towards its leftmost child, viewing the edges of `T` as being "walls" that we always keep to the left.

The complexity of the walk is `O(n)` - it progresses exactly to times along each of the `n-1` edges of the tree - once going downward along the edge, and later going upward along the edge. 

![Wikipedia source: https://en.wikipedia.org/wiki/Euler_tour_technique#/media/File:Stirling_permutation_Euler_tour.svg](./images/euler%20tour.png)

To unify this with `preorder` and `postorder` traversals, we can think of there being two notable "visits" to each position `p`:

- a `pre visit` occurs when first reaching the position, that is, whe the walk passes immediately left of the node in our visualization
- a `post visit` occurs when the walk later proceeds upward from that position, that is, when the walk pass to the right of the node in our visualization

Euler tour can be done recursively such that in between the pre-visit and post-visit of a position will be a recursve tour of each of the subtrees. 

```python
Algorithm eulertour(T, p):
     perform the "previsit" action for position p
     for each child c in T.children(p):
        eulertou(T, c)

    perform the "postvisit" action for position p
```

A more complete implementation is below, where we use the [Template Method](../../Object%20Oriented%20Programming/template%20method%20pattern.md) to define the generic traversal algorithm, and use two **hooks** (auxiliary functions) - one for the previsit before the subtrees are traversed, and another for the postvisit after the completion of subtree traversals.

The hooks can be overridden to provide specialized behavior.

```python
class EulerTour:

    def __init__(self, tree):
        self._tree = tree

    def tree(self):
        return self._tree

    def execute(self):
        if len(self._tree) > 0:
            return self._tour(self._tree.root(), 0, [])

    def _tour(self, p, d, path):
        """
        Perform the tour of a subtree rooted at Position p.

        p = position of current node being visited
        d = depth of p in the tree
        path = list of indices of children on path from root to p
        """

        # previsit p
        self._hook_previsit(p, d, path)

        results = []

        # add new index to the end of path before recursion
        path.append(0)

        for c in self._tree.children(p):
            # recur on child's subtree
            results.append(self._tour(c, d+1, path))
            # increment index
            path[-1] += 1
        # remove extraneous index from the end of path
        path.pop()
        # post visit p
        answer = self._hook_postvisit(p, d, path, results)

        return answer

    def _hook_previsit(self, p, d, path):
        pass

    def _hook_postvisit(self, p, d, path):
        pass 
```

## Applications

!!! note ""
    TODO: Add notes here - leaving blank for now.

