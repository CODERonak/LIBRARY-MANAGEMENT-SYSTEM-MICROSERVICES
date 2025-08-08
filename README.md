

---

# 📚 Library Management System (LMS) – Microservices Architecture

A cloud-native, scalable **Java Spring Boot-based Library Management System**, designed with clean modular **microservices**, **JWT authentication**, and deployed on **Google Cloud Run** using **Cloud SQL**.

---

## ✅ Features

* 🔐 **JWT-based Authentication** with role-based access (`ADMIN`, `LIBRARIAN`, `MEMBER`)
* 📘 Book catalog management
* 👥 Member profile management
* 🔄 Borrowing, returns & due date tracking
* ☁️ Cloud deployment-ready (GCP, Docker, Cloud SQL)
* 📦 Microservices architecture
* 🧱 Clean project structure (Controller → Service → Repository)
* 🧪 Integrated testing with JUnit5, Mockito

---

## 🧩 Microservices Overview

| Microservice     | Responsibility                        | Database   |
| ---------------- | ------------------------------------- | ---------- |
| `auth-service`   | Authentication, JWT, roles            | PostgreSQL |
| `book-service`   | Book catalog, authors, genres         | PostgreSQL |
| `user-service`   | Member profile data                   | PostgreSQL |
| `borrow-service` | Borrowing records, due dates, returns | PostgreSQL |

---

## 🧱 Tech Stack

* Java 21, Spring Boot 3.5.4
* Spring Web, Spring Security (JWT), Spring Data JPA
* Hibernate Validator, MapStruct, Lombok
* MySQL (Cloud SQL)
* Docker & Docker Compose
* GCP: Cloud Run, Artifact Registry, Secret Manager, Cloud SQL
* Spring Cloud Gateway (API Gateway)

---

## 🗂️ Project Structure

```
lms-auth-service/
lms-book-service/
lms-user-service/
lms-borrow-service/
```

Each microservice:

```
src/
└── main/
    ├── java/com/library/{service}/
    │   ├── config/
    │   ├── controller/
    │   ├── dto/
    │   ├── entity/
    │   ├── exception/
    │   ├── mapper/
    │   ├── repository/
    │   └── service/
    └── resources/
        └──  application.yml
```

---

## 📦 GitHub Repositories (Coming Soon)

| Microservice     | Repository Name      | Link             |
| ---------------- | -------------------- | ---------------- |
| `auth-service`   | `LMS-Auth-Service`   | 🔗 *Coming Soon* |
| `book-service`   | `LMS-Book-Service`   | 🔗 *Coming Soon* |
| `user-service`   | `LMS-User-Service`   | 🔗 *Coming Soon* |
| `borrow-service` | `LMS-Borrow-Service` | 🔗 *Coming Soon* |

---

## 🔐 Auth Service

### 🔑 Auth Endpoints

| Method | Endpoint         | Description         |
| ------ | ---------------- | ------------------- |
| POST   | `/auth/register` | Register a new user |
| POST   | `/auth/login`    | Login & get JWT     |

---

## 📘 Book Service

### 📚 Endpoints

| Method | Endpoint      | Description    |
| ------ | ------------- | -------------- |
| GET    | `/books`      | List all books |
| GET    | `/books/{id}` | Get book by ID |
| POST   | `/books`      | Add a new book |
| PUT    | `/books/{id}` | Update book    |
| DELETE | `/books/{id}` | Delete book    |

---

## 👤 User Service

### 👥 Endpoints

| Method | Endpoint        | Description    |
| ------ | --------------- | -------------- |
| POST   | `/members`      | Create profile |
| GET    | `/members/{id}` | Get profile    |
| PUT    | `/members/{id}` | Update profile |

---

## 📚 Borrow Service

### 🔄 Endpoints

| Method | Endpoint                    | Description            |
| ------ | --------------------------- | ---------------------- |
| POST   | `/borrow`                   | Borrow a book          |
| GET    | `/borrow/user/{userId}`     | Borrow history by user |
| PUT    | `/borrow/return/{borrowId}` | Return a borrowed book |

---

## 🌐 API Gateway (Spring Cloud Gateway)

Handles routing and JWT validation.

```yaml
routes:
  - id: book-service
    uri: lb://book-service
    predicates:
      - Path=/books/**
```

---

## 🔐 Security Architecture

* Stateless JWT Authentication
* JWT stored in `Authorization: Bearer <token>` header
* Role-based access control using `@PreAuthorize`
* Gateway filters all traffic for valid JWT

---

## 🧪 Testing Strategy

* ✅ Unit tests: Service, Repository, Security
* ✅ Integration tests: Auth + Book + Borrow flows
* ✅ API Tests: Swagger UI, Postman, MockMvc

---

## 🐳 Dockerization

### Dockerfile (for each service)

```dockerfile
FROM openjdk:17-jdk-alpine
COPY target/lms-auth-service.jar app.jar
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

### Docker Compose (Dev Environment)

```yaml
version: "3.8"
services:
  auth-service:
    build: ./lms-auth-service
    ports: ["8081:8080"]
    depends_on: [db]

  book-service:
    build: ./lms-book-service
    ports: ["8082:8080"]

  user-service:
    build: ./lms-user-service
    ports: ["8083:8080"]

  borrow-service:
    build: ./lms-borrow-service
    ports: ["8084:8080"]

  db:
    image: postgres:15
    environment:
      POSTGRES_DB: lms
      POSTGRES_USER: root
      POSTGRES_PASSWORD: root
    ports: ["5432:5432"]
```

---

## ☁️ Google Cloud Deployment (GCP)

### ✅ Steps

1. **Build Docker Images**

```bash
docker build -t gcr.io/<project-id>/auth-service ./lms-auth-service
docker push gcr.io/<project-id>/auth-service
```

Repeat for each service.

2. **Cloud SQL**

* Use PostgreSQL instance
* Store DB credentials securely in **GCP Secret Manager**

3. **Cloud Run Deployment**

* Deploy each service as a container
* Connect to Cloud SQL using Cloud SQL Auth Proxy or IAM-based connectivity
* Configure environment variables (DB URL, user, password, JWT secret, etc.)

4. **Monitoring & Logging**

* Enable **Cloud Logging**, **Cloud Monitoring**
* Optional: Use **Cloud Trace** for distributed tracing

---

## 🔐 Environment Configuration

Each service has an `application.yml`, for example:

```yaml
spring:
  datasource:
    url: jdbc:postgresql://<CLOUD_SQL_IP>:5432/lms
    username: ${DB_USER}
    password: ${DB_PASS}
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true

jwt:
  secret: ${JWT_SECRET}
  expiration: 3600000
```

---

