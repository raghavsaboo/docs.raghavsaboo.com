# Maps, Hash Tables, Skip Lists and Sets

## Maps and Dictionaries

Where unique **keys** are mapped to associated **vaLues**, and the relationship they express are commonly known as **associative arrays** or **maps**. Maps or dictionaries are implemented so that a search for a key, and its associated value, can be performed very efficiently.

In Python the `dict` class provides this functionality.

However Python also provides two abstract base class in the `collections` module that are relevant: `Mapping` and `MutableMapping`. `Mapping` includes all nonmutating methods supported by Python's `dict` class, and `MutableMapping` class extends that to include the mutating methods.

```python
from collections import MutableMapping

class MapBase(MutableMapping):
  """Our own abstract base class that includes a nonpublic _Item class."""

  #------------------------------- nested _Item class -------------------------------
  class _Item:
    """Lightweight composite to store key-value pairs as map items."""
    __slots__ = '_key', '_value'

    def __init__(self, k, v):
      self._key = k
      self._value = v

    def __eq__(self, other):
      return self._key == other._key   # compare items based on their keys

    def __ne__(self, other):
      return not (self == other)       # opposite of __eq__

    def __lt__(self, other):
      return self._key < other._key    # compare items based on their keys
```

## Hash Tables

Python's implementation of the `dict` class is a **hash table** - it uses a **hash function** to map general keys to corresponding indices in a table. Ideally the keys will be well distributed in the range from 0 to N-1 by a hash function, but in practice there may be two or more distinct keys that get mapped to the same index.

Thus we will conceptualize the **hash table** as a **bucket array** in which each bucket may manage a collection of items that are sent to a specific index by the hash function.

```python
class HashMapBase(MapBase):
  """Abstract base class for map using hash-table with MAD compression.

  Keys must be hashable and non-None.
  """

  def __init__(self, cap=11, p=109345121):
    """Create an empty hash-table map.

    cap     initial table size (default 11)
    p       positive prime used for MAD (default 109345121)
    """
    self._table = cap * [ None ]
    self._n = 0                                   # number of entries in the map
    self._prime = p                               # prime for MAD compression
    self._scale = 1 + randrange(p-1)              # scale from 1 to p-1 for MAD
    self._shift = randrange(p)                    # shift from 0 to p-1 for MAD

  def _hash_function(self, k):
    return (hash(k)*self._scale + self._shift) % self._prime % len(self._table)

  def __len__(self):
    return self._n

  def __getitem__(self, k):
    j = self._hash_function(k)
    return self._bucket_getitem(j, k)             # may raise KeyError

  def __setitem__(self, k, v):
    j = self._hash_function(k)
    self._bucket_setitem(j, k, v)                 # subroutine maintains self._n
    if self._n > len(self._table) // 2:           # keep load factor <= 0.5
      self._resize(2 * len(self._table) - 1)      # number 2^x - 1 is often prime

  def __delitem__(self, k):
    j = self._hash_function(k)
    self._bucket_delitem(j, k)                    # may raise KeyError
    self._n -= 1

  def _resize(self, c):
    """Resize bucket array to capacity c and rehash all items."""
    old = list(self.items())       # use iteration to record existing items
    self._table = c * [None]       # then reset table to desired capacity
    self._n = 0                    # n recomputed during subsequent adds
    for (k,v) in old:
      self[k] = v                  # reinsert old key-value pair
```

### Hash Functions

A **hash function** maps a key `k` to an integer in the range `[0,N-1]`, where `N` is the capacity of the bucket array for a hash table. Such a hash function is used to index into a bucket array.

If there are two or more keys with the same hash value, then two different items will be mapped to the same bucket in A. This is a **collision** - and there are strategies to deal with them. But it is best to avoid or minimize them.

The hash function also needs to be fast and easy to compute.

The hash function is also thought of being split into two components: 1) **hash code** that maps a key `k` to an integer and 2) **compression function** tat maps the hash code to an integer within a range of indices.

This separation allows hash coding to happen independently of the specific hash table size - so the hash table may be dynamically resized easily.

#### Hash Codes

The resulting integers may even be negative.

`Polynomial Hash Code`
: Useful for string typed keys.
A string is a sequence of characters encoded using the ASCII integer code.
Example: "H e l l o" is `72 101 108 108 111`.

    So given a string $s=x_{k-1}x_{k-2}...x_{0}$, the hash code is $h(s)=x_{k-1}a^{k-1}+x_{k-1}a^{k-2}+...+x_0a^{0}$.

    The choice of the value of `a` is important for the spread of the hash value. Emperically "good" values are `33, 37, 39, 41`.

`Cyclic Shift Hash Code`
: Same as the polynomial hash coed but using the `shift operation` to perform multiplication.

In Python the built-in function for hashing is `hash(x)` that returns an integer value that serves as the hash code for object `x`. However only **immutable** data types are deemed hashable in Python. This is to ensure that a particular object's hash code remains constant during the object's lifespan.

#### Compression Functions

A good compression function minimizes the number of collisions for a given set of distinct hash codes.

`Division Method`
: Maps an integer `i` to `i mod N`

    Here `N` is the size of the bucket array - a fixed positive integer.

    If `N` is a prime number then this compression function helps "spread out" the distribution of hashed values. Actually if `N` is not prime, then there is greater risk that patterns in the distribution of hash codes will be repeated in the distribution of hash values - causing collisions.

`The MAD Method`
: Helps eliminate repeated patterns in a set of integer keys - **Multiply-Add-and-Divide** - where an integer `i` is mapped to:
`[(ai+b) mod p] mod N`

    Where `N` is the size of the bucket array, `p` i the prime number larger than `N`, and `a` and `b` are integers chosen at random from interval `[0, p-1]` with `a>0`.

### Collision Handling

#### Separate Chaining

Have each bucket `A[j]` store its own secondary container, holding items `k,v` such that `h(k)=j`.

The secondary container can be a small map instance implemented using a list. Worst case - operations on an individual bucket take time proportional to the size of the bucket.

Assuming we use a good hash function to index the `n` items of our map in a bucket array of capacity `N`, the expected size of a bucket i `n/N`. Therefore the core map operations run in `O[n/N]` time - the ration `lambda = n/N` is called the **load factor**. This usually below 1 - and so the core operations on the has table run in `O(1)` expected time.

```python
class ChainHashMap(HashMapBase):
  """Hash map implemented with separate chaining for collision resolution."""

  def _bucket_getitem(self, j, k):
    bucket = self._table[j]
    if bucket is None:
      raise KeyError('Key Error: ' + repr(k))        # no match found
    return bucket[k]                                 # may raise KeyError

  def _bucket_setitem(self, j, k, v):
    if self._table[j] is None:
      self._table[j] = UnsortedTableMap()     # bucket is new to the table
    oldsize = len(self._table[j])
    self._table[j][k] = v
    if len(self._table[j]) > oldsize:         # key was new to the table
      self._n += 1                            # increase overall map size

  def _bucket_delitem(self, j, k):
    bucket = self._table[j]
    if bucket is None:
      raise KeyError('Key Error: ' + repr(k))        # no match found
    del bucket[k]                                    # may raise KeyError

  def __iter__(self):
    for bucket in self._table:
      if bucket is not None:                         # a nonempty slot
        for key in bucket:
          yield key
```

#### Linear Probing
!!! note ""
    TODO: Add notes here - leaving blank for now.

```python
class ProbeHashMap(HashMapBase):
  """Hash map implemented with linear probing for collision resolution."""
  _AVAIL = object()       # sentinal marks locations of previous deletions

  def _is_available(self, j):
    """Return True if index j is available in table."""
    return self._table[j] is None or self._table[j] is ProbeHashMap._AVAIL

  def _find_slot(self, j, k):
    """Search for key k in bucket at index j.

    Return (success, index) tuple, described as follows:
    If match was found, success is True and index denotes its location.
    If no match found, success is False and index denotes first available slot.
    """
    firstAvail = None
    while True:                               
      if self._is_available(j):
        if firstAvail is None:
          firstAvail = j                      # mark this as first avail
        if self._table[j] is None:
          return (False, firstAvail)          # search has failed
      elif k == self._table[j]._key:
        return (True, j)                      # found a match
      j = (j + 1) % len(self._table)          # keep looking (cyclically)

  def _bucket_getitem(self, j, k):
    found, s = self._find_slot(j, k)
    if not found:
      raise KeyError('Key Error: ' + repr(k))        # no match found
    return self._table[s]._value

  def _bucket_setitem(self, j, k, v):
    found, s = self._find_slot(j, k)
    if not found:
      self._table[s] = self._Item(k,v)               # insert new item
      self._n += 1                                   # size has increased
    else:
      self._table[s]._value = v                      # overwrite existing

  def _bucket_delitem(self, j, k):
    found, s = self._find_slot(j, k)
    if not found:
      raise KeyError('Key Error: ' + repr(k))        # no match found
    self._table[s] = ProbeHashMap._AVAIL             # mark as vacated

  def __iter__(self):
    for j in range(len(self._table)):                # scan entire table
      if not self._is_available(j):
        yield self._table[j]._key
```

## Sorted Maps

A map does not provide any way to get a list of all keys ordered by some comparison - in fact the hash-based implementation relies on the intentionally scattered keys to make them more uniformly dstirbuted in a hash table.

Data structures that implement a `sorted map` are also called `Sorted Search Tables`. 

!!! note ""
    TODO: Add notes here - leaving blank for now.

### Applications of Sorted Maps
!!! note ""
    TODO: Add notes here - leaving blank for now.

## Skip Lists
!!! note ""
    TODO: Add notes here - leaving blank for now.

## Sets, Multisets, and Multimaps

### Sets
Simply a map in which the keys do not have associated values. In python base classes `MutableSet` is available from the `collections` module.

### Multisets
The same element may occur several times in a multiset. One way to implement this is to have a count of the number of occurrences of an element within a multiset. 

In Python the `Counter` class in `collections` offers the capability. 

### Multimaps
Similar to a traditional map, in that it associates values with keys, however in a multimap the same key can be mapped to multiple values. 

Python does not have a native implementation of this.

```python
class MultiMap:
  """
  A multimap class built upon use of an underlying map for storage.

  This uses dict for default storage.

  Subclasses can override class variable _MapType to change the default.
  That catalog class must have a default constructor that produces an empty map.
  As an example, one might define the following subclass to use a SortedTableMap
 
  class SortedTableMultimap(MultiMap):
    _MapType = SortedTableMap
  """
  _MapType = dict                 # Map type; can be redefined by subclass

  def __init__(self):
    """Create a new empty multimap instance."""
    self._map = self._MapType()          # create map instance for storage
    self._n = 0

  def __len__(self):
    """Return number of (k,v) pairs in multimap."""
    return self._n

  def __iter__(self):
    """Iterate through all (k,v) pairs in multimap."""
    for k,secondary in self._map.items():
      for v in secondary:
        yield (k,v)
    
  def add(self, k, v):
    """Add pair (k,v) to multimap."""
    container = self._map.setdefault(k, [])     # create empty list, if needed
    container.append(v)
    self._n += 1

  def pop(self, k):
    """Remove and return arbitrary (k,v) pair with key k (or raise KeyError)."""
    secondary = self._map[k]                    # may raise KeyError
    v = secondary.pop()
    if len(secondary) == 0:
      del self._map[k]                          # no pairs left
    self._n -= 1
    return (k, v)

  def find(self, k):
    """Return arbitrary (k,v) pair with given key (or raise KeyError)."""
    secondary = self._map[k]                    # may raise KeyError
    return (k, secondary[0])
    
  def find_all(self, k):
    """Generate iteration of all (k,v) pairs with given key."""
    secondary = self._map.get(k)
```

**References**
[^1]: Skiena, Steven S.. (2008). The Algorithm Design Manual, 2nd ed. (2). : Springer Publishing Company.
[^2]: Goodrich, M. T., Tamassia, R., & Goldwasser, M. H. (2013). Data Structures and Algorithms in Python (1st ed.). Wiley Publishing.
[^3]: Cormen, T. H., Leiserson, C. E., & Rivest, R. L. (1990). Introduction to algorithms. Cambridge, Mass. : New York, MIT Press.
