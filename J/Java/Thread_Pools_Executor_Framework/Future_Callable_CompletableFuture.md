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