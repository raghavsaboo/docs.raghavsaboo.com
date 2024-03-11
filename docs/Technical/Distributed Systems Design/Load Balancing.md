# Load Balancing

A load balancer is a device or software component that distributes incoming network traffic across multiple servers or resources. It acts as a reverse proxy, intercepting client requests and forwarding them to the most suitable server based on predefined algorithms or rules. Load balancers are commonly used to improve the performance, availability, and reliability of applications and services.

**Impact on Availability and Reliability**:

- **Availability**: Load balancers enhance availability by distributing traffic across multiple servers, preventing any single server from becoming overwhelmed. If one server fails or becomes unreachable, the load balancer redirects traffic to healthy servers, ensuring continuous service availability.
- **Reliability**: Load balancers contribute to reliability by minimizing the risk of service downtime due to server failures or maintenance. By distributing traffic evenly and efficiently, load balancers help maintain consistent performance and responsiveness, even under high loads or adverse conditions.

In distributed systems, load balancers play a crucial role in achieving scalability, fault tolerance, and performance optimization. They are often deployed at the entry point of a distributed application or service, acting as the [**reverse proxy**](./Proxy%20and%20Reverse%20Proxy.md#Reverse%20Proxy) and to evenly distribute incoming requests among multiple instances or nodes. Load balancers can route traffic based on various factors such as server health, geographic location, or request type, ensuring efficient resource utilization and effective load distribution across the system.

## Placement of Load Balancers

![source: https://learn.microsoft.com/en-us/azure/load-balancer/media/load-balancer-overview/load-balancer.png](https://learn.microsoft.com/en-us/azure/load-balancer/media/load-balancer-overview/load-balancer.png)

In distributed systems, load balancers are typically placed at strategic points within the network to optimize traffic distribution and improve system performance. Some common placements for load balancers include:

**Frontend of the Application**:

- Load balancers positioned at the entry point of the distributed application.
- Serve as the first point of contact for incoming client requests.
- Distribute traffic among multiple backend servers or instances before reaching application logic.

**Between Clients and Web Servers**:

- Load balancers act as reverse proxies intercepting client requests.
- Positioned between clients (e.g., web browsers) and web servers hosting the application.
- Provide additional features such as SSL termination, caching, and request routing.

**Between Web Servers and Application Servers**:

- Load balancers distribute traffic from the web layer to the application layer.
- Common in traditional multi-tier architectures where web servers handle incoming requests and application servers process application logic.
- Ensure efficient utilization of resources and optimal performance.

**Within Microservices Architecture**:

- Load balancers used to distribute traffic between different microservices or service instances.
- Enable dynamic routing of requests based on load, health, or specific routing rules.

## Load Balancer Types

**Layer 4 Load Balancer / Session Level Load Balancing**:

- Operates at the transport layer (Layer 4) of the OSI model.
- Directs traffic based on information available in network and transport layer protocols (e.g., IP addresses, TCP/UDP ports).
- Typically faster and more efficient than Layer 7 load balancers because they don't inspect application layer data.
- Suitable for scenarios where routing decisions can be made solely based on network and transport layer information.
- Examples include HAProxy and NGINX when configured as a Layer 4 load balancer.

**Layer 7 Load Balancer**:

- Operates at the application layer (Layer 7) of the OSI model.
- Makes routing decisions based on application layer data, such as HTTP headers, URLs, cookies, or content.
- Offers more advanced routing and traffic management capabilities, including content-based routing, session persistence, and SSL termination.
- Provides greater flexibility and control over traffic routing but may introduce higher latency and overhead due to application layer inspection.
- Ideal for scenarios that require sophisticated routing logic or application-specific traffic management.
- Examples include F5 BIG-IP, AWS Application Load Balancer (ALB), and NGINX when configured as a Layer 7 load balancer.

## Load Balancing Algorithms

- **Round Robin**: Distributes incoming requests evenly across a pool of backend servers in a circular manner.

- **Least Connections**: Routes each new request to the backend server with the fewest active connections at the time.

- **IP Hash**: Uses a hash function to map the client's IP address to a specific backend server, ensuring session persistence.

- **Weighted Round Robin**: Assigns a weight to each backend server, determining the proportion of requests it receives relative to other servers.

- **Least Response Time**: Routes requests to the backend server with the fastest response time, based on historical performance metrics.

- **Least Bandwidth**: Directs requests to the backend server with the lowest current bandwidth usage, aiming to optimize resource utilization.

- **Random Selection**: Selects a backend server randomly from the pool without considering server load or capacity.

- **Geographical Load Balancing**: Directs clients to the nearest or most optimal server based on their geographic location, aiming to minimize latency and improve performance.
