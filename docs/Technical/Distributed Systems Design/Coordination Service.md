# Coordination Services

Coordinating operations and sharing data between the nodes of a distributed system can lead to complex behaviors like race conditions, deadlocks, and inconsistencies. Systems often use a Coordination Service (also called "Metadata Service" or "Configuration Service") to manage and synchronize a group of nodes (a **"cluster**) in a distributed environment.

This is unlike gossip protocols which pass information directly from node to node. Instead a centralized, replicated key value store built on top of consensus algorithms is used.

Example: Apache Zookeeper

The responsibilities of this service are:

1. **Data Sync** - sync metadata, config, and other data between nodes with strong consistency. Nodes should know about other nodes in the system and can read and write to other nodes atomically without race conditions.
2. **Heartbeats** - periodic messages sent by the nodes to the service to indicate that they are operating normally. The service monitors nodes that fail to heartbeat and send alerts.
3. **Adding and removing nodes** - if a system has **autoscaling**, nodes are added and removed based on traffic. Information about the newly added or removed nodes needs to be propagated through the system.
4. **Failed nodes** - if a node fails, the service should remove it from the metadata and notify services that could be impacted.
5. **Leader election** - the service should have a mechanism for leader election. If a primary node fails, the remaining nodes should elect a new primary node using this service.
6. **Maintain Distributed Locks** - if nodes need to access shared resources in a mutually exclusive way, distributed locks are used to synchronize their access.
7. **Persist Metadata** - the cluster's metadata is a key-value mapping of node key to node data that can be persisted to a database.
