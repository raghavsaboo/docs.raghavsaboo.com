# Consistency

Consistency is an overloaded term with multiple uses.

## Flavors of Consistency

`CAP Consistency/Strong Consistency/Strict Consistency/Immediate Consistency`
:   means that simultaneous reads at different nodes in a distributed system always return the same data. This holds even if there is a simultaneous write at one of the nodes.

`Weak Consistency`
:   means that simultaneous reads at different nodes could yield different values. Nodes can diverge in state, and there is no guarantee that they will converge.

`Eventual Consistency`
:   type of weak consistency where if there are no new writes, data at all nodes will eventually converge. Updates and writes are propogated throughout the nodes of a system, but those changes will not be immediately reflected. This is associated with BASE databases.

`Sequential Consistency`
:   means that the ordering of write requests at a single node is preserved in a distributed system. Two sequential writes cannot be applied out of order at another node in the system.

`ACID Consistency`
:   means that data stored by a database is in a correct and valid state. If a transaction violates data integrity constraints, that transaction is rejected, and the state is reverted. This is associated with ACID databases.

## Tradeoffs

A common decision to make is the choice between **strong** and **weak** consistency - e.g. a database with BASE characteristics, transactions have eventual consistency, a form of weak consistency.

This is a choice because achieving consistency in a distributed system has additional overhead and costs e.g. synchronization between nodes, verification mechanism to verify data is most up to date,
