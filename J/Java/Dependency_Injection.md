### Why Need?

1. Remember the example of `User` and `Order` Class, direct dependencies.
2. It breaks `Dependency Inversion` rule of SOLID principle.
	1. Here `DI` says that DO NOT depend on the concrete implementation, rather depends on abstraction.
3. We can achieve `Dependency Inversion Principle` Through Dependency Injection.
### What is DI?

- Using DI, we can make our class independent of its dependencies.
- It helps to remove the dependency on concrete implementation and inject the dependencies from external sources.

> `@Autowired`, first look for a bean of the required type.
> 	If bean found, Spring will inject it.
### Different ways of Injection and which one is better and most used in the Industry?

- [ ] Field Injection
- [ ] Setter Injection
- [ ] Constructor Injection ** (recommended)
      
### ✅ **Dependency Injection (DI) in Spring Boot**
**Dependency Injection** is a design pattern where one object supplies the dependencies of another object. In **Spring Boot**, DI is a core concept that helps with **loose coupling, easier testing, and better code management**.

---

### 🔧 **Why Use DI in Spring Boot?**
- Removes tight coupling between components
- Makes unit testing easier (you can mock dependencies)
- Promotes reusable and maintainable code
- Manages the lifecycle of beans
---
### 🧩 **Types of Dependency Injection in Spring Boot**
Spring supports **three** main types of Dependency Injection:

---
#### 1. **Constructor Injection** ✅ (Recommended)

- Dependencies are provided through the class constructor.
- Promotes immutability and makes it easy to test.
- Spring 4.3+ automatically injects dependencies if the class has only one constructor.

```java
@Component
public class UserService {
    private final UserRepository userRepository;

    // Constructor Injection
    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

---
#### 2. **Setter Injection**
- Spring uses setter methods to inject dependencies.
- Allows for optional dependencies and re-setting if needed.

```java
@Component
public class UserService {
    private UserRepository userRepository;

    // Setter Injection
    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

---
#### 3. **Field Injection** ❌ (Not Recommended for production)

- Spring injects the dependency directly into the field using reflection.
- Shorter code, but harder to test and violates encapsulation.
    
```java
@Component
public class UserService {

    @Autowired
    private UserRepository userRepository;
}
```
---
### 🏷️ **How Does Spring Know What to Inject?**

Spring uses **annotations** to identify and inject dependencies:

|Annotation|Purpose|
|---|---|
|`@Component`, `@Service`, `@Repository`, `@Controller`|Marks class as a Spring-managed bean|
|`@Autowired`|Marks a constructor/setter/field for injection|
|`@Qualifier("beanName")`|Specifies which bean to inject when multiple are available|
|`@Primary`|Marks a bean as default when multiple candidates exist|
|`@Value("${config.value}")`|Injects value from properties or environment|

---
### ✅ **Best Practices**
- Prefer **constructor injection** for required dependencies.
- Use `@Autowired` on a **constructor** only when you have **multiple constructors**.
- Avoid field injection in production code for better testability.
- Use `@Qualifier` when multiple beans of the same type exist.

---
#               Common Issues when dealing with Dependency Injection
---

1. Circular Dependency
2. Unsatisfied Dependency
### 🔄 1. Circular Dependency Issue in Spring Boot
A circular dependency occurs when two or more beans depend on each other directly or indirectly, causing Spring to get stuck in an infinite loop while trying to instantiate them.

> Use the example of `User` and `Order`

### Fix - Circular Dependencies
1. Refactor the Code
2. `@Lazy` Initialization
3. `@PostConstruct`

### 2. ❌ **Multiple Beans, No `@Qualifier`** | Unsatisfied Dependency

FIX
1. Use `@Primary` annotation
2. Use `@Qualifier` annotation


