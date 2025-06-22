### Producer Consumer Problem
>Two threads, a producer and a consumer, share a common, fixed-size buffer as a queue. The producer's job is to generate data and put it into the buffer, while the consumer's job is to consume the data from the buffer. The problem is to make sure that that producer won't produce data if the buffer is full, and the consumer won't consume data if the buffer is empty.

#### Implementation 

```java
public class SharedResource {  
  
    private Queue<Integer> sharedBuffer;  
    private int sharedBufferSize;  
  
    public SharedResource(int sharedBufferSize) {  
        sharedBuffer = new LinkedList<Integer>();  
        this.sharedBufferSize = sharedBufferSize;  
    }  
  
    // synchronized puts the monitor lock  
    public synchronized void addItem(int item) throws Exception {  
        // If Buffer is full, wait for the consumer to consume items...  
        while (sharedBuffer.size() == sharedBufferSize) {  
            System.out.println("[" + Thread.currentThread().getName() + "] Buffer is full, waiting for consumer");  
            wait();  
        }        sharedBuffer.add(item);  
        System.out.println("[" + Thread.currentThread().getName() + "] Item added: " + item+", Notifying the consumer that there are items to consume now");  
        notify();  
    }  

    public synchronized void consumeItem() throws Exception {  
        // If Buffer is empty, wait for the producer to produce items...  
        while (sharedBuffer.isEmpty()) {  
            System.out.println("[" + Thread.currentThread().getName() + "] Buffer is empty, waiting for producer to produce items...");  
            wait();  
        }        System.out.println("[" + Thread.currentThread().getName() + "] Attempting to consume item...");  
        int item = sharedBuffer.poll();  
        System.out.println("[" + Thread.currentThread().getName() + "] Item consumed: " + item);  
        // Notify the producer that there is space in the buffer now  
        notify();  
    }
}

// main class
public static void main(String[] args) {  
  
    SharedResource sharedResource = new SharedResource(3);  
  
    Thread producerThread = new Thread(() -> {  
        try {  
           for (int i = 0; i < 10; i++) {  
               sharedResource.addItem(i);  
           }        } catch (Exception e) {  
            // handle exception here  
        }  
    }, "Producer");  
  
    Thread consumerThread = new Thread(() ->{  
        try {  
            for (int i = 0; i < 10; i++) {  
                sharedResource.consumeItem();  
            }        } catch (Exception e) {  
            throw new RuntimeException(e);  
        }    }, "Consumer");  
  
    producerThread.start();  
    consumerThread.start();  
}
```

#### Output
```
[Producer] Item added: 0, Notifying the consumer that there are items to consume now
[Producer] Item added: 1, Notifying the consumer that there are items to consume now
[Producer] Item added: 2, Notifying the consumer that there are items to consume now
[Producer] Buffer is full, waiting for consumer
[Consumer] Attempting to consume item...
[Consumer] Item consumed: 0
[Consumer] Attempting to consume item...
[Consumer] Item consumed: 1
[Consumer] Attempting to consume item...
[Consumer] Item consumed: 2
[Consumer] Buffer is empty, waiting for producer to produce items...
[Producer] Item added: 3, Notifying the consumer that there are items to consume now
[Producer] Item added: 4, Notifying the consumer that there are items to consume now
[Producer] Item added: 5, Notifying the consumer that there are items to consume now
[Producer] Buffer is full, waiting for consumer
[Consumer] Attempting to consume item...
[Consumer] Item consumed: 3
[Consumer] Attempting to consume item...
[Consumer] Item consumed: 4
[Consumer] Attempting to consume item...
[Consumer] Item consumed: 5
[Consumer] Buffer is empty, waiting for producer to produce items...
[Producer] Item added: 6, Notifying the consumer that there are items to consume now
[Producer] Item added: 7, Notifying the consumer that there are items to consume now
[Producer] Item added: 8, Notifying the consumer that there are items to consume now
[Producer] Buffer is full, waiting for consumer
[Consumer] Attempting to consume item...
[Consumer] Item consumed: 6
[Consumer] Attempting to consume item...
[Consumer] Item consumed: 7
[Consumer] Attempting to consume item...
[Consumer] Item consumed: 8
[Consumer] Buffer is empty, waiting for producer to produce items...
[Producer] Item added: 9, Notifying the consumer that there are items to consume now
[Consumer] Attempting to consume item...
[Consumer] Item consumed: 9

Process finished with exit code 0
```


---
### Why Stop, Resume, Suspended method is deprecated?

- **Stop:** Terminates the thread abruptly, no lock release, no resource clean up happens.
- **Suspended:** Put the Thread on hold (suspended) for temporarily. no lock release.
- **Resume:** Used to resume the execution of suspended thread.
>These operations could led to issues like deadlock.


### What is Thread Join?
- When join method is invoked on a thread object. Current thread will be blocked and waits for the specific thread to finish.
- It is helpful when we want to coordinate between threads or to ensure we complete certain tasks before moving ahead.

Here's a **simple and clear example** demonstrating the use of `Thread.join()`:

---
#### ✅ **Purpose of `join()`**

- `join()` ensures that one thread **waits for the completion** of another.
- Useful when one thread must finish before the next can proceed.
    

---
#### 💡 **Example**

```java
public class JoinExample {
    public static void main(String[] args) {
        Thread worker = new Thread(() -> {
            System.out.println("Worker thread starting...");
            try {
                Thread.sleep(2000); // Simulate some work
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("Worker thread finished.");
        });

        worker.start(); // Start the worker thread

        try {
            // Main thread waits for worker to finish
            worker.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Main thread continues after worker thread is done.");
    }
}
```

---
#### 📝 **Output**

```
Worker thread starting...
Worker thread finished.
Main thread continues after worker thread is done.
```

---
#### 🔍 Summary

- `worker.start()` starts the worker thread.
- `worker.join()` pauses the **main thread** until the **worker thread finishes**.
- Without `join()`, the main thread might continue before the worker finishes.
---
### 🧵 What is **Thread Priority** in Java?

In Java, **Thread Priority** is a number that tells the **scheduler** how important a thread is compared to other threads. It **hints** to the JVM thread scheduler which thread should be executed first **when multiple threads are waiting to run**.

---
#### 📊 Priority Range

Java threads can have a priority between:

- `Thread.MIN_PRIORITY` = `1`
- `Thread.NORM_PRIORITY` = `5` _(default)_
- `Thread.MAX_PRIORITY` = `10`

---
#### ⚙️ Setting Thread Priority

```java
Thread t1 = new Thread(() -> {
    System.out.println("Running thread");
});
t1.setPriority(Thread.MAX_PRIORITY); // Set priority to 10
t1.start();
```

You can also get the priority using:

```java
int p = t1.getPriority();
System.out.println("Thread priority: " + p);
```

---
#### 🤔 Does it Guarantee Execution Order?

**No.** Thread priority is just a **suggestion** to the scheduler. The **actual behavior is platform-dependent** and varies by JVM and OS.

Even a thread with a higher priority **is not guaranteed** to run before lower-priority threads.

---
#### 🧠 When to Use Thread Priority?

- When you want to **give more CPU time** to important tasks.
- In real-time or performance-sensitive applications (but rarely used in modern systems due to OS-level thread management).

---
#### 🧪 Example: Multiple Threads with Different Priorities

```java
public class ThreadPriorityDemo {
    public static void main(String[] args) {
        Runnable task = () -> {
            System.out.println(Thread.currentThread().getName() +
                    " with priority " + Thread.currentThread().getPriority());
        };

        Thread t1 = new Thread(task, "Low Priority Thread");
        Thread t2 = new Thread(task, "Normal Priority Thread");
        Thread t3 = new Thread(task, "High Priority Thread");

        t1.setPriority(Thread.MIN_PRIORITY);    // 1
        t2.setPriority(Thread.NORM_PRIORITY);   // 5
        t3.setPriority(Thread.MAX_PRIORITY);    // 10

        t1.start();
        t2.start();
        t3.start();
    }
}
```

---
#### ✅ Summary

|Priority Constant|Value|Meaning|
|---|---|---|
|`Thread.MIN_PRIORITY`|`1`|Lowest importance|
|`Thread.NORM_PRIORITY`|`5`|Default priority|
|`Thread.MAX_PRIORITY`|`10`|Highest importance|

---

### Daemon Thread
> Something which is running ASYNC
> Daemon thread is alive till any one user thread is alive.

A **Daemon Thread** in Java is a **background thread** that runs **in support of user (non-daemon) threads**. It provides services to user threads and **does not prevent the JVM from exiting** when all user threads have finished.

---
#### 🧠 Key Characteristics

- **Low-priority** background thread.
- JVM **exits automatically** once all **user threads** finish, even if daemon threads are running.
- Common use: **Garbage Collector**, **Background tasks**, **Loggers**, **Watchdogs**.
---
#### 🧪 Example of a Daemon Thread

```java
public class DaemonThreadExample {
    public static void main(String[] args) {
        Thread daemon = new Thread(() -> {
            while (true) {
                System.out.println("Daemon thread running...");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        daemon.setDaemon(true); // Mark as daemon
        daemon.start();

        try {
            Thread.sleep(3000); // Let main thread run for 3 seconds
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Main thread exiting...");
        // JVM will exit now; daemon thread also stops
    }
}
```

---
#### 🧵 Output:

```
Daemon thread running...
Daemon thread running...
Daemon thread running...
Main thread exiting...
```

➡️ After this, **daemon thread stops automatically**, even though its loop was infinite.

---
#### ⚠️ Important Notes

- Call `setDaemon(true)` **before** starting the thread.
- Once started, a thread’s daemon status **cannot be changed**.
- If all user threads are done, **JVM forcibly kills** daemon threads.

---
#### ✅ Summary Table

|Feature|Daemon Thread|User Thread|
|---|---|---|
|Role|Background/support task|Main application logic|
|JVM exit condition|Dies when all user threads finish|Keeps JVM alive|
|Priority|Usually lower|Normal or higher|
|Example|Garbage Collector|Main method, Worker threads|

---
