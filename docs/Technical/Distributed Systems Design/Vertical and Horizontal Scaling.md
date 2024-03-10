# Vertical and Horizontal Scaling

## Vertical Scaling

## Horizontal Scaling

### Databases

#### Partitioning / Sharding

Horizontally scaling and scalability (data, users, and machines) is the key theme of partitioning.

Partitioning == Splitting == Sharding

##### Key Issues to Look Out For

- Hot spots / Skews
- Key Hash Based Partition
- Rebalancing strategies
- Routing Logic Placement

#### Replication

The process of syncing data between databases in a consistent way is refered to as
"replication".

This is needed due to a few reasons:

- Resilience of service to Machine Failures
- Improving latency for global audience
- Scaling to millions of users
- Offline/Network failures

## Types of Replication

### 1. Single Leader

### 2. Multi-Leader

### 3. Leaderless