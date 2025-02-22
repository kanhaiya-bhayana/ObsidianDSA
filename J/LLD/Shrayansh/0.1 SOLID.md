### S - Single Responsibility Principle
### O - Open/Closed Principle
### L - Liskov Substitution Principle
### I - Interface Segmented Principle
### D - Dependency Inversion Principle



[The S.O.L.I.D Principles in Pictures | by Ugonna Thelma | Backticks & Tildes | Medium](https://medium.com/backticks-tildes/the-s-o-l-i-d-principles-in-pictures-b34ce2f1e898)

Here’s an example of Java pseudocode that illustrates the 
##### Single Responsibility Principle (SRP) 
by separating responsibilities into distinct classes:

### Without SRP (Violates SRP)

```java
class MultiFunctionalRobot {
    void cook() {
        System.out.println("Cooking...");
    }

    void garden() {
        System.out.println("Gardening...");
    }

    void paint() {
        System.out.println("Painting...");
    }

    void drive() {
        System.out.println("Driving...");
    }
}
```

This robot does too many tasks, which makes the class difficult to maintain and violates SRP.

---

### With SRP (Follows SRP)

```java
// Define separate classes for each responsibility
class ChefRobot {
    void cook() {
        System.out.println("Cooking...");
    }
}

class GardenerRobot {
    void garden() {
        System.out.println("Gardening...");
    }
}

class PainterRobot {
    void paint() {
        System.out.println("Painting...");
    }
}

class DriverRobot {
    void drive() {
        System.out.println("Driving...");
    }
}

// Example Usage
public class RobotController {
    public static void main(String[] args) {
        ChefRobot chef = new ChefRobot();
        chef.cook();

        GardenerRobot gardener = new GardenerRobot();
        gardener.garden();

        PainterRobot painter = new PainterRobot();
        painter.paint();

        DriverRobot driver = new DriverRobot();
        driver.drive();
    }
}
```

By following SRP, each class has only one responsibility, making the code modular and easier to maintain or extend.

#### The Open/Closed Principle (OCP) states that a class should be open for extension but closed for modification. This allows you to add new functionality without altering existing code.

Here's an example of OCP using Java pseudocode:

---

### Without OCP (Violates OCP)

```java
class Robot {
    void work(String task) {
        if (task.equals("cut")) {
            System.out.println("Cutting...");
        } else if (task.equals("paint")) {
            System.out.println("Painting...");
        } else {
            System.out.println("Unknown task.");
        }
    }
}
```

If a new task is added, you need to modify the `work` method, which violates the OCP.

---

### With OCP (Follows OCP)

```java
// Abstract Task class
abstract class Task {
    abstract void execute();
}

// Concrete tasks
class CuttingTask extends Task {
    @Override
    void execute() {
        System.out.println("Cutting...");
    }
}

class PaintingTask extends Task {
    @Override
    void execute() {
        System.out.println("Painting...");
    }
}

// Robot class that works with tasks
class Robot {
    void performTask(Task task) {
        task.execute();
    }
}

// Example usage
public class Main {
    public static void main(String[] args) {
        Robot robot = new Robot();

        Task cuttingTask = new CuttingTask();
        robot.performTask(cuttingTask);

        Task paintingTask = new PaintingTask();
        robot.performTask(paintingTask);
    }
}
```

---

### Explanation

1. The `Task` class is abstract and serves as a base for different tasks.
2. Adding new tasks, like `WeldingTask`, only requires creating a new subclass of `Task` without modifying the `Robot` class.
3. This ensures the `Robot` class is **closed for modification** but **open for extension**.


## Liskov

```java
public class Main {
    public static void main(String[] args) {
        // Substituting Sam with Eden
        Sam sam = new Eden();
        sam.makeCoffee();
        
        System.out.println("Hello World");
    }
}

// Base class
class Sam {
    void makeCoffee() {
        System.out.println("Sam is making a coffee!");
    }
}

// Subclass
class Eden extends Sam {
    @Override
    void makeCoffee() {
        System.out.println("Eden is making a coffee!");
    }
}

```

>Remember the example of Vehicle engine and non-engine