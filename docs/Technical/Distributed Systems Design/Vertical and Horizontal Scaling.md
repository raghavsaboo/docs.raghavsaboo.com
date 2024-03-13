# Vertical and Horizontal Scaling

## Vertical Scaling

## Horizontal Scaling

Distributing data over multiple nodes.

### Partitioning / Sharding

Splitting database or service across multiple nodes for scalability, such that each one has a subset of the whole data. To handle increased query rates and data amounts, we strive for balanced partitions and blaanced read/write load.

Partitioning must be balanced so that each partition recieves about the same amount of data - if it is unbalanced a majority of the queries will fall into a few partitions and become heavily loaded system bottlenecks. 

#### Vertical Sharding

Distributing tables across multiple nodes, where separate databases are used for sets of tables (e.g. payments related tables in one database server, and user related tables in another database server).

We need to be careful if there are joins between multiple tables, and keep them together on one shard. 

Often used to increase the speed of data retrieval from a table consisting of columns with very wide text or binary object data. In this case the column with the large text or blob is split into another table.

This is usually more ameanable to manual sharding.

#### Horizontal Sharding

When tables in the database become too big they also affect read/write latency - and horizontal sharding helps splitting the data row-wise and distributing each partition over database servers.

##### Strategies

1. **Key-range** based sharding
    - Details:
        - each partition is assigned a continuous range of keys
        - for tables in a relation (bound by foreign key) use the same partition, and tables that belong to the same partition key are colocated in the same partition
        - a partition key table is used by applications to look for data in a specific shard
        - a `created_at` column serves as the data consistency point, assuming clocks of nodes are synhronized
    - Pros:
        - easy to implement range queries as we know exactly which node/shard to look for a specific range of keys
    - Cons:
        - range queries on data other than partition key is hard
        - some nodes may store more data due to uneven distribution

2. **Hash-based** sharding
    - Details:
        - uses hash function on an attribute
        - gives each partition a range of hashes
    - Pros:
        - uniform distribution of keys
    - Cons:
        - can't use to perform range queries

##### Some Key Concepts

1. **Consistent Hashing**
    - Details:
        - assigns each server or item in a distributed has table a place on an abstract circle (hash ring) irrespective of the number of servers/shard on the table. 
        - this permits servers/shard to scale without compromising the system's overall performance i.e. minimal reshuffle due to adding or removing shards when using hash based sharding
    - Pros:
        - easy to scale horizontally
        - increases throughput and improves latency
    - Cons:
        - randomly assigning nodes on the ring may cause non-uniformity

2. **Rebalancing**
    - Details:
        - query load can get imbalanced across nodes due to:
            - distributionof data not being equal
            - too much load on a single partition
            - increase in query traffic
        - key strategies:
          - avoid hash mod n (as changing number of shards will re-shuffle all data)
          - fixed number of partitions
          - dynamic partitioning
          - partition proportionally to nodes

3. **Partioning and Secondary Indexes**
    - strategies for managing secondary indexes in a partitioned database:
        - **Partition by Document**: Local indexes that are independent per partition.
        - **Partition by Term**: Global indexes that span all partitions, suitable for read-heavy workloads but complicating writes.

4. **Request Routing**

    - Ensuring clients connect to the correct data node:
        - **Direct Routing**: Clients have knowledge of the partitioning scheme.
        - **Routing Tier**: A middleware layer that directs client requests based on partition information.
        - **ZooKeeper**: Manages cluster state and routing information, notifying clients or routing layers of changes.


##### Key Issues to Look Out For

- Hot spots / Skews
- Key Hash Based Partition
- Rebalancing strategies
- Routing Logic Placement

### Replication

The process of keeping multiple copies of data and syncing data between databases nodes in a consistent way to achieve availability, scalability and performance.

This is needed due to a few reasons:

- Resilience of service to machine failures
- Improving latency for global audience
- Offline/Network failures

#### Types of Replication Models

- **Single Leader Replication**
    - **Characteristics**: One database (the leader) handles all write operations. Followers (slaves) replicate the leader's state and serve read operations.
    - **Advantages**: Simplifies conflict resolution and write consistency. Improves read scalability.
    - **Disadvantages**: Single point of failure at the leader. Potential for replication lag causing stale reads.

- **Multi-Leader Replication**
    - **Characteristics**: Multiple databases act as leaders, each capable of handling writes. Writes are replicated across all leaders.
    - **Advantages**: Eliminates single point of failure. Reduces write latency in geographically distributed setups.
    - **Disadvantages**: Complex conflict resolution. Higher system configuration and maintenance complexity.

- **Leaderless Replication**
    - **Characteristics**: Every node can accept both reads and writes, replicating data across several nodes without a central leader.
    - **Advantages**: High availability and fault tolerance. Enhanced scalability and performance.
    - **Disadvantages**: Complex conflict resolution mechanisms needed. Typically offers eventual consistency.

#### Methods to Capture Changes for Replication

- **Write-Ahead Logging (WAL)**
    - **Mechanism**: Before any changes are made to the database, they are logged. This log is then used to replicate changes to follower nodes.
    - **Used By**: Primarily single leader replication models for ensuring data consistency and aiding in recovery processes.
    - **Advantages**: Ensures data integrity and aids in crash recovery.
    - **Disadvantages**: Can increase disk I/O overhead.

- **Statement-Based Replication**
    - **Mechanism**: The SQL statements executed on the master (or leader) database are logged and then executed on the follower databases.
    - **Used By**: Both single leader and, in some configurations, multi-leader replication models.
    - **Advantages**: Simplicity in replicating changes.
    - **Disadvantages**: Can lead to issues with non-deterministic functions or statements leading to inconsistent states.

- **Row-Based Replication**
    - **Mechanism**: Instead of logging the SQL statements, the actual changes to the rows are logged and replicated.
    - **Used By**: Often used in scenarios where statement-based replication might lead to inconsistencies due to non-deterministic statements.
    - **Advantages**: Avoids problems with non-deterministic functions and ensures that exact changes are replicated.
    - **Disadvantages**: Can result in larger amounts of data needing to be logged and replicated, especially for large transactions.

- **Logical Replication**
    - **Mechanism**: Changes are captured and replicated at a higher logical level rather than the physical level, allowing for more flexibility.
    - **Used By**: Multi-leader replication models or leaderless models, supporting heterogeneous database systems.
    - **Advantages**: Flexibility to replicate between different database versions or different database systems.
    - **Disadvantages**: Might require additional logic to handle schema or data type differences.

- **Conflict Resolution Mechanisms**
    - **Context**: Necessary in multi-leader and leaderless replication models to resolve discrepancies when the same data is modified in different locations.
    - **Mechanisms**: Include last write wins (LWW), vector clocks, or application-specific logic.
    - **Advantages**: Ensures data consistency across replicated nodes.
    - **Disadvantages**: Can add complexity to the replication system and might result in data loss or overwrite under certain conditions.