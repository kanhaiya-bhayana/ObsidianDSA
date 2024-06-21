The Dependency Inversion Principle (DIP) is about structuring your code to depend on abstractions rather than concrete implementations. It has two key aspects:

1. **High-level modules should not depend on low-level modules. Both should depend on abstractions.**
2. **Abstractions should not depend on details. Details should depend on abstractions.**

In simpler terms, DIP suggests that the high-level logic of your application should not be directly tied to the low-level implementation details. Instead, both should depend on interfaces or abstract classes.

### Scenario

We'll implement a notification system where initially we use email notifications, but we want the flexibility to add other types like SMS or push notifications.

### Without Dependency Inversion

Here's an implementation where the `NotificationService` directly depends on the `EmailNotification` class.

```csharp
// Low-level module
public class EmailNotification {
    public void SendEmail(string message) {
        Console.WriteLine("Sending email with message: " + message);
    }
}

// High-level module
public class NotificationService {
    private EmailNotification emailNotification;

    public NotificationService() {
        this.emailNotification = new EmailNotification();
    }

    public void Send(string message) {
        emailNotification.SendEmail(message);
    }
}

public class Program {
    public static void Main(string[] args) {
        NotificationService notificationService = new NotificationService();
        notificationService.Send("Hello, Dependency Inversion Principle!");
    }
}
```

In this design, `NotificationService` is tightly coupled to `EmailNotification`. If we want to add SMS or push notifications, we would need to modify `NotificationService`.

### With Dependency Inversion

First, we define an abstraction for the notification mechanism:

```csharp
// Abstraction
public interface INotification {
    void Send(string message);
}
```

Next, we create concrete implementations of this abstraction for different notification types:

```csharp
// Low-level module for Email
public class EmailNotification : INotification {
    public void Send(string message) {
        Console.WriteLine("Sending email with message: " + message);
    }
}

// Low-level module for SMS
public class SMSNotification : INotification {
    public void Send(string message) {
        Console.WriteLine("Sending SMS with message: " + message);
    }
}

// Low-level module for Push Notification
public class PushNotification : INotification {
    public void Send(string message) {
        Console.WriteLine("Sending push notification with message: " + message);
    }
}
```

Now, we refactor `NotificationService` to depend on the `INotification` interface:

```csharp
// High-level module
public class NotificationService {
    private INotification notification;

    public NotificationService(INotification notification) {
        this.notification = notification;
    }

    public void Send(string message) {
        notification.Send(message);
    }
}

public class Program {
    public static void Main(string[] args) {
        // We can easily switch between different implementations
        INotification emailNotification = new EmailNotification();
        INotification smsNotification = new SMSNotification();
        INotification pushNotification = new PushNotification();

        NotificationService notificationService = new NotificationService(emailNotification);
        notificationService.Send("Hello, Dependency Inversion Principle with Email!");

        notificationService = new NotificationService(smsNotification);
        notificationService.Send("Hello, Dependency Inversion Principle with SMS!");

        notificationService = new NotificationService(pushNotification);
        notificationService.Send("Hello, Dependency Inversion Principle with Push Notification!");
    }
}
```

### Benefits

1. **Flexibility**: We can switch between different notification implementations without changing the `NotificationService` class.
2. **Testability**: We can create mock implementations of the `INotification` interface to test `NotificationService`.
3. **Maintainability**: The code is more modular and easier to extend with new notification types.

In this example, the high-level module (`NotificationService`) depends on the abstraction (`INotification`), and the low-level modules (`EmailNotification`, `SMSNotification`, `PushNotification`) implement this abstraction, adhering to the Dependency Inversion Principle.