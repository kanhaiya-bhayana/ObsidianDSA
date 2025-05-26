In Java, **interfaces** are a core part of its object-oriented programming model. An interface in Java is a **reference type**, similar to a class, that can contain only:

- **Abstract methods** (prior to Java 8)
    
- **Default methods** (Java 8 and later)
    
- **Static methods** (Java 8 and later)
    
- **Private methods** (Java 9 and later)
    
- **Constants** (public static final variables)
    

---

## 🔹 Purpose of Interfaces

Interfaces are used to:

- Achieve **abstraction**
    
- Provide a **contract** for classes to implement
    
- Enable **multiple inheritance** (since Java doesn't support multiple inheritance with classes)
    

---

## 🔹 Basic Syntax

```java
interface Animal {
    void makeSound(); // abstract method
}
```

Any class that implements `Animal` must provide an implementation of the `makeSound` method.

---

## 🔹 Implementing an Interface

```java
class Dog implements Animal {
    public void makeSound() {
        System.out.println("Woof");
    }
}
```

> Note: Methods in an interface are implicitly `public` and `abstract`, so the implementing class must make them `public`.

---

## 🔹 Interface with Default and Static Methods (Java 8+)

```java
interface Vehicle {
    void start();

    default void honk() {
        System.out.println("Honking!");
    }

    static void service() {
        System.out.println("Vehicle servicing");
    }
}
```

- `default` methods provide a body and can be overridden.
    
- `static` methods belong to the interface and can be called using `Vehicle.service()`.
    

---

## 🔹 Interface Inheritance

```java
interface Flyable {
    void fly();
}

interface Bird extends Flyable {
    void layEggs();
}
```

---

## 🔹 Multiple Interfaces

Java allows a class to implement multiple interfaces:

```java
interface Printable {
    void print();
}

interface Scannable {
    void scan();
}

class MultiFunctionPrinter implements Printable, Scannable {
    public void print() {
        System.out.println("Printing...");
    }

    public void scan() {
        System.out.println("Scanning...");
    }
}
```

---

## 🔹 Differences Between Interface and Abstract Class

|Feature|Interface|Abstract Class|
|---|---|---|
|Methods|Only abstract (default/static allowed from Java 8)|Can have both abstract and concrete|
|Multiple Inheritance|Supported|Not supported|
|Access Modifiers|Only `public`|Can have various access modifiers|
|Constructors|Not allowed|Allowed|

---

# Let's use an **e-commerce application** to demonstrate interfaces in a practical, real-world context.

---

## 🛒 **Scenario: E-commerce Payment System**

In an e-commerce app, you may have multiple payment methods like **Credit Card**, **PayPal**, and **UPI**. Each of them should follow a common contract, but the logic for processing the payment differs.

---

### 🔹 Step 1: Define the Interface

```java
public interface PaymentMethod {
    void pay(double amount);
}
```

This interface defines a contract that all payment methods must follow: implement the `pay` method.

---

### 🔹 Step 2: Implement the Interface in Different Classes

```java
public class CreditCardPayment implements PaymentMethod {
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using Credit Card.");
    }
}

public class PayPalPayment implements PaymentMethod {
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using PayPal.");
    }
}

public class UpiPayment implements PaymentMethod {
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using UPI.");
    }
}
```

Each class implements the interface and provides its own version of the `pay` method.

---

### 🔹 Step 3: Use the Interface in an Order Service

```java
public class CheckoutService {
    public void processOrder(double amount, PaymentMethod paymentMethod) {
        // other order logic...
        paymentMethod.pay(amount); // dynamically call the appropriate pay method
    }
}
```

---

### 🔹 Step 4: Main Class to Simulate Usage

```java
public class ECommerceApp {
    public static void main(String[] args) {
        CheckoutService checkout = new CheckoutService();

        PaymentMethod payment1 = new CreditCardPayment();
        PaymentMethod payment2 = new PayPalPayment();
        PaymentMethod payment3 = new UpiPayment();

        checkout.processOrder(250.75, payment1);
        checkout.processOrder(89.99, payment2);
        checkout.processOrder(120.50, payment3);
    }
}
```

---

### ✅ Output

```
Paid $250.75 using Credit Card.
Paid $89.99 using PayPal.
Paid $120.50 using UPI.
```

---

## 🔍 Benefits of Using Interfaces Here

- **Flexibility**: Easily add new payment methods without modifying existing code.
    
- **Polymorphism**: `CheckoutService` doesn’t care which payment method is used.
    
- **Maintainability**: Cleaner, modular code using abstraction.
    

---

Would you like to expand this further to include inventory, shipping, or user roles using interfaces?