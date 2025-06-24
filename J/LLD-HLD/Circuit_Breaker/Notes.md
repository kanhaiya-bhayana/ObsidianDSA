![[Circuit_breaker_HLD.png]]


---
# 🛡️ Circuit Breaker Integration in Spring Boot (using Resilience4j)

## 1. Required Dependencies

Add the following dependencies to your `pom.xml` (for Maven):

```xml
<!-- Resilience4j integration -->
<dependency>
    <groupId>io.github.resilience4j</groupId>
    <artifactId>resilience4j-spring-boot3</artifactId> <!-- Use spring-boot2 for Spring Boot 2.x -->
    <version>2.2.0</version> <!-- Match your Spring Boot compatibility -->
</dependency>

<!-- Spring Boot Actuator for exposing health and metrics -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>

<!-- Spring AOP for method-level circuit breaker annotations -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

> ✅ Make sure all dependencies align with your Spring Boot version (e.g., Resilience4j `spring-boot3` requires Spring Boot 3.x).

---
## 2. Using the `@CircuitBreaker` Annotation

Apply the circuit breaker to your service method:

```java
@CircuitBreaker(name = "userService", fallbackMethod = "getAllAvailableUsers")
public List<UserDto> getUsersFromExternalApi() {
    // logic to fetch users
}
```

Define the fallback method in the same class:

```java
public List<UserDto> getAllAvailableUsers(Throwable t) {
    // fallback logic (e.g., return from cache or empty list)
}
```

---
## 3. Configuration (`application.yml`)

Add the following configuration to your `application.yml`:

```yaml
management:
  health:
    circuitbreakers:
      enabled: true  # Enables health indicators for circuit breakers

  endpoints:
    web:
      exposure:
        include: health  # Exposes the /actuator/health endpoint

  endpoint:
    health:
      show-details: always  # Displays full health details in actuator output

resilience4j:
  circuitbreaker:
    instances:
      userService:
        register-health-indicator: true
        event-consumer-buffer-size: 10
        failure-rate-threshold: 50                       # % of failed calls before opening circuit
        minimum-number-of-calls: 5                       # Minimum number of calls to calculate failure rate
        wait-duration-in-open-state: 5s                  # Wait time before transitioning to half-open
        permitted-number-of-calls-in-half-open-state: 3  # Allowed test calls in half-open state
        sliding-window-size: 10
        sliding-window-type: COUNT_BASED                 # Use TIME_BASED for time-based sliding window
        automatic-transition-from-open-to-half-open-enabled: true
```

---
## 4. Verifying the Circuit Breaker

After running your application:

- Visit `http://localhost:8080/actuator/health` to see the circuit breaker status.
- Example output:
    
    ```json
    {
      "status": "UP",
      "components": {
        "circuitBreakers": {
          "status": "UP",
          "details": {
            "userService": {
              "status": "CLOSED",
              "failureRate": "0.0%",
              ...
            }
          }
        }
      }
    }
    ```
    

---
## ✅ Summary

- Add the correct dependencies.
- Use `@CircuitBreaker` with a fallback method.
- Configure your instance via `application.yml`.
- Monitor via Actuator health endpoints.
