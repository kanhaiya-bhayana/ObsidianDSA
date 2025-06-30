# Procedural vs OOPs

| S.No. | Procedural Programming                                                                 | OOPS                                                                            |
|-------|------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 1     | Program is divided into parts called functions                                           | Program is divided into Objects                                                 |
| 2     | Does not provide proper way to hide data, gives importance to function and data moves freely. | Objects provide data hiding, gives importance to data.                          |
| 3     | Overloading is not possible                                                              | Overloading is possible                                                         |
| 4     | Inheritance is not possible                                                              | Inheritance is possible                                                         |
| 5     | Code reusability does not present                                                        | Code reusability is present                                                     |
| 6     | Examples: Pascal, C, FORTRAN etc.                                                        | Examples: Java, C#, Python, C++ etc.                                            |

### Objects

Object has 2 things;
1. Properties or State
2. Behavior or Function

Example?
### Class

Blueprint or skeleton 
- To create an object class is required.
- So, class provides the template or Blueprint from which Object can be created.
- From one Class, we can create multiple Objects.
- To create a class, use the keyword `class`

# Pillars of OOPs
#### 1. Data Abstraction
#### 2. Data Encapsulation
#### 3. Inheritance 

#### 4. Polymorphism 
---
## 1. Data Abstraction
- It hides the internal implementation and shows only essential functionality to the user.
- It can be achieved through interface and abstract classes.
### 🔹 In Java, an **abstract class can contain both:**

- **Abstract methods** (methods without a body)
- **Non-abstract methods** (methods with full implementation)
### 🍔 Data Abstraction Example — Food Delivery App (Java)

When a user orders food with a delivery app, they simply:

1. **Select a restaurant**
2. **Choose dishes**
3. **Pay & place the order**

They *do not* see (or need to know) how the system:

- Queries restaurant & menu databases  
- Validates inventory  
- Processes payments via a gateway  
- Assigns delivery personnel  
- Confirms the order in real‑time  

All of that complexity is **abstracted** behind a clean interface.

---
##### 1. Core Abstraction
##### `FoodOrder` (Abstract Class)

```java
abstract class FoodOrder {

    // --- abstract behaviours (the *what*) ---
    public abstract void selectRestaurant(String name);
    public abstract void addItem(String item);
    public abstract void makePayment(double amount);

    // --- concrete behaviour (common for all orders) ---
    public void placeOrder() {
        System.out.println("✅  Your order has been placed successfully.");
    }
}

```

---
##### 2. Concrete Implementation
###### `OnlineFoodOrder`

```java
class OnlineFoodOrder extends FoodOrder {

    @Override
    public void selectRestaurant(String name) {
        // Imagine: query DB, fetch menu, handle errors, etc.
        System.out.println("Restaurant chosen: " + name);
    }

    @Override
    public void addItem(String item) {
        // Imagine: validate stock, update cart, recalc totals…
        System.out.println("Added to cart: " + item);
    }

    @Override
    public void makePayment(double amount) {
        // Imagine: call payment service, handle retries, etc.
        System.out.println("💰  Paid ₹" + amount + " via UPI");
    }
}
```

---
##### 3. Using the Abstraction

```java
public class FoodDeliveryApp {
    public static void main(String[] args) {

        FoodOrder order = new OnlineFoodOrder();  // work with the *abstraction*

        order.selectRestaurant("Biryani Express");
        order.addItem("Chicken Biryani");
        order.makePayment(299.0);
        order.placeOrder();   // prints confirmation
    }
}
```

The calling code focuses purely on **what** it wants done, not **how** it gets done.

---
##### 4. What’s Hidden vs. Exposed?

|Internal Complexity (Hidden)|Simple Public API (Exposed)|
|---|---|
|DB fetch / caching of restaurant & menu data|`selectRestaurant("…")`|
|Cart state, inventory checks, pricing rules|`addItem("…")`|
|Payment‑gateway integration, retries, fraud checks|`makePayment(299.0)`|
|Order orchestration, delivery assignment, notifications|`placeOrder()`|

---
##### 5. Benefits of Data Abstraction

- **Ease of use:** Callers interact with a tiny, intuitive surface.
- **Maintainability:** Internals can evolve (new DB schema, new payment provider) without breaking client code.
- **Security & Safety:** Sensitive details (credentials, transaction logic) stay hidden.
- **Reusability & Testing:** Different order types (e.g., dine‑in, takeaway) can reuse the same abstract interface with their own implementations.
#### Advantages
- Increase security and confidentiality.

---

### 🔄 Abstract Class vs Interface (Java)

|Feature|Abstract Class|Interface (Java 8+)|
|---|---|---|
|**Purpose**|Partial abstraction|Full abstraction (originally)|
|**Methods**|Can have abstract & non-abstract methods|Can have abstract, `default`, and `static` methods|
|**Constructors**|✅ Yes|❌ No|
|**Instance Variables**|✅ Yes (non-final, with access modifiers)|❌ No (only `public static final` constants)|
|**Access Modifiers**|Can be `private`, `protected`, `public`|Only `public` (methods are implicitly public)|
|**Multiple Inheritance**|❌ No (a class can extend only one abstract class)|✅ Yes (a class can implement multiple interfaces)|
|**Inheritance Type**|`extends`|`implements`|
|**When to Use**|When you want to share code (state/logic) and enforce a contract|When you just want to enforce a contract|

---
#### ✅ Code Example

#### 🔸 Abstract Class Example

```java
abstract class Vehicle {
    int speed;

    // Abstract method
    abstract void start();

    // Concrete method
    void stop() {
        System.out.println("Vehicle stopped.");
    }
}
```
#### 🔸 Interface Example

```java
interface Flyable {
    void fly(); // abstract by default

    default void land() {
        System.out.println("Landing safely.");
    }
}
```
#### 🔸 Usage Example

```java
class Airplane extends Vehicle implements Flyable {
    @Override
    void start() {
        System.out.println("Airplane engine started.");
    }

    @Override
    public void fly() {
        System.out.println("Airplane is flying.");
    }
}
```

---
#### 🧠 When to Use What?

|Scenario|Use Abstract Class|Use Interface|
|---|---|---|
|Need to share common code (fields/methods)|✅ Yes|❌ No (can't have state)|
|Need multiple inheritance behavior|❌ No|✅ Yes|
|Designing an extensible base class|✅ Yes|❌ Not ideal|
|Defining capabilities (e.g., `Flyable`)|❌ Not ideal|✅ Perfect for "can-do" behaviors|

---
#### 📌 TL;DR

- Use **abstract classes** when you want to provide **base implementation** and **shared state (fields)**.
- Use **interfaces** when you want to define **only behavior contracts** and **support multiple inheritance**.

---
## 2. Data Encapsulation

- Encapsulation bundles the data and the code working on that data in a single unit.
- Also known as DATA-HIDING.
- Steps to achieve encapsulation:
	- Declare variable of a class as private.
	- Provide public getters and setters to modify and view the value of the variables.

### Advantages of encapsulation 
- Loosely coupled code.
- Better access control and security

---
### 🔐 Encapsulation Example: User Login System

In any login system:

- A user has private data like `username`, `password`, and possibly `role` or `email`.
- The system should **never allow direct access** to sensitive data like passwords.
- You **control access** via public methods like `authenticate()`, not by exposing fields.

---
#### ✅ Java Code Example

```java
public class User {
    // 🔐 Private fields (hidden from the outside world)
    private String username;
    private String password;  // Sensitive information

    // Constructor
    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }

    // ✅ Public method to verify login (controlled access)
    public boolean authenticate(String inputPassword) {
        return this.password.equals(inputPassword);
    }

    // ✅ Read-only access to username
    public String getUsername() {
        return username;
    }

    // ❌ No getter for password — we don’t expose it
}
```

---
#### 🧑‍💻 How it’s used

```java
public class LoginSystem {
    public static void main(String[] args) {
        User user = new User("kanhaiya", "secure123");

        // Trying to access password directly (Not allowed)
        // System.out.println(user.password); // ❌ Compile-time error

        // Safe interaction via method
        boolean loginSuccess = user.authenticate("secure123");

        if (loginSuccess) {
            System.out.println("✅ Login successful! Welcome " + user.getUsername());
        } else {
            System.out.println("❌ Invalid credentials.");
        }
    }
}
```

---
### 🎯 Key Encapsulation Concepts in This Example:

|Principle|Applied As|
|---|---|
|**Private data**|`username` and `password` declared private|
|**Controlled access**|`authenticate()` method verifies credentials|
|**No direct access**|No public getter for password|
|**Modular & secure**|Logic for verification stays inside the class|

---
### 🧠 Why this is a better example?

- It’s **realistic** — login logic is everywhere.
- Shows the **importance of restricting access** (especially for sensitive data).
- Demonstrates how to **expose only what's necessary**, a key benefit of encapsulation.
- Encourages **secure coding practices** by design.
---


## 3. Inheritance

- **Definition:** The capability of a class to inherit properties and behaviors (methods and variables) from another class.
- **Purpose:** Avoid code duplication by reusing code in child classes, allowing more efficient and structured programming.
- **Achieved Using:**
  - The `extends` keyword for class inheritance.
  - Interfaces for multiple inheritance scenarios to resolve issues like the diamond problem.

### Types of Inheritance
- **Single Inheritance:** A class inherits from one parent class.
- **Multilevel Inheritance:** A class inherits from a class, which in turn inherits from another class.
- **Hierarchical Inheritance:** Multiple classes inherit from a single parent class.
- **Multiple Inheritance (via Interfaces):** Achieved by implementing multiple interfaces in Java to address ambiguity (diamond problem). | Same with c#.

### Advantages of Inheritance
- **Code Reusability:** Reduces redundancy by allowing classes to reuse existing functionality.
- **Polymorphism Support:** Enables dynamic method dispatch and other polymorphic behaviors.
- **Ease of Maintenance:** Centralized code modification in parent classes reflects in child classes.

### Examples of Inheritance in Java

#### **1. Single Inheritance**
```java
class BankAccount {
    String accountHolder;
    double balance;

    public void deposit(double amount) {
        balance += amount;
        System.out.println("Deposited: " + amount + ", New Balance: " + balance);
    }
}

class SavingsAccount extends BankAccount {
    double interestRate;

    public void addInterest() {
        balance += balance * (interestRate / 100);
        System.out.println("Interest added. New Balance: " + balance);
    }
}

public class Main {
    public static void main(String[] args) {
        SavingsAccount account = new SavingsAccount();
        account.accountHolder = "John Doe";
        account.balance = 1000.0;
        account.interestRate = 5.0;

        account.deposit(500);
        account.addInterest();
    }
}
```

#### **2. Multilevel Inheritance**
```java
class BankAccount {
    String accountNumber;
    double balance;

    public void deposit(double amount) {
        balance += amount;
        System.out.println("Deposited: " + amount + ", New Balance: " + balance);
    }
}

class SavingsAccount extends BankAccount {
    double interestRate;

    public void addInterest() {
        balance += balance * (interestRate / 100);
        System.out.println("Interest added. New Balance: " + balance);
    }
}

class PremiumSavingsAccount extends SavingsAccount {
    double cashbackRate;

    public void addCashback(double amount) {
        double cashback = amount * (cashbackRate / 100);
        balance += cashback;
        System.out.println("Cashback of " + cashback + " added. New Balance: " + balance);
    }
}

public class Main {
    public static void main(String[] args) {
        PremiumSavingsAccount premiumAccount = new PremiumSavingsAccount();
        premiumAccount.accountNumber = "1234567890";
        premiumAccount.balance = 2000.0;
        premiumAccount.interestRate = 4.0;
        premiumAccount.cashbackRate = 1.5;

        premiumAccount.deposit(1000);
        premiumAccount.addInterest();
        premiumAccount.addCashback(500);
    }
}
```

#### **3. Hierarchical Inheritance**
```java
class BankAccount {
    String accountHolder;
    double balance;

    public void displayBalance() {
        System.out.println("Account Holder: " + accountHolder + ", Balance: " + balance);
    }
}

class SavingsAccount extends BankAccount {
    double interestRate;

    public void addInterest() {
        balance += balance * (interestRate / 100);
        System.out.println("Interest added. New Balance: " + balance);
    }
}

class CurrentAccount extends BankAccount {
    double overdraftLimit;

    public void withdraw(double amount) {
        if (balance - amount >= -overdraftLimit) {
            balance -= amount;
            System.out.println("Withdrawn: " + amount + ", New Balance: " + balance);
        } else {
            System.out.println("Withdrawal exceeds overdraft limit!");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        SavingsAccount savings = new SavingsAccount();
        savings.accountHolder = "Alice";
        savings.balance = 1500;
        savings.interestRate = 3.0;
        savings.addInterest();

        CurrentAccount current = new CurrentAccount();
        current.accountHolder = "Bob";
        current.balance = 1000;
        current.overdraftLimit = 500;
        current.withdraw(1200);
        current.withdraw(400);
    }
}
```

---

## 4. Polymorphism

**Polymorphism** in Java is the ability of an object to take on many forms. It enables a single interface to represent different underlying types or behaviors.

### Types of Polymorphism
1. **Compile-time Polymorphism (Method Overloading):**
   - Same method name with different parameters in the same class.
   - Resolved at compile time.

   **Example:**
   ```java
   class Calculator {
       int add(int a, int b) {
           return a + b;
       }

       int add(int a, int b, int c) {
           return a + b + c;
       }

       double add(double a, double b) {
           return a + b;
       }
   }

   public class Main {
       public static void main(String[] args) {
           Calculator calc = new Calculator();
           System.out.println(calc.add(10, 20));          // Calls int add(int, int)
           System.out.println(calc.add(10, 20, 30));      // Calls int add(int, int, int)
           System.out.println(calc.add(5.5, 4.5));        // Calls double add(double, double)
       }
   }
   ```

2. **Runtime Polymorphism (Method Overriding):**
   - A subclass provides a specific implementation of a method already defined in its parent class.
   - Resolved at runtime.

   **Example:**
```java
abstract class Payment {
    abstract void pay(double amount);
}

class CreditCardPayment extends Payment {
    @Override
    void pay(double amount) {
        System.out.println("Paid " + amount + " using Credit Card");
    }
}

class PayPalPayment extends Payment {
    @Override
    void pay(double amount) {
        System.out.println("Paid " + amount + " using PayPal");
    }
}

public class Main {
    public static void main(String[] args) {
        Payment payment;

        payment = new CreditCardPayment();
        payment.pay(100.0);  // Outputs: Paid 100.0 using Credit Card

        payment = new PayPalPayment();
        payment.pay(200.0);  // Outputs: Paid 200.0 using PayPal
    }
}

```

---

### Key Features of Polymorphism
- **Extensibility:** Add new behaviors without altering existing code.
- **Dynamic Binding:** Resolves method calls at runtime.
- **Reusability:** Enables writing generic code that works with different object types.
