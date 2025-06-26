# Memory Management in Java

Memory management in Java is handled by the JVM (Java Virtual Machine), and it allocates memory in two main areas: **Stack** and **Heap**. Both are created by the JVM and stored in RAM.

---

## Two Types of Memory
1. **Stack**
2. **Heap**

---

## Stack Memory
- Stack memory is used to store:
  - **Temporary variables** and **method-specific data**.
  - **Primitive data types** (e.g., `int`, `double`, `boolean`, etc.).
  - **References to objects** stored in the heap.
    - Types of references:
      - Strong references
      - Weak references (Soft and Phantom references)

- **Thread-local memory**: Each thread has its own stack memory. 
- **Scope-based visibility**: Variables in the stack are visible only within their scope. Once a variable goes out of scope, it is automatically removed in a **Last-In-First-Out (LIFO)** order.

### Key Features:
- Efficient memory allocation and deallocation due to LIFO behavior.
- Limited in size, leading to a `java.lang.StackOverflowError` when memory is exhausted (e.g., due to deep recursion).

### Example:
```java
public class StackExample {
    public static void main(String[] args) {
        int x = 10; // Primitive stored in stack
        String name = "Java"; // Reference to a heap object stored in stack
        display(x, name);
    }

    public static void display(int number, String text) {
        System.out.println("Number: " + number);
        System.out.println("Text: " + text);
    }
}
```

### References:
1. **Strong Reference**: Default type; prevents the referenced object from being garbage collected.
   ```java
   String strongRef = new String("Strong Reference");
   ```
2. **Weak Reference**: Allows the referenced object to be garbage collected if no strong references exist.
   - **Soft Reference**: Retained longer, even under memory pressure.
     ```java
     SoftReference<String> softRef = new SoftReference<>("Soft Reference");
     ```
   - **Phantom Reference**: Used for cleanup operations post-finalization.

> `System.gc();` requests garbage collection but does **not guarantee** that it will run immediately.

---

## Heap Memory
Heap memory is used to store **objects** and **class-level data** (e.g., static variables). It is shared among all threads.

### Key Features:
- **No specific allocation order**.
- **Garbage Collection**: Unreferenced objects are automatically removed using Garbage Collection (GC).
  - Algorithms:
    - **Mark and Sweep**: Marks live objects and removes unreferenced ones.
    - **Mark and Sweep with Compact**: Rearranges live objects to eliminate fragmentation.

### Components of Heap Memory:
1. **Young Generation**: Stores short-lived objects. Minor GC occurs here frequently.
   - **Eden**: New objects are allocated here.
   - **Survivor Spaces**: Objects that survive a GC cycle in Eden are moved here.

2. **Old (Tenured) Generation**: Stores long-lived objects. Major GC happens here less frequently but is slower.

3. **String Pool**: A special area for string literals to optimize memory usage.
   ```java
   String s1 = "Java"; // Stored in string pool
   String s2 = new String("Java"); // Stored in heap
   ```

4. **Permanent Generation (Pre-Java 8)** / **Metaspace (Java 8+)**:
   - Stores metadata about classes (e.g., static variables, methods).
   - No size limit for Metaspace; grows dynamically.

### Exceptions:
- `java.lang.OutOfMemoryError`: Thrown when heap memory is exhausted.

### Example:
```java
public class HeapExample {
    public static void main(String[] args) {
        String name = new String("Java"); // Stored in heap
        List<Integer> numbers = new ArrayList<>(); // Stored in heap

        for (int i = 0; i < 1000; i++) {
            numbers.add(i);
        }
    }
}
```

---

## Non-Heap Memory
- **Metaspace** (Java 8+): Stores class metadata, including:
  - Static variables
  - Method information

### Example:
```java
public class NonHeapExample {
    static int staticVariable = 10; // Stored in Metaspace
}
```

---

## Garbage Collector Algorithms
1. **Mark & Sweep Algorithm**: Marks live objects and sweeps away unreferenced objects.
2. **Mark & Sweep with Compact**: Additionally compacts the memory to reduce fragmentation.

### Types of Garbage Collectors:
1. **Serial GC**:
   - Single-threaded; suitable for single-core processors.
   - Default for small applications.

2. **Parallel GC**:
   - Multi-threaded; uses multiple threads for garbage collection.
   - Default in Java 8.

3. **CMS (Concurrent Mark & Sweep)**:
   - Minimizes pause times by concurrently marking live objects.
   - Deprecated after Java 9.

4. **G1 (Garbage First) GC**:
   - Splits heap into regions and prioritizes regions with the most garbage.
   - Default in Java 9+.

### Example:
```java
// To set the Garbage Collector:
java -XX:+UseG1GC -jar MyApp.jar
```

---

### Summary Table
| Memory Type     | Stores                                    | Access Scope  | Exceptions                     |
|-----------------|------------------------------------------|---------------|--------------------------------|
| Stack Memory    | Primitives, method-specific data         | Thread-local  | `StackOverflowError`          |
| Heap Memory     | Objects, class-level data               | Shared        | `OutOfMemoryError`            |
| Metaspace       | Class metadata, static variables         | Shared        | `OutOfMemoryError` (rare)     |


