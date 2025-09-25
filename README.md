# 🏥 Doctor Appointment System (Microservices)

This project demonstrates a **microservices architecture** built with **Spring Boot**, **Spring Cloud**, **Eureka**, **JWT Authentication**, **MySQL**, and **Kafka**.  
Each microservice is developed and maintained in its own repository.

------------------------------------------------------------------------

## 📌 Microservices

- **User Service**  
  Handles user registration, login, authentication (JWT), password reset via email, and profile management.

- **Appointment Service**  
  Manages patients and doctor appointments, doctor schedules, and booking functionality.

- **Notification Service**  
  Listens to Kafka events (e.g., new appointment created) and sends email notifications to users.

- **Eureka Server**  
  Provides service discovery for microservices (Spring Cloud Netflix Eureka).

- **API Gateway**  
  Entry point for routing requests to backend microservices.

    
------------------------------------------------------------------------

## 🏗️ Architecture Overview

   ```mermaid
   flowchart LR
    subgraph Client
        UI[Frontend / API Client]
    end
    
    UI --> G[API Gateway]
    G --> A[User Service]
    G --> B[Appointment Service]
    G --> C[Notification Service]
    
    A -->|Registers| E[Eureka Server]
    B -->|Registers| E
    C -->|Registers| E
    
    subgraph Databases
        D1[(MySQL  )]
        D2[(MySQL )]
    end
    
    A --> D1
    B --> D2
    
    B -- Sends Events --> K[Kafka]
    K -- Consumes Events --> C
```

------------------------------------------------------------------------

## ✨ Features

- Environment Configuration via `.env` files (Dev/Prod profiles).
- JWT Authentication & Authorization.
- Forgot & Reset Password with email integration.
- Event-driven notifications with **Kafka + Spring Kafka**.
- Centralized Service Discovery & Load Balancing with Eureka.
- API Gateway as single entry point.
- Structured logging with SLF4J + Logback.
- Unit & Integration Testing with JUnit & Mockito.
- API Documentation via Spring REST Docs.

------------------------------------------------------------------------

## 🚀 Technologies Used

- **Java 21 + Spring Boot 3**
- **Spring Cloud Netflix Eureka**
- **Spring Cloud Gateway**
- **Spring Kafka**
- **MySQL**
- **JWT Authentication**
- **JUnit 5 & Mockito**
- **Spring REST Docs**
- **Docker & Docker Compose**
------------------------------------------------------------------------

## 🧪 Testing & Documentation

-   Unit tests written with JUnit 5 and Mockito.
-   Automated API documentation generated with Spring REST Docs.
-   Ensures reliable, well-tested, and documented microservices.

------------------------------------------------------------------------

## 📜 Logging

- Uses **SLF4J Logger** with **Logback** configuration.  
- Uses **SLF4J Logger** with **Logback**.
- Profile-based logging:
    - **Dev** 
    - **Prod** 


## 🎯 How to Run

### 🔹 Local (without Docker)

#### Prerequisites:
- Ensure you have the following installed:

   - Java 21
   - Maven
   - MySQL
   - Kafka & Zookeeper (can be run via Docker, see below)
   
1.  Clone each repository:

        git clone https://github.com/mahmoud-Ramadan2/user-service.git
        git clone https://github.com/mahmoud-Ramadan2/appointment-service.git
        git clone  https://github.com/mahmoud-Ramadan2/notification-service.git
        git clone https://github.com/mahmoud-Ramadan2/eureka-server.git
        git clone https://github.com/mahmoud-Ramadan2/gateway-service.git

2. Start **MySQL** and create databases (`doctor_appointment`, etc.).

3.  Start **Kafka & Zookeeper** [see Docker section below](#kafka-setup-with-docker-compose).
4.  Run Eureka Server (it should be up on `http://localhost:8761`).
5.  Update `.env` files in each service with appropriate configurations
    (DB credentials, JWT secrets, SMTP settings, Kafka brokers, etc.).
6.  Run via Maven in each service directory:
```bash
    mvn clean spring-boot:run
```
7.  Access the API Gateway at `http://localhost:8080`.
8.  Use Postman or similar tools to test the endpoints.
9.  Check logs for any issues.

---

### 🔹 Using Docker Compose

1. Start Kafka & Zookeeper:

```bash
docker compose -f docker-compose.kafka.yml up -d
```

2. Start Eureka, User Service, Appointment Service, Notification Service, and Gateway (each has its own Dockerfile).

3. Access services via Gateway:
    - API Gateway → `http://localhost:8080`
    - Eureka Dashboard → `http://localhost:8761`


#### Kafka Setup With Docker Compose
```bash
# Here docker-compose file to install kafka

services:
 # 1. Zookeeper service
 zookeeper:
  image: confluentinc/cp-zookeeper:7.7.0
  container_name: zookeeper
  ports:
   - "2181:2181"
  environment:
   ZOOKEEPER_CLIENT_PORT: 2181
   ZOOKEEPER_TICK_TIME: 2000
  networks:
    - kafka-network
    
 # 2. kafka service
 kafka:
  image: confluentinc/cp-kafka:7.7.0
  container_name: kafka
  ports:
   - "9092:9092"
  environment:
   KAFKA_BROKER_ID: 1
   KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
   KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://localhost:9092
   KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
  depends_on:
   - zookeeper
  networks:
    - kafka-network
    
networks:
 kafka-network:
  driver: bridge
 ```
------------------------------------------------------------------------

🔹 Services & Routes

All requests go through the API Gateway (http://localhost:8080) and are routed to the appropriate microservice.

| Service                 | Base Path (via Gateway)         | Example Endpoints                                                                                                                                                                                                                                       |
|-------------------------|---------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **User Service**        | `/users/**`                     | - `POST /users/auth/register` → Register a new user <br> - `POST /users/auth/login` → User login (JWT token) <br> - `POST /users/auth/forgot-password` → Send reset pass link mail <br> - `POST /users/auth/reset-password` → reset new pass            |
| **Appointment Service** | `/appointments/**`              | - `POST /appointments` → Book a new appointment <br> - `GET /appointments/doctor/{id}` → List doctor's appointment by ID <br> - `GET /appointments/patient/{id}` → List patient's appointment by ID <br> - `GET /appointments/` → List all appointments |
| **Notification Service** |     | Auto email on appointment booking (via Kafka event)                                                                                                                                                                                                     |
| **(Future) Services**   | `/payments/**` | To be implemented                                                                                                                                                                                                                                       |

------------------------------------------------------------------------

## 📖 Notes

- Each microservice has its own `pom.xml` and runs independently.
- Configurations use `.env.dev` and `.env.prod` for environment-specific variables.
- Future: Centralized Config Server, CI/CD pipeline, full Docker Compose orchestration.  