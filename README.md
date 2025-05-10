## ✅ **Spring Boot Concepts to Learn**

### 🔹 1. **Introduction to Spring Boot**

* What is Spring Boot?
* Advantages over traditional Spring
* Spring Boot Starter dependencies
* Spring Boot project structure (Maven/Gradle)

### 🔹 2. **Creating a Spring Boot Project**

* Using [Spring Initializr](https://start.spring.io/)
* Project structure and purpose of files (`pom.xml`, `application.properties`, etc.)

### 🔹 3. **Spring Boot Annotations**

* `@SpringBootApplication`
* `@RestController`, `@Controller`
* `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`
* `@RequestBody`, `@PathVariable`, `@RequestParam`
* `@Autowired`, `@Service`, `@Repository`

### 🔹 4. **Building RESTful APIs (CRUD for Notes)**

* Create Note (POST)
* Read Notes (GET)
* Update Note (PUT)
* Delete Note (DELETE)

### 🔹 5. **Model and Data Handling**

* Entity class (`@Entity`, `@Id`, `@GeneratedValue`)
* Data Transfer Objects (DTOs) – optional for cleaner APIs
* Lombok (`@Data`, `@Getter`, `@Setter`, `@NoArgsConstructor`, `@AllArgsConstructor`)

### 🔹 6. **Database Integration with JPA**

* Introduction to Spring Data JPA
* Setting up MySQL/PostgreSQL/H2
* Repositories (`JpaRepository` or `CrudRepository`)
* Basic custom queries (e.g., `findByTitleContaining`)

### 🔹 7. **application.properties / application.yml**

* Configuring port, DB connection, Hibernate settings

```properties
spring.datasource.url=
spring.datasource.username=
spring.datasource.password=
spring.jpa.hibernate.ddl-auto=update
```

### 🔹 8. **Exception Handling**

* Global Exception Handling with `@ControllerAdvice`
* Custom exceptions and `@ExceptionHandler`

### 🔹 9. **Validation**

* Bean Validation with annotations like `@NotNull`, `@Size`, `@Email`
* `@Valid` usage in request bodies

### 🔹 10. **Testing APIs**

* Use Postman or Swagger UI
* Write simple unit tests (optional for beginner level)

---
