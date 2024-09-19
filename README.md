![Architecture Diagram](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*43NgBoAW6h-vZTgyknM8xw.png)

![Eureka server](https://github.com/taha328/Spring-Cloud-Microservices-Architecture/blob/main/images/Eurekaserver.PNG?raw=true)
### Eureka Server Instance Overview

This screenshot shows the services currently registered with our Eureka Server, providing a clear view of service discovery in action. Each microservice is assigned a unique Docker container ID, indicating its running instance:

- **GATEWAY-SERVICE**: The entry point for routing and load balancing client requests, running on port 8089. The Docker container ID is `ac2ca7dcfeb4`.
- **ORDERSERVICE**: Handles order management functionalities, available on port 8083, with the Docker container ID `d94b9c6fa5d0`.
- **PRODUCT-SERVICE**: Manages product-related operations, running on port 8082. Its Docker container ID is `2691f9b880af`.
- **USER-SERVICE**: Manages user-related functionalities, available on port 8081, with the Docker container ID `57fffe6a430a`.

All services are currently in an "UP" state, ensuring that the microservices are actively registered and discoverable by other components in the system. The container IDs help in tracking and managing the services within the Docker environment.

For more detailed information on configuring and managing Eureka Server, you might find the [official Spring Cloud Netflix Eureka documentation](https://cloud.spring.io/spring-cloud-netflix/reference/html/#spring-cloud-eureka-server) helpful.

### Custom Route Configuration

In our Spring Cloud Gateway setup, we use the `customRouteLocator` method to define custom routing rules. This method is configured with `RouteLocatorBuilder` to route incoming requests to the appropriate microservices. Below is a summary of the routing configuration:

```java
@Bean
public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
    return builder.routes()
            .route("user_route", r -> r.path("/users/**")
                    .uri("lb://user-service"))
            .route("product_route", r -> r.path("/products/**")
                    .uri("lb://product-service"))
            .route("order_route", r -> r.path("/orders/**")
                    .uri("lb://orderservice"))
            .build();
}

#### Routing Rules

- **`user_route`**: Routes requests with paths starting with `/users/` to the `user-service` microservice. The `lb://` prefix indicates that the URI should be resolved using the load balancer.
- **`product_route`**: Routes requests with paths starting with `/products/` to the `product-service` microservice.
- **`order_route`**: Routes requests with paths starting with `/orders/` to the `orderservice` microservice.
