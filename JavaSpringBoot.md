Awesome 🚀 Let’s now go through **Spring Boot Interview Questions (Q1–Q30)** in **detailed interview style** with **examples**.

---

# 🔹 **Spring Boot Interview Questions – Detailed Answers**

---

### **1. What is Spring Boot?**

Spring Boot is a framework built on top of Spring to simplify application development.

* Provides **auto-configuration** → reduces boilerplate code.
* Comes with **embedded servers** (Tomcat, Jetty).
* Uses **starter dependencies** for quick setup.

👉 Example: With `spring-boot-starter-web`, you can create a REST API without configuring servlet containers manually.

---

### **2. How is it different from Spring Framework?**

* **Spring Framework:** Requires XML or manual Java config, boilerplate setup.
* **Spring Boot:** Convention over configuration → minimal setup with starters and auto-configuration.

👉 Example: In Spring, you configure `DispatcherServlet` manually. In Spring Boot, it’s auto-configured.

---

### **3. What is the role of `application.properties` or `application.yml`?**

They hold **external configurations** like:

* DB connection (`spring.datasource.url`)
* Server port (`server.port=8081`)
* Logging levels

👉 Example (`application.yml`):

```yaml
server:
  port: 8081
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/testdb
    username: root
    password: root
```

---

### **4. What is auto-configuration in Spring Boot?**

Spring Boot checks dependencies on the classpath and **auto-configures beans**.
👉 Example: If `spring-boot-starter-data-jpa` is present, Spring Boot auto-configures a `DataSource` and `EntityManagerFactory`.

---

### **5. How does Spring Boot handle dependency management?**

* Uses **Spring Boot Starter POMs**.
* Predefined versions → avoids version conflicts.

👉 Example:
Instead of adding 5 dependencies (Spring MVC, Jackson, Validation), you just add:

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

---

### **6. Difference between `@Component`, `@Service`, and `@Repository`.**

* `@Component`: Generic Spring bean.
* `@Service`: Specialization for service layer beans.
* `@Repository`: DAO layer, includes exception translation for persistence.

👉 Example:

```java
@Component
class Utility {}

@Service
class UserService {}

@Repository
interface UserRepository extends JpaRepository<User, Long> {}
```

---

### **7. What is the use of `@RestController`?**

Combines `@Controller` + `@ResponseBody`.
👉 Example:

```java
@RestController
class HelloController {
    @GetMapping("/hello")
    public String sayHello() {
        return "Hello World!";
    }
}
```

---

### **8. Difference between `@Controller` and `@RestController`.**

* `@Controller`: Returns a **view (JSP/Thymeleaf)**.
* `@RestController`: Returns **JSON/XML (REST APIs)**.

---

### **9. What is the role of `@Autowired`?**

* Injects dependencies automatically (by type).
  👉 Example:

```java
@Service
class UserService {}

@RestController
class UserController {
    @Autowired
    private UserService service;
}
```

---

### **10. Explain the use of `@SpringBootApplication`.**

Meta-annotation that combines:

* `@Configuration` → Marks config class.
* `@EnableAutoConfiguration` → Enables auto config.
* `@ComponentScan` → Scans components in package.

👉 Example:

```java
@SpringBootApplication
public class MyApp {
   public static void main(String[] args) {
      SpringApplication.run(MyApp.class, args);
   }
}
```

---

### **11. What is Dependency Injection (DI) and Inversion of Control (IoC)?**

* **DI:** Instead of creating objects manually, Spring injects them.
* **IoC:** Control of object creation is inverted from developer → Spring container.

👉 Example: Instead of `new UserService()`, Spring injects it via `@Autowired`.

---

### **12. Explain Bean Scopes in Spring.**

* **Singleton (default):** One instance per container.
* **Prototype:** New instance every time.
* **Request:** One per HTTP request.
* **Session:** One per HTTP session.

👉 Example:

```java
@Component
@Scope("prototype")
class MyBean {}
```

---

### **13. Difference between `@Bean` and `@Component`.**

* `@Component`: Auto-detected during scanning.
* `@Bean`: Declared in a `@Configuration` class.

👉 Example:

```java
@Configuration
class AppConfig {
   @Bean
   public MyService service() {
      return new MyService();
   }
}
```

---

### **14. Constructor injection vs Field injection.**

* **Field Injection:** Uses `@Autowired` directly on field.
* **Constructor Injection (preferred):** Safer, allows `final` fields, better for testing.

👉 Example:

```java
@Service
class UserService {
   private final UserRepository repo;

   @Autowired
   UserService(UserRepository repo) { this.repo = repo; }
}
```

---

### **15. What is the role of `ApplicationContext`?**

* Central Spring container.
* Manages beans, dependency injection, configuration, and lifecycle.

👉 Example:

```java
ApplicationContext ctx = SpringApplication.run(MyApp.class, args);
MyBean bean = ctx.getBean(MyBean.class);
```

---

### **16. How to configure a database in Spring Boot?**

👉 In `application.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/testdb
spring.datasource.username=root
spring.datasource.password=root
spring.jpa.hibernate.ddl-auto=update
```

Spring Boot auto-configures `DataSource` + Hibernate.

---

### **17. What is Spring Data JPA?**

* Abstraction over JPA/Hibernate.
* Provides repository interfaces with minimal boilerplate.

👉 Example:

```java
public interface UserRepository extends JpaRepository<User, Long> {
   List<User> findByName(String name);
}
```

---

### **18. Difference between `CrudRepository`, `JpaRepository`, and `PagingAndSortingRepository`.**

* **CrudRepository:** Basic CRUD.
* **JpaRepository:** CRUD + JPA-specific (flush, batch).
* **PagingAndSortingRepository:** Adds pagination/sorting.

---

### **19. How do you implement pagination in Spring Data JPA?**

👉 Example:

```java
Page<User> users = repo.findAll(PageRequest.of(0, 10, Sort.by("name")));
```

---

### **20. How does Spring Boot support Hibernate integration?**

If `spring-boot-starter-data-jpa` is added:

* Auto-configures Hibernate `EntityManagerFactory`.
* No need for XML config.

---

### **21. Difference between Monolithic and Microservices architecture.**

* **Monolithic:** Entire app is one large deployment.
* **Microservices:** App split into independent services communicating via REST/Messaging.

---

### **22. How does Spring Boot implement microservices?**

* Spring Boot + Spring Cloud (Netflix Eureka, API Gateway, Config Server, Feign clients).
* Each microservice runs as an independent Spring Boot app.

---

### **23. What are Spring Boot Actuators?**

Production-ready monitoring endpoints like:

* `/actuator/health`
* `/actuator/metrics`
* `/actuator/info`

👉 Example: Helps with health checks in Kubernetes/Docker.

---

### **24. What is Spring Boot Starter?**

* Starter = pre-defined dependency bundle.
  👉 Example:
* `spring-boot-starter-web` → MVC + Tomcat + Jackson.
* `spring-boot-starter-data-jpa` → Hibernate + JPA.

---

### **25. How do you handle exceptions in Spring Boot?**

Using `@ControllerAdvice` + `@ExceptionHandler`.

👉 Example:

```java
@ControllerAdvice
class GlobalExceptionHandler {
   @ExceptionHandler(Exception.class)
   public ResponseEntity<String> handleEx(Exception e) {
       return ResponseEntity.status(500).body("Error: " + e.getMessage());
   }
}
```

---

### **26. Explain how Spring Boot supports caching.**

* Enable caching with `@EnableCaching`.
* Use `@Cacheable`, `@CacheEvict`, `@CachePut`.

👉 Example:

```java
@Cacheable("users")
public User getUser(Long id) { ... }
```

---

### **27. Difference between synchronous and asynchronous calls in Spring Boot.**

* **Synchronous:** Blocking request/response.
* **Asynchronous:** Non-blocking → executed in another thread with `@Async`.

👉 Example:

```java
@Async
public CompletableFuture<String> process() {
   return CompletableFuture.completedFuture("Done");
}
```

---

### **28. How do you secure a Spring Boot application?**

* Using **Spring Security** (form login, OAuth2, JWT).

👉 Example (JWT flow):

1. User logs in → server returns JWT.
2. Every request → JWT in header.
3. Server validates JWT before allowing access.

---

### **29. What are filters and interceptors in Spring Boot?**

* **Filter:** Servlet-level, before request reaches DispatcherServlet (e.g., logging, authentication).
* **Interceptor:** Spring-level, before controller method execution (e.g., auditing, modifying request).

---

### **30. How do you deploy a Spring Boot application in production?**

1. Package as JAR: `mvn clean package`.
2. Run as standalone: `java -jar app.jar`.
3. Deploy inside Docker/Kubernetes.
4. Deploy on cloud platforms (AWS, GCP, Azure, Heroku).

---

✅ Now you have **Spring Boot Q1–Q30 detailed answers**.

Would you like me to also create a **concise 1-page revision sheet** (just key points + examples) so you can **quickly revise before interviews**?
