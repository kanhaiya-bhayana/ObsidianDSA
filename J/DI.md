## DI

Dependency Injection is a specific implementation of IoC, where dependencies are injected into objects rather than the objects creating their own dependencies. Highlight that IoC promotes loose coupling and enhances modularity by decoupling the execution of tasks from their implementation.

## IOC

Inversion of Control (IoC) in Spring is `a design principle that manages the creation and configuration of objects, and their dependencies`. It's a core part of the Spring Framework.
### @Component

The @Component annotation in Spring Boot is a core part of the Spring Framework and is used to mark a Java class as a Spring-managed component. This means that Spring will automatically detect and register the class as a bean in the application context during component scanning.

##### When to Use @Component:
Use @Component when you want Spring to manage the lifecycle of a class and make it available for dependency injection. For more specialized roles (e.g., service, repository, controller), consider using the appropriate stereotype annotations.




## When you have multiple implementations of an interface in a Spring application, Spring needs a way to resolve ambiguity during dependency injection. By default, Spring will throw a `NoUniqueBeanDefinitionException` if it cannot determine which bean to inject. There are several ways to handle this situation:

---

### 1. **Use `@Primary` Annotation**
Mark one of the implementations as the primary bean using the `@Primary` annotation. Spring will choose the primary bean by default unless specified otherwise.

```java
@Component
public interface MyService {
    void performTask();
}

@Component
@Primary
public class MyServiceImpl1 implements MyService {
    public void performTask() {
        System.out.println("MyServiceImpl1 executed");
    }
}

@Component
public class MyServiceImpl2 implements MyService {
    public void performTask() {
        System.out.println("MyServiceImpl2 executed");
    }
}

// Usage
@Component
public class MyConsumer {

    private final MyService myService;

    @Autowired
    public MyConsumer(MyService myService) {
        this.myService = myService;
    }

    public void execute() {
        myService.performTask(); // Will use MyServiceImpl1 because of @Primary
    }
}
```

---

### 2. **Use `@Qualifier` Annotation**
Explicitly specify which implementation to use by applying the `@Qualifier` annotation.

```java
@Component
public class MyServiceImpl1 implements MyService {
    public void performTask() {
        System.out.println("MyServiceImpl1 executed");
    }
}

@Component
public class MyServiceImpl2 implements MyService {
    public void performTask() {
        System.out.println("MyServiceImpl2 executed");
    }
}

// Usage
@Component
public class MyConsumer {

    private final MyService myService;

    @Autowired
    public MyConsumer(@Qualifier("myServiceImpl2") MyService myService) {
        this.myService = myService;
    }

    public void execute() {
        myService.performTask(); // Will use MyServiceImpl2
    }
}
```

---

### 3. **Use a `Map` or `List` for All Beans**
Inject all implementations as a `Map` or `List`, allowing dynamic resolution at runtime.

```java
@Component
public class MyServiceImpl1 implements MyService {
    public void performTask() {
        System.out.println("MyServiceImpl1 executed");
    }
}

@Component
public class MyServiceImpl2 implements MyService {
    public void performTask() {
        System.out.println("MyServiceImpl2 executed");
    }
}

// Usage
@Component
public class MyConsumer {

    private final Map<String, MyService> myServices;

    @Autowired
    public MyConsumer(Map<String, MyService> myServices) {
        this.myServices = myServices;
    }

    public void execute(String beanName) {
        myServices.get(beanName).performTask(); // Dynamically resolves the bean by name
    }
}
```

---

### 4. **Use Custom Annotations**
Create a custom qualifier annotation to avoid specifying the string-based bean name.

```java
@Qualifier
@Target({ElementType.FIELD, ElementType.PARAMETER, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface MyServiceType {
    String value();
}

@Component
@MyServiceType("type1")
public class MyServiceImpl1 implements MyService {
    public void performTask() {
        System.out.println("MyServiceImpl1 executed");
    }
}

@Component
@MyServiceType("type2")
public class MyServiceImpl2 implements MyService {
    public void performTask() {
        System.out.println("MyServiceImpl2 executed");
    }
}

// Usage
@Component
public class MyConsumer {

    private final MyService myService;

    @Autowired
    public MyConsumer(@MyServiceType("type1") MyService myService) {
        this.myService = myService;
    }

    public void execute() {
        myService.performTask(); // Will use MyServiceImpl1
    }
}
```

---

### 5. **Programmatic Resolution**
Use the `ApplicationContext` to programmatically retrieve the desired bean.

```java
@Component
public class MyConsumer {

    private final ApplicationContext context;

    @Autowired
    public MyConsumer(ApplicationContext context) {
        this.context = context;
    }

    public void execute(String beanName) {
        MyService myService = context.getBean(beanName, MyService.class);
        myService.performTask(); // Resolves the bean programmatically
    }
}
```

---

### Summary:
- Use `@Primary` for a default implementation.
- Use `@Qualifier` to explicitly specify the desired implementation.
- Inject all beans as a `List` or `Map` for dynamic resolution.
- Create custom qualifiers for better readability.
- Use programmatic resolution for advanced cases. 

Choose the approach that best suits your application's requirements.


## What is Factory Pattern?

The Factory Pattern is a creational design pattern used in object-oriented programming. Its main purpose is to provide a way to create objects without specifying the exact class of the object that will be created. Instead of instantiating objects directly using the new keyword, the pattern relies on a factory method to create the objects, which promotes loose coupling and better maintainability.



The provided code demonstrates a **Simple Factory Pattern** implementation in the context of shapes (`Circle` and `Rectangle`). To explain this in the **context of a notification system**, we can draw an analogy between the `Shape` interface and a `Notification` interface, with concrete classes implementing different types of notifications (e.g., `EmailNotification`, `SMSNotification`). The `ShapeFactory` would be replaced with a `NotificationFactory` that creates instances of specific notification types based on a string input.

Here’s how the code can be adapted to a **Notification System**:

---

### Updated Code for Notification System

#### 1. **Interface: Notification**
```java
// Notification Interface
public interface Notification {
    void notifyUser();
}
```

#### 2. **Concrete Implementations: Email and SMS**
```java
// Email Notification Implementation
public class EmailNotification implements Notification {
    public void notifyUser() {
        System.out.println("Sending Email Notification");
    }
}

// SMS Notification Implementation
public class SMSNotification implements Notification {
    public void notifyUser() {
        System.out.println("Sending SMS Notification");
    }
}
```

#### 3. **Factory Class: NotificationFactory**
```java
// Factory Class
public class NotificationFactory {
    public Notification getNotification(String notificationType) {
        if (notificationType == null) {
            return null;
        }
        if (notificationType.equalsIgnoreCase("EMAIL")) {
            return new EmailNotification();
        } else if (notificationType.equalsIgnoreCase("SMS")) {
            return new SMSNotification();
        }
        return null;
    }
}
```

#### 4. **Client Code: Main**
```java
// Client Code
public class Main {
    public static void main(String[] args) {
        NotificationFactory notificationFactory = new NotificationFactory();

        // Get EmailNotification instance
        Notification emailNotification = notificationFactory.getNotification("EMAIL");
        emailNotification.notifyUser(); // Output: Sending Email Notification

        // Get SMSNotification instance
        Notification smsNotification = notificationFactory.getNotification("SMS");
        smsNotification.notifyUser(); // Output: Sending SMS Notification
    }
}
```
---

### Benefits in the Context of Notifications:
1. **Encapsulation**:
   - The factory encapsulates the creation logic for different notification types, making the client code simpler and more maintainable.

2. **Flexibility**:
   - New notification types (e.g., `PushNotification`) can be added easily without modifying the client code.

3. **Loose Coupling**:
   - The client is decoupled from the specific implementation of notifications, relying only on the `Notification` interface.

---

This design is ideal for situations where different types of notifications need to be created dynamically based on some condition (e.g., user preference, external configuration, or runtime input). The factory handles all the complexity, leaving the client code clean and focused on using the notifications.



## What is Builder Pattern?

The **Builder Pattern** is a **creational design pattern** used to construct complex objects step by step. It allows you to create different representations of the same object while keeping the construction process consistent. This pattern is particularly useful when an object has numerous attributes, some of which are optional, or when constructing the object involves multiple steps.

---

### Key Features of the Builder Pattern:
1. **Step-by-Step Construction**:
   - Constructs the object piece by piece, ensuring the process is manageable and clear.

2. **Immutable Objects**:
   - Often used to create immutable objects where the builder sets all properties before building the object.

3. **Customization**:
   - Easily allows for creating different representations or configurations of the same object.

4. **Readable Code**:
   - The fluent interface of the Builder Pattern improves the readability of object creation code.

---

### Example: Without Builder Pattern (Problem)

Creating an object with multiple attributes can result in unreadable and error-prone code, especially if there are multiple constructors.

```java
public class Car {
    private String engine;
    private int wheels;
    private String color;

    public Car(String engine, int wheels, String color) {
        this.engine = engine;
        this.wheels = wheels;
        this.color = color;
    }
}

// Client Code
Car car = new Car("V8", 4, "Red");
```

If the number of attributes increases or some are optional, this approach becomes difficult to manage.

---

### Example: With Builder Pattern (Solution)

#### 1. **Car Class**
```java
public class Car {
    private String engine;
    private int wheels;
    private String color;

    // Private constructor to force object creation via Builder
    private Car(CarBuilder builder) {
        this.engine = builder.engine;
        this.wheels = builder.wheels;
        this.color = builder.color;
    }

    // Static nested Builder class
    public static class CarBuilder {
        private String engine;
        private int wheels;
        private String color;

        public CarBuilder setEngine(String engine) {
            this.engine = engine;
            return this;
        }

        public CarBuilder setWheels(int wheels) {
            this.wheels = wheels;
            return this;
        }

        public CarBuilder setColor(String color) {
            this.color = color;
            return this;
        }

        public Car build() {
            return new Car(this);
        }
    }

    @Override
    public String toString() {
        return "Car [engine=" + engine + ", wheels=" + wheels + ", color=" + color + "]";
    }
}
```

#### 2. **Client Code**
```java
public class Main {
    public static void main(String[] args) {
        Car car = new Car.CarBuilder()
                          .setEngine("V8")
                          .setWheels(4)
                          .setColor("Red")
                          .build();

        System.out.println(car); // Output: Car [engine=V8, wheels=4, color=Red]
    }
}
```

---

### Advantages of the Builder Pattern:
1. **Readability**:
   - Provides a clear and readable way to construct complex objects with multiple attributes.

2. **Flexibility**:
   - You can construct different variations of the object without creating multiple constructors.

3. **Encapsulation**:
   - Encapsulates the object construction logic in the builder, keeping the main class clean.

4. **Immutability**:
   - Often used with immutable objects to ensure the final object state cannot be changed after creation.

5. **Avoid Constructor Overloading**:
   - Eliminates the need for numerous constructors, which can lead to confusion.

---

### Real-World Examples:
1. **StringBuilder in Java**:
   - Used for creating and modifying strings.
2. **Lombok's @Builder**:
   - A popular Java library for reducing boilerplate code, which includes a builder feature.
3. **Database Queries**:
   - Builders are often used for constructing SQL queries dynamically.

---

### When to Use the Builder Pattern:
- When constructing an object requires many parameters.
- When some parameters are optional or have default values.
- When object creation is complex and involves multiple steps.
- When you want to create immutable objects. 

This pattern is particularly useful in scenarios like creating configuration objects, UI elements, or any case where readability and maintainability are important.
