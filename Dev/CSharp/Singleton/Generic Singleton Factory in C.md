# 🧠 Generic Singleton Factory in C#

This document explains how to create and use a **Generic, Lazy, and Thread-safe Singleton Factory** in C#. This approach allows creating a singleton for any class, even ones with parameterized constructors, with full thread safety.

---
## 🔧 Why Do We Need a Generic Singleton?

The traditional singleton pattern:
- Is tightly coupled to a specific class
- Is repetitive for each class
- Doesn't support constructors with arguments
- Needs custom thread safety

With a **generic factory**, we can:
- Reuse the singleton logic
- Provide custom instantiation logic via `Func<T>`
- Ensure thread-safe, lazy initialization

---
## ✅ Implementation

```csharp
public static class SingletonFactory<T>
{
    private static Lazy<T> _instance;
    private static readonly object _lock = new object();
    private static bool _isInitialized = false;

    public static void Initialize(Func<T> factory)
    {
        if (_isInitialized) return;

        lock (_lock)
        {
            if (_isInitialized) return;

            _instance = new Lazy<T>(factory, LazyThreadSafetyMode.ExecutionAndPublication);
            _isInitialized = true;
        }
    }

    public static T Instance
    {
        get
        {
            if (!_isInitialized)
            {
                throw new InvalidOperationException($"Singleton<{typeof(T).Name}> has not been initialized.");
            }
            return _instance.Value;
        }
    }
}
```

---
## 📌 How to Use It

### 1️⃣ Define a Class

```csharp
public class Logger
{
    private readonly string _name;
    public Logger(string name) => _name = name;
    public void Log() => Console.WriteLine($"Logger: {_name}");
}
```
### 2️⃣ Initialize the Singleton (once)

```csharp
SingletonFactory<Logger>.Initialize(() => new Logger("AppLogger"));
```
### 3️⃣ Use It Anywhere

```csharp
var logger = SingletonFactory<Logger>.Instance;
logger.Log(); // Logger: AppLogger
```

---
## 🧠 What is `Func<T>`?

`Func<T>` is a delegate that represents a method returning a value of type `T`, with no input parameters.

```csharp
Func<string> sayHi = () => "Hi!";
Console.WriteLine(sayHi()); // Hi!
```

In our factory:

```csharp
Initialize(Func<T> factory)
```

You pass a function (or lambda) that knows how to **create the object**, and the factory will call it the **first time** the instance is accessed.

---
## ✅ Key Benefits

|Feature|Description|
|---|---|
|Generic|Works for any class `T`|
|Thread-safe|Uses locking and `Lazy<T>`|
|Lazy-loaded|Object created only when first needed|
|Flexible|Accepts constructor parameters|
|Clean API|`Instance` access is simple and safe|

---
## ⚠️ Notes

- You **must call `Initialize()` once** before accessing `Instance`
- `Initialize()` is **idempotent** — calling it multiple times does nothing after the first

---
## 🛠️ Optional Enhancements

- Auto-initialize via `Activator.CreateInstance<T>()` for types with parameterless constructors
- Support for named singletons (per key)
- Singleton disposal support for `IDisposable` types

Let the team know if you'd like those additions!

---
## 📣 Summary

This `SingletonFactory<T>` gives your team a clean, reusable, and extensible way to manage singletons in modern C# applications — without boilerplate or thread-safety concerns.

# Better Example with clean code

```csharp
public static class SingletonFactory<T>
{
    private static Lazy<T> _instance;
    private static readonly object _lock = new object();
    private static bool _isInitialized = false;

    public static void Initialize(Func<T> factory)
    {
        if (_isInitialized) return;

        lock (_lock)
        {
            if (_isInitialized) return;

            _instance = new Lazy<T>(factory, LazyThreadSafetyMode.ExecutionAndPublication);
            _isInitialized = true;
        }
    }

    public static T Instance
    {
        get
        {
            if (!_isInitialized)
            {
                throw new InvalidOperationException($"Singleton<{typeof(T).Name}> has not been initialized.");
            }
            return _instance.Value;
        }
    }
}

public class CustomLogger
{
    private readonly string _name;
    public CustomLogger(string name) => _name = name;
    public void Log () => System.Console.WriteLine("Logging from " + _name);
}

public class App
{
    public static void Main(string[] args)
    {
        Thread p1 = new Thread(() =>
        {
            SingletonFactory<CustomLogger>.Initialize(() => new CustomLogger("MyLogger"));
            var logger = SingletonFactory<CustomLogger>.Instance;
            System.Console.WriteLine(logger.GetHashCode());
            logger.Log();
        });

        Thread p2 = new Thread(() =>
        {
            SingletonFactory<CustomLogger>.Initialize(() => new CustomLogger("MyLogger"));
            var logger = SingletonFactory<CustomLogger>.Instance;
            System.Console.WriteLine(logger.GetHashCode());
            logger.Log();
        });

        p1.Start();
        p2.Start();

        p1.Join();
        p2.Join();
    }

}
```