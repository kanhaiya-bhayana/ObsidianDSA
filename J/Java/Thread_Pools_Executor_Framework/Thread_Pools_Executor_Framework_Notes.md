
### #ThreadPoolExecutorFramework
### **ThreadPool in Java**

A **ThreadPool** in Java is a pool (collection) of worker threads that efficiently execute a large number of asynchronous tasks without the overhead of creating a new thread for each task.

---
### ✅ **Why Use ThreadPool?**

Creating threads is expensive in terms of time and memory. When you have many short-lived tasks, using a ThreadPool helps by:
- **Reusing threads** instead of creating new ones.    
- **Limiting the number of concurrent threads**, preventing resource exhaustion.
- **Improving performance and scalability.**

---
### 🔧 **How It Works**
1. You submit a task (usually implementing `Runnable` or `Callable`).
2. A worker thread from the pool picks it up and executes it.
3. When done, the thread returns to the pool, ready for another task.
    
---
### ⚙️ **Core Classes and Interfaces**
- `Executor`: Interface for executing tasks. 
- `ExecutorService`: A more advanced sub-interface of `Executor`.
- `Executors`: Utility class to create thread pools.
- `ThreadPoolExecutor`: The most configurable implementation.

---
### ✨ **Creating ThreadPool with Executors**

```java
ExecutorService executor = Executors.newFixedThreadPool(5); // 5 threads

for (int i = 0; i < 10; i++) {
    executor.execute(() -> {
        System.out.println("Thread: " + Thread.currentThread().getName());
    });
}

executor.shutdown(); // Gracefully shut down after tasks finish
```

---
### 🧩 **Types of Thread Pools (via `Executors` factory)**

| Method                          | Description                                 |
| ------------------------------- | ------------------------------------------- |
| `newFixedThreadPool(int n)`     | Fixed number of threads.                    |
| `newCachedThreadPool()`         | Unbounded pool, reuses idle threads.        |
| `newSingleThreadExecutor()`     | Single worker thread.                       |
| `newScheduledThreadPool(int n)` | Executes tasks after delay or periodically. |

---

### ThreadPoolExecuter Syntax
Here’s the **syntax** and structure of `ThreadPoolExecutor` in Java, including all parameters:
---
### ✅ **ThreadPoolExecutor Constructor Syntax**

```java
ThreadPoolExecutor executor = new ThreadPoolExecutor(
    int corePoolSize,
    int maximumPoolSize,
    long keepAliveTime, // to keep this param work, make sure to enable the boolean flag "allowCoreThreadTimeOut", else there is no use of this param. (Default behavior is false)
    TimeUnit unit,
    BlockingQueue<Runnable> workQueue,
    ThreadFactory threadFactory,         // optional
    RejectedExecutionHandler handler     // optional
);
```

---
### 🔍 **Parameter Explanation**

| Parameter                    | Description                                                                     |
| ---------------------------- | ------------------------------------------------------------------------------- |
| `corePoolSize`               | Minimum number of threads to keep in the pool (even if idle).                   |
| `maximumPoolSize`            | Maximum number of threads allowed in the pool.                                  |
| `keepAliveTime`              | Time to keep extra (non-core) threads alive when idle.                          |
| `unit`                       | Time unit for keepAliveTime (`TimeUnit.SECONDS`, etc).                          |
| `workQueue`                  | Queue to hold waiting tasks (`ArrayBlockingQueue`, `LinkedBlockingQueue`, etc). |
| `threadFactory` _(optional)_ | Factory to create new threads. Use default if null.                             |
| `handler` _(optional)_       | Strategy when task cannot be executed (queue full, max threads reached).        |

---
### 🧪 **Basic Example**

```java
import java.util.concurrent.*;

public class ThreadPoolExample {
    public static void main(String[] args) {
        ThreadPoolExecutor executor = new ThreadPoolExecutor(
            2,                      // corePoolSize
            4,                      // maximumPoolSize
            10,                     // keepAliveTime
            TimeUnit.SECONDS,       // time unit
            new ArrayBlockingQueue<>(2), // workQueue
            Executors.defaultThreadFactory(),
            new ThreadPoolExecutor.AbortPolicy() // Rejection policy
        );

        // Submit tasks
        for (int i = 0; i < 6; i++) {
            executor.execute(() -> {
                System.out.println("Running: " + Thread.currentThread().getName());
            });
        }

        executor.shutdown();
    }
}
```

---
### ⚠️ Rejection Policies (when queue full + max threads active)

- `AbortPolicy`: Throws `RejectedExecutionException` (default).
- `CallerRunsPolicy`: Runs the task in the caller’s thread.
- `DiscardPolicy`: Silently discards the task.
- `DiscardOldestPolicy`: Discards the oldest unhandled task and retries.
---
### 🧠 **Customizing with ThreadPoolExecutor**

```java
ThreadPoolExecutor executor = new ThreadPoolExecutor(
    2, 4, // corePoolSize, maximumPoolSize
    10, TimeUnit.SECONDS, // keepAliveTime
    new ArrayBlockingQueue<>(10) // task queue
);
```

You can also set rejection policies, like:
- `AbortPolicy` (default),
- `CallerRunsPolicy`,
- `DiscardPolicy`,
- `DiscardOldestPolicy`.
---
### 🔚 Summary
- ThreadPool improves performance by managing a pool of worker threads.
- Prevents resource exhaustion by limiting concurrent threads.
- Java provides `Executors` for easy setup, or `ThreadPoolExecutor` for full control.

## LifeCycle of a ThreadPoolExecutor

The **lifecycle of a `ThreadPoolExecutor`** can be broken down into distinct **states** and **transitions**, just like a finite state machine. These states govern how the executor handles tasks and shuts down.

---
### 🧭 **ThreadPoolExecutor Lifecycle States**

```text
          +--------------+
          |   RUNNING    |<-----------------+
          +--------------+                  |
                 |                          |
    executor.shutdown()                     |
                 v                          |
           +------------+                   |
           |  SHUTDOWN  |<-----+            |
           +------------+      |            |
                 |             |            |
         (queue is empty)      |            |
                 v             |            |
          +-------------+      |            |
          | TERMINATED  |      |            |
          +-------------+      |            |
                               |            |
executor.shutdownNow()         |            |
     or task throws error      |            |
           while running       |            |
                 v             |            |
           +-------------+     |            |
           |  STOP       |-----+------------+
           +-------------+                  
                 |
       (all tasks halted)
                 v
          +-------------+
          | TERMINATED  |
          +-------------+
```

---
### 🏁 **State Definitions**

|State|Description|
|---|---|
|**RUNNING**|Accepts new tasks and processes queued tasks.|
|**SHUTDOWN**|Does not accept new tasks. Continues running queued tasks.|
|**STOP**|Does not accept new tasks, **discards queued tasks**, and interrupts running tasks.|
|**TERMINATED**|All tasks finished, all threads stopped, and executor is completely shut down.|

---
### 🔧 **Methods Affecting Lifecycle**

|Method|Effect|
|---|---|
|`execute(Runnable task)`|Submits a task if state is RUNNING.|
|`shutdown()`|Initiates a graceful shutdown.|
|`shutdownNow()`|Attempts immediate shutdown; interrupts threads.|
|`isShutdown()`|Returns true if shutdown or shutdownNow was called.|
|`isTerminated()`|True if executor is fully terminated.|
|`awaitTermination()`|Blocks until termination or timeout.|

---
### ⚠️ Notes

- Once an executor reaches `TERMINATED`, it cannot be restarted.
- Tasks submitted **after shutdown** will be **rejected**.
- Using `shutdown()` is preferred for clean termination.
- Use `awaitTermination()` if you need to wait for graceful shutdown.

---

## Custom Example

```java
import java.util.concurrent.*;  
  
public class Main {  
    public static void main(String[] args) {  
        ThreadPoolExecutor executor = new ThreadPoolExecutor(  
                2,  
                4,  
                10, TimeUnit.MINUTES,  
                new ArrayBlockingQueue<>(2),  
                new CustomThreadFactory(),  
                new CustomRejectHandler()  
        );  
        
        executor.allowCoreThreadTimeOut(true);
        
        for (int i = 0; i < 7; i++) {  
            executor.submit(() -> {  
                try {  
                    Thread.sleep(5000);  
                }catch (InterruptedException e) {  
                    // handle exception here  
                }  
                System.out.println("Task processed by: " + Thread.currentThread().getName());  
	        });        
        }        
        executor.shutdown();  
    }  
}  
  
class CustomThreadFactory implements ThreadFactory {  
    @Override  
    public Thread newThread(Runnable r) {  
        Thread thread = new Thread(r);  
        thread.setPriority(Thread.NORM_PRIORITY);  
        thread.setDaemon(false);  
        return thread;  
    }
}  
  
class CustomRejectHandler implements RejectedExecutionHandler {  
    @Override  
    public void rejectedExecution(Runnable r, ThreadPoolExecutor executor) {  
        System.out.println("Task Rejected: " + r.toString());  
    }
}
```