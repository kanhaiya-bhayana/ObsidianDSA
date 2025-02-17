## Top 50 - Java
# 1. What all does JVM Comprise of?
1. **Classloader:** A JVM subsystem responsible for loading class files into memory when program starts.
2. **Heap:** Memory area used for dynamic memory allocation, where objects and their data are stored during runtime.
3. Method Area: Stores class-level information like static variables, constant pools, and method code. It holds per-class structure.
4. Stack: Used to store method calls and local variables. Each method call creates a new frame on the stack, which is removed when the method finished.
5. Execute Engine: Reads and execute bytecode. It can interpret bytecode or use the JIT compiler to covert it into machine code for faster execution.
6. JNI (Java Native Interface): Allows Java to interact with native applications or libraries in other languages like C or C++ for platform-specific features.

# 2. What is Object-oriented programming? Is Java an object-oriented language?
**What is Object-Oriented Programming?**  
Object-Oriented Programming (OOP) is a programming paradigm based on the concept of "objects," which can contain data in the form of fields (attributes) and code in the form of methods (functions). OOP principles include:

1. **Encapsulation**: Bundling data and methods that operate on the data within an object.
2. **Inheritance**: Enabling one class to inherit properties and behavior from another.
3. **Polymorphism**: Allowing methods or functions to behave differently based on the object or input.
4. **Abstraction**: Hiding complex implementation details and exposing only the necessary features of an object.

**Is Java an Object-Oriented Language?**  
Yes, Java is primarily an object-oriented language. It supports OOP principles through its class-based structure and features like inheritance, encapsulation, polymorphism, and abstraction. However, Java also includes some non-object-oriented features like primitive data types, making it not purely object-oriented.


# 3. What do you understand by Aggregation in context of Java?
**Aggregation in Java**

Aggregation is a relationship between two classes where one class contains a reference to another class. It represents a "has-a" relationship, meaning one object (the whole) contains another object (the part), but the lifecycle of the part object is independent of the whole object.

For example:  
If a `Department` class has a reference to an `Employee` class, the `Department` "has-a" `Employee`. Even if the `Department` object is deleted, the `Employee` object may still exist.

**Characteristics of Aggregation**:

1. It is a weaker relationship compared to composition.
2. The child object can exist independently of the parent object.
3. Represented in UML with a hollow diamond.

**Example in Java**:

```java
class Employee {
    String name;
    int id;

    public Employee(String name, int id) {
        this.name = name;
        this.id = id;
    }
}

class Department {
    String name;
    List<Employee> employees;

    public Department(String name, List<Employee> employees) {
        this.name = name;
        this.employees = employees;
    }
}

public class Main {
    public static void main(String[] args) {
        Employee e1 = new Employee("Alice", 101);
        Employee e2 = new Employee("Bob", 102);
        List<Employee> employeeList = new ArrayList<>(List.of(e1, e2));

        Department department = new Department("HR", employeeList);

        System.out.println("Department: " + department.name);
        for (Employee e : department.employees) {
            System.out.println("Employee: " + e.name);
        }
    }
}
```

In this example, the `Department` class aggregates `Employee` objects, and the `Employee` objects exist independently of the `Department`.


# 4. Name the superclass in Java
In Java, the **`Object` class** is the superclass of all classes.

**Key Points about the `Object` Class**:

1. It is the root of the class hierarchy, meaning every class in Java either directly or indirectly inherits from the `Object` class.
2. If a class does not explicitly extend another class, it implicitly extends `Object`.

**Common Methods Provided by the `Object` Class**:

- **`toString()`**: Returns a string representation of the object.
- **`equals(Object obj)`**: Compares the object with another for equality.
- **`hashCode()`**: Returns a hash code value for the object.
- **`clone()`**: Creates and returns a copy of the object (if supported).
- **`getClass()`**: Returns the runtime class of the object.
- **`finalize()`**: Invoked by the garbage collector before the object is destroyed.
- **`wait()`, `notify()`, and `notifyAll()`**: Used for thread synchronization.

For example:

```java
public class Main {
    public static void main(String[] args) {
        Object obj = new Object();
        System.out.println(obj.toString()); // Prints the object's string representation
    }
}
```

# 5. Explain the difference between 'finally' and 'finalize' in Java
The **`finally`** block and the **`finalize()`** method in Java serve entirely different purposes:

###### **`finally`**

- **Purpose**: It is used in exception handling to execute code after a `try` block, regardless of whether an exception occurred or not.
- **When it is used**: For cleanup actions, such as closing files, releasing resources, or resetting variables.
- **Execution**: Always executed, unless the program exits via a `System.exit()` call or a fatal error occurs.
- **Syntax**:
    
    ```java
    try {
        // Code that may throw an exception
    } catch (Exception e) {
        // Handle the exception
    } finally {
        // Cleanup code, always executed
    }
    ```
    
- **Example**:
    
    ```java
    try {
        int result = 10 / 0; // This will throw an exception
    } catch (ArithmeticException e) {
        System.out.println("Exception caught: " + e.getMessage());
    } finally {
        System.out.println("Finally block executed.");
    }
    ```
    

###### **`finalize()`**

- **Purpose**: It is a method in the `Object` class that the garbage collector calls before reclaiming the object's memory.
- **When it is used**: Rarely, for releasing unmanaged resources (e.g., file handles, sockets) before the object is destroyed. Note that it is largely deprecated or discouraged in modern Java because better resource management techniques (like `try-with-resources`) are available.
- **Execution**: Not guaranteed to be called, as the garbage collector may not always run.
- **Syntax**:
    
    ```java
    @Override
    protected void finalize() throws Throwable {
        // Cleanup code
    }
    ```
    
- **Example**:
    
    ```java
    public class FinalizeExample {
        @Override
        protected void finalize() throws Throwable {
            System.out.println("Finalize called");
        }
    
        public static void main(String[] args) {
            FinalizeExample obj = new FinalizeExample();
            obj = null; // Make the object eligible for garbage collection
            System.gc(); // Request garbage collection
        }
    }
    ```
    

 ###### **Key Differences**:

|Feature|`finally`|`finalize()`|
|---|---|---|
|**Purpose**|Ensures cleanup code is executed after `try`.|Cleanup before object destruction by GC.|
|**When called**|Always executed after `try-catch`.|Called by the garbage collector (not guaranteed).|
|**Scope**|Part of exception handling.|Part of garbage collection.|
|**Usage**|Explicitly written by the programmer.|Implicitly called by the JVM.|

# 6. What is an anonymous inner class? How is it different from an inner class?
 ###### **Anonymous Inner Class**

An **anonymous inner class** in Java is a type of inner class that does not have a name and is declared and instantiated all in one step. It is often used when you need to override a method of a class or implement an interface without creating a separate named class.

**Key Characteristics**:

1. It is declared and instantiated simultaneously using the `new` keyword.
2. It is typically used to provide a short-lived implementation of an interface or a class.
3. It cannot have explicit constructors since it does not have a name.
4. It is often used for event handling or simple implementations.

**Syntax Example**:

```java
public class Main {
    public static void main(String[] args) {
        // Anonymous inner class implementing an interface
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                System.out.println("Anonymous inner class implementation.");
            }
        };

        runnable.run();

        // Anonymous inner class extending a class
        Thread thread = new Thread() {
            @Override
            public void run() {
                System.out.println("Thread running with an anonymous inner class.");
            }
        };

        thread.start();
    }
}
```

 ###### **Differences Between Anonymous Inner Class and Outer Class**:

|Feature|Anonymous Inner Class|Outer Class|
|---|---|---|
|**Name**|Does not have a name.|Has a name defined in the class declaration.|
|**Scope**|Defined within a method or block.|Top-level class with its own scope.|
|**Usage**|For quick and specific functionality.|Represents broader functionalities or concepts.|
|**Declaration**|Declared using the `new` keyword.|Declared with the `class` keyword.|
|**Inheritance**|Typically implements an interface or extends a class.|Can inherit from other classes or implement interfaces.|
|**Constructors**|Cannot have explicit constructors.|Can define constructors.|
|**Reusability**|Cannot be reused; defined for a specific use.|Can be reused multiple times in the program.|

###### Use Cases:

- Anonymous inner classes are suitable for concise and specific tasks, such as:
    - Implementing event listeners.
    - Providing one-time implementations of an interface.
    - Overriding methods of a class without creating a separate subclass.

---
###### Summary
**Anonymous Inner Class**:  
A class without a name, declared and instantiated in one step. It is used for providing short-lived implementations of an interface or a class, often in a concise manner.

**Key Points**:

- Cannot have a name or explicit constructors.
- Defined within a method or block using `new`.
- Commonly used for event handling or quick overrides.

**Difference from Outer Class**:

- **Anonymous Inner Class**: No name, cannot be reused, and used for specific, temporary tasks.
- **Outer Class**: Named, reusable, and represents broader functionalities.


# 7. What is system class?
The **`System` class** in Java is a final class from the `java.lang` package that provides utility methods and fields for system-level operations. It cannot be instantiated because its constructor is private, and all its methods and fields are static.

###### **Key Features of the `System` Class**:

1. **Standard Input, Output, and Error Streams**:
	
	- **`System.in`**: Standard input stream (keyboard by default).
	- **`System.out`**: Standard output stream (console by default).
	- **`System.err`**: Standard error stream (console by default).

Example:
```java
System.out.println("Hello, World!");  // Prints to the console
```

# 8. What is daemon thread and How to create in Java?
###### **Daemon Thread in Java**

A **daemon thread** in Java is a background thread that runs in the JVM to provide services to user threads. It is low-priority and is terminated automatically when all user threads in the application have finished execution.

**Key Features of Daemon Threads**:

1. Used for background tasks, such as garbage collection or monitoring.
2. Automatically stops when all user threads are complete.
3. Not guaranteed to complete execution, as they can be terminated abruptly when the JVM exits.

###### **Creating a Daemon Thread**

You can create a daemon thread in Java by calling the **`setDaemon(true)`** method on a `Thread` object before starting the thread.

###### **Steps to Create a Daemon Thread**:

4. Create a thread using the `Thread` class or by implementing the `Runnable` interface.
5. Call the **`setDaemon(true)`** method on the thread object before starting the thread.
6. Start the thread using the **`start()`** method.

###### **Example**:

```java
public class DaemonThreadExample {
    public static void main(String[] args) {
        Thread daemonThread = new Thread(() -> {
            while (true) {
                System.out.println("Daemon thread running...");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        // Set the thread as a daemon
        daemonThread.setDaemon(true);

        // Start the daemon thread
        daemonThread.start();

        System.out.println("Main thread running...");
        try {
            Thread.sleep(3000); // Main thread sleeps for 3 seconds
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("Main thread finished.");
    }
}
```

###### **Output**:

```
Main thread running...
Daemon thread running...
Daemon thread running...
Daemon thread running...
Main thread finished.
```

In this example, the daemon thread stops automatically after the main thread (user thread) finishes execution.

###### **Key Points**:

- **`isDaemon()`**: Checks if a thread is a daemon thread.
- **Important Rule**: The **`setDaemon(true)`** method must be called before the thread is started; otherwise, it throws an `IllegalThreadStateException`.
- Daemon threads are typically used for tasks like logging, background monitoring, or cleanup.


# 9. Does java support global variables?
No, Java doesn't support global variables to avoid namespace collisions and maintain referential transparency.
Instead, variables are scoped within classes, methods, or blocks, providing better control and reducing unintended side effects, ensuring easier debugging and maintenance.
# 10. How is a RMI object is developed?
Remote Method Invocation (**RMI**) in Java allows an object to invoke methods on an object running in another JVM, even if it's on a different machine. RMI is used for distributed systems, enabling communication between different parts of an application.

1. Define the interface for remote methods.
2. Implement the interface.
3. Compile both the interface and implementation
4. Use the RMI compiler (rmic) to generate stubs.
5. Start the RMI registry.
6. Run the server and client applications to enable remote calls.

# 11. Explain the difference between time slicing and preemptive scheduling?

**Time Slicing** and **Preemptive Scheduling** are two mechanisms used in multitasking operating systems to manage CPU allocation among multiple processes or threads. While they share similarities, they have distinct characteristics.

---

### **Time Slicing**

1. **Definition**:  
    In time slicing, the CPU is allocated to a process or thread for a fixed time interval, known as a "time slice" or "quantum." After the time slice expires, the CPU is reallocated to the next process or thread in the queue.
    
2. **Characteristics**:
    
    - Ensures **fair sharing** of CPU time among threads.
    - No single thread can monopolize the CPU.
    - The switching happens in a **round-robin** manner, regardless of whether the current thread has completed its task.
3. **Advantages**:
    
    - Simpler implementation.
    - Provides fairness by dividing CPU time equally.
4. **Disadvantages**:
    
    - If the time slice is too short, overhead increases due to frequent context switching.
    - If the time slice is too long, responsiveness decreases.
5. **Example**:
    
    - Thread scheduling in simple systems where tasks are switched after fixed intervals.

---

### **Preemptive Scheduling**

6. **Definition**:  
    Preemptive scheduling allows the operating system to forcibly remove the CPU from a running thread or process, regardless of whether its time slice is over. The CPU is reallocated based on priority or system conditions.
    
7. **Characteristics**:
    
    - **Priority-based**: High-priority threads can interrupt low-priority threads.
    - Decisions are made dynamically based on system conditions (e.g., new high-priority task arrival or resource availability).
    - Ensures better **responsiveness** for critical tasks.
8. **Advantages**:
    
    - Allows critical tasks to execute promptly.
    - Prevents low-priority threads from blocking high-priority ones.
9. **Disadvantages**:
    
    - Requires **complex scheduling algorithms**.
    - Frequent context switching can cause overhead.
10. **Example**:
    
    - Real-time operating systems where critical tasks preempt less critical ones.

---

### **Key Differences**

|Feature|Time Slicing|Preemptive Scheduling|
|---|---|---|
|**Decision Basis**|Fixed time slice for each thread.|Based on thread priority or system state.|
|**Interrupts**|Threads are not interrupted mid-slice.|Threads can be interrupted at any time.|
|**Priority Handling**|No priority consideration.|High-priority tasks are given preference.|
|**Use Case**|Fair sharing in general-purpose systems.|Real-time or priority-driven tasks.|
|**Responsiveness**|Lower for critical tasks.|High for critical tasks.|
|**Complexity**|Simpler implementation.|More complex algorithms needed.|

---

### **Summary**

- **Time Slicing**: Divides CPU time into fixed intervals, ensuring fairness without prioritizing tasks.
- **Preemptive Scheduling**: Allocates CPU dynamically based on priorities or system requirements, ensuring responsiveness for high-priority tasks.

Both mechanisms are often used in modern operating systems, with **preemptive scheduling** being more common for its flexibility and efficiency.

> Time slicing gives each task a fixed time to run, after which it moves to the ready queue.
>In preemptive scheduling, tasks are prioritized, with higher-priority tasks preempting lower-priority ones. 
>Both manages the thread execution in multithreading.

# 12 Garbage collector thread is what kind of thread?

>The garbage collector in java is a low-priority daemon thread that automatically reclaims unused objects, managing memory efficiently without disrupting the main program. 
>It plays a key role in Java's memory management, enhancing performance and reliability. 


# 13. What is a lifecycle of a thread in Java?

A thread  in Java goes through the following stages:
1. **New:** Created but not started.
2. **Runnable:** Ready to run, but not executing.
3. **Running:** Actively executing.
4. **Blocked:** Waiting for a resource or event.
5. **Terminated:** Execution is complete.

# 14. State the methods used during Serialization and Deserialization process in Java?

Serialization and deserialization in Java involve converting an object to a stream of bytes and reconstructing the object from the byte stream, respectively. These processes are part of the **Java Object Serialization API** and are used for tasks like saving object states or transmitting objects across networks.

---

### **Serialization Process**

Serialization converts a Java object into a stream of bytes that can be stored or transmitted.

#### **Key Method for Serialization**:

- **`ObjectOutputStream.writeObject(Object obj)`**  
    This method serializes the given object and writes it to an output stream.

#### **Steps**:

1. Create an `ObjectOutputStream` linked to a file or other output destination.
2. Call `writeObject()` on the object to serialize it.

**Example**:

```java
import java.io.FileOutputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;

class Person implements Serializable {
    private static final long serialVersionUID = 1L;
    String name;
    int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

public class SerializationExample {
    public static void main(String[] args) {
        try {
            Person person = new Person("Alice", 30);

            // Serialize the object
            FileOutputStream fileOut = new FileOutputStream("person.ser");
            ObjectOutputStream out = new ObjectOutputStream(fileOut);
            out.writeObject(person); // Serialization
            out.close();
            fileOut.close();

            System.out.println("Object serialized successfully.");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

---

### **Deserialization Process**

Deserialization reconstructs the Java object from the serialized byte stream.

#### **Key Method for Deserialization**:

- **`ObjectInputStream.readObject()`**  
    This method reads the serialized object from an input stream and deserializes it into a Java object.

#### **Steps**:

1. Create an `ObjectInputStream` linked to the file or source of the serialized object.
2. Call `readObject()` to reconstruct the object.

**Example**:

```java
import java.io.FileInputStream;
import java.io.ObjectInputStream;

public class DeserializationExample {
    public static void main(String[] args) {
        try {
            // Deserialize the object
            FileInputStream fileIn = new FileInputStream("person.ser");
            ObjectInputStream in = new ObjectInputStream(fileIn);
            Person person = (Person) in.readObject(); // Deserialization
            in.close();
            fileIn.close();

            System.out.println("Object deserialized successfully.");
            System.out.println("Name: " + person.name + ", Age: " + person.age);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

---

### **Methods Overview**

| **Method**                    | **Used For**                         | **Class**            |
| ----------------------------- | ------------------------------------ | -------------------- |
| **`writeObject(Object obj)`** | Serialize an object to a stream.     | `ObjectOutputStream` |
| **`readObject()`**            | Deserialize an object from a stream. | `ObjectInputStream`  |

---

### **Key Points**:

1. The object being serialized must implement the **`Serializable`** interface.
2. Use **`serialVersionUID`** to ensure compatibility between serialized and deserialized objects.
3. **Transient** fields are not serialized (they are skipped during the process).
4. `NotSerializableException` is thrown if the object does not implement `Serializable`.

Serialization is a powerful feature in Java, often used in file storage, network communication, and deep cloning of objects.

# 15. What are volatile variables and what is their purpose?


In Java, the **`volatile`** keyword is used to declare a variable as volatile, which means that the value of this variable will always be read from and written to the **main memory**, rather than a thread's local cache. This ensures visibility and consistency of the variable's value across multiple threads.
### **When to Use Volatile**

- Use `volatile` for **flags, status indicators**, or **simple shared variables** that are read and written by multiple threads.
- Do **not** use `volatile` for **compound actions** like incrementing a counter (`counter++`) or adding to a collection. Use **synchronized** blocks or atomic classes (e.g., `AtomicInteger`) for such operations.

# 16. What are wrapper classes in Java?

**Wrapper classes** in Java are used to convert primitive data types (like `int`, `char`, `double`, etc.) into corresponding objects. They "wrap" the primitive types in an object, allowing them to be used in scenarios where objects are required, such as in Java's Collections Framework or with generics.

For every primitive data type, Java provides a corresponding wrapper class in the **`java.lang`** package.

---

### **Primitive Types and Their Wrapper Classes**

|**Primitive Type**|**Wrapper Class**|
|---|---|
|`byte`|`Byte`|
|`short`|`Short`|
|`int`|`Integer`|
|`long`|`Long`|
|`float`|`Float`|
|`double`|`Double`|
|`char`|`Character`|
|`boolean`|`Boolean`|

---

### **Purpose of Wrapper Classes**

1. **Object Requirements**:
    
    - Primitive types cannot be used directly in collections or frameworks that operate on objects, such as `ArrayList` or `HashMap`. Wrapper classes allow primitives to be used in such scenarios.
2. **Utility Methods**:
    
    - Wrapper classes provide utility methods for operations like parsing strings, comparing values, or converting between types.
    - Example: `Integer.parseInt("123")` to convert a string to an integer.
3. **Autoboxing and Unboxing**:
    
    - Wrapper classes enable **autoboxing** (automatic conversion of primitives to objects) and **unboxing** (conversion of objects back to primitives).
4. **Null Values**:
    
    - Wrapper classes can represent `null`, which is useful in applications where a "missing" value is significant.

---

### **Autoboxing and Unboxing**

#### Autoboxing:

- Automatic conversion of a primitive type to its corresponding wrapper class.
- Example:
    
    ```java
    int x = 5;
    Integer obj = x; // Autoboxing
    ```
    

#### Unboxing:

- Automatic conversion of a wrapper class object to its corresponding primitive type.
- Example:
    
    ```java
    Integer obj = 10;
    int y = obj; // Unboxing
    ```
    

---

### **Example: Using Wrapper Classes**

```java
import java.util.ArrayList;

public class WrapperExample {
    public static void main(String[] args) {
        // Using primitive type
        int num = 10;

        // Wrapping into an object
        Integer wrappedNum = num; // Autoboxing

        // Adding to a collection (ArrayList accepts only objects)
        ArrayList<Integer> numbers = new ArrayList<>();
        numbers.add(wrappedNum); // Works because wrappedNum is an Integer object

        // Retrieving and unwrapping
        int unwrappedNum = numbers.get(0); // Unboxing

        System.out.println("Primitive: " + num);
        System.out.println("Wrapped: " + wrappedNum);
        System.out.println("Unwrapped: " + unwrappedNum);
    }
}
```

---

### **Commonly Used Utility Methods in Wrapper Classes**

#### **1. Parsing Strings into Primitives**

- Example:
    
    ```java
    int num = Integer.parseInt("123"); // Converts string to int
    double val = Double.parseDouble("3.14"); // Converts string to double
    ```
    

#### **2. Converting Primitives to Strings**

- Example:
    
    ```java
    String str = Integer.toString(123); // Converts int to string
    ```
    

#### **3. Comparing Values**

- Example:
    
    ```java
    Integer a = 10;
    Integer b = 20;
    int comparison = Integer.compare(a, b); // Returns -1 if a < b, 0 if a == b, 1 if a > b
    ```
    

---

### **Advantages of Wrapper Classes**

1. **Object Functionality**:
    - Allow primitive values to be used as objects, enabling usage in collections and frameworks.
2. **Utility Methods**:
    - Provide methods for parsing, conversion, and manipulation.
3. **Autoboxing/Unboxing**:
    - Simplifies the interaction between primitive types and their corresponding objects.

---

### **Disadvantages of Wrapper Classes**

4. **Performance Overhead**:
    - Converting between primitives and objects introduces some overhead compared to using primitive types directly.
5. **Memory Usage**:
    - Objects take more memory than primitive types.

---

### **Conclusion**

Wrapper classes in Java bridge the gap between primitives and objects, allowing for more versatile programming, especially in collections and frameworks. They come with additional utility methods but should be used judiciously to avoid unnecessary overhead.

# 17. How can we make a singleton class in Java?

A **singleton class** in Java is a design pattern where only one instance of the class is created during the application's lifecycle. This is useful for scenarios where a single point of control or shared resource management is needed, such as configuration settings, logging, or database connections.

Here’s how you can implement a singleton class in Java:

---

### **Steps to Create a Singleton Class**

1. **Private Constructor**:
    
    - Prevents instantiation of the class from outside the class.
2. **Static Instance Variable**:
    
    - Holds the single instance of the class.
3. **Public Static Method**:
    
    - Provides global access to the instance and ensures only one instance is created.

---

### **1. Eager Initialization**

The singleton instance is created at the time of class loading.

```java
public class Singleton {
    // Static instance of Singleton class
    private static final Singleton INSTANCE = new Singleton();

    // Private constructor to prevent instantiation
    private Singleton() {}

    // Public method to provide access to the instance
    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```

**Pros**:

- Simple to implement.
- Thread-safe by default.

**Cons**:

- The instance is created even if it is never used, which can waste resources.

---

### **2. Lazy Initialization**

The instance is created only when it is needed.

```java
public class Singleton {
    // Static variable to hold the single instance
    private static Singleton instance;

    // Private constructor
    private Singleton() {}

    // Public method to provide access to the instance
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton(); // Create instance when first accessed
        }
        return instance;
    }
}
```

**Pros**:

- Instance is created only when needed, saving resources.

**Cons**:

- Not thread-safe in its basic form (can lead to multiple instances in a multi-threaded environment).

---

### **3. Thread-Safe Singleton (Double-Checked Locking)**

Ensures thread safety while maintaining performance.

```java
public class Singleton {
    private static volatile Singleton instance; // Use volatile for visibility across threads

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) { // Synchronize on the class
                if (instance == null) {     // Double-check to avoid multiple initializations
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

**Pros**:

- Thread-safe.
- Lazy initialization with better performance compared to fully synchronized methods.

**Cons**:

- Slightly more complex implementation.

---

### **4. Bill Pugh Singleton (Static Inner Class)**

This approach leverages the Java classloader mechanism to ensure thread safety and lazy initialization.

```java
public class Singleton {
    // Private constructor
    private Singleton() {}

    // Static inner class - SingletonHelper
    private static class SingletonHelper {
        private static final Singleton INSTANCE = new Singleton();
    }

    // Public method to provide access to the instance
    public static Singleton getInstance() {
        return SingletonHelper.INSTANCE;
    }
}
```

**Pros**:

- Thread-safe.
- Lazy initialization without explicit synchronization.
- Simple and clean implementation.

**Cons**:

- None significant; widely regarded as the best approach.

---

### **5. Enum Singleton**

The simplest and safest way to implement a singleton in Java, leveraging Java's `enum` type.

```java
public enum Singleton {
    INSTANCE;

    public void doSomething() {
        System.out.println("Singleton using Enum");
    }
}
```

**Pros**:

- Thread-safe.
- Handles serialization and reflection attacks out of the box.

**Cons**:

- May not feel intuitive if you aren't familiar with `enum` for singletons.

---

### **Preventing Issues in Singleton**

4. **Reflection Attack**:
    
    - Reflection can be used to access private constructors.
    - To prevent this, throw an exception in the constructor if an instance already exists.
    
    ```java
    private Singleton() {
        if (instance != null) {
            throw new IllegalStateException("Instance already created");
        }
    }
    ```
    
5. **Serialization Issue**:
    
    - Serialization can create new instances.
    - To fix this, override the `readResolve` method in your singleton class:
    
    ```java
    private Object readResolve() {
        return instance;
    }
    ```
    
6. **Cloning Issue**:
    
    - Cloning can create a new instance.
    - To prevent this, override the `clone` method to throw an exception:
    
    ```java
    @Override
    protected Object clone() throws CloneNotSupportedException {
        throw new CloneNotSupportedException("Cannot clone a singleton");
    }
    ```
    

---

### **When to Use a Singleton**

- Logging frameworks.
- Configuration managers.
- Connection pools.
- Caching mechanisms.
- Resource managers.

By carefully choosing the implementation, you can ensure the singleton is robust, efficient, and suitable for your application's needs.

# 18. What are important methods of Exception class in Java?

1. **String getMessage():** Returns a detailed message about the exception.
2. **String toString():** Returns a string representation of the exception
3. **void printStackTrace():** Prints the stack trace of the exception to the console.
4. **synchronized Throwable getCause():** Returns the cause of the exception.
5. **public stackTraceElement[] getStackTrace():** Returns an array representing the stack trace of the exception.

# 19. How can we make a thread in Java?

In Java, threads can be created in two primary ways:

1. **By extending the `Thread` class.**
2. **By implementing the `Runnable` interface.**

Java also provides a more advanced option using the **`Callable`** interface, which allows threads to return results or throw exceptions.

---

### **1. Creating a Thread by Extending the `Thread` Class**

You can create a custom thread class by extending the `Thread` class and overriding its `run()` method.

#### Example:

```java
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread is running: " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread thread = new MyThread(); // Create an instance of the thread
        thread.start();                  // Start the thread
    }
}
```

#### Key Points:

- The `run()` method contains the code that will be executed in the thread.
- Call `start()` to begin the thread; calling `run()` directly will not start a new thread but execute the method on the main thread.

---

### **2. Creating a Thread by Implementing the `Runnable` Interface**

Another way to create a thread is to implement the `Runnable` interface and pass it to a `Thread` object.

#### Example:

```java
class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Thread is running: " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();
        Thread thread = new Thread(myRunnable); // Pass Runnable to Thread
        thread.start();                        // Start the thread
    }
}
```

#### Key Points:

- Preferred over extending `Thread` because it allows your class to extend another class.
- It decouples the task logic from the thread itself.

---

### **3. Using Lambda Expressions (Since Java 8)**

If the `Runnable` implementation contains only a single method, you can use a lambda expression for brevity.

#### Example:

```java
public class Main {
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            System.out.println("Thread is running: " + Thread.currentThread().getName());
        });
        thread.start();
    }
}
```

#### Key Points:

- Simplifies thread creation when using `Runnable` with minimal logic.

---

### **4. Creating a Thread with `Callable`**

The `Callable` interface (in the `java.util.concurrent` package) is similar to `Runnable` but allows the thread to return a result or throw exceptions. It works with `ExecutorService`.

#### Example:

```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class Main {
    public static void main(String[] args) throws Exception {
        Callable<String> task = () -> {
            return "Thread executed by Callable";
        };

        ExecutorService executor = Executors.newSingleThreadExecutor();
        Future<String> future = executor.submit(task); // Submit Callable to ExecutorService

        // Get the result from the Callable
        System.out.println(future.get());

        executor.shutdown();
    }
}
```

#### Key Points:

- Use `Callable` when the thread needs to return a value.
- Requires an `ExecutorService` to manage threads.

---

### **Thread Lifecycle**

Threads in Java go through the following states:

1. **New**: Created but not started (`new Thread()`).
2. **Runnable**: Ready to run (`start()` called).
3. **Running**: Actively running on the CPU.
4. **Blocked/Waiting**: Waiting for a resource or signal.
5. **Terminated**: Completed execution.

---

### **Choosing the Right Approach**

- Use **`Runnable`** for flexibility and when you do not need thread results.
- Use **`Callable`** for complex tasks requiring results or exceptions.
- Use **`Thread`** directly for simple standalone threads.
# 20. Explain the difference between get() and load() methods?

In Hibernate, get() and load() retrieve objects differently:

- **get()** returns **null** if the object isn't found, while **load()** throws an "**ObjectNotFoundException**".
- **get()** returns a **real object;** **load()** returns a proxy for lazy **initialization**.
- **get()** immediately hits the database; **load()** may delay the hit until the object is accessed.
