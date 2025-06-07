### ✅ What is `Future<?>` in Java?

`Future<?>` is a **generic interface** in Java that represents the **result of an asynchronous computation** — a task that will complete in the future.

It is returned when you submit a task to an `ExecutorService` (like a `ThreadPoolExecutor`) and allows you to:
- **Check if the task is complete**
- **Cancel the task**
- **Retrieve the result** once the task finishes
---
### 🧠 Method Supports

|Method|Purpose|
|---|---|
|`get()`|Waits for task to complete and gets result|
|`get(timeout, unit)`|Waits for limited time|
|`cancel(true/false)`|Attempts to stop the task|
|`isCancelled()`|Checks if the task was cancelled|
|`isDone()`|Checks if the task is finished|

---
### Example

```java
public static void main(String[] args) {  
   ThreadPoolExecutor executor = new ThreadPoolExecutor(  
            2,  
            4,  
            10, TimeUnit.MINUTES,  
            new ArrayBlockingQueue<>(2),  
            new CustomThreadFactory(),  
            new CustomRejectHandler()  
    );    executor.allowCoreThreadTimeOut(true);  
    
    Future<?> futureObj = executor.submit(() -> {  
        try {  
            Thread.sleep(5000);  
            System.out.println("this is the task, thread will execute it");  
        }        catch (InterruptedException e) {  
            // handle exception here  
        }  
    });  
    
    System.out.println("Is Done: " + futureObj.isDone());  
  
    try {  
        futureObj.get(2, TimeUnit.SECONDS);  
    } 
    catch (TimeoutException e) {  
        System.out.println("Timeout exception happened");  
    } 
    catch (Exception e) {  
    }  
    
    try {  
        futureObj.get();  
    }
    catch (Exception e) {  
    }
      
    System.out.println("Is Done: " + futureObj.isDone());  
    System.out.println("Is Cancelled: " + futureObj.isCancelled());  
  
    executor.shutdown();  
}
```

## Callable
### ✅ What is `Callable` in Java?

`Callable` is a **functional interface** in Java that represents a task that **returns a result** and may **throw a checked exception**.

It is part of the `java.util.concurrent` package and is often used with **ExecutorService** to run tasks asynchronously and get results via `Future`.

---

### 🔍 Key Differences from `Runnable`

|Feature|`Runnable`|`Callable<V>`|
|---|---|---|
|Returns value?|❌ No|✅ Yes (generic type `V`)|
|Throws checked exception?|❌ No|✅ Yes|
|Method|`void run()`|`V call() throws Exception`|

---

### 🔧 Callable Interface

```java
@FunctionalInterface
public interface Callable<V> {
    V call() throws Exception;
}
```

---

### 🧪 Example

```java
import java.util.concurrent.*;

public class CallableExample {
    public static void main(String[] args) throws Exception {
        ExecutorService executor = Executors.newSingleThreadExecutor();

        Callable<String> task = () -> {
            Thread.sleep(2000);
            return "Task Completed!";
        };

        Future<String> future = executor.submit(task); // Submits Callable

        System.out.println("Waiting for result...");
        String result = future.get(); // Blocks until task completes
        System.out.println("Result: " + result);

        executor.shutdown();
    }
}
```

---

### 🧠 Why Use Callable?

- To **return a result** from a thread
    
- To **handle exceptions** thrown in background tasks
    
- To **chain or combine** results using `Future`, `ExecutorService`, or `CompletableFuture`
    

---

### ✅ When to Use It

- Asynchronous operations (e.g., file download, computation)
    
- Batch processing tasks with results
    
- Timeout-based execution
    
- When you want error handling in the background task
    

---

### Callable` example with `ThreadPoolExecutor

Here's a full working example of using **`Callable` with `ThreadPoolExecutor`**, along with how to retrieve the result using **`Future`**.

---
### ✅ Example: `Callable` with `ThreadPoolExecutor`

```java
import java.util.concurrent.*;

public class CallableWithThreadPoolExecutor {
    public static void main(String[] args) {
        // Create a ThreadPoolExecutor with 2 core threads, 4 max, and a queue of size 2
        ThreadPoolExecutor executor = new ThreadPoolExecutor(
                2,
                4,
                10, TimeUnit.SECONDS,
                new ArrayBlockingQueue<>(2),
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.AbortPolicy()
        );

        // Create a Callable task
        Callable<String> task = () -> {
            Thread.sleep(2000); // simulate long task
            return "Callable Task Completed by: " + Thread.currentThread().getName();
        };

        // Submit the task to the executor and receive a Future
        Future<String> future = executor.submit(task);

        try {
            // Wait for the result
            System.out.println("Waiting for result...");
            String result = future.get(); // This will block until result is ready
            System.out.println("Result: " + result);
        } catch (InterruptedException | ExecutionException e) {
            e.printStackTrace();
        } finally {
            executor.shutdown();
        }
    }
}
```

---

### 🧠 How It Works
- **Callable** returns a value (`String` here).
- **submit(task)** returns a `Future<String>`.
- **future.get()** blocks and retrieves the result when done.
- **ThreadPoolExecutor** manages the thread that executes the task.

---
### 🚀 Output Example

```
Waiting for result...
Result: Callable Task Completed by: pool-1-thread-1
```
---

## CompletableFuture

### ✅ What is `CompletableFuture` in Java?

`CompletableFuture` is an advanced feature in Java (since **Java 8**) that represents a **future result** of an asynchronous computation — with **powerful ways to chain, compose, or handle tasks** once they complete.

It belongs to `java.util.concurrent` and is part of the **Completable Futures API**, offering **non-blocking, functional-style** programming for async workflows.

---
### 🔍 Core Idea

Unlike `Future`, which only blocks until a result is ready, **`CompletableFuture` is non-blocking** and supports:

|Feature|`Future`|`CompletableFuture`|
|---|---|---|
|Get result (blocking)|✅|✅|
|Set result manually|❌|✅|
|Combine multiple tasks|❌|✅|
|Chain callbacks|❌|✅|
|Exception handling|❌|✅|
|Async execution helpers|❌|✅|

---
### ⚙️ Common Methods

|Method|Description|
|---|---|
|`supplyAsync(Supplier<T>)`|Run async task that returns result|
|`runAsync(Runnable)`|Run async task without returning result|
|`thenApply(fn)`|Transform result when done|
|`thenAccept(Consumer)`|Consume result when done|
|`thenRun(Runnable)`|Run action after completion|
|`thenCombine()`|Combine two futures|
|`exceptionally(fn)`|Handle exceptions|
|`complete(value)`|Manually complete the future|
|`join()`|Get result, throw unchecked exceptions|

---

### 🧪 Example: Basic `CompletableFuture`

```java
import java.util.concurrent.*;

public class CompletableFutureExample {
    public static void main(String[] args) {
        CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
            try {
                Thread.sleep(2000); // Simulate delay
            } catch (InterruptedException e) {
                throw new IllegalStateException(e);
            }
            return "Hello from async!";
        });

        // Transform result after completion
        future.thenApply(result -> result + " [processed]")
              .thenAccept(System.out::println); // Final consumer

        System.out.println("Main thread continues...");
        
        // Optional: block and wait for result (not recommended in reactive apps)
        future.join();
    }
}
```

**Output:**

```
Main thread continues...
Hello from async! [processed]
```

---
### 💥 Exception Handling Example

```java
CompletableFuture<Integer> future = CompletableFuture.supplyAsync(() -> {
    if (true) throw new RuntimeException("Boom");
    return 42;
});

future.exceptionally(ex -> {
    System.out.println("Caught: " + ex.getMessage());
    return -1;
}).thenAccept(System.out::println);
```

---
### 🧠 When to Use `CompletableFuture`

Use it when you need:
- **Asynchronous task chaining**
- **Multiple futures to combine or race**
- **Non-blocking concurrency**
- Clean **error handling in async code**
- Alternative to traditional `Future` + `ExecutorService`

---
### You can use `CompletableFuture` with your own `ThreadPoolExecutor` instead of the default `ForkJoinPool.commonPool()` for better control over thread usage — especially important in production apps, Android, servers, or low-latency systems.

---

### ✅ Example: `CompletableFuture` with Custom `ThreadPoolExecutor`

```java
import java.util.concurrent.*;

public class CompletableFutureWithExecutor {
    public static void main(String[] args) {
        // Step 1: Create a ThreadPoolExecutor
        ThreadPoolExecutor executor = new ThreadPoolExecutor(
                2,
                4,
                10, TimeUnit.SECONDS,
                new ArrayBlockingQueue<>(2),
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.AbortPolicy()
        );

        // Step 2: Run CompletableFuture with this executor
        CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
            return "Task executed by: " + Thread.currentThread().getName();
        }, executor);  // <<< using your custom executor

        // Step 3: Add async transformation using the same executor
        future.thenApplyAsync(result -> {
            return result + " | transformed!";
        }, executor).thenAccept(System.out::println);

        System.out.println("Main thread continues...");

        // Step 4: Gracefully shutdown
        future.join(); // Wait for completion
        executor.shutdown();
    }
}
```

---

### 🔍 Key Differences

```java
// Default pool (common fork-join)
CompletableFuture.supplyAsync(() -> "data");

// Custom pool
CompletableFuture.supplyAsync(() -> "data", myExecutor);
```

---
### 📦 When to Use a Custom `ThreadPoolExecutor` with CompletableFuture?

- If you want to **limit concurrency**
- Avoid **blocking** or overloading the default `ForkJoinPool`
- Track and name threads (with `ThreadFactory`)
- Ensure **isolation** between different task groups
---
### Bonus: Submit Multiple `CompletableFutures` with Same Executor

```java
List<CompletableFuture<String>> futures = IntStream.range(0, 5)
    .mapToObj(i -> CompletableFuture.supplyAsync(() -> {
        return "Task " + i + " by " + Thread.currentThread().getName();
    }, executor))
    .toList();

CompletableFuture.allOf(futures.toArray(new CompletableFuture[0]))
    .thenRun(() -> System.out.println("All tasks done!"))
    .join();
```

---
