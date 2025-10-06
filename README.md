# 🏥 Doctor Appointment Microservices System

A microservices-based system for managing doctor appointments, user accounts, notifications, and payments.  
The project is built using **Spring Boot**, **Spring Cloud**, and **Docker Compose** for orchestration.

---

## 🧱 Architecture Overview

The system consists of several independent Spring Boot microservices:

| Service | Description | Repository |
|----------|--------------|-------------|
| **User Service** | Manages users (patients, doctors, admins) | [user-service](https://github.com/mahmoud-Ramadan2/user-service.git) |
| **Appointment Service** | Handles appointment booking and management | [appointment-service](https://github.com/mahmoud-Ramadan2/appointment-service.git) |
| **Notification Service** | Sends email or message notifications | [notification-service](https://github.com/mahmoud-Ramadan2/notification-service.git) |
| **Payment Service** | Handles appointment payment operations | [payment-service](https://github.com/mahmoud-Ramadan2/payment-service.git) |
| **Gateway Service** | Central entry point routing requests to microservices | [gateway-service](https://github.com/mahmoud-Ramadan2/gateway-service.git) |
| **Service Registry (Eureka)** | Service discovery for all microservices | [eureka-server](https://github.com/mahmoud-Ramadan2/eureka-service.git) |


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
---

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

---

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

---

## 🧪 Testing & Documentation

-   Unit tests written with JUnit 5 and Mockito.
-   Automated API documentation generated with Spring REST Docs.
-   Ensures reliable, well-tested, and documented microservices.

---

## 📜 Logging

- Uses **SLF4J Logger** with **Logback** configuration.  
- Uses **SLF4J Logger** with **Logback**.
- Profile-based logging:
    - **Dev** 
    - **Prod** 

---

## 📂 Project Structure

```plaintext
doctor-appointment-microservices/
│
├── README.md
├── docker-compose.yml          # Docker orchestration file (already included)
├── user-service/               # Cloned from GitHub
├── appointment-service/
├── notification-service/
├── payment-service/
├── gateway-service/
└── eureka-server/
```

Each microservice contains:
- Its own `README.md` for local setup
- A separate `.env` file defining environment variables
- Spring Boot configuration for its domain logic

---

## ⚙️ Local Setup (Without Docker)

#### Prerequisites:
- Ensure you have the following installed:

   - Java 21
   - Maven
   - MySQL
   - Kafka & Zookeeper (can be run via Docker, see below)
     
1. **Clone all repositories** inside this parent folder:
   ```bash
   git clone https://github.com/Mahmoud-Ramadan2/user-service.git
   git clone https://github.com/Mahmoud-Ramadan2/appointment-service.git
   git clone https://github.com/Mahmoud-Ramadan2/notification-service.git
   git clone https://github.com/Mahmoud-Ramadan2/payment-service.git
   git clone https://github.com/Mahmoud-Ramadan2/gateway-service.git
   git clone https://github.com/Mahmoud-Ramadan2/eureka-server.git
   ```

2. **Set up dependencies manually:**
   - Run **MySQL**, **Kafka**, and **Zookeeper** locally or using Docker.  
   - Create databases (`doctor_appointment`, etc.).

3. **Run in order:**
   1. Start `uereka-server`
   2. Start Kafka & Zookeeper
   3. Start `user-service`, `appointment-service`, `notification-service`, and `payment-service`
   4. Start `gateway-service`

4. Each service has its own `README.md` explaining its environment variables and configuration.

---

## 🐳 Running with Docker Compose (Recommended)

This project includes a preconfigured `docker-compose.yml` file for easy orchestration.

### 2️⃣ Clone All Services
```bash
git clone https://github.com/Mahmoud-Ramadan2/user-service.git
git clone https://github.com/Mahmoud-Ramadan2/appointment-service.git
git clone https://github.com/Mahmoud-Ramadan2/notification-service.git
git clone https://github.com/Mahmoud-Ramadan2/payment-service.git
git clone https://github.com/Mahmoud-Ramadan2/gateway-service.git
git clone https://github.com/Mahmoud-Ramadan2/eureka-server.git
```

### 3️⃣ Ensure Each Service Has `.env` File
Each service uses its own `.env` (check inside each repository).

### 4️⃣ Run Everything
```bash
docker compose up -d
```

Once running:
- Eureka Dashboard → [http://localhost:8761](http://localhost:8761)
- API Gateway → [http://localhost:8080](http://localhost:8080)

Check container status:
```bash
docker ps
```

---

## 🧾 Simplified Docker Compose Example

> The full version already exists as `docker-compose.yml` in this repository.  
> Below is a simplified version for reference.

```yaml
version: '3.9'
services:
  
  zookeeper:
   image: confluentinc/cp-zookeeper:7.7.0
   container_name: zookeeper
   ports:
    - "2181:2181"
   environment:
    ZOOKEEPER_CLIENT_PORT: 2181
    ZOOKEEPER_TICK_TIME: 2000
   networks:
    - appointment-network
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
      - appointment-network

  appointment-db:
    image: mysql:8.0
    container_name: appointment-db
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: doctor_appointment
      MYSQL_USER: appuser
      MYSQL_PASSWORD: apppassword


    volumes:
      - mysql_data:/var/lib/mysql

    ports:
      - "3305:3306"
    networks:
      - appointment-network

  eureka-server:
    build:
      dockerfile: dockerfile
      context: ./eureka-server
    container_name: eureka-server
    ports:
      - "8761:8761"
    networks:
      - appointment-network

  gateway-service:
    build:
      dockerfile: dockerfile
      context: ./gateway-service
    container_name: gateway-service
    environment:
      EUREKA_CLIENT_SERVICEURL_DEFAULTZONE: http://eureka-server:8761/eureka/
    ports:
      - "8080:8080"
    depends_on:
      - eureka-server
    networks:
      - appointment-network

  user-service:
    build: ./user-service
    env_file: ./user-service/.env
    depends_on:
      - appointment-db
      - eureka-server
    ports:
      - "8081:8081"

  appointment-service:
    build: ./appointment-service
    env_file: ./appointment-service/.env
    depends_on: 
      - appointment-db
      - eureka-server
      - user-service
    ports:
       - "8082:8082"
    networks:
      - appointment-network
  
  notification-service:
    build: ./notification-service
    env_file: ./notification-service/.env
    depends_on:
      - appointment-db
      - eureka-server
      - kafka
    ports:
       - "8083:8083"
    networks:
       - appointment-network
  payment-service:
      build: ./payment-service
      env_file: ./payment-service/.env
      depends_on:
        - appointment-db
        - eureka-server
      ports:
        - "8084:8084"
      networks:
        - appointment-network

volumes:
  mysql_data:
networks:
  appointment-network:
    driver: bridge
```

---


## 🚀 Future Enhancements
- Add centralized logging (ELK or Prometheus)
- Add distributed tracing (Zipkin / Sleuth)
- Introduce API versioning
- Integrate CI/CD pipeline
- Deploy to Kubernetes.

---

## 🔹 Services & Routes

All requests go through the API Gateway ([http://localhost:8080](http://localhost:8080)) and are routed to the appropriate microservice.

| Service                 | Base Path (via Gateway)         | Example Endpoints                                                                                                                                                                                                                                       |
|--------------------------|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **User Service**        | `/users/**`                     | - `POST /users/auth/register` → Register a new user <br> - `POST /users/auth/login` → Login (JWT token) <br> - `POST /users/auth/forgot-password` → Send reset email <br> - `POST /users/auth/reset-password` → Reset password                          |
| **Appointment Service** | `/appointments/**`              | - `POST /appointments` → Book appointment <br> - `GET /appointments/doctor/{id}` → Doctor's appointments <br> - `GET /appointments/patient/{id}` → Patient's appointments <br> - `GET /appointments/` → List all appointments                          |
| **Notification Service** | `/notifications/**`             | Auto email notification triggered by Kafka event on new appointment                                                                                                                                                                                    |
| **Payment Service**     | `/payments/**`                  | - `POST /payments` → Process payment <br> - `GET /payments/{id}` → Retrieve payment details                                                                                                                      |

---

## 🧰 Database Access (Optional)

Use **DBeaver** or similar clients to inspect data during development.

---

## 📜 License
This project is licensed under the **MIT License**.  
You’re free to use, modify, and distribute it with attribution.

---

**Author:** [Mahmoud Ramadan](https://github.com/Mahmoud-Ramadan2)  
**Main Repository:** [doctor-appointment-microservices](https://github.com/Mahmoud-Ramadan2/doctor-appointment-microservices)
