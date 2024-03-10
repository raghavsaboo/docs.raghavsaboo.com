# Overview

Data intensive applications are one where:

1. the amount of data that is generated/uses increases quickly OR
2. the complexity of data generated/used increases quickly OR
3. the speed of change in data increases quickly

## Key Components of Modern Data Intensive Applications

1. **Database** - source of truth for any consumer.
2. **Cache** - for temporarily storing an expensive operation to speed up reads
3. **Full-text Reverse Index** - for quickly searching data by keyword or filter
4. **Message Queues** - for message passing between processes
5. **Stream Processing** - near/realtime processing of data
6. **Batch Processing** - crunching large amounts of collected data
7. **Application Code** - logic and connective tissue between the components above
8. **Load Balancer**
9. **CDN**

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

## Key Challenges

1. **Network Reliability** - networks are inherently unreliable, and communication between machines is not guaranteed
2. **Data Replication** - replicating data across multiple nodes over a network can result in lag and write conflicts
3. **Consistency** - data is spread across multiple machines, and there can be different versions of the data

