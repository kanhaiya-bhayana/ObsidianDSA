
### **Process in Java**

A **process** is an independent executing program with its own memory space and system resources. In Java, each application runs in its own process, managed by the operating system.

#### **Key Characteristics**:

1. **Isolation**: Processes are isolated; one process cannot directly access another process's memory.
2. **Resources**: Each process has its own heap, stack, and runtime environment.
3. **Heavyweight**: Creating and managing processes involves significant overhead.

#### **Example**:

Running two Java applications (`App1` and `App2`) creates two separate processes.

---

### **Thread in Java**

A **thread** is a lightweight sub-process that shares the process's memory and resources. It enables concurrent execution within a process.

#### **Key Characteristics**:

1. **Shared Memory**: Threads of the same process share the heap but have separate stacks.
2. **Lightweight**: Creating threads is less resource-intensive compared to processes.
3. **Concurrency**: Multiple threads can execute independently but within the same process.

#### **Example**:

In a Java application, spawning multiple threads can handle tasks like file I/O, network calls, or computations concurrently.

---

### **Java Implementation of Threads**

1. **Extending `Thread` Class**:
    
    ```java
    class MyThread extends Thread {
        public void run() {
            System.out.println("Thread is running.");
        }
    }
    public class Main {
        public static void main(String[] args) {
            MyThread t = new MyThread();
            t.start(); // Starts a new thread
        }
    }
    ```
    
2. **Implementing `Runnable` Interface**:
    
    ```java
    class MyRunnable implements Runnable {
        public void run() {
            System.out.println("Thread is running.");
        }
    }
    public class Main {
        public static void main(String[] args) {
            Thread t = new Thread(new MyRunnable());
            t.start(); // Starts a new thread
        }
    }
    ```
    

---

### **Comparison of Process vs. Thread**

| **Aspect**        | **Process**                                 | **Thread**                    |
| ----------------- | ------------------------------------------- | ----------------------------- |
| **Definition**    | Independent program                         | Sub-process within a program  |
| **Memory**        | Separate memory space                       | Shared memory within process  |
| **Overhead**      | High (heavyweight)                          | Low (lightweight)             |
| **Communication** | Inter-process communication (e.g., sockets) | Shared memory access          |
| **Use Case**      | Running separate programs                   | Concurrent tasks in a program |