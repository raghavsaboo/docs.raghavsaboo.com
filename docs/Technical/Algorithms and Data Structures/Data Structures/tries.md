# Tries

Tries, also known as digital trees or radix trees, are tree-like data structures used for storing and searching a set of strings or keys. Tries offer efficient lookup operations, especially for string-related tasks like autocomplete, spell checking, and searching.

#### Standard Tries:

- **Definition**: A standard trie is a tree structure where each node represents a single character of the string. The root node is typically empty, and each edge corresponds to a character in the string. Nodes are labeled with characters, and each node may have multiple children representing possible continuations of the string.
- **Representation in Python**: Tries can be represented using nested dictionaries or arrays. Each node can be a dictionary where keys represent characters and values represent pointers to child nodes.
- **Complexity**:
    - **Space Complexity**: $O(n \cdot m)$, where $n$ is the number of strings and $m$ is the average length of the strings.
    - **Search Complexity**: $O(m)$, where $m$ is the length of the string being searched.

#### Compressed Tries:

- **Definition**: Compressed tries aim to reduce space consumption by merging chains of nodes with a single child into a single node. This compression technique reduces the memory footprint of the trie while preserving search efficiency.
- **Representation in Python**: Compressed tries can also be represented using nested dictionaries or arrays similar to standard tries. The main difference lies in the compression algorithm used during trie construction.
- **Complexity**:
    - **Space Complexity**: Varies based on the compression ratio achieved. In practice, it can significantly reduce space consumption compared to standard tries.
    - **Search Complexity**: Similar to standard tries, $O(m)$.
##### Compression Algorithm:

1. **Iterative Compression**: Starting from the root of the trie, traverse each branch of the trie. If a node has only one child, merge it with its child node and continue the traversal until no further compression can be performed.

2. **Updating Pointers**: During compression, ensure that pointers to the merged nodes are updated appropriately. Update the parent node's reference to point directly to the merged child node.

3. **Recursive Compression**: To handle deeper levels of compression, the compression process can be applied recursively to each child node.

#### Suffix Tries:

- **Definition**: Suffix tries are a type of trie specifically designed to store all suffixes of a given string. They are useful in pattern matching algorithms and string-related tasks where efficient suffix searches are required.
- **Representation in Python**: Suffix tries can be represented similarly to standard tries, but they specifically store all suffixes of a string. Each node in the trie represents a suffix of the original string.
- **Complexity**:
    - **Space Complexity**: $O(n^2)$, where $n$ is the length of the original string.
    - **Search Complexity**: $O(m)$, where $m$ is the length of the search string.
##### Construction Algorithm:

1. **Suffix Tree**: Start with an empty trie. Insert all suffixes of the input string into the trie, ensuring that each suffix is inserted as a separate branch from the root.

2. **Suffix Link**: For each internal node representing a suffix, add a suffix link pointing to the node representing the longest proper suffix of the current suffix.

3. **Leaf Nodes**: Leaf nodes in the trie represent the end positions of suffixes in the original string.

## Search Engine Indexing
Search engine indexing is the process of organizing and storing information retrieved from web pages to facilitate efficient and accurate search queries. Central to this process is the construction of an inverted index, which maps each unique term in a document collection to the documents that contain it.

### Inverted Index:

An inverted index is a data structure that maps terms (words or phrases) to the documents that contain them. Instead of searching through entire documents, a search engine can quickly locate relevant documents by querying the inverted index.

### Occurrence Lists:

For each term in the inverted index, an occurrence list (also known as a posting list) is maintained. The occurrence list contains information about the occurrences of the term in the document collection, such as the document IDs or positions where the term appears.

### Index Terms:

Index terms refer to the terms extracted from documents that are indexed for searching. Commonly, index terms undergo preprocessing steps like tokenization, stemming, and stop word removal to improve search accuracy and efficiency.

### Data Structure:

The inverted index is typically implemented using a data structure like a hash table or a balanced tree (e.g., B-tree or AVL tree) for efficient term lookup. Each entry in the inverted index points to an occurrence list containing information about the occurrences of the term.

### Example:

Consider a small document collection with the following documents:

1. Document 1: "The quick brown fox jumps over the lazy dog."
2. Document 2: "A brown fox is quick and agile."
3. Document 3: "The dog is lazy but friendly."

The inverted index for this collection might look like:

| Term   | Occurrence List         |
|--------|-------------------------|
| a      | {2}                     |
| agile  | {2}                     |
| and    | {2}                     |
| brown  | {1, 2}                  |
| but    | {3}                     |
| dog    | {1, 3}                  |
| fox    | {1, 2}                  |
| friendly | {3}                    |
| is     | {2, 3}                  |
| jumps  | {1}                     |
| lazy   | {1, 3}                  |
| over   | {1}                     |
| quick  | {1, 2}                  |
| the    | {1, 3}                  |

In this example, each term is mapped to the documents where it occurs, forming the inverted index. For instance, the term "brown" appears in documents 1 and 2, so its occurrence list contains document IDs 1 and 2.

### Importance:

Search engine indexing is crucial for enabling fast and accurate information retrieval from large document collections. By organizing documents into an inverted index and maintaining occurrence lists, search engines can efficiently identify relevant documents in response to user queries.

**References**
[^1]: Skiena, Steven S.. (2008). The Algorithm Design Manual, 2nd ed. (2). : Springer Publishing Company.
[^2]: Goodrich, M. T., Tamassia, R., & Goldwasser, M. H. (2013). Data Structures and Algorithms in Python (1st ed.). Wiley Publishing.
[^3]: Cormen, T. H., Leiserson, C. E., & Rivest, R. L. (1990). Introduction to algorithms. Cambridge, Mass. : New York, MIT Press.