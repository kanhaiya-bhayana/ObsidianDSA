# 🧠 Singleton Pattern in C# (Thread-safe & Native Implementations)

This document describes how to implement the **Singleton Pattern** in C# using both native and thread-safe techniques. This approach is useful when you do **not** need a custom factory method but still want to guarantee **single instance** and **thread safety**.

---
## 🔧 What is a Singleton?

A **Singleton** is a design pattern that ensures a class has **only one instance**, and provides a **global point of access** to that instance.

---
## 🛠️ 1. Basic Singleton (NOT Thread-safe)

```csharp
public sealed class Singleton
{
    private static Singleton _instance;
    private Singleton() { }

    public static Singleton GetInstance()
    {
        if (_instance == null)
        {
            _instance = new Singleton();
        }
        return _instance;
    }
}
```

### ❌ Issues:

- In a multithreaded application, two threads could enter `GetInstance()` simultaneously and create **two instances**.
---
## ✅ 2. Thread-safe Singleton using `lock`

```csharp
public sealed class Singleton
{
    private static Singleton _instance;
    private static readonly object _lock = new object();

    private Singleton() { }

    public static Singleton GetInstance()
    {
        lock (_lock)
        {
            if (_instance == null)
            {
                _instance = new Singleton();
            }
        }
        return _instance;
    }
}
```

### ✔ Safe, but slightly slower due to locking on every call.

---
## 🚀 3. Double-Checked Locking

```csharp
public sealed class Singleton
{
    private static Singleton _instance;
    private static readonly object _lock = new object();

    private Singleton() { }

    public static Singleton GetInstance()
    {
        if (_instance == null)
        {
            lock (_lock)
            {
                if (_instance == null)
                {
                    _instance = new Singleton();
                }
            }
        }
        return _instance;
    }
}
```

### ✅ Benefits:

- Thread-safe
- Avoids lock overhead after the instance is initialized

### ⚠️ Note:

- Use only with `volatile` keyword in older .NET versions to avoid instruction reordering issues

---
## 💡 4. Eager Initialization (Thread-safe by default)

```csharp
public sealed class Singleton
{
    private static readonly Singleton _instance = new Singleton();

    private Singleton() { }

    public static Singleton GetInstance()
    {
        return _instance;
    }
}
```

### ✅ Thread-safe and simple

### ❌ Instance is always created, even if never used

---
## 🔄 5. Lazy Based Singleton (Recommended)

```csharp
public sealed class Singleton
{
    private static readonly Lazy<Singleton> _lazyInstance = new Lazy<Singleton>(() => new Singleton());

    private Singleton() { }

    public static Singleton GetInstance()
    {
        return _lazyInstance.Value;
    }
}
```

### ✅ Advantages:

- Thread-safe by default
- Instance is created only when accessed (lazy)
- Clean and modern syntax

---
## ✅ Summary Table

|Version|Thread-safe|Lazy-loaded|Lock Overhead|Notes|
|---|---|---|---|---|
|Basic Singleton|❌|✅|❌|Not safe for multi-threading|
|lock-based|✅|✅|✅|Safe but slower|
|Double-checked locking|✅|✅|Minimal|Most common in manual pattern|
|Eager initialization|✅|❌|❌|Simple, but always created|
|Lazy|✅|✅|❌|✅ Best practice|

---
## 🧪 Example Usage

```csharp
public class App
{
    public static void Main()
    {
        var s1 = Singleton.GetInstance();
        var s2 = Singleton.GetInstance();

        Console.WriteLine(Object.ReferenceEquals(s1, s2)); // True
    }
}
```

---
## 📣 Recommendation

Use **`Lazy<T>` based singleton** for most use cases. It's simple, efficient, and safe without needing locks or manual code.

---
If you need support for custom constructors or dependency injection, consider using a **Generic Singleton Factory** instead (see separate documentation).