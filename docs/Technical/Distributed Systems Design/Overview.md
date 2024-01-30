# Overview

Data intensive applications are one where:

1. the amount of data that is generated/uses increases quickly OR
2. the complexity of data generated/used increases quickly OR
3. the speed of change in data increases quickly

## Key Components of Modern Data Intensive Applications

1. **Database** - source of truth for any consumer.
2. **Cache** - for temporarily storing an expensive operation to speed up reads
3. **Full-text index** - for quickly searching data by keyword or filter
4. **Message queues** - for message passing between processes
5. **Stream processing** - near/realtime processing of data
6. **Batch processing** - crunching large amounts of collected data
7. **Application code** - logic and connective tissue between the components above

## Key Requirements for Data Intensive Applications

### Reliability

* Fault tolerance
* No un-authorized access
* Chaos testing
* Full machine failures
* Bugs - Automating tests
* Staging/Testing environment
* Quickly roll-back

### Scalability

* Handle higher traffic volume
* Meeting traffic load with peak number of reads, writes and simultaneous users
* Capacity planning
* Response time vs throughput
* End user response = server response time + network response time
* 90th, 95th Percentile Service Level Objectives (SLOs) / Service Level Agreements (SLAs)
* Scaling
  * up (more powerful machine)
  * out (distributed over many smaller machines)

### Maintainability

* Add new people to work
* Productivity
* Operable: Configurable and testable
* Simple: Easy to understand and ramp up
* Evolveable: Easy to change