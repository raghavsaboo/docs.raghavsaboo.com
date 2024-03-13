# Consistency

Consistency is an overloaded term with multiple uses.

## Flavors of Consistency

1. CAP Consistency/Strong Consistency/Linearizability/Strict Consistency/Immediate Consistency
    - Means that simultaneous reads at different nodes in a distributed system always return the same data. This holds even if there is a simultaneous write at one of the nodes.
    - Example:
      - Updating passwords

2. Weak Consistency
    - Means that simultaneous reads at different nodes could yield different values. Nodes can diverge in state, and there is no guarantee that they will converge - which is different from eventual consistency!

3. Eventual Consistency
    - Type of weak consistency where if there are no new writes, data at all nodes **will** eventually converge. Updates and writes arepropogated throughout the nodes of a system, but those changes will not be immediately reflected. This is associated with BASE databases.
    - Examples: 
        - The **Domain Name System (DNS)** is highly available with high throughput, but may not reflect the latest value at any given time.

4. Sequential Consistency
    - Means that the ordering of write requests at a single node is preserved in a distributed system. Two sequential writes cannot be applied out of order at another node in the system.
    - Example:
      - Ordering of scial media posts of friends. We expect that for a given friend the comments or news posts are displayed in the order they were submitted.

5. ACID Consistency
    - Means that data stored by a database is in a correct and valid state - e.g. if a schema specifies that a value must be unique, the database will ensure that the value is unique throughout all actions. If a transaction violates data integrity constraints, that transaction is rejected, and the state is reverted. This is associated with ACID databases.
    - Example: 
        - if a foreign key is deleted, associated rows in other tables will also be deleted. 

## Tradeoffs

A common decision to make is the choice between **strong** and **weak** consistency - e.g. a database with BASE characteristics, transactions have eventual consistency, a form of weak consistency.

This is a choice because achieving consistency in a distributed system has additional overhead and costs e.g. synchronization between nodes, verification mechanism to verify data is most up to date,
