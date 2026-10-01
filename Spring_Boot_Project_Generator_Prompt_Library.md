# Spring Boot Project Generator - Prompt Library

This document contains ready-to-use prompts for the **Spring Boot Project Generator** skill.

---

## 1. Create a New Project

```text
Create a production-ready Spring Boot Employee Management REST API.

Requirements:
- Java 21
- Spring Boot 3.5.x
- Maven
- PostgreSQL
- Flyway
- OpenAPI
- Actuator
- Layered Architecture
- Validation
- Global Exception Handling

Generate the complete project.
```

---

## 2. Generate Project Structure Only

```text
Create only the project structure.

Project:
Employee Service

Technology:
Java 21
Spring Boot 3

Show package structure before generating code.
```

---

## 3. Generate pom.xml

```text
Generate only pom.xml.

Requirements:
Java 21
Spring Boot 3.5
PostgreSQL
Flyway
Validation
OpenAPI
Actuator
JUnit 5
Mockito

Do not generate any Java classes.
```

---

## 4. Generate Entity

```text
Generate Employee entity.

Fields:
id
employeeCode
name
email
salary
department
joiningDate
status

Use JPA annotations.
Use validation annotations.
Do not use Lombok.
```

---

## 5. Generate DTOs

```text
Generate DTOs.

Create:
- EmployeeCreateRequest
- EmployeeUpdateRequest
- EmployeeResponse

Use Java Records.
Include validation.
```

---

## 6. Generate Repository

```text
Generate EmployeeRepository.

Requirements:
- Spring Data JPA
- Find By Email
- Find By Department
- Pagination support
- Custom JPQL example
```

---

## 7. Generate Service Layer

```text
Generate:
- EmployeeService
- EmployeeServiceImpl

Include:
- CRUD operations
- Validation
- Business exceptions
- Transactions
```

---

## 8. Generate REST Controller

```text
Generate EmployeeController.

Endpoints:
POST /employees
PUT /employees/{id}
GET /employees/{id}
DELETE /employees/{id}
GET /employees

Support pagination.
Add OpenAPI annotations.
```

---

## 9. Generate Global Exception Handling

```text
Generate:
- BusinessException
- ResourceNotFoundException
- GlobalExceptionHandler

Return standard API error response.
```

---

## 10. Generate Flyway Scripts

```text
Generate Flyway migration.

Create Employee table.
Include indexes.
Use PostgreSQL syntax.
```

---

## 11. Generate application.yml

```text
Generate application.yml.

Include:
- PostgreSQL
- Flyway
- Actuator
- Swagger
- Logging
- Profiles

No secrets.
```

---

## 12. Generate Unit Tests

```text
Generate JUnit 5 tests.

Create tests for:
- Service
- Repository
- Controller

Use Mockito and MockMvc.
```

---

## 13. Generate Integration Tests

```text
Generate integration tests.

Use:
- @SpringBootTest
- Testcontainers
- PostgreSQL

Test complete CRUD flow.
```

---

## 14. Generate README

```text
Generate README.

Include:
- Project Overview
- Architecture
- Setup
- Build
- Run
- Swagger URL
- Health URL
- Environment Variables
- Package Structure
```

---

## 15. Generate Complete Project

```text
Create a complete production-ready Spring Boot project.

Project:
Employee Management System

Technology:
- Java 21
- Spring Boot 3.5
- Maven
- PostgreSQL
- Flyway
- OpenAPI
- Actuator

Requirements:
- CRUD
- Pagination
- Validation
- Exception Handling
- DTO
- Mapper
- Repository
- Service
- Controller
- Unit Tests
- Integration Tests
- README

Generate file-by-file.
```

---

## 16. Add JWT Security

```text
Extend the existing project.

Add:
- Spring Security
- JWT Authentication
- Role-based Authorization
- Refresh Token
- Password Encryption

Do not change existing APIs.
```

---

## 17. Add Docker

```text
Containerize the application.

Generate:
- Dockerfile
- docker-compose.yml
- PostgreSQL container
- Healthcheck
- Production-ready configuration
```

---

## 18. Add Kafka

```text
Extend the project.

Add:
- Apache Kafka
- Producer
- Consumer
- Topic Configuration
- JSON Serialization
- Error Handling
```

---

## 19. Convert to Microservices

```text
Convert the project into Microservice architecture.

Add:
- API Gateway
- Config Server
- Service Discovery
- OpenFeign
- Centralized Logging
- Distributed Tracing
```

---

## 20. Production Improvements

```text
Review the project.

Suggest improvements for:
- Performance
- Security
- Maintainability
- Scalability
- Code Quality
- SOLID
- Best Practices

Return recommendations only.
```

---

## 21. Code Review

```text
Review the generated Spring Boot project.

Check:
- SOLID
- Naming
- Package Structure
- Performance
- Security
- Exception Handling
- Validation

Return issues and fixes.
```

---

## 22. Bug Fix

```text
The application fails to compile.

Analyze every error.
Explain the root cause.
Fix all errors.

Return updated files only.
```

---

## 23. Refactoring

```text
Refactor the project.

Goals:
- Reduce duplication
- Improve readability
- Follow SOLID
- Improve performance

Do not change functionality.
```

---

## 24. Generate API Documentation

```text
Generate API documentation.

Include:
- Request
- Response
- Status Codes
- Validation Errors
- Swagger Examples
- Sample JSON
```

---

## 25. Enterprise Prompt

```text
Act as a Senior Java Architect.

Build a production-ready Employee Management REST API.

Technology:
- Java 21
- Spring Boot 3.5
- Maven
- PostgreSQL
- Flyway
- OpenAPI
- Actuator

Requirements:
- Layered Architecture
- REST API
- CRUD
- DTO
- Validation
- Repository
- Service
- Controller
- Exception Handling
- Pagination
- Sorting
- Search
- Unit Tests
- Integration Tests
- README

Constraints:
- No Lombok
- Constructor Injection
- Java Records for DTOs
- Environment Variables
- No Secrets

Generate one file at a time.
Verify compilation before moving to the next file.
```

---

# Recommended Workflow

1. Generate Project Structure
2. Generate pom.xml
3. Generate Configuration
4. Generate Entities & DTOs
5. Generate Repository
6. Generate Service
7. Generate Controller
8. Generate Exception Handling
9. Generate Flyway
10. Generate Tests
11. Generate README
12. Review & Optimize
