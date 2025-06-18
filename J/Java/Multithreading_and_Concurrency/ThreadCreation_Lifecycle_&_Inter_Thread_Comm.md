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

