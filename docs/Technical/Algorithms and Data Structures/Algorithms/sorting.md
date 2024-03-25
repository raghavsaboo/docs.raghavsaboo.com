# Sorting Algorithms
Sorting algorithms can be stable or unstable. A stable sort algorithm preserves the relative order of records with equal keys in the sorted output as they appear in the unsorted output.

On the other hand unstable sorting algorithms do not guarantee that any two or more objects with the same keys will appear in the same order before an after sorting.

| Sorting Algorithm | Best Time Complexity | Average Time Complexity | Worst Time Complexity | Space Complexity | Stable Sort |
|-------------------|----------------------|-------------------------|-----------------------|------------------|-------------|
| Bubble Sort       | O(n)                 | O(n^2)                  | O(n^2)                | O(1)             | Yes         |
| Selection Sort    | O(n^2)               | O(n^2)                  | O(n^2)                | O(1)             | No          |
| Insertion Sort    | O(n)                 | O(n^2)                  | O(n^2)                | O(1)             | Yes         |
| Merge Sort        | O(n log n)           | O(n log n)              | O(n log n)            | O(n)             | Yes         |
| Quick Sort        | O(n log n)           | O(n log n)              | O(n^2)                | O(log n)         | No          |
| Bucket Sort       | O(n+k)               | O(n+k)                  | O(n^2)                | O(n)             | Yes         |
| Radix Sort        | O(nk)                | O(nk)                   | O(nk)                 | O(n+k)           | Yes         |
| Counting Sort     | O(n+k)               | O(n+k)                  | O(n+k)                | O(k)             | Yes         |
| Heap Sort         | O(n log n)           | O(n log n)              | O(n log n)            | O(1)             | No          |
| Cycle Sort        | O(n^2)               | O(n^2)                  | O(n^2)                | O(1)             | No          |

- `n` is the number of elements being sorted.
- `k` is the range of the input (for non-comparison based sorts).
- Space complexity considers auxiliary space.

## Bubble Sort
Bubble Sort repeatedly compares and swaps adjacent elements if they are in the wrong order.
- **Complexity**: Best: O(n), Average and Worst: O(n^2)

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        for j in range(0, n-i-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
    return arr
```
## Selection Sort
Divides the input into two parts: sorted and unsorted.

First the **smallest** element in the **unsorted** sublist is found, and placed at the **end** of the **sorted sublist**. This is repeated until the list is fully sorted.

```python
def selection_sort(arr):
    for i in range(len(arr)):
        # Find the minimum element in the remaining unsorted array
        min_idx = i # assume current is the index of minimum
        for j in range(i+1, len(arr)):
            if arr[j] < arr[min_idx]:
                min_idx = j
        # Swap the found minimum element with the first element
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    return arr
```
## Insertion Sort
Segments the list into sorted and unsorted parts like selection sort. It iterates over the unsorted sublist, and then inserts the element being viewed into the correct position of the sorted sublist.

```python
def insertion_sort(arr):
    # Traverse through 1 to len(arr)
    for i in range(1, len(arr)):
        key = arr[i]
        # Move elements of arr[0..i-1], that are
        # greater than key, to one position ahead
        # of their current position
        j = i-1
        while j >=0 and key < arr[j]:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
    return arr
```

## Merge Sort
Merge Sort is a divide-and-conquer algorithm that divides the input array into two halves, calls itself for the two halves, and then merges the two sorted halves. The merge function is key to its performance, efficiently combining two sorted arrays into a single sorted array.

```python
def merge(left, right):
    result = []
    i = j = 0
    
    while i < len(left) and j < len(right):
        if left[i] < right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    
    # Append remaining elements
    result += left[i:]
    result += right[j:]
    
    return result

def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    return merge(left, right)
```
## Quick Sort

Quicksort is a divide-and-conquer algorithm that sorts an array by selecting a 'pivot' element and partitioning the other elements into two sub-arrays, according to whether they are less than or greater than the pivot. The steps can be summarized as:

1. Divide: Choose a pivot and partition the array such that elements less than the pivot are on the left, and elements greater than the pivot are on the right.
2. Conquer: Recursively apply the same process to the sub-arrays.
3. Combine: Since the algorithm sorts in place, no explicit merge step is needed like in merge sort.

### Choice of Pivot
The choice of the pivot can significantly affect the performance of Quicksort. Common strategies include:

- First Element: Choose the first element as the pivot.
- Last Element: Choose the last element as the pivot.
- Random Element: Choose a random element as the pivot to reduce the chance of worst-case complexity.
- Median: Ideally, choose the median of the array as the pivot, though this is computationally expensive to determine.
- Median of Three: Choose the median of the first, middle, and last elements as the pivot. This is a good compromise, offering better performance without significant overhead.

### Hoare Partition Scheme
Hoare's partitioning algorithm is an efficient way of partitioning an array into two parts around a pivot. It works by initializing two pointers (one at the start and one at the end of the array) and moving them towards each other until they find an inversion (a pair of elements where one is greater than the pivot and the other is smaller). These elements are then swapped.

```python
def partition_hoare(arr, low, high):
    pivot = arr[low]
    i = low - 1
    j = high + 1
    while True:
        i += 1
        while arr[i] < pivot:
            i += 1
        j -= 1
        while arr[j] > pivot:
            j -= 1
        if i >= j:
            return j
        arr[i], arr[j] = arr[j], arr[i]

def quicksort_hoare(arr, low, high):
    if low < high:
        p = hoare_partition(arr, low, high)
        quicksort_hoare(arr, low, p)
        quicksort_hoare(arr, p + 1, high)

def quicksort(arr):
    quicksort_hoare(arr, 0, len(arr) - 1)
```

### Lomuto Partition Scheme
The Lomuto partition scheme is simpler but less efficient than Hoare's scheme. It works by choosing the last element as the pivot, then moving all elements smaller than the pivot to the left of it and all greater elements to the right.

```python
def lomuto_partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    for j in range(low, high):
        if arr[j] < pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1

def quicksort_lomuto(arr, low, high):
    if low < high:
        p = lomuto_partition(arr, low, high)
        quicksort_lomuto(arr, low, p - 1)
        quicksort_lomuto(arr, p + 1, high)

def quicksort(arr):
    quicksort_lomuto(arr, 0, len(arr) - 1)
```

## Bucket Sort
Bucket Sort is a distribution sort that divides the input into several groups, called "buckets," and then sorts these buckets individually using a suitable sorting algorithm or recursively applying Bucket Sort. 

It's especially useful for data that is uniformly distributed over a range - i.e. when the range of values is N.

```python
def bucket_sort(arr):
    # 1. Create empty buckets
    bucket_count = 10
    buckets = [[] for _ in range(bucket_count)]
    
    # 2. Insert elements into their respective buckets
    for i in arr:
        index = int(bucket_count * i)
        buckets[index].append(i)
    
    # 3. Sort the elements of each bucket
    for i in range(bucket_count):
        buckets[i] = sorted(buckets[i])
        
    # 4. Concatenate all buckets into arr
    k = 0
    for i in range(bucket_count):
        for j in range(len(buckets[i])):
            arr[k] = buckets[i][j]
            k += 1
    return arr

# Example usage
arr = [0.42, 0.32, 0.23, 0.52, 0.25, 0.47, 0.51]
print("Original array:", arr)
sorted_arr = bucket_sort(arr)
print("Sorted array:", sorted_arr)
```

## Counting Sort
Counting Sort is a non-comparative sorting algorithm suitable for sorting items with integer keys. It operates by counting the number of objects that have each distinct key value and using arithmetic on those counts to determine the positions of each key value in the output sequence.

- **Non-Comparative**: Unlike other sorting algorithms, Counting Sort does not compare the elements of the array to sort them.
- **Stable Sort**: Preserves the relative order of records with equal keys.
- **Efficient for Small Range of Keys**: Best suited when the range of potential items (k) is not significantly greater than the number of items (n).


### Algorithm Steps
1. **Count Occurrences**: Count the occurrences of each unique element.
2. **Cumulative Count**: Transform the count array to store the cumulative sum of counts.
3. **Place the Elements**: Place each element in its correct position based on the cumulative count and decrease the count by one for each placement.

```python
def counting_sort(arr):
    max_val = max(arr)
    m = max_val + 1
    count = [0] * m
                
    for a in arr:
        count[a] += 1
    i = 0
    for a in range(m):
        for c in range(count[a]):
            arr[i] = a
            i += 1
    return arr

# Example usage
arr = [4, 2, 2, 8, 3, 3, 1]
sorted_arr = counting_sort(arr)
print("Sorted array:", sorted_arr)
```

## Radix Sort
Radix Sort is a non-comparative integer sorting algorithm that sorts data with integer keys by grouping keys by the individual digits which share the same significant position and value. It processes integer keys from least significant digit to most significant digit.Radix Sort is a non-comparative integer sorting algorithm that sorts data with integer keys by grouping keys by the individual digits which share the same significant position and value. It processes integer keys from least significant digit to most significant digit.Radix Sort is a non-comparative integer sorting algorithm that sorts data with integer keys by grouping keys by the individual digits which share the same significant position and value. It processes integer keys from least significant digit to most significant digit.

- **Non-Comparative**: Does not compare elements directly.
- **Stable Sort**: Maintains the relative order of records with equal keys.
- **Efficient for Large Keys**: Performs well when the keys are large.

### Algorithm Steps
1. **Get the Maximum Number**: To know the number of digits.
2. **Counting Sort for Each Digit**: Apply counting sort to sort elements based on the current digit.
    - Repeat for each digit until the largest number.

```python
def counting_sort_for_radix(arr, exp):
    n = len(arr)
    output = [0] * n
    count = [0] * 10

    for i in range(n):
        index = arr[i] // exp
        count[index % 10] += 1

    for i in range(1, 10):
        count[i] += count[i - 1]

    i = n - 1
    while i >= 0:
        index = arr[i] // exp
        output[count[index % 10] - 1] = arr[i]
        count[index % 10] -= 1
        i -= 1

    for i in range(n):
        arr[i] = output[i]

def radix_sort(arr):
    max_val = max(arr)
    exp = 1
    while max_val // exp > 0:
        counting_sort_for_radix(arr, exp)
        exp *= 10

# Example usage
arr = [170, 45, 75, 90, 802, 24, 2, 66]
radix_sort(arr)
print("Sorted array:", arr)
```
- Radix Sort uses counting sort as a subroutine to sort.
- The efficiency of Radix Sort depends on the digit length of the numbers.
- It's excellent for sorting numbers or strings of characters that can be represented as numbers.

## Heap Sort
Heap Sort is a comparison-based sorting algorithm that builds a heap from the input data, then repeatedly extracts the maximum element from the heap and rebuilds the heap until all elements are sorted.

- **In-Place Sorting**: Though it operates on the array itself to sort it, a small constant extra space is used for the heap.
- **Not Stable**: Does not maintain the relative order of records with equal keys.
- **Efficient for Large Data Sets**: Offers good performance on large data sets.


### Algorithm Steps
1. **Build a Max Heap**: Rearrange the array to form a max heap, where the largest element is at the root.
2. **Extract Elements from Heap**: Repeatedly move the root of the heap to the end of the array, reduce the size of the heap by one, and then rebuild the heap.
3. **Repeat Until Sorted**: Continue the extraction process until all elements are sorted.

```python
def heapify(arr, n, i):
    largest = i
    l = 2 * i + 1
    r = 2 * i + 2

    # See if left child of root exists and is greater than root
    if l < n and arr[l] > arr[largest]:
        largest = l

    # See if right child of root exists and is greater than the largest so far
    if r < n and arr[r] > arr[largest]:
        largest = r

    # Change root, if needed
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]

        # Heapify the root
        heapify(arr, n, largest)

def heap_sort(arr):
    n = len(arr)

    # Build a maxheap
    for i in range(n//2 - 1, -1, -1):
        heapify(arr, n, i)

    # Extract elements one by one
    for i in range(n-1, 0, -1):
        arr[i], arr[0] = arr[0], arr[i]  # swap
        heapify(arr, i, 0)

# Example usage
arr = [12, 11, 13, 5, 6, 7]
heap_sort(arr)
print("Sorted array is:", arr)
```

## Cycle Sort
Cycle Sort is an in-place, comparison-based sorting algorithm that is theoretically optimal in terms of the total number of writes to the original array. It's particularly useful where memory write or swap operations are costly.

- **In-Place Sorting**: Does not require additional space for sorting, except for variable storage.
- **Not Stable**: May change the relative order of equal elements.
- **Write-Efficient**: Minimizes the number of memory writes, which is its main advantage.

### Algorithm Steps
1. **Find Cycles**: Start from the first element and consider it as the starting point of a cycle. Then, find the correct position for this element by counting the number of elements that are smaller.
2. **Rotate Cycle**: Once the correct position is found, place the element there and repeat the process for all elements to complete the cycle.
3. **Repeat**: Continue until the end of the array is reached.

```python
def cycle_sort(arr):
    writes = 0
    for cycle_start in range(0, len(arr) - 1):
        item = arr[cycle_start]
        pos = cycle_start
        for i in range(cycle_start + 1, len(arr)):
            if arr[i] < item:
                pos += 1
        if pos == cycle_start:
            continue
        while item == arr[pos]:
            pos += 1
        arr[pos], item = item, arr[pos]
        writes += 1
        while pos != cycle_start:
            pos = cycle_start
            for i in range(cycle_start + 1, len(arr)):
                if arr[i] < item:
                    pos += 1
            while item == arr[pos]:
                pos += 1
            arr[pos], item = item, arr[pos]
            writes += 1
    return arr

# Example usage
arr = [1, 8, 3, 9, 10, 10, 2, 4 ]
cycle_sort(arr)
print("Sorted array is:", arr)
```