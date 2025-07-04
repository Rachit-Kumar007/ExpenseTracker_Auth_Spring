# 🧾 Expense Tracker – Authentication Service (Spring Boot + JWT)

This is a backend authentication and authorization service for an **Expense Tracker** application. It uses **Spring Boot**, **Spring Security**, and **JWT** (JSON Web Token) for secure login, token-based session handling, and role-based access control. Designed with clean architecture and LLD principles in mind, this service can easily be plugged into a microservices setup using an API Gateway.

---

##  Problem Statement

The Auth Service is responsible for authenticating and authorizing all incoming requests from:
- The **Templatisation Service**
- The **API Gateway**

It ensures:
- Secure signup and login
- Token-based session management
- Scalable, fault-tolerant event publishing
- Clean DB design avoiding complex joins and long-running queries (LRQs)

---

##  Functional Requirements

- ✅ User should be able to sign up and log in
- ✅ Encrypted password storage using Bcrypt
- ✅ JWT and Refresh Tokens generated during login
- ✅ Tokens securely passed via HTTPS
- ✅ Refresh token flow to maintain user session
- ✅ Fault-tolerant event publishing to userService

---

##  Non-Functional Requirements

- 🔄 Minimal login delay to avoid slow app load
- 💽 Optimized database schema for fast reads
- 🧠 Stateless authentication using JWT
- ⚙️ Scalable and modular design

---

##  Low-Level Design (LLD)

### Key Components

| Component               | Responsibility |
|------------------------|----------------|
| `SecurityConfig`        | Configures filters, security context, and auth manager |
| `JWTFilter`             | Filters requests and validates JWT tokens |
| `AuthController`        | Handles `/signup` and `/ping` endpoints |
| `TokenController`       | Issues new access tokens using refresh tokens |
| `JwtService`            | Generates and validates JWTs, extracts claims |
| `RefreshTokenService`   | Creates and validates refresh tokens |
| `UserDetailsServiceImpl`| Custom user loader and signup logic |
| `AuthenticationManager` | Authenticates username-password credentials |
| `EventPublisher`        | Publishes user events to downstream consumers |

---

## 🗃️ Database Schema

### Entity Relationship Diagram

- `users`: Stores user credentials (ID, username, password)
- `tokens`: Stores JWT tokens and expiration timestamps
- `roles`: Maps roles like USER / ADMIN
- `users_roles`: Many-to-many relationship between users and roles

---

## 🔧 Tech Stack

- **Java 18**
- **Spring Boot 3**
- **Spring Security**
- **JWT (io.jsonwebtoken)**
- **MySQL (via JPA)**
- **Gradle**
- **Lombok**

---

## 🚀 Getting Started Locally

### 1. Clone the repo
```bash
git clone https://github.com/Rachit-Kumar007/ExpenseTracker_Auth_Spring.git
cd ExpenseTracker_Auth_Spring
```

### 2. Configure MySQL
- Create a database named `expense_tracker_auth`
- Update `application.properties` with your MySQL credentials:
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/expense_tracker_auth
spring.datasource.username=your_username
spring.datasource.password=your_password
```

### 3. Build and Run
```bash
./gradlew build
./gradlew bootRun
```

### 4. Test Endpoints
| Method | Endpoint                | Description                               |
| ------ | ----------------------- | ----------------------------------------- |
| `POST` | `/auth/v1/signup`       | Register a new user                       |
| `POST` | `/auth/v1/login`        | Login and receive JWT + refresh token     |
| `POST` | `/auth/v1/refreshToken` | Get a new JWT using a valid refresh token |
| `GET`  | `/auth/v1/ping`         | Ping the server (auth check)              |