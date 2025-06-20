## Thread Creation Ways
1. Implementing `Runnable` interface
2. Extending `Thread` class

### Why 2 ways of Creation?
- A class can implement more than 1 interface
- A class can extend only 1 class


Here’s a well-structured summary of the **two main ways to create threads in Java**:

---
## ✅ Ways to Create a Thread in Java

### 1. **Implementing `Runnable` Interface**

#### ✅ Code Example:

```java
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Thread using Runnable is running.");
    }
}

public class Main {
    public static void main(String[] args) {
        Thread thread = new Thread(new MyRunnable());
        thread.start();
    }
}
```

#### ✅ Notes:

- Preferred for **better OOP design** (since Java supports single inheritance).
    
- The class can **extend another class** and still be a thread.
    
- Can be reused with different `Thread` objects.
    

---
### 2. **Extending `Thread` Class**

#### ✅ Code Example:

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread using Thread class is running.");
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread thread = new MyThread();
        thread.start();
    }
}
```

#### ✅ Notes:

- Easier and quicker for **simple use cases**.
    
- Less flexible — your class **can't extend any other class**.
    

---
### 🔁 Comparison Table:

|Feature|Runnable Interface|Thread Class|
|---|---|---|
|Inheritance|Allows extending another class|Cannot extend another class|
|Reusability|High (can be shared with Thread)|Low|
|Separation of task & thread|Yes|No|
|Code reuse|Better|Limited|

---

## Thread Life Cycle
![[thread_life_cycle.png]]

Here's a **visual representation of the Thread Life Cycle in Java** (in text diagram format):

---
### 🧵 Java Thread Life Cycle Diagram:

```
             +----------------+
             |    New         |
             | (Created but   |
             |  not started)  |
             +----------------+
                     |
                     | start()
                     v
             +----------------+
             |   Runnable     |
             | (Ready to run, |
             |  waiting for   |
             |  CPU)          |
             +----------------+
                     |
                     |   (Thread scheduler picks it)
                     v
             +----------------+
             |   Running      |
             | (Thread is     |
             |  executing)    |
             +----------------+
                 /     |     \
     sleep(),   /      |      \  wait(), yield()
   join(), etc /       |       \ 
              v        v        v
       +----------+  +--------------+
       | Blocked  |  |  Waiting      |
       | (I/O etc)|  | (Until notify)|
       +----------+  +--------------+
                          |
                    notify()/notifyAll()
                          v
                    +--------------+
                    | Runnable     | (again waiting for CPU)
                    +--------------+

                     |
                     | run() completes or stop()
                     v
             +----------------+
             | Terminated     |
             | (Dead)         |
             +----------------+
```

---

### 🔄 States Summary:

| State             | Description                                                                                                                                                                                                                                                              |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **New**           | Thread created but not started.                                                                                                                                                                                                                                          |
| **Runnable**      | Ready to run, waiting for CPU time.                                                                                                                                                                                                                                      |
| **Running**       | Actually executing.                                                                                                                                                                                                                                                      |
| **Blocked**       | Different scenarios where runnalbe thread goes into the Blocking state:<br>- I/O: like reading from a file or database<br>- Lock acquired: if thread want to lock on a resource which is  locked by other thread, it has to wait.<br><br>> Release all the MONITOR LOCKS |
| **Waiting**       | - Thread goes into this state when we call the wait() method, makes it non runnable.<br>- Its goes back to runnable, once we call notify() or notifyAll() method.<br>- Release all the MONITOR LOCKS.                                                                    |
| **Timed Waiting** | - Waiting for a period (e.g., `sleep(1000)`, `join(timeout)`).<br>- Do not release any MONITOR LOCK.                                                                                                                                                                     |
| **Terminated**    | - Finished execution or aborted.<br>- Life of thread is completed, it can not be  started back again.                                                                                                                                                                    |

---
## Monitor Lock
- It helps to make sure that only 1 thread goes inside the particular section of code (a synchronized block or method).

```java
/**
 * Example: Demonstration of Monitor Lock in Java using synchronized, wait(), and notifyAll().
 *
 * This example simulates a basic Producer-Consumer scenario using a shared resource.
 * - The producer thread waits for 2 seconds and then adds an item.
 * - The consumer thread tries to consume the item.
 * - If the item is not available, the consumer waits (releases monitor lock).
 * - When the producer adds an item, it calls notifyAll() to wake up waiting threads.
 * - Proper synchronization ensures only one thread accesses the critical section at a time.
 *
 * Key Concepts:
 * - synchronized methods for acquiring the monitor lock on the shared object.
 * - wait() causes the current thread to wait and release the lock.
 * - notifyAll() wakes up all waiting threads on that object.
 * - while-loop used around wait() to guard against spurious wakeups.
 *
 * Output is printed with thread names to clearly observe the coordination between threads.
 */

public class SharedResource {
    private boolean itemAvailable = false;

    // synchronized puts the monitor lock
    public synchronized void addItem() {
        itemAvailable = true;
        System.out.println("[" + Thread.currentThread().getName() + "] Item added. Notifying waiting threads...");
        notifyAll();
    }

    public synchronized void consumeItem() {
        System.out.println("[" + Thread.currentThread().getName() + "] Attempting to consume item...");

        while (!itemAvailable) {
            try {
                System.out.println("[" + Thread.currentThread().getName() + "] No item. Waiting...");
                wait(); // Releases monitor lock
            } catch (InterruptedException e) {
                System.err.println("[" + Thread.currentThread().getName() + "] Interrupted while waiting");
                Thread.currentThread().interrupt(); // restore interrupted status
            }
        }

        System.out.println("[" + Thread.currentThread().getName() + "] Item consumed.");
        itemAvailable = false;
    }
}


// -----------------------------------------------------------------------------------------------------
// -------------------------------------------Main Class------------------------------------------------------
// -----------------------------------------------------------------------------------------------------

public class Main {
    public static void main(String[] args) {

        SharedResource sharedResource = new SharedResource();

        Thread producerThread = new Thread(() -> {
            try {
                Thread.sleep(2000); // Simulate some delay
            } catch (InterruptedException e) {
                System.err.println("[Producer] Interrupted during sleep");
                Thread.currentThread().interrupt();
            }
            sharedResource.addItem();
        }, "Producer");

        Thread consumerThread = new Thread(sharedResource::consumeItem, "Consumer");

        producerThread.start();
        consumerThread.start();
    }
}

```
#### Output:
```
[Consumer] Attempting to consume item...
[Consumer] No item. Waiting...
[Producer] Item added. Notifying waiting threads...
[Consumer] Item consumed.
```
