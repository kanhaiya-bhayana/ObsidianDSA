
## Default Method
## Static Method
## Functional Interface and lambda Expression


# 1. Default Method
---

## 🔷 What are Default Methods?

A **default method** is a method in an interface that has a **default implementation**.

Before Java 8, interfaces could only have **abstract methods** (i.e., method signatures only). Java 8 introduced default methods to allow method bodies in interfaces, enabling **interface evolution** without breaking existing code.

---

## 🔹 Syntax

```java
interface InterfaceName {
    default void methodName() {
        // method body
    }
}
```

---

## ✅ Example: E-commerce Logging System

Let's say you want every service in your e-commerce app (e.g., payment, shipping, inventory) to include **basic logging**, but you don't want to write the same code in every implementing class.

### Interface with a default method:

```java
public interface Loggable {
    default void log(String message) {
        System.out.println("[LOG] " + message);
    }
}
```

### Class implementing the interface:

```java
public class PaymentService implements Loggable {
    public void processPayment() {
        log("Processing payment...");
        // payment logic
    }
}
```

> The `log` method is inherited from the interface, so no need to define it again in `PaymentService`.

---

## 🔍 Why Use Default Methods?

### 1. **Backward Compatibility**

Suppose you have an interface already used in multiple classes:

```java
interface Shipping {
    void shipItem();
}
```

Now, you want to add a method `trackShipment()`. Before Java 8, this would break all implementing classes. With default methods, you can do:

```java
interface Shipping {
    void shipItem();

    default void trackShipment() {
        System.out.println("Tracking shipment...");
    }
}
```

Now old classes don’t break and new ones can override `trackShipment()` if needed.

---

## 🔀 Default vs Abstract vs Static Methods in Interfaces

|Feature|Abstract Method|Default Method|Static Method|
|---|---|---|---|
|Implementation|No|Yes (with default body)|Yes (static body)|
|Overridable|Must override|Can override|Cannot override|
|Accessed via|Object reference|Object reference|Interface name|

---

## ⚠️ Things to Watch Out For

### 🔸 Multiple Inheritance Conflict

If two interfaces provide the same default method, a **conflict** occurs.

```java
interface A {
    default void greet() {
        System.out.println("Hello from A");
    }
}

interface B {
    default void greet() {
        System.out.println("Hello from B");
    }
}

class MyClass implements A, B {
    public void greet() {
        A.super.greet(); // or B.super.greet();
    }
}
```

You **must override** the method to resolve ambiguity.

---

## 🧠 Use Cases of Default Methods

- Logging, auditing
    
- Utility/helper methods for implementers
    
- Interface versioning and API evolution
    
- Template behavior (like in the Strategy pattern)
    

---


# 2. Static Method (Java8)

## 🔷 What is a Static Method in an Interface?

A **static method** in an interface is a method with a body that **belongs to the interface itself**, **not to the instances** of classes implementing it.

It’s similar to static methods in classes — you call them using the interface name.

---

## 🔹 Syntax

```java
interface MyInterface {
    static void staticMethod() {
        System.out.println("Static method in interface");
    }
}
```

### ✅ Usage:

```java
MyInterface.staticMethod(); // ✅ This works
```

> ❌ `new MyClass().staticMethod();` – **Not allowed**, even if `MyClass` implements `MyInterface`.

---

## 🛒 Example in E-Commerce Context

Let’s say you want to provide a **utility method** to validate payment details in the `PaymentMethod` interface.

### 🔸 Interface with Static Method

```java
public interface PaymentMethod {
    void pay(double amount);

    static boolean validateCard(String cardNumber) {
        // Dummy validation logic
        return cardNumber != null && cardNumber.length() == 16;
    }
}
```

### 🔸 Using the Static Method

```java
public class CreditCardPayment implements PaymentMethod {
    public void pay(double amount) {
        String cardNumber = "1234567812345678";
        if (PaymentMethod.validateCard(cardNumber)) {
            System.out.println("Paid $" + amount + " using Credit Card.");
        } else {
            System.out.println("Invalid card number.");
        }
    }
}
```

> You call the method via the interface: `PaymentMethod.validateCard(...)`.

---

## 🔍 Key Points About Static Methods in Interfaces

|Feature|Description|
|---|---|
|Call via|Interface name (`InterfaceName.method()`)|
|Cannot be overridden|Implementing classes cannot override static methods|
|Use cases|Utility/helper methods specific to the interface’s domain|
|Access from implementing class|No — must use `InterfaceName.method()`|

---

## ✅ When to Use Static Methods in Interfaces?

- **Utility methods** related to the interface’s role (e.g., validators, formatters)
    
- To keep related logic encapsulated **within the interface**, rather than in a separate utility class
    

---

## 🧠 Comparison: Static vs Default vs Abstract in Interfaces

|Feature|Abstract|Default|Static|
|---|---|---|---|
|Has body|❌ No|✅ Yes|✅ Yes|
|Inherited?|✅ Yes|✅ Yes|❌ No|
|Can override?|✅ Yes|✅ Yes|❌ No|
|Called via|Object|Object|Interface name (`Interface.staticMethod()`)|

---
# 2.1 Private method and Private Static method (Java9)
## 🔷 Why Were Private Methods Introduced in Interfaces?

Before Java 9:

- **Default** and **static** methods could have implementations.
    
- But if multiple default or static methods shared common logic, you'd have to **duplicate code**.
    

Java 9 added **private methods** to help:

- Avoid duplication inside interfaces
    
- Keep helper logic hidden from implementing classes
    

---

## 🔹 Types of Private Methods in Interfaces

|Type|Syntax|Accessible From|
|---|---|---|
|`private`|`private void helper() { ... }`|Inside **default** methods|
|`private static`|`private static void helper() {}`|Inside **static** methods|

---

## ✅ Example: E-Commerce Logging Interface

### 🔸 Interface with Private and Private Static Methods

```java
public interface Loggable {

    default void logInfo(String message) {
        log("INFO", message);
    }

    default void logError(String message) {
        log("ERROR", message);
    }

    private void log(String level, String message) {
        System.out.println("[" + level + "] " + message);
    }

    static void logSystemEvent(String event) {
        formatAndPrint(event);
    }

    private static void formatAndPrint(String text) {
        System.out.println("[SYSTEM] " + text);
    }
}
```

### 🔸 Usage

```java
public class OrderService implements Loggable {
    public void processOrder() {
        logInfo("Processing order...");
        logError("Payment failed.");
        Loggable.logSystemEvent("Order service started.");
    }
}
```

---

## ⚠️ Important Notes

- 🔒 **Private methods are not inherited** by implementing classes.
    
- 👀 They are only visible **inside the interface** itself.
    
- ✅ They help avoid duplicating logic across multiple default/static methods.
    
- ❌ Cannot be `protected` or `public` — only `private`.
    

---

## 🧠 When to Use

Use private methods in interfaces when:

- You have **repeated logic** across `default` or `static` methods.
    
- You want to **encapsulate** helper methods within the interface.
    
- You want cleaner and more maintainable interface code.
    

---

Let me know if you'd like a hands-on coding demo or to combine this with default/static behavior in a mini project!

# 3. What is Functional Interface?

- If an interface contains only 1 abstract method, that is known as Functional Interface.
- Also known as SAM Interface (Single Abstract Method).

```
@FunctionalInterface keyword can be used at top of the interface (But its optional).
```

> If you are using the annotation and adding more than 1 method into interface, then you will get a compilation error.

**NOTE:** There could be couple of non-abstract methods but can have only 1 abstract method.


**Summary**: In Functional Interface, only 1 abstract method is allowed, but we can have other methods like default, static method or Methods inherited from the Object Class.

## Different ways to implements the Functional Interface

1. Using "Implements"
2. Using "anonymous Class"
3. Using "Lambda expression" in (Java8) --> Lambda expression is a way to implement the Functional Interface.



Absolutely! Let's break down the **different ways to implement a Functional Interface** in Java.

---

## ✅ What is a Functional Interface?

A **Functional Interface** is an interface that contains **only one abstract method**. It can have multiple **default**, **static**, or **private** methods, but **only one abstract method**.

> Java 8 introduced the `@FunctionalInterface` annotation to mark such interfaces.

```java
@FunctionalInterface
interface PaymentProcessor {
    void process(double amount);
}
```

---

## 🔃 Ways to Implement a Functional Interface

---

### 1️⃣ **Using `implements` keyword (traditional way)**

Create a class that implements the interface and overrides the method.

```java
public class CreditCardPayment implements PaymentProcessor {
    public void process(double amount) {
        System.out.println("Processing $" + amount + " using Credit Card.");
    }
}
```

**Usage:**

```java
PaymentProcessor payment = new CreditCardPayment();
payment.process(100.0);
```

✅ **Pros:** Clear, reusable  
❌ **Cons:** Verbose for simple use cases

---

### 2️⃣ **Using an Anonymous Class**

Define the implementation **inline** without creating a separate class.

```java
PaymentProcessor payment = new PaymentProcessor() {
    public void process(double amount) {
        System.out.println("Processing $" + amount + " using Anonymous Class.");
    }
};
payment.process(200.0);
```

✅ **Pros:** No need to create a separate class  
❌ **Cons:** Still a bit verbose

---

### 3️⃣ **Using a Lambda Expression (Java 8+)**

Functional interfaces can be implemented using **lambda expressions** — concise and elegant.

```java
PaymentProcessor payment = amount -> System.out.println("Processing $" + amount + " using Lambda.");
payment.process(300.0);
```

✅ **Pros:** Clean, short, ideal for simple logic  
❌ **Cons:** Not ideal for complex logic blocks

---

## 🧠 Summary Table

|Method|Syntax Type|When to Use|
|---|---|---|
|Implements|Traditional class|Reusable logic, complex behaviors|
|Anonymous Class|Inline class declaration|One-off use, slightly less verbose than class|
|Lambda Expression|`parameter -> body`|Short, clean, functional-style programming|

---

## 🧪 Bonus: Built-in Functional Interfaces (Java 8+)

You don’t always need to create your own — Java provides many via `java.util.function`:

|Interface|Abstract Method|Example Use Case|
|---|---|---|
|`Runnable`|`void run()`|Threads, tasks|
|`Consumer<T>`|`void accept(T)`|Process an object|
|`Supplier<T>`|`T get()`|Provide a value|
|`Predicate<T>`|`boolean test(T)`|Filtering logic|
|`Function<T,R>`|`R apply(T)`|Transforming input to output|

---

### Let’s break down the most important **built-in functional interfaces** provided in the **`java.util.function`** package (Java 8+). These are powerful tools for writing cleaner, functional-style code.

---

## ✅ 1. **`Consumer<T>`**

### ➤ Represents: An operation that **accepts a single input** and **returns no result**.

```java
Consumer<String> greet = name -> System.out.println("Hello, " + name);
greet.accept("Alice"); // Output: Hello, Alice
```

**Use case:** Iterating through a list and performing an action (e.g., printing, logging).

---

## ✅ 2. **`Supplier<T>`**

### ➤ Represents: A function that **returns a result** but **takes no input**.

```java
Supplier<String> supplyName = () -> "OpenAI";
System.out.println(supplyName.get()); // Output: OpenAI
```

**Use case:** Lazy generation of values, default values, or configurations.

---

## ✅ 3. **`Predicate<T>`**

### ➤ Represents: A function that **evaluates a condition** and returns `true` or `false`.

```java
Predicate<Integer> isEven = n -> n % 2 == 0;
System.out.println(isEven.test(4)); // true
```

**Use case:** Filtering data (e.g., in streams).

---

## ✅ 4. **`Function<T, R>`**

### ➤ Represents: A function that **accepts one argument** and **produces a result**.

```java
Function<String, Integer> stringLength = str -> str.length();
System.out.println(stringLength.apply("ChatGPT")); // 7
```

**Use case:** Mapping or transforming data.

---

## ✅ 5. **`BiFunction<T, U, R>`**

### ➤ Represents: A function that **accepts two arguments** and **returns a result**.

```java
BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;
System.out.println(add.apply(10, 20)); // 30
```

**Use case:** Operations involving two inputs (e.g., merging, comparing).

---

## ✅ 6. **`UnaryOperator<T>`** (a special case of `Function<T, T>`)

### ➤ Represents: A function that **takes one argument and returns the same type**.

```java
UnaryOperator<String> toUpper = s -> s.toUpperCase();
System.out.println(toUpper.apply("hello")); // HELLO
```

---

## ✅ 7. **`BinaryOperator<T>`** (a special case of `BiFunction<T, T, T>`)

### ➤ Represents: A function that **takes two arguments of the same type** and **returns a result of the same type**.

```java
BinaryOperator<Integer> multiply = (a, b) -> a * b;
System.out.println(multiply.apply(3, 4)); // 12
```

---

## 🔁 Summary Table

|Interface|Method|Input(s)|Output|Use Case|
|---|---|---|---|---|
|`Consumer<T>`|`accept(T)`|One|None|Print/log/process something|
|`Supplier<T>`|`get()`|None|One|Supply/generate a value|
|`Predicate<T>`|`test(T)`|One|Boolean|Condition/filtering|
|`Function<T, R>`|`apply(T)`|One|One (diff type)|Transform/mapping|
|`BiFunction<T,U,R>`|`apply(T,U)`|Two|One|Combine or process 2 values|
|`UnaryOperator<T>`|`apply(T)`|One|One (same type)|Modify single input (e.g., format)|
|`BinaryOperator<T>`|`apply(T,T)`|Two|One (same type)|Combine two same-type values|

---

## 💡 Real-World Use in Streams (Example)

```java
List<String> names = List.of("Alice", "Bob", "Alex", "Steve");

names.stream()
     .filter(name -> name.startsWith("A")) // Predicate
     .map(String::toUpperCase)             // Function
     .forEach(System.out::println);        // Consumer
```

---

## Runnable ()->{}
## callable = runnable + return ()->{return T;}
## Function<T,K> (T)->return K;

Function.identity()
x->x