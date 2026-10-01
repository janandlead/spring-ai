# Industry-Standard Spring Boot Project Structure Using a Codex Skill

This guide explains how to create a reusable Codex skill that generates an initial Java and Spring Boot project structure according to common industry standards.

---

## 1. Recommended Skill Folder Structure

Create the following files inside your repository:

```text
your-project/
├── .agents/
│   └── skills/
│       └── spring-boot-project-generator/
│           └── SKILL.md
├── AGENTS.md
└── README.md
```

The repository-specific skill is stored under:

```text
.agents/skills/spring-boot-project-generator/SKILL.md
```

---

## 2. `SKILL.md`

Create this file:

```text
.agents/skills/spring-boot-project-generator/SKILL.md
```

Add the following content:

```markdown
---
name: spring-boot-project-generator
description: Creates a production-ready Java Spring Boot project structure using industry-standard architecture, naming conventions, testing, validation, exception handling, observability, database migrations, and documentation. Use this skill when creating a Spring Boot service, REST API, microservice, or initial Java project scaffold.
---

# Spring Boot Project Generator

## Objective

Create a clean, maintainable, testable, and production-ready Spring Boot project.

Use this skill when the user asks to:

- Create a new Spring Boot project
- Generate an initial project structure
- Scaffold a REST API
- Create a microservice
- Set up a Maven-based Java service
- Create a reusable enterprise project template

## Default Technology Stack

Unless the user specifies otherwise, use:

- Java 21
- Spring Boot 3.x
- Maven
- Spring Web
- Spring Data JPA
- Jakarta Bean Validation
- PostgreSQL
- Flyway
- Spring Boot Actuator
- Springdoc OpenAPI
- JUnit 5
- Mockito
- MockMvc
- Testcontainers
- JaCoCo
- Maven Enforcer Plugin
- Spotless or Checkstyle

Do not add the following unless requested:

- Docker
- Kubernetes
- Kafka
- Redis
- Spring Security
- Cloud-provider dependencies
- Messaging infrastructure

Verify dependency compatibility with the selected Spring Boot version.

## Default Project Values

When values are not supplied, use:

```text
Project name: employee-service
Group ID: com.javacodeex
Artifact ID: employee-service
Base package: com.javacodeex.employee
Java version: 21
Build tool: Maven
Database: PostgreSQL
Application type: REST API
Initial feature: employee
```

## Architecture Standard

Use package-by-feature as the default architecture.

Do not create a global package structure such as:

```text
controller/
service/
repository/
entity/
dto/
```

Instead, keep all code related to one business feature together.

## Required Project Structure

```text
src/
├── main/
│   ├── java/
│   │   └── com/javacodeex/employee/
│   │       ├── EmployeeServiceApplication.java
│   │       │
│   │       ├── common/
│   │       │   ├── api/
│   │       │   │   ├── ApiError.java
│   │       │   │   └── ApiResponse.java
│   │       │   ├── exception/
│   │       │   │   ├── BusinessException.java
│   │       │   │   ├── GlobalExceptionHandler.java
│   │       │   │   └── ResourceNotFoundException.java
│   │       │   ├── validation/
│   │       │   └── util/
│   │       │
│   │       ├── config/
│   │       │   ├── OpenApiConfiguration.java
│   │       │   └── PersistenceConfiguration.java
│   │       │
│   │       └── employee/
│   │           ├── api/
│   │           │   └── EmployeeController.java
│   │           ├── application/
│   │           │   ├── EmployeeService.java
│   │           │   └── EmployeeServiceImpl.java
│   │           ├── domain/
│   │           │   ├── Employee.java
│   │           │   └── EmployeeStatus.java
│   │           ├── infrastructure/
│   │           │   └── EmployeeRepository.java
│   │           ├── dto/
│   │           │   ├── EmployeeCreateRequest.java
│   │           │   ├── EmployeeUpdateRequest.java
│   │           │   └── EmployeeResponse.java
│   │           └── mapper/
│   │               └── EmployeeMapper.java
│   │
│   └── resources/
│       ├── application.yml
│       ├── application-local.yml
│       ├── application-test.yml
│       └── db/
│           └── migration/
│               └── V1__create_employee_table.sql
│
└── test/
    └── java/
        └── com/javacodeex/employee/
            ├── employee/
            │   ├── api/
            │   │   └── EmployeeControllerTest.java
            │   ├── application/
            │   │   └── EmployeeServiceTest.java
            │   └── infrastructure/
            │       └── EmployeeRepositoryTest.java
            └── integration/
                └── EmployeeApiIntegrationTest.java
```

## Package Responsibilities

### API Package

The `api` package contains:

- REST controllers
- HTTP request handling
- Request validation
- HTTP status mapping
- OpenAPI annotations

Controllers must remain thin.

Controllers must not contain business logic or direct database access.

### Application Package

The `application` package contains:

- Business-use-case orchestration
- Service interfaces when meaningful
- Service implementations
- Transaction boundaries
- Coordination between domain and infrastructure

### Domain Package

The `domain` package contains:

- Domain entities
- Value objects
- Domain enums
- Domain-specific rules

Avoid placing HTTP-specific logic in domain classes.

### Infrastructure Package

The `infrastructure` package contains:

- Spring Data JPA repositories
- Database adapters
- External API clients
- Messaging adapters
- Infrastructure-specific implementations

### DTO Package

Create separate DTOs for:

- Create requests
- Update requests
- Responses
- Search and filter requests

Never return JPA entities directly from REST controllers.

### Mapper Package

The `mapper` package converts:

- Request DTOs to entities
- Entities to response DTOs
- External models to internal domain objects

Use explicit mapping methods.

Use MapStruct only when the project contains enough mappings to justify it.

## Java Coding Standards

Follow these standards:

1. Use constructor injection.
2. Do not use field injection.
3. Prefer immutable DTOs.
4. Use Java records for DTOs where appropriate.
5. Never expose JPA entities through APIs.
6. Use meaningful class, variable, and method names.
7. Avoid wildcard imports.
8. Avoid static mutable state.
9. Keep methods focused.
10. Avoid unnecessary interfaces.
11. Add interfaces only where they provide a real abstraction.
12. Use `Optional` primarily as a return type.
13. Do not use `Optional` as a field or method parameter.
14. Use `BigDecimal` for monetary values.
15. Use `Instant`, `LocalDate`, or `OffsetDateTime` instead of `Date`.
16. Store timestamps consistently in UTC.
17. Never log secrets, passwords, access tokens, or personal data.
18. Never hard-code credentials.
19. Use comments to explain why, not what.
20. Follow SOLID principles without over-engineering.

## REST API Standards

Use resource-oriented endpoints.

Example:

```text
POST   /api/v1/employees
GET    /api/v1/employees/{employeeId}
GET    /api/v1/employees
PUT    /api/v1/employees/{employeeId}
PATCH  /api/v1/employees/{employeeId}
DELETE /api/v1/employees/{employeeId}
```

Follow these rules:

- Use nouns instead of verbs in URLs.
- Use plural resource names.
- Version public APIs.
- Return `201 Created` after resource creation.
- Return `200 OK` for successful reads and updates.
- Return `204 No Content` for successful deletion without a body.
- Return `400 Bad Request` for invalid input.
- Return `404 Not Found` for missing resources.
- Return `409 Conflict` for duplicates and business conflicts.
- Support pagination for collection endpoints.
- Do not expose stack traces to consumers.
- Use consistent error responses.

## Validation Standards

Use Jakarta Bean Validation.

Example:

```java
public record EmployeeCreateRequest(
        @NotBlank(message = "Employee name is required")
        @Size(max = 100, message = "Employee name must not exceed 100 characters")
        String name,

        @NotBlank(message = "Email is required")
        @Email(message = "Email must be valid")
        String email,

        @NotNull(message = "Department ID is required")
        Long departmentId
) {
}
```

Implement:

- Request-level validation
- Business-rule validation
- Database constraints

Do not use Bean Validation as a replacement for domain rules.

## Exception Handling

Create global exception handling with `@RestControllerAdvice`.

The error response should contain:

```text
timestamp
status
error
code
message
path
correlationId
validationErrors
```

Example:

```java
public record ApiError(
        Instant timestamp,
        int status,
        String error,
        String code,
        String message,
        String path,
        String correlationId,
        Map<String, String> validationErrors
) {
}
```

Do not expose:

- Stack traces
- SQL statements
- Database details
- Internal class names
- Sensitive request data

## Persistence Standards

Use Spring Data JPA.

Follow these practices:

- Use explicit table names.
- Use explicit column names.
- Add database constraints.
- Add indexes to frequently searched columns.
- Prefer lazy relationships.
- Avoid unnecessary bidirectional relationships.
- Detect and prevent N+1 queries.
- Use pagination for large result sets.
- Use projections for read-heavy queries when useful.
- Define transactions in the application layer.
- Use `@Transactional(readOnly = true)` for read-only operations.
- Do not use `ddl-auto=update` in production.
- Use Flyway for database migrations.
- Keep schema changes version controlled.

Recommended configuration:

```yaml
spring:
  jpa:
    open-in-view: false
    hibernate:
      ddl-auto: validate
    properties:
      hibernate:
        format_sql: true
```

## Configuration Standards

Use YAML configuration.

Create:

```text
application.yml
application-local.yml
application-test.yml
```

Read secrets from environment variables:

```yaml
spring:
  datasource:
    url: ${DB_URL}
    username: ${DB_USERNAME}
    password: ${DB_PASSWORD}
```

Never commit real credentials.

Create `.env.example` with placeholder values only.

## Logging and Observability

Use SLF4J.

Include:

- Meaningful structured logs
- Correlation ID support
- Spring Boot Actuator
- Health endpoint
- Info endpoint
- Metrics foundation

Use logging levels correctly:

- `ERROR`: unrecoverable operation failure
- `WARN`: unexpected but recoverable condition
- `INFO`: meaningful business or lifecycle event
- `DEBUG`: diagnostic details for developers

Do not log every method entry and exit.

## OpenAPI Documentation

Configure Springdoc OpenAPI.

Document:

- Endpoint summaries
- Request parameters
- Request bodies
- Response bodies
- HTTP response codes
- Validation errors
- Example payloads

Swagger UI should be available in local development.

## Testing Standards

Generate the following test layers.

### Unit Tests

Use JUnit 5 and Mockito for:

- Service logic
- Mapper logic
- Validation helpers
- Exception scenarios

### Controller Tests

Use MockMvc and `@WebMvcTest` for:

- Request validation
- HTTP status codes
- JSON request and response bodies
- Exception mapping

### Repository Tests

Use `@DataJpaTest` for:

- Custom repository queries
- Persistence rules
- Database constraints
- Entity mappings

### Integration Tests

Use `@SpringBootTest` and Testcontainers for:

- End-to-end API flows
- Flyway migrations
- Repository integration
- Transaction behaviour
- PostgreSQL compatibility

Testing rules:

- Follow Arrange, Act, Assert.
- Cover success and failure paths.
- Keep tests independent.
- Avoid unnecessary `Thread.sleep`.
- Use descriptive test names.
- Do not mock the class under test.
- Avoid testing Spring Framework internals.

## Maven Standards

Configure:

- Java compiler version
- Spring Boot Maven Plugin
- Maven Surefire Plugin
- Maven Failsafe Plugin when integration tests are separated
- JaCoCo
- Maven Enforcer Plugin
- Spotless or Checkstyle

Include the Maven Wrapper:

```text
mvnw
mvnw.cmd
.mvn/
```

Verification command:

```bash
./mvnw clean verify
```

Windows:

```powershell
.\mvnw.cmd clean verify
```

## Required Root Files

Generate:

```text
.gitignore
README.md
pom.xml
mvnw
mvnw.cmd
.mvn/
.env.example
```

Generate only when requested:

```text
Dockerfile
docker-compose.yml
.github/workflows/build.yml
sonar-project.properties
CODEOWNERS
CONTRIBUTING.md
```

## README Requirements

The generated README must contain:

1. Project overview
2. Architecture
3. Technology stack
4. Prerequisites
5. Local setup instructions
6. Environment variables
7. Database setup
8. Build commands
9. Test commands
10. Run commands
11. API endpoints
12. Swagger URL
13. Health-check URL
14. Package structure
15. Development standards

## Skill Execution Workflow

When this skill is invoked:

1. Inspect the existing repository.
2. Do not overwrite existing files without evaluating the impact.
3. Identify project requirements from the prompt.
4. Select the architecture and dependencies.
5. Briefly report the selected approach.
6. Generate the Maven project.
7. Create the package-by-feature structure.
8. Configure environment profiles.
9. Add Flyway migrations.
10. Add centralized exception handling.
11. Add one initial business feature.
12. Add DTOs and mappers.
13. Add unit tests.
14. Add controller tests.
15. Add repository tests.
16. Add integration tests.
17. Add OpenAPI configuration.
18. Add Actuator configuration.
19. Add README documentation.
20. Run formatting.
21. Run Maven verification.
22. Fix compilation and test failures.
23. Report the generated files and verification result.

## Definition of Done

The task is complete only when:

- The application compiles.
- Unit tests pass.
- Integration tests pass when supported by the environment.
- Maven verification succeeds.
- No credentials are committed.
- No JPA entity is exposed through the API.
- Validation is implemented.
- Centralized exception handling is implemented.
- Flyway migrations are present.
- OpenAPI documentation is configured.
- Health endpoints are configured.
- README documentation is complete.
- The structure follows package-by-feature architecture.

## Final Response Format

After project generation, respond with:

```text
Project created:
<project-name>

Architecture:
<selected architecture>

Main packages:
<package list>

Dependencies:
<dependency list>

Verification:
<command and result>

Important URLs:
Swagger: <URL>
Health: <URL>

Next recommended step:
<one clear recommendation>
```
```

---

## 3. Repository-Level `AGENTS.md`

Create this file in the repository root:

```text
AGENTS.md
```

Add:

```markdown
# Repository Instructions

## Technology

- Java 21
- Spring Boot 3
- Maven
- PostgreSQL
- Spring Data JPA
- Flyway
- JUnit 5
- Mockito
- Testcontainers

## Architecture

- Use package-by-feature.
- Keep controllers thin.
- Place business logic in application services.
- Never return JPA entities from REST controllers.
- Use separate request and response DTOs.
- Use centralized exception handling.
- Use constructor injection only.
- Add database changes through Flyway migrations.
- Add tests for every new behaviour.
- Do not add Docker unless explicitly requested.

## Coding Standards

- Use records for immutable DTOs when appropriate.
- Use `BigDecimal` for monetary values.
- Use Java time classes for dates and timestamps.
- Do not hard-code secrets.
- Do not log sensitive data.
- Avoid unnecessary abstractions.
- Keep methods small and focused.
- Use meaningful names.

## Verification

Before completing any implementation task, run:

```bash
./mvnw clean verify
```

On Windows:

```powershell
.\mvnw.cmd clean verify
```

Fix all compilation, formatting, and test failures before reporting completion.

## Skills

Use the `spring-boot-project-generator` skill whenever creating or scaffolding:

- A new Spring Boot application
- A REST API
- A microservice
- A new Java service
- An initial project structure
```

---

## 4. Prompt to Invoke the Skill

Start Codex from the project directory:

```bash
codex
```

Then use this prompt:

```text
Use the spring-boot-project-generator skill.

Create a new Employee Management REST API.

Project details:

- Project name: employee-service
- Group ID: com.javacodeex
- Artifact ID: employee-service
- Base package: com.javacodeex.employee
- Java version: 21
- Build tool: Maven
- Database: PostgreSQL
- Architecture: package-by-feature

Include:

- Spring Web
- Spring Data JPA
- Jakarta Bean Validation
- PostgreSQL driver
- Flyway
- Spring Boot Actuator
- Springdoc OpenAPI
- JUnit 5
- Mockito
- MockMvc
- Testcontainers
- JaCoCo
- Maven Enforcer Plugin
- Spotless

Create Employee CRUD operations with:

- id
- employeeCode
- firstName
- lastName
- email
- department
- status
- createdAt
- updatedAt

Requirements:

- Use separate create, update, and response DTOs.
- Use constructor injection.
- Add centralized exception handling.
- Add request validation.
- Add pagination to the list API.
- Add a Flyway migration.
- Add unit tests.
- Add controller tests.
- Add repository tests.
- Add one complete API integration test.
- Configure Swagger UI.
- Configure Actuator health checks.
- Add application-local.yml and application-test.yml.
- Add a complete README.
- Do not add Docker.
- Do not add Spring Security.
- Do not expose JPA entities through REST endpoints.
- Run Maven clean verify.
- Fix all compilation and test failures before completing the task.
```

---

## 5. Recommended Package-by-Feature Structure

For standard Spring Boot business services:

```text
employee/
├── api/
├── application/
├── domain/
├── dto/
├── infrastructure/
└── mapper/
```

This structure keeps related code together and scales better than a large global layered structure.

---

## 6. Optional Hexagonal Architecture

For complex domains or services with multiple external adapters, use:

```text
employee/
├── domain/
├── application/
│   ├── port/
│   │   ├── in/
│   │   └── out/
│   └── service/
└── adapter/
    ├── in/
    │   └── web/
    └── out/
        └── persistence/
```

Use Hexagonal Architecture when the application requires:

- Strong domain isolation
- Multiple databases or persistence adapters
- Multiple inbound interfaces
- External system integrations
- Messaging adapters
- Easy replacement of infrastructure
- Long-term enterprise evolution

For normal CRUD and standard business applications, begin with package-by-feature and avoid unnecessary complexity.
