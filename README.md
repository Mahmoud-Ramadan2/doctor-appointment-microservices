# 🏥 Doctor Appointment System (Microservices)

This project demonstrates a microservices architecture built with Spring
Boot, Spring Cloud, Eureka, JWT Authentication, and MySQL.
Each microservice is developed and maintained in its own repository.

------------------------------------------------------------------------

## 📌 Microservices

-   User Service
    Handles user registration, login, authentication (JWT), password
    reset via email, and profile management.

-   Doctor Appointment Service
    Manages appointments, doctor schedules, and booking functionality.

-   Eureka Server
    Service discovery for microservices (Spring Cloud Netflix Eureka).

------------------------------------------------------------------------

## 🏗️ Architecture Overview

    flowchart LR
        A[User Service] -->|Registers & Authenticates| B[Eureka Server]
        C[Doctor Appointment Service] -->|Registers & Discovers| B
        A -->|Calls| C
        subgraph Database Layer
            D[MySQL]
            E[MySQL]
        end
        A --> D
        C --> E

------------------------------------------------------------------------

## ✨ Features

-   Environment Configuration: .env file auto-loaded at startup.
-   JWT Authentication & Authorization.
-   Forgot & Reset Password via email integration.
-   Global Exception Handler with structured error responses.
-   Unit Testing with JUnit & Mockito.
-   API Documentation using Spring REST Docs.
-   Service Discovery with Eureka Server.

------------------------------------------------------------------------

## 🚀 Technologies Used

-   Java 21 + Spring Boot 3
-   Spring Cloud Netflix Eureka
-   JWT Authentication
-   MySQL
-   JUnit 5 & Mockito for testing
-   Spring REST Docs for API documentation
-   Docker (optional)

------------------------------------------------------------------------

## 🧪 Testing & Documentation

-   Unit tests written with JUnit 5 and Mockito.
-   Automated API documentation generated with Spring REST Docs.
-   Ensures reliable, well-tested, and documented microservices.

------------------------------------------------------------------------

## 📜 Logging

- Uses **SLF4J Logger** with **Logback** configuration.  
- **Profile-based logging**:
  - **Dev** → Console + rolling file logs, includes Hibernate SQL and parameter bindings.  
  - **Prod** → Rolling file logs with `INFO` level and log rotation.  
- Log files are stored under `logs/doctor-app-logs.log`for each service with rotation (`10MB`, 30 days, 100MB cap).  


## 🎯 How to Run

1.  Clone each repository:

        git clone https://github.com/mahmoud-Ramadan2/user-service.git
        git clone https://github.com/mahmoud-Ramadan2/doctor-appointment-system.git
        git clone https://github.com/mahmoud-Ramadan2/eureka-server.git

2.  Start Eureka Server first.

3.  Run User Service and Doctor Appointment Service (they will register
    with Eureka).

4.  Access services via REST APIs or integrate with a frontend.

------------------------------------------------------------------------

## 📖 Notes

-   This is a demo microservices project for learning & practice.
-   Each microservice is fully independent with its own pom.xml.
-   Future enhancements: API Gateway (Spring Cloud Gateway), centralized
    config server, Docker Compose for containerized deployment.
