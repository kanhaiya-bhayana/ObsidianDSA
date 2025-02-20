# 1. Why will you choose Spring Boot over Spring framework?

1. Dependency Resolution/Avoid Version conflict
2. Avoid additional configuration
3. Embeded Tomcat, Jetty (no need to deploy WAR files)
4. Provide production-ready features such as metrics, health checks


# 2. What all spring boot starter you have used or what all module you have worked on?
1. spring boot starter web
2. spring starter data JPA
3. spring boot starter AOP
4. spring boot starter web services
5. spring boot starter security
6. Spring Boot Starter for Apache Kafka
7. Spring Boot Starter for Spring Cloud
8. spring boot starter thymeleaf

https://spring.io/projects/spring-data


# 3. How will you run your Spring Boot Application?

### **Key Differences at a Glance**:

|Aspect|Run Button in IDE|`mvn spring-boot:run`|
|---|---|---|
|**Execution**|Direct invocation of `main()` method.|Maven plugin executes the application.|
|**Dependency Management**|IDE's classpath configuration.|Maven resolves dependencies.|
|**Build Updates**|May not reflect recent `pom.xml` changes.|Reflects recent `pom.xml` changes.|
|**Speed**|Typically faster for development.|Slower due to build and dependency checks.|
|**Debugging**|Easy to attach a debugger automatically.|Requires manual debugging configuration.|
|**Environment Consistency**|Depends on IDE configuration.|Consistent with Maven build lifecycle.|
```sh
mvn spring=boot:run
```


# 4. What is the purpose of @SpringBootApplication annotation in a Spring Boot application?

```java
@SpringBootApplication
```

The `@SpringBootApplication` annotation is a key annotation in Spring Boot that simplifies the configuration and setup of a Spring Boot application. It combines three commonly used annotations into one, making it easier to create and run a Spring Boot application with minimal configuration.

---

### **Purpose and Components of `@SpringBootApplication`**

1. **Combination of Three Annotations**: The `@SpringBootApplication` annotation is a shorthand for the following three annotations:
    
    - **`@Configuration`**:
        - Indicates that the class is a source of bean definitions for the application context.
    - **`@EnableAutoConfiguration`**:
        - Enables Spring Boot’s auto-configuration mechanism, which automatically configures Spring components based on the classpath and beans defined in the application.
    - **`@ComponentScan`**:
        - Enables component scanning, which automatically detects and registers Spring beans (e.g., `@Component`, `@Service`, `@Repository`, and `@Controller`) in the specified package and its sub-packages.
    
    ```java
    @Target(ElementType.TYPE)
    @Retention(RetentionPolicy.RUNTIME)
    @Documented
    @Inherited
    @SpringBootConfiguration
    @EnableAutoConfiguration
    @ComponentScan
    public @interface SpringBootApplication {
    }
    ```
    

---

### **Key Responsibilities of `@SpringBootApplication`**

2. **Auto-Configuration**:
    
    - Automatically configures the application based on dependencies in the classpath. For example:
        - If `spring-web` is present, it configures a `DispatcherServlet`.
        - If `spring-data-jpa` is present, it configures a JPA entity manager.
3. **Component Scanning**:
    
    - Automatically scans the package where the annotated class is located and its sub-packages for Spring components.
4. **Entry Point for the Application**:
    
    - It marks the class as the main entry point for the Spring Boot application, often used with the `main` method to run the application.

---

### **Example of Usage**

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MySpringBootApplication {

    public static void main(String[] args) {
        SpringApplication.run(MySpringBootApplication.class, args);
    }
}
```

5. **`SpringApplication.run`**:
    
    - Starts the Spring Boot application, setting up the Spring ApplicationContext, scanning for components, and starting embedded servers (e.g., Tomcat).
6. **Component Scanning**:
    
    - If the `MySpringBootApplication` class is in the package `com.example`, all sub-packages (e.g., `com.example.service`, `com.example.controller`) are scanned for Spring components.

---

### **Customizing Behavior**

You can customize the behavior of `@SpringBootApplication` by excluding specific auto-configurations or defining a custom scan path.

7. **Excluding Auto-Configurations**:
    
    ```java
    @SpringBootApplication(exclude = DataSourceAutoConfiguration.class)
    public class MyApplication {
    }
    ```
    
8. **Custom Component Scan**:
    
    ```java
    @SpringBootApplication
    @ComponentScan(basePackages = "com.example.custom")
    public class MyApplication {
    }
    ```
    

---

### **Benefits of `@SpringBootApplication`**

9. **Simplifies Configuration**:
    - Reduces boilerplate code by combining multiple annotations.
10. **Easy to Use**:
    - Automatically handles complex configurations.
11. **Entry Point**:
    - Clearly indicates the main class for starting the application.

---

### **Conclusion**

The `@SpringBootApplication` annotation is central to building and running Spring Boot applications. By combining essential annotations, it streamlines the development process and minimizes the need for manual configuration, allowing developers to focus on business logic.


# 5. What is Auto configuration in Spring Boot?

Spring Boot auto-configuration is a feature that automatically configures a Spring application based on the dependencies in the project's classpath.
### **Example of Auto-Configuration**

1. **Adding a Dependency**:
    
    - Adding the dependency `spring-boot-starter-web` automatically configures a web application:
        - Registers a **DispatcherServlet**.
        - Sets up an embedded web server (e.g., Tomcat).
        - Configures default error handling.



