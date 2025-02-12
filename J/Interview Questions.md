## Top 50
##### 1. What all does JVM Comprise of?
1. **Classloader:** A JVM subsystem responsible for loading class files into memory when program starts.
2. **Heap:** Memory area used for dynamic memory allocation, where objects and their data are stored during runtime.
3. Method Area: Stores class-level information like static variables, constant pools, and method code. It holds per-class structure.
4. Stack: Used to store method calls and local variables. Each method call creates a new frame on the stack, which is removed when the method finished.
5. Execute Engine: Reads and execute bytecode. It can interpret bytecode or use the JIT compiler to covert it into machine code for faster execution.
6. JNI (Java Native Interface): Allows Java to interact with native applications or libraries in other languages like C or C++ for platform-specific features.

##### 2. What is Object-oriented programming? Is Java an object-oriented language?
**What is Object-Oriented Programming?**  
Object-Oriented Programming (OOP) is a programming paradigm based on the concept of "objects," which can contain data in the form of fields (attributes) and code in the form of methods (functions). OOP principles include:

1. **Encapsulation**: Bundling data and methods that operate on the data within an object.
2. **Inheritance**: Enabling one class to inherit properties and behavior from another.
3. **Polymorphism**: Allowing methods or functions to behave differently based on the object or input.
4. **Abstraction**: Hiding complex implementation details and exposing only the necessary features of an object.

**Is Java an Object-Oriented Language?**  
Yes, Java is primarily an object-oriented language. It supports OOP principles through its class-based structure and features like inheritance, encapsulation, polymorphism, and abstraction. However, Java also includes some non-object-oriented features like primitive data types, making it not purely object-oriented.


##### 3. What do you understand by Aggregation in context of Java?
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


##### 4. Name the superclass in Java
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

##### 5. Explain the difference between 'finally' and 'finalize' in Java
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

##### 6. What is an anonymous inner class? How is it different from an inner class?
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


##### 7. What is system class?
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

##### 8. What is daemon thread and How to create in Java?
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


###### 9. Does java support global variables?
No, Java doesn't support global variables to avoid namespace collisions and maintain referential transparency.
Instead, variables are scoped within classes, methods, or blocks, providing better control and reducing unintended side effects, ensuring easier debugging and maintenance.
###### 10. How is a RMI object is developed?
Remote Method Invocation (**RMI**) in Java allows an object to invoke methods on an object running in another JVM, even if it's on a different machine. RMI is used for distributed systems, enabling communication between different parts of an application.

1. Define the interface for remote methods.
2. Implement the interface.
3. Compile both the interface and implementation
4. Use the RMI compiler (rmic) to generate stubs.
5. Start the RMI registry.
6. Run the server and client applications to enable remote calls.
