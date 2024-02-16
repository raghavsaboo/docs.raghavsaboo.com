## Which Database to Use?

- Every storage engine is optimized for different use cases. Select the right storage
  engine for your use case
- As an application developer we need to have a rough idea on what the storage engine
  is doing under the hood
- Tuning and optimizing pointers

### Categories of Databases

There are two main categories of databases - OLTP (Online Transaction Processing Database)
and OLAP (Online Analytical Processing Database) each with a different read pattern,
write patterns, user using it, data size etc.

1. OLTP - Online Transaction Processing Database optimized for latency.
   - eg. MySQL
   - Usually row-order store
     - easy to modify/add a record
     - might read in unnecessary data
2. OLAP - Online Analytical Processing Databases optimized for data crunching.
   - Data Warehousing (Star/Snowflake schema), column oriented
   - Column compression, data cubes, optimized for reads/queries
   - Materialized views, lack of flexibility
   - HBase, Hive, Spark
   - Usually column-order store
     - Only need to read in relevant data
     - Tuple writes require multiple acesses
     - Suitable for read-mostly, read-intensive, large data repositories

### Database Index

- An index is an additional structure that is derived from the primary data. A well chosen
  index optimizes for reads but slows down the write.
- Simple database index is a Hash based Index. Some issues for an index:
  - File format (encoding)
  - Deleting records
  - Crash recovery
  - Partially written records
  - Concurrency control
  - Range queries?

### Types of Storage Engines

- Two families of storage engines used by databases:
  - Log structured - LSM-Trees e.g. SSTables -> HBASE, Cassandra
  - Page-Oriented - B-trees -> RDBMS
- These are answers to limitations of disk access.

#### LSM-Trees (Log Sort Merge) and SSTables (Sort String Tables)

- SSTables - in-memory mem table backend by Disk SSTable file, sorted by keys.
  - e.g. Red-Black tree or AVL trees. Supports high write throughput.
- Lucene - full-text search is much more complex than key-value index like SSTables. However,
  it does internally use SSTables for term dictionary.
- Bloom filters - memory efficient data structure used for approximating the contents
  of a set. It can tell you if a key does not appear in the database, thus saves many
  unnecessary disk reads for non-existent keys.
- Compaction is a background process of the means of throwing away duplicate keys in
  the log and keeping only the most recent update for each key.

#### B-Trees Index

- Most widely used indexing structure is B-Trees. One place per key!
- They are the standard implementation in RDBMS and NoSQL stores today.
- It also keeps key-value sorted by keys which allows quick lookups.
- B-Trees are designed and optimized for the hardward as disks are arranged in fixed
  sized blocks, B-Trees also break down the data into fixed size 4KB blocks. There is
  a root node and a branching factor (references to child pages)
- 4 level tree with 4KB pages with branching factor of 500 can store up to 256TB! B-Tree
  is optimized for reads!
- Write ahead log is used for crash recovery, latches for concurrency.
- Sibling references in child node allows for easier scannig of sequential keys.

### Other Indexing Concepts

- Clustered index - inline storing of row values
- Secondary index - helps with joins
- Covering index - few columns are included
- Multi-column index - multiple keys concatenated
- Full-text search and fuzzy indexes help with spelling mistakes (edit distances), grammar,
  synonyms, words near each other, linguistics etc.
- In Memory Stores - Redis, Memcaches etc.
