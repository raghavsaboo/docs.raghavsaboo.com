# Consensus Algorithms

Algorithms that achieve consensus and help solve atomic commits. 

Atomic commits are when we want distributed transactions that touch multiple partitions (needed in a partitioned relational database) to succeed or fail together. This is easy to do on a single node (like traditional RDBMS) but when transactions span multiple nodes connected via a network, they all need to agree on whether the transaction is committed or oborted.

Atomic commits are needed to prevent partitions from getting out of sync due to partially completed transactions:

- cross partition transactions
- global secondary indexes
- keeping other derived data consistent like data warehouses or cache

## Two-Phase Commit (2PC)
Two-Phase Commit is a classic consensus algorithm used to ensure all nodes agree on committing a transaction in a distributed database.

### Overview:
- **Coordinator-Participant Model:** One node acts as the coordinator, while others are participants.
- **Two Phases:**
  1. **Prepare Phase:** Coordinator asks participants if they are ready to commit.
  2. **Commit Phase:** If all participants agree, coordinator instructs them to commit.

### Advantages:
- Provides atomicity, ensuring all nodes either commit or abort a transaction.
- Guarantees consistency by coordinating transaction commits across distributed nodes.

### Limitations:
- Synchronous: Coordination requires blocking communication, leading to potential scalability issues.
- Blocking Failure: Coordinator failure during the commit phase can lead to a blocking state, requiring timeouts for recovery.

## Raft
Raft is a consensus algorithm designed for fault tolerance and ease of understanding, often used in distributed systems.

### Overview:
- **Leader-Based:** One node acts as the leader, coordinating replication of log entries to followers.
- **Leader Election:** Raft uses a leader election mechanism to ensure fault tolerance and continuity of operations.
- **Log Replication:** Leader replicates log entries to followers, ensuring consistency across the cluster.

### Advantages:
- Simplified Design: Raft's leader-based approach and clear separation of roles make it easier to understand and implement.
- Fault Tolerance: Raft ensures fault tolerance through leader election and log replication mechanisms.

### Limitations:
- Scalability: Large Raft clusters may experience performance issues due to leader bottleneck.
- Availability: Raft requires a majority of nodes to be available for progress, which can limit availability in some scenarios.

## Paxos
Paxos is a foundational consensus algorithm used for achieving agreement among distributed nodes, particularly in fault-tolerant systems.

### Overview:
- **Phase-Based Protocol:** Paxos operates in phases, including proposal, acceptance, and commitment.
- **Leaderless:** Paxos does not have a designated leader; any node can propose values.
- **Quorum-Based Decision:** Consensus is achieved when a quorum of nodes agrees on a value.

### Advantages:
- Fault Tolerance: Paxos ensures consistency and fault tolerance in distributed systems, even in the presence of failures.
- Decentralization: Paxos does not rely on a single leader, enabling distributed decision-making.

### Limitations:
- Complexity: Paxos can be challenging to understand and implement correctly due to its phase-based protocol.
- Scalability: Paxos may suffer from performance issues in large-scale deployments due to the need for quorum-based decision-making.


