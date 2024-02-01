# Recursion

Recursion is another way to achieve repetition apart from loops. In recursion a function makes one or more calls to itself during execution, or by which a data structure relies on smaller instances of the same type of structure in its representation.

A recursive function has some basic properties:

1. it contains one or more *base cases* which are defined non-recursively in terms of fixed quantities
2. it contains one or more *recursive cases* which are defined by appealing to the definition of the function being defined


```python
def factorial(n):

    # base case
    if n == 0:
        return 1
    else:
        return n * factorial(n-1)
```

In Python, each time a function (recursive or otherwise) is called, a structure known as an **activation record** or **frame** is created to store information about the progress of that invocation of the function. This activation record includes a namespace for storing the function call's parameters and local variables, and information about which command in the body of the function is currently executing.

When the execution of a function leads to a nested function call, the execution of the former call is suspended and its activation record stores the place in the source code at which the flow of control should continue upon return of the nested call. This process is used both in the standard case of one function calling a different function, or in the recursive case in which a function invokes itself. The key point is that there is a different activation record for each active cell.




```python
import os

def disk_usage(path):
    """
    Return the number of bytes used by a file/folder and any descendents.
    """
    total = os.path.getsize(path)

    if os.path.isdir(path):
        for filename in os.listdir(path):
            childpath = os.path.join(path, filename)
            total += disk.usage(childpath)

        print(f'{total}', path)
        return total
```

## Maximum Recursive Depth in Python
If each recursive call makes another recursive call, without ever reaching a base case, then we have an infinite series of such calls. 

This is called **infinite  recursion** and it is a fatal error as it can quickly swamp computing resources, not only due to rapid use of the CPU, but because each successive call creates an activation record requiring additional memory.

To avoid this, we should always ensure that each recursive call is in some way progressing toward a base case. Also in Python there is an intentional design decision to limit the overall number of function activations that can be simultaneously active at ~ 1,000. 

In Python this can be dynamically reconfigured:


```python
import sys

old = sys.getrecursionlimit()
sys.setrecursionlimit(100000)
```

## Other Examples of Recursion

- If a recursive call starts at most one other, we call this a **linear recursion**
- If a recursive call may start two others, we all this a **binary recursion**
- If a recursive call may start three or more ohers, this is **multiple recursion**

Note that the linear recursion terminology here reflects the structure of the recursion trace, and not the time complexity.

### Linear Recursion Examples

#### Linear Sum
Calculating the sum of a sequence of numbers by adding the last number to the sum of the first `n-1`.


```python
def linear_sum(S, n):
    """Return the sum of the first n numbers of sequence S.
    """
    if n == 0:
        return 0
    else:
        return linear_sum(S, n-1) + S[n-1]
```

#### Reversing a Sequence
Reversal of a sequence by swapping the first and last elements, and then recursively reversing the remaining elements. 


```python
def reverse(S, start, stop):
    """Reverse elements in implicit slice S[start:stop]
    """
    if start < stop - 1:
        S[start], S[stop-1] = S[stop-1], S[start]
        reverse(S, start+1, stop-1)
```

#### Computing Powers
Raising a number $x$ to an arbitrary non-negaive integer, $n$ - i.e. compute the **power function**, defined as $\text{power}(x,n)=x^n$.

A trivial definition would follow from the fact that $x^n=x \cdot x^{n-1}$

$$
\text{power}(x,n)= 
    \begin{cases}
    1 & \text{if n=0} \\
    x \cdot \text{power}(x, n-1) & \text{otherwise}
    \end{cases}
$$

This is `O(n)`

A much faster way to compute the power function is the squaring technique. Let $k=\lfloor \frac{n}{2} \rfloor$ denote the floor of the division. When $n$ is even, $k=\lfloor \frac{n}{2} \rfloor=\frac{n}{2}$ and therefore $(x^k)^2=(x^{\frac{n}{2}})^2=x^n$. When $n$ is odd $k=\lfloor \frac{n}{2} \rfloor=\frac{n-1}{2}$ and therefore $(x^k)^2=(x^{\frac{n-1}{2}})^2=x^{n-1}$.

$$
\text{power}(x,n)= 
    \begin{cases}
    1 & \text{if n=0} \\
    x \cdot (\text{power}(x, \lfloor \frac{n}{2} \rfloor))^2 & \text{if n>0 is odd} \\
    \text{power}(x, \lfloor \frac{n}{2} \rfloor)^2 & \text{if n>0 is even} \\
    \end{cases}
$$

This is `O(log n)`


```python
def power(x, n):
    """Compute the value x**n for integer n."""
    if n == 0:
        return 1
    else:
        return x*power(x, n-1)
    
def faster_power(x, n):
    """Compute the value x**n for integer n."""
    if n == 0:
        return 1
    else:
        partial = power(x, n//2)
        result = partial * partial
        if n % 2 == 1:
            result *= x
        return result
```

### Binary Recursion

#### Binary Sum
Summing the $n$ elements of a sequence, $S$, of numbers by computing the sum of the first half and the sum of the second half, and then adding them together. 


```python
def binary_sum(S, start, stop):
    """Return the sum of the numbers in implicit slic S[start:stop]"""
    if start >= stop:
        return 0
    elif start == stop - 1:
        return S[start]
    else: 
        mid = (start + stop) // 2
        return binary_sum(S, start, mid) + binary_sum(S, mid, stop)
```

### Multiple Recursion

#### Summation Puzzle
A common example is when we want to enumerate various configurations in order to solve a combinatorial puzzle e.g. a **summation puzzle**.

$$
\begin{split}
pot + pan = bib \\
dog + cat = pig \\
boy + girl = baby
\end{split}
$$

Here we need to assign a unique digit (0...9) to each letter in the equation in order to make the equation true. So we can use the computer to enumerate all possibilities and test each one. 

The general algorithm for building sequences is:

1. Recursively generate the sequences of `k - 1` elements
2. Append to each such sequence an element not already contained in it
3. Use a set `U` to keep track of the elements not contained in the current sequence, so that na element `e` is considered to not have been used yet if and only if `e` is in `U`

```python
puzzle_solve(k, S, U):
    Input: An integer k, sequence S, and set U
    Output: enumeration of all k-length extensions to S using elements in U without repetitions

    for each e in U do
        Add e to the end of S
        Remove e from U
        if k==1 then
            Test whether S is a configuration that solves the puzzle
            if S solves the puzzle then
                return "Solution found: " S
        else
            puzzle_solve(k-1, S, U)
        Remove r from the end of S
        Add e back to U
```

## Designing Recursive Algorithms
Think of the different ways to define subproblems that have the same general structure as the original problem. 

Add necessary parameterization to the function (e.g. passing beginning and end of sub-array).

**Test for base cases** - there should at least be one, and defined so that every possible chain of recursive calls eventually reaches a base case.

**Recur** - for non-base cases, perform one or more recursive calls.

### Eliminating Tail Recursion
The usefulness of recursion comes at a modest cost - in particular the Python interpreter must maintain activation records that keep track of the state of each nested call. 

Some forms of recursion can be eliminated without the use of axillary memory (like stacks) - and a notable form is **tail recursion**.

A recursion is a tail recursion if any recursive call is made from one context is the very last operation in that context, with the return value of the recursive call immediately returned by the enclosing recursion. In this can by necessity, a tail recursion must be a linear recursion.

Examples are binary search and the sequence reversing algorithm.

However the factorial function is *not* a tail recursion as it involves `return n * factorial(n-1)` which means an additional multiplication is performed after the recursive call.

Tail recursions can be reimplemented non-recursively by enclosing the body in a loop for repetition, and replacing a recursive call with new parameters by reassignment of the existing parameters to those values.


```python
def binary_search_iterative(data, target):
    low = 0
    high = len(data) - 1

    while low <= high:
        mid = (low + high) // 2
        if target == data[mid]:
            return True
        
        elif target < data[mid]:
            high = mid - 1
        
        else:
            low = mid + 1

    return False

def reverse_iterative(S):

    start, stop = 0, len(S)

    while start < stop - 1:
        S[start], S[stop-1] = S[stop-1], S[start]
        start, stop = start + 1, stop - 1

```

[^1]: Data Structures and Algorithms in Python by M. Goodrich, R. Tamassia, M. Goldwasser
