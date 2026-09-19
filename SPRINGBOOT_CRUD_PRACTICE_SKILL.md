---
name: springboot-crud-practice
description: A complete 10-module masterclass skill guide for building, testing, validating, and debugging Spring Boot RESTful CRUD APIs step-by-step with JPA, PostgreSQL/H2, DTOs, Request Validation, Exception Handling, API testing, and troubleshooting.
---

# Spring Boot RESTful CRUD Practice Skill Guide — Masterclass Edition

A comprehensive, step-by-step masterclass guide for designing, building, validating, testing, and debugging Spring Boot CRUD applications from scratch to production-ready standard.

---

## 1. Learning & Practice Flow

```
1. Project Architecture & Setup
           ↓
2. Entity & Database Mapping (JPA)
           ↓
3. Repository Layer (Spring Data JPA)
           ↓
4. Business Logic Layer (Service)
           ↓
5. REST Controller & Routing Layer
           ↓
6. DTOs & Request Validation (@Valid)
           ↓
7. Global Exception Handling (@RestControllerAdvice)
           ↓
8. API Endpoint Testing (PowerShell / cURL / REST Client)
           ↓
9. Unit & Integration Testing (MockMvc & Spring Boot Test)
           ↓
10. Root-Cause Debugging & Troubleshooting Mastery
```

---

## 2. Core Engineering Principles

1. **Strict Layer Separation**: 
   `Controller` handles HTTP -> `Service` handles business logic -> `Repository` handles database IO -> `Entity` represents DB table.
2. **Always Use Wrapper Types for `@Id`**:
   Use `Integer` or `Long` for primary keys instead of primitive `int`/`long` so that unassigned IDs map cleanly to `null` on creation without triggering `HttpMessageNotReadableException`.
3. **Java Naming Conventions**:
   Use `camelCase` or standard lowercase for Java entity fields (`student_name` or `studentName`) to prevent Jackson JSON mapping failures (`400 Bad Request`).
4. **Never Expose Raw Entities directly in Production APIs**:
   Use DTOs (`RequestDTO`, `ResponseDTO`) for API boundaries to prevent security leaks and over-posting.
5. **Fail Fast with Validation**:
   Use `@Valid` and Bean Validation annotations (`@NotBlank`, `@NotNull`, `@Email`, `@Size`) at the Controller level.
6. **Centralized Error Responses**:
   Return consistent, human-readable JSON error structures using `@RestControllerAdvice`.

---

## 3. Step-by-Step Module Guide

### Module 1: Project Setup & Dependencies
Ensure your `pom.xml` contains the essential Spring Boot starters:

```xml
<dependencies>
    <!-- Web / REST API -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-webmvc</artifactId>
    </dependency>

    <!-- JPA / Hibernate ORM -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- Bean Validation -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>

    <!-- PostgreSQL Driver -->
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <scope>runtime</scope>
    </dependency>

    <!-- Lombok (Optional helper for boilerplate code) -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
```

---

### Module 2: Entity & Database Layer (`Entity`)

Create the persistent model using JPA annotations in `com.practice.Crud.Operation.Entity`:

```java
package com.practice.Crud.Operation.Entity;

import jakarta.persistence.*;

@Entity
@Table(name = "students")
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer student_id; // Using Integer wrapper instead of primitive int

    private String student_name;
    private String department;
    private String phone_number;
    private String email;
    private String address;

    // Default Constructor
    public Student() {}

    // Parameterized Constructor
    public Student(Integer student_id, String student_name, String department, String phone_number, String email, String address) {
        this.student_id = student_id;
        this.student_name = student_name;
        this.department = department;
        this.phone_number = phone_number;
        this.email = email;
        this.address = address;
    }

    // Getters and Setters
    public Integer getStudent_id() { return student_id; }
    public void setStudent_id(Integer student_id) { this.student_id = student_id; }

    public String getStudent_name() { return student_name; }
    public void setStudent_name(String student_name) { this.student_name = student_name; }

    public String getDepartment() { return department; }
    public void setDepartment(String department) { this.department = department; }

    public String getPhone_number() { return phone_number; }
    public void setPhone_number(String phone_number) { this.phone_number = phone_number; }

    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }

    public String getAddress() { return address; }
    public void setAddress(String address) { this.address = address; }
}
```

---

### Module 3: Repository Layer (`Repo`)

Create the data access interface in `com.practice.Crud.Operation.Repo`:

```java
package com.practice.Crud.Operation.Repo;

import com.practice.Crud.Operation.Entity.Student;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import java.util.Optional;

@Repository
public interface StudentRepo extends JpaRepository<Student, Integer> {
    
    // Custom finder method
    Optional<Student> findByEmail(String email);
}
```

---

### Module 4: Service Layer & Business Logic (`Service`)

Implement business rules and CRUD methods in `com.practice.Crud.Operation.Service`:

```java
package com.practice.Crud.Operation.Service;

import com.practice.Crud.Operation.Entity.Student;
import com.practice.Crud.Operation.Repo.StudentRepo;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.List;
import java.util.Optional;

@Service
public class StudentService {

    @Autowired
    private StudentRepo studentRepo;

    // Create
    public Student createStudent(Student student) {
        return studentRepo.save(student);
    }

    // Read All
    public List<Student> getAllStudents() {
        return studentRepo.findAll();
    }

    // Read by ID
    public Optional<Student> getStudentById(Integer id) {
        return studentRepo.findById(id);
    }

    // Full Update (PUT)
    public Student updateStudent(Integer id, Student updatedStudent) {
        Student existing = studentRepo.findById(id)
                .orElseThrow(() -> new RuntimeException("Student not found with ID: " + id));

        existing.setStudent_name(updatedStudent.getStudent_name());
        existing.setDepartment(updatedStudent.getDepartment());
        existing.setPhone_number(updatedStudent.getPhone_number());
        existing.setEmail(updatedStudent.getEmail());
        existing.setAddress(updatedStudent.getAddress());

        return studentRepo.save(existing);
    }

    // Partial Update (PATCH)
    public Student patchStudent(Integer id, Student partialStudent) {
        Student existing = studentRepo.findById(id)
                .orElseThrow(() -> new RuntimeException("Student not found with ID: " + id));

        if (partialStudent.getStudent_name() != null) existing.setStudent_name(partialStudent.getStudent_name());
        if (partialStudent.getDepartment() != null) existing.setDepartment(partialStudent.getDepartment());
        if (partialStudent.getPhone_number() != null) existing.setPhone_number(partialStudent.getPhone_number());
        if (partialStudent.getEmail() != null) existing.setEmail(partialStudent.getEmail());
        if (partialStudent.getAddress() != null) existing.setAddress(partialStudent.getAddress());

        return studentRepo.save(existing);
    }

    // Delete
    public void deleteStudent(Integer id) {
        if (!studentRepo.existsById(id)) {
            throw new RuntimeException("Student not found with ID: " + id);
        }
        studentRepo.deleteById(id);
    }
}
```

---

### Module 5: REST Controller Layer (`Controller`)

Expose HTTP REST endpoints in `com.practice.Crud.Operation.Controller`:

```java
package com.practice.Crud.Operation.Controller;

import com.practice.Crud.Operation.Entity.Student;
import com.practice.Crud.Operation.Service.StudentService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/students")
public class StudentController {

    @Autowired
    private StudentService studentService;

    // CREATE -> POST /api/students
    @PostMapping
    public ResponseEntity<Student> createStudent(@RequestBody Student student) {
        Student created = studentService.createStudent(student);
        return new ResponseEntity<>(created, HttpStatus.CREATED);
    }

    // GET ALL -> GET /api/students
    @GetMapping
    public ResponseEntity<List<Student>> getAllStudents() {
        return ResponseEntity.ok(studentService.getAllStudents());
    }

    // GET BY ID -> GET /api/students/{id}
    @GetMapping("/{id}")
    public ResponseEntity<Student> getStudentById(@PathVariable Integer id) {
        return studentService.getStudentById(id)
                .map(ResponseEntity::ok)
                .orElse(ResponseEntity.notFound().build());
    }

    // FULL UPDATE -> PUT /api/students/{id}
    @PutMapping("/{id}")
    public ResponseEntity<Student> updateStudent(@PathVariable Integer id, @RequestBody Student student) {
        return ResponseEntity.ok(studentService.updateStudent(id, student));
    }

    // PARTIAL UPDATE -> PATCH /api/students/{id}
    @PatchMapping("/{id}")
    public ResponseEntity<Student> patchStudent(@PathVariable Integer id, @RequestBody Student student) {
        return ResponseEntity.ok(studentService.patchStudent(id, student));
    }

    // DELETE -> DELETE /api/students/{id}
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteStudent(@PathVariable Integer id) {
        studentService.deleteStudent(id);
        return ResponseEntity.noContent().build();
    }
}
```

---

### Module 6: Request DTOs & Bean Validation (`DTO`)

To validate input data cleanly, create a Request DTO with Bean Validation:

```java
package com.practice.Crud.Operation.DTO;

import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.Size;

public class StudentRequestDTO {

    @NotBlank(message = "Student name is required")
    private String student_name;

    @NotBlank(message = "Department is required")
    private String department;

    @Size(min = 10, max = 15, message = "Phone number must be between 10 and 15 digits")
    private String phone_number;

    @Email(message = "Invalid email format")
    @NotBlank(message = "Email is required")
    private String email;

    private String address;

    // Getters and Setters...
}
```

Enable validation in your controller method:
```java
@PostMapping
public ResponseEntity<Student> createStudent(@Valid @RequestBody StudentRequestDTO dto) {
    // map DTO to entity and save
}
```

---

### Module 7: Global Exception Handling (`Exception`)

Handle errors gracefully using `@RestControllerAdvice`:

```java
package com.practice.Crud.Operation.Exception;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

import java.time.LocalDateTime;
import java.util.HashMap;
import java.util.Map;

@RestControllerAdvice
public class GlobalExceptionHandler {

    // Handle Resource Not Found
    @ExceptionHandler(RuntimeException.class)
    public ResponseEntity<Map<String, Object>> handleRuntimeException(RuntimeException ex) {
        Map<String, Object> error = new HashMap<>();
        error.put("timestamp", LocalDateTime.now());
        error.put("status", HttpStatus.NOT_FOUND.value());
        error.put("error", ex.getMessage());
        return new ResponseEntity<>(error, HttpStatus.NOT_FOUND);
    }

    // Handle Request Validation Errors
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String, String>> handleValidationExceptions(MethodArgumentNotValidException ex) {
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getFieldErrors().forEach(error -> 
            errors.put(error.getField(), error.getDefaultMessage())
        );
        return new ResponseEntity<>(errors, HttpStatus.BAD_REQUEST);
    }
}
```

---

### Module 8: API Verification Commands

#### 1. Create Student (POST):
```powershell
Invoke-RestMethod -Uri "http://localhost:8081/api/students" -Method POST -Headers @{"Content-Type"="application/json"} -Body '{
  "student_name": "Mounika",
  "department": "Computer Science",
  "phone_number": "9876543210",
  "email": "mounika@example.com",
  "address": "Hyderabad"
}'
```

#### 2. Get All Students (GET):
```powershell
Invoke-RestMethod -Uri "http://localhost:8081/api/students" -Method GET
```

#### 3. Get Student by ID (GET):
```powershell
Invoke-RestMethod -Uri "http://localhost:8081/api/students/1" -Method GET
```

#### 4. Update Student (PUT):
```powershell
Invoke-RestMethod -Uri "http://localhost:8081/api/students/1" -Method PUT -Headers @{"Content-Type"="application/json"} -Body '{
  "student_name": "Mounika S",
  "department": "AI & Data Science",
  "phone_number": "9876543210",
  "email": "mounika.s@example.com",
  "address": "Hyderabad"
}'
```

#### 5. Delete Student (DELETE):
```powershell
Invoke-RestMethod -Uri "http://localhost:8081/api/students/1" -Method DELETE
```

---

## 4. Debugging & Troubleshooting Matrix

| Symptom / Error Log | Root Cause | Solution |
| :--- | :--- | :--- |
| `Cannot map null into type int` | ID field declared as primitive `int` instead of `Integer` wrapper object | Change `private int student_id;` to `private Integer student_id;` in Entity class. |
| `(400) Bad Request` on JSON submit | Lombok getters/setters missing or capitalized field names (`Student_name`) | Add explicit getters/setters in Entity or use standard `lowercase`/`camelCase` naming. |
| `(404) Not Found` on valid URL | Mismatched server port or missing `@RequestMapping` / `@RestController` | Verify `server.port` in `application.properties` (e.g. 8081 vs 8080) and check URL path. |
| Code changes don't reflect on HTTP requests | Running process in terminal was launched before saving file edits | Stop running app with `Ctrl + C` and execute `.\mvnw.cmd spring-boot:run` to recompile. |
| `Connection refused: connect` | PostgreSQL service not running or port 5432 unreachable | Verify PostgreSQL service status or switch to H2 database for local testing. |

---

## 5. Mastery Checklist

- [x] Spring Boot dependencies configured in `pom.xml`
- [x] JPA Entity mapped with `@Entity`, `@Id`, `@GeneratedValue` using `Integer` ID
- [x] Repository extending `JpaRepository<Student, Integer>`
- [x] Service handling CRUD business logic
- [x] REST Controller exposing standard HTTP status codes (`201 Created`, `200 OK`, `204 No Content`, `404 Not Found`)
- [x] Request payload tested successfully via PowerShell `Invoke-RestMethod`
- [x] Global Exception Handler capturing custom runtime errors and validation failures

## 6. Real Project Mistake Log & Root-Cause Lessons

This section tracks actual mistakes made during local Spring Boot development and the exact fixes required:

### Mistake 1: Void Method Return Type with Return Statement
- **Symptom:** Compilation error on `@GetMapping public void getstudent() { return studentService.getstudent(); }`
- **Root Cause:** Declared method return type as `void` while attempting to return `List<Student>`.
- **Fix:** Update controller return type to match service return type: `public ResponseEntity<List<Student>> getstudent()`.

### Mistake 2: Server Port Mismatch (`404 Not Found`)
- **Symptom:** `Invoke-RestMethod : The remote server returned an error: (404) Not Found` when calling `http://localhost:8080/1`.
- **Root Cause:** Spring Boot server was configured to run on port `8081` in `application.properties` (or tomcat selected 8081 due to port conflict), but request was sent to default port `8080`.
- **Fix:** Check terminal output or `application.properties` for `server.port` and invoke `http://localhost:8081/1`.

### Mistake 3: Omitted Field Setter in Service Layer (`PUT` Request)
- **Symptom:** Department field was not updating when invoking `PUT` request.
- **Root Cause:** In `updateStudent` method, `student1.setDepartment(student.getDepartment());` was missing from the service setter mapping.
- **Fix:** Explicitly set all updated fields in Service layer for `PUT` methods or use `PATCH` for partial updates.

### Mistake 4: Raw Domain Object Return vs `ResponseEntity`
- **Symptom:** POST request returns default `200 OK` instead of `201 CREATED`.
- **Root Cause:** Returning entity object directly from `@PostMapping` method without wrapping in `ResponseEntity`.
- **Fix:** Wrap return value in `new ResponseEntity<>(createdStudent, HttpStatus.CREATED)`.

### Mistake 5: Assuming Headers Cannot Be Added to `ResponseEntity`
- **Symptom:** Confusion on how to attach metadata to HTTP responses.
- **Root Cause:** Unaware of `ResponseEntity.ok().header("Header-Name", "Value").body(data)` fluent API builder.
- **Fix:** Use `.header(key, value)` builder method on `ResponseEntity`.

---
*Created for Spring Boot Mastery Practice.*

