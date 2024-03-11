# Interview Notes

## Components to know about

- [Domain Name System (DNS)](./Domain%20Name%20System.md)
- [Proxies and Reverse Proxies](./Proxy%20and%20Reverse%20Proxy.md)
- [Load Balancers](./Load%20Balancing.md)
- [Relational and NoSQL Databases](./Databases.md)
- [Object Storage](./Object%20Storage.md)
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

## Key Topics

### System Requirements Analysis

- Understand the functional and non-functional requirements of the system.
- Discuss use cases, user expectations, and system constraints.

### System Architecture

- Design a high-level architecture that addresses the system's requirements.
- Discuss components, their responsibilities, and interactions.
- Consider microservices, service-oriented architecture (SOA), or serverless architecture based on the requirements.

### Scalability

- Discuss horizontal and vertical scaling strategies.
- Consider sharding, partitioning, and replication techniques.
- Discuss load balancing and auto-scaling mechanisms.

### Availability and Fault Tolerance

- Discuss redundancy, failover, and disaster recovery strategies.
- Consider techniques such as replication, data mirroring, and distributed consensus algorithms.
- Discuss how to handle network partitions and node failures.

### Consistency and Concurrency

- Discuss consistency models (e.g., eventual consistency, strong consistency) based on application requirements.
- Consider distributed locking, consensus algorithms (e.g., Paxos, Raft), and coordination services (e.g., ZooKeeper, etcd).
- Discuss isolation levels and transaction management.

### Data Storage and Retrieval

- Discuss database selection based on requirements (e.g., relational databases, NoSQL databases, distributed key-value stores).
- Consider data partitioning, indexing, and caching strategies.
- Discuss data replication, consistency, and durability guarantees.

### Messaging and Communication

- Discuss message queuing, publish-subscribe patterns, and event-driven architectures.
- Consider message brokers (e.g., Kafka, RabbitMQ) and communication protocols (e.g., HTTP, gRPC).

### Security

- Discuss authentication, authorization, encryption, and data privacy requirements.
- Consider network security, access control mechanisms, and secure communication protocols.

### Monitoring and Management

- Discuss logging, monitoring, and alerting requirements.
- Consider metrics collection, distributed tracing, and centralized logging solutions.
- Discuss deployment strategies and continuous integration/continuous deployment (CI/CD) pipelines.

### Cost Optimization

- Discuss resource utilization, cost-effective scaling strategies, and resource provisioning.
- Consider serverless computing, containerization, and cloud services pricing models.

## Writing Service Interfaces

Write the outline of the service interface to easily describe the service behavior to the interviewer, by including:

1. **message data structures** - describes the request and response messages that a service uses to communicate over a netwrk
2. **method signature** - contains a methods name, return type, and parameter list

```bash
GetCourseRequest
    int64 user_id
    int64 timestamp
    string course_name

GetCourseResponse
    int64 userid
    repeated string lessons
    int64 timestamp

# CourseService has a method getCourse which accepts a request message GetCourseRequest and returns the response message GetCourseResponse

# It accepts a single argument, the request, and returns a single object, the response

CourseService
    GetCourseResponse getCourse (GetCourseRequest request)

```

==Reminder==: the **message data structure** is different from the **data model** - e.g. song_names could be stored as rows that are aggregated into a sequential datastructure like an array in the course response.

A system would have multiple services, and each service would have multiple methods. Making them descriptive makes the interview self-explanatory and self-documenting.
