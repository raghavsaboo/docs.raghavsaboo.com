# Web Server

We distinguish between **frontend server**, that is part of the first layer of servers that a request reachers, and **backend-server**, that is not part of the first layer of servers and often contains services and controls resource access.

**Frontend servers** are designed to be external facing, and handle unregulated and unfiltered requests to the system. **Backend servers** are designed to run services, perform computations, and access databases.

![source: https://docs.oracle.com/cd/E19566-01/819-4654/images/csagCLDPlugin2.gif](https://docs.oracle.com/cd/E19566-01/819-4654/images/csagCLDPlugin2.gif)

A **web server** is a **frontend server** that is stateless and responds to network requests over HTTP and other web protocols. It is commonly used to process and deliver webpages to clients such as web browsers. Its job is to sit between the external clients and the system's services. Its roles may include:

1. **Receiving Requests** - Listens on a port for network requests (e.g., HTTP) to the web server.
2. **Request Routing** - Directs incoming requests to appropriate endpoints or services based on predefined rules.
3. **Request Deduplication** - Eliminates duplicate requests to prevent unnecessary processing and improve efficiency.
4. **Session Management** - Tracks and maintains user sessions to enable stateful interactions with the server.
5. **Request Validation** - Validates incoming requests to ensure they meet predefined criteria for security and integrity.
6. **Authentication** - Verifies the identity of users or clients attempting to access the server or its resources.
7. **Authorization** - Determines whether authenticated users have permission to access requested resources.
8. **Malicious Traffic Detection** - Identifies and blocks or mitigates malicious requests or traffic aimed at compromising server security.
9. **Throttling** - Limits the rate of incoming requests from clients to prevent overloading the server and ensure fair resource allocation.
10. **Load Shedding** - Discards or delays requests when the server is under heavy load to prioritize essential operations and maintain stability.
11. **Response Caching** - Stores frequently accessed responses to reduce server load and improve performance by serving cached content.
12. **TLS/SSL Termination** - Decrypts incoming HTTPS requests, allowing the server to process them in plain text.
13. **TLS/SSL Encryption** - Encrypts outgoing responses before transmission to ensure data confidentiality and integrity over the network.
14. **Server-side Encryption** - Encrypts data stored on the server to protect it from unauthorized access or tampering.
15. **Usage Analytics and Data Collection** - Gathers and analyzes data on server usage, user interactions, and performance metrics for monitoring and optimization purposes.

## API Gateway

Web Servers can often also be used as or be called **API Gateways**, which provides a single point of access to multiple services. This provides an external facing interface that masks the structure of the services in the system, providing better readability to external clients. Also backend services can be refactored or redesigned without impacting external client interfaces.

The advantages of an API Gateway are listed below:
- **Centralized Access Point**: Provides a single entry point for all client requests, simplifying management and monitoring.
- **Security Enforcement**: Implements security measures like authentication, authorization, and encryption to protect backend services.
- **Traffic Management**: Offers features like rate limiting, throttling, and load balancing to ensure optimal performance and resource utilization.
- **Protocol Translation**: Translates incoming requests from various protocols to a standard format, facilitating communication between clients and backend services.
- **Analytics and Monitoring**: Collects data on API usage, performance metrics, and errors for monitoring, troubleshooting, and optimization.
- **Service Composition**: Allows for combining multiple microservices into a unified API, abstracting complexity for clients.
- **Versioning and Lifecycle Management**: Supports versioning of APIs and manages their lifecycle, enabling smooth updates and deprecations.
- **Caching and Response Optimization**: Implements caching strategies to improve latency and reduce load on backend services by serving cached responses.
- **Cross-Origin Resource Sharing (CORS)**: Facilitates CORS policies to manage access control for web applications accessing APIs from different origins.
- **Scalability and Flexibility**: Scales horizontally to handle growing traffic and can be easily configured to adapt to changing business requirements.
