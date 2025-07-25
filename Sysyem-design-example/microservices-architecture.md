### Ever wondered how Flipkart handles millions of orders during a sale—without crashing?

The secret lies in smart Load Balancers and intelligent API Gateways that route, protect, and scale microservices like pros.
If you're building or scaling microservices, this is the traffic control strategy you can’t afford to ignore.

This document explores the use of load balancers and API gateways in a microservice architecture. It addresses the challenges of horizontal scaling, where server IPs change dynamically, and explains how load balancers distribute traffic evenly across healthy servers. Furthermore, it delves into the role of API gateways in routing requests to the appropriate microservice based on the URL or domain, enhancing the overall efficiency and manageability of the system.

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0336eslwm3lntyd67sqe.png)


#### Load Balancing in Horizontal Scaling

In horizontal scaling, we increase the capacity of a system by adding more servers to the pool. However, this introduces a challenge: the IP addresses of these servers can change dynamically. Traditional DNS resolution might point to only one IP address, leading to uneven distribution of traffic and potential bottlenecks.

To address this, we employ a **load balancer**. The load balancer sits in front of the servers and acts as a single point of contact for incoming requests. It then distributes these requests across the available servers based on various load balancing algorithms.

##### Load Balancing Techniques

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pnooewafl5f0ahdfk6f6.png)

- **Round Robin**: Distributes requests sequentially to each server in the pool.
- **Least Connections**: Sends requests to the server with the fewest active connections.
- **Weighted Round Robin**: Assigns weights to servers based on their capacity, distributing requests accordingly.
- **IP Hash**: Uses the client's IP address to determine which server to send the request to, ensuring that requests from the same client are consistently routed to the same server.

The load balancer also monitors the health of the servers. If a server becomes unhealthy, the load balancer automatically removes it from the pool, preventing traffic from being directed to it. Once the server recovers, the load balancer adds it back to the pool.

#### Load Balancing in a Microservice Architecture

In a microservice architecture, applications are structured as a collection of loosely coupled, independently deployable services. Each service is responsible for a specific business function. For example, consider a large e-commerce platform like Flipkart during a sale:

- **Orders Service**: Handles order placement, tracking, and management.
- **Payments Service**: Processes payments and manages transactions.
- **API Servers**: Provide a public interface for accessing the platform's functionality.
- **Authentication Servers**: Authenticate users and manage their sessions.

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/cjhrix268hzqvtopxpcy.png)

Each of these services runs on its own set of servers and can be scaled independently based on its specific needs. To ensure high availability and even distribution of traffic, we place a load balancer in front of each service.

##### This setup offers several advantages:

- **Scalability**: Each service can be scaled independently to handle varying levels of traffic.
- **Resilience**: If one server fails, the load balancer automatically redirects traffic to the remaining healthy servers.
- **Isolation**: Each service is isolated from the others, preventing failures in one service from affecting the others.
- **Flexibility**: Different services can use different technologies and be deployed independently.

#### API Gateway

In addition to load balancing individual services, we often use an **API gateway** to manage external access to the microservices. The API gateway acts as a single entry point for all client requests and provides several key functionalities:

- **Routing**: Directs requests to the appropriate microservice based on the URL or domain.
- **Authentication and Authorization**: Verifies the identity of the client and ensures that they have the necessary permissions to access the requested resource.
- **Rate Limiting**: Prevents abuse of the API by limiting the number of requests that a client can make within a given time period.
- **Request Transformation**: Modifies the request before sending it to the microservice, for example, by adding headers or transforming the data format.
- **Response Aggregation**: Combines responses from multiple microservices into a single response for the client.

##### Host-Based Routing

Host-based routing directs requests based on the domain name used in the request. For example:

- `orders.example.com` might be routed to the Orders service.
- `payments.example.com` might be routed to the Payments service.

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/eqfuisgn9cymftzdg44x.png)


##### Path-Based Routing

Path-based routing directs requests based on the URL path used in the request. For example:

- `example.com/orders` might be routed to the Orders service.
- `example.com/payments` might be routed to the Payments service.

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/q16lzf210nlcupm76r8x.png)

By using an API gateway, we can simplify the client's interaction with the microservices and provide a consistent and secure interface. It also allows us to evolve the microservices independently without affecting the client.

#### Conclusion

Load balancers and API gateways are essential components of a well-designed microservice architecture. Load balancers ensure high availability and even distribution of traffic across multiple server instances of each service, while API gateways provide a single entry point for all client requests and handle routing, authentication, and other cross-cutting concerns. Together, they enable us to build scalable, resilient, and maintainable microservice applications.

---

✍️ **Author**: *Omkar Sharma*  
📬 *Feel free to connect on [LinkedIn](https://www.linkedin.com/in/omkarsharmaa/) or explore more on [GitHub](https://github.com/omkarsharma2821)* 
