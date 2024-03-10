# Overview

Data intensive applications are one where:

1. the amount of data that is generated/uses increases quickly OR
2. the complexity of data generated/used increases quickly OR
3. the speed of change in data increases quickly

Distributed systems are a group of processes that run on different machines and communicate through a network to provide services and functionality. Such systems are often used to serve data intensive use cases.

## Key Requirements for Data Intensive Applications

=== "Reliability"

    - **Fault Tolerance** - a single node failure does not result in system failure
    - No un-authorized access
    - Chaos testing
    - Automating tests for bugs
    - Staging/Testing environment
    - Ability to quickly roll-back

=== "Scalability"

    - **Horizontal** - by adding more storage and compute nodes
    - **Vertically** - using more powerful machines
    - **Latency/Throughput Tradeoff** - Meeting traffic load with peak number of reads, writes and simultaneous users
    - Requires capacity planning with anticipated Latency and Throughput estimates
    - End user response is measured as the server response time + network response time
    - 90th, 95th, 99th Percentile Service Level Objectives (SLOs) / Service Level Agreements (SLAs) are established

=== "Maintainability"

    - Operable: Configurable and Testable
    - Simple: Easy to understand and ramp up
    - Evolveable: Easy to change

## Advantages of Distributed Systems

- **Scalability** - can horizontally scale by adding more storage and/or compute
- **Fault Tolerance through Replication** - a single node failure does not result in system failure - traffic can be rerouted to functioning nodes.
- **Performance** - have lower latency and higher throughput. Nodes of a system can be geographically spread out so that storage and compute is closer to the external clients/consumers.

## Disadvantages of Distributed Systems / Tradeoffs

- **Unreliable Network** - networks are inherently unreliable, and communication between different machines can be disrupted by network faults and partitions.
- **Need for data replication** - replicating data across multiple nodes over an unreliable network can result in lag and write conflicts
- **Loss of consistency** - different versions of the data are located at each node, and reconciling data differences between nodes allow non-deterministic behavior to arise.
- **Coordination Overhead** - additional components such as load balancers and replicas are needed to handle the distirbuted nature of the system. Strategies like leader election and failover need to be devised during outages.

## Distributed System Management

Coordinating operations and sharing data between the nodes of a distributed system can lead to complex behaviors like race conditions, deadlocks, and inconsistencies. Systems often use a Coordination Service (also called "Metadata Service" or "Configuration Service") to manage and synchronize a group of nodes (a **"cluster**) in a distributed environment.

Example: Apache Zookeeper

The responsibilities of this service are:

1. **Data Sync** - sync metadata, config, and other data between nodes with strong consistency. Nodes should know about other nodes in the system and can read and write to other nodes atomically without race conditions.
2. **Heartbeats** - periodic messages sent by the nodes to the service to indicate that they are operating normally. The service monitors nodes that fail to heartbeat and send alerts.
3. **Adding and removing nodes** - if a system has **autoscaling**, nodes are added and removed based on traffic. Information about the newly added or removed nodes needs to be propagated through the system.
4. **Failed nodes** - if a node fails, the service should remove it from the metadata and notify services that could be impacted.
5. **Leader election** - the service should have a mechanism for leader election. If a primary node fails, the remaining nodes should elect a new primary node using this service.
6. **Maintain Distributed Locks** - if nodes need to access shared resources in a mutually exclusive way, distributed locks are used to synchronize their access.
7. **Persist Metadata** - the cluster's metadata is a key-value mapping of node key to node data that can be persisted to a database.
