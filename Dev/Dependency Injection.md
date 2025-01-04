### Introduction

- Dependency Injection (DI) is a software design pattern that helps developers build better software. 
- It allows us to develop loosely-coupled code that is easy to maintain. Dependency Injection reduces the hard-coded dependencies among your classes by injecting those dependencies at run time instead of design time technically.
- This pattern improves code modularity, testability, and maintainability. 
### Key Concepts:

1. **Dependency**: A class or service that another class requires to function.
2. **Injection**: The process of supplying the dependency to the dependent object.
3. **Inversion of Control (IoC)**: The principle of transferring the control of object creation and dependency management to a container or framework (like the **ASP.NET Core** dependency injection container).

### Benefits of Dependency Injection

- **Loose Coupling**: Reduces direct dependencies between components, making code easier to maintain and test.
- **Improved Testability**: Facilitates unit testing by allowing mock objects or stubs to be injected.
- **Reusability**: Promotes the use of interfaces and reduces duplicate code.
- **Scalability**: Simplifies swapping or extending dependencies without altering the dependent classes.
### Dependency Injection in .NET Core

In **ASP.NET Core**, DI is built into the framework, making it easy to register and resolve dependencies using the built-in IoC container.


#### Create Project

```sh
dotnet new webapi -o MyWebAPI
```


### Services Lifecycle
The **lifecycle of services** in a dependency injection (DI) container in C# defines how and when instances of the service are created and managed. The three most commonly used lifecycles in DI are **Singleton**, **Transient**, and **Scoped**.

---

### **1. Singleton**

- **Description**: A **single instance** of the service is created and shared across the entire application lifetime.
- **When is it created?**  
    At the first request for the service (lazy initialization) or when the application starts (if explicitly initialized).
- **Usage**: Use when the service:
    - Maintains a global state.
    - Is expensive to create (e.g., database connection pools, configuration providers).
    - Must be reused across the application to ensure consistency.

**Behavior**:

- Only **one instance** exists regardless of how many times the service is injected.
- Shared across all requests and threads.

**Code Example**:

```csharp
builder.Services.AddSingleton<IMyService, MyService>();
```

**Key Points**:

- **Global Instance**: Shared across all HTTP requests.
- Avoid storing user-specific or request-specific data in a singleton, as this could lead to race conditions or incorrect data being shared between requests.

---

### **2. Transient**

- **Description**: A **new instance** of the service is created **every time** it is requested.
- **When is it created?**  
    Whenever the service is requested (via constructor injection, method injection, or directly from the container).
- **Usage**: Use when the service:
    - Is lightweight and stateless.
    - Does not need to maintain a global or request-wide state.

**Behavior**:

- Every consumer gets a **new instance** of the service.
- Not shared between requests or injections.

**Code Example**:

```csharp
builder.Services.AddTransient<IMyService, MyService>();
```

**Key Points**:

- Ideal for **stateless services**.
- Can be expensive if creating the service involves heavy computation or resource initialization.

---

### **3. Scoped**

- **Description**: A **new instance** of the service is created **once per HTTP request**. The same instance is reused for the **lifetime of the request**.
- **When is it created?**  
    Once per HTTP request and disposed of when the request ends.
- **Usage**: Use when the service:
    - Needs to maintain state for the duration of a request (e.g., database context, user session).

**Behavior**:

- Same instance is shared within a single request.
- Different requests will have different instances.

**Code Example**:

```csharp
builder.Services.AddScoped<IMyService, MyService>();
```

**Key Points**:

- Works well for **request-specific operations**, such as database contexts (e.g., `DbContext` in Entity Framework).
- Ensures thread safety for the request lifecycle.

---

### **Comparison Chart**

|**Lifecycle**|**Instance Count**|**Scope**|**Usage Examples**|
|---|---|---|---|
|**Singleton**|One instance shared across the app.|Global|Configuration, logging, caching.|
|**Transient**|New instance every time it's requested.|Per injection/request.|Lightweight and stateless services.|
|**Scoped**|One instance per HTTP request.|Per HTTP request lifecycle.|Database contexts, user request state.|

---

### **Best Practices**

1. **Singleton**:
    
    - Avoid using for services that depend on user-specific or request-specific data.
    - Be cautious of threading issues (e.g., mutable state in a singleton).
2. **Transient**:
    
    - Use for simple, lightweight, and stateless services.
    - Be aware of the cost if the service is expensive to instantiate.
3. **Scoped**:
    
    - Ideal for request-specific services like database contexts.
    - Use when state needs to persist across multiple operations in a request.

---

### **Visualizing Lifecycles in an HTTP Request**

1. **Singleton**:
    - One instance is shared across all requests:
        
        ```
        Request 1 --> Singleton Instance A
        Request 2 --> Singleton Instance A
        ```
        
2. **Transient**:
    - Each request gets a new instance, and even within a request, each injection results in a new instance:
        
        ```
        Request 1 --> Transient Instance A
        Request 2 --> Transient Instance B
        ```
        
3. **Scoped**:
    - One instance is shared within the same request but isolated across different requests:
        
        ```
        Request 1 --> Scoped Instance A
        Request 2 --> Scoped Instance B
        ```
        

---

This understanding helps in designing services with appropriate lifecycles based on their role and usage in the application.


#### [Keyed Services](/Keyed Services)
