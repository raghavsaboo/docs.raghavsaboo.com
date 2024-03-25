# Text Processing and Pattern Matching
In classic pattern matching, we are given a text string `T` of length `n` and a pattern string `P` of length `m`, and want to find whether `P` is a substring of `T`. If so, we may want to find the lowest index `j` within `T` at which `P` begins, such that `T[j:j+m]` equals `P`, or perhaps find all indices of `T` at which pattern `P` begins. 

## Brute Force
Time complexity: $O(nm)$

Compare all substrings in `T` with `P`. 

```python
def find_brute(T, P):
    """Return lowst index of T at which substring P begins (or else -1)"""
    n, m = len(T), len(P)                      # introduce convenient notations
    for i in range(n-m+1):                     # try every potential starting index within T
        k = 0                                    # an index into pattern P
        while k < m and T[i + k] == P[k]:        # kth character of P matches
        k += 1
        if k == m:                               # if we reached the end of pattern,
        return i                               # substring T[i:i+m] matches P
    return -1                                  # failed to find a match starting with any i
```

## Boyer-Moore Algorithm

Introduces two time-saving heuristics:

1. **Looking Glass Heuristic** - when testing a possible placement of `P` against `T`, begin the comparisons from the of `P` and move bckward to the front of `P`.
2. **Character Jump Heuristic** - during the testing of a possible placement of `P` within `T`, a mismatch of text character `T[i]=c` with the corresponding pattern character `P[k]` is handled as follows: if `c` is not contained anywhere in `P`, then shift `P` completely past `T[i]` (for it cannot match any character in `P`), otherwise shift `P` until an occurance of character `c` in `P` gets aligned with `T[i]`.

The time complexity of the Boyer-Moore algorithm depends on the characteristics of the input text and pattern. 

- **Best Case**: In the best-case scenario, when there are many mismatches and few matches, the algorithm can achieve linear time complexity ($O(n/m)$), where $n$ is the length of the text and $m$ is the length of the pattern.

- **Worst Case**: In the worst-case scenario, the algorithm may exhibit quadratic time complexity ($O(mn)$). 

Despite the worst-case time complexity, Boyer-Moore is often faster than other string searching algorithms in practice, especially for longer patterns and texts, due to its ability to skip large portions of the text efficiently based on precomputed rules.

```python
def find_boyer_moore(T, P):
  """Return the lowest index of T at which substring P begins (or else -1)."""
  n, m = len(T), len(P)                   # introduce convenient notations
  if m == 0: return 0                     # trivial search for empty string
  last = {}                               # build 'last' dictionary
  for k in range(m):
    last[ P[k] ] = k                      # later occurrence overwrites
  # align end of pattern at index m-1 of text
  i = m-1                                 # an index into T
  k = m-1                                 # an index into P
  while i < n:
    if T[i] == P[k]:                      # a matching character
      if k == 0:
        return i                          # pattern begins at index i of text
      else:
        i -= 1                            # examine previous character
        k -= 1                            # of both T and P
    else:
      j = last.get(T[i], -1)              # last(T[i]) is -1 if not found
      i += m - min(k, j + 1)              # case analysis for jump step
      k = m - 1                           # restart at end of pattern
  return -1
```

## Knuth-Morris-Pratt Algorithm
The Knuth-Morris-Pratt (KMP) algorithm works by utilizing the information about previous matches to avoid unnecessary comparisons.

### Key Components:
1. **Failure Function (Prefix Function)**: The algorithm precomputes a "failure function" or "prefix function," which stores the length of the longest proper prefix that is also a suffix of the pattern. This information is used to determine the amount to shift the pattern when a mismatch occurs.

2. **Partial Match Table (LPS Array)**: The failure function is often implemented using a partial match table, also known as the "Longest Proper Prefix which is also Suffix" (LPS) array. This table helps to efficiently compute the shift amount during the search phase.

### Search Phase:
1. **Comparison of Pattern and Text**: The algorithm compares characters of the pattern with corresponding characters in the text from left to right. When a mismatch occurs, instead of shifting the pattern by one position, KMP uses the failure function to determine the maximum possible shift without missing potential matches.

2. **Utilizing LPS Array**: The LPS array helps to avoid redundant comparisons by efficiently skipping over previously matched characters. When a mismatch occurs, the algorithm uses the LPS value of the previous character to determine the next possible comparison position in the pattern.

### Knuth-Morris-Pratt Algorithm: Example

Consider searching for the pattern "ABABCAB" in the text "ABABDABACDABABCABAB". Here's a brief overview of how the KMP algorithm works:

1. Precompute the LPS array for the pattern: "ABABCAB"
   - LPS array: $[0, 0, 1, 2, 0, 1, 2]$

2. Start the search phase:
   - Begin comparing characters of the pattern and the text from left to right.
   - When a mismatch occurs, use the LPS value of the previous character to determine the next comparison position.

3. Progress through the text:
   - When a match occurs, continue comparing subsequent characters.
   - When a mismatch occurs, use the LPS value to determine the next position to start comparing from.

4. Continue until the end of the text is reached or a complete match of the pattern is found.

### Python implementation
```python
def find_kmp(T, P):
  """Return the lowest index of T at which substring P begins (or else -1)."""
  n, m = len(T), len(P)            # introduce convenient notations
  if m == 0: return 0              # trivial search for empty string
  fail = compute_kmp_fail(P)       # rely on utility to precompute
  j = 0                            # index into text
  k = 0                            # index into pattern
  while j < n:
    if T[j] == P[k]:               # P[0:1+k] matched thus far
      if k == m - 1:               # match is complete
        return j - m + 1           
      j += 1                       # try to extend match
      k += 1
    elif k > 0:                    
      k = fail[k-1]                # reuse suffix of P[0:k]
    else:
      j += 1
  return -1                        # reached end without match

def compute_kmp_fail(P):
  """Utility that computes and returns KMP 'fail' list."""
  m = len(P)
  fail = [0] * m                   # by default, presume overlap of 0 everywhere
  j = 1
  k = 0
  while j < m:                     # compute f(j) during this pass, if nonzero
    if P[j] == P[k]:               # k + 1 characters match thus far
      fail[j] = k + 1
      j += 1
      k += 1
    elif k > 0:                    # k follows a matching prefix
      k = fail[k-1]
    else:                          # no match found starting at j
      j += 1
  return fail
```

### Knuth-Morris-Pratt Algorithm: Time Complexity

The time complexity of the Knuth-Morris-Pratt algorithm is $O(n + m)$, where $n$ is the length of the text and $m$ is the length of the pattern. This complexity arises from the fact that each character of the text is compared at most once, and the precomputation of the LPS array takes $O(m)$ time.

## Least Common Substring Algorithm
Find the longest string `S` that is a subsequence of both character string `X` and `Y`.

The dynamic programming solution for the LCS problem involves building a table to store the lengths of the longest common subsequences of prefixes of the given sequences. The table is filled iteratively, starting from the shortest prefixes and gradually building up to the entire sequences.

### Key Steps:
1. **Initialization**: Initialize a table $dp$ with dimensions $(n+1) \times (m+1)$, where $n$ and $m$ are the lengths of the two sequences, respectively. Initialize the first row and first column of the table to 0.

2. **Filling the Table**: Iterate over the sequences and fill the table according to the following rules:
   - If the current characters match, increment the value in the table corresponding to the previous characters by 1.
   - If the current characters do not match, take the maximum of the values obtained by either excluding one of the characters and considering the LCS obtained so far or by excluding the other character and considering the LCS obtained so far.
   
3. **Backtracking**: Once the table is filled, backtrack from the bottom-right corner to reconstruct the longest common subsequence.

### Example:
Consider two sequences "ABCBDAB" and "BDCAB". Here's a step-by-step explanation of the dynamic programming solution:

1. Initialize the table:
```css
   |   | A | B | C | B | D | A | B |
   |---|---|---|---|---|---|---|---|
   |   | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
 B | 0 | 0 | 1 | 1 | 1 | 1 | 1 | 1 |
 D | 0 | 0 | 1 | 1 | 1 | 2 | 2 | 2 |
 C | 0 | 0 | 1 | 2 | 2 | 2 | 2 | 2 |
 A | 0 | 1 | 1 | 2 | 2 | 2 | 3 | 3 |
 B | 0 | 1 | 2 | 2 | 3 | 3 | 3 | 4 |
```
2. Backtrack from the bottom-right corner to reconstruct the LCS: "BCAB"

### Time Complexity:
The time complexity of the dynamic programming solution for the LCS problem is $O(nm)$, where $n$ and $m$ are the lengths of the two sequences, respectively. This is because we fill an $n \times m$ table, and each cell requires constant time to compute based on the values of its neighbors.

### Python Implementation

```python
def LCS(X, Y):
  """Return table such that L[j][k] is length of LCS for X[0:j] and Y[0:k]."""
  n, m = len(X), len(Y)                      # introduce convenient notations
  L = [[0] * (m+1) for k in range(n+1)]      # (n+1) x (m+1) table
  for j in range(n):
    for k in range(m):
      if X[j] == Y[k]:                       # align this match
        L[j+1][k+1] = L[j][k] + 1            
      else:                                  # choose to ignore one character
        L[j+1][k+1] = max(L[j][k+1], L[j+1][k])
  return L

def LCS_solution(X, Y, L):
  """Return the longest common substring of X and Y, given LCS table L."""
  solution = []
  j,k = len(X), len(Y)
  while L[j][k] > 0:                   # common characters remain
    if X[j-1] == Y[k-1]:
      solution.append(X[j-1])
      j -= 1
      k -= 1
    elif L[j-1][k] >= L[j][k-1]:
      j -=1
    else:
      k -= 1
  return ''.join(reversed(solution))   # return left-to-right version
```

## Text Compression
The text compression problem involves efficiently encoding a string $x$ into a smaller binary string $y$ using only the characters 0 and 1. The goal is to reduce the size of the encoded string while preserving its essential information.

### Huffman Coding

Huffman coding is a popular algorithm used for lossless data compression. It works by assigning variable-length codes to input characters, with shorter codes assigned to more frequent characters and longer codes assigned to less frequent characters. This ensures that the most common characters are represented by shorter codes, resulting in efficient compression.

#### Key Concepts:

1. **Character Frequencies**: Huffman coding takes advantage of the frequency distribution of characters in the input text. Characters that occur more frequently are assigned shorter codes, while characters that occur less frequently are assigned longer codes.

2. **Prefix Code**: Huffman codes are prefix-free codes, also known as prefix codes. This means that no code word is a prefix of another code word. This property allows for uniquely decodable encoding and decoding.

3. **Binary Tree Representation**: Huffman codes can be represented using binary trees, where each leaf node corresponds to a character and each internal node corresponds to the sum of the frequencies of its children. The tree is constructed in a way that minimizes the total encoding length.

### Huffman Coding Algorithm

The Huffman coding algorithm follows these steps:

1. **Frequency Counting**: Count the frequency of each character in the input text.
2. **Build Huffman Tree**: Build a Huffman tree using a priority queue (min-heap) or similar data structure. The tree is constructed by repeatedly merging the two nodes with the lowest frequencies until only one node remains, which becomes the root of the Huffman tree.
3. **Generate Codes**: Traverse the Huffman tree to generate Huffman codes for each character. The code for each character is determined by the path from the root of the tree to the leaf node representing that character.
4. **Encode Data**: Encode the input text using the generated Huffman codes.
5. **Decode Data**: Decode the encoded text using the same Huffman tree.

### Pseudocode

```plaintext
Node:
    character
    frequency
    left_child
    right_child

HuffmanCoding(X):
    1. Count the frequency of each character in string X.
    2. Create an empty min-heap Q.
    3. for each character c in X:
         - Insert a node containing c, its frequency, and null left and right children into Q.
    4. while |Q| > 1:
         - Remove two nodes with the lowest frequencies, say node1 and node2, from Q.
         - Create a new internal node with frequency = node1.frequency + node2.frequency.
         - Set node1 as the left child and node2 as the right child of the new internal node.
         - Insert the new internal node back into Q.
    5. root = Q.pop()  // The root of the Huffman tree is the last node remaining in Q.
    6. GenerateCodes(root, "")  // Call a recursive function to generate Huffman codes for each character.

GenerateCodes(node, code):
    if node is a leaf node:
        Output node.character and code  // Output the character and its corresponding code
    else:
        GenerateCodes(node.left_child, code + "0")  // Traverse left and append '0' to the code
        GenerateCodes(node.right_child, code + "1")  // Traverse right and append '1' to the code
```

### Complexity Analysis

- **Building Huffman Tree**: The time complexity of building the Huffman tree is $O(n \log n)$, where $n$ is the number of unique characters in the input text. This is because each insertion and deletion operation in the priority queue takes $O(\log n)$ time, and there are $n$ characters to process.
- **Generating Codes**: The time complexity of generating Huffman codes is $O(n)$, where $n$ is the number of unique characters in the input text. This is because each character corresponds to a leaf node in the Huffman tree, and traversing from the root to each leaf node takes constant time.
- **Encoding and Decoding**: The time complexity of encoding and decoding the text using Huffman codes is $O(m)$, where $m$ is the length of the input text. This is because each character in the input text is replaced by its corresponding Huffman code, which takes constant time.