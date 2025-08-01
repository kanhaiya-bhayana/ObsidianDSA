```csharp
using System;
using System.Threading;

// 1. Native Singleton (Not thread-safe)
public sealed class NativeSingleton
{
    private static NativeSingleton _instance;
    private NativeSingleton() { }
    public static NativeSingleton GetInstance()
    {
        if (_instance == null)
        {
            _instance = new NativeSingleton();
        }
        return _instance;
    }
}

// 2. Thread-Safe Singleton (Double-checked locking)
public sealed class ThreadSafeSingleton
{
    private static ThreadSafeSingleton _instance;
    private static readonly object _lock = new object();
    private ThreadSafeSingleton() { }
    public static ThreadSafeSingleton GetInstance()
    {
        if (_instance == null)
        {
            lock (_lock)
            {
                if (_instance == null)
                {
                    _instance = new ThreadSafeSingleton();
                }
            }
        }
        return _instance;
    }
}

// 3. Generic Singleton Factory (Thread-safe using Lazy<T>)
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
                throw new InvalidOperationException($"Singleton<{typeof(T).Name}> has not been initialized.");
            return _instance.Value;
        }
    }
}

// Example class for factory
public class CustomLogger
{
    private readonly string _name;
    public CustomLogger(string name) => _name = name;
    public void Log() => Console.WriteLine("Logging from " + _name);
}

public class App
{
    public static void Main(string[] args)
    {
        // Example 1: Native Singleton (Not thread-safe)
        var native1 = NativeSingleton.GetInstance();
        var native2 = NativeSingleton.GetInstance();
        Console.WriteLine($"NativeSingleton: {native1.GetHashCode()} == {native2.GetHashCode()}");

        // Example 2: Thread-Safe Singleton
        ThreadSafeSingleton ts1 = null, ts2 = null;
        Thread t1 = new Thread(() => ts1 = ThreadSafeSingleton.GetInstance());
        Thread t2 = new Thread(() => ts2 = ThreadSafeSingleton.GetInstance());
        t1.Start(); t2.Start();
        t1.Join(); t2.Join();
        Console.WriteLine($"ThreadSafeSingleton: {ts1.GetHashCode()} == {ts2.GetHashCode()}");

        // Example 3: Singleton Factory with CustomLogger
        Thread p1 = new Thread(() =>
        {
            SingletonFactory<CustomLogger>.Initialize(() => new CustomLogger("MyLogger"));
            var logger = SingletonFactory<CustomLogger>.Instance;
            Console.WriteLine($"Factory Logger: {logger.GetHashCode()}");
            logger.Log();
        });
        Thread p2 = new Thread(() =>
        {
            SingletonFactory<CustomLogger>.Initialize(() => new CustomLogger("MyLogger"));
            var logger = SingletonFactory<CustomLogger>.Instance;
            Console.WriteLine($"Factory Logger: {logger.GetHashCode()}");
            logger.Log();
        });
        p1.Start(); p2.Start();
        p1.Join(); p2.Join();
    }
}
```