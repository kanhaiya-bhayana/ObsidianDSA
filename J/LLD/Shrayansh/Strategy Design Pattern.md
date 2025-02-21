This pattern enables the algorithm's behavior to be selected at runtime.

### Structure

The Strategy pattern consists of the following components:

1. **Strategy Interface**: Defines a common interface for all supported strategies.
2. **Concrete Strategies**: Implement the interface with specific algorithms or behaviors.
3. **Context**: Maintains a reference to a Strategy object and allows the client to set or change the strategy.


Here’s a **Java example** demonstrating the Strategy Design Pattern in the context of a payment system:

### Code Example

```java
// Strategy Interface
interface PaymentStrategy {
    void pay(int amount);
}

// Concrete Strategy 1: Credit Card Payment
class CreditCardPayment implements PaymentStrategy {
    private String cardNumber;

    public CreditCardPayment(String cardNumber) {
        this.cardNumber = cardNumber;
    }

    @Override
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using Credit Card (Card Number: " + cardNumber + ").");
    }
}

// Concrete Strategy 2: PayPal Payment
class PayPalPayment implements PaymentStrategy {
    private String email;

    public PayPalPayment(String email) {
        this.email = email;
    }

    @Override
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using PayPal (Email: " + email + ").");
    }
}

// Context
class PaymentContext {
    private PaymentStrategy paymentStrategy;

    // Set the strategy dynamically
    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    // Execute payment
    public void pay(int amount) {
        if (paymentStrategy == null) {
            throw new IllegalStateException("Payment strategy not set!");
        }
        paymentStrategy.pay(amount);
    }
}

// Client Code
public class StrategyPatternExample {
    public static void main(String[] args) {
        PaymentContext paymentContext = new PaymentContext();

        // Using Credit Card Strategy
        paymentContext.setPaymentStrategy(new CreditCardPayment("1234-5678-9012-3456"));
        paymentContext.pay(500);

        // Switching to PayPal Strategy
        paymentContext.setPaymentStrategy(new PayPalPayment("user@example.com"));
        paymentContext.pay(300);
    }
}
```

### Explanation

1. **`PaymentStrategy` Interface**: Defines the common method `pay()` that all payment strategies must implement.
2. **Concrete Strategies (`CreditCardPayment` and `PayPalPayment`)**: Implement the `PaymentStrategy` interface, providing specific payment logic.
3. **`PaymentContext` Class**: Holds a reference to a `PaymentStrategy` object. The `setPaymentStrategy()` method allows changing the strategy dynamically.
4. **Client Code**: Demonstrates how the client can switch strategies during runtime.

### Output

```
Paid 500 using Credit Card (Card Number: 1234-5678-9012-3456).
Paid 300 using PayPal (Email: user@example.com).
```

### Advantages of Using Strategy Pattern in Java

- **Flexibility**: Easily add new payment methods without modifying existing code.
- **Reusability**: Payment algorithms are encapsulated in separate classes, making them reusable.
- **Testability**: Each strategy can be tested independently.

This example shows how the Strategy Pattern is implemented in Java for a clean and dynamic approach to managing behaviors.