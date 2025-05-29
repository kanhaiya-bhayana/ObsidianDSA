## HashMap & HashSet

### HashMap
>Search on Key => O(1)

### Here's a **time complexity table** for common operations on a **HashMap** in Java (specifically `java.util.HashMap`):

|Operation|Average Time Complexity|Worst-Case Time Complexity|
|---|---|---|
|`put(K key, V value)`|O(1)|O(n)|
|`get(Object key)`|O(1)|O(n)|
|`remove(Object key)`|O(1)|O(n)|
|`containsKey(Object key)`|O(1)|O(n)|
|`containsValue(Object value)`|O(n)|O(n)|
|`size()`|O(1)|O(1)|
|`isEmpty()`|O(1)|O(1)|
|`keySet()`, `values()`, `entrySet()` (iteration)|O(n)|O(n)|

### Explanation:

- **Average case (O(1))**: HashMap uses hashing; when keys are well-distributed, operations like `get`, `put`, and `remove` are constant time.
    
- **Worst case (O(n))**: If many keys hash to the same bucket (poor hash function or hash collision attack), operations degrade to linear time. Java 8+ mitigates this by converting buckets to balanced trees (O(log n) for worst-case lookup/insert in such buckets).
    


### Here's the **time complexity table** for **`TreeMap`** and **`LinkedHashMap`** in Java:

---

### 📘 `TreeMap` (`java.util.TreeMap`)

- Implemented as a **Red-Black Tree**
    
- Maintains keys in **sorted order**
    

|Operation|Time Complexity|
|---|---|
|`put(K key, V value)`|O(log n)|
|`get(Object key)`|O(log n)|
|`remove(Object key)`|O(log n)|
|`containsKey(Object key)`|O(log n)|
|`containsValue(Object value)`|O(n)|
|`firstKey()`, `lastKey()`|O(log n)|
|`size()`|O(1)|
|`isEmpty()`|O(1)|
|Iteration (`entrySet()`, etc.)|O(n)|

---

### 📗 `LinkedHashMap` (`java.util.LinkedHashMap`)

- Maintains **insertion order** (or access order if configured)
    
- Backed by a **hash table + doubly linked list**
    

|Operation|Average Time Complexity|Worst-Case Time Complexity|
|---|---|---|
|`put(K key, V value)`|O(1)|O(n)|
|`get(Object key)`|O(1)|O(n)|
|`remove(Object key)`|O(1)|O(n)|
|`containsKey(Object key)`|O(1)|O(n)|
|`containsValue(Object value)`|O(n)|O(n)|
|`size()`|O(1)|O(1)|
|`isEmpty()`|O(1)|O(1)|
|Iteration (preserves order)|O(n)|O(n)|

---

### Summary:

|Feature|`HashMap`|`TreeMap`|`LinkedHashMap`|
|---|---|---|---|
|Ordering|None|Sorted|Insertion/Access|
|Null keys|1 allowed|Not allowed|1 allowed|
|Performance|Fastest (avg)|Slower|Slightly slower|
|Thread-safe|No|No|No|

Let me know if you want the same table for `ConcurrentHashMap` or a visual comparison chart.


### Here’s a **time complexity table and feature comparison** for **`ConcurrentHashMap`** in Java:

---

### 🧵 `ConcurrentHashMap` (`java.util.concurrent.ConcurrentHashMap`)

- Thread-safe implementation of a hash table
    
- Introduced in Java 5, majorly improved in Java 8
    
- Uses **lock striping** (Java 7) or **CAS + synchronized blocks** (Java 8+) for concurrency
    

#### Time Complexity Table

|Operation|Average Time Complexity|Worst-Case Time Complexity|
|---|---|---|
|`put(K key, V value)`|O(1)|O(n)|
|`get(Object key)`|O(1)|O(n)|
|`remove(Object key)`|O(1)|O(n)|
|`containsKey(Object key)`|O(1)|O(n)|
|`containsValue(Object value)`|O(n)|O(n)|
|`size()` (approximate in parallel)|O(1)-O(n)|O(n)|
|Iteration (`entrySet()`, etc.)|O(n)|O(n)|

---

### 📊 Feature Comparison Chart

|Feature / Map Type|`HashMap`|`TreeMap`|`LinkedHashMap`|`ConcurrentHashMap`|
|---|---|---|---|---|
|Ordering|None|Sorted (natural or custom)|Insertion / Access|None|
|Thread-Safe|❌|❌|❌|✅|
|Null Keys / Values|1 null key, many null values|❌ null keys, ✅ null values|✅|❌ null keys, ❌ null values|
|Performance (avg case)|🔥 Fastest|🐢 Slower|⚖️ Slightly slower|⚡ Concurrent, fast|
|Backed By|Hash Table|Red-Black Tree|Hash + Linked List|Segmented Hash Table (pre-Java 8), CAS + bins (Java 8+)|
|Allows Custom Comparator|❌|✅|❌|❌|
|Ordered Iteration|❌|✅|✅|❌|
|Use Case|General use|Sorted keys|Order-sensitive|Thread-safe maps|

---
