# Microservices Configuration Repository

This repository stores the configuration files for a distributed microservices application built with Spring Boot and Spring Cloud. The following sections provide an overview of the project's architecture, the role of each service, and instructions for using these configurations.

## Architecture

This project follows a classic microservices pattern, with several key components working together:

- **Config Server**: Centralizes configuration management for all other services. It uses a Git repository as a backend to store configuration files.
- **Discovery Server (Eureka)**: Allows services to find and communicate with each other without hardcoding hostnames and ports. All services register themselves with the discovery server.
- **Edge Server (API Gateway)**: The single entry point for all client requests. It routes requests to the appropriate backend service and can also handle concerns like authentication, SSL termination, and rate limiting.
- **Application Services**: These are the core business logic services, including `product-service`, `recommendation-service`, and `review-service`.
- **Composite Service**: The `product-composite-service` aggregates calls to the underlying application services to provide a unified API.
- **Authorization Server**: Manages user authentication and authorization.

All services are configured to send tracing data to a **Zipkin** server for distributed tracing and monitoring.

## Services

This section provides a detailed breakdown of each service and its configuration.

### Global Configuration (`application.properties`)

This file contains properties that are shared across all services:

- `spring.zipkin.base-url`: Sets the address of the Zipkin server for distributed tracing.
- `management.tracing.sampling.probability`: Configures the percentage of traces to be sent to Zipkin (set to `1.0` for 100%).
- `management.endpoints.web.exposure.include=*`: Exposes all Spring Boot Actuator endpoints.

### Config Server (`config-server-production.properties`)

The Config Server provides centralized configuration management.

- `server.port`: The port on which the server runs (production: `9888`).
- `spring.cloud.config.server.git.uri`: The URL of the Git repository that stores the configuration files.

### Discovery Server (`discovery-server-production.properties`)

The Discovery Server (Eureka) allows services to find each other on the network.

- `server.port`: The port for the discovery server (production: `9761`).
- `eureka.client.register-with-eureka=false`: Prevents the server from registering itself.
- `eureka.client.fetch-registry=false`: Prevents the server from fetching the registry from another Eureka server.

### Edge Server (`edge-server.properties` & `edge-server-production.properties`)

The Edge Server (API Gateway) is the single entry point for all client requests.

- `server.port`: The port for the gateway (non-production: `8000`, production: `9000`).
- `spring.cloud.gateway.server.webflux.routes`: Defines the routing rules for incoming requests, mapping them to the appropriate backend services.

### Product Composite Service (`product-composite-service.properties` & `product-composite-service-production.properties`)

This service aggregates data from the `product`, `recommendation`, and `review` services.

- `server.port`: The port for the service (non-production: `8070`, production: `9070`).
- `resilience4j.circuitbreaker.instances.*`: Configures the Resilience4j circuit breaker to handle failures when communicating with other services.

## Usage

To use these configuration files, you will need to have a Spring Boot application for each service (`config-server`, `discovery-server`, etc.). The name of the property file should match the `spring.application.name` property in your `bootstrap.yml` or `application.yml` file.

For example, to run the `product-composite-service` in a non-production environment, you would use the `product-composite-service.properties` file. For a production environment, you would use `product-composite-service-production.properties` by activating the `production` Spring profile.

### Setup

1. **Clone the repository**:
   ```bash
   git clone https://github.com/THUNDER1298/tp-microservice-config.git
   ```
2. **Run the Config Server**: Start the `config-server` and point it to this Git repository.
3. **Run the Discovery Server**: Start the `discovery-server`.
4. **Run the other services**: Start the remaining services. They will fetch their configuration from the Config Server and register with the Discovery Server.
