# 🏥 Doctor Appointment System (Microservices)

This project demonstrates a microservices architecture built with Spring
Boot, Spring Cloud, Eureka, JWT Authentication, and MySQL.
Each microservice is developed and maintained in its own repository.

------------------------------------------------------------------------

## 📌 Microservices

-   User Service
    Handles user registration, login, authentication (JWT), password
    reset via email, and profile management.

-   Appointment Service
    Manages patients and doctor appointments, doctor schedules, and booking functionality.

-   Eureka Server
    Service discovery for microservices (Spring Cloud Netflix Eureka).
-   Gateway service
    entry point for application.
    
------------------------------------------------------------------------

## 🏗️ Architecture Overview

    flowchart LR
        A[User Service] -->|Registers & Authenticates| B[Eureka Server]
        C[Appointment Service] -->|Registers & Discovers| B
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
-   Service Discovery  & palance loading with Eureka Server.
-   Single entry point usnig API Gateway

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
        git clone https://github.com/mahmoud-Ramadan2/appointment-service.git
        git clone https://github.com/mahmoud-Ramadan2/eureka-server.git
        git clone https://github.com/mahmoud-Ramadan2/gateway-service.git


3.  Start Eureka Server first.

4.  Run User Service, Appointment Service and /Gateway-Service (they will register
    with Eureka).

5.  Access services via REST APIs or integrate with a frontend.

------------------------------------------------------------------------

🔹 Services & Routes

All requests go through the API Gateway (http://localhost:8080) and are routed to the appropriate microservice.

| Service                 | Base Path (via Gateway)             | Example Endpoints                                                                                                                                                                                                                                       |
|-------------------------|-------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **User Service**        | `/users/**`                         | - `POST /users/auth/register` → Register a new user <br> - `POST /users/auth/login` → User login (JWT token) <br> - `POST /users/auth/forgot-password` → Send reset pass link mail <br> - `POST /users/auth/reset-password` → reset new pass            |
| **Appointment Service** | `/appointments/**`                  | - `POST /appointments` → Book a new appointment <br> - `GET /appointments/doctor/{id}` → List doctor's appointment by ID <br> - `GET /appointments/patient/{id}` → List patient's appointment by ID <br> - `GET /appointments/` → List all appointments |
| **(Future) Services**   | `/payments/**`, `/notifications/**` | To be implemented                                                                                                                                                                                                                                       |

------------------------------------------------------------------------

## 📖 Notes

-   This is a demo microservices project for learning & practice.
-   Each microservice is fully independent with its own pom.xml.
-   Future enhancements: API Gateway (Spring Cloud Gateway), centralized
    config server, Docker Compose for containerized deployment.
