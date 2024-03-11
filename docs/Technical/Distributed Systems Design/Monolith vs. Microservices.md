# Monolith vs. Microservices

![source: https://www.suse.com/c/wp-content/uploads/2021/09/rancher_blog_microservices-and-monolithic-architectures.jpg](https://www.suse.com/c/wp-content/uploads/2021/09/rancher_blog_microservices-and-monolithic-architectures.jpg)

## Monolith

Software that is built and deployed as a single unit - usually consisting of a user interface, a service-side application, and a database.

Here different parts of the system are combined and launched as a single component.

### Advantages

1. Easy development and fast deployment
2. Limited number of components need integration or communication, simplifying testing and monitoring
3. Performant and low latency - requests are served from a singe unit, which means the request path contains few network hops and components

### Disadvantages

1. All modules are scaled together, and resource allocation might be inefficient
2. If one part of the monolith fails, the entire monolith can fail
3. Often have [tightly coupled](./Coupling%20and%20Cohesion.md) components that make it difficult to incorporate new technology and other components
4. Deployment becomes difficult at large scale, with multiple teams

## Microservice

Software that is built and deployed as a collection of independent services.

### Advantages

1. **Granular Scalability** - independent services mean each service can be scaled independently instead of being scaled in conjunction with other services - reduces bottlenecks and allows for more efficient resource usage and cost savings
2. **Reusability** - microservices can be used across different applications and systems in the same business unit or enterprise
3. **Reliability** - microservices are more robust due to **failure isolation**, where the failure is isolated to a single service, node, or region. This failure does not result in system-wide outages. Traffic redirection can also happen.
4. **Technology agnostic** - allows for use of different tech stacks and frameworks. The communication protocols are decoupled from the language and implementation of the service.

### Disadvantages

1. **Increased Complexity** -- independent services need to communicate and coordinate over a network - ppotentially greater latency, and code/comm protocol overhead
2. **Difficult testing and debugging** - end to end testing requires launching all services an testing interactions between them. Logging and debugging would also be spread around multiple files and executibles, making it difficult to understand interaction and integration behaviour.
3. **Increased design and deployment overhead** - more services mean more design and deployment work, which is challenging for smaller teams who need to iterate quickly with low overhead.
