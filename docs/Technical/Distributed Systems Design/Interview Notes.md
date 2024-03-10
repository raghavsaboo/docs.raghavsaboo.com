# Interview Notes

## Components to know about

- [Domain Name System (DNS)](./Domain%20Name%20System.md)
- [Proxies and Reverse Proxies](./Proxy%20and%20Reverse%20Proxy.md)
- [Load Balancers](./Load%20Balancing.md)
- [Relational and NoSQL Databases](./Databases.md)
- [Object Storage]
- [CDN]
- [Servers](./Servers.md)
- [Cache](./Cache.md)

## Concepts to know

- [CAP](./CAP%20Theorem.md)
- [PACELC](./PACELC%20Theorem.md)
- [Distributed Systems Requirements](./index.md)
- [Testing](./Testing.md)
- [Consistent Hashing](./Consistent%20Hashing.md)

## Data Structures to Know

- [Trie]() - Search Autocomplete
- [Quadtree]() - Location Based Indexing
- [R-Tree](./Key%20Datastructures.md) - Location Based Indexing, sercing multi-dimension shapes, asuch as nearest neighbor lookups
- [Geohash]() - Location Based Indexing
- [Skiplist](./Key%20Datastructures.md) - in memory index type
- [Hash Index](./Key%20Datastructures.md) - in-memory index type - common implementation of the "Map" datastructure
- [SSTable](./Key%20Datastructures.md) - disk based "Map" datastructure
- [LSM Tree](./Key%20Datastructures.md) - skiplist (memory) + sstable (disk) combination to provide high write throughput
- [B-Tree](./Key%20Datastructures.md) - disk based index with consistent read/write performance - most popular index used in databases
- [Inverted Index](./Key%20Datastructures.md) - used for document indexing in search
- [Suffix Tree](./Key%20Datastructures.md) - used for string pattern search, such as string suffix match

## Algorithms to Know

- [Consistent Hashing]() - Balancing load within a cluster of services
- [Bloomfilter]() - Eliminate costly lookups
- [Leaky Bucket]() - Rate limiter
- [Token Bucket]() - Rate limited
- [RSync]() - File transfers
- [Raft/Paxos]() - Consensus algorithm
- [Merkle Tree]() - Identify inconsistencies between nodes
- [HyperLogLog]() - Count unique values fast
- [Count-min Sketch]() - Estimate frequencies of items