# Consistent Hashing

## What is Consistent Hashing?

Consistent hashing is a technique used in distributed systems for load balancing and data partitioning. It provides a way to distribute data or requests among a cluster of nodes in a way that minimizes the need for remapping when nodes are added or removed.

## Why is Consistent Hashing Useful?

Consistent hashing solves two main problems in distributed systems:

1. **Load Balancing**: It allows for an even distribution of data or requests across nodes in a cluster, ensuring no single node gets overloaded.

2. **Partition Tolerance**: When a node is added or removed from the cluster, only a small fraction of data or requests need to be remapped, ensuring minimal disruption to the system.

## How Does Consistent Hashing Work?

1. **Hash Function**: A hash function is used to map data or requests to points on a circular hash ring. The hash function should provide a uniform distribution of values.

2. **Hash Ring**: The hash ring is a circular space where each node in the cluster is assigned a position on the ring using the hash function.

3. **Data/Request Mapping**: To map a data item or request to a node, its key is hashed using the same hash function, and the resulting value is mapped to the nearest node position on the ring in a clockwise direction.

4. **Node Addition/Removal**: When a new node is added to the cluster, it is assigned a position on the ring. Only the data or requests that fall between the new node and the next node in a clockwise direction need to be remapped. Similarly, when a node is removed, only the data or requests that were previously mapped to that node need to be remapped to other nodes.

## Advantages of Consistent Hashing

- **Incremental Scalability**: Nodes can be added or removed without disrupting the entire system, ensuring scalability.
- **Even Load Distribution**: Data or requests are distributed evenly across nodes, preventing hotspots and ensuring better performance.
- **Fault Tolerance**: If a node fails, its data or requests are automatically remapped to other nodes, providing fault tolerance.

## Disadvantages of Consistent Hashing

- **Non-uniform Load Distribution**: In some cases, data or requests may not be distributed evenly across nodes due to the hash function's behavior or the distribution of keys.
- **Remapping Overhead**: While consistent hashing minimizes remapping, there is still some overhead involved when nodes are added or removed.
- **Hot Spot Prevention**: Additional techniques like virtual nodes may be required to prevent hotspots when the distribution of keys is skewed.

## Use Cases for Consistent Hashing

Consistent hashing is widely used in distributed systems, including:

- **Distributed Caching**: Memcached, Redis
- **Distributed Storage**: Cassandra, Amazon DynamoDB
- **Load Balancing**: HAProxy, Nginx
- **Peer-to-Peer Networks**: BitTorrent
- **Content Delivery Networks (CDNs)**: Akamai, Cloudflare
