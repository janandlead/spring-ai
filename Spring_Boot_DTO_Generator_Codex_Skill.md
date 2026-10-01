# DTO Generator Skill for Java and Spring Boot

This document provides a reusable Codex skill for generating DTOs in Java and Spring Boot projects according to common industry standards.

---

## 1. Recommended Skill Folder

Create this folder inside your repository:

```text
.agents/
└── skills/
    └── spring-boot-dto-generator/
        └── SKILL.md
```

---

## 2. `SKILL.md`

Create:

```text
.agents/skills/spring-boot-dto-generator/SKILL.md
```

Add the following content:

```markdown
---
name: spring-boot-dto-generator
description: Generates production-ready request, response, search, filter, pagination, and error DTOs for Java and Spring Boot applications. Use this skill when creating or updating API contracts, validation models, mapping models, or DTO tests.
---

# Spring Boot DTO Generator

## Objective

Generate clean, immutable, validated, and maintainable DTOs for Java and Spring Boot applications.

Use this skill when the user asks to:

- Create request DTOs
- Create response DTOs
- Create update DTOs
- Create search or filter DTOs
- Create pagination DTOs
- Convert an entity into DTOs
- Add Bean Validation
- Generate DTO mappers
- Add DTO tests
- Refactor APIs so entities are not exposed directly

## Default Standards

Unless the user specifies otherwise:

- Use Java 21
- Prefer Java records for immutable DTOs
- Use Jakarta Bean Validation
- Use explicit request and response DTOs
- Keep create and update DTOs separate
- Keep API DTOs independent from JPA entities
- Do not include persistence annotations in DTOs
- Do not expose internal IDs unless required
- Use clear field names
- Use ISO-8601 date and time types
- Use `BigDecimal` for monetary values
- Use enums for controlled values
- Do not add Lombok unless the project already uses it

## DTO Categories

Generate DTOs based on API purpose.

### Create Request DTO

Used when creating a new resource.

Example:

```java
public record EmployeeCreateRequest(
        @NotBlank(message = "Employee code is required")
        @Size(max = 30, message = "Employee code must not exceed 30 characters")
        String employeeCode,

        @NotBlank(message = "First name is required")
        @Size(max = 100, message = "First name must not exceed 100 characters")
        String firstName,

        @NotBlank(message = "Last name is required")
        @Size(max = 100, message = "Last name must not exceed 100 characters")
        String lastName,

        @NotBlank(message = "Email is required")
        @Email(message = "Email must be valid")
        @Size(max = 150, message = "Email must not exceed 150 characters")
        String email,

        @NotNull(message = "Department ID is required")
        Long departmentId
) {
}
```

### Update Request DTO

Used when updating an existing resource.

For full updates, require mandatory fields.

For partial updates, use nullable fields and validate only supplied values.

Example:

```java
public record EmployeeUpdateRequest(
        @Size(max = 100, message = "First name must not exceed 100 characters")
        String firstName,

        @Size(max = 100, message = "Last name must not exceed 100 characters")
        String lastName,

        @Email(message = "Email must be valid")
        @Size(max = 150, message = "Email must not exceed 150 characters")
        String email,

        Long departmentId,

        EmployeeStatus status
) {
}
```

### Response DTO

Used for API responses.

Example:

```java
public record EmployeeResponse(
        Long id,
        String employeeCode,
        String firstName,
        String lastName,
        String email,
        DepartmentSummaryResponse department,
        EmployeeStatus status,
        Instant createdAt,
        Instant updatedAt
) {
}
```

### Summary DTO

Use when only a subset of related resource data is needed.

Example:

```java
public record DepartmentSummaryResponse(
        Long id,
        String name
) {
}
```

### Search DTO

Use for search criteria.

Example:

```java
public record EmployeeSearchRequest(
        String employeeCode,
        String name,
        String email,
        Long departmentId,
        EmployeeStatus status
) {
}
```

### Pagination Response DTO

Example:

```java
public record PageResponse<T>(
        List<T> content,
        int page,
        int size,
        long totalElements,
        int totalPages,
        boolean first,
        boolean last
) {
}
```

### Error DTO

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

## Naming Conventions

Use these suffixes:

```text
CreateRequest
UpdateRequest
PatchRequest
Response
SummaryResponse
SearchRequest
FilterRequest
PageResponse
ApiError
```

Examples:

```text
EmployeeCreateRequest
EmployeeUpdateRequest
EmployeePatchRequest
EmployeeResponse
EmployeeSummaryResponse
EmployeeSearchRequest
EmployeeFilterRequest
PageResponse
ApiError
```

Avoid unclear names such as:

```text
EmployeeDto
EmployeeData
EmployeeModel
EmployeePayload
EmployeeObject
```

Use purpose-specific names instead.

## Package Structure

Place DTOs inside the related feature.

Preferred structure:

```text
employee/
├── api/
├── application/
├── domain/
├── infrastructure/
├── dto/
│   ├── EmployeeCreateRequest.java
│   ├── EmployeeUpdateRequest.java
│   ├── EmployeePatchRequest.java
│   ├── EmployeeResponse.java
│   ├── EmployeeSummaryResponse.java
│   └── EmployeeSearchRequest.java
└── mapper/
    └── EmployeeMapper.java
```

For large projects, separate by API direction:

```text
employee/
└── dto/
    ├── request/
    │   ├── EmployeeCreateRequest.java
    │   ├── EmployeeUpdateRequest.java
    │   └── EmployeeSearchRequest.java
    └── response/
        ├── EmployeeResponse.java
        └── EmployeeSummaryResponse.java
```

Use the simpler structure unless the number of DTOs becomes large.

## Validation Rules

Use Jakarta Bean Validation.

Common annotations:

```text
@NotNull
@NotBlank
@NotEmpty
@Size
@Email
@Pattern
@Min
@Max
@Positive
@PositiveOrZero
@Negative
@Past
@PastOrPresent
@Future
@FutureOrPresent
@DecimalMin
@DecimalMax
@Digits
@Valid
```

## Validation Guidelines

Follow these rules:

1. Add validation to request DTOs.
2. Do not add request validation annotations to response DTOs.
3. Use clear validation messages.
4. Match DTO validation with database constraints.
5. Use `@Valid` for nested DTOs.
6. Use `@NotNull` for required non-text values.
7. Use `@NotBlank` for required strings.
8. Use `@Size` for text length constraints.
9. Use `@Email` for email fields.
10. Use `@Pattern` only when a simpler constraint is not enough.
11. Avoid duplicate validation logic.
12. Keep business rules in the service or domain layer.
13. Use custom validators only for cross-field or domain-specific rules.
14. Do not validate generated fields such as IDs and timestamps in create requests.

## Date and Time Rules

Use:

- `LocalDate` for date-only values
- `LocalDateTime` for local date-time values without timezone
- `OffsetDateTime` when the timezone offset matters
- `Instant` for timestamps stored and exchanged in UTC

Avoid:

```text
java.util.Date
java.sql.Date
String for date values
```

Example:

```java
public record AppointmentCreateRequest(
        @NotNull(message = "Appointment date is required")
        @FutureOrPresent(message = "Appointment date must be today or later")
        LocalDate appointmentDate
) {
}
```

## Monetary Fields

Use `BigDecimal`.

Example:

```java
public record ProductCreateRequest(
        @NotBlank(message = "Product name is required")
        String name,

        @NotNull(message = "Price is required")
        @DecimalMin(value = "0.0", inclusive = false, message = "Price must be greater than zero")
        @Digits(integer = 10, fraction = 2, message = "Price must contain at most 10 integer digits and 2 decimal places")
        BigDecimal price
) {
}
```

Do not use:

```text
double
float
```

for money.

## Enum Fields

Use enums for controlled values.

Example:

```java
public enum EmployeeStatus {
    ACTIVE,
    INACTIVE,
    ON_LEAVE
}
```

DTO:

```java
public record EmployeeUpdateRequest(
        EmployeeStatus status
) {
}
```

Do not use arbitrary strings when values come from a fixed set.

## Nested DTO Rules

Use nested DTOs when returning related data.

Example:

```java
public record EmployeeResponse(
        Long id,
        String employeeCode,
        String firstName,
        String lastName,
        DepartmentSummaryResponse department
) {
}
```

Avoid returning full nested entities because this can:

- Expose internal data
- Create large payloads
- Cause recursion
- Trigger lazy-loading problems
- Create unstable API contracts

## Mapping Standards

Create a mapper for every feature with multiple DTO transformations.

Example:

```java
@Component
public class EmployeeMapper {

    public Employee toEntity(EmployeeCreateRequest request) {
        Employee employee = new Employee();
        employee.setEmployeeCode(request.employeeCode());
        employee.setFirstName(request.firstName());
        employee.setLastName(request.lastName());
        employee.setEmail(request.email());
        return employee;
    }

    public EmployeeResponse toResponse(Employee employee) {
        DepartmentSummaryResponse department = null;

        if (employee.getDepartment() != null) {
            department = new DepartmentSummaryResponse(
                    employee.getDepartment().getId(),
                    employee.getDepartment().getName()
            );
        }

        return new EmployeeResponse(
                employee.getId(),
                employee.getEmployeeCode(),
                employee.getFirstName(),
                employee.getLastName(),
                employee.getEmail(),
                department,
                employee.getStatus(),
                employee.getCreatedAt(),
                employee.getUpdatedAt()
        );
    }

    public void updateEntity(EmployeeUpdateRequest request, Employee employee) {
        if (request.firstName() != null) {
            employee.setFirstName(request.firstName());
        }

        if (request.lastName() != null) {
            employee.setLastName(request.lastName());
        }

        if (request.email() != null) {
            employee.setEmail(request.email());
        }

        if (request.status() != null) {
            employee.setStatus(request.status());
        }
    }
}
```

## MapStruct Guidance

Use MapStruct only when:

- The project already uses it
- There are many repetitive mappings
- Nested mapping is manageable
- The team accepts compile-time generated mapping

Do not introduce MapStruct for one or two simple DTOs.

Example interface:

```java
@Mapper(componentModel = "spring")
public interface EmployeeMapper {

    Employee toEntity(EmployeeCreateRequest request);

    EmployeeResponse toResponse(Employee employee);

    void updateEntity(
            EmployeeUpdateRequest request,
            @MappingTarget Employee employee
    );
}
```

For partial updates with MapStruct, configure null-value handling explicitly.

## JSON Serialization

Use Jackson annotations only when required.

Examples:

```java
@JsonProperty("employee_code")
String employeeCode
```

```java
@JsonFormat(pattern = "yyyy-MM-dd")
LocalDate joiningDate
```

Prefer consistent global naming and date configuration instead of adding annotations everywhere.

Do not expose fields solely because serialization allows it.

## Security and Privacy

Do not include these fields in response DTOs unless explicitly required:

- Passwords
- Password hashes
- Access tokens
- Refresh tokens
- Secret keys
- Internal audit details
- Internal database identifiers
- Personal or medical information not required by the endpoint

Create separate internal models when administrative fields are needed.

## API Versioning

Do not reuse a DTO across API versions when the contract changes significantly.

Example:

```text
api/v1/employee/dto/EmployeeResponse.java
api/v2/employee/dto/EmployeeResponse.java
```

Avoid changing an existing public DTO in a breaking way.

## Update Semantics

Use different request DTOs for different update behaviours.

### Full Update

For `PUT`, all replaceable fields may be required.

```text
EmployeeUpdateRequest
```

### Partial Update

For `PATCH`, fields may be optional.

```text
EmployeePatchRequest
```

Do not use the same DTO for create, full update, and partial update unless the contract is truly identical.

## DTO Testing Standards

Generate tests for:

- Required-field validation
- Invalid email
- Invalid field length
- Invalid numeric ranges
- Invalid date ranges
- Nested validation
- Mapper conversions
- Partial update behaviour
- Null-handling
- Enum serialization where relevant

Example validation test:

```java
class EmployeeCreateRequestValidationTest {

    private Validator validator;

    @BeforeEach
    void setUp() {
        validator = Validation.buildDefaultValidatorFactory().getValidator();
    }

    @Test
    void shouldRejectBlankEmployeeCode() {
        EmployeeCreateRequest request = new EmployeeCreateRequest(
                "",
                "Anand",
                "J",
                "anand@example.com",
                1L
        );

        Set<ConstraintViolation<EmployeeCreateRequest>> violations =
                validator.validate(request);

        assertThat(violations)
                .extracting(ConstraintViolation::getPropertyPath)
                .extracting(Object::toString)
                .contains("employeeCode");
    }
}
```

Example mapper test:

```java
class EmployeeMapperTest {

    private EmployeeMapper mapper;

    @BeforeEach
    void setUp() {
        mapper = new EmployeeMapper();
    }

    @Test
    void shouldMapCreateRequestToEntity() {
        EmployeeCreateRequest request = new EmployeeCreateRequest(
                "EMP-1001",
                "Anand",
                "J",
                "anand@example.com",
                1L
        );

        Employee employee = mapper.toEntity(request);

        assertThat(employee.getEmployeeCode()).isEqualTo("EMP-1001");
        assertThat(employee.getFirstName()).isEqualTo("Anand");
        assertThat(employee.getEmail()).isEqualTo("anand@example.com");
    }
}
```

## Generation Workflow

When this skill is invoked:

1. Inspect the related entity, controller, service, and API contract.
2. Identify create, update, patch, response, summary, search, and filter needs.
3. Avoid creating unnecessary DTOs.
4. Use purpose-specific names.
5. Prefer records for immutable DTOs.
6. Add request validation.
7. Create nested summary DTOs instead of exposing entities.
8. Create or update mapper methods.
9. Update controller method signatures.
10. Update service method signatures where needed.
11. Add validation tests.
12. Add mapper tests.
13. Run formatting.
14. Run Maven verification.
15. Fix compilation and test failures.
16. Report generated and modified files.

## Definition of Done

The task is complete only when:

- JPA entities are not exposed through REST endpoints.
- Create and update concerns are separated.
- Request DTOs contain appropriate validation.
- Response DTOs contain only required fields.
- Sensitive fields are excluded.
- Nested relationships use summary DTOs where appropriate.
- Mapping logic is implemented.
- Validation tests are added.
- Mapper tests are added.
- The project compiles.
- Tests pass.
- Maven verification succeeds.

## Final Response Format

After generating DTOs, report:

```text
Feature:
<feature-name>

Generated DTOs:
<list>

Updated mapper:
<mapper-name>

Validation added:
<summary>

Tests added:
<test list>

Verification:
<command and result>
```
```

---

## 3. Repository Instructions

You may add the following section to your repository-level `AGENTS.md`:

```markdown
## DTO Standards

- Never expose JPA entities through REST APIs.
- Use separate DTOs for create, update, patch, and response operations.
- Prefer Java records for immutable DTOs.
- Add Jakarta Bean Validation to request DTOs.
- Do not add validation annotations to response DTOs.
- Use `BigDecimal` for monetary fields.
- Use Java time types for dates and timestamps.
- Use enums for controlled values.
- Exclude passwords, tokens, secrets, and unnecessary personal data.
- Use summary DTOs for nested relationships.
- Add mapper and validation tests.
- Use the `spring-boot-dto-generator` skill for DTO generation and API contract refactoring.
```

---

## 4. Prompt to Invoke the Skill

Use this prompt in Codex:

```text
Use the spring-boot-dto-generator skill.

Generate DTOs for the Employee feature.

Entity fields:

- id: Long
- employeeCode: String
- firstName: String
- lastName: String
- email: String
- department: Department
- status: EmployeeStatus
- joiningDate: LocalDate
- salary: BigDecimal
- createdAt: Instant
- updatedAt: Instant

Generate:

- EmployeeCreateRequest
- EmployeeUpdateRequest
- EmployeePatchRequest
- EmployeeResponse
- EmployeeSummaryResponse
- EmployeeSearchRequest
- DepartmentSummaryResponse

Requirements:

- Use Java records.
- Use Jakarta Bean Validation.
- Add clear validation messages.
- Do not expose JPA entities.
- Use BigDecimal for salary.
- Use LocalDate for joiningDate.
- Use Instant for timestamps.
- Use EmployeeStatus enum.
- Use DepartmentSummaryResponse in EmployeeResponse.
- Do not expose sensitive or internal fields.
- Create or update EmployeeMapper.
- Add mapper tests.
- Add Bean Validation tests.
- Update controller and service signatures if required.
- Run Maven clean verify.
- Fix all compilation and test failures.
```

---

## 5. Example Generated DTOs

### `EmployeeCreateRequest.java`

```java
package com.javacodeex.employee.dto;

import jakarta.validation.constraints.DecimalMin;
import jakarta.validation.constraints.Digits;
import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.NotNull;
import jakarta.validation.constraints.PastOrPresent;
import jakarta.validation.constraints.Size;

import java.math.BigDecimal;
import java.time.LocalDate;

public record EmployeeCreateRequest(
        @NotBlank(message = "Employee code is required")
        @Size(max = 30, message = "Employee code must not exceed 30 characters")
        String employeeCode,

        @NotBlank(message = "First name is required")
        @Size(max = 100, message = "First name must not exceed 100 characters")
        String firstName,

        @NotBlank(message = "Last name is required")
        @Size(max = 100, message = "Last name must not exceed 100 characters")
        String lastName,

        @NotBlank(message = "Email is required")
        @Email(message = "Email must be valid")
        @Size(max = 150, message = "Email must not exceed 150 characters")
        String email,

        @NotNull(message = "Department ID is required")
        Long departmentId,

        @NotNull(message = "Joining date is required")
        @PastOrPresent(message = "Joining date cannot be in the future")
        LocalDate joiningDate,

        @NotNull(message = "Salary is required")
        @DecimalMin(value = "0.0", inclusive = false, message = "Salary must be greater than zero")
        @Digits(integer = 12, fraction = 2, message = "Salary must contain at most 12 integer digits and 2 decimal places")
        BigDecimal salary
) {
}
```

### `EmployeeUpdateRequest.java`

```java
package com.javacodeex.employee.dto;

import jakarta.validation.constraints.DecimalMin;
import jakarta.validation.constraints.Digits;
import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.PastOrPresent;
import jakarta.validation.constraints.Size;

import java.math.BigDecimal;
import java.time.LocalDate;

public record EmployeeUpdateRequest(
        @Size(max = 100, message = "First name must not exceed 100 characters")
        String firstName,

        @Size(max = 100, message = "Last name must not exceed 100 characters")
        String lastName,

        @Email(message = "Email must be valid")
        @Size(max = 150, message = "Email must not exceed 150 characters")
        String email,

        Long departmentId,

        EmployeeStatus status,

        @PastOrPresent(message = "Joining date cannot be in the future")
        LocalDate joiningDate,

        @DecimalMin(value = "0.0", inclusive = false, message = "Salary must be greater than zero")
        @Digits(integer = 12, fraction = 2, message = "Salary must contain at most 12 integer digits and 2 decimal places")
        BigDecimal salary
) {
}
```

### `EmployeeResponse.java`

```java
package com.javacodeex.employee.dto;

import java.math.BigDecimal;
import java.time.Instant;
import java.time.LocalDate;

public record EmployeeResponse(
        Long id,
        String employeeCode,
        String firstName,
        String lastName,
        String email,
        DepartmentSummaryResponse department,
        EmployeeStatus status,
        LocalDate joiningDate,
        BigDecimal salary,
        Instant createdAt,
        Instant updatedAt
) {
}
```

### `DepartmentSummaryResponse.java`

```java
package com.javacodeex.employee.dto;

public record DepartmentSummaryResponse(
        Long id,
        String name
) {
}
```

---

## 6. Recommended Usage

Use this skill whenever:

- A new API feature is created
- An entity is currently returned directly
- Validation must be added
- API contracts need to be separated
- Create and update payloads differ
- Nested relationships need safer response models
- A public API version is being introduced
