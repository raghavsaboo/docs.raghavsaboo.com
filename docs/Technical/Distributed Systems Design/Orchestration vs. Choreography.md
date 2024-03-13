# Orchestration vs. Choreography

There are two styles of communication between services to run workflows (e.g. sending an email confirmation of purchase to a user, notifying the seller, and assign for delivery):

1. **Synchronous** - request/response pattern where the client initiates a request and waits for a response
2. **Asynchronous** - event based pattern where a service emits an event that other services react to

Synchronous request/reponse is associated with **orchstration**, where one service acts as the orchestator and handles communication between services. For example when services are invoked in a serial order with blocking calls. Such systems can become a distributed monolith with a single point of failure (the service orchestrator/controller). e.g. order service sends requests to the email service, notification service, and delivery service.

Asynchronous event based patter is associated with **choreography**, where an **event stream** or **message queue** is used to hold events, and each service is a consumer and/or producer of events. Multiple services can process the same events simultaneously. This enables greater system throughput because services can execute requests async and in parallel. 

Both orchestration and choreography are useful for solving different problems, and can be used together in a **hybrid architecture**.

![source: https://solace.com/wp-content/uploads/2019/11/Orchestration-VS-Choreography-1200x600.png](https://solace.com/wp-content/uploads/2019/11/Orchestration-VS-Choreography-1200x600.png)

## Benefits of Orchestration

1. Reliability - has built-in transaction management and error handling, while choreography is point-to-point communications and the fault tolerance scenarios are much more complicated
2. Scalability - when adding a new service, only the orchestrator needs to modify the interaction rules

## Limits of Orchestration

1. Performance - all services talk via a centralized orchestrator, so latecy is higher than it is with choreography. The throughput is bound to the capacity of the orchestrator.
2. Single poit of failure - if the orchestrator goes down, no services can talk to each other. Needs to be highly avialable.