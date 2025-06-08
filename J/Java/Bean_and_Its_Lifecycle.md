## What is Bean?
```
Bean is a Java Object, which is managed by Spring Container (also known as IOC Container).
```
```
IOC Container: contains all the beans which get created and also manage them.
```

### Two ways to create beans
1. @Component Annotation
2. @Bean Annotation

### How Spring boot find these Beans?
1. Using the `@ComponentScan` annotation, it will scan the specified package and sub-package for classes annotated with @Component, @Service etc.
2. Through Explicit defining of bean via `@Bean` annotation in `@Configuration` class.

### At what time, these beans get created
1. Eagerness
	1. Beans get created, when we start the application.
	2. For ex: Beans with Singleton Scope are Eagerly initialized.
2. Lazy
	1. Beans get created Lazily, means when they actually needed.
	2. For ex: Beans with Scope like Prototype etc. are Lazily initialized.
	3. Use `@Lazy` Annotation

## What Is the IoC Container in Spring Boot?

The IoC (Inversion of Control) container is a fundamental component of the Spring Framework, including Spring Boot. It is responsible for managing the lifecycle, configuration, and dependencies of application objects, which are known as _beans_[1](https://docs.spring.io/spring-framework/docs/3.2.x/spring-framework-reference/html/beans.html)[3](https://www.codingshuttle.com/spring-boot-handbook/spring-ioc-container).

## **Core Responsibilities of the IoC Container**

- **Object Creation:** The container creates instances of beans defined in the application.
    
- **Dependency Injection:** It automatically injects dependencies into these beans, either through constructors, properties, or methods, based on configuration or annotations[1](https://docs.spring.io/spring-framework/docs/3.2.x/spring-framework-reference/html/beans.html)[3](https://www.codingshuttle.com/spring-boot-handbook/spring-ioc-container).
    
- **Lifecycle Management:** The container manages the entire lifecycle of beans, from instantiation to destruction[3](https://www.codingshuttle.com/spring-boot-handbook/spring-ioc-container).
    
- **Configuration Management:** It uses metadata (XML, annotations, or Java configuration) to understand how beans should be created and wired together[3](https://www.codingshuttle.com/spring-boot-handbook/spring-ioc-container).
    

## **How IoC Works in Spring Boot**

- In a traditional application, developers manually create and manage objects and their dependencies, leading to tightly coupled code.
    
- With IoC, this control is "inverted"—the Spring container takes over the responsibility of creating objects and injecting their dependencies, resulting in loosely coupled, modular, and easily testable code[3](https://www.codingshuttle.com/spring-boot-handbook/spring-ioc-container)[2](https://www.simplilearn.com/tutorials/spring-tutorial/spring-ioc-container).
    

## **Types of IoC Containers in Spring**

|Container Type|Description|Typical Use Case|
|---|---|---|
|BeanFactory|The basic IoC container, provides basic dependency injection features[1](https://docs.spring.io/spring-framework/docs/3.2.x/spring-framework-reference/html/beans.html)[3](https://www.codingshuttle.com/spring-boot-handbook/spring-ioc-container).|Lightweight/simple applications|
|ApplicationContext|Extends BeanFactory, adds advanced features (event propagation, AOP, internationalization, etc.)|Most Spring/Spring Boot projects|

- In most Spring Boot applications, **ApplicationContext** is used as the IoC container because of its rich feature set[1](https://docs.spring.io/spring-framework/docs/3.2.x/spring-framework-reference/html/beans.html)[3](https://www.codingshuttle.com/spring-boot-handbook/spring-ioc-container).
## **How the IoC Container Gets Configuration**

- The container can be configured using:
    
    - XML files
    - Java annotations (e.g., `@Component`, `@Autowired`)
    - Java-based configuration classes (using `@Configuration` and `@Bean`)[3](https://www.codingshuttle.com/spring-boot-handbook/spring-ioc-container)
## **Example**

```java
@Configuration
public class AppConfig {
    @Bean
    public Engine engine() {
        return new Engine();
    }

    @Bean
    public Car car() {
        return new Car(engine());
    }
}
```

In this example, the IoC container creates and manages the `Engine` and `Car` beans. When a `Car` is needed, the container injects an `Engine` instance automatically[3](https://www.codingshuttle.com/spring-boot-handbook/spring-ioc-container).

>The `@Configuration` annotation in Spring is used to mark a class as a source of bean definitions. It indicates that the class will provide configurations for the Spring IoC (Inversion of Control) container.
## **Benefits**

- **Decoupling:** Objects are less dependent on each other, making code easier to maintain and extend.
- **Testability:** Components can be tested independently by mocking dependencies.
- **Modularity:** Each component focuses on its own logic, not on managing dependencies[3](https://www.codingshuttle.com/spring-boot-handbook/spring-ioc-container).
---

**In summary:**  
The IoC container in Spring Boot is the part of the framework that creates, configures, and manages the lifecycle and dependencies of application objects (beans) using dependency injection. It enables loose coupling, easier testing, and more maintainable code by shifting control of object management from the developer to the framework[1](https://docs.spring.io/spring-framework/docs/3.2.x/spring-framework-reference/html/beans.html)[3](https://www.codingshuttle.com/spring-boot-handbook/spring-ioc-container).