# Factory Pattern

The **Factory Pattern** is a **creational design pattern** in C# that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created. It encapsulates the object creation logic, making the code more flexible, reusable, and easier to maintain.
### Key Points:

1. **Encapsulation of Object Creation**:

   - Instead of creating objects directly using `new`, the creation logic is encapsulated in a factory class or method.

2. **Decouples Object Instantiation**:

   - The client code does not need to know the specific class being instantiated; it just works with a common interface or abstract class.

3. **Improves Maintainability**:

   - Changes to the object creation logic or adding new types can be handled in one place (the factory), without altering the client code.


---
### Example of Factory Pattern in C#:

#### Scenario:

Let's explain the **Factory Pattern** using a **Notification system example** where different types of notifications (`Email`, `SMS`, and `Push`) need to be created dynamically.


---
### Scenario:

You have a system that sends different types of notifications. Based on the user's preference, you may need to send:

1. **Email Notification**

2. **SMS Notification**

3. **Push Notification**

Using the Factory Pattern ensures that the client code doesn't need to worry about the details of creating these notifications.
 

---
### Step-by-Step Explanation:

#### Step 1: Define a common interface or abstract class

All notifications will implement a common interface, ensuring they share a common behavior.

```csharp

public interface INotification

{

    void Notify(string message);

}

```

---
#### Step 2: Implement concrete notification classes

Each notification type implements the `INotification` interface with its specific behavior.

```csharp

public class EmailNotification : INotification

{

    public void Notify(string message)

        => Console.WriteLine($"Sending Email: {message}");

}

  

public class SMSNotification : INotification

{

    public void Notify(string message)

        => Console.WriteLine($"Sending SMS: {message}");

}

  

public class PushNotification : INotification

{

    public void Notify(string message)

        => Console.WriteLine($"Sending Push Notification: {message}");

}

```


---
#### Step 3: Create the Notification Factory

The factory class handles the object creation logic, deciding which notification object to create based on input.

```csharp

public class NotificationFactory

{

    public static INotification GetNotification(string notificationType)

    {

        return notificationType.ToLower() switch

        {

            "email" => new EmailNotification(),

            "sms" => new SMSNotification(),

            "push" => new PushNotification(),

            _ => throw new ArgumentException("Invalid notification type")

        };

    }

}

```

---
#### Step 4: Use the Factory in the Client Code

The client code only interacts with the factory and the `INotification` interface. It doesn’t need to know the exact implementation details.

```csharp

class Program

{

    static void Main(string[] args)

    {

        // Example: User selects notification type

        Console.WriteLine("Enter notification type (Email, SMS, Push): ");

        string notificationType = Console.ReadLine();

  

        try

        {

            // Create the notification using the factory

            INotification notification = NotificationFactory.GetNotification(notificationType);

  

            // Send the notification

            notification.Notify("This is your notification message!");

        }

        catch (Exception ex)

        {

            Console.WriteLine($"Error: {ex.Message}");

        }

    }

}

```
---
### How It Works:

  
1. **Encapsulation of Object Creation**:

   - The `NotificationFactory` encapsulates the logic of creating the appropriate notification object based on the type.

   - The client (`Program` class) does not directly instantiate the concrete classes.
  
2. **Decoupling**:

   - The client code is decoupled from the specific notification classes (`EmailNotification`, `SMSNotification`, etc.). It works only with the `INotification` interface.
  
3. **Flexibility**:

   - If a new notification type (e.g., `WhatsAppNotification`) needs to be added, you only modify the factory (`NotificationFactory`) without changing the client code.

  ---

  ### Example Output:

If the user inputs **email**:

```

Enter notification type (Email, SMS, Push):

email

Sending Email: This is your notification message!

```

If the user inputs **sms**:

```

Enter notification type (Email, SMS, Push):

sms

Sending SMS: This is your notification message!

```

If the user inputs an invalid type:

```

Enter notification type (Email, SMS, Push):

unknown

Error: Invalid notification type

```


---

### Benefits in this Example:

1. **Scalability**: Adding new notification types is straightforward.

2. **Maintainability**: The client code remains clean and focused on business logic.

3. **Reusability**: The factory logic can be reused wherever object creation is needed.

This example showcases how the Factory Pattern provides a robust, flexible design for handling object creation dynamically.

---
### Benefits:

1. **Code Reusability**: Centralizes the object creation logic.

2. **Flexibility**: Easily extendable for new types without modifying existing code.

3. **Abstraction**: Client code works with interfaces or abstract classes, not concrete implementations.

  
---
### When to Use:

- When you need to decouple the instantiation of objects from their use.

- When you have complex creation logic for objects.

- When you want to provide a single point of control for creating related objects.

This pattern is especially useful in situations where the exact type of the object to be created might vary or when the creation involves some complex logic.