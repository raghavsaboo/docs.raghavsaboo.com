# PACELC Theorem

This theorem expands the [CAP Theorem](./CAP%20Theorem.md): during a `Network Partition (P)`, a distributed system chooses between `Availability (A)` and `Consistency (C)`. `Else (E)`, where there is no network partition, a distributed system chooses between either `Latency (L)` or `Consistency (C)`.

Therefore an additional tradeoff is introduced when a distributed system is running normally, without network partitions, an operation can either have

- low latency but possibly inconsistent data
- consistent data by syncing between nodes at the cost of latency

![source: https://www.scylladb.com/wp-content/uploads/PACELC-theorem-diagram-1.jpg](https://www.scylladb.com/wp-content/uploads/PACELC-theorem-diagram-1.jpg)

Databases with AP characteristics are usually also EL systems and are known as `AP/EL` systems.

Databases with CP characteristics are usually also EC systems and are known as CP/EC systems. These typically support ACID transactions and prioritize consistency during network partition.
