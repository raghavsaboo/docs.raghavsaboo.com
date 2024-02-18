# Tree Traversals

Systematic way of accessing all of the positions of a tree.

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

## Euler Tour and Template Pattern
![Wikipedia source: https://en.wikipedia.org/wiki/Euler_tour_technique#/media/File:Stirling_permutation_Euler_tour.svg](./images/euler%20tour.png)\

## Applications
