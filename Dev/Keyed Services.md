### YouTube Video Script: Keyed Services in .NET

#### Opening (0:00 - 0:20)

**[On screen: Presenter with a welcoming smile]**

- "Hey everyone! Welcome back to my channel, where we dive deep into everything .NET and software development. Today, we’re exploring a really interesting and powerful concept in .NET Core—Keyed Services. By the end of this video, you’ll understand what they are, when to use them, and how to implement them in your projects. Let’s get started!"

#### Introduction to Keyed Services (0:21 - 1:30)

**[On screen: Slides or code snippets]**

- "Keyed Services are a way to register and resolve multiple implementations of the same interface in the dependency injection container using a unique key."
- "Imagine you have a scenario where you need different implementations for a Payment service—one for UPI, one for CreditCard, and maybe even one for PayPal or ApplePay. Keyed Services let you elegantly manage these implementations without relying on workarounds."
- "Keyed Services provide clarity, scalability, and maintainability when you have multiple strategies or variations of a service."

#### Why Use Keyed Services? (1:31 - 2:30)

**[On screen: Comparison chart]**

- "Let’s compare it with some common alternatives like named services or manually resolving dependencies based on conditions."
- "With Keyed Services:
    - You avoid hardcoding logic to differentiate implementations.
    - Your code stays clean and adheres to SOLID principles, especially the Dependency Inversion Principle."
- "Plus, you can extend your application easily without making massive changes to your existing codebase."

#### Workarounds Without Keyed Services (2:31 - 3:30)

**[On screen: Presenter discussing alternatives with visual aids]**

- "If you don’t use Keyed Services, here are some common workarounds:

1. **Named Services:**
    
    - Register services with specific names and resolve them using those names.
    
    ```csharp
    builder.Services.AddTransient<ILoggerService, FileLoggerService>("File");
    builder.Services.AddTransient<ILoggerService, DatabaseLoggerService>("Database");
    ```
    
    - However, resolving named services often requires additional libraries or manual management, which can become cumbersome.
2. **Manual Resolution Logic:**
    
    - Use a factory pattern or conditional statements to resolve the appropriate service.
    
    ```csharp
    public ILoggerService GetLogger(string type)
    {
        return type switch
        {
            "File" => new FileLoggerService(),
            "Database" => new DatabaseLoggerService(),
            _ => throw new NotSupportedException()
        };
    }
    ```
    
    - While this works, it introduces tight coupling and is less maintainable.
3. **Service Locator Pattern:**
    
    - Use a service locator to resolve dependencies dynamically.
    - This is generally discouraged as it violates the Dependency Inversion Principle and can make testing harder."

#### Setting Up Keyed Services (3:31 - 6:30)

**[On screen: Code editor with step-by-step demonstration]**

- "Now, let’s implement Keyed Services in a .NET Core application. Here’s the step-by-step guide:

1. **Create the Interface**
    
    ```csharp
    public interface ILoggerService
    {
        void Log(string message);
    }
    ```
    
2. **Implement Multiple Variations**
    
    ```csharp
    public class FileLoggerService : ILoggerService
    {
        public void Log(string message)
        {
            Console.WriteLine($"File Logger: {message}");
        }
    }
    
    public class DatabaseLoggerService : ILoggerService
    {
        public void Log(string message)
        {
            Console.WriteLine($"Database Logger: {message}");
        }
    }
    ```
    
3. **Register Services with Keys**
    
    ```csharp
    builder.Services.AddTransient<ILoggerService, FileLoggerService>("File");
    builder.Services.AddTransient<ILoggerService, DatabaseLoggerService>("Database");
    ```
    
4. **Resolve the Services**
    
    ```csharp
    public class LoggingController
    {
        private readonly IKeyedService<string, ILoggerService> _loggerResolver;
    
        public LoggingController(IKeyedService<string, ILoggerService> loggerResolver)
        {
            _loggerResolver = loggerResolver;
        }
    
        public void LogToService(string key, string message)
        {
            var logger = _loggerResolver[key];
            logger?.Log(message);
        }
    }
    ```
    
5. **Using the Controller**
    
    ```csharp
    var loggingController = new LoggingController(serviceProvider);
    loggingController.LogToService("File", "This is a file log.");
    loggingController.LogToService("Database", "This is a database log.");
    ```
    

#### Best Practices and Tips (6:31 - 7:30)

**[On screen: Presenter with bullet points on the screen]**

- "Always keep your keys meaningful. Use descriptive strings like 'File' or 'Database' instead of ambiguous keys."
- "Avoid overusing Keyed Services. Use them only when you genuinely have distinct implementations of the same interface."
- "Consider using extension methods for clean and reusable service registration."

#### Closing (7:31 - 8:00)

**[On screen: Presenter wrapping up]**

- "That’s it for today’s video! We covered what Keyed Services are, why they’re useful, workarounds when not using them, and how to implement them in your .NET Core projects. If you found this helpful, please give it a thumbs up and subscribe to the channel for more such content."
- "Have any questions or specific topics you’d like me to cover next? Drop them in the comments below. Thanks for watching, and I’ll see you in the next video!"

#### Outro (8:01 - 8:10)

**[On screen: Channel logo with upbeat music]**