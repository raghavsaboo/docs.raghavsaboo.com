# Positional ADT

Stacks, queues, and double ended queues only allow update operations that occur at one end of a sequence or the other. But these are too limiting - we want to be able to refer to elements anywhere in a sequence, and to perform arbitrary insertions and deletions.

Using indices is not preffered as:
1. linked lists don't allow access through indexing without traversing the list incrementally from the begening or end
2. it is not convenient to describe the location of an object purely by reference to the beginning or end of the data structure

## Position Abstraction

Two ADTS:
1. `Postion` - A marker or token within the broader positional list. A position `p` is unaffected by changes elsewhere in a list; the only way in which a position becomes invalid is if an explicit command is issued to delete it.
2. `PositionalList` - serves as a container of sequential elements that allow positional access.

In the implementation below refer to the Doubly Linked List class defined in [Linked Lists: Doubly Linked List](./linked%20lists.md#doubly-linked-list)

```python
class Position

    def __init__(self, container, node):
        self._container = container
        self._node = node

    def element(self):
        return self._node._element

    def __eq__(self, other):
        return type(other) is type(self) and other._node is self._node

    def __ne__(self, other):
        return not (self == other)

class PositionalList(DoublyLinkedBase):

    def _validate(self, position):
        if not isinstance(position, self.Position):
            raise TypeError('p must be proper Position type')
        if p._container is not self:
            raise ValueError('p does not belong to this container')
        if p._node._next is None:
            raise ValueError('p is no longer valid')
        return p._node

    def _make_position(self, node):
        if node is self._header or node is self._trailer:
            return None
        else:
            return self.Position(self, node)

    def first(self):
        return self._make_position(self._header._next)

    def last(self):
        return self._make_position(self._trailer._prev)

    def before(self, position):
        node = self._validate(position)
        return self._make_position(node._prev)

    def after(self, position):
        node = self._validate(position)
        return self._make_position(node._next)

    def __iter__(self):
        cursor = self.first()
        while cursor is not None:
            yield cursor.element()
            cursor = self.after(cursor)

    def _insert_between(self, element, predecessor, successor):
        node = super()._insert_between(element, predecessor, successor)
        return self._make_position(node)

    def add_first(self, element):
        return self._insert_between(element, self._header, self._header._next)

    def add_last(self, element):
        return self._insert_between(element, self._trailer._prev, self._trailer)

    def add_before(self, position, element):
        original = self._validate(position)
        return self._insert_between(element, original._prev, original)

    def add_after(self, position, element):
        original = self._validate(position)
        return self._insert_between(element, original, original._next)

    def delete(self, postion):
        original = self._validate(position)
        return self._delete_node(original)

    def replace(self, position, element):
        original = self._validate(position)
        old_value = original._element
        original._element = element
        return old_value
```

# Sorting a Positional List
We can implement [Insertion Sort](../Algorithms/sorting.md) for a `PositionalList`.

```python
def insertion_sort(pos_list):
    # no need to sort an list of 1
    if len(pos_list) > 1:

        marker = pos_list.first()

        while marker != pos_list.last():
            # get the next item to compare against
            pivot = pos_list.after(marker)
            value = pivot.element()
            # pivot already sorted
            if value > marker.element():
                # pivot becomes the leftmost new marker
                marker = pivot
            # need to sort pivot 
            else:
                # find leftmost item greater than value
                walk = marker
                while walk != pos_list.first() and pos_list.before(walk).element() > value:
                    walk = pos_list.before(walk)

                pos_list.delete(pivot)
                # reinsert value before walk
                pos_list.add_before(walk, value)

```