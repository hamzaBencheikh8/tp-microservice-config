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

*Coming soon: A breakdown of each service and its configuration.*

## Usage

*Coming soon: Instructions on how to use these configuration files to run the microservices application.*
