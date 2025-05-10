### **Spring Boot Annotations**

Spring Boot provides a comprehensive set of annotations to make development easier. These annotations simplify various aspects of the application, from creating RESTful APIs to wiring beans. Here's a breakdown of the annotations you mentioned:

---

### **1. `@SpringBootApplication`**

#### **Purpose**:

* This is a **meta-annotation** that combines three important annotations:

  * `@Configuration`: Marks the class as a source of bean definitions for the application context.
  * `@EnableAutoConfiguration`: Tells Spring Boot to automatically configure the application based on the dependencies in the classpath.
  * `@ComponentScan`: Automatically scans for Spring components (controllers, services, repositories, etc.) in the same package and sub-packages.

#### **Usage**:

* It is typically placed on the main application class to enable Spring Boot auto-configuration and component scanning.

```java
@SpringBootApplication
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args);
    }
}
```

---

### **2. `@RestController`**

#### **Purpose**:

* Combines `@Controller` and `@ResponseBody`.
* It is used for **RESTful web services** in Spring.
* The `@ResponseBody` annotation is used to indicate that the data returned from each `@RequestMapping` method will be written directly to the HTTP response body in JSON/XML format (usually JSON in most cases).

#### **Usage**:

* Annotate a class to indicate it is a controller for REST endpoints that returns data directly.

```java
@RestController
public class MyController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello, World!";
    }
}
```

---

### **3. `@Controller`**

#### **Purpose**:

* The `@Controller` annotation is used to **define a Spring MVC controller**.
* It is typically used in traditional web applications that return **views** (HTML, JSP, Thymeleaf, etc.).

#### **Usage**:

* Annotate a class to indicate it handles HTTP requests and returns views or data.

```java
@Controller
public class MyController {
    
    @GetMapping("/home")
    public String home() {
        return "index";  // Returns index.html view (Thymeleaf or JSP)
    }
}
```

---

### **4. `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`**

#### **Purpose**:

* These annotations are specialized versions of `@RequestMapping` used to **handle specific HTTP methods** in RESTful web services.

  * `@GetMapping`: Handles HTTP GET requests (retrieve data).
  * `@PostMapping`: Handles HTTP POST requests (create data).
  * `@PutMapping`: Handles HTTP PUT requests (update data).
  * `@DeleteMapping`: Handles HTTP DELETE requests (delete data).

#### **Usage**:

* These annotations are placed on methods in controllers to map the specific HTTP methods to the method.

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.getUserById(id);
    }

    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.createUser(user);
    }

    @PutMapping("/{id}")
    public User updateUser(@PathVariable Long id, @RequestBody User user) {
        return userService.updateUser(id, user);
    }

    @DeleteMapping("/{id}")
    public void deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
    }
}
```

---

### **5. `@RequestBody`**

#### **Purpose**:

* Used to **bind the incoming HTTP request body** to a method parameter in the controller.
* Commonly used to deserialize JSON or XML data sent to the server into a Java object.

#### **Usage**:

* Typically used in `@PostMapping`, `@PutMapping`, or `@PatchMapping` methods where the client sends data in the body.

```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@RequestBody User user) {
    userService.save(user);
    return ResponseEntity.status(HttpStatus.CREATED).body(user);
}
```

---

### **6. `@PathVariable`**

#### **Purpose**:

* Used to extract values from the URI **path** and bind them to method parameters.

#### **Usage**:

* Commonly used in RESTful APIs to extract variable values from the URI.

```java
@GetMapping("/users/{id}")
public ResponseEntity<User> getUserById(@PathVariable Long id) {
    User user = userService.getUserById(id);
    return ResponseEntity.ok(user);
}
```

---

### **7. `@RequestParam`**

#### **Purpose**:

* Used to **bind query parameters** from the URL to method parameters in controllers.

#### **Usage**:

* Commonly used in REST APIs when parameters are passed in the query string of the URL.

```java
@GetMapping("/users")
public ResponseEntity<List<User>> getUsersByAge(@RequestParam int age) {
    List<User> users = userService.getUsersByAge(age);
    return ResponseEntity.ok(users);
}
```

---

### **8. `@Autowired`**

#### **Purpose**:

* Used to **inject dependencies** automatically into Spring beans.
* Spring will automatically wire the dependencies at runtime (i.e., dependency injection).

#### **Usage**:

* It is often used for service, repository, or other beans that need to be injected into the class.

```java
@RestController
public class MyController {

    @Autowired
    private UserService userService;

    @GetMapping("/users")
    public List<User> getUsers() {
        return userService.getAllUsers();
    }
}
```

---

### **9. `@Service`**

#### **Purpose**:

* The `@Service` annotation is a **specialization of `@Component`**.
* It is used to define service classes in Spring.
* It's used to indicate that a class performs business/service-related operations.

#### **Usage**:

* Marking a service class to hold business logic that interacts with repositories or other services.

```java
@Service
public class UserService {
    
    @Autowired
    private UserRepository userRepository;

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
}
```

---

### **10. `@Repository`**

#### **Purpose**:

* The `@Repository` annotation is a **specialization of `@Component`**.
* It is used to define **Data Access Object (DAO)** classes in Spring.
* It indicates that the class performs **data access operations** (CRUD) and should be treated as a Spring bean.

#### **Usage**:

* Used to define classes that interact with the database (using `JPA`, `JDBC`, or other ORM tools).

```java
@Repository
public class UserRepository {

    @Autowired
    private EntityManager entityManager;

    public List<User> findAll() {
        return entityManager.createQuery("FROM User", User.class).getResultList();
    }
}
```

---

### **Summary**

| Annotation               | Description                                                               |
| ------------------------ | ------------------------------------------------------------------------- |
| `@SpringBootApplication` | Marks the main application class with essential Spring Boot setup.        |
| `@RestController`        | Defines a controller for RESTful web services (returns JSON/XML).         |
| `@Controller`            | Defines a controller for MVC-based web applications (returns views).      |
| `@GetMapping`            | Handles HTTP GET requests for retrieving data.                            |
| `@PostMapping`           | Handles HTTP POST requests for creating data.                             |
| `@PutMapping`            | Handles HTTP PUT requests for updating data.                              |
| `@DeleteMapping`         | Handles HTTP DELETE requests for deleting data.                           |
| `@RequestBody`           | Binds the HTTP request body to a method parameter.                        |
| `@PathVariable`          | Extracts a variable from the URI path and binds it to a method parameter. |
| `@RequestParam`          | Binds query parameters from the URL to method parameters.                 |
| `@Autowired`             | Automatically injects dependencies into Spring beans.                     |
| `@Service`               | Defines a service class that contains business logic.                     |
| `@Repository`            | Defines a data access class for interacting with the database.            |

---
